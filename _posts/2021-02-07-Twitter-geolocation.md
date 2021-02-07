---
layout:     post
title:      "Twitter geolocation"
subtitle:   " \" I am in ____ \""
date:       2021-02-07 12:00:00
author:     "J"
catalog: false
header-style: text
tags:
    - Rando
---
My first real hit at modelling was probably building a model that predicts twitter users' geolocation through text. Also, my first kaggle competition (internally). Going to keep this fresh, and with as little mathematics as possible.

The dataset given was already partitioned to only give Australian users, and their tweets. I used different regular expressions to remove URLs and non-alphabetical characters, collapsing redundant whitespaces and case-folding the tweets.

To better represent the data, a few different feature engineering techniques were employed. In natural language processing, the baseline often used is a bag-of-words (unigram) model to tokenise a corpus. As a filter, I included a minimum frequency as an additional layer, discarding words with a low usage.

An interesting method for tackling textual data is through the character n-gram bag of words. Where a sentence is broken into sections of an n number of characters, including whitespaces. Success with this method has been found in problems such as spam filtering.

Another method utilised was a simple term frequency–inverse document frequency (tfidf). Where it's mathematical representation is in the form,

$$tf(t,d) = f_{t,d}$$

$$idf(t,D) = \log\frac{N}{|{d \in D : t \in d}|}$$

$$tfidf(t,d,D) = tf(t,d) \cdot idf(t,D)$$

<br/>
I found this interesting heuristic [here](https://arxiv.org/abs/1811.07497), called the word locality heuristic. In short, it assesses words that are often used by a locale and pushes it to a higher probability. Defined in the paper as follows, where $w$ denotes the word, and $s$ the states in $S$.

$$WLH(w) = \max_{s \in S} \frac{P(w|s)}{P(w)}$$

This heuristic was implemented to help reduce the feature space and decrease our error rate; seen below by increasing the % of sorted features used. Where the error rate for development data flatlines around the 20% mark, 25% of the top features were utilised for modelling.
![image info](/img/twitter-geolocation-1.jpg)

A favourite of the kaggle community is the Stacked Generalisation method, from David H. Wolpert. This is where we select base classifiers at Level 0, to make predictions on the data; where we then use the predictions as inputs to a new metaclassifer at Level 1.
