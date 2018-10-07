# Python

## print 
print会依次打印每个字符串，遇到逗号“,”会输出一个空格，因此，输出的字符串是这样拼起来的：


![](http://img.mukewang.com/54055502000179c205060086.jpg)


`r'''...'''`
和
`'''...'''`

```python
print r'''Python is created by "Guido".
It is free and easy to learn.
Let's start learn Python in imooc!'''

print '''Python is created by "Guido".
It is free and easy to learn.
Let's start learn Python in imooc!'''
```
这个情况下两种输出是一样的


```python
print r'''\nPython is created by "Guido".
It is free and easy to learn.
Let's start learn Python in imooc!'''

print '''\nPython is created by "Guido".
It is free and easy to learn.
Let's start learn Python in imooc!'''
```
但是这个字符串里有转义字符的时候, 两种方法就有区别了
r的作用就是忽略\n这样的转义字符, 一视同仁的输出
不使用r特殊处理的话,\n会自动输出为换行

> 一个网上的回答:
> 不一样，'r'是防止字符转义的 如果路径中出现'\t'的话 不加r的话\t就会被转义 而加了'r'之后'\t'就能保留原有的样子。在字符串赋值的时候 前面加'r'可以防止字符串在时候的时候不被转义 原理是在转义字符前加'\'。


# 集合是指包含一组元素的数据结构，我们已经介绍的包括：
1. 有序集合：list，tuple，str和unicode；
2. 无序集合：set
3. 无序集合并且具有 key-value 对：dict


## List列表类型(数组)
List1 = [1,2,3,4]

List1.insert(index, target)
List1.append()
List1.pop([index]), 默认删除最后一个

索引可以是负值, 代表从后往前数, L[-1]代表L列表的倒数第一个元素

###  Tuple元组(一旦创建不能改变的数组)
Tuple1 = (1,2,3,4)
特殊的, 单元素tuple要多加一个逗号 Tuple2 = (1,)

### 条件判断

```python
if xx:
    xxxx
elif xx:
    xxxx
else:
    xxx
```

### 遍历数组用for in

```py
L = ['Adam', 'Lisa', 'Bart']
for name in L:
    print name
```

break / continue


### dict 表

类似对象
len()可以得到集合的长度

```py
d = {
    'Adam': 95,
    'Lisa': 85,
    'Bart': 59
}
```

从dict里取一个不存在的key对应的value, 会报错

> 要避免 KeyError 发生，有两个办法：

一是先判断一下 key 是否存在，用 in 操作符：

```py
if 'Paul' in d:
    print d['Paul']
```
`如果 'Paul' 不存在，if语句判断为False，自然不会执行 print d['Paul'] ，从而避免了错误。`

二是使用dict本身提供的一个 get 方法，在Key不存在的时候，返回None：

```py
>>> print d.get('Bart')
59
>>> print d.get('Paul')
None
```

dict使用for in遍历
.items(), 返回一个加工后的list,list的每一项都是包含key和value的tuple

### set

1. 是key的集合(元素必须是不变对象)
1. 不能重复,
2. 无序(不能通过索引访问子元素)

`s = set(['A', 'B', 'C'])`

#### 遍历

用 in 操作符 判断一个元素在不在set里
通过 for in 遍历

> for in 遍历的永远是value, 一些有序集合想得到index怎么办??
> 使用enumerate()函数, 传入list, 每一项处理为tuple,返回新的list
> `for index, name in enumerate(L):`

> for in 遍历dict的永远是key, 一些dict想得到value怎么办??
> 使用dict对象的values()函数, 得到一个包含所有value的list
>  itervalues() 方法不会转换为一个包含所有value的list，它会在迭代过程中依次从 dict 中取出 value，所以 itervalues() 方法比 values() 方法节省了生成 list 所需的内存。

> 使用dict对象的items()函数, 得到一个包含所有(key,value)的tuple的list
>  iteritems() 方法不会转换为一个包含所有value的list，它会在迭代过程中依次从 dict 中取出 给tuple，所以 iteritems() 方法比 items() 方法节省了生成 list 所需的内存。


#### 操作

添加元素: set1.add(newItem) *注意: 添加重复的元素, 不会有效果, 因为set不允许元素重复*
删除元素: set1.remove(item) *注意: 删除set中不存在的元素会报错*

*结论: 删除前要判断, 添加则不需要*



## 函数

ads(-100) ==> 100
cmp(1,2) ==> 第一个数减第二个数的差值 -1

### python原生函数类型转换
int() 转整数
str() 转字符串
sum() 求和
pow() 乘方
range(第一个元素, 最后一个元素, 间隔的倍数)包含第一个, 不包含最后一个
enumerate()函数, 传入list, 每一项处理为tuple
zip()函数可以把两个 list 变成一个 list：
> zip([10, 20, 30], ['A', 'B', 'C'])
[(10, 'A'), (20, 'B'), (30, 'C')]
### 自定义函数

1. 返回值 默认返回None
2. 格式
    ```py
    def func(x):
        return 'Hello, FUNC'
    ```

### 返回多值

`return res1, res2`
本质上 返回了一个**tuple**(语法上 返回一个tuple可以省略括号)
等价于:

`return (res1, res2)`

**而多个变量可以同时接收一个tuple，按位置赋给对应的值**

所以可以写成 `val1, val2 = func1()`



### math模块

一些高级数学运算符需要引入原生的math模块

```py
import math

math.pi
math.cos()
math.sin()
math.sqrt() # 平方根求解
```


ax^2 + b*y + c = 0

(-b +- sqrt(b^2-4*a*c))/2*a


> 栈溢出
> 使用递归函数需要注意防止栈溢出。在计算机中，函数调用是通过栈（stack）这种数据结构实现的，每当进入一个函数调用，栈就会加一层栈帧，每当函数返回，栈就会减一层栈帧。由于栈的大小不是无限的，所以，递归调用的次数过多，会导致栈溢出。可以试试计算 fact(10000)。

汉诺塔  

n个盘 从第一个 移到 第三个


n-1个盘 从第一个 移到 第二个 
1
n-1个盘 从第二个 移到第三个



        
n-2个盘 从第一个 移到 第三个
1
n-2个盘 从第三个 移到第二个
        
1
        
n-2个盘 从第二个 移到第一个  
1
n-2个盘 从第一个 移到第三个




1 0个 + 1 + 0个 1
2 1个 + 1 + 1个 3
3 2个 + 1 + 2个 7
4 3个 + 1 + 3个 15
5 4个 + 1 + 4个 31
6 5个 + 1 + 5个 63
7 6个 + 1 + 6个 127

n 

2^1*(n-1个)+2^0

2*(  2*(n-2个)+1  )+1
2^2*(n-2个)+2^1 +2^0

2^2*( 2*(n-3个)+1 )+2 +1
2^3*(n-3个)+2^2 + 2^1 + 2^0

n-(n-1) = 1

2 ^ (n -1 ) + 2^(n-2) + 2^ (n-3) + ... 2^0

-(1-2^(n-1)*2)


`2^n-1` 是n层汉诺塔完成的最少步数


实现汉诺塔 移动步骤的打印

```py
def move(n, a, b, c):
    if n == 1:
        print a,'-->',c 
    else:
        move(n-1, a, c, b)
        move(1, a, b, c)   
        move(n-1, b, a, c)

move(4, 'A', 'B', 'C')
```





`int('123', 8) ` 作用是把八进制的123转换为10进制的


### 函数参数默认值

```py
def fn1(a="defaultValue"):
    xxxx....
```

### def fn2(*args)

args本质是tuple

`*args  用来[解包list]将参数打包成tuple给函数体调用`
`**kw   打包关键字参数成dict给函数体调用`


### 想要得到小数结果

和一个小数进行运算就可以
比如:
`a*1.0`
或者初始化为0.0
`a=0.0`


### str/list/tuple/str的切片slice

语法:
`list[startIndex:endIndex:interval]`
包含起始索引，不包含结束索引

`list2[0:10]`
等价于
`list2[:10]`

从索引为 0 切到 10

只用一个 [:], 表示从头切到尾,(等价于复制)

`list2[::2]` 第三个参数表示每N个元素里取一个

***也可以传负索引,一定要注意,从-1开始的,意味着要多负一位***


### str123.upper/lower() 转换大小写


### 列表生成式

`[x * x for x in range(1, 11) for x in range(1, 11) ...  if (条件) x % 2 == 0]`



### 字符串

字符串可以通过 % 进行格式化，用指定的参数替代 %s。字符串的join()方法可以把一个 list 拼接成一个字符串。(注意 s% 一定要放在字符串双引号里)

%f 浮点数字(用小数点符号)
`'<tr><td>%s</td><td>%s</td></tr>' % (name, score)`

`'-'.join['a','b','c'] => 'a-b-c'`

1. isinstance(x, str) 可以判断变量 x 是否是字符串；

2. 字符串的 upper() 方法可以返回大写的字母。

3. strip(rm) 去首尾的rm(当rm为空时，默认删除空白符（包括'\n', '\r', '\t', ' '))

