---
layout: post
title: Toxic Comment Competition
subtitle: Hosted by Kaggle and Google Jigsaw
image: /img/iceberg/iceberg_icon.png
tags: [Retrospective, Classifier, Neural Networks, GRU, EDA, Feature Engineering, Keras, Tensorflow]
---

Recently I competed in a Kaggle / Google Jigsaw Toxic Comment Classification Contest which ended in early March.

<p align="center">
_**Disclaimer: Clicking on source code linked to in this page will link to offensive words and text**_
</p>

The first thing that I did was some exploratory data analysis that compared toxic comments with normal comments. I did this to get an idea for some feature engineering to try later. This is because most forms of word vectorization strip the documents of spaces and other punctuation, as well as making all letters lower case. Therefore the concept was to generate some information to concatenate on the model for information that is typically lost before being fed into a model.

While I did do term frequency and most common words per class analysis, given the nature of the subject I'm not including those for now because they're foul, racist, and otherwise terrible.  Also during the initial development of the data pipeline it was discovered that some highly repetitive comments were poisoning some of the statistics. _(Which is why a repetition metric was created, more on this later.)_ 

  Anyways, this is being noted up front because the comments analyzed in this EDA had the super repetitive comments cropped to 20 words. So those extreme data didn't have as much of an effect.

<p align="center">
<img src="/img/words_per_comment.png" width="512" align="middle">
</p>
###### [ Words per Comment ]
(redo this graph)
One of the most basic things to look at was to see if there was a difference in length between toxic comments and normal comments. Because it's an exponential diUse data from Kaggle and Statoil to make a classifier that predicts if something is an iceberg or a ship.

<p align="center">
<img src="/img/log10_words_per_comment.png" width="512" align="middle">
</p>
###### [ Log10 Words per Comment ]
(redo this graph) 

### "Loudness"
Going through and exploring the toxic comments I noticed that a lot of the toxic comments seemed to have more CAPS than usual. Given that is internet 'yelling' it makes sense as toxic messages seemed to be more emotionally charged.

I made a function that counted the proportion of characters that were capitals relative to the total amount of ascii characters. Think `(# A-Z / # a-zA-Z) + ε` Because a log transformation was going to be needed again a small epsilon was added so that the log operation was never performed on a 0 value. Therefore the spike at `~-5.25` are the comments that are all lower case `(0.0 + ε)`

<p align="center">
<img src="/img/log_loudness.png" width="512" align="middle">
</p>

Both have characteristic spikes in _all lower_ and _ALL CAPS_ but the effect is expressed quite a bit more on the toxic comments. This implies that toxic comments are also lower case much more frequently than regular comments.

### "Urgency"
Very similar to loudness, toxic comments seemed to have many more explanation points.
