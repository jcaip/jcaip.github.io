---
layout: page
permalink: /log/
title: Reserach Log
---

Log of my research experience, just for kicks. 

### 04-28-2019: 
---
Basically converted all the code to python3 for quick thoughts, but now I need to get the eval scripts set up.

Why are people using python2 in 2019?...

Got training working + training on GPU - saw a 40x speedup (thank god).

### 04-29-2019:
---
And then I fucked up by running 
```
    chown -R jcaip /
```
by accident today... looks like when my system reboots I'll be screwed (and reinstall), but at least it's running fine right now.

JFC just trying to run this code is a PITA. looks like skip-thoughts, the original paper, needs to be cloned as well, but it uses freaking python2.7, so converting it here.

Honestly, I should probably just get this running with 2.7..... that's a bit of a bitter pill to swallow.

I just did it :(

### 05-05-2019:
---
So some good progress today, finally got the dataset part working, and build a simple BOW max encoder. 

Now working on the model, actually seems to be going pretty well. 
Looks like I can just slap a softmax on top and do some shit, so no need to write it all myself. 

For now, just testing to see if I can get a full training script to run.


I got it working, but it looks like torchtext can't really handle the huge file well - so just going to use the simple approach and write some wrapper python. 

A little peeved, because i thought torchtext would handle this better - although tbf I'm probably not using it properly.
Actually, i could totally see it being an ease of use and not a speed thing, because writing this now is a pita.

I probably should have just split the file into multiple datasets, and then iterated through each set....

and then figure out a different way to build the vocab?

Finally some good news - got a basic testing script running, and it's able to handle all of BC.

even better news - I added in visdom...

now making progress on getting an lstm encoder, since it didn't look like the training was right.

- added in gradient clipping too, so today has been a nice day productivity wise.

maybe I should buy some more ram? looks like this whole thing uses just about 32 gigs.

But overall a bunch of good progress today.

CUDA working now too!

Really nwo i just need to focus on - some basic evaluation (nearest neighbors?)
and then speeding it up as much as possible

Came up with a realitvely shitty evaluation idea, but it was fast to implement. 

Basically i came up with 4 sentences with varying degree of simularity (sujbjectively), and then I'm just going to inspect the scores matrix manually. 

Also added in checkpointing, so now this is running forreal for at least a night.... wonder what's going to happen


