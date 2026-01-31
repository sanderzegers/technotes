# Regex Cheatsheet



### Characters

| Syntax | Description                                                  | Example      |
| ------ | ------------------------------------------------------------ | ------------ |
| \d     | Digits                                                       | [0-9]        |
| \D     | Not a digit (depends on regex engine)                        |              |
| \w     | Word character. ASCII letter, digit or underline, unicode chars (ä,æ,ß, etc) [^1] | [A-Za-z0-9_] |
| \W     | Not a word character                                         |              |
| \s     | whitespace characters (including \n)                         |              |
| \h     | horizontal whitespace (tab or unicode space separator)       |              |
| \v     | vertical whitespaces                                         |              |
| [\x41] | Ascii char in hexadecimal format                             | A            |
| .      | Matches any character                                        |              |

[^1]: Unicode chars inclusion depends on regex engine.

### Anchors and boundaries

| Syntax | Description     | Example |
| ------ | --------------- | ------- |
| ^      | Start of string |         |
| $      | End of string   |         |
| \b     | word boundry    |         |



### Quantifiers

| Syntax | Description              | Example       |
| ------ | ------------------------ | ------------- |
| a?     | Zero or One of a         |               |
| a*     | Zero or more of a        |               |
| a+     | One or more of a         |               |
| a+?    | One ore more of a (lazy) |               |
| a*?    | Zero or more of a (lazy) | ```\[.*?\]``` |
| a{1,5} | Between one or five a's  |               |
| a{3,}  | At least three a's       |               |
| a{5}   | Exactly 5 a's            |               |



### Pattern collection

| Syntax      | Description                                               | Example |
| ----------- | --------------------------------------------------------- | ------- |
| [A-Z]       | Matches any pattern with uppercase characters from a to z |         |
| [a-z]       |                                                           |         |
| [0-9a-zA-Z] |                                                           |         |
| [^dfg]      | Matches any characters but not 'd', 'f' or 'g'            |         |



### Lookarounds

| Syntax   | Description         | Example |
| -------- | ------------------- | ------- |
| (?=...)  | Positive lookahead  |         |
| (?<=...) | Positive lookbehind |         |
| (?!...)  | Negative lookahead  |         |
| (?<!...) | Negative lookbehind |         |



### Capture Groups

| Syntax         | Description              | Example |
| -------------- | ------------------------ | ------- |
| (...)          | Capture group            |         |
| (?:..)         | Non capture group        |         |
| (?\<name\>...) | Named capture group      |         |
| \1             | Backreference to group 1 |         |

