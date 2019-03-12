chapter07-面向对象之绑定方法与非绑定方法



### **一 类中定义的函数分成两大类**

**一：绑定方法（绑定给谁，谁来调用就自动将它本身当作第一个参数传入）：**

　　　　**1. 绑定到类的方法：用classmethod装饰器装饰的方法。**

​                **为类量身定制**

​                **类.boud_method(),自动将类当作第一个参数传入**

​              **（其实对象也可调用，但仍将类当作第一个参数传入）**

　　　　**2. 绑定到对象的方法：没有被任何装饰器装饰的方法。**

​               **为对象量身定制**

​               **对象.boud_method(),自动将对象当作第一个参数传入**

​             **（属于类的函数，类可以调用，但是必须按照函数的规则来，没有自动传值那么一说）**

**二：非绑定方法：用staticmethod装饰器装饰的方法**

　　      **1. 不与类或对象绑定，类和对象都可以调用，但是没有自动传值那么一说。就是一个普通工具而已**

　　　　**注意：与绑定到对象方法区分开，在类中直接定义的函数，没有被任何装饰器装饰的，都是绑定到对象的方法，可不是普通函数，对象调用该方法会自动传值，而staticmethod装饰的方法，不管谁来调用，都没有自动传值一说**



### 二 绑定方法

绑定给对象的方法（略）

绑定给类的方法（classmethod）

　　classmehtod是给类用的，即绑定到类，类在使用时会将类本身当做参数传给类方法的第一个参数（即便是对象来调用也会将类当作第一个参数传入），python为我们内置了函数classmethod来把类中的函数定义成类方法

settings.py

```python
HOST='127.0.0.1'
PORT=3306
DB_PATH=r'C:\Users\Administrator\PycharmProjects\test\面向对象编程\test1\db'
```



```python
import settings
class MySQL:
    def __init__(self,host,port):
        self.host=host
        self.port=port

    @classmethod
    def from_conf(cls):
        print(cls)
        return cls(settings.HOST,settings.PORT)

print(MySQL.from_conf) #<bound method MySQL.from_conf of <class '__main__.MySQL'>>
conn=MySQL.from_conf()

conn.from_conf() #对象也可以调用，但是默认传的第一个参数仍然是类
```



### 三 非绑定方法

在类内部用staticmethod装饰的函数即非绑定方法，就是普通函数

statimethod不与类或对象绑定，谁都可以调用，没有自动传值效果

```python
import hashlib
import time
class MySQL:
    def __init__(self,host,port):
        self.id=self.create_id()
        self.host=host
        self.port=port
    @staticmethod
    def create_id(): #就是一个普通工具
        m=hashlib.md5(str(time.time()).encode('utf-8'))
        return m.hexdigest()


print(MySQL.create_id) #<function MySQL.create_id at 0x0000000001E6B9D8> #查看结果为普通函数
conn=MySQL('127.0.0.1',3306)
print(conn.create_id) #<function MySQL.create_id at 0x00000000026FB9D8> #查看结果为普通函数
```



### 四 classmethod与staticmethod的区别

mariadb是mysql

```python
import settings
class MySQL:
    def __init__(self,host,port):
        self.host=host
        self.port=port

    @staticmethod
    def from_conf():
        return MySQL(settings.HOST,settings.PORT)

    # @classmethod #哪个类来调用,就将哪个类当做第一个参数传入
    # def from_conf(cls):
    #     return cls(settings.HOST,settings.PORT)

    def __str__(self):
        return '就不告诉你'

class Mariadb(MySQL):
    def __str__(self):
        return '<%s:%s>' %(self.host,self.port)


m=Mariadb.from_conf()
print(m) #我们的意图是想触发Mariadb.__str__,但是结果触发了MySQL.__str__的执行，打印就不告诉你：
```



### 五 练习

定义MySQL类

　　1.对象有id、host、port三个属性

　　2.定义工具create_id，在实例化时为每个对象随机生成id，保证id唯一

　　3.提供两种实例化方式，方式一：用户传入host和port 方式二：从配置文件中读取host和port进行实例化

　　4.为对象定制方法，save和get_obj_by_id，save能自动将对象序列化到文件中，文件路径为配置文件中DB_PATH,文件名为id号，保存之前验证对象是否已经存在，若存在则抛出异常，;get_obj_by_id方法用来从文件中反序列化出对象

创建唯一id之UUID

