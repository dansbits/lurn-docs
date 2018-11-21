---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
title: Lurn | Multinomial Naive Bayes
layout: guides
---

### Multinomial Naive Bayes
Naive bayes is a bayesian model often used for text classification. Multinomial Naive Bayes specifically classifies observations based on variables with a multinomial distribution (a.k.a. numbers).

Below is a simple text classification using Multinomial Naive Bayes in Lurn.

Start with some text documents

  ```ruby
  documents = [
    'ruby is a great programming language',
    'the giants recently won the world series',
    'java is a compiled programming language',
    'the jets are a football team'
  ]

  labels = ['computers','sports','computers','sports']
  ```

These documents need to be converted to a multinomial document model (counts of the words). This can be accomplished easily with the
`WordCountVectorizer`.
```ruby
vectorizer = Lurn::Text::WordCountVectorizer.new
vectorizer.fit(documents)
vectors = vectorizer.transform(documents)
```

With documents now in the expected format the model can be trained by supplying the new vectors along with the original class corresponding
to each document.

```ruby
model = Lurn::NaiveBayes::MultinomialNaiveBayes.new
model.fit(vectors, labels)
```

The model is ready to classify a new document.

```ruby
new_vectors = vectorizer.transform(['programming is fun'])

# get the most probable class for the new document given the training data
model.max_class(new_vectors.first)

# get the probability score for the most probable class
model.max_probability(new_vectors.first)
```
This is of course a contrived example but shows the general API for Multinomial Naive Bayes. To use this model for a more real-world example you
should have a deeper understanding of naive bayes. Below are some additional resources for learning more.

#### Additional Resources
- [API Docs](/docs/Lurn/NaiveBayes/MultinomialNaiveBayes.html)
- [Wikipedia: Naive Bayes Classifiers](https://en.wikipedia.org/wiki/Naive_Bayes_classifier)