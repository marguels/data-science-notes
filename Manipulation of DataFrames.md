---
tags: data_science, pandas, dataframes
---
# Joins in tabular data
>Combines data from two tables based on matching observations in both tables

## One-to-one and One-to-many relationships
> [!TIP] One-to-one & One-to-many
> - **One-to-one**: In a one-to-one relationship, one record in a table is associated with one and only one record in another table.
> - **One-to-many**: In a one-to-many relationship, one record in a table can be associated with one or more records in another table.

Example:
-   A `customer` table with information about each customer
-   A `cust_tax_info` table with customers unique tax IDs
-   An `orders` table with information about each order
-   A `products` table with details about each unique product sold
-   An `inventory` table with information on how much total inventory is available to sell for each product

| One-to-one                   | One-to-many            |
| ---------------------------- | ---------------------- |
| `customer <-> cust_tax_info` | `orders ->> products`  |
| `products <-> inventory`     | `customers ->> orders` |

## Merge method and joins
One can use the `.merge()` method to join two table which share a column with matching values. Because of the arbitrary choice of the column, these values might not be *unique*, and they might occur differently in different tables. The `.merge()` method expects one [[One-to-one]] relationship, but this is more of a [[One-to-many]], or [[many-to-many]].
If we use `.merge()` on a single column with duplicates values in table_a, which are unique in table_b, we will repeat table_b's values to match them in number with those of table_a.
```python
		table_a.merge(table_b, on=['column_with_duplicates', 'other_column_with_duplicates'])
```
Running the code above will take into account the match of both columns. If there isn't a match in both columns, it won't consider it a match and it won't duplicates values in table_b.

## Inner join
Inner join will return rows that have matching values in both tables, with all the columns from both tables.

```python
		table_a.merge(table_b, on='key', suffixes=('_a', '_b'))
```
The columns of `table_a` will be on the left, while `table_b` will be on the right.
If DataFrame `table_a` has a column with the same name as one of `table_b`, the `df.merge()` method will append a default suffix (`_x`, `_y`), which can be customised for easier use.
The `on` property of the `merge()` method can read also dataFrame's indexes, and can take multiple indexes into account.

If the `key` column is named differently in the two tables, in the merge method we can specify which column of the left and right to pick.
```python
		table_a.merge(table_b, left_on='key_a', right_on='key_b')
```

## Left join
By default the merge method performs an Inner join, returning only the rows of data with matching values in the `key` columns of both tables.
However, given a `left_df` and a `right_df`, we can perform a left join.

>[!TIP] Left join
> A left join returns all the rows from the `left_df` and only those rows from the `right_df` where key columns match.
```python
				left_df.merge(right_df, on='key', how='left')
```
The output of a One-to-many merge with a left join will have greater than or equal rows than the left table.
Similarly, we can perform a *Right join*, the mirror opposite of it.

## Outer join
An outer join will return all the rows from both tables, regardless if there is a match or not.
```python
				left_df.merge(right_df, on='key', how='outer')
```

## Self join
>[!TIP] Self join
>When do you need to merge a table to itself?
>It might happen that we have data points that are related, not in a bilateral way.

Common examples:
- Hierarchical relationships
- Sequential relationships
- Graph data
```python
		self_df.merge(self_df, left_on='id_next', right_on='id',
						suffixes=('_first', '_next'))
```
This will return only data which finds `id_next` values in the `id` column, so data that is related. If we want to keep all data, but still spell out the relationship in columns, we need to take a Left join.

## Filtering joins
Filter observations from table based on whether or not they match an observation in another table.

Panda does not provide direct support for filtering joins, but we can replicate them.
To explain them, the following sample data sets are used:

###### `genres`

| gid | name        |
| --- | ----------- |
| 1   | Rock        |
| 2   | Jazz        |
| 3   | Metal       |
| 4   | Alternative |
| ... | ...         |

###### `top_tracks`

| tid | name            | aid | mtid | gid | composer        |
| --- | --------------- | --- | ---- | --- | --------------- |
| 1   | For those...    | 1   | 1    | 1   | Angus Young,... |
| 2   | Balls to the... | 2   | 2    | 1   | nan             |
| 3   | Fast as a shark | 3   | 2    | 1   | F. Baltes, S... |
| ... | ...             | ... | ...  | ... | ...             |

## Semi join
A semi join filters the left table down to those observations that have a match in the right table.

Returns the intersection, similar to a Inner join, but only columns from the left table and not the right.
No duplicate rows from the left table are return, even if there is a *One-to-many* relationship.

**Procedure**:
- Merge the left and the right tables on key column using an Inner join.
- Search if the `key` column in the left table is in the merged tables using the `isin()` method creating a Boolean `Series`.
- Subset the rows of the left table.

**Example**:
We want to discover what are the top genres in the `top_tracks` table released by Spotify at the end of 2022.
```python
genres_tracks = genres.merge(top_tracks, on='gid')
semi_join_filter = genres['gid'].isin(genres_tracks['gid'])
top_genres = genres[semi_join_filter]
```
We first merge the two tables with an inner join, then we filter the `genres` table by what's in the `top_tracks` table.

## Anti join
Returns the observation in the left table that do not have a matching observation in the right table.

