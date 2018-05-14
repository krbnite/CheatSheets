


* What version of NumPy: `np.__version__`
* Convert a list to a NumPy array: `np.array([1,2,3]) # also works for tuples`
* Quickly create a vector:  
  - index-like vector:  `np.arange(start,stop,step)`
    * np.arange uses python's clopen interval convention, [start,stop) 
  - evenly-spaced float vector:  `np.linspace(start,stop,num=50)`
    * np.linspace use a closed interval, [start,stop]
    * set endPoint=False for clopen
  - all ones: `np.ones(num)`
  - all zeros: `np.zeros(num)`

* Create a more general array
  - from vector: e.g., `myArr = np.arange(start,stop,step).reshape(k,n)`
  - all ones: `np.ones((n1,n2,...,nk))`
  - all zeros: `np.zeros((n1,n2,...,nk))`
  - NOTE: np.ones((3,2)) will create a 3rx2c array, so what might np.ones((3,2,2)) return? I don't know about you
  but my expectation would be to see 2 copies of the (3,2)-shaped array since I added the extra dimension to something
  I already looked at... But np.ones((3,2,2)) will, in fact, return 3 copies of np.ones((2,2)).  Play with it; you'll
  see what I mean.
    * ex: (3,4,4) is the tuple representation of 3 4x4 arrays
    * ex: (5,5,2) is the tuple representation of 5 5x2 arrays

* Size of array: `myArr.size`
* Shape of an array: `myArr.shape`
* Number of array dimensions (rank): `myArr.ndim`
* Data type of array: `myArr.dtype`
* Set array data type: e.g., `np.ones((k,m), dtype='int32')`

* Array Indexing:  
  - e.g., 2D array `myArr[2:4,-3:-1]`
  - NOTE: array index intervals are clopen (e.g., 2:4 = [2,3])
  - e.g., 3D array
    * return ith 2D subarray: `myArr[i]`
    * return jth row of ith 2D subarray:  `myArr[i,j]`
    * return kth col of jth row of it 2D subarray:  `myArr[i,j,k]`
* Boolean Mask
  - any shape array
  - some conditional: 
    * e.g., `mask1 = (myArr % 3) == 0`
    * e.g., `mask2 = np.logical_and(myArr > 2, myArr < 7)`
  - np.logical_and: NumPy functions are optimized for speed, so this is preferred over `&` operator
  - True Subset: `myArr[mask]`
  - False Subset: `myArr[~mask]`
  - NOTE: returned subsets are 1D sequences
* Broadcasting
  - Some of it is "what you expect", e.g., "broadcasting a scalar over a vector" is just scalar multiplication
  - For other things, it generalizes this: a scalar can broadcast over a 2D array b/c it can be considered a 1x1 2D array
  that fits an integer amount of times along the rows and cols
    * you have to play with it a bit for it all to make sense
    * basically, a (1,6) and (1,3) array are incompatible, even though one might think you can fit the (1,3) array twice into the (1,6) array)
    * that said, (1,6) and (3,1) work together: in this case we get a (3,6) array where the rows are the (1,6) array multiplied by the components of the (3,1) array
  - Broadcasting a function, like sum(), over an axis is simply choosing a "collapse axis"
    * simplest to think of 2D array here: want to sum along the columns, leaving a row vector; or sum along rows, leaving a col vector?
* Structured arrays
  - arrays of structs
  - first you define your data type: `new_data_type = [('name','s6'),('age','f8'),('rank','i8')]`
  - then you create an array w/ this dtype: `ppl = np.zeros(3, dtype=new_data_type)`
  - populate it: e.g., `ppl[0]=('Jeff',56,4); ppl[1]=('Tom',61,5); ppl[2]=('Jimmy',23,2)`
  - this is kind of like building your own Pandas DataFrames
    * `ppl['name']`
    * `ppl['age']`
    * `ppl[['name','rank']]`
