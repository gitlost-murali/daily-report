---
toc: true
layout: post
description: Why a single space tokenizer doesn't help
categories: [slp100]
title: Tokenization
---

# Tokenization

## Cases to handle while tokenizing

Instead of a formal definition, let's understand what is needed from a tokenizer from the following examples:

Punctuation based sentence tokenization: It is a general pattern to split text based on "(.) period" and consider the chunks as sentences. However that logic fails in the following instances:

1. Currency : $ 55,000.98
2. Names: K.L. John
3. Website urls: http://google.com
4. Abbreviations: C.D.C/M.S


__Entity Recognition__

A good tokenizer should recognize `New York`, `Walkie-Talkie` as single tokens. This tells us that tokenization is inherently tied-up with Entity Recognition.

__Clitic__

{% include note.html content="Clitic: Part of a word that can't stand on its own & can only occur when attached with other words. Ex: `'re` in You're " %}

A good tokenizer must be able convert a clitic back to a meaningful word. For example: Tokenizing `you're` to two tokens `you`, `are`.

__Dealing with Ambiguities__

Apostrophe usage in different cases: 

1. Genitive marker -> Book's cover
2. Qoutative marker -> 'Alright, move now', she said.
3. Clitics -> "They're"

__Complexities in different languages__

Languages like Japanese, Thai and Chinese don't use space to mark word boundaries. Especially, in Chinese language, a token is made up of characters which themselves carry a meaning (Morpheme). Because of this property, a sentence can be perceived differently by tokenizing it differently. Also, it becomes increasingly difficult to maintain a vocabulary when different character combinations are possible. Therefore, most NLP applications use Character-Based tokenization.

{% include note.html content="Morpheme: Morpheme is a smallest unit of a word that carries meaning." %}

Ex: Incoming -> In, come, -ing

{% include note.html content="It is important to note that Morpheme sometimes doesn't exist alone compared to word which always exists alone." %}

## Subword Tokenization

As the list gets exhaustive, it becomes increasingly tough to build a tokenizer that handles all such cases. And since tokenization is just the first step of an NLP application, it must be fast.

Considering all the situations above, research fraternity is increasingly adopting a third-way (apart from word/char based tokenization) i.e Sub-Word Segmentation. 

A Subword tokenizer statistically fragments a token into subtokens, which may or may not make sense. This helps in handling new words in inference. If training set only had `low` and system received the word `lower`, it is helpful to break it into `low` + `er`.

## Tokenization schemes

Most subword tokenization schemes usually have two parts, 

1. Token learner : Induces a vocabulary based on the training corpus.
2. Token Segmenter: Splits a sentence into tokens in the learned vocabulary.

BPE, Unigram Language Modeling and WordPiece are some of the widely adopted tokenization methods.

Let's understand the algo behind BPE (how a vocabulary is induced) tomorrow.