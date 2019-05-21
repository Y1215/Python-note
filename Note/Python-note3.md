## 迭代器
- isinstance()判断Iterable对象是否可迭代
```python
from collections import Iterable

print(isinstance([],Iterable))

print(isinstance({},Iterable))

```

- 可迭代对象通过__iter__方法提供一个迭代器

- 使用iter()获取可迭代对象中的迭代器
```python
alist = [1,2,3,4,5]
g = iter(alist) 
```


## 生成器
- 生成器是特殊的迭代器

- 创建一个生成器的方式

```python
G = ( x*2 for x in range(5))   #G是生成器
   
def gen():
    for i in range(5):
        yield i
G = gen()    #G 是生成器
```
- send和next的区别
```
send可以向生成器传入一个值，并让程序继续执行
next可以理解为send(None)
注意：对于一个刚被启动的生成器，只能send(None)
```

## 闭包
- 闭包就是在一个外部函数中定义了一个内部函数，并且在内部函数中使用了外部函数的变量，并返回了内部函数。

```python
def callFunc():
    n = 1
    def show():
        print('show: ', n)
    return show

s = callFunc()
s()
# show() 因为 show 函数定义在 callFunc 内部，所以外部不可见，不能使用
```
- **nonlocal 的使用** 

> 如果在闭包的内部函数中直接使用外部函数的变量时，不需要任何操作，直接使用就可以了。但是如果要修改外部变量的值，需要将变量声明为 nonlocal

```python
def callFunc():
    m = 1
    n = 2
    def show():
        print('show - m: ', m)
        nonlocal n #如果不加会报错。
        n *= 10
        print('show - n: ', n)
    return show

s = callFunc()
s()
```

## 装饰器
- 装饰器 本身也是一个函数 ，作用是为现有存在的函数，在不改变函数的基础上去增加一些功能进行装饰。

- 装饰器实际上就是一个函数 ，这个函数以闭包的形式定义

- 在使用这个装饰器函数时，在被装饰的函数的前一行，使用 **@装饰器函数名** 形式来装饰

### 万能装饰器

通过可变参数和关键字参数来接收不同的参数类型

```python
def setFunc(func):
    def wrapper(*args, **kwargs):   # 接收不同的参数
        print('wrapper context')
        return func(*args, *kwargs) # 再原样传回给被装饰的函数

    return wrapper

@setFunc
def show(name, age):
    print(name,age)

show('tom',12)

```

## 私有属性

- 如果在属性名前面加了2个下划线'__'，则表明该属性是私有属性，否则为公有属性
- **私有属性只能在类的内部访问**
 ```python
class People(object):

    def __init__(self, name):
        self.__name = name

    def getName(self):
        return self.__name

    def setName(self, newName):
        if len(newName) >= 5:
            self.__name = newName
        else:
            print("error:名字长度需要大于或者等于5")

xiaoming = People("dongGe")

xiaoming.setName("wanger")
print(xiaoming.getName())
```
## 私有方法

- 私有方法和私有属性类似，在方法名前面加了2个下划线'__'，则表明该方法是私有方法
- **私有方法只能在类内部使用**
- 私有方法和私有属性的 设计目的主要有两个:
  - 保护数据或操作的安全性
  - 向使用者隐藏核心开发细节

```python
class People:

    def __init__(self):
        self.money = 0  # 账户余额

    def send_msg(self):
        if self.money >= 1:
            self.__send_msg()
        else:
            print("余额不足,请充值")

    def __send_msg(self): 
        print("发送短信")


xiaoming = People()
xiaoming.send_msg()
xiaoming.money = 50
xiaoming.send_msg()
```
## `__del__()`方法

- 创建对象后，python解释器默认调用`__init__()`方法；
- 当删除一个对象时，python解释器也会默认调用一个方法，这个方法为`__del__()`方法
- 当有1个变量保存了某个对象的引用时，此对象的引用计数就会加1
- 当使用del删除变量时，只有当变量指向的对象的所有引用都被删除后，也就是引用计数为0时，才会真删除该对象（释放内存空间），这时才会触发`__del__()`方法

