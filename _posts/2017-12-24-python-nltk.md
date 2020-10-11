---
title: Address Normalization with Python and NLTK
categories: data-science
tags:
    - nltk
    - natural language processing
date: 2017-12-24
---

Addresses in databases, especially ones that are inserted by human operators, are prone to a wide range of forms and errors.
To be able to correctly identify a location from a address and to compare two entities we need to normalize them.
(We're calling normalization both the entire process and one of the processing steps.)

Two problems: first is to identify address on a string by field, with errors; second is to match with existing address database to remove uncertanty.

## Preparation steps

To start tackling the problem we have first to prepare the data. The usual steps for that are:

- normalization
- stemming
- lemmatization
- segmentation (tokenization)
- text rebuild

### Normalization

Normalization consists on transforming the text to a canonical form (equal to all entries) so they can be compared.
The usual steps are

- standardize encoding
- remove punctuation
- transform to lowercase
- remove stopwords and punctuation (with care!)
- separate prefixes and suffixes that doesn't contain information

### Stemming

Stemming is the process of reducing words in different forms (conjugated verbs, plural) to a radical form.
This step is not useful for addresses because most of the addresses are not in different forms.
Proper names, for example, are very common in addresses and don't benefit a lot from stemming.

## Lemmatization

As we're not going to stem the words, we also don't need to lemmatizate them.
Lemmatization is the process of grouping together the flexionated forms of the words so they can be analysed together.

### Segmentation

Segmentation is the task of breaking up the text into tokens, so each token can be analysed separately.
In our case, the tokenization can be done by address field: preffixes, location, complements and suffixes. For example, the address

`Rua Nove de Julho, 2983 ap 33 bloco 1 CEP 00043-424 São Paulo SP`

can be break up into

- Preffix: `Rua`
- Location: `Nove de Julho 2983`
- Complements: `ap 33 bloco 1 CEP 00043-424`
- Suffixes: `São Paulo SP`

This is helpful because now we can match each part of the address with an existing canonical form without a lot of noise.
Each of the fields can be further processed to extract more information, like the postal code number.

### Parsing

The next step is to parse the address.
Parsing consists in break up the address string into fields that compose the address, the breaking up of the fields
mentioned above. To parse we have to assume a structure for the address, either by rules or by some techiques like
 Named Entity Recognition.

### Rebuild text

This task consists in rebuild the normalized and annotated text to a final form. This will be done after the match phase.

## Identification and Match

After cleaning up and normalizing the text we need to check if the value of the address exists in our canonical database. Two approaches:

1. Match with existing database
2. Name Entity Recognition on address

### Match with canonical database

If we have a canonical database with the data considered correct, our job is to match the target addresses with the ones
on this canonical database. This is a *match problem*. We can attack this problem following these steps:

- split address by field (prefix, location, suffixes)
- retrieve match candidates (search engine)
- Match address with candidates by similarity

For this approach we're going to work directly on the text patterns, without any kind of machine learning.
The canonical database is usually provided by the Post Office.

The match between two addresses is a way to check if two addresses are the same.
For example, let's say that we have in our canonical database the entry

| CEP  | Location | City | State |
|------|----------|------|-------|
|00043243 | Nove de Julho | São Paulo | SP |
|00032312 | Nove de Setembro | São Paulo | SP |

and we need to compare with the address above. Which one is the best match?
We could try to do an exact match: only the location strings that are exactly same are the same address.
But this would miss lots of entries that could have typing errors but are otherwise valid addresses, like

`Rua nov de Julho, 2938`

So how do we compensate for these errors?

### Match

One approach is to retrieve candidates from the canonical database that are similar to the address we want to normalize. Search engines do that using different strategies. We're not going to detail this process, so let's just say that our search engine returned candidates to be compared.

For each of these candidates we do a comparison with our target address using some metric of similarity. There are several of such metrics:

- Jaro distance
- Jaro-Winkler distance
- Cosine distance

For now we're going to use Jaro-Winkler distance. We compare the target address with each of the candidates and rank by the similarity between them.

#### Search engines

Search engines usually already make the string similarity comparison to retrive the candidates, so it could, in principle, already compute the similarity score withou the need to program it by ourselves. But sometimes the search engine similarity algorithm cannot be tuned to the type of text, like addresses. We also have more information than only the Location string, like the postal code and suffixes. This could help in the decision process.

### NER

Instead of using regular expressions to break up the address text into components we could create a Named Entity Recognizer and let it separate the address by fields.

- tag canonical database with relevant tags
- train CRF with tagged database
- classify each address
- match classified entity with canonical base

## Decision process

After the text normalization and match we hopefully have a list of candidates with a similarity score between the target and a canonical address. How we decide if the address is indeed the correct address? We can set a score threshold, for example, based on our experience, and test the error rate. We also can create a classification model and train manually with some entries.

## In Python

Let's show the steps above now with some Python and the Natural Language Toolkit [here](https://github.com/ispmarin/text_norm).
