---
layout: post
title: '"On Intelligence" in a nutshell (plus a few thoughts)'
---

I've read a once popular book by Jeff Hawkins where he firstly introduced his 
HTM framework and tried to squeeze out his insights on how can we improve modern DL models
to make them more stable and sample-efficient.

![Brain structure](/images/abstract_brain.jpg)

<!--more-->

First of all, in my humble opinion, the book is written in a very, _very_ informal way,
with tons of remarks, repetitions and unspecified assumptions. If you'd ever wonder
what's the point in using mathematical notation when the main idea of the article you read
is simple and general, I strongly encourage you to try this one.

Secondly, note that the book was considered to be one of the bestsellers... in 2004. Since then,
a lot has changed. Hence, along the way I'll try to mention how the subsequent research partially
solved the problems described in the book.

Thirdly, I'm still a small modest country guy that may not get all the details correctly. Feel free to
argue with me in comments, if you think I'm wrong. 



# General vision

The recent successes of Deep Learning have awaken the long-term dream about AGI again. However,
they have a number of serious flaws that prevent us from creating something _really_ intelligent.
Those include, but not limit to: tremendous data-inefficency, as compared with humans, failure to resist
imperceptible adversarial attacks and training instability.

In this book, Jeff states that it might be due to the fundamental misunderstanding of what a
human brain actually computes. More specifically, he says that at least three things are essential to understand
neocortex behaviour:

* Inclusion of time
* Feedback connections
* Sparse and hierarchical organization





Here is an example of a neocortex column:
![Neocortex structure](/images/neocortex_column.png)

