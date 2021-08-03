# Python入门Day01

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

计算机内部使用补码表示

原码：就是其二进制表示，有一位符号位（最高位1为负，0为正）

反码：原码符号位不变，其余位取反

补码：正数的补码就是原码，负数的补码是反码+1

在位运算中符号位也参与运算

### 按位运算

