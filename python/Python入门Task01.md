- [Python入门Task01](#python入门task01)
  - [变量、运算符与数据类型](#变量运算符与数据类型)
    - [注释](#注释)
    - [运算符](#运算符)
      - [算术运算符](#算术运算符)
      - [比较运算符](#比较运算符)
      - [逻辑运算符](#逻辑运算符)
      - [位运算符](#位运算符)
      - [三元运算符](#三元运算符)
      - [其他运算符](#其他运算符)
      - [运算符的优先级](#运算符的优先级)
    - [变量和赋值](#变量和赋值)
    - [数据类型与转换](#数据类型与转换)
      - [整型](#整型)
      - [浮点型](#浮点型)
      - [布尔型](#布尔型)
      - [获取类型信息](#获取类型信息)
      - [类型转换](#类型转换)
    - [print()函数](#print函数)
  - [<span id = "位运算">位运算</span>](#位运算)
    - [原码、反码、补码](#原码反码补码)
    - [按位运算](#按位运算)
    - [利用位运算实现快速计算](#利用位运算实现快速计算)
    - [利用位运算实现整数集合](#利用位运算实现整数集合)
  - [条件语句](#条件语句)
    - [if语句](#if语句)
    - [if-else语句](#if-else语句)
    - [if-elif-else语句](#if-elif-else语句)
    - [assert关键字](#assert关键字)
  - [循环语句](#循环语句)
    - [while循环](#while循环)
    - [while-else循环](#while-else循环)
    - [for循环](#for循环)
    - [for-else循环](#for-else循环)
    - [range()函数](#range函数)
    - [enumerate()函数](#enumerate函数)
    - [break语句](#break语句)
    - [continue语句](#continue语句)
    - [pass语句](#pass语句)
    - [推导式](#推导式)
      - [列表推导式](#列表推导式)
      - [元组推导式](#元组推导式)
      - [字典推导式](#字典推导式)
      - [集合推导式](#集合推导式)
      - [其他](#其他)
  - [异常处理](#异常处理)
    - [Python标准异常总结](#python标准异常总结)
    - [Python标准警告总结](#python标准警告总结)
    - [try-except语句](#try-except语句)
    - [try-except-finally语句](#try-except-finally语句)
    - [try-except-else语句](#try-except-else语句)
    - [raise语句](#raise语句)

# Python入门Task01

天池龙珠计划的笔记



## 变量、运算符与数据类型

### 注释

`#`单行注释

```python
'''
多行注释方式一：单引号
'''

"""
多行注释方式二：双引号
"""
```

### 运算符

#### 算术运算符

`+`加、`-`减、`*`乘、`%`取余

```python
#/除，结果取小数
print(3/4)#0.75

#//整除，结果只取商
print(3//4)#0

#**幂
print(2**3)#8
```

#### 比较运算符

`>`大于、`>=`大于等于、`<`小于、`<=`小于等于、`==`等于、`！=`不等于

`==`和`！=`比较的是内容，不是内存地址

#### 逻辑运算符

| 操作符 | 名称 | 逻辑               |
| ------ | ---- | ------------------ |
| `and`  | 与   | 有假为假，全真为真 |
| `or`   | 或   | 有真为真，全假为假 |
| `not`  | 非   | 有真为假，有假为真 |

#### 位运算符

| 操作符 | 名称     | 作用                                       |
| ------ | -------- | ------------------------------------------ |
| `~`    | 按位取反 | 二进制数的每一位1变0，0变1                 |
| `&`    | 按位与   | 两个二进制数的每一位比较，全为1为1，有0为0 |
| `|`    | 按位或   | 两个二进制数的每一位比较，全为0为0，有1为1 |
| `^`    | 按位异或 | 两个二进制数的每一位比较，相同为0，相异为1 |
| `<<`   | 左移     | 将二进制数表示向左移动指定位数             |
| `>>`   | 右移     | 将二进制数表示向右移动指定位数             |

更详细的位运算信息看[位运算部分](#位运算)

#### 三元运算符

```python
x,y = 4,5
small = x if x<y else y
print(small)#4
```

先判断if后面的条件，如果为真则执行x，否则执行else后的y

#### 其他运算符

| 操作符   | 名称   | 作用                   |
| -------- | ------ | ---------------------- |
| `in`     | 存在   | 判断元素是否在集合中   |
| `not in` | 不存在 | 判断元素是否不在集合中 |
| `is`     | 是     | 判断二者是否相同       |
| `not is` | 不是   | 判断二者是否不同       |

`is`和`not is`对比的是两个变量的内存地址，和`==`和`！=`在指向地址可变的类型是有区别，在指向不可变的类型没有区别

#### 运算符的优先级

| 运算符                     | 描述                         |
| -------------------------- | ---------------------------- |
| **                         | 指数                         |
| ~ 、+、 -                  | 按位取反、一元加号、一元减号 |
| *、 /、 %、 //             | 乘、除、取模、取整除         |
| +、 -                      | 加法、减法                   |
| <<、 >>                    | 左移、右移                   |
| &                          | 位与                         |
| ^、\|                      | 位运算                       |
| <=、 <、 >、 >=            | 比较运算                     |
| <>、 ==、 !=               | 等于运算                     |
| =、%=、/=、//=、-=、+=、*= | 赋值运算符                   |
| is 、not is                | 身份运算                     |
| in、 not in                | 成员运算                     |
| not、and、or               | 逻辑运算                     |

从下向上，优先级提高

```python
print(-3 ** 2)  # -9
```

### 变量和赋值

Python中使用变量需要先赋值，变量名可以包括字母、数字、下划线，但是变量名不能以数字开头，且变量名大小写敏感

### 数据类型与转换

| 类型  | 名称                  |
| ----- | --------------------- |
| int   | 整型<class 'int'>     |
| float | 浮点型<class 'float'> |
| bool  | 布尔型<class 'bool'>  |

Python中万事万物皆对象，都有对应的属性和方法，详细使用可以查阅文档

#### 整型

```python
a = 1000
print(a,type(a))#1000 <class 'int'>
a = dir(int)
print(a)
"""
['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__init_subclass__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'as_integer_ratio', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
"""
```

> type()查看数据类型
>
> dir()查看属性和方法

#### 浮点型

```python
print(1., type(1.))# 1.0 <class 'float'>
a = 0.00000023
b = 2.3e-7
print(a)  # 2.3e-07
print(b)  # 2.3e-07
```

可以使用`decimal`包中的`Decimal`对象和`getcontext()`方法来实现保留浮点型的小数点后n位

```python
import decimal
from decimal import Decimal
decimal.getcontext().prec = 4#调整精度，prec默认为28
b = Decimal(1)/Decimal(3)
print(b)#0.3333
```

> 包也是对象，可以使用dir(decimal)查看其属性和方法
>
> `decimal.getcontext()`可以查看`Decimal`对象的信息

#### 布尔型

只有两个值`True`和`False`，放在数字运算中可以用数字1，0分别代表

除了直接赋值之外，可以使用`bool(x)`来创建变量，x可以是基本类型（整型、浮点型、布尔型）和容器类型（字符串、元组、列表、字典和集合）

* 基本类型：`X` 只要不是整型 `0`、浮点型 `0.0`，`bool(X)` 就是 `True`
* 容器类型：`X` 只要不是空的变量，`bool(X)` 就是 `True`，对于容器变量，里面没元素就是空

```python
print(type(''), bool(''), bool('python'))
# <class 'str'> False True

print(type(()), bool(()), bool((10,)))
# <class 'tuple'> False True

print(type([]), bool([]), bool([1, 2]))
# <class 'list'> False True

print(type({}), bool({}), bool({'a': 1, 'b': 2}))
# <class 'dict'> False True

print(type(set()), bool(set()), bool({1, 2}))
# <class 'set'> False True
```

#### 获取类型信息

* `type(object)`，获取类型信息，它不会认为子类是一种父类类型，不考虑继承关系
* `isinstance()`，它认为子类是一种父类类型，考虑继承关系

如果要判断两个类型是否相同推荐使用`isinstance()`

#### 类型转换

* 转换成整型`int(x,base=10)`
* 转换成字符串`str(object='')`
* 转换成浮点型`float(x)`

```python
print(str(10 + 10))  # 20
```

### print()函数

```python
print(*object,sep=' ',end='\n',file=sys.stdout,flush=False)
```

* 将对象以字符串表示的方式格式化输出到流文件对象file里，其中所有非关键字参数都按str()方式进行转换为字符串输出

* 关键字参数sep是实现分隔符，比如多个参数输出时想要输出中间的分隔字符

* 关键字参数end是输出结束时的字符，默认是换行符\n

* 关键字file是定义流输出的文件，可以是标准的系统输出sys.stdout，也可以重定义为别的文件

* 关键字参数flush是立即把内容输出到流文件，不作缓存

* 没有参数时，每次输出后都会换行

* 设置end改变结尾

  ```python
  shoplist = ['apple', 'mango', 'carrot', 'banana']
  print("This is printed with 'end='&''.")
  
  for item in shoplist:
      print(item, end='&')
      
  #apple&mango&carrot&banana&hello world
  
  print('hello world')
  ```

* 设置sep改变分隔

  ```python
  shoplist = ['apple', 'mango', 'carrot', 'banana']
  print("This is printed with 'sep='&''.")
  for item in shoplist:
      print(item, 'another string', sep='&')
  
  # apple&another string
  # mango&another string
  # carrot&another string
  # banana&another string
  
  print("*********","11111111")#********* 11111111
  ```



## <span id = "位运算">位运算</span>

### 原码、反码、补码

计算机内部使用==补码==表示

原码：就是其二进制表示，有一位符号位（最高位1为负，0为正）

反码：原码符号位不变，其余位取反

补码：正数的补码就是原码，负数的补码是反码+1

在位运算中符号位也参与运算

### 按位运算

* `~`按位非运算：将补码中的0和1全部取反（0变为1，1变为0），有符号整数的符号位同样取反
* `&`按位与运算：只有两个对应位都为1时才为1
* `|`按位或运算：只要两个对应位中有一个1时就为1
* `^`按位异或运算：只有两个对应位不同时才为1，它满足交换律和结合律（`A^B=B^A`、`A^A=0`、`A^0=A`、`A^B^A=A^A^B=B`）
* `<<`按位左移运算：将二进制表示向左移动指定位数所得的值
* `>>`按位右移运算：将二进制表示向右移动指定位数所得的值

### 利用位运算实现快速计算

* 利用`<<`、`>>`快速计算2的倍数问题

  左移乘以2的次方，右移除以2的次方

* 利用`^`快速交换两个整数

  ```python
  a ^= b#a=a^b
  b ^= a#b=b^ a^b=a
  a ^= b#a=a^b ^a=b
  ```

* 利用`a&(-a)`快速获取a的最后为1位置的整数

### 利用位运算实现整数集合

一个数的二进制表示可以看作一个集合，0表示不在集合中，1表示在集合中

如集合 `{1, 3, 4, 8}`，可以表示成 `01 00 01 10 10` 而对应的位运算也就可以看作是对集合进行的操作

元素与集合的操作：

```python
a | (1<<i)  -> 把 i 插入到集合中
a & ~(1<<i) -> 把 i 从集合中删除
a & (1<<i)  -> 判断 i 是否属于该集合（零不属于，非零属于）
```

集合之间的操作：

```python
a 补   -> ~a
a 交 b -> a & b
a 并 b -> a | b
a 差 b -> a & (~b)
```

注意：整数在内存中是以补码的形式存在的，输出自然也是按照补码输出。

* Python中bin一个负数（十进制表示），输出的是它的原码的二进制加一个负号
* Python中的整型是补码形式存储的
* Python中整型是不限制长度的不会超范围溢出

为了得到负数的补码，需要手动将其和十六进制数（`0xffffffff`）进行按位与操作，再交给bin()进行输出

```python
print(bin(-3 & 0xffffffff))#0b11111111111111111111111111111101
```



## 条件语句

### if语句

```python
if expression:
    expr_true_suite
```

当expression为真时执行expr_true_suite，否则跳过expr_true_suite，继续执行紧跟在该代码块后面的语句

### if-else语句

```python
if expression:
    expr_true_suite
else:
    expr_false_suite
```

`if`语句支持嵌套

* Python使用缩进而不是大括号来标记代码块边界，因此要特别注意esle的悬挂问题

> input函数将接收的任何数据类型都默认为str

### if-elif-else语句

```python
if expression1:
    expr1_true_suite
elif expression2:
    expr2_true_suite
    .
    .
elif expressionN:
    exprN_true_suite
else:
    expr_false_suite
```

elif即为else if

### assert关键字

assert这个关键词我们称之为断言，当这个关键词后面的条件为False时，程序自动崩溃并抛出AssertionError的异常

* 在进行单元测试时，可以用来在程序中置入检查点，只有条件为True才能让程序正常工作



## 循环语句

### while循环

```python
while 布尔表达式:
    代码块
```

### while-else循环

```python
while 布尔表达式:
    代码块
else:
    代码块
```

当while循环正常执行完后，执行else输出，如果while循环中执行了跳出循环的语句（break），将不执行else代码块的内容

### for循环

在Python中相当于一个通用的序列迭代器，可以遍历任何有序序列（str、list、tuple等）和可迭代对象（dict）

```python
for 迭代变量 in 可迭代对象:
    代码块
```

### for-else循环

```python
for 迭代变量 in 可迭代对象:
    代码块
else:
    代码块
```

和while-else语句一样

### range()函数

```python
range([start,] stop[, step=1])
```

* 有三个参数，用中括号括起来的两个表示这两个参数可选
* step=1表示第三个参数的默认值是1，它表示两个数之间的间隔数
* range的作用是生成一个从start参数的值开始到stop参数的值结束的数字序列，序列包含start但不包含stop

### enumerate()函数

```python
enumerate(sequence, [start=0])
```

* sequence：一个序列、迭代器或者其他支持迭代对象
* start：下标起始位置
* 返回enumerate（枚举）对象

**与for循环的结合使用：**

```python
for i, a in enumerate(A)
    do something with a  
```

enumerate(A)不仅返回A中的元素，顺便给该元素一个索引值（默认从0开始）

### break语句

可以跳出当前所在层的循环

### continue语句

终止本轮循环并开始下一轮循环

```python
for i in range(10):
    if i % 2 != 0:
        print(i)
        continue
    i += 2
    print(i)

# 2
# 1
# 4
# 3
# 6
# 5
# 8
# 7
# 10
# 9
```

> for循环内循环体中对循环数的改变不影响循环数下一轮的值

### pass语句

当你在需要有语句的地方不写任何语句，解释器就会提示出错，pass语句就是解决这一问题，它的意思是“不做任何事”

例：

```python
def a_func():
    pass
```

pass是空语句，不做任何操作，只起到占位的作用，其作用是为了保持程序结构的完整性；如果暂时不确定要在一个位置上放上什么样的代码，可以先用pass使程序可以正常运行

### 推导式

#### 列表推导式

```python
[ expr for value in collection [if condition] ]
```

对于collection中的每一个值如果满足condition，则执行expr

```python
a = [(i, j) for i in range(0, 3) for j in range(0, 3)]
print(a)

# [(0, 0), (0, 1), (0, 2), (1, 0), (1, 1), (1, 2), (2, 0), (2, 1), (2, 2)]
```

#### 元组推导式

```python
( expr for value in collection [if condition] )
```

#### 字典推导式

```python
{ key_expr: value_expr for value in collection [if condition] }
```

#### 集合推导式

```python
{ expr for value in collection [if condition] }
```

#### 其他

```python
next(iterator[, default])
```

返回迭代器的下一项，如果迭代器用尽，则返回默认值而不是抛出StopIteration异常



## 异常处理

异常是运行期间检测到的错误，计算机语言针对可能出现的错误定义了异常类型，某种错误引发对应的异常时，异常处理程序将被启动，从而恢复程序的正常运行

### Python标准异常总结

- BaseException：所有异常的 **基类**
- Exception：常规异常的 **基类**
- StandardError：所有的内建标准异常的基类
- ArithmeticError：所有数值计算异常的基类
- FloatingPointError：浮点计算异常
- OverflowError：数值运算超出最大限制
- ZeroDivisionError：除数为零
- AssertionError：断言语句（assert）失败
- AttributeError：尝试访问未知的对象属性
- EOFError：没有内建输入，到达EOF标记
- EnvironmentError：操作系统异常的基类
- IOError：输入/输出操作失败
- OSError：操作系统产生的异常（例如打开一个不存在的文件）
- WindowsError：系统调用失败
- ImportError：导入模块失败的时候
- KeyboardInterrupt：用户中断执行
- LookupError：无效数据查询的基类
- IndexError：索引超出序列的范围
- KeyError：字典中查找一个不存在的关键字
- MemoryError：内存溢出（可通过删除对象释放内存）
- NameError：尝试访问一个不存在的变量
- UnboundLocalError：访问未初始化的本地变量
- ReferenceError：弱引用试图访问已经垃圾回收了的对象
- RuntimeError：一般的运行时异常
- NotImplementedError：尚未实现的方法
- SyntaxError：语法错误导致的异常
- IndentationError：缩进错误导致的异常
- TabError：Tab和空格混用
- SystemError：一般的解释器系统异常
- TypeError：不同类型间的无效操作
- ValueError：传入无效的参数
- UnicodeError：Unicode相关的异常
- UnicodeDecodeError：Unicode解码时的异常
- UnicodeEncodeError：Unicode编码错误导致的异常
- UnicodeTranslateError：Unicode转换错误导致的异常

![imgaliyun01](https://tianchi-public.oss-cn-hangzhou.aliyuncs.com/public/files/forum/162210513255214581622105132094.png)

### Python标准警告总结

- Warning：警告的基类
- DeprecationWarning：关于被弃用的特征的警告
- FutureWarning：关于构造将来语义会有改变的警告
- UserWarning：用户代码生成的警告
- PendingDeprecationWarning：关于特性将会被废弃的警告
- RuntimeWarning：可疑的运行时行为(runtime behavior)的警告
- SyntaxWarning：可疑语法的警告
- ImportWarning：用于在导入模块过程中触发的警告
- UnicodeWarning：与Unicode相关的警告
- BytesWarning：与字节或字节码相关的警告
- ResourceWarning：与资源使用相关的警告

### try-except语句

```python
try:
    检测范围
except Exception[as reason]:
    出现异常后的处理代码
```

如果在执行try子句的过程中发生了异常，那么try子句余下的部分将被忽略。如果异常的类型和except之后的名称相符，那么对应的except子句将被执行，最后执行try-except语句之后的代码

如果一个异常没有与任何的except匹配，这个异常将被传递给上层的try中

```python
try:
    f = open('test.txt')
    print(f.read())
    f.close()
except OSError as error:
    print('打开文件出错\n原因是：' + str(error))

# 打开文件出错
# 原因是：[Errno 2] No such file or directory: 'test.txt'
```

一个try可能有多个except，最多只有一个会被执行，且在使用多个except时，必须坚持对其规范排序，要从最具针对性的异常到最通用的异常

一个except可以同时处理多个异常，将这些异常放在一个括号里成为元组

### try-except-finally语句

如果一个异常在try中被抛出，又没有任何except把它截住，那么这个异常会在finally子句执行后被抛出

### try-except-else语句

```python
try:
    检测范围
except:
    出现异常后的处理代码
else:
    如果没有异常执行这块代码
```

else语句的存在必须以except语句的存在为前提，在没有except语句的try语句中使用else语句，会引发语法错误。

### raise语句

Python使用raise语句抛出一个指定的异常

```python
try:
    raise NameError('HiThere')
except NameError:
    print('An exception flew by!')
    
# An exception flew by!
```





