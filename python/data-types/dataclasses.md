# Dataclasses

You can use dataclasses to store structured data. It removes lot of boilerplate code compared to regular classes.

Use the @dataclass decorator:

```python
from dataclasses import dataclass

@dataclass
class Address:
    street: str
    city: str
    country: str

@dataclass
class User:
    name: str
    age: int
    address: Address
```

Now you automatically get useful behavior such as an initializer and readable representation.

```python
address = Address( "Main Street 10", "Zurich", "Switzerland" )

user = User("Anna", 25)

print(user)
# User( # name='Anna', # age=25, # address=Address(...) # )
```

You effectively avoid writing:

```
class User:
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

Dataclasses are part of the Python standard library.

### Default Values

Fields can have default values:

```python
@dataclass
class User:
    name: str
    age: int = 0
    active: bool = True
```

```python
user = User("Anna")
```

Fields without defaults must come before fields with defaults.

```python
# Good
name: str
age: int = 0
```

### Equality

Dataclasses automatically compare their field values:

```python
@dataclass
class Point:
    x: int
    y: int
    
Point(1, 2) == Point(1, 2)  # True
Point(1, 2) == Point(2, 3)  # False
```

With regular classes, you would normally need to implement `__eq__()` yourself to get this behavior.

***

### Immutable Dataclasses

Use `frozen=True` when the data should not change after creation:

```python
@dataclass(frozen=True)
class Point:
    x: int
    y: int

point = Point(10, 20)

point.x = 5
# FrozenInstanceError
```



Useful for value-like objects that should stay fixed.

## Naming Conventions

Classes normally use `PascalCase`:

```python
class BankAccount:
    pass
```

Variables, attributes, and methods use `snake_case`:

```python
account_balance = 100

def calculate_interest():
    ...
```

