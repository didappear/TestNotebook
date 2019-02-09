Chapter03-Python文件处理

# 一、文件操作

**一、介绍**

计算机系统分为：计算机硬件，操作系统，应用程序三部分。

我们用python或其他语言编写的应用程序若想要把数据永久保存下来，必须要保存于硬盘中，这就涉及到应用程序要操作硬件。众所周知，应用程序是无法直接操作硬件的，这就用到了操作系统。

操作系统把复杂的硬件操作封装成简单的接口给用户/应用程序使用，其中文件就是操作系统提供给应用程序来操作硬盘的虚拟概念，用户或应用程序通过操作文件，可以将自己的数据永久保存下来。

有了文件的概念，我们无需再去考虑操作硬盘的细节，只需要关注操作文件的流程：

```python
#1. 打开文件，得到文件对象并赋值给一个变量
#2. 对文件进行操作(读、写)
#3. 关闭文件
```

**二、在python中**

```python
#1. 打开文件，得到文件对象并赋值给一个变量
f=open('a.txt','r',encoding='utf-8') #默认打开模式就为r

#2. 通过文件对象对文件进行操作(对操作系统发起系统调用，操作系统从硬盘里取出文件，把内容返回给你)
data=f.read()

#3. 关闭文件(如果不关，操作系统要一直开这个接口，一直占用操作系统的资源)
f.close()
```

**三、f=open('a.txt','r')的过程分析**

```python
#1、由我们写的Python应用程序向操作系统发起系统调用open(...)

#2、操作系统打开该文件，并返回一个文件对象给应用程序

#3、应用程序将文件对象赋值给变量f
```

**四、强调！！！**

1. 强调第一点：字符编码

```python
#强调第一点：字符编码
f=open(...)是由操作系统打开文件，那么如果我们没有为open指定编码，那么打开文件的默认编码很明显是操作系统说了算了，操作系统会用自己的默认编码去打开文件，在windows下是gbk，在linux下是utf-8。
这就用到了上节课讲的字符编码的知识：若要保证不乱码，文件以什么方式存的，就要以什么方式打开。

f=open('a.txt','r',encoding='utf-8')
```

2. 强调第二点：资源回收

```python
# 强调第二点：资源回收
打开一个文件包含两部分资源：操作系统级打开的文件+应用程序的变量。在操作完毕一个文件时，必须把与该文件的这两部分资源一个不落地回收，回收方法为：
1、f.close() #回收操作系统级打开的文件
2、del f #回收应用程序级的变量

其中del f一定要发生在f.close()之后，否则就会导致操作系统打开的文件还没有关闭，白白占用资源，
而python自动的垃圾回收机制决定了我们无需考虑del f，这就要求我们，在操作完毕文件后，一定要记住f.close()

虽然这么说，但是很多同学还是会很不要脸地忘记f.close(),对于这些不长脑子的同学，我们推荐傻瓜式操作方式：使用with关键字来帮我们管理上下文
with open('a.txt','w') as f:
    pass
 
with open('a.txt','r') as read_f,open('b.txt','w') as write_f:
    data=read_f.read()
    write_f.write(data)
```

**五、python2中的file与open**

```python
#首先在python3中操作文件只有一种选择，那就是open()

#而在python2中则有两种方式：file()与open()
两者都能够打开文件，对文件进行操作，也具有相似的用法和参数，但是，这两种文件打开方式有本质的区别，file为文件类，用file()来打开文件，相当于这是在构造文件类，而用open()打开文件，是用python的内建函数来操作，我们一般使用open()打开文件进行操作，而用file当做一个类型，比如type(f) is file
```

# 二、打开文件的模式

```python
文件对象 = open('文件路径', '模式')
```

模式可以是以下方式以及他们之间的组合：

| Character | Meaning                                                      |
| --------- | ------------------------------------------------------------ |
| ‘r'       | open for reading (default)                                   |
| ‘w'       | open for writing, truncating the file first                  |
| ‘a'       | open for writing, appending to the end of the file if it exists |
| ‘b'       | binary mode                                                  |
| ‘t'       | text mode (default)                                          |
| ‘+'       | open a disk file for updating (reading and writing)          |
| ‘U'       | universal newline mode (for backwards compatibility; should not be used in new code) |



```python
#1. 打开文件的模式有(默认为文本模式)：
r，只读模式【默认模式，文件必须存在，不存在就报错】
w，只写模式【不可读；不存在则创建；存在则清空内容】
a，之追加写模式【不可读；不存在则创建；存在则只追加内容】

#2. 对于非文本文件，我们只能使用b模式，"b"表示以字节的方式操作（而所有文件也都是以字节的形式存储的，使用这种模式无需考虑文本文件的字符编码、图片文件的jgp格式、视频文件的avi格式）
rb 
wb
ab
注：以b方式打开时，读取到的内容是字节类型，写入时也需要提供字节类型，不能指定编码

#3. 了解部分
"+" 表示可以同时读写某个文件
r+， 读写【可读，可写】
w+，写读【可读，可写】
a+， 写读【可读，可写】


x， 只写模式【不可读；不存在则创建，存在则报错】
x+ ，写读【可读，可写】
xb
```



了解U模式与换行符 

