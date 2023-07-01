# re正则表达式
::: tip 什么是正则表达式
正则表达式（***Regular Expression***）是一种被用于从文本中检索符合某些特定模式的文本。
:::

正则表达式是从左到右来匹配一个字符串的。可以被用来 **替换** 字符串中的文本、验证表单、基于模式匹配从一个字符串中 **提取** 字符串等等。

## 元字符
元字符是正则表达式的基本组成元素。元字符在这里跟它通常表达的意思不一样，而是以某种特殊的含义去解释。有些元字符在写在方括号内时有特殊含义。


| 元字符 | 描述|
| :----: | ----- |
|   .    | 匹配除换行符以外的任意字符。|
|  [ ]   | 字符类，匹配方括号中包含的任意字符。连字符可以指定范围，例如 `[1-9]`、`[a-z]` 。|
|  [^ ]  | 否定字符类。匹配方括号中不包含的任意字符 |
|   \*   | 匹配前面的子表达式零次或多次 |
|   +    | 匹配前面的子表达式一次或多次 |
|   ?    | 匹配前面的子表达式零次或一次，或指明一个非贪婪限定符。 |
| {n,m}  | 花括号，匹配前面字符至少 n 次，但是不超过 m 次。 |
| (xyz)  | 字符组，按照确切的顺序匹配字符 xyz。 |
| &#124; | 分支结构，匹配符号之前的字符或后面的字符。 |
| &#92;  | 转义符，它可以还原元字符原来的含义，允许你匹配保留字符 <code>[ ] ( ) { } . \* + ? ^ $ \ &#124; |
|   ^    | 从匹配行的首字符匹配     |
|   $    | 匹配行的末尾要匹配     |

特殊序列


| 序列 | 描述 |
| -- | -- |
| \b | boundary，匹配一个单词的边界，即开始和结尾 |
| \d | 匹配任意一个十进制数字，等价于[0-9] |
| \D | 匹配任意一个非数字字符，等价于[^0-9] |
| \w | 匹配任意一个字母数字（不区分大小写）或下划线字符等价于[a-zA-Z0-9_] |
| \W | 匹配任意一个非字母数字字符[^a-zA-Z0-9_] |
| \s | 匹配任意一个空白字符（等价于[\f\n\r\t\v]） |
| \S | 匹配任意一个非空白字符（[^\f\n\r\t\v]） |
| `\\` | 匹配反斜杠 |

## 函数
This module exports the following functions:
* **`match`**：     Match a regular expression pattern to the beginning of a string.
* **`fullmatch`**： Match a regular expression pattern to all of a string.
* **`search`**：    Search a string for the presence of a pattern.
* **`sub`**：       Substitute occurrences of a pattern found in a string.
* **`subn`**：      Same as sub, but also return the number of substitutions made.
* **`split`**：     Split a string by the occurrences of a pattern.
* **`findall`**：   Find all occurrences of a pattern in a string.
* **`finditer`**：  Return an iterator yielding a Match object for each match.
* **`compile`**：   Compile a pattern into a Pattern object.
* **`purge`**：     Clear the regular expression cache.
* **`escape`**：    Backslash all non-alphanumerics in a string.

## FUNCTIONS
    compile(pattern, flags=0)
        Compile a regular expression pattern, returning a Pattern object.
    
    escape(pattern)
        Escape special characters in a string.
    
    findall(pattern, string, flags=0)
        Return a list of all non-overlapping matches in the string.
        
        If one or more capturing groups are present in the pattern, return
        a list of groups; this will be a list of tuples if the pattern
        has more than one group.
        
        Empty matches are included in the result.
    
    finditer(pattern, string, flags=0)
        Return an iterator over all non-overlapping matches in the
        string.  For each match, the iterator returns a Match object.
        
        Empty matches are included in the result.
    
    fullmatch(pattern, string, flags=0)
        Try to apply the pattern to all of the string, returning
        a Match object, or None if no match was found.
    
    match(pattern, string, flags=0)
        Try to apply the pattern at the start of the string, returning
        a Match object, or None if no match was found.
    
    purge()
        Clear the regular expression caches
    
    search(pattern, string, flags=0)
        Scan through string looking for a match to the pattern, returning
        a Match object, or None if no match was found.
    
    split(pattern, string, maxsplit=0, flags=0)
        Split the source string by the occurrences of the pattern,
        returning a list containing the resulting substrings.  If
        capturing parentheses are used in pattern, then the text of all
        groups in the pattern are also returned as part of the resulting
        list.  If maxsplit is nonzero, at most maxsplit splits occur,
        and the remainder of the string is returned as the final element
        of the list.
    
    sub(pattern, repl, string, count=0, flags=0)
        Return the string obtained by replacing the leftmost
        non-overlapping occurrences of the pattern in string by the
        replacement repl.  repl can be either a string or a callable;
        if a string, backslash escapes in it are processed.  If it is
        a callable, it's passed the Match object and must return
        a replacement string to be used.
    
    subn(pattern, repl, string, count=0, flags=0)
        Return a 2-tuple containing (new_string, number).
        new_string is the string obtained by replacing the leftmost
        non-overlapping occurrences of the pattern in the source
        string by the replacement repl.  number is the number of
        substitutions that were made. repl can be either a string or a
        callable; if a string, backslash escapes in it are processed.
        If it is a callable, it's passed the Match object and must
        return a replacement string to be used.
    
    template(pattern, flags=0)
        Compile a template pattern, returning a Pattern object

Each function other than purge and escape can take an optional 'flags' argument

consisting of one or more of the following module constants, joined by "|".

A, L, and U are mutually exclusive.
    A  ASCII       For string patterns, make \w, \W, \b, \B, \d, \D
                    match the corresponding ASCII character categories
                    (rather than the whole Unicode categories, which is the
                    default).
                    For bytes patterns, this flag is the only available
                    behaviour and needn't be specified.
    I  IGNORECASE  Perform case-insensitive matching.
    L  LOCALE      Make \w, \W, \b, \B, dependent on the current locale.
    M  MULTILINE   "^" matches the beginning of lines (after a newline)
                    as well as the string.
                    "$" matches the end of lines (before a newline) as well
                    as the end of the string.
    S  DOTALL      "." matches any character at all, including the newline.
    X  VERBOSE     Ignore whitespace and comments for nicer looking RE's.
    U  UNICODE     For compatibility only. Ignored for string patterns (it
                    is the default), and forbidden for bytes patterns.

### 查找
#### findall()
findall(pattern, string, flags=0)
    Return a list of all non-overlapping matches in the string.
    
    If one or more capturing groups are present in the pattern, return
    a list of groups; this will be a list of tuples if the pattern
    has more than one group.
    
    Empty matches are included in the result.

### 编译re表达式
compile(pattern, flags=0)
    Compile a regular expression pattern, returning a Pattern object.


