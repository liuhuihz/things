.. -*- coding: utf-8 -*-

==============================
Python 语言学习笔记
==============================

:Authors: 刘晖 <liuhui.hz@gmail.com>
:Version: V1.0

.. contents:: 目录

概述
==============================
| `Python <http://www.python.org>`_ （英国发音：/ˈpaɪθən/ 美国发音：/ˈpaɪθɑːn/）的创始人为吉多·范罗苏姆（Guido van Rossum），
  这个新语言的名称源自于他钟爱的“蒙提·派森的飞行马戏团”。
| 动态类型语言，强类型语言，解释型语言（Interpreted language）。
| 强制使用缩进来划分语句块，缩进可以是 tab 字符或者空格字符，同一语句块使用相同缩进。

语法
==============================

变量命名规则
------------------------------
由英文字母、数字与下划线组成，开头不能为数字，区分大小写。

关键字（保留字）
------------------------------

+----------+--------+---------+-------+-------+
| and      | as     | assert  | break | class |
+----------+--------+---------+-------+-------+
| continue | def    | del     | elif  | else  |
+----------+--------+---------+-------+-------+
| except   | exec   | finally | for   | form  |
+----------+--------+---------+-------+-------+
| global   | if     | import  | in    | is    |
+----------+--------+---------+-------+-------+
| lambda   | not    | or      | pass  | print |
+----------+--------+---------+-------+-------+
| raise    | return | try     | while | with  |
+----------+--------+---------+-------+-------+
| yield    |        |         |       |       |
+----------+--------+---------+-------+-------+

类型
------------------------------

基础类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
数字
整型
浮点数
复数

布尔值
True
Flase

字符串
字符串的表示方法为单引号或者双引号包括。
如果字符串中包含单引号而且没有双引号，使用双引号，否则使用单引号。

字符串是不可修改的（immutable）
Python 没有单独的字符类型，字符就是长度为 1 的字符串。

数据结构
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
列表（list）
语法：[ "abc", 123, [ 'a', '1' ], 3.7 ]
与 Haskell 中的列表不同， Python 中的列表允许不同类型的元素，是否因为 Python 是动态类型语言，而 Haskell 是静态类型语言导致如此设计？

列表推导（List comprehensions）
列表推导由

.. code-block:: python

   squares = [ x ** 2 for x in range(10) ]

等价于

.. code-block:: python

   squares = []
   for x in range(10):
     squares.append(x ** 2)

再一个

.. code-block:: python

   combs = [(x, y) for x in [1, 2, 3] for y in [3, 1, 4] if x != y]

等价于

.. code-block:: python

   combs = []
   for x in [1, 2, 3]:
     for y in [3, 1, 4]:
       if x != y:
         combs.append((x, y))

元组（tuple）
语法： ( "abc", 123 )

序列（sequence）

字典（dictionary）


变量定义
------------------------------

运算符
------------------------------
1. 算术运算符

   +, -, *, **, /, //, %

2. 比较运算符

   <, >, ==, !=

3. 逻辑运算符

   not, and, or

4. 移位操作符

   <<, >>

流程控制语句
------------------------------
1. if 语句

  BNF:
    if_stmt ::= "if" expression ":" suite
                ( "elif" expression ":" suite )*
                [ "else" ":" suite ]

  .. code:: python

    if a > 0 and b > 0:
      print a - b
    elif a < 0 and b < 0:
      print b - a
    else:
      print a + b

2. for 语句

  BNF:
    for_stmt ::= "for" target_list "in" expression_list ":" suite
                 [ "else" ":" suite ]

  迭代序列中的元素（列表或者字符串）

  .. code-block:: python
                  :linenos:

    a = [ 'cat', 'window', 'defenestrate' ]
    for x in a:
      print x, len(x)

3. while 语句

  BNF:
    while_stmt ::= "while" expression ":" suite
                   [ "else" ":" suite ]

  .. code-block:: python
                  :linenos:

    a, b = 0, 1
    while b < 10:
      print b,
      a, b = b, a + b

