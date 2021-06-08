---
toc: true
layout: post
description: Perplexity vs N-gram size
categories: [slp100]
title: Generalization and Zeroes
---

# Generalization and Zeroes

## Perplexity vs Coherence vs Overfitting vs N-Gram size

It is a known fact that Statistical models are heavily dependent on the training corpus. These models encode specific facts about the training corpus. An implication of this of could be overfitting. Before we talk about overfitting, let's go through an exercise to understand how N-gram size impacts the coherence of sentences generated.

The exercise is to generate sentences from different n-gram models by randomly generating each word.

### Coherence Test

In case of Uni-gram, we randomly generate each word. To perform this random sampling, we first generate a space (0 to 1) filled with words whose interval is proportional to their frequencies. Next, we randomly sample a number b/w 0-1 and pick the word whose interval has this generated number. We continue these steps until \<\/s\> is generated.

1. Sort the words based on frequency
2. Next, for each word, divide its frequency with total counts to get the percentage of space occupied by this word. This way, we generate intervals for each word. For more clarity, look at the example below.
3. Next, we randomly sample a number b/w 0-1 and pick the word whose interval has this generated number.

For example, if you have 18000 total counts and _k_ words.

1. After sorting, let's say our top word (say $w_{1}$) has 9000 counts and next word ($w_{2}$) has 180 counts and so on...
2. Now for $w_{1}$, percentage = 9000/18000 = 0.5. So, the interval for this word is 0 - 0.5
3. For $w_{2}$, percentage = 180/18000 = 0.01. So, the interval for this word is 0.5 - 0.51
....

Once we have the intervals, if the randomly generated number is 0.43. Since 0.43 lies in the interval of $w_{1}$ (0-0.5), $w_{1}$ is randomly selected word.

Incase of bigram, we start with \<s\> and pick one bigram (randomly) out of all the bigrams that start with \<s\>. We can use the same technique (used for unigram earlier) for randomly sampling an item. Say, the second word is _w_. Now, we look for bigrams that start with _w_. And so on.

When we generate sentences with these n-gram models, it is found that,
> Larger the value of N in N-Gram, greater the coherence of generated sentences.

4-gram sentences are more coherent and look more like Shakespeare. If we look at the corpus statistics, N = 884647 & V = 29006, possible bigrams are $V^{2}$ and 4-grams are $V^{4}$. There's no way that all the 4-grams are shakespearean. If we look closely, N-Gram table will be mostly sparse. So, if our 1st 4-gram is _It cannot be but_, there are only few possible combinations. In most cases, this also means a single continuation. This is the reason why what's looking like Shakespeare are actually Shakespeare sentences.

If we compare the sentences generated from N-grams trained on different domains, say Shakespeare and News domain, there will be little overlap b/w them. This points to the fact that statistical models are too dependent on the training corpus and useless for inference in a different domain. __So, we must always ensure the training and inference set must have same genre and dailect too.__

Despite getting the same genre & dailect, models are still subject to the sparsity problem. Any n-gram/phrase that has occured many times in the training set will have a good probability estimate when occured in the test set. However, it is not possible for a corpus to encode all possible words or variations of phrases. Because of this, n-grams that should have non-zero probability will have zero-probability assigned to them.

For example, training set has following details

denied the rumours 5
denied the speculations 2 

In test set, if we have the following phrases

denied the loan
denied the offer

Model will incorrectly estimate that P(load\|denied the) is __0!__.

This leads to two problems.

1. This undermines the generalizability and usage of model in real-life applications.
2. If the probability of any word is 0, then probability of entire set is 0 (remember chain rule).

In the next post, we'll discuss how to handle the issue of unseen n-grams through Smoothing.

### Unknown words

In the age of Social media, lingo is always evolving adding more words to the vocabulary. It is possible that a language model trained on one corpus can come across Out-of-Vocabulary(OOV) words in the test set. These words are often converted into \<UNK\> tokens. There are two ways to train probabilities for \<UNK\> token,

1. Come up with a pre-built lexicon or words and mask words that are not in the set with \<UNK\>.
2. Create the lexicon from training corpus (the usual way) and then mask words that have $freq<k$ as <UNK>.
3. After generating \<UNK\>s in training corpus in either way (Step 1 or 2), we can start training the LM.

The choice of \<UNK\> assigning process has an impact on the metrics like Perplexity, If we have a small vocabulary, model can achieve less perplexity by predicting everything as \<UNK\> which anyway has higher probability. Note that, two LMs can only be compared when they have same vocabulary.