

## Training, Validation, and Test Sets
[train_test_split](http://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html)
```python
from sklearn.model_selection import train_test_split
x_trn, x_other, y_trn, y_other = train_test_split(x, y, train_size=0.7, random_state=0)
x_val, x_tst, y_val, y_tst = train_test_split(x_other, y_other, test_size=0.33, random_state=1)
```

## Preprocessing
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
# Standarization / Z Scoring
from sklearn.preprocessing import StandardScaler
rescale = StandardScalar.fit(x_trn)
xx_trn = rescale.transform(x_trn)
xx_val = rescale.transform(x_val)
xx_tst = rescale.transform(x_tst)
```