4. break, continue 和 else 在循环结构中的使用

  .. code-block:: python

    for n in range(2, 10):
      for x in range(2, n):
        if n % x == 0:
          print n, 'equals', x, '*', n / x
          break
      else:
        # loop fell through without finding a factor
        print n, 'is a prime number'


函数
------------------------------

定义
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

  def fib(n): # write Fibonacci series up to n
    """Print a Fibonacci series up to n."""
    a, b = 0, 1
    while a < n:
      print a,
      a, b = b, a + b

外部变量可以引用，但如果需要修改，则需要使用 global 语句来说明。

传值
缺省参数
关键值参数
*name 参数及 **name 参数

lambda 函数

BNF:
  lambda_form ::= "lambda" [parameter_list]: expression

类
------------------------------

.. code-block:: python

  class Child(Parent):

    def Fun1():
      pass

    def Fun2():
      pass

继承

异常
------------------------------

模块
------------------------------

字符串
------------------------------
python 中的字符串
python 中的 unicode 字符串使用 u 前缀。它的字节序列表示不是任何一种特定的 UTF 编码。
比如 ASCII 字符并不用 UCS2 的两字节或者 UCS4 的四字节（填充 00 字节），而是 UTF-8 的单字节表示；而中文却是用 UCS （ UTF-16 编码）的方式。
这正好与它是字符的 list 而不是单纯的字节序列的特性契合。

.. code-block:: python

   str_unicode          = u'1中文1'
   str_utf8             = str_unicode.encode('utf-8')
   str_utf16            = str_unicode.encode('utf-16')
   str_utf32            = str_unicode.encode('utf-32')
   str_unicode_internal = str_unicode.encode('unicode_internal')

   # u'1\u4e2d\u65871'
   print repr(str_unicode)

   # 31 4e2d 6587 31
   print " ".join("{:02x}".format(ord(c)) for c in str_unicode)

   # '1\xe4\xb8\xad\xe6\x96\x871'
   print repr(str_utf8)

   # 31 e4 b8 ad e6 96 87 31
   print " ".join("{:02x}".format(ord(c)) for c in str_utf8)

   # ff fe 31 00 2d 4e 87 65 31 00
   print " ".join("{:02x}".format(ord(c)) for c in str_utf16)

   # ff fe 00 00 31 00 00 00 2d 4e 00 00 87 65 00 00 31 00 00 00
   print " ".join("{:02x}".format(ord(c)) for c in str_utf32)

   # 31 00 2d 4e 87 65 31 00
   print " ".join("{:02x}".format(ord(c)) for c in str_unicode_internal)


print 语句和环境 locale 设置有关。 print 输出时会根据当前的 locale 设置对 unicode 字符串编码然后输出。如果编码失败，则 print 语句也执行失败。
比如

.. code-block:: python

   $ locale
   LANG=
   LC_COLLATE="C"
   LC_CTYPE="C"
   LC_MESSAGES="C"
   LC_MONETARY="C"
   LC_NUMERIC="C"
   LC_TIME="C"
   LC_ALL=

   $ python
   Python 2.7.8 (default, Jul 14 2014, 12:48:12)
   [GCC 4.2.1 Compatible Apple LLVM 5.1 (clang-503.0.40)] on darwin
   Type "help", "copyright", "credits" or "license" for more information.
   >>> a = u'1中文1'
   a = u'1中文1'
   >>> print a
   print a
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   UnicodeEncodeError: 'ascii' codec can't encode characters in position 1-6: ordinal not in range(128)
   >>> print " ".join("{:02x}".format(ord(c)) for c in a)
   print " ".join("{:02x}".format(ord(c)) for c in a)
   31 e4 b8 ad e6 96 87 31
   >>> print repr(a)
   print repr(a)
   u'1\xe4\xb8\xad\xe6\x96\x871'
   >>>