```python
import time
class Animal(object):

    # 初始化方法
    # 创建完对象后会自动被调用
    def __init__(self, name):
        print('__init__方法被调用')
        self.__name = name


    # 析构方法
    # 当对象被删除时，会自动被调用
    def __del__(self):
        print("__del__方法被调用")
        print("%s对象马上被干掉了..."%self.__name)

# 创建对象
dog = Animal("哈皮狗")

# 删除对象
del dog

cat = Animal("波斯猫")
cat2 = cat
cat3 = cat

print("---马上 删除cat的对象引用")
del cat
print("---马上 删除cat2的对象引用")
del cat2
print("---马上 删除cat3的对象引用")
del cat3

print("程序2秒钟后结束")
time.sleep(2)
```

## 继承
- 面向对象三大特性: 封装 继承 多态

- 继承：某个类直接具备另一个类的能力（属性和方法）

```python
class Animal:
    def eat(self):
        print("-----吃-----")

class Dog(Animal):
    pass

wang_cai = Dog()
wang_cai.eat()
```

### 子类添加新功能

```python
class Animal:
    def eat(self):
        print("-----吃-----")

class Dog(Animal):
    def bark(self):
        print("-----汪汪叫------")

wang_cai = Dog()
wang_cai.eat()
wang_cai.bark()
```
### 多层继承

```python
class Animal:
    def eat(self):
        print("-----吃-----")

    def drink(self):
        print("-----喝-----")

class Dog(Animal):
    def bark(self):
        print("-----汪汪叫------")

class XTQ(Dog):
    pass

xtq = XTQ()
xtq.eat()
xtq.bark()
```

### 重写父类方法

- 当子类实现一个和父类同名的方法时，叫做 **重写父类方法**
- 子类重写了父类方法，子类再调用该方法将不会执行父类的处理

```python
class Animal:
    def eat(self):
        print("-----吃-----")

    def drink(self):
        print("-----喝-----")


class Dog(Animal):
    def bark(self):
        print("-----汪汪叫------")


class XTQ(Dog):
    """定义了一个哮天犬 类"""
    def bark(self):
        print("----嗷嗷叫-----")

xtq = XTQ()
xtq.eat()
xtq.bark()
```

# 调用被重写的父类方法

- 子类重写了父类方法，仍然想执行父类中的方法，则可以在类中使用`super（）`来调用方法

```python
class Animal(object):
    def eat(self):
        print("-----吃-----")

    def drink(self):
        print("-----喝-----")

class Dog(Animal):
    def bark(self):
        print("-----汪汪叫------")

class XTQ(Dog):
    def bark(self):
        super(XTQ, self).bark()  # 调用已经被重写的方法2
        #super().bark()  # 调用已经被重写的方法3
        print("----嗷嗷叫-----")

xtq = XTQ()
xtq.eat()
xtq.bark()
```

# 私有方法、属性，继承问题

- 父类中的 私有方法、属性，不会被子类继承
- 可以通过调用继承的父类的共有方法，间接的访问父类的私有方法、属性

```python
class Animal(object):
    def __init__(self):
        self.num1 = 1
        self.__num2 = 2

    def __run(self):
        print("----跑---")

    def test(self):
        print(self.__num2)
        self.__run()

class Dog(Animal):
    def bark(self):
        print("-----汪汪叫------")
        # self.__run()  # 父类中的私有方法，没有被子类继承
        print(self.num1)
        # print(self.__num2)  # 父类中的私有属性，没有被子类继承

wang_cai = Dog()
wang_cai.bark()
wang_cai.test()
```

### 多继承

```python
class A:
    def printA(self):
        print('----A----')

class B:
    def printB(self):
        print('----B----')

class C(A,B):
    def printC(self):
        print('----C----')

obj_C = C()
obj_C.printA()
obj_C.printB()
```

