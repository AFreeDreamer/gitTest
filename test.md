[TOC]
# 说明
Python中的文本数据使用str对象或字符串处理。字符串是Unicode代码点的不可变序列。字符串以多种方式编写：
- 单引号：'allows embedded "double" quotes'
- 双引号："allows embedded 'single' quotes".
- 三引号：'''Three single quotes''', """Three double quotes"""

三引号字符串可以跨越多个行——所有相关的空格将被包含在字符串文字中。

字符串字面量是单个表达式的一部分，它们之间只有空格，将隐式地转换为单个字符串字面量。也就是说，(“spam”“eggs”)==“spam eggs”。

有关各种形式的字符串文字(包括支持的转义序列)和r(“raw”)前缀(它禁用了大多数转义序列处理)的更多信息，请参见[String和Bytes literals](file:///D:/Seafile/BigFile/Python/python-3.7.5-docs-html/reference/lexical_analysis.html#strings)。


也可以使用str构造函数从其他对象创建字符串。
由于没有单独的“字符”类型，因此对字符串进行索引将生成长度为1的字符串。也就是说，对于非空字符串s，s [0] == s [0：1]。

也没有可变的字符串类型，但是str.join（）或io.StringIO可以用于从多个片段有效地构造字符串。

版本3.3中的变化:为了向后兼容Python 2系列，在字符串文字上再次允许使用u前缀。它对字符串文字的含义没有影响，不能与r前缀组合。

# 字符串方法
字符串实现所有常见的序列操作以及下面描述的其他方法。
字符串还支持两种格式的字符串格式，一种提供高度的灵活性和自定义性（请参阅str.format（），格式字符串语法和自定义字符串格式），另一种基于C printf样式格式，可处理较小范围的类型。并且正确使用起来稍难，但是在可以处理的情况下通常更快（printf样式的字符串格式设置）。
标准库的“文本处理服务”部分涵盖了许多其他模块，这些模块提供了各种与文本相关的实用程序（包括re模块中的正则表达式支持）。

## str.capitalize()
返回该字符串的一个副本，该副本的第一个字符大写，其余字符小写。

## str.casefold()
返回字符串的casefolded副本。折叠的字符串可用于无大小写的匹配。
大小写折叠类似于小写字母，但更具侵略性，因为它旨在消除字符串中的所有大小写区别。例如，德语小写字母“ß”等效于“ ss”。由于已经是小写字母，所以lower（）对'ß'无效。 casefold（）将其转换为“ ss”。

## str.center(width[, fillchar])
返回以字符串为中心width为宽度的字符串。使用指定的fillchar填充（默认为ASCII空间）。如果width小于或等于len，则返回原始字符串。

## str.count(sub[, start[, end]])
返回子字符串sub在[start，end]范围内不重叠的次数。可选参数start和end解释为切片表示法

## str.encode(encoding="utf-8", errors="strict")
将字符串的编码版本返回为一个字节对象。默认编码是utf-8。错误可以设置不同的错误处理方案。错误的默认是“严格的”,这意味着编码错误会增加一个单字符错误。其他可能的值是“忽略”、“替换”、“xmlcharrefresh place”、“backslashreplace”以及通过codecs.register_error()注册的任何其他名称。对于一个可能的编码列表,请参见section标准编码。

##  str.endswith(suffix[, start[, end]])
如果字符串以指定的后缀结尾，则返回True，否则返回False。后缀也可以是要查找的后缀元组。使用可选的启动，从该位置开始测试。在可选端，停止在该位置进行比较。

## str.expandtabs(tabsize=8)
根据当前列和给定的制表符大小，返回字符串的副本，其中所有制表符都被一个或多个空格替换。每个制表符都会出现制表符位置(默认为8，在列0、8、16等处给出制表符位置)。要展开字符串，将当前列设置为0，并逐个字符检查字符串。如果字符是制表符(\t)，则在结果中插入一个或多个空格字符，直到当前列等于下一个制表符位置。(制表符本身不会被复制。)如果字符是换行(\n)或返回(\r)，则复制它并将当前列重置为零。任何其他字符都将不加更改地复制，并且当前列将增加一个，而不管打印时字符是如何表示的。

```python
>>> '01\t012\t0123\t01234'.expandtabs()
'01      012     0123    01234'
>>> '01\t012\t0123\t01234'.expandtabs(4)
'01  012 0123    01234'
```

## str.find(sub[, start[, end]])
返回在切片s [start：end]中找到子字符串sub的字符串中的最低索引。可选参数start和end解释为切片表示法。如果未找到sub，则返回-1

仅当您需要知道sub的位置时才应使用find（）方法。要检查sub是否是子字符串，请使用in运算符：
```python
>>> 'Py' in 'Python'
True
```

## str.format(*args, **kwargs)
执行字符串格式化操作。调用此方法的字符串可以包含用大括号{}分隔的文本或替换字段。每个替换字段要么包含位置参数的数字索引，要么包含关键字参数的名称。返回字符串的副本，其中每个替换字段都用相应参数的字符串值替换.
```python
>>> "The sum of 1 + 2 is {0}".format(1+2)
'The sum of 1 + 2 is 3
```
当格式化一个数字(整数、浮点数、复数、小数)时。十进制和子类)使用n类型(例如:'{:n}'.format(1234))，函数临时将LC_CTYPE区域设置为LC_NUMERIC区域，以便对localeconv()的decimal_point和1000s_sep字段进行解码(如果它们是非ascii或大于1字节，并且LC_NUMERIC区域设置与LC_CTYPE区域设置不同)。此临时更改将影响其他线程。

