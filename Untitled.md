
# 第一章数据结构和算法
## 1.1将序列分解为单个变量
任何一个可以迭代的对象都可以通过简单的赋值操作分解成单个变量

```python
p = (2, 3)
x, y = p
```


```python
x
```




    2




```python
y
```




    3




```python
data = ['acme', 90, 40, (2018,12,12)]
name, price, shares, date = data
name
```




    'acme'




```python
date
```




    (2018, 12, 12)




```python
name, price, shares, (a,d,c) = data
```


```python
a
```




    2018




```python
d
```




    12




```python
#如果数量不匹配
a, c = data
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-10-5f238cb0a912> in <module>()
          1 #如果数量不匹配
    ----> 2 a, c = data
    

    ValueError: too many values to unpack (expected 2)


*只要对象可以迭代，那么就可以进行分解操作*


```python
h, e, l , l, o = 'hello'
```


```python
h
```




    'h'




```python
l
```




    'l'



*可以使用一些用不到的变量名来丢弃数值*


```python
h, _,_,_,_ = 'hello'
```


```python
h
```




    'h'



## 1.2从任意长度的可迭代对象分解元素

**解决方案：‘*表达式’可以解决这个问题**


```python
#去掉最高最低分取得平均分
def drop_first_last(data):
    fist,*_,last = data
    return avg(middle)
```


```python
record = ('libo', 'libo_vae@163.com', '110', '120')
name, email, phone, *others = record
name
```




    'libo'




```python
*others
```


      File "<ipython-input-19-06f24e34ae71>", line 1
        *others
               ^
    SyntaxError: can't use starred expression here
    



```python
others
```




    ['120']




```python
record = ('libo', 'libo_vae@163.com', '110', '120')
*others, phome = record
```


```python
others
#返回的是一个列表
```




    ['libo', 'libo_vae@163.com', '110']



*语句在迭代一个可变长度的元组序列时，尤其有用


```python
record = [('a',1,2), ('b',1,2,3), ('c',1,2,3,4)]

for i, *other in record:
    print(i, other)
```

    a [1, 2]
    b [1, 2, 3]
    c [1, 2, 3, 4]
    


```python
#综合使用
record = ['abc',50,123.3,(2018,12,20)]
name, *ignore, (year, *ignore) = record

```


```python
name
```




    'abc'




```python
year
```




    2018




```python
#一个神奇的递归，使用*表达式
def _sum(items):
    head, *_it = items
    return head + sum(_it) if _it else head
```


```python
_sum([i for i in range(10)])
```




    45




```python
from collections import deque

def search(lines, pattern, history=5):
    previous_lines = deque(maxlen=history)
    for line in lines:
        if pattern in line:
            yield line, previous_lines
        previous_lines.append(line)
```

**注意这里的previous_lines.append（line）放在yield的后面，依然会被执行，这与return不相同。如果把这一句提前，会print两次python**
* deque（maxlen=number）创建了一个固定长度的队列，当有新的记录出现的时候，且队列已经满了，旧的会被挤掉，新的添加。
有append方法


```python
with open('untitled.txt', encoding='utf-8') as f:
    for line, previlines in search(f, 'python', 5):
        for pline in previlines:
            print(pline)
        print(line, end='')
        print('-'*10)
