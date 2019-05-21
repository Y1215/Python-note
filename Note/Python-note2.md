## Map
Map会将一个函数映射到一个输入列表的所有元素上。
### 规范
map(function_to_apply, list_of_inputs)  
通常的方法
```python
items = [1, 2, 3, 4, 5]
squared = []
for i in items:
    squared.append(i**2)
```
用Map实现
```pythonitems = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, items))

```
## Filter  
filter过滤列表中的元素，并且返回一个由所有符合要求的元素所构成的列表，符合要求即函数映射到该元素时返回值为True. 
```python
number_list = range(-5, 5)
less_than_zero = filter(lambda x: x < 0, number_list)
print(list(less_than_zero))  
```
> Output: [-5, -4, -3, -2, -1]

## reduce
- reduce() 函数会对参数序列中元素进行累积。
- 函数将一个数据集合（链表，元组等）中的所有数据进行下列操作：用传给 reduce 中的函数 function（有两个参数）先对集合中的第 1、2 个元素进行操作，得到的结果再与第三个数据用 function 函数运算，最后得到一个结果。
```python
from functools import reduce

def add(x, y) :
    return x + y

print(reduce(add, [1, 2, 3, 4, 5]))
```
# 文件状态
## os模块


## time模块

## open()的不同模式
- r	 以读方式打开文件，可读取文件信息。
- w	 以写方式打开文件，可向文件写入信息。如文件存在，则清空该文件，再写入新内容
- a	 以追加模式打开文件（即一打开文件，文件指针自动移到文件末尾），如果文件不存在则创建
- r+ 以读写方式打开文件，可对文件进行读和写操作。
- w+ 消除文件内容，然后以读写方式打开文件。
- a+ 以读写方式打开文件，并把文件指针移到文件尾。
- b	 以二进制模式打开文件，而不是以文本模式。该模式只对 Windows 或 Dos 有效，类 Unix 的文件是用二进制模式进行操作的。

### 内建函数
- fp.read([size]) #size为读取的长度，以byte为单位 
- fp.readline([size]) #读一行，如果定义了size，有可能返回的只是一行的一部分 ,如果超过，则返回一行到\n 
- fp.readlines([size]) #把文件每一行作为一个list的一个成员，并返回这个list。其实它的内部是通过循环调用readline()来实现的。如果提供size参数，size是表示读取内容的总长，也就是说可能只读到文件的一部分。 
- fp.write(str) #把str写到文件中，write()并不会在str后加上一个换行符 
- fp.writelines(seq) #把seq的内容全部写到文件中(多行一次性写入)。这个函数也只是忠实地写入，不会在每行后面加上任何东西。 
- fp.close() #关闭文件。python会在一个文件不用后自动关闭文件，不过这一功能没有保证，最好还是养成自己关闭的习惯。 如果一个文件在关闭后还对其进行操作会产生ValueError 
- fp.flush() #把缓冲区的内容写入硬盘 
- fp.fileno() #返回一个长整型的”文件标签“ 
- fp.isatty() #文件是否是一个终端设备文件（unix系统中的） 
- fp.tell() #返回文件操作标记的当前位置，以文件的开头为原点 
- fp.next() #返回下一行，并将文件操作标记位移到下一行。把一个file用于for … in file这样的语句时，就是调用next()函数来实现遍历的。 
- fp.seek(offset[,whence]) #将文件打操作标记移到offset的位置。这个offset一般是相对于文件的开头来计算的，一般为正数。但如果提供了whence参数就不一定了，whence可以为0表示从头开始计算，1表示以当前位置为原点计算。2表示以文件末尾为原点进行计算。需要注意，如果文件以a或a+的模式打开，每次进行写操作时，文件操作标记会自动返回到文件末尾。 
- fp.truncate([size]) #把文件裁成规定的大小，默认的是裁到当前文件操作标记的位置。如果size比文件的大小还要大，依据系统的不同可能是不改变文件，也可能是用0把文件补到相应的大小，也可能是以一些随机的内容加上去。

## 正则表达式

### 元字符
- .	 匹配除换行符以外的任意字符
- \w 匹配字母或数字或下划线或汉字
- \s 匹配任意的空白符
- \d 匹配数字
- \b 匹配单词的开始或结束
- ^  匹配字符串的开始
- $	 匹配字符串的结束

### 重复
- \*	重复零次或更多次
- \+	重复一次或更多次
- ?	    重复零次或一次
- {n}	重复n次
- {n,}	重复n次或更多次
- {n,m}	重复n到m次
### 分组
- (\d{1,3}\.){3}  
()重复三次

### re 模块
re.match
- 切分字符串  
re.split

### 贪婪与懒惰
#### 贪婪匹配
- a.*b，它将会匹配最长的以 a 开始，以 b 结束的字符串。如果用它来搜索 aabab 的话，它会匹配整个字符串 aabab。这被称为贪婪匹配。

#### 懒惰匹配
- *?	 重复任意次，但尽可能少重复
- +?	 重复1次或更多次，但尽可能少重复
- ??	 重复0次或1次，但尽可能少重复
- {n,m}? 重复n到m次，但尽可能少重复
- {n,}?	 重复n次以上，但尽可能少重复

## 异常
### NameError异常
```python
print(dave)
```
>  -->NameError: name 'dave' is not defined

### ZeroDivisionError: 除数为零
```python
print(1/0)
```

> -->ZeroDivisionError: division by zero

### SyntaxError: Python 语法错误
```python
print(for)
```
> -->SyntaxError: invalid syntax

### IndexError:请求的索引超出序列范围
```python
a=[]
print(a[0])
```
> -->IndexError: list index out of range

### KeyError:请求一个不存在的字典关键字
```python
aDict = {'host': 'earth', 'port': 80}
print(aDict['server'])
```
> -->KeyError: 'server'

### IOError: 输入/输出错误
```python
f=open('anqing.txt')
print(f)
```
> -->IOError: [Errno 2] No such file or directory: 'anqing.txt'


### AttributeError: 尝试访问未知的物件属性
```python
class myClass(object):
    pass
myInst = myClass()
myInst.bar = 'spam'
print(myInst.bar)
print(myInst.dave)
```
> -->AttributeError: 'myClass' object has no attribute 'dave'