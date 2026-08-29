# Tuples

A tuple is a collection which is ordered and unchangeable.

Faster as lists.

Tuples are hashable. So they can become keys of a dictionary, or members of a set

```python
mapping = {}
mapping[("key1","key2")] = "value"
```

```python
single_tuple_memeber = (43,)
# (43)

coordinates = (10, 20, 30)
coordinates[0]     # 10
coordinates[-1]    # 30
coordinates[:2]    # (10, 20)
coordinates[::-1]  # (30, 20, 10)

#unpack tuples
x, y, z = coordinates
# x=10, y=20, z=30

first, *middle, last = (1, 2, 3, 4, 5)
# first = 1
# middle = [2, 3, 4]
# last = 5


# useful operations:
numbers = (1, 2, 2, 3)

len(numbers)       # 4
2 in numbers       # True
numbers.count(2)   # 2
numbers.index(3)   # 3
numbers + (4, 5)   # creates a new tuple
numbers * 2        # creates a repeated tuple

list((1, 2, 3))               
# [1, 2, 3]

for index, value in enumerate(("A", "B")):
     print(index,value)     
#0 A
#1 B

list(zip(("A", "B"), (1, 2)))
#[('A','1'),('B','2')]
```

```python
# Assign variables names to tuple values. Values are still constansts.

from typing import NamedTuple

class Point(NamedTuple):
    x: int
    y: int

point = Point(10, 20)

point.x       # 10
point[0]      # 10
hash(point)   # works
```
