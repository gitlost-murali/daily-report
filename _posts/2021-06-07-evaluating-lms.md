---
toc: true
layout: post
description: Different ways to measure the efficacy of a Language Model
categories: [slp100]
title: Evaluating Language Models
---

# Evaluating Language Model

## Extrinsic and Intrinsic evaluation

One way to check the performance of Language Model is to embed it in a application and check the performance improvement of the application. This end-end evaluation of the Language Model is called __Extrinsic evaluation__. For instance, in case of Speech recognition, we can run it twice with both LMs and compare which LM is giving better transcription. Intuitively, this evaluation makes more sense since we can know if the particular component is improving the application or not. However, running systems end-end for evaluating LMs is costly and not recommended.

{% include note.html content="Other applications could be Spell correction system or a Machine Translation system." %}

Instead, we can check the quality of the Language Model independent of any application. This is often referred to as __Intrinsic Evaluation__. After dividing the corpus into train and test splits, we train two different N-Gram models on the training data and check which LM returns high probability for the test sentences. This evaluation with the test set is called intrinsic evaluation.

{% include note.html content="A good language model assigns higher probability to real or frequently observed sentences." %}


## Perplexity

Perplexity is a quantifiable metric that is used for intrinsic evaluation. Mathematically, it is the inverted probability of test set normalized by the length N (no of words).

$$ PP(W) = P(w_{1}...w_{N})^{-1/N} $$

Higher the probability of a test sentence, lower will be the perplexity considering the inverse. In other terms, maximizing probability would mean lowering perplexity.

Intuitively, Perplexity can be thought of as weighted average branching factor of the language. The branching factor of a language stands is the no.of possible next words that can follow any word. If you have 10 words in a toy language, branching factor is 10.