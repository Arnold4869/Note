[TOC]

# 1. 数据类型和变量
## 1.1 整数

可以处理任意大小的整数，包括负整数
- 十六进制：`Ox`前缀，例如`0xff00`

## 1.2 浮点数

和`java`类似，不能精确，计算时会有四舍五入的误差

## 1.3 字符串

**字符串是以单引号`'`或双引号`"`括起来的任意文本**
对于内部含有`'`和`"`的字符串，可以用转义字符`\`来**标识**
如果有很多需要转义，可以在用`r''`来表示`''`内部的字符串里的字符正常显示

如果字符串有很多**换行**，为了简化，可以用`'''...'''`的格式表示多行内容(**在程序中是靠换行自动产生的**)

```python
>>> print('''line1
...line2
...line3''')
```
## 1.4 布尔值

默认一个布尔值只能为`True`和`False`
布尔值可以通过`and`、`or`、`not`运算

## 1.5 空值

空值是Python里一个特殊的值，用`None`表示，`None`不能理解为`0`，类似于java里的`null`

## 1.6 变量

变量不仅可以是数字，还可以是任意数据类型

## 1.7 常量

Python中，通常用**大写的bianliangming**表示常量

## 1.8 除法
有两种**除法**
- 一种就是`/`，和java类似，不过在Python中直接取浮点数
- 另一种是`//`,称为**地板除**

其中，后者会将结果直接转换为**整数**，类似于java中对结果进行**强转**

# 2 使用list和tuple
主要区别
- list可变，tuple不可变
- list用`[]`,tuple用`()`

## 2.1 list
主要方法：
- `pop()`:`pop()`默认删除末尾的元素
- `append()`
- `insert()`

## 2.2 tuple
tuple不可变，代码更安全，能用tuple就尽量用tuple。

**tuple类似于数组**

# 3 条件判断

# 4 循环

# 5 使用dict和set

**dict**用`{}`定义
**set**用`set([])`定义

## 5.1 dict
**dict**的`key`必须为**不可变对象**(字符串、整数)

类似于`map`
dict可以在内部计算出`键`对应存放位置的页面，所以它的读取速度很快，但相应的占用内存多
list因为是链表，所以需要从头查到尾

### 5.1.1 判断key是否存在于map

```python
>>>'Thomas' in d
False
```
也可以通过dict提供的`get`方法。如果key不存在，可以返回`None`,或者指定的`value`

```python
>>>d.get('Thomas')
>>>d.get('Thomas',-1)
-1
```
**返回`None`时，交互式命令行不显示结果
**

### 5.1.2 删除key
使用`pop()`方法，并且对应的`value`也会从dict中删除

## 5.2 set
也是一组key的集合，但不存储value值，且key不重复

创建set,需要提供一个list作为集合

```python
>>>s = set([1,2,3])
>>>s
{1,2,3}
```
set的特点：

- 显示顺序是随机的
- 重复元素自动被过滤

常用方法：

- `add(key)`
-- `remove(key)`

set可以看成数学意义上的无序和无重复元素的集合，因此，两个set可以做数学意义上的交集、并集等操作：

```python
>>> s1 = set([1, 2, 3])
>>> s2 = set([2, 3, 4])
>>> s1 & s2
{2, 3}
>>> s1 | s2
{1, 2, 3, 4}
```

# 6. 函数
## 6.1 定义函数
## 6.2 内置函数
## 6.3 函数的参数
### 默认函数

```python
def power(x,n = 2):
    s = 1
    while m > 0:
        n = n - 1
        s = s * n
    return s
```

```python
def enroll(name, gender, age=6, city='Beijing'):
    print('name:', name)
    print('gender:', gender)
    print('age:', age)
    print('city:', city)
```
### 可变函数

```python
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
```
定义可变参数仅需要在可变的参数前加`*`，并且可以直接传入一个`list`或者`tuple`(实际你传入0个或任意个参数，这些可变参数在函数调用时自动组装为一个tuple)

### 关键字参数
关键字参数允许你传入**0**个或**任意**个含参数名的参数

定义函数：

```python
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)
```

使用：

```python
>>> person('Michael', 30)
name: Michael age: 30 other: {}

>>> person('Bob', 35, city='Beijing')
name: Bob age: 35 other: {'city': 'Beijing'}
>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
```

如果要限制关键字参数的名字，就可以用命名关键字参数，例如，只接收city和job作为关键字参数。这种方式定义的函数如下：

```python
def person(name, age, *, city, job):
    print(name, age, city, job)
```
### 命名关键字函数
和关键字参数**kw不同，命名关键字参数需要一个特殊分隔符`*`，`*`后面的参数被视为命名关键字参数。
使用：

```python
>>> person('Jack', 24, city='Beijing', job='Engineer')
```

如果函数定义中已经有了一个**可变参数**，后面跟着的命名关键字参数就**不再**需要一个特殊分隔符`*`了

### 参数组合

在Python中定义函数，可以用**必选参数**、**默认参数**、**可变参数**、**关键字参数**和**命名关键字参数**，这5种参数都可以组合使用。但是请注意，参数定义的**顺序**必须是：*必选参数*、*默认参数*、*可变参数*、*命名关键字参数*和*关键字参数*。

比如定义一个函数，包含上述若干种参数：

```python
def f1(a, b, c=0, *args, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

def f2(a, b, c=0, *, d, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)
```

**要注意定义可变参数和关键字参数的语法**：

`*args`是可变参数，`args`接收的是一个tuple；

`**kw`是关键字参数，`kw`接收的是一个dict。
