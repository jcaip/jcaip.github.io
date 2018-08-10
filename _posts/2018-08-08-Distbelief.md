---
layout: post
title: Implementing DistBelief (Pt. I)
tags: [machine-learning, systems, optimization]

images:
  - url: https://raw.githubusercontent.com/ucla-labx/distbelief/master/docs/train_time.png 
  - alt: distbelief
  - title: distbelief
---

Over the past couple months, I've been working with [Rohan Varma](http://rohanvarma.me/) on a PyTorch implementation of DistBelief.

DistBelief is a Google paper that describes how to train models in a distributed fashion. In particular, we were interested in implementing a distributed optimization method, DownpourSGD.

This is an overview of our implementation, along with some problems we faced along our way. 

Please check out the code [here](https://github.com/ucla-labx/distbelief).

## A Brief Introduction to Distbelief

For some context, I'm going to briefly explain DistBelief. You can check out the original paper [here](https://static.googleusercontent.com/media/research.google.com/en//archive/large_deep_networks_nips2012.pdf).
Distbelief describes an entire framework for parallelizing neural networks, we focused on the primary concept of the paper - two distributed opitimization methods, DownpourSGD and SandblasterLBFGS.

We only implemented DownpourSGD, which is much simpler. 

### Downpour SGD 
In DownpourSGD, there are two core concepts - a parameter server and a training node.

![paper_diagram](/images/distbelief/paper_diagram.png)

The parameter server is just a copy of the model parameters, which can send/receive parameters and can also receive an accumulated gradeint, which it can then apply to it's own copy of the parameters.

The training node asynchronously pulls the parameters, and then does your usual train step (compute loss, the backprop).
Once we've gotten the gradients, we apply the normally. 
However at the same time, we also accumulate them in another tensor. Once we sum $$N_{push}$$ gradients, we send this accumulated gradient to the parameter server, and zero the accumulated gradients. 
We also periodically fetch gradients every $$N_{fetch}$$ times. In both cases, we push/pull the gradients asynchronously. so training continues as usual when pushing/pulling data.

![paper_alg](/images/distbelief/paper_alg.png)

## Starting Off 

When we were first brainstorming how we wanted to implement this, we discovered an [Akka implementation]() of DistBelief. 

This along with
This let us break the problem down into two different pieces.

We needed a reduimentery emssage passing system, with shich we could then use to implement DistBelief.  (Which worked out well)
Next, with this message passing system, we could implement DistBelief as a series of Actors.  (Which did **NOT** work out)

### Implementing our Message Passing system

#### Sending/Receiving Tensors
we came up with two different approaches. 

We both had some experience with PyTorch, so natrually our first thoughts landed there. We figuered we could use PyTorch's distributed `send` and `recv` commands to build a system to pass around gradient data.
Alternatively, we could send data in REST request, which meant we needed to figure out how to serializing the pytorch tensors and send them over HTTP. 

We greatly perferred the first approach, as it had several key advantages in our eyes:
- Faster messaging interface, with support for Gloo and OpenMPI, not just TCP. Furthermore, switching between messaging backends would be much much simpler.
- Avoid costly serialization. This isn't entirely true, as I'm sure the message is serialized - but we were avoiding doing it in Python. 
- Better integration with PyTorch. We wanted something that was simple to use, so we figuered implementing it as a PyTorch optimizer would be the easiest. 

However, we struggled a bit to first define how we were going to do this. It took as a while

we ran into some problems. Namely, the way we envisioned it, we would just `send` each param to the server. 

We noticed some limitations w/r/t send/recv of tensors. 

But this has a problem - there's two things that we can send to the server, either a GradientUpdate or a ParameterRequest.

Building in this fashion, we realized that we needed to establish some sort of message passing layer that used `send` and `recv` to exchange information. 

There was a gevent actor model that we took a look at. We used that to implement our Actors. 
We started out with just two actors. `ParameterServer` and `DownpourSGDClient`. 


### Sending messages

We're using `dist.isend` and `dist.recv` to send and receive tensors via PyTorch.

For any given model, we squash the parameters to a single parameter vector. The gradients for a backward pass are also the same size as this single parameter vector. 

This is the payload of our message. We also insert an extra element at the start of the vector, which describes what action we want to take with the data we send.

To do this, we went through each parameter in the model, and serialize it into one long vector. We also include a message code as well the rank of the sender, which serves as some sort of id. 

With this in mind, we can **kind** of express our system with this message passing framework. 

<p align="center">
  <img width="400" height="400" src="https://raw.githubusercontent.com/ucla-labx/distbelief/master/docs/diagram.jpg">
</p>

One thing that does not fit cleanly into this model is the fact that we have some shared memory - we have two threads in the training node. 

One thread listens just waiting for a ParameterUpdate, which comes from the server. 
The other does the training. 

These two threads must share memory (otherwise what would be the point?). I think it helps to think of the actor system as just encompassing the DownpourListener and the ParameterServer. 

The training thread is pretty much the same as downpourSGD, except it will ocassionally send messages, which correspond to a `GradientUpdate` or a `ParameterRequest`. 

Here you can so a big downside of our implementation - a `ParameterRequest` is the same size as a `GradientUpdate`, but a `ParameterRequest` does not need all that room - we just send over 0s.

This is a shame, but I couldn't figure out a clean way around this, so it's just something to keep in mind for now. 

this got us to a working prototype - loss converges, albeit poorly. 

We can see this by training LeNet on MNIST.

![prototype](/images/distbelief/prototype/server.png)

## Extending Pytorch

The first thing we had to do was refactor most of the code b/c we just shitted most of it out. We also took the time to write this as a pytorch optimizer. 

This allows you to use distbelief by importing 
```python 
from distbelief.optim import DownpourSGD

optimizer = DownpourSGD(net.parameters(), lr=0.001, freq=50, model=net)
```
That proved actually very helpful, as we were able to pull down any arbritrary model and train on it with minimal modification. 

We ended up pulling this CIFAR10 benchmarking code, and used that to compare our perfromance. 

## Benchmarking

### v0 (Our first shot)
We were running n_freq = 10

![v0_train](/images/distbelief/v0/cpu_dist2_train.png)
![v0_test](/images/distbelief/v0/cpu_dist2_test.png)

So obviously we had some problems with the lr here, so we bumped it back up

### v1 (Adjusted the lr, reduced n_freq)

![v1_train](/images/distbelief/v1/cpu_dist2_train.png)
![v1_test](/images/distbelief/v1/cpu_dist2_test.png)

### v2 (Ran on a seprate nodes, instead of all locally)
![v2_train](/images/distbelief/v2/cpu_dist2_train.png)
![v2_test](/images/distbelief/v2/cpu_dist2_test.png)

### Final
![final_train](https://raw.githubusercontent.com/ucla-labx/distbelief/master/docs/train_time.png)
![final_test](https://raw.githubusercontent.com/ucla-labx/distbelief/master/docs/test_time.png)

## Conclusion
GPUS are fast, we're faster than single node though!
