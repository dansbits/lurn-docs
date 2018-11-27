---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
title: Lurn | Bernoulli Vectorizer
layout: guides
active_dropdown: text_processing
active_guide: bernoulli_vec
---

## Bernoulli Vectorizer (word presence vectorizer)

A bernoulli document model is one that represents a piece of text as an array of boolean values. Each boolean represents the presence (true) or absence (false) of a word in the document.

`Lurn::Text::BernoulliVectorizer` is intended to make it easy to convert text into Bernoulli vectors.

### Basic example
```ruby
docs = ['hello world', 'hello fred']

vectorizer = Lurn::Text::BernoulliVectorizer.new

# vectorizers must be trained in order to know
# what features (words) exist in the data set
vectorizer.fit(docs)

vectorizer.transform(docs)
```

### Configuration
The initializer accepts a number of options for configuring how documents are vectorized. A few include:
- max_df[int]: Excludes words which appear in more than `max_df` documents
- min_df[int]: Excludes words which appear in fewer than `min_df` documents
- strip_stopwords[boolean]: Removes stop words if true
- stem_words[boolean]: Stems words in the documents if true
- ngrams[int]: Features will be determined based on groupings of `ngrams` consecutive words instead of individual words

```ruby
Lurn::BernoulliVectorizer.new(strip_stopwords: true, min_df: 10)
```