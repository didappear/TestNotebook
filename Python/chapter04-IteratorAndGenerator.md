chapter04-迭代器、生成器、面向过程编程



# 一、迭代器

## **一 迭代的概念**

```python
#迭代器即迭代的工具，那什么是迭代呢？
#迭代是一个重复的过程，每次重复即一次迭代，并且每次迭代的结果都是下一次迭代的初始值
while True: #只是单纯地重复，因而不是迭代
    print('===>') 
    
l=[1,2,3]
count=0
while count < len(l): #迭代
    print(l[count])
    count+=1
```



## **二 为何要有迭代器？什么是可迭代对象？什么是迭代器对象？**

```python
#1、为何要有迭代器？
对于序列类型：字符串、列表、元组，我们可以使用索引的方式迭代取出其包含的元素。但对于字典、集合、文件等类型是没有索引的，若还想取出其内部包含的元素，则必须找出一种不依赖于索引的迭代方式，这就是迭代器

#2、什么是可迭代对象？
可迭代对象指的是内置有__iter__方法的对象，即obj.__iter__，如下
'hello'.__iter__
(1,2,3).__iter__
[1,2,3].__iter__
{'a':1}.__iter__
{'a','b'}.__iter__
open('a.txt').__iter__

#3、什么是迭代器对象？
可迭代对象执行obj.__iter__()得到的结果就是迭代器对象
而迭代器对象指的是即内置有__iter__又内置有__next__方法的对象

文件类型是迭代器对象
open('a.txt').__iter__()
open('a.txt').__next__()


#4、注意：
迭代器对象一定是可迭代对象，而可迭代对象不一定是迭代器对象
```



## **三 迭代器对象的使用**



```python
dic={'a':1,'b':2,'c':3}
iter_dic=dic.__iter__() #得到迭代器对象，迭代器对象即有__iter__又有__next__，但是：迭代器.__iter__()得到的仍然是迭代器本身
iter_dic.__iter__() is iter_dic #True

print(iter_dic.__next__()) #等同于next(iter_dic)
print(iter_dic.__next__()) #等同于next(iter_dic)
print(iter_dic.__next__()) #等同于next(iter_dic)
# print(iter_dic.__next__()) #抛出异常StopIteration，或者说结束标志

#有了迭代器，我们就可以不依赖索引迭代取值了
iter_dic=dic.__iter__()
while 1:
    try:
        k=next(iter_dic)
        print(dic[k])
    except StopIteration:
        break
        
#这么写太丑陋了，需要我们自己捕捉异常，控制next，python这么牛逼，能不能帮我解决呢？能，请看for循环
```



## **四 for循环**

```python
#基于for循环，我们可以完全不再依赖索引去取值了
dic={'a':1,'b':2,'c':3}
for k in dic:
    print(dic[k])

#for循环的工作原理
#1：执行in后对象的dic.__iter__()方法，得到一个迭代器对象iter_dic
#2: 执行next(iter_dic),将得到的值赋值给k,然后执行循环体代码
#3: 重复过程2，直到捕捉到异常StopIteration,结束循环
```



## **五 迭代器的优缺点**

```python
#优点：
  - 提供一种统一的、不依赖于索引的迭代方式
  - 惰性计算，节省内存
#缺点：
  - 无法获取长度（只有在next完毕才知道到底有几个值）
  - 一次性的，只能往后走，不能往前退
```



# 二 生成器

## **一 什么是生成器**

```python
#只要函数内部包含有yield关键字，那么函数名()的到的结果就是生成器，并且不会执行函数内部代码

def func():
    print('====>first')
    yield 1
    print('====>second')
    yield 2
    print('====>third')
    yield 3
    print('====>end')

g=func()
print(g) #<generator object func at 0x0000000002184360> 
```



## **二 生成器就是迭代器**

```python
g.__iter__
g.__next__
#2、所以生成器就是迭代器，因此可以这么取值
res=next(g)
print(res)
```

## **三 练习**

1、自定义函数模拟range(1,7,2)

2、模拟管道，实现功能:tail -f access.log | grep '404'

```python
#题目一：
def my_range(start,stop,step=1):
    while start < stop:
        yield start
        start+=step

#执行函数得到生成器，本质就是迭代器
obj=my_range(1,7,2) #1  3  5
print(next(obj))
print(next(obj))
print(next(obj))
print(next(obj)) #StopIteration

#应用于for循环
for i in my_range(1,7,2):
    print(i)

#题目二
import time
def tail(filepath):
    with open(filepath,'rb') as f:
        f.seek(0,2)
        while True:
            line=f.readline()
            if line:
                yield line
            else:
                time.sleep(0.2)

def grep(pattern,lines):
    for line in lines:
        line=line.decode('utf-8')
        if pattern in line:
            yield line

for line in grep('404',tail('access.log')):
    print(line,end='')

#测试
with open('access.log','a',encoding='utf-8') as f:
    f.write('出错啦404\n')
```



## **四 协程函数**

```python
#yield关键字的另外一种使用形式：表达式形式的yield
def eater(name):
    print('%s 准备开始吃饭啦' %name)
    food_list=[]
    while True:
        food=yield food_list
        print('%s 吃了 %s' % (name,food))
        food_list.append(food)

g=eater('egon')
g.send(None) #对于表达式形式的yield，在使用时，第一次必须传None，g.send(None)等同于next(g)
g.send('蒸羊羔')
g.send('蒸鹿茸')
g.send('蒸熊掌')
g.send('烧素鸭')
g.close()
g.send('烧素鹅')
g.send('烧鹿尾')
```