```

    12345
    
    678910
    
    python
    ----------
    678910
    
    python
    
    1112
    
    1
    
    2
    
    python
    ----------
    


```python
q = deque(maxlen=3)
q.append(1)
q.append(2)
q.append(3)
q.append(4)
```


```python
q
```




    deque([2, 3, 4])




```python
#当不指定maxlen的时候，队列长度无限
q = deque()
q.append(1)
q.append(2)
```


```python
q
```




    deque([1, 2])




```python
q.appendleft(10)
```


```python
q
```




    deque([10, 1, 2])




```python
q.pop()
```




    2




```python
q.popleft()
```




    10




```python
q
```




    deque([1])



## 1.4寻找最大最小的n个元素
在某个集合中寻找最大或者最小的n个元素


```python
# heapq 中有两个函数——nlargest（）和nsmallest（）， 可以完成这一工作
import heapq
num = [-1.2,3,3-45,33-4,44,0]
print(heapq.nlargest(3, num))
print(heapq.nsmallest(2, num))
```

    [44, 29, 3]
    [-42, -1.2]
    


```python
# 这两个函数还可以接受参数key， 传递一个函数fun， 根据fun的值进行选择
portfolio = [
    {'name':'libo', 'price':10, 'grades':3},
    {'name':'libobo','price':11, 'grades':4}
]
print(heapq.nlargest(2, portfolio, key=lambda s: s['grades']))
print(heapq.nsmallest(1, portfolio, key=lambda s: s['grades']))
```

    [{'name': 'libobo', 'price': 11, 'grades': 4}, {'name': 'libo', 'price': 10, 'grades': 3}]
    [{'name': 'libo', 'price': 10, 'grades': 3}]
    

使用堆的方法可以更加节省性能，当数据量很大，我们只要取得最小的那么几个的时候，这个速度更快


```python
import math
nums = [int(math.sin(3*i)*15) for i in range(10)]
nums
```




    [0, 2, -4, 6, -8, 9, -11, 12, -13, 14]




```python
heap = list(nums)
heapq.heapify(heap) # heapify（）
heap
```




    [-13, -8, -11, 2, 0, 9, -4, 12, 6, 14]




```python
heapq.heappop(heap)
```




    -13




```python
heapq.heappop(heap)
# secondly
```




    -11



值得注意的是，当获取的数量和集合元素数量接近时，排序是最好的办法，sorted（item）[:N]

## 1.5实现优先级队列
对不同元素按照优先级进行排序


```python
class Item():
    def __init__(self, name):
        self.name = name
    def __repr__(self):
        return 'Item({!r})'.format(self.name)
```


```python
item1 = Item('book')
```


```python
item1
# 注意这里使用！r和不使用它的区别，不使用时：结果是Item（book），没有引号
```




    Item('book')




```python
import heapq
class PriorityQueue():
    def __init__(self):
        self.queue = []
        self.index = 0
    def push(self, item, priority):
        heapq.heappush(self.queue, (-priority, self.index, item)) # 注意 heappush（）的用法
        self.index += 1 # index是第二排序准则
    def pop(self):
        return heapq.heappop(self.queue)
```


```python
q = PriorityQueue()
q.push(Item('foo'), 2)
q.push(Item('fooo'), 2)
q.push(Item('foooo'), 3)
q.push('123', 4)
```


```python
q.pop()
```




    (-4, 3, '123')




```python
q.pop()
```




    (-3, 2, Item('foooo'))




```python
q.pop()
```




    (-2, 0, Item('foo'))




```python
q.pop()
```




    (-2, 1, Item('fooo'))



（priority，item）形式也可以比较优先级，但优先级相同会出现问题。可以考虑添加inde


```python
a = (1, 'a')
b = (2, 'b')
```


```python
a < b
```




    True




```python
b < (2, 'c')
```




    True



## 1.6在字典中将键映射到多个值上


```python
d = {
    'a':[1,2],
    'b':[3,4]
}
```


```python
d['c']=[5,6]
```


```python
d
```




    {'a': [1, 2], 'b': [3, 4], 'c': [5, 6]}




```python
from collections import defaultdict

d = defaultdict(list)
d['a'].append(1)
d['a'].append(2)
d['b'].append(3)
```


```python
d
```




    defaultdict(list, {'a': [1, 2], 'b': [3]})




```python
# 自己构建多值映射也可以
d = {
    'a':[2]
}
pairs = [
    ('a',1),
    (2,2)
]
for key, values in pairs:
    if key not in d:
        d[key] = []
    d[key].append(values)
```


```python
d
```




    {2: [2], 'a': [2, 1]}



## 1.7让字典有序
可以使用collection中的OrderDict类。对字典做迭代时候，会按照添加顺序


```python
from collections import OrderedDict

d = OrderedDict()
d['a'] = 1
d['b'] = 2
```


```python
d
```




    OrderedDict([('a', 1), ('b', 2)])




```python
import json
json.dumps(d)
```




    '{"a": 1, "b": 2}'



## 1.8与字典有关的排序问题


```python
# 找到股价最低的股票
price ={
    'Alibaba':30,
    'Tenxun':20,
    'IBM':21,
    'FB':35
}
```


```python
min_price = min(zip(price.values(), price.keys()))
```


```python
min_price
```




    (20, 'Tenxun')




```python
# zip()的用法
z = zip(['a', 'b', 'c'], [1, 2, 3])
```


```python
print(list(z))
```

    [('a', 1), ('b', 2), ('c', 3)]
    

其他一般方法


```python
min(price)
```




    'Alibaba'




```python
max(price)
```




    'Tenxun'




```python
min(price.values())
```




    20




```python
key = min(price, key=lambda x: price[x]) # 'Tenxun'
```


```python
price[key]
```




    20



## 1.9寻找两个字典中的相同点（key，value）


```python
a = {
    'a':1,
    'y':2,
    'c':3
}
b = {
    'x':3,
    'y':2,
    'z':1
}
```


```python
# Find keys in common
a.keys() & b.keys()
```




    {'y'}




```python
a.values() & b.values()
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-190-29f31ab220a5> in <module>()
    ----> 1 a.values() & b.values()
    

    TypeError: unsupported operand type(s) for &: 'dict_values' and 'dict_values'



