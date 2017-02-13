要从整数相除中得到一个整数, 丢弃任何小数部分, 可以使用另一个操作符, //:

```
>>> # 整数相除返回地板数:
... 7//3
2
>>> 7//-3
-3
```

---

在交互模式下, 最后一个表达式的值被分配给变量 _.

---

继续行可以被使用, 在一行最后加上一个反斜杠以表明下一行是这行的逻辑延续:

```
hello = "这是一个相当长的字符串包含\n\
几行文本, 就像你在 C 里做的一样.\n\
    注意开通的空白是\
 有意义的."

print(hello)

=>

这是一个相当长的字符串包含
几行文本, 就像你在 C 里做的一样.
    注意开通的空白是 有意义的.
```

另一种方法, 字符串可以使用一对匹配的三引号对包围: """ 或 '''. 当使用三引号时, 回车不需要被舍弃, 他们会包含在字符串里. 于是下面的例子使用了一个反斜杠来避免最初不想要的空行.

```
print("""\
用途: thingy [OPTIONS]
     -h                        显示用途信息
     -H hostname               连接到的主机名
""")

=>

用途: thingy [OPTIONS]
     -h                        显示用途信息
     -H hostname               连接到的主机名
```

如果我们把字符串变为一个 “未处理” 字符串, \n 序列不会转义成回车, 但是行末的反斜杠, 以及源中的回车符, 都会当成数据包含在字符串里. 因此, 这个例子:

```
hello = r"这是一个相当长的字符串包含\n\
几行文本, 就像你在 C 里做的一样."

print(hello)

=>

这是一个相当长的字符串包含\n\
几行文本, 就像你在 C 里做的一样.
```

---

字符串可以使用 + 操作符来连接 (粘在一起), 使用 * 操作符重复:

```
>>> word = 'Help' + 'A'
>>> word
'HelpA'
>>> '<' + word*5 + '>'
'<HelpAHelpAHelpAHelpAHelpA>'
```

两个靠着一起的字符串会自动的连接; 上面例子的第一行也可以写成 word = 'Help' 'A'; 这只能用于两个字符串常量, 而不能用于任意字符串表达式:

```
>>> 'str' 'ing'                   #  <-  可以
'string'
>>> 'str'.strip() + 'ing'   #  <-  可以
'string'
>>> 'str'.strip() 'ing'     #  <-  不正确
  File "<stdin>", line 1, in ?
    'str'.strip() 'ing'
                      ^
SyntaxError: invalid syntax
```

---

字符串可以使用下标 (索引); 就像 C 一样, 字符串的第一个字符的下标 (索引) 为 0. 没有独立的字符串类型; 一个字符串就是一个大小为一的字符串. 就像 Icon 程序语言一样, 子字符串可以通过*切片符号*指定: 冒号分隔的两个索引.

```
>>> word[4]
'A'
>>> word[0:2]
'He'
>>> word[2:4]
'lp'
```

切片索引有一些有用的默认值; 省略的第一个索引默认为零, 省略的第二个索引默认为字符串的大小.

```
>>> word[:2]    # 头两个字符
'He'
>>> word[2:]    # 除了头两个字符
'lpA'
```

---

与 C 字符串不一样, Python 字符串不可以改变. 给字符串的索引位置赋值会产生一个错误:

```
>>> word[0] = 'x'
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
TypeError: 'str' object does not support item assignment
>>> word[:1] = 'Splat'
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
TypeError: 'str' object does not support slice assignment
```

然而, 使用内容组合创建新字符串是简单和有效的:

```
>>> 'x' + word[1:]
'xelpA'
>>> 'Splat' + word[4]
'SplatA'
```

这有一个有用的切片操作的恒等式: s[:i] + s[i:] 等于 s.

```
>>> word[:2] + word[2:]
'HelpA'
>>> word[:3] + word[3:]
'HelpA'
```

退化的切片索引被处理地很优雅: 太大的索引会被字符串大小所代替, 上界比下界小就返回空字符串.

```
>>> word[1:100]
'elpA'
>>> word[10:]
''
>>> word[2:1]
''
```

索引可以是负数, 那样就会从右边开始算起. 例如:

```
>>> word[-1]     # 最后一个字符
'A'
>>> word[-2]     # 倒数第二个字符
'p'
>>> word[-2:]    # 最后两个字符
'pA'
>>> word[:-2]    # 除最后两个字符的其他字符
'Hel'
```

但是要注意, -0 与 0 是完全一样的, 因此它不会从右边开始数!

```
>>> word[-0]     # (因为 -0 等于 0)
'H'
```

越界的负切片索引会被截断, 当不要对单元素 (非切片) 索引使用越界索引.

```
>>> word[-100:]
'HelpA'
>>> word[-10]    # 错误
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
IndexError: string index out of range
```

记忆切片工作方式的一个方法是把索引看作是字符*之间*的点, 第一字符的左边记作 0. 包含 n 个字符的字符串最后一个字符的右边的索引就是 n, 例如:

```
 +---+---+---+---+---+
 | H | e | l | p | A |
 +---+---+---+---+---+
 0   1   2   3   4   5
-5  -4  -3  -2  -1
```

第一行给出了索引 0...5 在字符串里的位置; 第二行给出了相应的负索引. i 到 j 的切片由标号为 i 和 j 的边缘中间的字符所构成.

对于没有越界非负索引, 切片的长度就是两个索引之差. 例如, word[1:3] 的长度是 2.

内建函数 len() 返回字符串的长度:

```
>>> s = 'supercalifragilisticexpialidocious'
>>> len(s)
34
```

# list

Python 有一些*复合*数据类型, 用来把其它值分组. 最全能的就是 list, 它可以写为在方括号中的通过逗号分隔的一列值 (项). 列表的项并不需要是同一类型.

```
>>> a = ['spam', 'eggs', 100, 1234]
>>> a
['spam', 'eggs', 100, 1234]
```

就像字符串索引, 列表的索引从 0 开始, 列表也可以切片, 连接等等:

```
>>> a[0]
'spam'
>>> a[3]
1234
>>> a[-2]
100
>>> a[1:-1]
['eggs', 100]
>>> a[:2] + ['bacon', 2*2]
['spam', 'eggs', 'bacon', 4]
>>> 3*a[:3] + ['Boo!']
['spam', 'eggs', 100, 'spam', 'eggs', 100, 'spam', 'eggs', 100, 'Boo!']
```

所有的切片操作返回一个包含请求元素的新列表. 这意味着, 下面的的切片返回列表 a 的一个浅复制:

```
>>> a[:]
['spam', 'eggs', 100, 1234]
```

不像*不可变*的字符串, 改变列表中单个元素是可能的.

```
>>> a
['spam', 'eggs', 100, 1234]
>>> a[2] = a[2] + 23
>>> a
['spam', 'eggs', 123, 1234]
```

为切片赋值同样可能, 这甚至能改变字符串的大小, 或者完全的清除它:

```
>>> # 替代一些项:
... a[0:2] = [1, 12]
>>> a
[1, 12, 123, 1234]
>>> # 移除一些:
... a[0:2] = []
>>> a
[123, 1234]
>>> # 插入一些:
... a[1:1] = ['bletch', 'xyzzy']
>>> a
[123, 'bletch', 'xyzzy', 1234]
>>> # 在开始处插入自身 (的一个拷贝)
>>> a[:0] = a
>>> a
[123, 'bletch', 'xyzzy', 1234, 123, 'bletch', 'xyzzy', 1234]
>>> # 清除列表: 用空列表替代所有的项
>>> a[:] = []
>>> a
[]
```

内建函数 len() 同样对列表有效:

```
>>> a = ['a', 'b', 'c', 'd']
>>> len(a)
4
```

嵌套列表 (创建包含其它列表的列表) 是可能的, 例如:

```
>>> q = [2, 3]
>>> p = [1, q, 4]
>>> len(p)
3
>>> p[1]
[2, 3]
>>> p[1][0]
2
```

```
你可以在列表末尾加入一些东西:

>>> p[1].append('xtra')
>>> p
[1, [2, 3, 'xtra'], 4]
>>> q
[2, 3, 'xtra']
注意在最后的例子里, p[1] 和 q 确实指向同一个对象! 我们在以后会回到*对象语义*.
```

---

当然, 我们可以使用 Python 做比 2 + 2 更复杂的任务. 例如, 我们可以如下的写出 Fibonacci 序列的最初子序列:

```
>>> # Fibonacci 序列:
... # 两个元素的值定义下一个
... a, b = 0, 1
>>> while b < 10:
...     print(b)
...     a, b = b, a+b
...
1
1
2
3
5
8
```

这个例子介绍了几个新特性.

* 第一行包括一次*多重赋值*: 变量 a 和 b 同时地得到新值 0 和 1. 在最后一行又使用了一次, 演示了右边的表达式在任何赋值之前就已经被计算了. 右边表达式从左至右地计算.
* 当条件 (在这里: b < 10) 保持为真时, while 循环会一直执行. 在 Python 中, 就像 C 里一样, 任何非零整数都为真; 零为假. 条件也可以是字符串或列表, 实际上可以是任意序列; 长度不为零时就为真, 空序列为假. 本例中使用的测试是一个简单的比较. 标准比较符与 C 中写得一样: < (小于), > (大于), == (等于), <= (小于或等于), >= (大于或等于) 和 != (不等于).
* 循环*体*是*缩进*的: 缩进是 Python 分组语句的方法. Python 不 (到目前!) 提供智能输入行编辑功能, 因此, 你需要为每个缩进键入制表符或空格. 在练习中, 你会使用一个文本编辑器来为 Python 准备更复杂的输入; 大多文本编辑器带有自动缩进功能. 当一个复合语句交互地输入时, 必须跟上一个空行以表明语句结束 (因为语法分析器猜不到何时你键入了最后一行). 注意, 在同一基本块里的每一行必须以同一个数量缩进.
* print() 函数写出给它的表达是的值. 它与就写出你想要写的表达式有所不同 (就像我们在之前计算器例子中一样), 它可以处理多个表达式, 浮点数, 和字符串. 打印字符串时没有引号, 在不同项之间插入了一个空格, 因此, 你可以把东西格式得漂亮, 就像这:

```
>>> i = 256*256
>>> print('The value of i is', i)
The value of i is 65536
```

关键词 end 可以用来避免输出后的回车, 或者以一个不同的字符串结束输出:

```
>>> a, b = 0, 1
>>> while b < 1000:
...     print(b, end=',')
...     a, b = b, a+b
...
1,1,2,3,5,8,13,21,34,55,89,144,233,377,610,987,
```