**五 练习**
1、编写装饰器，实现初始化协程函数的功能

2、实现功能：grep  -rl  'python'  /etc

```python
#题目一：
def init(func):
    def wrapper(*args,**kwargs):
        g=func(*args,**kwargs)
        next(g)
        return g
    return wrapper
@init
def eater(name):
    print('%s 准备开始吃饭啦' %name)
    food_list=[]
    while True:
        food=yield food_list
        print('%s 吃了 %s' % (name,food))
        food_list.append(food)

g=eater('egon')
g.send('蒸羊羔')

#题目二：
#注意：target.send(...)在拿到target的返回值后才算执行结束
import os
def init(func):
    def wrapper(*args,**kwargs):
        g=func(*args,**kwargs)
        next(g)
        return g
    return wrapper

@init
def search(target):
    while True:
        filepath=yield
        g=os.walk(filepath)
        for dirname,_,files in g:
            for file in files:
                abs_path=r'%s\%s' %(dirname,file)
                target.send(abs_path)
@init
def opener(target):
    while True:
        abs_path=yield
        with open(abs_path,'rb') as f:
            target.send((f,abs_path))
@init
def cat(target):
    while True:
        f,abs_path=yield
        for line in f:
            res=target.send((line,abs_path))
            if res:
                break
@init
def grep(pattern,target):
    tag=False
    while True:
        line,abs_path=yield tag
        tag=False
        if pattern.encode('utf-8') in line:
            target.send(abs_path)
            tag=True
@init
def printer():
    while True:
        abs_path=yield
        print(abs_path)


g=search(opener(cat(grep('你好',printer()))))
# g.send(r'E:\CMS\aaa\db')
g=search(opener(cat(grep('python',printer()))))
g.send(r'E:\CMS\aaa\db')
```



## **六 yield总结**

```python
#1、把函数做成迭代器
#2、对比return，可以返回多次值，可以挂起/保存函数的运行状态
```



## 三 面向过程编程



```python
#1、首先强调：面向过程编程绝对不是用函数编程这么简单，面向过程是一种编程思路、思想，而编程思路是不依赖于具体的语言或语法的。言外之意是即使我们不依赖于函数，也可以基于面向过程的思想编写程序

#2、定义
面向过程的核心是过程二字，过程指的是解决问题的步骤，即先干什么再干什么

基于面向过程设计程序就好比在设计一条流水线，是一种机械式的思维方式

#3、优点：复杂的问题流程化，进而简单化

#4、缺点：可扩展性差，修改流水线的任意一个阶段，都会牵一发而动全身

#5、应用：扩展性要求不高的场景，典型案例如linux内核，git，httpd

#6、举例
流水线1：
用户输入用户名、密码--->用户验证--->欢迎界面

流水线2：
用户输入sql--->sql解析--->执行功能
```



 ps：函数的参数传入，是函数吃进去的食物，而函数return的返回值，是函数拉出来的结果，面向过程的思路就是，把程序的执行当做一串首尾相连的功能，该功能可以是函数的形式，然后一个函数吃，拉出的东西给另外一个函数吃，另外一个函数吃了再继续拉给下一个函数吃。。。

示例:复杂的问题变得简单,但扩展功能麻烦

```python
#=============复杂的问题变得简单
#注册功能:
#阶段1: 接收用户输入账号与密码,完成合法性校验
def talk():
    while True:
        username=input('请输入你的用户名: ').strip()
        if username.isalpha():
            break
        else:
            print('用户必须为字母')

    while True:
        password1=input('请输入你的密码: ').strip()
        password2=input('请再次输入你的密码: ').strip()
        if password1 == password2:
            break
        else:
            print('两次输入的密码不一致')

    return username,password1

#阶段2: 将账号密码拼成固定的格式
def register_interface(username,password):
    format_str='%s:%s\n' %(username,password)
    return format_str

#阶段3: 将拼好的格式写入文件
def handle_file(format_str,filepath):
    with open(r'%s' %filepath,'at',encoding='utf-8') as f:
        f.write(format_str)


def register():
    user,pwd=talk()
    format_str=register_interface(user,pwd)
    handle_file(format_str,'user.txt')


register()


#=============牵一发而动全身,扩展功能麻烦
#阶段1: 接收用户输入账号与密码,完成合法性校验
def talk():
    while True:
        username=input('请输入你的用户名: ').strip()
        if username.isalpha():
            break
        else:
            print('用户必须为字母')

    while True:
        password1=input('请输入你的密码: ').strip()
        password2=input('请再次输入你的密码: ').strip()
        if password1 == password2:
            break
        else:
            print('两次输入的密码不一致')


    role_dic={
        '1':'user',
        '2':'admin'
    }
    while True:
        for k in role_dic:
            print(k,role_dic[k])

        choice=input('请输入您的身份>>: ').strip()
        if choice not in role_dic:
            print('输入的身份不存在')
            continue
        role=role_dic[choice]

    return username,password1,role

#阶段2: 将账号密码拼成固定的格式
def register_interface(username,password,role):
    format_str='%s:%s:%s\n' %(username,password,role)
    return format_str

#阶段3: 将拼好的格式写入文件
def handle_file(format_str,filepath):
    with open(r'%s' %filepath,'at',encoding='utf-8') as f:
        f.write(format_str)


def register():
    user,pwd,role=talk()
    format_str=register_interface(user,pwd,role)
    handle_file(format_str,'user.txt')


register()


#ps:talk内对用户名\密码\角色的合法性校验也可以摘出来做成单独的功能,但本例就写到一个函数内了,力求用更少的逻辑来为大家说明过程式编程的思路
```

 