```python
原文链接：http://www.cnblogs.com/dkblog/archive/2011/10/10/2205200.html
 Python官方Doc：《20.15. uuid — UUID objects according to RFC 4122》
    UUID的算法介绍：《A Universally Unique IDentifier (UUID) URN Namespace》

概述：

    UUID是128位的全局唯一标识符，通常由32字节的字符串表示。
    它可以保证时间和空间的唯一性，也称为GUID，全称为：
            UUID —— Universally Unique IDentifier      Python 中叫 UUID
            GUID —— Globally Unique IDentifier          C#  中叫 GUID

    它通过MAC地址、时间戳、命名空间、随机数、伪随机数来保证生成ID的唯一性。
    UUID主要有五个算法，也就是五种方法来实现：

       1、uuid1()——基于时间戳

               由MAC地址、当前时间戳、随机数生成。可以保证全球范围内的唯一性，
               但MAC的使用同时带来安全性问题，局域网中可以使用IP来代替MAC。

       2、uuid2()——基于分布式计算环境DCE（Python中没有这个函数）

                算法与uuid1相同，不同的是把时间戳的前4位置换为POSIX的UID。
                实际中很少用到该方法。

      3、uuid3()——基于名字的MD5散列值

                通过计算名字和命名空间的MD5散列值得到，保证了同一命名空间中不同名字的唯一性，
                和不同命名空间的唯一性，但同一命名空间的同一名字生成相同的uuid。    

       4、uuid4()——基于随机数

                由伪随机数得到，有一定的重复概率，该概率可以计算出来。

       5、uuid5()——基于名字的SHA-1散列值

                算法与uuid3相同，不同的是使用 Secure Hash Algorithm 1 算法

使用方面：

    首先，Python中没有基于DCE的，所以uuid2可以忽略；
    其次，uuid4存在概率性重复，由无映射性，最好不用；
    再次，若在Global的分布式计算环境下，最好用uuid1；
    最后，若有名字的唯一性要求，最好用uuid3或uuid5。

编码方法：

    # -*- coding: utf-8 -*-

    import uuid

    name = "test_name"
    namespace = "test_namespace"

    print uuid.uuid1()  # 带参的方法参见Python Doc
    print uuid.uuid3(namespace, name)
    print uuid.uuid4()
    print uuid.uuid5(namespace, name)
```



```python
#settings.py内容
'''
HOST='127.0.0.1'
PORT=3306
DB_PATH=r'E:\CMS\aaa\db'
'''
import settings
import uuid
import pickle
import os
class MySQL:
    def __init__(self,host,port):
        self.id=self.create_id()
        self.host=host
        self.port=port

    def save(self):
        if not self.is_exists:
            raise PermissionError('对象已存在')
        file_path=r'%s%s%s' %(settings.DB_PATH,os.sep,self.id)
        pickle.dump(self,open(file_path,'wb'))

    @property
    def is_exists(self):
        tag=True
        files=os.listdir(settings.DB_PATH)
        for file in files:
            file_abspath=r'%s%s%s' %(settings.DB_PATH,os.sep,file)
            obj=pickle.load(open(file_abspath,'rb'))
            if self.host == obj.host and self.port == obj.port:
                tag=False
                break
        return tag
    @staticmethod
    def get_obj_by_id(id):
        file_abspath = r'%s%s%s' % (settings.DB_PATH, os.sep, id)
        return pickle.load(open(file_abspath,'rb'))

    @staticmethod
    def create_id():
        return str(uuid.uuid1())

    @classmethod
    def from_conf(cls):
        print(cls)
        return cls(settings.HOST,settings.PORT)

# print(MySQL.from_conf) #<bound method MySQL.from_conf of <class '__main__.MySQL'>>
conn=MySQL.from_conf()
conn.save()

conn1=MySQL('127.0.0.1',3306)
conn1.save() #抛出异常PermissionError: 对象已存在


obj=MySQL.get_obj_by_id('7e6c5ec0-7e9f-11e7-9acc-408d5c2f84ca')
print(obj.host)
```

其他练习

```python
class Date:
    def __init__(self,year,month,day):
        self.year=year
        self.month=month
        self.day=day
    @staticmethod
    def now(): #用Date.now()的形式去产生实例,该实例用的是当前时间
        t=time.localtime() #获取结构化的时间格式
        return Date(t.tm_year,t.tm_mon,t.tm_mday) #新建实例并且返回
    @staticmethod
    def tomorrow():#用Date.tomorrow()的形式去产生实例,该实例用的是明天的时间
        t=time.localtime(time.time()+86400)
        return Date(t.tm_year,t.tm_mon,t.tm_mday)

a=Date('1987',11,27) #自己定义时间
b=Date.now() #采用当前时间
c=Date.tomorrow() #采用明天的时间

print(a.year,a.month,a.day)
print(b.year,b.month,b.day)
print(c.year,c.month,c.day)


#分割线==============================
import time
class Date:
    def __init__(self,year,month,day):
        self.year=year
        self.month=month
        self.day=day
    @staticmethod
    def now():
        t=time.localtime()
        return Date(t.tm_year,t.tm_mon,t.tm_mday)

class EuroDate(Date):
    def __str__(self):
        return 'year:%s month:%s day:%s' %(self.year,self.month,self.day)

e=EuroDate.now()
print(e) #我们的意图是想触发EuroDate.__str__,但是结果为
'''
输出结果:
<__main__.Date object at 0x1013f9d68>
'''
因为e就是用Date类产生的,所以根本不会触发EuroDate.__str__,解决方法就是用classmethod

import time
class Date:
    def __init__(self,year,month,day):
        self.year=year
        self.month=month
        self.day=day
    # @staticmethod
    # def now():
    #     t=time.localtime()
    #     return Date(t.tm_year,t.tm_mon,t.tm_mday)

    @classmethod #改成类方法
    def now(cls):
        t=time.localtime()
        return cls(t.tm_year,t.tm_mon,t.tm_mday) #哪个类来调用,即用哪个类cls来实例化

class EuroDate(Date):
    def __str__(self):
        return 'year:%s month:%s day:%s' %(self.year,self.month,self.day)

e=EuroDate.now()
print(e) #我们的意图是想触发EuroDate.__str__,此时e就是由EuroDate产生的,所以会如我们所愿
'''
输出结果:
year:2017 month:3 day:3
'''
```

