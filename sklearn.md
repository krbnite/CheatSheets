
* [Training, Validation, and Test Sets](#datasplits)
* [Preprocessing](#preprocessing)
* [Modeling](#models)
  - [Supervised Learning](#models_supervised)
    * [Linear Regression](#models_linreg)
    * [Logistic Regression](#models_logreg)
    * [Support Vector Machines](#models_svm)
    * [Naive Bayes](#models_nbayes)
    * [kNN](#models_knn)
  - [Unsupervised Learning](#models_unsupervised)
    * [PCA](#models_pca)
    * [k-Means](#models_kmeans)



<a name="datasplits"></a>
# Training, Validation, and Test Sets
[train_test_split](http://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html)
```python
from sklearn.model_selection import train_test_split
x_trn, x_other, y_trn, y_other = train_test_split(x, y, train_size=0.7, random_state=0)
x_val, x_tst, y_val, y_tst = train_test_split(x_other, y_other, test_size=0.33, random_state=1)
```
<a name="preprocessing"></a>
# Preprocessing
For many machine learning models, various preprocessing techniques not only help
improve efficiency, but often are important for ensuring meaningful results.
* [StandardScaler](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html) (aka Z-score)
* [Normalizer](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html) (vector normalization)
* [Binarizer](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Binarizer.html)
The typical process is:
1. Choose appropriate preprocessing method and import it
2. Construct a rescale object by fitting the chosen method to the training set only!
3. Transform your training, validation, and test sets using constructed rescale object.
```python
# Example:  Standarization / Z Scoring
#   -- the procedure is the same for Normalizer and Binarizer
from sklearn.preprocessing import StandardScaler
rescale = StandardScalar.fit(x_trn)
xx_trn = rescale.transform(x_trn)
xx_val = rescale.transform(x_val)
xx_tst = rescale.transform(x_tst)
```

<a name="models"></a>
# Modeling
Scikit-Learn makes modeling really easy.  The recipe is:
1. Construct
2. Fit
3. Predict
4. Evaluate

In the subsections that follow, we cover the first 3 steps.  
Model evaluation is covered in the next section.

<a name="models_supervised"></a>
## Supervised Learning

<a name="models_linreg"></a>
### Linear Regression
[Function Documentation](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html)
```python
from skearn.linear_model import LinearRegression
model = LinearRegression()
model.fit(xx_trn, y_tr)
y_pred = model.predict(xx_val)
```

<a name="models_logreg"></a>
### Logistic Regression
[Function Documentation](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)
```python
from skearn.linear_model import LogisticRegression
model = LogisticRegression()
model.fit(xx_trn, y_tr)
y_pred = model.predict(xx_val)
```

<a name="models_svm"></a>
### Support Vector Machines
[Documentation](http://scikit-learn.org/stable/modules/svm.html)
```python
from skearn.svm import SVC
model = SVC(kernel='linear') # other kernels: polynomial, rbf, sigmoid
model.fit(xx_trn, y_tr)
y_pred = model.predict(xx_val)
```

<a name="models_nbayes"></a>
### Naive Bayes
```python
from skearn.naive_bayes import GaussianNB
model = GaussianNB()
model.fit(xx_trn, y_tr)
y_pred = model.predict(xx_val)
```

<a name="models_knn"></a>
### kNN
```python
from skearn.neighbors import KNeighborsClassifier
model = KNeighborsClassifier(n_neighbors=3)
model.fit(xx_trn, y_tr)
y_pred = model.predict(xx_val)
```

<a name="models_unsupervised"></a>
## Unsupervised Learning

<a name="models_pca"></a>
### PCA
```python
from skearn.decomposition import PCA
model = PCA(n_components=0.95)
model.fit(xx_trn, y_tr)
#y_pred = model.predict(xx_val)
```

<a name="models_kmeans"></a>
### k-Means Clustering
```python
from skearn.cluster import KMeans
model = KMeans(n_clusters=3, random_state=1)
model.fit(xx_trn, y_tr)
#y_pred = model.predict(xx_val)
```
