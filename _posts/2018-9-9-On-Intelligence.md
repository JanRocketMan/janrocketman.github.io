---
layout: post
title: '"On Intelligence" in a nutshell (plus a few thoughts)'
---

I've finally finished reading a once popular book by Jeff Hawkins where he firstly introduced his 
HTM framework and tried to squeeze out his insights on how can we improve modern DL models
to make them more stable and sample-efficient.

![Brain structure](/images/abstract_brain.jpg)

<!--more-->

First of all, in my opinion, the book is written in a very, _very_ informal way,
with tons of remarks, repetitions and unspecified assumptions. Therefore, this summarization
may contain errors, especially when I describe the topology of a human neocortex. Feel free
to correct me in comments, if you see any.

# General vision

The recent successes of Deep Learning have awaken the long-term dream about AGI again. However,
they have a number of serious flaws that prevent us from creating something _really_ intelligent.
Those include, but not limit to: tremendous data-inefficency, as compared with humans, failure to resist
imperceptible adversarial attacks and training instability.

In this book, Jeff states that it might be due to the fundamental misunderstanding of what a
human brain actually computes. More specifically, he says that at least three things are essential in neocortex
behaviour:

#### 1. Inclusion of time

Brains process rapidly changing streams of information. In order to
replicate them (and their possibilities), it may be worth adding time dimension even if there is no any.
In other words, RNNs are more human-like.

#### 2. Importance of feedback connections

Or predictions. Cortical neurons effectively combines unsupervised and supervised learning


#### 3. Deep hierarcical structure




Here is an example of a neocortex column:
![Neocortex structure](/images/neocortex_column.png)