## 高阶函数(接受函数做参数的函数)

map(fn, list)
reduce(fn, list, initValue(=>计算的初始值))
filter(fn, list)
sorted(list, (fn))   -1不换位置
*sorted不会修改原list*

> python2下:
> sorted的参数表:
> 
> cmp, key
> 
> cmp表示比较函数
> key表示处理每一项使用的函数


# 匿名函数
匿名函数 lambda x: x * x 实际上就是：

def f(x):
    return x * x
关键字lambda 表示匿名函数，冒号前面的 x 表示函数参数。


三目这么写: -x if x < 0 else x







# 装饰器
def log(fn):
    def f1(x):
        print 'log'
        return fn(x)
    return f1

@log
def f1(x):
    return x*x
    
相当于:
def f1(x):
    return x*x
f1 = log(f1)




time.time()   此时时间戳浮点型数
f.__name__ 函数名字符串




# 偏函数

## 举例

**INt**

### 语法: 
int(数字, 进制:默认10进制)


### 示例
`int('12345', base=16)`
或者
`int('12345', 16)`

输出
`74565`


> `1*16^4+2*16^3+3*16^2+4*16^1+5 === 74565`


### 描述

第二个参数功能是:介绍第一个参数是几进制的, 
最后转化出来的都是10进制






## 创建偏函数的工具

