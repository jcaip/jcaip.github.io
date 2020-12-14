---
published: False
layout: post
---

This is a short recap of what I've been working on this year. 
<!--more-->

### April

The first thing I worked on was quite simple - basically just fine-tuning BERT on a custom dataset. We use this model to classify intents and BERT was a ~10% point boost over the previous deep learning based model (I think a LSTM). 

### May
I experimented a bit with SSL or semi supervised learning methods in order to improve BERT some more. 

UDA, honestly it was very hard to see an improvement in the scoring. But we did collect and use a contrast set in order to test our model - which did improve, but this was too noisy to be considered reproducable. 

### June
I got a slack dump and wanted to try to train a sequence model. Without a sequence dataset though, this was a tough task. 

Eventurally I just distilled the model outputs of the previous intent model. What was suprising was that there were some instances where it was able to perform bettter, which is suprising. 

I also developed a context model - which is a basically a next sequence prediction, but this can be mapped very cleanly to missed opportunities for recognition. 
### July
Worked on pipeline improvements - batching the endpoints, which ultimately lead to a 15x improvement in speed. 

### August
Started working on a placeholder flagger. 
Devised way to train these flaggers with just a couple hundred examples, with quite decent accuracy. Used concepts from contrastive learning to mine samples

### September

### October
Trained new bot placeholder model. Finding that having a wide array of models is kinda cool because if then you can segment and filter, you can usually train good models. Feels like now if you want a model with good precision and recall then you need to do something smart and have talent but if you just need one that you don't really need data anymore. This is great. 

Started working on multitask model

### November

Made multitask model multilingual model. I know that we bag on big companies sometimes because their research is not really reproducible at all but you can't deny this stuff is high quality. Anyways I thing it's great when the system works - here Facebook's investment into the multilingual model enables us to sell to more clients which really affects the bottom line.

### December
Played around with T5 model - initial thoughts is that this shit is really cool. Trained a small feedback softener with just a couple of examples. Finding pairs is hard but actually my background in contrastive learning makes this all the more interesting. Feeld like I can take a lot of the concepts. 

