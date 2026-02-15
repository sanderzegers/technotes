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



Load from CSV

```
df = pd.read_csv('data.csv')
```





## Data Inspection

```
df.head()             # First 5 rows
df.tail(10)           # Last 10 rows
df.shape              # Dimensions (rows, columns)
df.info()             # Summary of data
df.describe()         # Summary statistics
df.columns            # Column names
df.dtypes             # Data types of columns
df.isnull().sum()     # Count missing values per column
```







Sources: https://www.almabetter.com/bytes/cheat-sheet/pandas