## str.format_map(mapping)
类似于str.format(**mapping)，只是映射是直接使用的，而不是复制到一个dict中。
```python
>>> class Default(dict):
...     def __missing__(self, key):
...         return key
...
>>> '{name} was born in {country}'.format_map(Default(name='Guido'))
'Guido was born in country'
```

## str.index(sub[, start[, end]])
与find（）类似，但是在未找到子字符串时引发ValueError。

## str.isalnum()
如果字符串中的所有字符均为字母数字并且至少包含一个字符，则返回true，否则返回false。如果下列之一返回True，则字符c为字母数字：c.isalpha（），c.isdecimal（），c.isdigit（）或c.isnumeric（）。

## str.isalpha()
如果字符串中的所有字符都是字母并且至少有一个字符，则返回true，否则返回false。字母字符是在Unicode字符数据库中定义为“字母”的那些字符，即，具有一般类别属性为“ Lm”，“ Lt”，“ Lu”，“ Ll”或“ Lo”之一的那些字符。请注意，这与Unicode标准中定义的“字母”属性不同。


## str.isascii()
如果字符串为空或字符串中的所有字符均为ASCII，则返回true，否则返回false。 ASCII字符的代码点范围为U + 0000-U + 007F。

## str.isdecimal()
如果字符串中的所有字符均为十进制字符并且至少包含一个字符，则返回true，否则返回false。十进制字符是可用于以10为基数的数字的字符，例如U + 0660，阿拉伯文-印度数字零。十进制字符形式上是Unicode通用类别“ Nd”中的字符。

## str.isdigit()
如果字符串中的所有字符均为数字并且至少包含一个字符，则返回true，否则返回false。数字包括需要特殊处理的十进制字符和数字，例如兼容性上标数字。它涵盖了不能用于​​以10为底的数字的数字，例如Kharosthi数字。形式上，数字是具有属性值Numeric_Type =数字或Numeric_Type =十进制的字符。

## str.isidentifier()
如果根据语言定义，标识符和关键字部分，字符串是有效标识符，则返回true。
使用keyword.iskeyword（）测试保留标识符，例如def和class。

##  str.islower()
如果字符串中所有大小写的字符为小写且至少有一个大小写的字符，则返回true，否则返回false。

## str.isnumeric()
如果字符串中的所有字符均为数字字符，并且至少包含一个字符，则返回true，否则返回false。数字字符包括数字字符，以及所有具有Unicode数值属性的字符，例如U + 2155，俗语分数五分之一。形式上，数字字符是具有属性值Numeric_Type =数字，Numeric_Type =十进制或Numeric_Type =数字的字符。

## str.isprintable()
如果字符串中的所有字符都可打印或字符串为空，则返回true，否则返回false。不可打印字符是Unicode字符数据库中定义为“其他”或“分隔符”的字符，ASCII空格(0x20)除外，它被认为是可打印的。(注意，在这个上下文中，可打印字符是在字符串上调用repr()时不应该转义的字符。它与向sys写入的字符串的处理无关。stdout或sys.stderr。)

