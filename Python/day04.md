day03-函数基础

# 函数知识体系

什么是函数？
为什么要用函数？
函数的分类：内置函数与自定义函数
如何自定义函数？
​	- 语法
​	- 定义有参数函数，及有参函数的应用场景
​	- 定义无参数函数，及无参函数的应用场景
​	- 定义空函数，及空函数的应用场景
调用函数
​	- 如何调用函数
​	- 函数的返回值
​	- 函数参数的应用：形参和实参，位置参数，关键字参数，默认参数，*args，**kwargs
高阶函数（函数对象）
函数嵌套
作用域与名称空间
装饰器
迭代器与生成器及协程函数
三元运算，列表解析、生成器表达式
函数的递归调用
内置函数
面向过程编程与函数式编程

# 一、引子

**一、为何要用函数之不用函数的问题**

```python
#1、代码的组织结构不清晰，可读性差
#2、遇到重复的功能只能重复编写实现代码，代码冗余
#3、无法统一管理且维护难度极大。功能需要扩展时，需要找出所有实现该功能的地方进行修改
```

**二、函数是什么？**

```python
针对这个问题，想象生活中的例子，修理工需要在工具箱里放好锤子，扳手，钳子等工具，然后遇到锤钉子的场景，拿来锤子用就可以，而无需临时再制造一把锤子。

修理工===>程序员
具备某一功能的工具===>函数

要想使用工具，需要事先准备好，然后拿来就用且可以重复使用
要想用函数，需要先定义，再使用
```

**三、函数分类**

```python
#1、内置函数
为了方便我们的开发，针对一些简单的功能，python解释器已经为我们定义好了的函数即内置函数。对于内置函数，我们可以拿来就用而无需事先定义，如len(),sum(),max()

#2、自定义函数
很明显内置函数所能提供的功能是有限的，这就需要我们自己根据需求，事先定制好我们自己的函数来实现某种功能，以后，在遇到应用场景时，调用自定义的函数即可。例如
```

### 二、定义函数

**一、如何自定义函数？**

```python
#函数名要能反映其意义
#语法
def 函数名(参数1,参数2,参数3,...):
    '''文档注释：对函数进行完整描述，比如函数功能，参数，返回值'''
    函数体
    return 返回的值
```

```python
def auth(user:str,password:str)->int:
    '''
    auth function
    :param user: 用户名
    :param password: 密码
    :return: 认证结果
    '''
    if user == 'amy' and password == '123':
        return 1
# print(auth.__annotations__) #{'user': <class 'str'>, 'password': <class 'str'>, 'return': <class 'int'>}

user=input('用户名>>: ').strip()
pwd=input('密码>>: ').strip()
res=auth(user,pwd)
print(res)
```

**二、函数使用的原则：先定义，再调用**

```python
# 函数即“变量”，“变量”必须先定义后引用。未定义而直接引用函数，就相当于在引用一个不存在的变量名

#测试一
def foo():
    print('from foo')
    bar() #未定义
foo() #报错

#测试二
def bar():
    print('from bar')

def foo():
    print('from foo')
    bar()
foo() #正常

#测试三
def foo():
    print('from foo')
    bar()
    
def bar():
    print('from bar')
foo() #会报错


#结论:函数的使用,必须遵循原则:先定义,后调用
#先定义函数，再拼接逻辑。
#我们在使用函数时,一定要明确地区分定义阶段和调用阶段

#定义阶段
def foo():
    print('from foo')
    bar()
def bar():
    print('from bar')
#调用阶段
foo()
```


**三、函数在定义阶段都干了哪些事？**

```python
#只检测语法，不执行代码
也就说，语法错误在函数定义阶段就会检测出来，而代码的逻辑错误只有在执行时才会知道
```

**四、定义函数的三种形式**

