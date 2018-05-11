


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
  - all ones: `np.ones((n1,n2,...,nk))`
  - all zeros: `np.zeros((n1,n2,...,nk))`
  - NOTE: np.ones((3,2)) will create a 3rx2c array, so what might np.ones((3,2,2)) return? I don't know about you
  but my expectation would be to see 2 copies of the (3,2)-shaped array since I added the extra dimension to something
  I already looked at... But np.ones((3,2,2)) will, in fact, return 3 copies of np.ones((2,2)).  Play with it; you'll
  see what I mean.
* Size of array: `myArr.size`
* Data type of array: `myArr.dtype`
* Set array data type: e.g., `np.ones((k,m), dtype='int32')