* Multi-Dimensional Structured Arrays
  - what is important to understand is that the data type you create takes the place of `int32` or `float64`, etc
  - this means that you can use that data type in any shaped array (if you have a use case!)
* Record Arrays
  - record arrays are structured arrays with just a touch more functionality
  - in code, the record array is the result of a thin wrapper over structured arrays
  - basically, the functionality given is the ability to access attributes using the "dot method" in addition to the "index method"
    * `ppl['name']`
    * `ppl.name`


## Same, View, or Copy
For some data types or in another language, you might have defined a new variable using an old
variable, like so `new_var = old_var`, with the intent of doing some processing on `new_var` and
the desire to keep `old_var` hanginging around unchanged.  Well, for this behavior on numpy arrays,
the proper syntax is `new_var = old_var.copy()`, otherwise you just have to variable names pointing to
the same array in memory.  

You can test these things w/ Python's `is` operator or `id()` function.
```
newArr1 = oldArr
newArr2 = oldArr.copy()

# "is" they the same?! :-p
newArr1 is oldArr
newArr2 is oldArr

# Do they have the same memory ID?
print(
  id(oldArr),
  id(newArr1),
  id(newArr2)
)
```

So `.copy()` is obviously important...  What is `.view()`?  

The `view` method allows you to change the data type, while keeping the arrays linked together... e.g.:
```python
a = np.array([[1,2,3]],dtype='int32')
b = a.view(dtype='float32')
b[0,0]=0
a[0]
  0
```

## Stacking Arrays
### Append two arrays
Seems to me this fcn flattens both arrays and appends them as lists.
```
w = np.array([[1,2,3],[4,5,6]])
np.append(w, [7,8,9])
  array([1,2,3,4,5,6,7,8,9])
np.append(w, [[7,8,9],[10,11,12]])
  array([1,2,3,4,5,6,7,8,9,10,11,12])
```

If your goal was to append \[7,8,9\] as a 3rd row in w, you must chain in the `.reshap()` method
or use the axis parameter:
```python
w = np.array([[1,2,3],[4,5,6]])
nrows, ncols = w.shape
np.append(w, [7,8,9]).reshape(nrows+1, ncols)

# If using axis parameter, [7,8,9] must be same rank array as w --> [[7,8,9]]
np.append(w, [[7,8,9]], axis=0)
```

This last method is referred to as "vertically stacking" the arrays (i.e., adding rows in 2D case).  There is a 
convenience function for it:
```python
np.vstack( (w,[[7,8,9]]) ) # vstack takes in tuple of arrays
```

Similarly, one can "horizontall stack" arrays (e.g., add columns in 2D case) using `np.hstack()`.

## Deleting Stuff
Pretty simple: you have an array, you choose an axis, and you choose the index to delete along that axis...  The
result of `np.delete(...)` is a new array.  In other words, if you want to keep the result, you have to save it 
in a new var name (or over the old var name).

```
arr = np.arrary([[1,2,3],[4,5,6],[7,8,9]])
arr
  array([[1,2,3],
         [4,5,6],
         [7,8,9]])

# Delete zeroth subarray along 0th axis (i.e., zeroth row of 2D array arr)
np.delete(arr, 0, axis=0)
  array([[4,5,6],
         [7,8,9]])

# Did anything happen to arr? (No)
arr
  array([[1,2,3],
         [4,5,6],
         [7,8,9]])

# Make new array where last row and col are deleted
new_arr = np.delete(
    np.delete(arr, 2, axis=0),
    2, axis=1)
new_arr
  array([[1,2],
         [4,5]])
```

## Insertions
There is a function, `np.insert(...)`, that works similarly to `np.delete(...)` but for overwriting portions of 
an array (again, outputing a new array, not inserting "in place").

## More General Tools for Joining/Splitting Arrays
We've covered `np.append`, `np.hstack`, `np.vstack`, `np.insert`, and `np.delete`...  
There are other convenience functions for this as well: `np.stack`, `np.split`, `np.concatenate`....

