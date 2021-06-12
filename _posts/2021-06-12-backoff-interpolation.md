---
toc: true
layout: post
description: Alternatives to Smoothing incase of unseen phrases
categories: [slp100]
title: Back Off & Interpolation
---

# Back-Off

As we encounter an unseen phrase in the testing set, we back-off to its lower-rank n-gram and assign its probability to the unseen phrase. This is called Back-Off.

There can be a situation where a phrase has frequency = 1. This doesn't tell much about the n-gram. So, here, we will do a weighted average of its lower ranking n-grams. Consider a 3-Gram,

$$P(w_{n}|w_{n-2}w_{n-1}) = \lambda_{1}\ x\ P(w_{n}|w_{n-2}w_{n-1}) +  \lambda_{2}\ x\ P(w_{n}|w_{n-1}) + \lambda_{3}\ x\ P(w_{n}) $$

$$ where \sum_{1}^{k}{\lambda_{k}} = 1 $$

Instead, we can do a 