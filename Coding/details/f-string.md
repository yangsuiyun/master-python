# Python f-string tutorial
Python f-string tutorial shows how to format strings in Python with f-string.
Refer PEP 498 [https://www.python.org/dev/peps/pep-0498/#format-specifiers]

## Python f-string
Python f-string is the newest Python syntax to do string formatting. It is available since Python 3.6. Python f-strings provide a faster, more readable, more concise, and less error prone way of formatting strings in Python.

The f-strings have the f prefix and use {} brackets to evaluate values.
Format specifiers for types, padding, or aligning are specified after the colon character; for instance: f'{price:.3}', where price is a variable name.

    f ' <text> { <expression> <optional !s, !r, or !a> <optional : format specifier> } <text> ... '

## Python string formatting
The following example summarizes string formatting options in Python.

formatting_strings.py
```python
#!/usr/bin/env python3

name = 'Peter'
age = 23

print('%s is %d years old' % (name, age))
print('{} is {} years old'.format(name, age))
print(f'{name} is {age} years old')
```
The example formats a string using two variables.

    print('%s is %d years old' % (name, age))
This is the oldest option. It uses the % operator and classic string format specifies such as %s and %d.

    print('{} is {} years old'.format(name, age))
Since Python 3.0, the format() function was introduced to provide advance formatting options.

    print(f'{name} is {age} years old')
Python f-strings are available since Python 3.6. The string has the f prefix and uses {} to evaluate variables.

    $ python formatting_string.py
    Peter is 23 years old
    Peter is 23 years old
    Peter is 23 years old
    We have the same output.

Python f-string expressions
We can put expressions between the {} brackets.

expressions.py
```python
#!/usr/bin/env python3

bags = 3
apples_in_bag = 12

print(f'There are total of {bags * apples_in_bag} apples')
````
The example evaluates an expression inside f-string.

    $ python expressions.py
    There are total of 36 apples
This is the output.

## Python f-string dictionaries
We can work with dictionaries in f-strings.

dicts.py
```python
#!/usr/bin/env python3

user = {'name': 'John Doe', 'occupation': 'gardener'}

print(f"{user['name']} is a {user['occupation']}")
```
The example evaluates a dictionary in an f-string.

    $ python dicts.py
    John Doe is a gardener
This is the output.

## Python multiline f-string
We can work with multiline strings.

multiline.py
```
#!/usr/bin/env python3

name = 'John Doe'
age = 32
occupation = 'gardener'

msg = (
    f'Name: {name}\n'
    f'Age: {age}\n'
    f'Occupation: {occupation}'
)

print(msg)
```
The example presents a multiline f-string. The f-strings are placed between square brackets; each of the strings is preceded with the f character.

    $ python multiline.py
    Name: John Doe
    Age: 32
    Occupation: gardener
This is the output.

## Python f-string calling function
We can also call functions in f-strings.

call_function.py
```
#!/usr/bin/env python3

def mymax(x, y):

    return x if x > y else y

a = 3
b = 4

print(f'Max of {a} and {b} is {mymax(a, b)}')
```
The example calls a custom function in the f-string.

    $ python call_fun.py
    Max of 3 and 4 is 4
This is the output.

## Python f-string objects
Python f-string accepts objects as well; the objects must have either __str__() or __repr__() magic functions defined.

objects.py
```
#!/usr/bin/env python3

class User:
    def __init__(self, name, occupation):
        self.name = name
        self.occupation = occupation

    def __repr__(self):
        return f"{self.name} is a {self.occupation}"

u = User('John Doe', 'gardener')

print(f'{u}')
```
The example evaluates an object in the f-string.

    $ python objects.py
    John Doe is a gardener
This is the output.

## Python f-string escaping characters
The following example shows how to escape certain characters in f-strings.

escaping.py
```python
#!/usr/bin/env python3

print(f'Python uses {{}} to evaludate variables in f-strings')
print(f'This was a \'great\' film')
```
To escape a curly bracket, we double the character. A single quote is escaped with a backslash character.

    $ python escaping.py
    Python uses {} to evaludate variables in f-strings
    This was a 'great' film
This is the output.

## Python f-string format datetime
The following example formats datetime.

format_datetime.py
```python
#!/usr/bin/env python3

import datetime

now = datetime.datetime.now()

print(f'{now:%Y-%m-%d %H:%M}')
```
The example displays a formatted current datetime. The datetime format specifiers follow the : character.

    $ python format_datetime.py
    2019-05-11 22:39
This is the output.

## Python f-string format floats
Floating point values have the f suffix. We can also specify the precision: the number of decimal places. The precision is a value that goes right after the dot character.

format_floats.py
```python
#!/usr/bin/env python3

val = 12.3

print(f'{val:.2f}')
print(f'{val:.5f}')
```
The example prints a formatted floating point value.

    $ python format_floats.py
    12.30
    12.30000
The output shows the number having two and five decimal places.

## Python f-string format width
The width specifier sets the width of the value. The value may be filled with spaces or other characters if the value is shorter than the specified width.

format_width.py
```python
#!/usr/bin/env python3

for x in range(1, 11):
    print(f'{x:02} {x*x:3} {x*x*x:4}')
```
The example prints three columns. Each of the columns has a predefined width. The first column uses 0 to fill shorter values.

    $ python format_width.py
    01   1    1
    02   4    8
    03   9   27
    04  16   64
    05  25  125
    06  36  216
    07  49  343
    08  64  512
    09  81  729
    10 100 1000
This is the output.

Python f-string justify string
By default, the strings are justified to the left. We can use the > character to justify the strings to the right. The > character follows the colon character.

justify.py
```python
#!/usr/bin/env python3

s1 = 'a'
s2 = 'ab'
s3 = 'abc'
s4 = 'abcd'

print(f'{s1:>10}')
print(f'{s2:>10}')
print(f'{s3:>10}')
print(f'{s4:>10}')
```
We have four strings of different length. We set the width of the output to ten characters. The values are justified to the right.

    $ python justify.py
            a
            ab
        abc
        abcd
This is the output.

## Python f-string numeric notations
Numbers can have various numeric notations, such as decadic or hexadecimal.

format_notations.py
```python
#!/usr/bin/env python3

a = 300

# hexadecimal
print(f"{a:x}")

# octal
print(f"{a:o}")

# scientific
print(f"{a:e}")
```
The example prints a value in three different notations.

    $ python format_notations.py
    12c
    454
    3.000000e+02
This is the output.

## 自定义格式：对齐、宽度、符号、补零、精度、进制等
f-string采用 {content:format} 设置字符串格式，其中 content 是替换并填入字符串的内容，可以是变量、表达式或函数等，format 是格式描述符。采用默认格式时不必指定 {:format}，如上面例子所示只写 {content} 即可。

关于格式描述符的详细语法及含义可查阅Python官方文档，这里按使用时的先后顺序简要介绍常用格式描述符的含义与作用：

对齐相关格式描述符
格式描述符 |含义与作用
----------| ------------
< | 左对齐（字符串默认对齐方式）
> |右对齐（数值默认对齐方式）
^ |居中

数字符号相关格式描述符
格式描述符 |含义与作用
----------| ------------
+ |负数前加负号（-），正数前加正号（+）
- |负数前加负号（-），正数前不加任何符号（默认）
（空格） ||负数前加负号（-），正数前加一个空格
注：仅适用于数值类型。

数字显示方式相关格式描述符
格式描述符 |含义与作用
----------| ------------
# |切换数字显示方式
注1：仅适用于数值类型。
注2：# 对不同数值类型的作用效果不同，详见下表：

数值类型 |不加#（默认）|加#|区别
----------| ------------ |----------|-----
二进制整数 | '1111011'|'0b1111011'|开头是否显示 0b
八进制整数|'173'|'0o173'|开头是否显示 0o
十进制整数|'123'|'123'|无区别
十六进制整数（小写字母）|'7b'|'0x7b'|开头是否显示 0x
十六进制整数（大写字母）|'7B'|'0X7B'|开头是否显示 0X

宽度与精度相关格式描述符
格式描述符|含义与作用
---------|-----------
width|整数 width 指定宽度
0width|整数 width 指定宽度，开头的 0 指定高位用 0 补足宽度
width.precision|整数 width 指定宽度，整数 precision 指定显示精度
注1：0width 不可用于复数类型和非数值类型，width.precision 不可用于整数类型。

注2：width.precision 用于不同格式类型的浮点数、复数时的含义也不同：用于 f、F、e、E 和 % 时 precision 指定的是小数点后的位数，用于 g 和 G 时 precision 指定的是有效数字位数（小数点前位数+小数点后位数）。

注3：width.precision 除浮点数、复数外还可用于字符串，此时 precision 含义是只使用字符串中前 precision 位字符。

示例：

    >>> a = 123.456
    >>> f'a is {a:8.2f}'
    'a is   123.46'
    >>> f'a is {a:08.2f}'
    'a is 00123.46'
    >>> f'a is {a:8.2e}'
    'a is 1.23e+02'
    >>> f'a is {a:8.2%}'
    'a is 12345.60%'
    >>> f'a is {a:8.2g}'
    'a is  1.2e+02'

    >>> s = 'hello'
    >>> f's is {s:8s}'
    's is hello   '
    >>> f's is {s:8.3s}'
    's is hel     '


千位分隔符相关格式描述符
格式描述符|含义与作用
---------|----------
,|使用,作为千位分隔符
_|使用_作为千位分隔符
注1：若不指定 , 或 _，则f-string不使用任何千位分隔符，此为默认设置。

注2：, 仅适用于浮点数、复数与十进制整数：对于浮点数和复数，, 只分隔小数点前的数位。

注3：_ 适用于浮点数、复数与二、八、十、十六进制整数：对于浮点数和复数，_ 只分隔小数点前的数位；对于二、八、十六进制整数，固定从低位到高位每隔四位插入一个 _（十进制整数是每隔三位插入一个 _）。

示例：

    >>> a = 1234567890.098765
    >>> f'a is {a:f}'
    'a is 1234567890.098765'
    >>> f'a is {a:,f}'
    'a is 1,234,567,890.098765'
    >>> f'a is {a:_f}'
    'a is 1_234_567_890.098765'

    >>> b = 1234567890
    >>> f'b is {b:_b}'
    'b is 100_1001_1001_0110_0000_0010_1101_0010'
    >>> f'b is {b:_o}'
    'b is 111_4540_1322'
    >>> f'b is {b:_d}'
    'b is 1_234_567_890'
    >>> f'b is {b:_x}'
    'b is 4996_02d2'

格式类型相关格式描述符
基本格式类型

格式描述符|含义与作用|适用变量类型
---------|---------|-----------
s|普通字符串格式|字符串
b|二进制整数格式|整数
c|字符格式，按unicode编码将整数转换为对应字符|整数
d|十进制整数格式|整数
o|八进制整数格式|整数
x|十六进制整数格式（小写字母）|整数
X|十六进制整数格式（大写字母）|整数
e|科学计数格式，以 e 表示 ×10^|浮点数、复数、整数（自动转换为浮点数）
E|与 e 等价，但以 E 表示 ×10^|浮点数、复数、整数（自动转换为浮点数）
f|定点数格式，默认精度（precision）是6|浮点数、复数、整数（自动转换为浮点数）
F|与 f 等价，但将 nan 和 inf 换成 NAN 和 INF|浮点数、复数、整数（自动转换为浮点数）
g|通用格式，小数用 f，大数用 e|浮点数、复数、整数（自动转换为浮点数）
G|与 G 等价，但小数用 F，大数用 E|浮点数、复数、整数（自动转换为浮点数）
%|百分比格式，数字自动乘上100后按 f 格式排版，并加 % 后缀|浮点数、整数（自动转换为浮点数）
常用的特殊格式类型：标准库 datetime 给定的用于排版时间信息的格式类型，适用于 date、datetime 和 time 对象

格式描述符|含义|显示样例
---------|----|-------
%a|星期几（缩写）|'Sun'
%A|星期几（全名）|'Sunday'
%w|星期几（数字，0 是周日，6 是周六）|'0'
%u|星期几（数字，1 是周一，7 是周日）|'7'
%d|日（数字，以 0 补足两位）|'07'
%b|月（缩写）|'Aug'
%B|月（全名）|'August'
%m|月（数字，以 0 补足两位）|'08'
%y|年（后两位数字，以 0 补足两位）|'14'
%Y|年（完整数字，不补零）|'2014'
%H|小时（24小时制，以 0 补足两位）|'23'
%I|小时（12小时制，以 0 补足两位）|'11'
%p|上午/下午|'PM'
%M|分钟（以 0 补足两位）|'23'
%S|秒钟（以 0 补足两位）|'56'
%f|微秒（以 0 补足六位）|'553777'
%z|UTC偏移量（格式是 ±HHMM[SS]，未指定时区则返回空字符串）|'+1030'
%Z|时区名（未指定时区则返回空字符串）|'EST'
%j|一年中的第几天（以 0 补足三位）|'195'
%U|一年中的第几周（以全年首个周日后的星期为第0周，以 0 补足两位）|'27'
%w|一年中的第几周（以全年首个周一后的星期为第0周，以 0 补足两位）|'28'
%V|一年中的第几周（以全年首个包含1月4日的星期为第1周，以 0 补足两位）|'28'

综合示例

    >>> a = 1234
    >>> f'a is {a:^#10X}'      # 居中，宽度10位，十六进制整数（大写字母），显示0X前缀
    'a is   0X4D2   '

    >>> b = 1234.5678
    >>> f'b is {b:<+10.2f}'    # 左对齐，宽度10位，显示正号（+），定点数格式，2位小数
    'b is +1234.57  '

    >>> c = 12345678
    >>> f'c is {c:015,d}'      # 高位补零，宽度15位，十进制整数，使用,作为千分分割位
    'c is 000,012,345,678'

    >>> d = 0.5 + 2.5j
    >>> f'd is {d:30.3e}'      # 宽度30位，科学计数法，3位小数
    'd is           5.000e-01+2.500e+00j'

    >>> import datetime
    >>> e = datetime.datetime.today()
    >>> f'the time is {e:%Y-%m-%d (%a) %H:%M:%S}'   # datetime时间格式
    'the time is 2018-07-14 (Sat) 20:46:02'