## str.isspace()
如果字符串中只有空白字符并且至少有一个字符，则返回true，否则返回false。
如果在Unicode字符数据库(请参阅unicodedata)中，字符的一般类别是Zs(“Separator, space”)，或者其双向类是WS、B或S中的一个，则字符是空白。

## str.istitle()
如果字符串是用大写字母区分大小写的字符串并且至少有一个字符，则返回true，例如，大写字符只能跟在无大小写的字符之后，而小写字母只能跟在大写字符之后。否则返回false。

## str.isupper()
如果字符串中的所有大小写字符都是大写的，并且至少有一个大小写字符，则返回true，否则为false。

## str.join(iterable)
返回一个字符串,它是可遍历字符串的连接。如果在iterable中有任何非字符串值,包括字节对象,就会提出一个类型错误。元素之间的分隔符是提供此方法的字符串。

## str.ljust(width[, fillchar])
返回width宽度字符串中左对齐的字符串。填充是使用指定的fillchar完成的(默认是ASCII空间)。如果宽度小于或等于len(s)，则返回原始字符串。

## str.lower()
返回字符串的副本，将所有大小写字符转换为小写。
使用的小写字母算法在Unicode标准的第3.13节中进行了描述。

## str.lstrip([chars])
返回删除了前导字符的字符串的副本。chars参数是一个指定要删除的字符集的字符串。如果省略或没有，chars参数默认删除空白。chars参数不是前缀;相反，它的所有值的组合都被剥夺了:
```python
>>> '   spacious   '.lstrip()
'spacious   '
>>> 'www.example.com'.lstrip('cmowz.')
'example.com'
```

## static str.maketrans(x[, y[, z]])
此静态方法返回可用于str.translate（）的转换表。
如果只有一个参数，则它必须是将Unicode序号（整数）或字符（长度为1的字符串）映射到Unicode序号，任意长度的字符串或无的字典。然后，字符键将转换为普通字符。
如果有两个参数,它们必须是字符串的长度,在生成的词典,每个字符x将在同一位置映射到角色y。如果有第三个参数,它必须是一个字符串的字符将映射到没有一个结果。

## str.partition(sep)
在第一次出现sep时分割字符串，并返回一个三元组，其中包含分隔符之前的部分，分隔符本身以及分隔符之后的部分。如果找不到分隔符，则返回一个包含字符串本身的3元组，然后是两个空字符串。

## str.replace(old, new[, count])
返回该字符串的副本，其中所有出现的子字符串old都替换为new。如果给出了可选的参数count，则仅替换第一个出现的计数

## str.rfind(sub[, start[, end]])
返回找到子字符串sub的字符串中的最高索引，以使sub包含在s [start：end]中。可选参数start和end解释为切片表示法。失败时返回-1。

## str.rindex(sub[, start[, end]])
与rfind（）类似，但是在未找到子字符串sub时引发ValueError。

## str.rjust(width[, fillchar])
返回width长度为字符串的右对齐字符串。使用指定的fillchar填充（默认为ASCII空间）。如果width小于或等于len，则返回原始字符串。

## str.rpartition(sep)
最后一次出现sep时分割字符串，并返回一个三元组，其中包含分隔符之前的部分，分隔符本身以及分隔符之后的部分。如果找不到分隔符，则返回一个包含两个空字符串的三元组，然后是字符串本身。

## str.rsplit(sep=None, maxsplit=-1)
使用sep作为分隔符字符串，返回字符串中单词的列表。如果给出了maxsplit，则最多完成maxsplit拆分，最右边的拆分完成。如果未指定sep或无，则任何空格字符串都是分隔符。除了从右侧拆分外，rsplit（）的行为类似于split（），下面将对其进行详细说明

## str.rstrip([chars])
返回删除了结尾字符的字符串副本。 chars参数是一个字符串，指定要删除的字符集。如果省略或为None，则chars参数默认为删除空格。 chars参数不是后缀；而是删除其值的所有组合：
```python
>>> '   spacious   '.rstrip()
'   spacious'
>>> 'mississippi'.rstrip('ipz')
'mississ'
```

