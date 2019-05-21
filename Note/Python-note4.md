## 万能装饰器
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

### `__call__`方法
```python
class Student(object):
    def __init__(self, name):
        self.name = name

    def __call__(self, classmate):
        print("我的名字是%s, 我的同桌叫%s" % (self.name, classmate))


s = Student("小明")
s("小红")  # 直接调用实例化对象
```

- 在类中通过使用 `__init__` 和` __call__`方法来实现

```python
class Test(object):
    # 通过初始化方法，将要被装饰的函数传进来并记录下来
    def __init__(self, func):
        self.__func = func
    # 重写 __call__ 方法来实现装饰内容
    def __call__(self, *args, **kwargs):
        print('wrapper context')
        self.__func(*args, **kwargs)


# 实际通过类的魔法方法call来实现
@Test  # --> show = Test(show) show由原来引用函数,装饰后变成引用Test装饰类的对象
def show():
    pass

show()  # 对象调用方法,实际上是调用魔法方法call,实现了装饰器
```
## property
在 Python 中，提供了一个叫做 property 的类，通过对这个创建这个类的对象的设置，在使用对象的私有属性时，可以不在使用属性的函数的调用方式，而像普通的公有属性一样去使用属性，为开发提供便利。