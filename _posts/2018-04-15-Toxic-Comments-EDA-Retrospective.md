---
layout: post
title: Toxic Comment Competition EDA
subtitle: Hosted by Kaggle and Google Jigsaw
image: /img/iceberg/iceberg_icon.png
tags: [Retrospective, Classifier, Neural Networks, GRU, EDA, Feature Engineering, Keras, Tensorflow]
---

<p align="center">
<b>Disclaimer: Clicking on source code linked to in this page will link to offensive words and text</b>
</p>

Recently I competed in a Kaggle / Google Jigsaw Toxic Comment Classification Contest which ended in early March. The challenge was to classify comments as being normal or as being in one or more toxic categories. Some of the classes had large class imbalances and shown by the following graph.

<p align="center">
<img src="/img/class_distribution.png" width="800" align="middle">
</p>

Initially I performed Eploratory Data Analysis (EDA) looking at the class distributions as well as most common word frequencies per class. Then I started looking at the comments themselves, of different types of classes, and started looking for patterns. Especially patterns that I did not think would survive the normal word vectorization process. The main intuition was to look for features that could be concatenated late in the RNN that would be used to make predictions, providing a bit more signal for the network's decision boundaries.

## Sentence Structure Features:

### "Words per Comment"

<p align="center">
<img src="/img/words_per_comment.png" width="800" align="middle">
</p>

###### Words per Comment: _One of the most basic things to look at was to see if there was a difference in length between toxic comments and normal comments. Because it's an exponential distribution a log transformation was also evaluated also._

<p align="center">
<img src="/img/log10_words_per_comment.png" width="800" align="middle">
</p>

###### Log10 Words per Comment: The sawtooth patterns on the left sides of the plots is due to integer values having a log transformation applied.

### "Repetition"

Anecdotally toxic comments sometimes had some extreme data in which a certain comment was copy and pasted hundreds of times until the message buffer was maxed out.  Initially I discovered this during data pipeline related operations and initial EDA with term frequency. Namely because oddly spelled words would end up being in the top 10 non-stop words of a toxic class. One of those examples was "mothjer" which is covered nicely by a fellow Kaggler [here warning: offensive words](https://www.kaggle.com/fcostartistican/don-t-mess-with-my-mothjer/output)

<p align="center">
<img src="/img/repetition.png" width="800" align="middle">
</p>

###### Formula is: 1 - (# of unique words / # of all words) 0 is the value of comments that are totally unique, near 1 would be totally repetitive.  The zero values distort the plot so those are dropped in the next plot.  There is a decent bump in uniqueness for toxic comments.

<p align="center">
<img src="/img/crop_repetition.png" width="800" align="middle">
</p>

###### There is also a good bump for toxic comments, indicating that some are very repetitive, on the right side of the plot which is distinctly missing from the normal comments.

### "Loudness"

Going through and exploring the toxic comments I noticed that a lot of the toxic comments seemed to have more CAPS than usual. Given that is internet 'yelling' it makes sense as toxic messages seemed to be more emotionally charged.

I made a function that counted the proportion of characters that were capitals relative to the total amount of ascii characters. Think (# A-Z / # a-zA-Z) + ε Because a log transformation was going to be needed again a small epsilon was added so that the log operation was never performed on a 0 value. Therefore the spike at about -5.25 are the comments that are all lower case, ie: 0.0 + ε

<p align="center">
<img src="/img/log_loudness.png" width="800" align="middle">
</p>

###### _Both have characteristic spikes in **all lower** and **ALL CAPS** but the effect is expressed quite a bit more on the toxic comments. This implies that toxic comments are also lower case much more frequently than regular comments._

## Punctuation Based Features:

### "Punctuation Utilization"

How do typical comments compare with toxic comments as far as amounts of punctuation (python `string.punctuation`) per word in a comment.

<p align="center">
<img src="/img/punctuation.png" width="800" align="middle">
</p>


### "Urgency"
Very similar to loudness, toxic comments seemed to have many more explanation points.

<p align="center">
<img src="/img/urgency_stack.png" width="1200" align="middle">
</p>

###### Distribution of Urgency in Comments: _13% of regular comments had an exclamation mark "!" but 27% of toxic comments had at least one "!". The plot above has exluded counts of 0 as that makes up the majority of both classes. One can clearly see that the toxic comments are right shifted (more "!"s per comment) relative to normal comments._

### "Questioning"
Do toxic comments have more or less "?" than normal comments, typically?

<p align="center">
<img src="/img/question_stack.png" width="1200" align="middle">
</p>

###### Distribution of Questioning in Comments: _26% of regular comments have a question mark and 26% of toxic comments have question marks.  Overlaying the two plots with the same scale there is a slight right shift of toxic comments but the effect is not pronounced._

### "Dots"
Do toxic comments have more or less "." or "..." than normal comments, typically? A logical if mundane progression of the statistical punctuation line of thought.

<p align="center">
<img src="/img/dot_stack.png" width="1200" align="middle">
</p>

###### Distribution of Periods in Comments: _13% of normal comments have no "." but 29% of toxic comments have no "." This alone is probably a relatively useful piece of information for a model, which it potentially wouldn't normally have. The toxic comments are again somewhat right shifted so if a toxic comment DOES have "."s it tends to have more of them. Anecdotally this might be because toxic comments seem to have shorter sentences. So it would be interesting to explore some kind of words per sentence feature in the future, although that would require a bit more cleverness to implement well, given the non-standard prose of the typical toxic comment._