```python
#1、无参：应用场景仅仅只是执行一些操作，比如与用户交互，打印
#2、有参：需要根据外部传进来的参数，才能执行相应的逻辑，比如统计长度，求最大值最小值。通常有返回值
#3、空函数：设计代码结构
```

1. 无参、有参

```python
#定义阶段
def tell_tag(tag,n): #有参数
    print(tag*n)

def tell_msg(): #无参数
    print('hello world')

#调用阶段
tell_tag('*',12)
tell_msg()
tell_tag('*',12)

'''
************
hello world
************
'''

#结论：
#1、定义时无参，意味着调用时也无需传入参数
#2、定义时有参，意味着调用时则必须传入参数
```

2. 空函数

```python
#程序的体系结构立见    
def auth(user,password):                             
    '''                                                           
    auth function                                                 
    :param user: 用户名                                              
    :param password: 密码                                           
    :return: 认证结果                                                 
    '''                                                           
    pass                                                          
                                                                  
def get(filename):                                                
    '''                                                           
    :param filename:                                              
    :return:                                                      
    '''                                                           
    pass                                                          
                                                                  
def put(filename):                                                
    '''                                                           
    :param filename:                                              
    :return:                                                      
    '''                                                           
def ls(dirname):                                                  
    '''                                                           
    :param dirname:                                               
    :return:                                                      
    '''                                                           
    pass                                                                 
```


### 三、调用函数

**一、调用函数**

```
函数的调用：函数名加括号
1 先找到名字
2 根据名字调用代码
```

**二、函数返回值**

```python
#无return->None

#return 1个值->返回1个值

#return 逗号分隔多个值->元组

#返回值没有类型限制

#什么时候该有返回值？
　　　　调用函数，经过一系列的操作，最后要拿到一个明确的结果，则必须要有返回值
　　　　通常有参函数需要有返回值，输入参数，经过计算，得到一个最终的结果

#什么时候不需要有返回值？
　　　　调用函数，仅仅只是执行一系列的操作，最后不需要得到什么结果，则无需有返回值
　　　　通常无参函数不需要有返回值
```

```python
# 序列的解压
t = (1,2,3)
a,b,c = t # 少一个会报错 ValueError: too many values to unpack (expected 3)
print(a)
```

**三、函数调用的三种形式**

```python
1 语句形式：foo()
2 表达式形式：3*len('hello')
3 当作另外一个函数的参数：range(len('hello'))
```


# 四、函数的参数

**一、形参与实参**

```python
#从大的角度看，函数的参数分为两种：形参、实参
#形参即变量名，实参即变量值，函数调用时，将值绑定到变量名上，函数调用结束，解除绑定。

def foo(x,y): # 定义参数名，没有检查语法，只是一个标识符，不占内存空间。
    print(x)
    print(y)

foo(1,2) # 调用函数时，实参的值要传给形参，相当于变量的赋值操作。此时，形参才会开辟一块内存空间。函数执行完，释放这块内存地址。

# 函数定义阶段到底做了什么事？
声明了一个变量名，指向了一块函数的内存地址。
只检查函数体的语法正不正确，并不会执行。

```

**二、具体应用**

1. 位置参数：按照从左到右的顺序定义的参数
   - 位置`形参`：必选参数
   - 位置`实参`：按照位置给形参传值
2. 关键字参数：按照key=value的形式定义的`实参`
  无需按照位置为形参传值。
  注意的问题：
  - 关键字实参必须在位置实参右面
  - 对同一个形参不能重复传值
3. 默认参数：`形参`在定义时就已经为其赋值
  可以传值也可以不传值，经常需要变的参数定义成位置形参，变化较小的参数定义成默认参数（形参）。
  注意的问题：
  - 只在定义时赋值一次
  - 默认参数的定义应该在位置形参右面
  - 默认参数通常应该定义成不可变类型


