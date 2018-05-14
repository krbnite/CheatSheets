


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
  - from vector: e.g., `myArr = np.arange(start,stop,step); myArr.shape = (k,n)`
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