```python
a.keys() | b.keys()
```




    {'a', 'c', 'x', 'y', 'z'}




```python
a.keys() - b.keys()
```




    {'a', 'c'}




```python
# find (key, value) pairs in item
a.items() & b.items()
```




    {('y', 2)}




```python
a.items() | b.items()
```




    {('a', 1), ('c', 3), ('x', 3), ('y', 2), ('z', 1)}




```python
list(a.items())
```




    [('a', 1), ('y', 2), ('c', 3)]



## 1.10从序列总移除重复项且保持元素间的迅速不变


```python
# 如果序列中的值是可哈希的（hashable）
def dedupe(items):
    seen = set()
    for item in items:
        if item not in seen:
            yield item
            seen.add(item)
```


```python
a = [i*i for i in range(-5,5)]
```


```python
list(dedupe(a))
```




    [25, 16, 9, 4, 1, 0]




```python
a.append([1,2])
a
```




    [25, 16, 9, 4, 1, 0, 1, 4, 9, 16, [1, 2]]




```python
[1,2] in a
```




    True




```python
list(dedupe(a))
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-220-10e1eea397e9> in <module>()
    ----> 1 list(dedupe(a))
    

    <ipython-input-214-74764d4efed6> in dedupe(items)
          3     seen = set()
          4     for item in items:
    ----> 5         if item not in seen:
          6             yield item
          7             seen.add(item)
    

    TypeError: unhashable type: 'list'



```python
set(a) # 列表不能哈希化
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-221-5b4c643a6feb> in <module>()
    ----> 1 set(a)
    

    TypeError: unhashable type: 'list'


1.可哈希（hashable）和不可改变性（immutable）

如果一个对象在自己的生命周期中有一哈希值（hash value）是不可改变的，那么它就是可哈希的（hashable）的，因为这些数据结构内置了哈希值，每个可哈希的对象都内置了__hash__方法，所以可哈希的对象可以通过哈希值进行对比，也可以作为字典的键值和作为set函数的参数。所有python中所有不可改变的的对象（imutable objects）都是可哈希的，比如字符串，元组，也就是说可改变的容器如字典，列表不可哈希（unhashable）。我们用户所定义的类的实例对象默认是可哈希的（hashable），它们都是唯一的，而hash值也就是它们的id()。

2.  哈希

它是一个将大体量数据转化为很小数据的过程，甚至可以仅仅是一个数字，以便我们可以用在固定的时间复杂度下查询它，所以，哈希对高效的算法和数据结构很重要。

3. 不可改变性

它指一些对象在被创建之后不会因为某些方式改变，特别是针对任何可以改变哈希对象的哈希值的方式

4. 联系：

因为哈希键一定是不可改变的，所以它们对应的哈希值也不改变。如果允许它们改变，，那么它们在数据结构如哈希表中的存储位置也会改变，因此会与哈希的概念违背，效率会大打折扣


```python
# 稍作改变，使得值哈希化
def dedupe(items, key=None):
    seen = set()
    for item in items:
        val = item if key==None else key(item)
        if val not in seen:
            yield item
        seen.add(val)
```


```python
b = [{'x':1,'y':2}, {'x':4,'y':2}, {'x':1,'y':3}, {'x':1,'y':2}]
de = dedupe(b, key=lambda x:(x['x'],x['y']))
```


```python
list(de) # 去除了重复的{'x':1,'y':2}
```




    [{'x': 1, 'y': 2}, {'x': 4, 'y': 2}, {'x': 1, 'y': 3}]



其实使用集合的方法就可以实现，但是定义函数的目的在于，对任意可以迭代对象都可以使用该方法，比如 \n
f = open('aa.txt')
for line in dedupe(f):
    ……


```python
set(b)
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-228-1390747a0750> in <module>()
    ----> 1 set(b)
    

    TypeError: unhashable type: 'dict'