4. 可变长参数：
可变长指的是`实参`值的个数不固定
而实参有按位置和按关键字两种形式定义，针对这两种形式的可变长，形参对应有两种解决方案来完整地存放它们，分别是*args，**kwargs
```python
# ===========*args 位置实参可变长===========
def foo(x,y,*args):
    print(x,y)
    print(args) # args <==> (3,4)
foo(1,2,3,4) # 3,4 <==> *(3,4)

def foo(x,y,z):
    print(x,y,z)
foo(*[1,2,3])
# *后面可以跟元组或列表，*后面跟的值，都是对应的可变的位置参数。元组或列表中的元素就是位置参数

# ===========**kwargs 关键字实参可变长===========
def foo(x,y,**kwargs):
    print(x,y)
    print(kwargs)
foo(1,y=2,a=1,b=2,c=3)

def foo(x,y,**kwargs):
    print(x,y)
    print(kwargs)
foo(1,y=2,**{'a':1,'b':2,'c':3})

def foo(x,y,z):
    print(x,y,z)
foo(**{'z':1,'x':2,'y':3})

# ===========*args+**kwargs===========
def foo(x,y):
    print(x,y)

def wrapper(*args,**kwargs):
    print('====>')
    foo(*args,**kwargs)
```
5. 命名关键字参数：*后定义的参数，必须被传值（有默认值的除外），且`必须`按照`关键字实参`的形式传递。
可以保证，传入的参数中一定包含某些关键字。
```python
def foo(x,y,*args,a=1,b,**kwargs):
    print(x,y)
    print(args)
    print(a)
    print(b)
    print(kwargs)

foo(1,2,3,4,5,b=3,c=4,d=5)
结果：
    1
    2
    (3, 4, 5)
    1
    3
    {'c': 4, 'd': 5}
```



# 五、练习题

1、写函数，，用户传入修改的文件名，与要修改的内容，执行函数，完成批了修改操作
2、写函数，计算传入字符串中【数字】、【字母】、【空格] 以及 【其他】的个数

3、写函数，判断用户传入的对象（字符串、列表、元组）长度是否大于5。

4、写函数，检查传入列表的长度，如果大于2，那么仅保留前两个长度的内容，并将新内容返回给调用者。

5、写函数，检查获取传入列表或元组对象的所有奇数位索引对应的元素，并将其作为新列表返回给调用者。

6、写函数，检查字典的每一个value的长度,如果大于2，那么仅保留前两个长度的内容，并将新内容返回给调用者。
dic = {"k1": "v1v1", "k2": [11,22,33,44]}
PS:字典中的value只能是字符串或列表

```python
#题目一
def modify_file(filename,old,new):
    import os
    with open(filename,'r',encoding='utf-8') as read_f,\
        open('.bak.swap','w',encoding='utf-8') as write_f:
        for line in read_f:
            if old in line:
                line=line.replace(old,new)
            write_f.write(line)
    os.remove(filename)
    os.rename('.bak.swap',filename)

modify_file('/Users/jieli/PycharmProjects/爬虫/a.txt','alex','SB')

#题目二
def check_str(msg):
    res={
        'num':0,
        'string':0,
        'space':0,
        'other':0,
    }
    for s in msg:
        if s.isdigit():
            res['num']+=1
        elif s.isalpha():
            res['string']+=1
        elif s.isspace():
            res['space']+=1
        else:
            res['other']+=1
    return res

res=check_str('hello name:aSB passowrd:alex3714')
print(res)


#题目三：略

#题目四
def func1(seq):
    if len(seq) > 2:
        seq=seq[0:2]
    return seq
print(func1([1,2,3,4]))


#题目五
def func2(seq):
    return seq[::2]
print(func2([1,2,3,4,5,6,7]))


#题目六
def func3(dic):
    d={}
    for k,v in dic.items():
        if len(v) > 2:
            d[k]=v[0:2]
    return d
print(func3({'k1':'abcdef','k2':[1,2,3,4],'k3':('a','b','c')}))
```



 