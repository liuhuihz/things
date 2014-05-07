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

元组（tuple）
语法： ( "abc", 123 )

序列（sequence）

字典（dictionary）


变量定义
------------------------------

运算符
------------------------------
1. 算术运算符

   +, -, *, /, %

2. 比较运算符

   <, >, ==, !=

3. 逻辑运算符

   not, and, or

流程控制语句
------------------------------
1. if 语句

  .. code:: python

    if a > 0 and b > 0:
      print a - b
    elif a < 0 and b < 0:
      print b - a
    else:
      print a + b

2. for 语句

  迭代序列中的元素（列表或者字符串）

  .. code-block:: python
                  :linenos:

    a = [ 'cat', 'window', 'defenestrate' ]
    for x in a:
      print x, len(x)

3. while 语句

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