### functools.partial 工具函数

`int2 = functools.partial(int, base=2)`

等价于

```py
def int2(x, base=2):
    return int(x, base)
```

总的来说就是通过提供一些默认值,把一个参数多的函数变成一个参数少的新函数







# python 模块和包




`import 模块名`来引入模块名

包-模块

模块名称冲突, 放在不同的暴力就可以

`import 包.模块名`
使用的时候也要这么用


包和普通目录的区别: 每层包里一定要有一个 `__init__.py`文件



引入模块的小技巧:
只想引入模块内部分函数时, 使用: `from math import abs, pow`


可以起别名:
`from math import abs as mathAbs`


## python的try-catch

try:
    statement
except ImportError(错误名):
    statement




## 导入未来语法
示例导入了__future__的division(除法), 此后, //才会取整数部分, /会得到浮点数


`from __future__ import division`

这样一些未来的语法就可以使用了




## 安装第三方模块

pip, pip install xxx.py

pypi.python.org 去找名字, 知道名字就可以安装了



# python面向对象

## 1. 示例
```py
class Person:
    def init(self, name):
        self.name = name

p1 = Person('小明')
print p1.name == '小明'

```

同理

`str.upper('abs')`和`'abs'.upper`

调用结果是一样的


## 2. 创建类

```py
class Person(object)
    pass
    
xiaoming = Person()
```

参数代表(父类?), 表示是从object类继承下来的
pass: 啥都不想写的时候, 写一个pass




## 3. 给实例赋属性

`setattr(p1, name, 'xx')`





# 特殊方法(是类的方法)
print调用的是class的 __str__ 方法
交互界面: 是 __repr__ 方法

改变 

```py
class Person(object):
    def __init__(self, name, gender):
        self.name = name
        self.gender = gender
    def __str__(self):
        return '(Person: %s, %s)' % (self.name, self.gender)
    __repr__ = __str__
    
```

# 排序

内置数据str, int类型排序使用`sorted()`默认用cmp, 

普通对象比较一定要定义 __cmp__



```py
def __cmp__(self, s):
    if self.name < s.name:
        return -1
    elif self.name > s.name:
        return 1
    else:
        return 0
```

也可以在内部直接使用cmp, 如:

```py
def __cmp__(self, s):
    return cmp(slef.name)
```



-1 不换位置
1 换位置
0 相当




类数组对象获取元素个数调用len()函数对应 __len__(), 返回元素的个数




这么传值, 不用考虑调换的问题
a, b = b, a + b




公约数: 

```py
def gcd(a, b):
    if b == 0:
        return a
    return gcd(b, a % b)
```


getter:  @property

setter: @ClassName.setter




__slots__

会继承下来, 继承部分不需要重写

直接 "__slots__=('attr1', 'attr2', 'attr3')"





可调用对象, 写__call__
