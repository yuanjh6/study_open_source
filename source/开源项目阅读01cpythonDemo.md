# 开源项目阅读01cpythonDemo
## beer
地址:https://github.com/python/cpython/blob/master/Tools/demo/beer.py
```
n = 100
if sys.argv[1:]:
    n = int(sys.argv[1])
```  
更佳写法:  
```
n = int(sys.argv[1]) if sys.argv[1:] else 100) #推荐，易懂
n = sys.argv[1:] and int(sys.argv[1]) or 100)
```

## hanoi
地址:https://github.com/python/cpython/blob/master/Tools/demo/hanoi.py  
```
    if sys.argv[2:]:
        bitmap = sys.argv[2]
        # Reverse meaning of leading '@' compared to Tk
        if bitmap[0] == '@': bitmap = bitmap[1:]
        else: bitmap = '@' + bitmap
    else:
        bitmap = None
```
更佳写法:(一定程度上牺牲了可读性)  
```
bitmap=None if not sys.argv[2:] else sys.argv[2][1] if sys.argv[2][0]=='@' else '@' + sys.argv[2]
```

## rpythond
地址:https://github.com/python/cpython/blob/master/Tools/demo/rpythond.py  
```
def execute(request):
    stdout = sys.stdout
    stderr = sys.stderr
    sys.stdout = sys.stderr = fakefile = io.StringIO() #链式赋值
    try:
        try:
            exec(request, {}, {})
        except:
            print()
            traceback.print_exc(100) #代替print e 来输出详细的异常信息
    finally:
        sys.stderr = stderr # 替换回来
        sys.stdout = stdout
    return fakefile.getvalue()
```

try except finally如果使用with的上下文管理逻辑更好  

## vector
地址:https://github.com/python/cpython/blob/master/Tools/demo/vector.py  
```

class Vec:
    """A simple vector class.
    Instances of the Vec class can be constructed from numbers
    >>> a = Vec(1, 2, 3)
    >>> b = Vec(3, 2, 1)
    added
    >>> a + b
    Vec(4, 4, 4)
    subtracted
    >>> a - b
    Vec(-2, 0, 2)
    and multiplied by a scalar on the left
    >>> 3.0 * a
    Vec(3.0, 6.0, 9.0)
    or on the right
    >>> a * 3.0
    Vec(3.0, 6.0, 9.0)
    """
    def __init__(self, *v):
        self.v = list(v)

    @classmethod
    def fromlist(cls, v):
        if not isinstance(v, list):
            raise TypeError
        inst = cls() #创建实例？
        inst.v = v
        return inst

    def __repr__(self):
        args = ', '.join(repr(x) for x in self.v)
        return 'Vec({})'.format(args)

    def __len__(self):
        return len(self.v)

    def __getitem__(self, i):
        return self.v[i]

    def __add__(self, other):
        # Element-wise addition
        v = [x + y for x, y in zip(self.v, other.v)]
        return Vec.fromlist(v)

    def __sub__(self, other):
        # Element-wise subtraction
        v = [x - y for x, y in zip(self.v, other.v)]# 列表同位置元素减法运算
        return Vec.fromlist(v)

    def __mul__(self, scalar):
        # Multiply by scalar
        v = [x * scalar for x in self.v]
        return Vec.fromlist(v)

    __rmul__ = __mul__ # 函数赋值，(int*自己)和（自己*int）同方法


def test():
    import doctest
    doctest.testmod() # 文档测试方法

test()
```

## 要点
```
sys.stdout = sys.stderr = fakefile = io.StringIO() #链式赋值
traceback.print_exc(100) #代替print e 来输出详细的异常信息
inst = cls() #创建实例？
v = [x - y for x, y in zip(self.v, other.v)]# 列表同位置元素减法运算
__rmul__ = __mul__ # 函数赋值，(int*自己)和（自己*int）同方法
doctest.testmod() # 文档测试方法
```
