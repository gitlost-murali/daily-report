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

Intuitively, Perplexity can be thought of as __weighted average branching factor of the language__. The branching factor of a language is the no.of possible next words that can follow any word. So, in simple terms, Perplexity tells us, on an average, how many words can be expected for the next word.  

### Understanding it through an example,

#### Instance-1:

If you have 10 words in a toy language and all the words have equal probability of occuring (in train and test set), then for a test sentence of length N, Perplexity = 10.

Here's how, 

$$ PP(W)\ =\ P(w_{1}...w_{N})^{-1/N} 
\newline PP(W)\ =\ ((1/10)^{N})^{-1/N}
\newline PP(W)\ =\ 10$$

{% include note.html content="We are considering unigram probabilities here." %}
#### Instance-2

Let's take another instance where the probabilities are different, say, 1st word has 91 occurences and others occur 1 time each. Now, perplexity would be low for a test sentence with all _1st word_ s.
In this case, Perplexity will be low.

Note that, in instance 1 & 2, branching factor is still 10. However, perplexity is different.