## str.split(sep=None, maxsplit=-1)
使用sep作为分隔符字符串，返回字符串中单词的列表。如果指定了maxsplit，则最多完成maxsplit个分割（因此，列表最多包含maxsplit + 1个元素）。如果未指定maxsplit或-1，则分割数没有限制（进行所有可能的分割）。 如果给定sep，则不将连续的定界符分组在一起，而是将其视为定界空字符串（例如'1，，2'.split（'，'）返回['1'，''，'2']）。 sep参数可以包含多个字符（例如，'1 <> 2 <> 3'.split（'<>'）返回['1'，'2'，'3']）。使用指定的分隔符分割空字符串将返回['']。

```python
>>> '1,2,3'.split(',')
['1', '2', '3']
>>> '1,2,3'.split(',', maxsplit=1)
['1', '2,3']
>>> '1,2,,3,'.split(',')
['1', '2', '', '3', '']
```

如果未指定sep或为None，则将应用不同的拆分算法：连续的空白行将被视为单个分隔符，并且如果字符串的开头或结尾处有空格，则结果在开头或结尾将不包含空字符串。因此，使用None分隔符拆分空字符串或仅包含空格的字符串将返回[]。

```python
>>> '1 2 3'.split()
['1', '2', '3']
>>> '1 2 3'.split(maxsplit=1)
['1', '2 3']
>>> '   1   2   3   '.split()
['1', '2', '3']
```

## str.splitlines([keepends])
返回字符串中的行列表，在行边界处断开。结果列表中不包括换行符，除非给出了keepends并为真。
这种方法在下面的行边界上分割。特别是,边界是一种通用的换行线。

| 符号 | 描述 |
| -- | -- |
| \n | 换行 |
| \r | 回车 |
| \r\n | 回车换行 |
| \v or \x0b | 行制表 |
| \f or \x0c | 换页 |
| \x1c | 文件分隔符 |
| \x1d | 组分隔符|
|\x1e|记录分隔符|
|\x85|下一行(C1控制代码)|
|\u2028|行分隔符|
|\u2029| 段落分隔符|
```python
>>> 'ab c\n\nde fg\rkl\r\n'.splitlines()
['ab c', '', 'de fg', 'kl']
>>> 'ab c\n\nde fg\rkl\r\n'.splitlines(keepends=True)
['ab c\n', '\n', 'de fg\r', 'kl\r\n']
```

与split（）不同，当给定分隔符字符串sep时，此方法返回空字符串的空列表，并且终端换行不会导致多余的行：
```python
>>> "".splitlines()
[]
>>> "One line\n".splitlines()
['One line']
```
为了进行比较，split（'\ n'）给出：
```python
>>> ''.split('\n')
['']
>>> 'Two lines\n'.split('\n')
['Two lines', '']
```

## str.startswith(prefix[, start[, end]])
如果字符串以前缀开头，则返回True，否则返回False。 prefix也可以是要查找的前缀的元组。使用可选的开始，测试字符串从该位置开始。在可选端，停止在该位置比较字符串。

## str.strip([chars])
返回删除前导和尾随字符的字符串的副本。 chars参数是一个字符串，指定要删除的字符集。如果省略或为None，则chars参数默认为删除空格。 chars参数不是前缀或后缀；而是删除其值的所有组合：
```python
>>> '   spacious   '.strip()
'spacious'
>>> 'www.example.com'.strip('cmowz.')
'example'
```

从字符串中去除最外面的前和后字符char参数值。从前端删除字符，直到到达不包含在char字符集中的字符串字符。在尾端发生类似的动作。例如：
```python
>>> comment_string = '#....... Section 3.2.1 Issue #32 .......'
>>> comment_string.strip('.#! ')
'Section 3.2.1 Issue #32'
```
## str.swapcase()
返回将大写字符转换为小写字符的字符串的副本，反之亦然。注意，s.swapcase().swapcase() == s不一定是真的。

## str.title()
返回字符串的带标题的版本，其中单词以大写字母开头，其余字符为小写字母
```python
>>> 'Hello world'.title()
'Hello World'
```

该算法使用单词的简单语言独立定义作为连续字母的组。该定义在许多情况下都有效，但是它意味着缩略语和所有格中的撇号形成单词边界，这可能不是期望的结果：

