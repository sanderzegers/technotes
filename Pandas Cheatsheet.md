# Pandas Cheatsheet

- **Series:** A one-dimensional labeled array that can hold data of any type.
- **DataFrame:** A two-dimensional table with labeled axes (rows and columns).





## Creating and Loading DataFrames

Creating DataFrames

```python
import pandas as pd

# From dictionary
data = {'A': [1, 2, 3], 'B': [4, 5, 6]}
df = pd.DataFrame(data)

# From list of lists
data = [[1, 4], [2, 5], [3, 6]]
df = pd.DataFrame(data, columns=['A', 'B'])
```



File Management

```
# Reading
df = pd.read_csv('data.csv')

# Saving

df.to_csv('out.csv',sep=';',index=False,encoding='utf-8-sig')
```





## DataFrame Inspection

```
df.head()             # First 5 rows
df.tail(10)           # Last 10 rows
df.shape              # Dimensions (rows, columns)
df.info()             # Summary of data
df.describe()         # Summary statistics
df.columns            # Column names
df.dtypes             # Data types of columns
df.isnull().sum()     # Count missing values per column


val = df.loc[494, "srcip"] # Get srcip column at row 494
```



## DataFrame Manipulation

```python
# Concat multiple tables (list of tables), ignore list name
pd.concat(tables,ignore_index=True)

# Add new column
df = df.rename(columns={"old_name": "new_name", "a": "A"})

# Set value based on another column
df["dstport_udp"] = df["dstport"].where(df["Protocol"].eq(17)).astype("Int64")

# Drop columns
df = df.drop(columns=["dstport", "dstport"])

# Drop duplicate columns
df = df.drop_duplicates()

# Order by column
df = df.sort_values(by="col")
```









Sources: https://www.almabetter.com/bytes/cheat-sheet/pandas

