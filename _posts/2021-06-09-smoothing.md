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

### Understanding through Unigrams

Let's understand the application of Laplace smoothing through unigram probabilities and slowly progress towards bigram probabilities.

The unigram probability of a word is 

$ P(w_{i}) = c/N \newline where\ N\ =\ no.\ of\ total\ words\ $

After smoothing, i.e adding 1 to all words, probability is

$$ P(w_{i}) = (c+1)/(N+V) $$

N+V since each unique word's count is only updated by 1. So, that's \|V\| +1s.

Instead of viewing in terms of numerator and denominator, we can represent smoothing in terms of changes made to numerator. Let's call the changed numerator as $c^{*}$.

$$ c^{*} = (c+1) * N/(N+V) $$

We can get the probability by normalizing $c^{*}$ by N.

$$ P(w_{i}) = c^{*}/N $$

After cancelling out N, we'll again arrive at the previous smoothing equation only,
$$ P(w_{i}) = (c+1)/(N+V) $$

$c^{*}$ representation makes it easier to notice the changes made by smoothing. This change can be considered as __discounting__ (lessening) the non-zero counts for the probability mass to be shifted. Let's take an example from a corpus of 100 words & 20 unique words.

word1 - frequency of 10

$$ c^{*} = (10+1) * 100/(100+20) \newline c^{*} = 9.16 $$

We could see the lessening of the frequency for this word1. A clearer picture can be seen in the bigram section.

### Extending Laplace to Bigrams

<p><img src="{{ site.baseurl }}/images/lms/bigram-table-slp.png" alt="" title="Fig1: Bigram counts from the 
sample corpus. Reference: https://web.stanford.edu/~jurafsky/slp3/"></p>

__i__ | __want__ | __to__ | __eat__ | __chinese__ | __food__ | __lunch__ | __spend__
2533 | 927 | 2417 | 746 | 158 | 1093 | 341 | 278

Before smoothing, the bigram probability of a word $w_{n}$ is 

$$ P(w_{n}) = c(w_{n} w_{n-1})/c(w_{n-1}) $$

After smoothing, i.e adding 1 to all bigram possibilities, probability is

$$ P(w_{n}) = (c(w_{n} w_{n-1})+1)/(c(w_{n-1}) + V) $$

Now, our $c^{*}$ is

$$ c^{*} = (c(w_{n} w_{n-1})+1) * c(w_{n-1})/(c(w_{n-1}) + V) $$

<p><img src="{{ site.baseurl }}/images/smoothing/smoothing-reconstructed-counts.png" alt="" title="Fig.2: Reconstructed (c*) Bigram counts from the sample corpus. Here, V = 1466 Reference: https://web.stanford.edu/~jurafsky/slp3/"></p>

The bigram probability of a word $w_{n}$ in terms of $c^{*}$ is,

$$ P(w_{n}) = c^{*}/c(w_{n-1}) $$

<p><img src="{{ site.baseurl }}/images/smoothing/smoothing-bigram-probs.png" alt="" title="Fig-3: Bigram probabilties from the sample corpus. Reference: https://web.stanford.edu/~jurafsky/slp3/"></p>

From Fig-1 & Fig-2, we could notice a big change in counts due to one-add smoothing. This is also visible in the probability space.