### 多继承重名
```python
class A:
    def printA(self):
        print('----A----')

class B:
    def printA(self):
        print('----B----')

class C(A,B):
    def printA(self):
        # super(C, self).printA()
        # super(A, self).printA()
        A.printA(self)
        B.printA(self)
        print('----C----')

obj_C = C()
obj_C.printA()
```

## 类方法、静态方法
```python
class Dog:
    def demo_method(self):
        print("对象方法")

    @classmethod    #类方法
    def demo_method(cls):
        print("类方法")

    @staticmethod    #静态方法
    def demo_method():  # 被最后定义,调用时优先执行
        print("静态方法")

dog1 = Dog()
Dog.demo_method()  # 结果: 静态方法
dog1.demo_method()  # 结果: 静态方法
```
## `__str__()`方法

- 如果直接print打印对象，会看到创建出来的对象在内存中的地址
- 当使用`print（xx）`输出对象的时候，只要对象定义了`__str__(self)`方法，就会 **打印该方法return的信息描述**

```python
class Cat:

    def __init__(self, new_name, new_age):
        self.name = new_name
        self.age = new_age 
       
    def __str__(self):
        return "名字是:%s , 年龄是:%d" % (self.name, self.age)

tom = Cat("汤姆", 30)
print(tom)
```

## 多态

- Python崇尚“鸭子类型”。
- 所谓多态：定义时的类型和运行时的类型不一样，此时就成为多态
- ***鸭子类型语言中，函数/方法可以接受一个任意类型的对象作为参数/返回值，只要该对象实现了代码后续用到的属性和方法就不会报错***


## 实例属性、类属性

- 实例属性:
  - 通过类创建的对象 又称为 **实例对象**，**对象属性 又称为 实例属性**，记录对象各自的数据，不同对象的同名实例属性，记录的数据可能各不相同
- 类属性:
  - 类属性就是 **类对象** 所拥有的属性，它被 **该类的所有实例对象 所共有**。
  - 类属性可以使用 **类对象** 或 **实例对象** 访问

```python
class Dog:
    type = "狗"  # 类属性

dog1 = Dog()
dog1.name = "旺财"

# 类属性  取值
print(Dog.type)  # 结果：狗
print(dog1.type)  # 结果：狗

```
> 提示：在python中 “万物皆对象”，类本身也是一个对象，执行class语句时会被创建，称为 类对象。

#### 使用场景：

- **类的实例 记录的某项数据 始终保持一致时**，则定义类属性。
- **实例属性** 要求 **每个对象** 为其 **单独开辟一份内存空间** 来记录数据，而 **类属性** 为全类所共有 ，**仅占用一份内存**，**更加节省内存空间**。

- 注意点：
  - 1> **尽量避免类属性和对象属性同名**。如果有同名对象属性，**实例对象会优先访问对象属性**
  - 2> **类属性只能通过类对象修改，不能通过实例对象修改**
  - 3> 类属性也可以设置为 **私有**，前边添加两个下划线。 如:

## `__new__`方法

- 创建对象时，系统会自动调用**new**方法
- 开发者可以实现**new**方法来自定义对象的创建过程

```python
class Cat:
    def __new__(cls, name):
        print("创建对象")
        # return super().__new__(cls)
        return object.__new__(cls)

    def __init__(self, name):
        print("对象初始化")
        self.name = name

    def __str__(self):
        return "%s" % self.name


lanmao = Cat("蓝猫")
lanmao.age = 20
print(lanmao)
```
#### 总结
- `__new__`至少要有一个参数cls，代表要实例化的类，此参数在实例化时由Python解释器自动提供
- `__new__`必须要有返回值，返回实例化出来的实例，这点在自己实现`__new__`时要特别注意，可以return父类`__new__`出来的实例，或者直接是object的`__new__`出来的实例
- `__init__`有一个参数self，就是这个`__new__`返回的实例，`__init__`在`__new__`的基础上可以完成一些其它初始化的动作，`__init__`不需要返回值
- 如果创建对象时传递了自定义参数，且重写了new方法，则new也必须 "预留" 该形参,否则init方法将无法获取到该参数
