---
title: "How to: PCA in sklearn"
date: 2022-10-31T14:46:05.988Z
summary: A﻿ tutorial about how to perform PCA using sklearn
draft: false
featured: false
tags:
  - how to
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
# PCA in sklearn

```python
import numpy as np
from sklearn.decomposition import PCA
from sklearn import datasets
digits = datasets.load_digits()
X = digits.data
y = digits.target
X.shape,y.shape
```

    ((1797, 64), (1797,))

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y, random_state = 666)
X_train.shape, X_test.shape
```

    ((1347, 64), (450, 64))

## Determine the final dimension by explaining the variance

`pca = PCA(n_components=0.8)`

- `n_components` indicates the final number of dimensions to keep
- If 0<`n_components`<1, select the dimension automatically: select the number of components such that the amount of variance that needs to be explained is greater than the percentage specified by n_components

```python
pca = PCA(n_components=0.8)
pca.fit(X_train)

print(pca.explained_variance_ratio_)

print(pca.singular_values_)
```

    [0.14566817 0.13735469 0.11777729 0.08499689 0.0586019 0.05115429
     0.04266053 0.03601197 0.03411058 0.03054078 0.02423377 0.02287006
     0.01803046]
    [486.58225959 472.49333601 437.52685721 371.68535734 308.62406795
     288.34670584 263.32194683 241.9342541 235.46074035 222.79939634
     198.46521983 192.80023903 171.18961162]

```python
pca.n_components_
```

    13

## Re-dimension reduction

```python
X_train_reduction = pca.transform(X_train)
X_test_reduction = pca.transform(X_test)
```

```python
X_train_reduction.shape
```

    (1347, 13)
