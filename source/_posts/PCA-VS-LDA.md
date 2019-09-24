---
title: PCA VS. LDA
date: 2019-07-05 14:34:48
tags:
---

## What's PCA?

Principal Component Analysis is to maximize variance of dataset. It's based on the idea that some features may be more important/diversified than others. 

1. PCA is a unsupervised algorithm, while LDA is supervised.



## How to use them?

```python
from sklearn.decomposition import PCA
pca = PCA(n_components=2)
feature_reduced = pca.fit(X).transform(X)
```
```python
from sklearn.decomposition import LDA
lda = LDA(n_components=2)
feature_reduced = lda.fit(X).transform(X)
```



[reference](https://sebastianraschka.com/faq/docs/lda-vs-pca.html)
[reference2](http://www.vfirst.com/blog/techfirst/dimension-reduction-techniques-pca-vs-lda-in-machine-learning-part-2/)