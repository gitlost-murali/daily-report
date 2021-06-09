---
toc: true
layout: post
description: Handling zero probability N-grams in test set
categories: [slp100]
title: Smoothing
---

# Smoothing

LM fails in test set when a known word occurs in an unknown context. We will discuss how to handle this issue in this blog.

{% include note.html content="If any of it sounds alien, refer to the previous blog." %}

If any word has a zero probability in the test set, then probability of entire set becomes zero. To avoid this, smoothing algoritms shave off the probability mass from high frequency words and give it to the zero probability ones.

## Laplace smoothing

This is the most straightforward approach where add 1 to all bigram counts. Laplace smoothing is only used as baselines but not employed in any modern LM systems.


