# Marshal

Marshal module exists to mainly support reading and writing the pseudo-compiled code for Python modules of .pyc files. It's a low-level serialization module. \
If you need to serialize general objects use pickle or json.

The code object is NOT compatible between Python versions.

Python object -> marshal.dumps() -> bytes -> marshal.loads() -> Python object

```python
import marshal

data = {
    "name": "Alice",
    "scores": [10, 20, 30]
}

encoded = marshal.dumps(data)

print(encoded)
# b'\xfb\xda\x04name\xda...'  binary representation

decoded = marshal.loads(encoded)

print(decoded)
# {'name': 'Alice', 'scores': [10, 20, 30]}
```

Python needs a quick way to save certain internal objects, especially code objects. When Python compiles a module, roughly this happens

source.py -> compile() -> Python code object -> marshal() -> \_\_pycache\_\_/source.pyc

```python
import marshal

code = compile(
    "print('Hello!')",
    "<example>",
    "exec"
)

data = marshal.dumps(code)

restored_code = marshal.loads(data)

exec(restored_code)
```

## Parsing .pyc files

.pyc File format looks like:

```
+--------------------------+
| .pyc header (16 bytes)   |
+--------------------------+
| marshalled code object   |
| marshal.load(...)        |
+--------------------------+
```

Example file which we'll compile to a .pyc file.

```python
# hello.py
def add(a, b):
    return a + b

x = add(10, 20)
print(x)
```

Note .pyc files are created automatically for all imported modules. In this case we have to compile it manually:

```
python -m py_compile .\hello.py
```

Retrieve the code object from the pyc file:

```python
import marshal

pyc_file = "__pycache__/hello.cpython-313.pyc"

with open(pyc_file, "rb") as f:
    header = f.read(16)
    code = marshal.load(f)

print(code)
```

```python
print(code)
print(code.co_name)     # <module>
print(code.co_filename) # .\hello.py
print(code.co_names)    # ('add', 'x', 'print')
print(code.co_consts)   # (<code object add at 0x000002058153C570, file ".\hello.py", line 2>, 10, 20, None)
```

Note that co\_consts, contains another code ojbect:

```python
import types

for const in code.co_consts:
    if isinstance(const, types.CodeType):
        print("Function:", const.co_name)        # Function: add
        print("Names:", const.co_names)          # Names: ()
        print("Constants:", const.co_consts)     # Constants: (None,)
```

To see the actual byte code:

```python
import dis

dis.dis(code)
```

```
  0           RESUME                   0

  2           LOAD_CONST               0 (<code object add at 0x000002058153C570, file ".\hello.py", line 2>)
              MAKE_FUNCTION
              STORE_NAME               0 (add)

  5           LOAD_NAME                0 (add)
              PUSH_NULL
              LOAD_CONST               1 (10)
              LOAD_CONST               2 (20)
              CALL                     2
              STORE_NAME               1 (x)

  6           LOAD_NAME                2 (print)
              PUSH_NULL
              LOAD_NAME                1 (x)
              CALL                     1
              POP_TOP
              RETURN_CONST             3 (None)

Disassembly of <code object add at 0x000002058153C570, file ".\hello.py", line 2>:
  2           RESUME                   0

  3           LOAD_FAST_LOAD_FAST      1 (a, b)
              BINARY_OP                0 (+)
              RETURN_VALUE
```

