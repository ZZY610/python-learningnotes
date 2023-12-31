# random库用法简析
我们在密码学中提到过随机数和伪随机数发生器的概念。

随机数在公钥密码体制中有着广泛的应用。例如使用随机数作为公钥密码算法中的密钥，RSA加密和数字签名的素数，DES的密钥等。

random模块为我们提供了一个方便且快速的伪随机数发生器。

## 1.random函数 返回[0,1)随机值

* 返回一个落在 `[0,1)` 范围内的随机浮点值。

## 2.uniform函数 返回[a,b]随机值

* 返回指定区间 `[a,b]` 内的一个随机值。

## 3.randint函数 返回随机整数值

* 返回闭区间 `[a,b]` 的一个随机整数值。

## 4.randrange函数

除了开始值和结束值，还有一个步长值。等价于从range(start，stop，step)中选择一个随机值返回。

该函数更加高效，因为它没有真正构造一个区间。

```python
for i in range(5):
    x=rd.random()
    print(x)
    
'''
0.6330991939020664
0.8830984197895451
0.8244798733795524
0.7175085341098889
0.010479617950157616
'''

for i in range(5):
    x=rd.uniform(5,10)
    print(x)

'''
5.123263387529127
9.971978038032656
9.26300088246108
7.740537789455242
8.84881415728859
'''

for x in range(5):
    print(rd.randint(1,100),end=' ')
    
'''
for x in range(5):
    print(rd.randint(1,100),end=' ')
'''

for x in range(5):
    print(rd.randrange(0,100,5),end=' ')
    
'''
10 5 95 5 25 
'''
```

## 5.choice函数 随机选择元素
`rd.choice()`函数，从一个类数组序列中随机选择元素。
```python
#一个简单的抛硬币程序
A={'a':0,'b':0}
B=['a','b']
for x in range(50000):
   A[rd.choice(B)]+=1
print(A)

'''
{'a': 24799, 'b': 25201}
'''
```
## 6.随机排列
`rd.shuffle()`函数，对可变类型进行随机排列。

```python
A=list(range(16))
for i in range(4):
    rd.shuffle(A)
    print(A)
    
'''
[13, 14, 1, 2, 10, 9, 0, 8, 5, 15, 12, 3, 4, 7, 6, 11]
[9, 11, 2, 7, 14, 1, 15, 6, 5, 13, 0, 8, 12, 4, 3, 10]
[4, 10, 1, 5, 7, 15, 9, 0, 2, 3, 14, 6, 8, 13, 11, 12]
[11, 13, 7, 0, 14, 10, 2, 12, 8, 5, 4, 6, 15, 3, 9, 1]
'''
```

## 7.采样

`rd.sample()`函数，从输入数据中返回一个随机无重复值样本，且不会修改输入序列。
```python
with open(r'C:\Users\zzy\PycharmProjects\pythonProject\x.txt','rt') as f:
    words=f.readlines()
words=[w.rstrip() for w in words]
print(rd.sample(words,5))

'''
['smile', 'cat', 'say', 'apple', 'dragon']
'''

x=rd.sample(range(1000),50)
print(x)

'''
[936, 233, 607, 837, 606, 408, 566, 2, 383, 222, 774, 578, 554, 820, 811, 74, 253, 43, 911, 592, 322, 906, 626, 641, 609, 428, 75, 601, 378, 102, 882, 555, 523, 41, 977, 827, 677, 119, 628, 573, 965, 652, 236, 14, 510, 672, 973, 591, 347, 670]

'''
```

## 8.种子
`rd.seed()`函数，用来初始化伪随机数发生器，因为公式是确定的，产生的序列也将是确定的。
```python
for x in range(10):#重复十次
    rd.seed(5)#设定种子
    print(rd.random())
    
'''
0.6229016948897019
0.6229016948897019
0.6229016948897019
0.6229016948897019
0.6229016948897019
0.6229016948897019
0.6229016948897019
0.6229016948897019
0.6229016948897019
0.6229016948897019
'''

for x in range(3):#重复三次
    rd.seed(5)#设定种子
    for i in range(5):#生成五个随机值
        print('{:4.3f}'.format(rd.random()),end=' ')
    print('\n')
    
'''
0.623 0.742 0.795 0.942 0.740 

0.623 0.742 0.795 0.942 0.740 

0.623 0.742 0.795 0.942 0.740 
'''

def fun(n,s=5):
    rd.seed(s)
    x=[]
    for i in range(n):
        x.append(rd.randint(1,1000))
    return x
print(fun(10,5))
print(fun(10,5))
a=fun(10000,5)
b=fun(10000,5)
print(a == b)

'''
[638, 262, 760, 368, 815, 708, 966, 862, 758, 668]
[638, 262, 760, 368, 815, 708, 966, 862, 758, 668]
True

'''
#可以看出，这个伪随机数发生器的周期是非常大的
```