```python
# 回车与换行的来龙去脉
http://www.cnblogs.com/linhaifeng/articles/8477592.html

# U模式
'U' mode is deprecated and will raise an exception in future versions
of Python.  It has no effect in Python 3.  Use newline to control
universal newlines mode.

# 总结：
在python3中使用默认的newline=None即可，换行符无论何种平台统一用\n即可
```



# 三、操作文件的方法

```python
#掌握
f.read() #读取所有内容,光标移动到文件末尾
f.readline() #读取一行内容,光标移动到第二行首部
f.readlines() #读取每一行内容,存放于列表中

f.write('1111\n222\n') #针对文本模式的写,需要自己写换行符
f.write('1111\n222\n'.encode('utf-8')) #针对b模式的写,需要自己写换行符
f.writelines(['333\n','444\n']) #文件模式
f.writelines([bytes('333\n',encoding='utf-8'),'444\n'.encode('utf-8')]) #b模式

#了解
f.readable() #文件是否可读
f.writable() #文件是否可读
f.closed #文件是否关闭
f.encoding #如果文件打开模式为b,则没有该属性
f.flush() #立刻将文件内容从内存刷到硬盘
f.name #文件名称
```

```python
# f.readlind()一个细节
f = open('testfile.txt',mode='r',encoding='utf-8')
data1 = f.readline()
data2 = f.readline()
print(data1,end='')
print(data2,end='')

f.close()
# print()函数默认 end='\n'，指定 end=''，去掉print()函数默认的换行符
def print(self, *args, sep=' ', end='\n', file=None): 
```

```python
文件有修改一说，文件是来自于操作系统的概念。
但是硬盘上的数据没有修改一说,全都是覆盖操作。
文件的修改是将文件加载到内存，在内存中修改完成，保存时，将文件覆盖到硬盘上。


```



练习，利用b模式，编写一个cp工具，要求如下：

　　1. 既可以拷贝文本又可以拷贝视频，图片等文件

　　2. 用户一旦参数错误，打印命令的正确使用方法，如usage: cp source_file target_file

　　提示：可以用import sys，然后用sys.argv获取脚本后面跟的参数

```python
import sys
if len(sys.argv) != 3:
    print('usage: cp source_file target_file')
    sys.exit()

source_file,target_file=sys.argv[1],sys.argv[2]
with open(source_file,'rb') as read_f,open(target_file,'wb') as write_f:
    for line in read_f:
        write_f.write(line)
```



# 四、文件内光标移动

一: read(3)：

　　1. 文件打开方式为文本模式时，代表读取3个字符

　　2. 文件打开方式为b模式时，代表读取3个字节

二: 其余的文件内光标移动都是以字节为单位如seek，tell，truncate

注意：

　　1. seek有三种移动方式0，1，2，其中1和2必须在b模式下进行，但无论哪种模式，都是以bytes为单位移动的

　　2. truncate是截断文件，所以文件的打开方式必须可写，但是不能用w或w+等方式打开，因为那样直接清空文件了，所以truncate要在r+或a或a+等模式下测试效果

练习：基于seek实现tail -f功能

```python
import time
with open('test.txt','rb') as f:
    f.seek(0,2)
    while True:
        line=f.readline()
        if line:
            print(line.decode('utf-8'))
        else:
            time.sleep(0.2)

```



# **五、文件的修改**

文件的数据是存放于硬盘上的，因而只存在覆盖、不存在修改这么一说，我们平时看到的修改文件，都是模拟出来的效果，具体的说有两种实现方式：

方式一：将硬盘存放的该文件的内容全部加载到内存，在内存中是可以修改的，修改完毕后，再由内存覆盖到硬盘（word，vim，nodpad++等编辑器）

```python
import os

with open('a.txt') as read_f,open('.a.txt.swap','w') as write_f:
    data=read_f.read() #全部读入内存,如果文件很大,会很卡
    data=data.replace('alex','SB') #在内存中完成修改

    write_f.write(data) #一次性写入新文件

os.remove('a.txt')
os.rename('.a.txt.swap','a.txt') 
```

方式二：将硬盘存放的该文件的内容一行一行地读入内存，修改完毕就写入新文件，最后用新文件覆盖源文件

```python
import os

with open('a.txt') as read_f,open('.a.txt.swap','w') as write_f:
    for line in read_f:
        line=line.replace('alex','SB')
        write_f.write(line)

os.remove('a.txt')
os.rename('.a.txt.swap','a.txt') 
```

**总结**

```python
with open('a.txt',mood='r',encoding='utf-8') as f:
    f.read()
    f.seek()
    f.readline()
    f.readline()
    
with open('a.txt',mood='w',encoding='utf-8') as f:
    f.write('123\n')
    f.writelines(['123\n','abc\n'])
    
with open('a.txt',mood='w',encoding='utf-8') as f:
    f.write('123\n')
    f.writelines(['123\n','abc\n'])
    
r+
w+
a+
rb
wb
```





练习题：

```python
1. 文件a.txt内容：每一行内容分别为商品名字，价钱，个数，求出本次购物花费的总钱数
apple 10 3
tesla 100000 1
mac 3000 2
lenovo 30000 3
chicken 10 3

2. 修改文件内容，把文件中的alex都替换成SB
```



```python
buffer 还没有写到硬盘上的内容，内存往硬盘上写要经过一个时间差。一个数据写一次经历一次IO延迟。IO层面的问题就放大了。先不往硬盘写，存到buffer中，经历一次IO延迟
cache 读到内存中，CPU经常取的数据。
```

