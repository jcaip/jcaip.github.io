---
layout: post
tags: [machine-learning, system]
published: false
---
Over the past couple months, I've been working with [Rohan Varma]() on a PyTorch implementation of DistBelief. 

DistBelief is a google paper that describes how to train models in a distributed fashion. 

This is an overview of the implementation we decided on, and some problems and solutions we came up with.

Please check out the code [here]()!

## A Brief Introduction to Distbelief

Distbelief describes two opitimization methods, DownpourSGD and SandblasterLBFGS.
We only implemented DownpourSGD, which is pretty simple, there are two core concepts - a parameter server and a training node.

The parameter server is just a copy of the model parameters, which can send/receive parameters and can also receive a gradeint.

The training node asynchronously pulls the parameters, and then does your usual train step (compute loss, the backprop).
Once we've gotten the gradients, we accumulate them, and then every $$N_{push}$$ times, we send this accumulated gradient to the parameter server, and then continue training. 

We also periodically fetch gradients every $$N_{fetch}$$ times. In both cases, we push/pull the gradients asynchronously. 


## Starting off
There were two approaches that we were looking at. 

We could use pytorch's distributed `send` and `recv` commands. 


Or we could figure out our own message passing system - serializing the numpy tensors and sending them over as JSON. 

What really helped here was the actor model implementation described by [] here. 

Pytorch's distribute implementation seemed easier, and that's where we started, but we ran into some problems. Namely, the way we envisioned it, we would just `send` each param to the server. 

But this has a problem - there's two things that we can send to the server, either a GradientUpdate or a ParameterRequest.

Building in this fashion, we realized that we needed to establish some sort of message passing layer that used `send` and `recv` to exchange information. 

### Sending messages

We're using `dist.isend` and `dist.recv` to send and receive tensors via PyTorch.

For any given model, we squash the parameters to a single parameter vector. The gradients for a backward pass are also the same size as this single parameter vector. 

This is the payload of our message. We also insert an extra element at the start of the vector, which describes what action we want to take with the data we send.

To do this, we went through each parameter in the model, and serialize it into one long vector. We also include a message code as well the rank of the sender, which serves as some sort of id. 

With this in mind, we can **kind** of express our system as actors. 

One thing that does not fit cleanly into this model is the fact that we have some shared memory - we have two threads in the training node. 

One thread listens just waiting for a ParameterUpdate, which comes from the server. 
The other does the training. 

These two threads must share memory (otherwise what would be the point?). I think it helps to think of the actor system as just encompassing the DownpourListener and the ParameterServer. 


The training thread is pretty much the same as downpourSGD, except it will ocassionally send messages, which correspond to a `GradientUpdate` or a `ParameterRequest`. 

Here you can so a big downside of our implementation - a `ParameterRequest` is the same size as a `GradientUpdate`, but a `ParameterRequest` does not need all that room - we just send over 0s.

This is a shame, but I couldn't figure out a clean way around this, so it's just something to keep in mind for now. 

this got us to a working prototype - loss converges, albeit poorly

As an example, you can see our implementation running by opening three seperate terminal windows and running `make server`, `make first` and `make second`, which will train a CNN on CIFAR10.

## Extending Pytorch

The first thing we had to do was refactor most of the code b/c we just shitted most of it out. We also took the time to write this as a pytorch optimizer. 

That proved actually very helpful, as we were able to pull down any arbritrary model and train on it with minimal modification. 

We ended up pulling this CIFAR10 benchmarking code, and used that to compare our perfromance. 
We wanted to run more systematic trials but the first thing we did was refactor the code so it actually made sense. Also cleaned up the example code to be a bit better. 

## Installation/Development instructions

You'll want to create a python3 virtualenv first, after which, you should run `make install`. 
You'll then be able to use distbelief by importing 
```python 

from distbelief.optim import DownpourSGD

optimizer = DownpourSGD(net.parameters(), lr=0.001, freq=50, model=net)

```

## Benchmakring

We compared against a GPU and got some interesting results. 
One thing that stands out is just how fucking fast training on a GPU is - it's really actually kind of depressing.


## Improving
There were several theories as to why we were observing this behavior.

First thing we did was profile the code - how much time is being spent where.

