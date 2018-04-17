---
layout: post
title: Toxic Comment Competition
subtitle: Hosted by Kaggle and Google Jigsaw
image: /img/iceberg/iceberg_icon.png
tags: [Retrospective, Classifier, Neural Networks, GRU, EDA, Feature Engineering, Keras, Tensorflow]
---

Recently I competed in a Kaggle / Google Jigsaw Toxic Comment Classification Contest which ended in early March.

The first thing that I did was some exploratory data analysis that compared toxic comments with normal comments. I did this to get an idea for some feature engineering to try later.

While I did do term frequency and most common words per class, given the nature of the subject I'm not including those for now as, unsurprisingly, they're foul, racist, and otherwise terrible.

<img src="/img/words_per_comment.png" width="512" align="middle">
###### [ Words per Comment ]
(redo this graph)
One of the most basic things to look at was to see if there was a difference in length between toxic comments and normal comments. Because it's an exponential diUse data from Kaggle and Statoil to make a classifier that predicts if something is an iceberg or a ship.

###### [ Log10 Words per Comment ]
(redo this graph) 

### "Loudness"
Going through and exploring the toxic comments I noticed that a lot of the toxic comments seemed to have more CAPS than usual. Given that is internet 'yelling' it makes sense as toxic messages seemed to be more emotionally charged.

I made a method that counted the proportion of characters that were capitals relative to the total amount of ascii characters. Think **A-Z / a-zA-Z**


<p align="center">
<img src="/img/iceberg/log_scores_a.png" width="512" align="middle">
</p>
