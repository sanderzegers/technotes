# Enums

Enums are useful when a value should be one of a small, predefined set of choices.

Python provides enums through the standard-library `enum` module.

### `IntEnum`

`IntEnum` creates enum members that also behave like integers.

```python
from enum import IntEnum

class FirewallAddressType(IntEnum):
    SUBNET = 1
    RANGE = 2
    FQDN = 3
```

Access a member:

```python
address_type = FirewallAddressType.SUBNET
```

Because `IntEnum` members behave like integers:

```python
FirewallAddressType.SUBNET == 1  # True

isinstance(FirewallAddressType.SUBNET, FirewallAddressType)  # True
isinstance(1, FirewallAddressType)                            # False
isinstance(FirewallAddressType.SUBNET, int)                  # True
```

### Name and value

Every enum member has a `.name` and `.value`:

```python
address_type = FirewallAddressType.SUBNET

address_type.name   # "SUBNET"
address_type.value  # 1
```

### Get a member from its value

Call the enum class with a value:

```python
FirewallAddressType(1)
# <FirewallAddressType.SUBNET: 1>

FirewallAddressType(3)
# <FirewallAddressType.FQDN: 3>
```

An unknown value raises `ValueError`:

```python
FirewallAddressType(4)
# ValueError: 4 is not a valid FirewallAddressType
```



### Get a member from its name

Use square brackets to look up an enum member by name:

```python
FirewallAddressType["SUBNET"]
# <FirewallAddressType.SUBNET: 1>
```

An unknown name raises `KeyError`:

```python
FirewallAddressType["HOST"]
# KeyError: 'HOST'
```

### Printing and representation

One slightly surprising behavior of `IntEnum` is that printing a member prints its integer value:

```python
print(FirewallAddressType.SUBNET)
# 1
```

Use `.name` when you want the symbolic name:

```python
print(FirewallAddressType.SUBNET.name)
# SUBNET
```

Or `repr()` to see the complete enum representation:

```python
print(repr(FirewallAddressType.SUBNET))
# <FirewallAddressType.SUBNET: 1>
```

Behind the scenes, `print()` calls `str()` on the object, which uses its `__str__()` method, while `repr()` uses the object's `__repr__()` method to produce a more detailed developer-oriented representation.

### Iterating over an enum

Enums can be iterated over:

```python
for address_type in FirewallAddressType:
    print(address_type.name, address_type.value)
```

Output:

```
SUBNET 1
RANGE 2
FQDN 3
```

### `Enum` vs `IntEnum`

Use `Enum` when the values should be distinct symbolic constants:

```python
from enum import Enum


class FirewallAddressType(Enum):
    SUBNET = 1
    RANGE = 2
    FQDN = 3
```

With a normal `Enum`:

```python
FirewallAddressType.SUBNET == 1  # False
```

With `IntEnum`:

```python
FirewallAddressType.SUBNET == 1  # True
```

A good rule of thumb:

| Type      | Use when                                                                      |
| --------- | ----------------------------------------------------------------------------- |
| `Enum`    | The values are symbolic choices and should stay separate from normal integers |
| `IntEnum` | The enum also needs to work with code that expects integer values             |

Prefer `Enum` unless integer compatibility is specifically useful, for example when dealing with APIs, database values, protocols, or existing numeric constants.

### Common operations

| Operation       | Example                                  |
| --------------- | ---------------------------------------- |
| Access member   | `FirewallAddressType.SUBNET`             |
| Get name        | `FirewallAddressType.SUBNET.name`        |
| Get value       | `FirewallAddressType.SUBNET.value`       |
| Lookup by value | `FirewallAddressType(1)`                 |
| Lookup by name  | `FirewallAddressType["SUBNET"]`          |
| Compare members | `value == FirewallAddressType.SUBNET`    |
| Check enum type | `isinstance(value, FirewallAddressType)` |
| Iterate members | `for value in FirewallAddressType:`      |
