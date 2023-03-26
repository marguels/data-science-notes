# Tabular data
Tabular data is a type of structured data that is organized into rows and columns, similar to a table. Each row represents a single record or instance of data, while each column represents a specific attribute or feature of that record.

In tabular data, each column has a unique name, which is used to label the data in that column. The rows may also be labeled, but this is not required. The values in each cell of the table represent the data for a specific record and attribute combination.

Tabular data is commonly used in a variety of applications, including databases, spreadsheets, and statistical analysis. It is often used for storing and organizing large amounts of data, making it easy to search, filter, and manipulate data. It is also a common format for exchanging data between different software systems.

Pandas is a popular data manipulation and analysis library in Python for tabular data. 
Pandas is built on NumPy, so we need to first import NumPy.
```py
import numpy as np
import pandas as pd
```

# Panda's data structures
It provides two main data structures for handling data - Series and DataFrame.
Both Series and DataFrame provide a wide range of methods and functions for data manipulation and analysis, such as filtering, grouping, aggregation, merging, joining, and more. Pandas also supports various data input/output formats, including CSV, Excel, SQL databases, and more, making it a powerful tool for data manipulation and analysis in Python.

## Series
A **Series** is a one-dimensional array-like object that can hold any data type such as integers, floats, strings, etc. It is similar to a column in a spreadsheet or a SQL table. Each element in a Series has a unique axis label, commonly referred to as *index*, used to access the data. 
```py
s = pd.Series(data, index=index)
```
The param `data` can be either a Python list, NumPy array, a dictionary, or a scalar value. All data in the series needs to be of the same type. The passed `index` is a list of axis labels. Thus, this separates into a few cases depending on what data is:

| data type | index                                                           |
| --------- | --------------------------------------------------------------- |
| `ndarray` | list (if none is passed, it will create `[0,..., len(data)-1]`) |
| `dict`    | index will be a list of `key`                                   |
| `scalar`  | list with length equal to scalar number                         | 

```python
# Creation from an ndarray
# Specifying index
s = pd.Series(np.random.randn(5), index=["a", "b", "c", "d", "e"])
s.index
>>> Index(['a', 'b', 'c', 'd', 'e'], dtype='object')
# Not specifying the index
pd.Series(np.random.randn(5))

# Creation from a dict
# Index will be inferred from the dict keys
d = {"b": 1, "a": 0, "c": 2}
pd.Series(d)

# Creation from a scalar
pd.Series(5.0, index=["a", "b", "c", "d", "e"])
```

1. *Series is ndarray-like*
	Most of the NumPy functions can be applied to Series, with some exceptions in behaviour. Slicing series will also slice the index.
	When working with raw NumPy arrays, looping through value-by-value is usually not necessary thanks to [[NumPy#Vectorization | Vectorization]]. The same is true when working with `Series` in pandas. `Series` can also be passed into most NumPy methods expecting an ndarray.

2. *Series is dict-like*
	You can get values by passing the index label. If the label is not containet, it raises an exception.

## DataFrame
A **DataFrame** is a 2-dimensional table-like data structure, where each column can have a different data type. It is similar to a spreadsheet or a SQL table. It consists of rows and columns, where each row represents an observation or sample, and each column represents a feature or variable. A DataFrame can be created from a Python dictionary, NumPy array, or from an external data source like a CSV file.

In a DataFrame we can identify two axis, used in many DataFrame's methods to specify whether you want it to operate over a column or a row.
![[df_axis.png]]

# Common operations with DataFrames

## Creating a DataFrame
We can create DataFrames from a variety of objects using the `pd.DataFrame()` method.
```python
# From a List of lists
data = [[1, 2, "A"], [3, 4, "B"]]
df = pd.DataFrame(data, columns = ["col1", "col2", "col3"])

# From a Dictionary
data = {'col1': [1, 2],
		'col2': [3, 4],
		'col3': ["A", "B"]}		
df = pd.DataFrame(data=data)
```

## Reading/Creating a CSV

CSV (Comma Separated Values) is an easy and lightweight way to store data. They are typically the most prevalent file format to read Pandas DataFrames from.
```python
# Reads CSV from file
# specify the separator if it's not a comma
# specify the columns if needed
df = pd.read_csv("file.csv", sep=";", columns=["A", "B"])

# Write data to a CSV file
df.to_csv("file.csv", sep = "|", index = False)
```

```python
# Returns summary of dataframe with:
# count, unique, top, frequency
df.describe()

# Drop rows and columns from a dataframe
# axis=1 -> drops columns and values in it
# axis=0 -> drops indices and associated rows
new_df = df.drop(columns = [''], axis=1)
```

## Inspecting the DataFrame
With the `.head(N)` and `.tail(N)` methods, we print respectively the first and last `N` rows of the DataFrame. By default, if we don't pass the  parameter `N`, they will print the first/last 5 rows.
```python
# Prints the first 5 rows
df.head()
# Prints the last 8 rows
df.tail(8)
```

### DataFrame .info()
If we want to check if there are missing values, column types and memory usage of the data in the DataFrame, the method `df.info()` summarises all of the above: 

```python
In[1]: df.info()
Out[1]: <class 'pandas.core.frame.DataFrame'>
	RangeIndex: 10 entries, 0 to 9
	Data columns (total 3 columns):
	# Column Non-Null Count Dtype
	 --- ------ -------------- -----
	  0   col1   10 non-null    int8
	  1   col2   10 non-null    int64
	  2   col3   10 non-null    object
	dtypes: int64(1), int8(1), object(1)
	memory usage: 298.0+ bytes
```

### DataFrame .describe()
Pandas has a method to describe and print standard statistics of every numeric-valued column: `.describe()`

```python
In[2]:  df.describe()
Out[2]: 
	   col1   col2
 count 10.00 10.00
 mean  10.00 11.00
 std   6.06   6.06
 min   1.00   2.00
 25%   5.50   6.50
 50%   10.00 11.00
 75%   14.50 15.50
 max   19.00 20.00
```

## Fill NaN values

## Copy issue with .drop()

```python
# Gets the i-th row of the dataframe
df.iloc[i]
```