```python
>>> "they're bill's friends from the UK".title()
"They'Re Bill'S Friends From The Uk"
```
撇号的变通办法可以使用正则表达式构造：
```python
>>> import re
>>> def titlecase(s):
...     return re.sub(r"[A-Za-z]+('[A-Za-z]+)?",
...                   lambda mo: mo.group(0)[0].upper() +
...                              mo.group(0)[1:].lower(),
...                   s)
...
>>> titlecase("they're bill's friends.")
"They're Bill's Friends."
```

## str.translate(table)
返回字符串的副本，其中每个字符都已通过给定的转换表进行映射。该表必须是通过__getitem __（）实现索引的对象，通常是映射或序列。当用Unicode序数（整数）索引时，表对象可以执行以下任一操作：返回Unicode序数或字符串，以将字符映射到一个或多个其他字符；返回None，从返回字符串中删除字符；或引发LookupError异常，以将字符映射到自身。
您可以使用str.maketrans（）从不同格式的字符到字符映射创建翻译映射。
另请参阅编解码器模块，以获取更灵活的自定义字符映射方法

## str.upper()
返回字符串的副本，其中所有大小写的字符4都转换为大写。请注意，如果s包含无大小写的字符，或者生成的字符的Unicode类别不是“ Lu”（大写字母），而是s.upper（）。isupper（）可能为False。 “ Lt”（字母，大写）。
## str.zfill(width)
返回一个字符串的副本，该字符串的左半部分用ASCII'0'数字填充，以形成长度为width的字符串。前导符号前缀（'+'/'-'）通过在符号字符之后而不是之前插入填充来处理。如果width小于或等于len，则返回原始字符串。
```python
>>> "42".zfill(5)
'00042'
>>> "-42".zfill(5)
'-0042'
```

# printf样式的字符串格式
> 此处描述的格式化操作表现出各种古怪，导致许多常见错误（例如未能正确显示元组和字典）。使用更新的格式化字符串文字，str.format（）接口或模板字符串可能有助于避免这些错误。这些选择中的每一个都提供了自己的权衡，并带来了简单性，灵活性和/或可扩展性的好处。

字符串对象具有一个唯一的内置操作：％运算符（模）。这也称为字符串格式化或插值运算符。给定格式％值（其中format是字符串），格式中的％转换规范将替换为零个或多个值元素。效果类似于在C语言中使用sprintf（）。
如果format需要单个参数，则值可以是单个非元组对象。 否则，值必须是具有由格式字符串指定的项目数的元组，或者是单个映射对象（例如，字典）。

转换说明符包含两个或多个字符，并具有以下组件，这些组件必须按此顺序出现:
1. '％'字符，标志着说明符的开始
2. 映射键（可选），由带括号的字符序列组成（例如，（某名称））
3. 转换标志（可选），会影响某些转换类型的结果。
4. 最小字段宽度（可选）。如果指定为“ *”（星号），则会从元组的下一个元素读取值中的实际宽度，并且要转换的对象位于最小字段宽度和可选精度之后。
5. 精度（可选），以“。”表示。 （点）后跟精度。如果指定为“ *”（星号），则会从元组的下一个元素读取值中的实际精度，并且要转换的值位于精度之后。
6. 长度修改器（可选）
7. 转换类型

如果正确的参数是字典（或其他映射类型），则字符串中的格式必须在该字典中包含带括号的映射键，该键直接插入在'％'字符之后。映射键从映射中选择要格式化的值。例如：
```python
>>> print('%(language)s has %(number)03d quote types.' %
...       {'language': "Python", "number": 2})
Python has 002 quote types.
```

在这种情况下，*指示符不能以某种格式出现（因为它们需要一个顺序的参数列表）
转换标志字符为：
|标志|含义|
|-|-|
|#|值转换将使用“替代形式”（在下面定义）。|
|0|转换将零填充数字值|
|-|转换后的值将进行左调整（如果都给出，则覆盖“ 0”转换）。|
|' '|（空格）在带符号的转换产生的正数（或空字符串）之前应留一个空格。|
|+|在转换之前将使用符号字符（“ +”或“-”）（覆盖“空格”标志）。|

可以使用长度修饰符（h，l或L），但会被忽略，因为Python不需要使用它，例如％ld与％d相同。
![转换类型](../转换类型.jpg)




