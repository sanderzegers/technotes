# Lists

A list is like an array in other languages such as C and Java. Except:

* can grow and shrink dynamically
* can contain values of different types
* stores references to objects (rather than the objects themselves

```python
# Creating lists
numbers = [1, 2, 3]
mixed = ["beep", 4, -1.2]
values = [0, 1, "two", 3, 4, 5, 6, 7]

# Create a list from a range
nums = list(range(10))
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

chars = list("hello world")
# ['h', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l', 'd']

# Length
len(nums)               # 10

# Add an item
values.append(8)
# [0, 1, "two", 3, 4, 5, 6, 7, 8]

# Indexing
mixed[-2]               # 4
# Negative indexes count from the end:
# -1 = last item, -2 = second-last item

# Slicing: [start:stop]
values[3:5]             # [3, 4]
# Includes start index (3)
# Excludes stop index (5)

alphabet = [chr(a) for a in (range(65,65+26))]
#['A','B','C','D','E','F','G',...,'Z']

alphabet[0]
# 'A'

alphabet[:3]
#['A', 'B', 'C']

alphabet[4:6]
#['E', 'F']

alphabet[::-1]
#['Z','Y','X','W',...,'A']

alphabet[::3]
#['A', 'D', 'G', 'J', 'M', 'P', 'S', 'V', 'Y']

# Core Rule: sequence[start:stop:step]

# Check whether a value exists
-1.2 in mixed           # True


# Assignment does NOT copy a list
other = nums

id(other) == id(nums)   # True
# Both variables refer to the same list object.

other.append(10)
nums                     # [..., 8, 9, 10]

# Make a shallow copy instead
copy = nums.copy()

id(copy) == id(nums)    # False
copy == nums             # True


# Remove all items
values.clear()
values                   # []
```

| Method             | What it does                                    | Example                |
| ------------------ | ----------------------------------------------- | ---------------------- |
| `append(x)`        | Add one item to the end                         | `items.append(4)`      |
| `extend(iterable)` | Add multiple items to the end                   | `items.extend([4, 5])` |
| `insert(i, x)`     | Insert an item at index `i`                     | `items.insert(1, "x")` |
| `remove(x)`        | Remove the first occurrence of `x`              | `items.remove(3)`      |
| `pop()`            | Remove and **return** the last item             | `items.pop()`          |
| `pop(i)`           | Remove and **return** item at index `i`         | `items.pop(0)`         |
| `clear()`          | Remove all items                                | `items.clear()`        |
| `index(x)`         | Return the index of the first occurrence of `x` | `items.index("a")`     |
| `count(x)`         | Count how many times `x` occurs                 | `items.count(3)`       |
| `sort()`           | Sort the list in place                          | `items.sort()`         |
| `reverse()`        | Reverse the list in place                       | `items.reverse()`      |
| `copy()`           | Create a shallow copy                           | `copy = items.copy()`  |

| Operation             | Example              |
| --------------------- | -------------------- |
| Number of items       | `len(items)`         |
| Check membership      | `"a" in items`       |
| Access by index       | `items[0]`           |
| Slice                 | `items[1:4]`         |
| Delete by index/slice | `del items[2]`       |
| Create a sorted copy  | `sorted(items)`      |
| Iterate               | `for item in items:` |

