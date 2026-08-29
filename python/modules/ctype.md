# Ctype

The built-in ctype library allows you to call functions from compiled c-libraries: .dll (windows), .so (linux) and .dylib (mac).

Hello world in Linux:

```python
import ctypes

libc = ctypes.CDLL("libc.so.6") 

libc.printf(b"Hello from C!\n")
```

Linux GetPID():

```python
import ctypes

libc = ctypes.CDLL("libc.so.6")

libc.getpid.restype = ctypes.c_int # declare return type
libc.getpid.argtypes = [] # declare arguments

pid = libc.getpid()
print("Process ID:", pid)
```

Windows show Tickcount:

```python
import ctypes

kernel32 = ctypes.WinDLL("kernel32", use_last_error=True)

kernel32.GetTickCount64.argtypes = []
kernel32.GetTickCount64.restype = ctypes.c_ulonglong

milliseconds = kernel32.GetTickCount64()

print("Windows has been running for:")
print(milliseconds, "milliseconds")
print(milliseconds / 1000, "seconds")
```

Windows show messagebox:

```python
import ctypes

user32 = ctypes.WinDLL("user32", use_last_error=True)

user32.MessageBoxW.argtypes = [
    ctypes.c_void_p,
    ctypes.c_wchar_p,
    ctypes.c_wchar_p,
    ctypes.c_uint,
]

user32.MessageBoxW.restype = ctypes.c_int

result = user32.MessageBoxW(
    None,
    "Hello from Python!",
    "ctypes example",
    0
)

print("Result:", result)
```

Windows beep:

```python
import ctypes

kernel32 = ctypes.WinDLL("kernel32")

kernel32.Beep.argtypes = [
    ctypes.c_uint,
    ctypes.c_uint,
]

kernel32.Beep.restype = ctypes.c_int

for a in range(400,3000,100):
    kernel32.Beep(a, 60)
```

{% hint style="info" %}
Leaving out the argument and return types might work in some cases, since `ctypes` falls back to `c_int` by default. However, this can cause a loss of precision, for example when working with 64-bit integers.

Additionally, when argument types are defined, values can be converted and validated before the C function is called.
{% endhint %}

### Python - C type mapping

| `c_bool`                     | `_Bool` / `bool`                   |
| ---------------------------- | ---------------------------------- |
| `c_char`                     | `char`                             |
| `c_byte`                     | `signed char`                      |
| `c_ubyte`                    | `unsigned char`                    |
| `c_short`                    | `short`                            |
| `c_ushort`                   | `unsigned short`                   |
| `c_int`                      | `int`                              |
| `c_uint`                     | `unsigned int`                     |
| `c_long`                     | `long`                             |
| `c_ulong`                    | `unsigned long`                    |
| `c_longlong`                 | `long long`                        |
| `c_ulonglong`                | `unsigned long long`               |
| `c_float`                    | `float`                            |
| `c_double`                   | `double`                           |
| `c_longdouble`               | `long double`                      |
| `c_char_p`                   | `char *`                           |
| `c_wchar`                    | `wchar_t`                          |
| `c_wchar_p`                  | `wchar_t *`                        |
| `c_void_p`                   | `void *`                           |
| `None` as `restype`          | `void` return type                 |
| Python `None` as an argument | often `NULL` for pointer arguments |

### Pointers

kernel32.GetPhysicallyInstalledSystemMemory takes a pointer as an argument and stores the retrieved information at the referenced memory location.

Use ctypes.byref in this case.

<pre class="language-python"><code class="lang-python">import ctypes

kernel32 = ctypes.WinDLL("kernel32")

value = ctypes.c_uint()

<strong>kernel32.GetPhysicallyInstalledSystemMemory(
</strong><strong>    ctypes.byref(value)
</strong><strong>)
</strong>
print("Installed memory:", value.value/1024/1024, "GB")
</code></pre>

### Structures

Windows retrieve system time.

Function: [https://learn.microsoft.com/en-us/windows/win32/api/sysinfoapi/nf-sysinfoapi-getsystemtime](https://learn.microsoft.com/en-us/windows/win32/api/sysinfoapi/nf-sysinfoapi-getsystemtime)

```c
void main()
{
    SYSTEMTIME st, lt;
    
    GetSystemTime(&st);
    GetLocalTime(&lt);
    
    printf("The system time is: %02d:%02d\n", st.wHour, st.wMinute);
    printf(" The local time is: %02d:%02d\n", lt.wHour, lt.wMinute);
}
```

Structure: [https://learn.microsoft.com/en-us/windows/win32/api/minwinbase/ns-minwinbase-systemtime](https://learn.microsoft.com/en-us/windows/win32/api/minwinbase/ns-minwinbase-systemtime)

```c
typedef struct _SYSTEMTIME {
  WORD wYear;
  WORD wMonth;
  WORD wDayOfWeek;
  WORD wDay;
  WORD wHour;
  WORD wMinute;
  WORD wSecond;
  WORD wMilliseconds;
} SYSTEMTIME, *PSYSTEMTIME, *LPSYSTEMTIME;
```

<pre class="language-python"><code class="lang-python">import ctypes

<strong>class SYSTEMTIME(ctypes.Structure):
</strong>    _fields_ = [
        ("wYear", ctypes.c_ushort),
        ("wMonth", ctypes.c_ushort),
        ("wDayOfWeek", ctypes.c_ushort),
        ("wDay", ctypes.c_ushort),
        ("wHour", ctypes.c_ushort),
        ("wMinute", ctypes.c_ushort),
        ("wSecond", ctypes.c_ushort),
        ("wMilliseconds", ctypes.c_ushort),
    ]

kernel32 = ctypes.WinDLL("kernel32")

<strong>kernel32.GetSystemTime.argtypes = [
</strong><strong>    ctypes.POINTER(SYSTEMTIME)
</strong><strong>]
</strong>
kernel32.GetSystemTime.restype = None


system_time = SYSTEMTIME()

kernel32.GetSystemTime(
    ctypes.byref(system_time)
)

print("Year:",system_time.wYear)
print("Month:",system_time.wMonth)
print("Day:",system_time.wDay)
print("Hour:",system_time.wHour)
print("Minute:",system_time.wMinute)
</code></pre>

### Arrays

C Array definitions look a bit different compared to regular Python arrays.

<pre class="language-python"><code class="lang-python">import ctypes

<strong>IntArray5 = ctypes.c_int * 5
</strong>numbers = IntArray5(10, 20, 30, 40, 50)

for number in numbers:
    print(number)
</code></pre>

Note the `ctype.c_int*5`, it defines the array size and member type.

&#x20;
