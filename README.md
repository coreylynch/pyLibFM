# Factorization Machines in Python

This is a pure python implementation of Factorization Machines [1]. This uses stochastic gradient descent with adaptive regularization as a learning method, which adapts the regularization automatically while training the model parameters. See [2] for details. From libfm.org: "Factorization machines (FM) are a generic approach that allows to mimic most factorization models by feature engineering. This way, factorization machines combine the generality of feature engineering with the superiority of factorization models in estimating interactions between categorical variables of large domain."

[1] Steffen Rendle (2012): Factorization Machines with libFM, in ACM Trans. Intell. Syst. Technol., 3(3), May.
[2] Steffen Rendle: Learning recommender systems with adaptive regularization. WSDM 2012: 133-142

## Dependencies
* numpy
* sklearn

## Tip
The easiest way to use this class is to represent your training data as lists of standard Python dict objects, where the dict elements map each instance's categorical and real valued variables to its values. Then use a [sklearn DictVectorizer](http://scikit-learn.org/dev/modules/generated/sklearn.feature_extraction.DictVectorizer.html#sklearn.feature_extraction.DictVectorizer) to convert them to a one-of-K or “one-hot” coding.

Here's an example 
```python
In [21]: import numpy as np

In [22]: from sklearn.feature_extraction import DictVectorizer

In [23]: train = [
   ....:     {"user": "1", "item": "5", "age": 19},
   ....:     {"user": "2", "item": "43", "age": 33},
   ....:     {"user": "3", "item": "20", "age": 55},
   ....:     {"user": "4", "item": "10", "age": 20},
   ....: ]

In [24]: v = DictVectorizer()

In [25]: X = v.fit_transform(train)

In [26]: print X.toarray()
[[ 19.   0.   0.   0.   1.   1.   0.   0.   0.]
 [ 33.   0.   0.   1.   0.   0.   1.   0.   0.]
 [ 55.   0.   1.   0.   0.   0.   0.   1.   0.]
 [ 20.   1.   0.   0.   0.   0.   0.   0.   1.]]

In [27]: y = np.repeat(1.0,X.shape[0])

In [28]: fm = FM(learn_rate = 0.01, num_factors=10, num_iter=1,
   ....:         param_regular=(0,0,0.1))

In [29]: fm.fit(X,y)

In [30]: fm.predict(v.transform({"user": "1", "item": "10", "age": 24}))
Out[30]: 6.5658623991470835
```

(Cython implementation in the works)
