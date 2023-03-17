---
% this is metadata, feel free to remove it
tags: python, numpy
---
# NumPy array

NumPy is the foundational library for scientific computing in Python. It introduces the fundamental `ndarray` object, encapsulating *n*-dimensional arrays for items of the same data type.

An instance of an ndarray consists in a contiguous one-dimensional segment of computer memory combined with an indexing scheme map, arranging the items of an N-dimensional array in a 1-dimensional block[\[1\]](https://numpy.org/doc/stable/reference/arrays.ndarray.html#internal-memory-layout-of-an-ndarray). The range in which the indices can vary is given by the `numpy.shape` of the array. How many bites each item takes and how they are interpreted are defined by a data-type `numpy.dtype` object, making up the [memory layout](https://numpy.org/doc/stable/reference/arrays.ndarray.html#id1) of the 1-d segment, describing as well which item takes which part of the segment. Hence, a data-type object holds the information on how the bytes in the fixed-size block of memory corresponding to an array item should be interpreted.

An ndarray is *homogeneous*, as in, every item takes the same memory space and they are interpreted in the same way according to their type.
Extracting an item by indexing, this will be represented by an **array-scalar** [\[2\]](https://numpy.org/doc/stable/reference/arrays.scalars.html#arrays-scalars).
>[!tip] Ndarray
>- An ndarray is a fixed-size multidimensional container of items of the same type and memory size.
>- The type of the items is represented by a separate data-type object (`dtype`).
>- The number of dimensions is defined by the `shape`, a tuple of N non-negative integers.
![[Pasted image 20230306231956.png]]

Libraries such as Pandas and SciPy, Matplotlib are built on top of NumPy, such as machine learning ones as TensorFlow.

Why Array over than lists?
- Accepts only one data type
	- Less space in memory
	- No type mismatch when doing our calculations

## Creation routines
Documentation: [Creation routines](https://numpy.org/doc/stable/reference/routines.array-creation.html#routines-array-creation)
```py
# Creates array of with 5 rows and 3 columns with 0-valued elements
np.zeros((5,3))

# Creates array of with 5 rows and 3 columns with 1-valued elements
np.ones((2,3))

# Creates array with integers within a range, with distance 1
np.arange(-3, 4) # array([-3, -2, -1, 0, 1, 2, 3])

# Creates array with integers within a range, with distance 3
np.arange(-3, 4, 3) # array([-3, 0, 3])

# Creates array with random floats within 0 and 1
np.random.random((2,4))
```

## Shape manipulation
In principle, NumPy arrays can be N-D arrays. Hence, they can represent easily a [[Tensor]].

```py
array = np.array([1, 2], [5, 7], [6, 6])
array.flatten() # array([1, 2, 5, 7, 6, 6])

array.reshape((2,3)) # array([1, 2, 5], [7, 6, 6])

```

## Array conversion
...
## Item selection and manipulation
...


# Filtering arrays
> Fancy indexing approach.

Say we want to know which class of a school has an even number of students.
```py
# We want to return the classroom ids (1st column) with even number of students (2nd column)
classroom_ids_and_sizes = np.array([1,22], [2,21], [3,27], [4,26])

# first, we create a mask to filter out the columns with even values
classroom_ids_and_sizes[:, 1] % 2 == 0

# Then we index the first column using the mask
classroom_ids_and_sizes[:,0][classroom_ids_and_sizes[:, 1] % 2 == 0]
> array([1,4])
```

### Filtering with fancy indexing vs. `np.where()`

| **Fancy indexing**        | `np.where()`                                                  |
| ------------------------- | ------------------------------------------------------------- |
| Returns array of elements | returns array of indices                                      |
|                           | can create array based on whether elements meet the condition |


We can replace values in an array with `np.where()`.
For example, we might want to substitute 0 with `""` in a sudoku game matrix.
```py
np.where(sudoku_game==0, "", sudoku_game)
```


Currently, the stump diameter and trunk diameter values in `tree_census` are in two different columns. Living trees have a stump diameter of zero while stumps have a trunk diameter of zero. If you'd like to include both living trees and stumps in certain research calculations, it might be useful to have their diameters together in just one column.

`numpy` is loaded as `np`, and the `tree_census` array is available. As a reminder, the tree census **columns in order refer to a tree's ID, its block ID, its trunk diameter, and its stump diameter.**
Create and print a 1D array called `trunk_stump_diameters`, which replaces a tree's trunk diameter with its stump diameter if the trunk diameter is zero.
```py
# Create and print a 1D array of tree and stump diameters

trunk_stump_diameters = np.where(tree_census[:,2]==0, tree_census[:,3], tree_census[:,2])

print(trunk_stump_diameters)
```

# Add/remove data
### Concatenation
It refers to adding data to any array, along any existing axis
$$
\begin{equation}
	\begin{array}{|c|c|c|}
		\hline 18 & 12 & 3 \\
		\hline 6 & 7 & 8 \\
		\hline 11 & 15 & 13 \\
		\hline

	\end{array}
	\hspace{10px} + \hspace{10px}
	\begin{array}{|c|c|}
		\hline 7 & 1 \\
		\hline 23 & 18 \\
		\hline 4 & 11 \\
		\hline
	\end{array}
	\hspace{10px} = \hspace{10px}
	\begin{array}{|c|c|c|c|c|}
		\hline 18 & 12 & 3 & 7 & 1\\
		\hline 6 & 7 & 8 & 23 & 18\\
		\hline 11 & 15 & 13 & 4 & 11\\
		\hline
	\end{array}
\end{equation}
$$
>[!attention] ValueError
>For array $x_{n,m}$ and array $y_{k,j}$ where $k\neq n$ and $j\neq m$ the concatenation will throw a `ValueError`

By default, the concatenation happens along the axis-0 (adding a row) if the array we're trying to add is compatible with both axis sizes.
In order to concatenate information along the second axis (adding a column), we must specify the `axis=1` property.
```py
classroom_ids_and_sizes = np.array([1,22], [2,21], [3,27], [4,26])
grade_levels_and_teachers = np.array([[1, "James"], [1, "George"], [3, "Amy"], [3, "Meehir"]])

np.concatenate((classroom_ids_and_sizes, grade_levels_and_teachers), axis=1)
```

In order to append an array (`shape(n,)`) to a matrix (`shape(n,m)`) we need to reshape it first to a `shape(n,1)`.
```py
one_d = np.array([7, 23, 4])
two_d = np.array([1,22], [2,21], [3,27])
print(one_d.shape) # shape(3,1)

one_d.reshape((3,1)) # array([[1], [2], [3]])

np.concatenate((two_d, one_d))
```

### Deletion

```py
data=np.array([[1, 22, 1, "James"],
				[2, 21, 1, "George"],
				[3, 27, 3, "Amy"],
				[4, 26, 3, "Meehir"]])
np.delete(data, 1, axis=0)
# array([[1, 22, 1, "James"],
		 [3, 27, 3, "Amy"],
		 [4, 26, 3, "Meehir"]])
```

## Vectorization

Vectorization refers to the process of performing mathematical operations on entire arrays or matrices at once, rather than iterating over each element in a loop. This can result in significant performance improvements, particularly when working with large arrays.

For example, consider the following code snippet that adds two arrays element-wise using a loop:

```py
import numpy as np

a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

result = np.zeros_like(a)
for i in range(len(a)):
    result[i] = a[i] + b[i]

```

This code creates two arrays, `a` and `b`, and then uses a loop to add the corresponding elements together and store the result in a third array called `result`. While this code works, it can be slow when working with large arrays.

Now, consider the same operation performed using vectorization:


```py
import numpy as np

a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

result = a + b

```

This code achieves the same result as the previous example, but it does so using vectorized addition. In this case, NumPy performs the addition operation on the entire `a` and `b` arrays at once, and returns a new array called `result`.

In summary, NumPy's vectorization capability allows for more efficient and concise manipulation of large arrays, making it a powerful tool for scientific computing and data analysis in Python.

You can transform regular functions into vectorized functions.

```py
import numpy as np
names = np.array(["John", "Helen", "Lisa"])
vectorized_upper= np.vectorize(str.upper)

print(vectorized_upper(names)) # Output: array(["JOHN", "HELEN", "LISA"])
```