Returns the intersection, but only columns from the left table and not the right.
**Procedure**:
- Merge the left and the right tables on key column using an Left join, setting the `indicator` to `True`.
- We select the elements that only belong to the left table (from the `indicator`, resulting in a `_merge` column).
- Search if the `key` column in the left table is in the merged tables using the `isin()` method creating a Boolean `Series`.
- Subset the rows of the left table.

**Example**:
We want to find out if there are genres that didn't make it to the top charts.
```python
genres_tracks= genres.merge(top_tracks, on='gid', how='left', indicator=True)
gid_list= genres_tracks.loc[genres_tracks['_merge'] == 'left_only', 'gid']
flop_genres_filter= genres['gid'].isin(gid_list)
flop_genres= genres[flop_genres_filter]
```

To find `gid` that are not present on the `top_tracks` table:
`indicator=True` will add a column called `_merge` to the output. This column tells the source of each row.
We then use the `.loc[]` accessor on `_merge` to select the rows that only appeared on the left table and return only the `'gid'`column from the `genres_tracks`.

# Concatenation of DataFrames

If we have a list of tables, `table_a`, `table_b`, `table_c`, we can concatenate them by passing a list of them in the `pd.concat()` method.
```python
pd.concat([table_a, table_b, table_c], ignore_index=False, keys=['a','b','c'])
```
The specified keys are associated with the pieces of the original table, resulting in a table with multi-index, with the label on the first level. It can only be used by having `ignore_index` as `False`, since we cannot add a key and ignore the index at the same time.
By default the `concat()` method *includes all the columns*, whether they are in common or not, filling empty values with `NaN`

If we only want matching columns in the tables, we can set the `join` argument to `'inner'`.
```python
	pd.concat([table_a, table_b], join='inner')
```
Default value is `'outer'`.

##### Concatenation with `.append()`
It's a simplified `.concat()` method, which *doesn't* support `keys` nor `join`, as it's always set to `'outer'`.

## Verify integrity
Often, real world data is not clean. When we perform a merge, we do it on tables which have One-to-one relationships. However, one of the columns we're merging on might have a duplicated value, turning the relationship on a One-to-many, or Many-to-many.
Both `.concat()` and `.append()` have features to verify the structure of our data.

##### `.merge()`

```python
				table_a.merge(table_b, validate=None)
```
To the `validate` argument we can provide the followings key strings:
- `one_to_one`
- `one_to_many`
- `many-to-one`
- `many-to-many`
If we specify the relationship and then it's not validated, then it will throw an error.

##### `.concat()`
```python
		pd.concat([table_a, table_b], verify_integrity=True)
```

## Merging ordered and Time-Series Data

### `merge_ordered()`
```python
pd.merge_ordered(table_a, table_b, left_on='left_key',
				right_on='right_key', how='left' suffixes=('_a', '_b'),
				fill_method='ffill')
```
`ffill` specifies the *forward fill*, filling missing values with the value from the previous row.
### `merge_asof()`
```python
pd.merge_asof(table_a, table_b, on='common_key', suffixes('_a', '_b'),
				direction='forward')
```
`direction` default argument is `backward`.
Setting it to `forward` will allow to select rows on the right table whose `'key'` has value greater than or equal to the left's `key`.
Setting it to `nearest` will return the nearest row in the right table, regardless if it's forward or backwards.
**When to use `merge_asof()`?**
- Data sampled from a process (dates and time may not align)
- Developing a training set (no data leakage)


# Querying data

```python
		table.query('column_a > x || (column_b >= y && column_c =='a')')
```

# Reshaping data
```python
	table_tall=table.melt(id_vars=['columns_not_to_change']
							value_vars=['2018', '2017']
							var_name=['year'], value_name='dollars')
```

##### Example:
`inflation`:

|     | country | indicator   | 2017 | 2018 | 2019 |
| --- | ------- | ----------- | ---- | ---- | ---- |
| 0   | Brazil  | Inflation % | 3.45 | 3.66 | 3.73 |
| 1   | Canada  | Inflation % | 1.60 | 2.27 | 1.95 |
| 2   | France  | Inflation % | 1.03 | 1.85 | 1.11 |
| 3   | India   | Inflation % | 2.49 | 4.86 | 7.66 | 

```python
inflation.melt(id_vars=['country', 'indicator'], var_name='year',
				value_name='annual')
```
Will produce:

|     | country | indicator   | year | annual |
| --- | ------- | ----------- | ---- | ------ |
| 0   | Brazil  | Inflation % | 2017 | 3.45   |
| 1   | Canada  | Inflation % | 2017 | 1.60   |
| 2   | France  | Inflation % | 2017 | 1.03   |
| 3   | India   | Inflation % | 2017 | 2.49   |
| 4   | Brazil  | Inflation % | 2018 | 3.66   |
| 5   | Canada  | Inflation % | 2018 | 2.27   |
| 6   | France  | Inflation % | 2018 | 1.85   |
| 7   | India   | Inflation % | 2018 | 4.86   |
| 8   | Brazil  | Inflation % | 2019 | 3.73   |
| 9   | Canada  | Inflation % | 2019 | 1.95   |
| 10  | France  | Inflation % | 2019 | 1.11   |
| 11  | India   | Inflation % | 2019 | 7.66   | 
