# Python基础入门

## 开发工具

- Python安装自带的IDLE
- Anaconda
- aptana
- pycharm

## Python基础

### 常量与变量

#### 常量

常量文件：`const.py`

```python
#coding=utf-8

class _const(object):
    class ConstError(TypeError):pass

    def __setattr__(self, key, value):
        if self.__dict__.has_key(key):
            raise self.ConstError("Can't rebind const(%s)" % key);
        self.__dict__[key]=value

    def __delattr__(self, key):
        if key in self.__dict__:
            raise self.ConstError("Can't rebind const(%s)" % key);
        raise NameError,key

import sys
sys.modules[__name__]=_const()
```

测试使用：

```python
#coding=utf-8

import const

const.value=23
print(const.value)
const.value=25
```

结果：

```shell
23
Traceback (most recent call last):
  File "D:/myspace/python/HelloPython/src/testConst.py", line 7, in <module>
    const.value=25
  File "D:\myspace\python\HelloPython\src\const.py", line 8, in __setattr__
    raise self.ConstError("Can't rebind const(%s)" % key);
const.ConstError: Can't rebind const(value)
```

#### 变量

```python
#codeing=utf-8

i=10
print(i)
```

全局变量`global`：

```python
#coding

def testFun():
    global i
    i=10

testFun()
print(i)  #10
```

### 数据类型

#### 数

- 整数型（`int`）：如:0、1、-1
- 长整型（`long`）:后缀是`l`，如：8888l、-9999l
- 浮点 型（float）：如：3.14
- 布尔型（bool）：只有两个值：True、False
- 复数型（complex）:如：4+2j、-9+8j等形式

#### 字符串

- 字符串：在Python中用引号引起来的字符集是字符串，如`'hello'、"helo chen"`等。

  引号包含：

  - 单引号：单引号中可以包含双引号，如：`'hello  "chentiefeng"' `
  - 双引号：双引号可以包含单引号，如：`"hello i'm chentiefeng"`
  - 三引号：三引号(`'''test'''`或者`"""test"""`)引起来的字符串可以换行

- 自然字符串：在字符串前加`r`，如：`r"hello world"`，自然字符串可以将转义符也原样保留，不进行转义处理。

- 字符串的重复：可以使用`字符串*数字`重复字符串， 如：重复20次hello：`"hello"*20`
- 子字符串：有几种取法：`[索引]、[开始索引:]、[:结束索引]、[开始索引:结束索引]`

  - 索引从0开始
- 转义符`\`：如`\n`：换行

  - 注意：转义符放在行末可以做连接符连接两行内容

##### 常用方法

- `replace`字符串替换

  ```python
  a = 'hello word'
  b = a.replace('word','python')
  print(b) #hello python
  ```

  注意：还可以使用正则表达式替换，如下：

  ```python
  import re
  a = 'hello word'
  strinfo = re.compile('word')
  b = strinfo.sub('python',a)
  print(b) #hello python
  print(re.sub('word','python',a)) #hello python
  ```

- 

#### 列表

列表：列表用来存储一连串元素的容器，使用`[]`表示。

- 列表中的元素是有序排列的
- 下标从0开始

示例：

```python
#coding=utf-8

learnLang=['java','python','shell']
print(learnLang) #['java', 'python', 'shell']
print(learnLang[2])#shell
```

#### 元组(tuple)

元组类似列表，但是元组有以下特征：

- 元组使用符合`()`
- 元组里的值不能修改

示例：

```python
#coding=utf-8

learnLang=('java','python','shell')
print(learnLang) #('java','python','shell')
print(learnLang[2])#shell
learnLang[2]='html5'  #TypeError: 'tuple' object does not support item assignment
print(learnLang[2])
```

#### 集合

Python中集合有 两个功能：1、建立关系；2、消除重复元素

- 集合格式：`set(集合元素)`
- 交集：`集合a&集合b`
- 并集：`集合a|集合b`
- 差集：`集合a-集合b`
- 去除重复元素：`set(集合a)`，集合a中的重复元素就会去除

#### 字典

Python中的字典也叫关联数组，使用`{}`,如：`{'name':'chen','age':23,'like':'music'}`

示例：

```python
#coding=utf-8

chen={'name':'chentiefeng','age':25,'like':'music'}
print(chen['name']) #chentiefeng
```

### 标识符

标识符的命名原则：

- 第一个字符壁必须是字母或下划线
- 其他字符可是是字母、下划线或数字 
- Python关键字：and、or、global、elif、else、pass、while、for、break、continue、import、class、return等。

### 运算符

常见运算



#### 常见运算符

常见的运算符有：`+、-、*、/、**、<、>、!=、//、% 、&、|、^、~、>>、<<、<=、>=、==、not、and、or`

示例：

```python
#"+":两个对象相加
#两个数字相加
print 7+8
#两个字符串相加
print "GOOD"+" JOB!"

#"-":取一个数字的相反数或者实现两个数字相减
print -7
print 19-1

#"*":两个数相乘或者字符串重复
print 4*7
print "hello"*7

#"/":两个数字相除
print 7/2
print 7.0/2
print 7/2.0

#"**":求幂运算
print 2**3   #相当于2的3次幂，就是2*2*2

#"<"：小于符号，返回一个bool值
print 3<7

#">":大于符号，返回一个bool值
print 3>7

#"!=":不等于符号，同样返回一个bool值
print 2!=3

#"//":除法运算，然后返回其商的整数部分，舍掉余数
print 10//3

#"%":除法运算，然后返回其商的余数部分，舍掉商
print 10%3

#"&":按位与运算，所谓的按位与是指一个数字转化为二进制，然后这些二进制的数按位来进行与运算
print 7&18 ##为什么7跟18与会得到2呢？？
'''首先我们打开计算器，然后我们将7转化为二进制，得到7的二进制值是：111，自动补全为8位，即00000111
   然后我们将18转化为二进制，得到18二进制的值是10010，同样补全为8位，即00010010
   再然后，我们将00000111
   ，跟        00010010按位进行与运算，
   得到的结果是：00000010，然后，我们将00000010转化为十进制
   得到数字二，所以7跟18按位与的结果是二进制的10，即为十进制的2
'''

#"|":按位或运算，同样我们要将数字转化为二进制之后按位进行或运算
print 7|18 
'''我们来分析一下，同样我们的7的二进制形式是00000111，18的二进制形式是00010010
   我们将      00000111
   跟         00010010按位进行或运算，
   得到的结果是 00010111，然后，我们将00010111转化为十进制
   得到数字23，所以7跟18按位或的结果是二进制的10111，即为十进制的23
'''

#"^"按位异或
print 7^18
'''
   首先，异或指的是，不同则为1，相同则为0.
   我们来分析一下，同样我们的7的二进制形式是00000111，18的二进制形式是00010010
   我们将      00000111
   跟         00010010按位进行异或运算，
   得到的结果是 00010101，然后，我们将00010101转化为十进制
   得到数字21，所以7跟18按位异或的结果是二进制的10101，即为十进制的21
'''

#"~":按位翻转~x=-（x+1）
print ~18  #~18=-（18+1）=-19

#"<<":左移
print 18<<1
'''
比如18左移就是将他的二进制形式00100100左移，即移后成为00100100，即成为36，左移一个单位相当于乘2,左移动两个单位
相当于乘4，左移3个单位相当于乘8，左移n个单位相当于乘2的n次幂。
'''

#"<<"：右移
print 18>>1
'''
右移是左移的逆运算，即将对应的二进制数向右移动，右移一个单位相当于除以2,右移动两个单位相当于除以4，右移3个单位相当于
除以8，右移n个单位相当于除以2的n次幂。
'''

#"<=":小于等于符号，比较运算，小于或等于，返回一个bool值
print 3<=3

#">="
print 1>=3

#"==":比较两个对象是否相等
print 12==13
print "hello"=="hello"

#not:逻辑非
a=True
#print not a

#and:逻辑与
print True and True
'''
True and True等于True
True and False等于False
False and True等于False
'''

#or:逻辑或
print True or False
'''
True and True等于True
True and False等于True
False and False等于False
'''
```

#### 运算符优先级

1. 函数调用、寻址、下标
2. 幂运算`**`
3. 翻转运算`~`
4. 正负号
5. `*、/、%`
6. `+、-`
7. `<<、>>`
8. 按位`&、^、|`，其实这三个中也是有优先级顺序的，但是他们处于同一级别，故而不细分
9. 比较运算符
10. 逻辑的`not、and、or`
11. lambda表达式



### 判断语句

```python
#coding=utf-8

score=90
if score>=80:
    print("优秀")
elif score>=60:
    print("及格")
elif score>=30:
    print("不及格")
else:
    print("需要补课啦")
```

### 循环语句

#### for循环

```python
#coding=utf-8

#for i in 集合:   如：for i in [1,2,,5,6]:
for i in range(0,100):
#for i in range(0,100,2):
    print(i)
    print("hello {0},{1}".format(i,"chen"))
```

#### while循环

```python
#coding=utf-8

a=5
while a:
    print(a)
    a-=1
    
#或者
a=False
while a:
    print "this is False"
else:
    print(a)
```

注意：循环语句中，可以使用`continue`和`break`

### 定义函数

无参方法：

```python
#coding=utf-8
def sayHello():
    print("Hello chen")

sayHello()
```

有参方法：

```python
#coding=utf-8
def getMax(a,b):
    if a>b:
        return a
    else:
        return b

print(getMax(3,8))
```

带关键参数（带有默认值的参数）的方法：

```python
#coding=utf-8

def testFun(a,b=8):
    print(a)
    print(b)

testFun(6) #6  8
testFun(6,6) #6  6
testFun(b=6,a=5) #5   6
testFun(6,b=5) #6  5
```

返回多个值的函数：

```python
#codiing=utf-8

def testFun(a,b):
    return (a,b,8)

print(testFun(5,1)) #(5, 1, 8)

#或者
a,b,c=testFun(5,1)
print(a) #5
print(b) #1
print(c) #8
```

#### 文档字符串

文档字符串用于对函数进行说明，使用`'''函数说明'''`放置在函数内容的开始，如下：

```python
#coding=utf-8

def testFun():
    '''this is a testFun()
     return 8
    '''
    return 8

print(testFun.__doc__) ##输出文档字符串
help(testFun)   ##输出函数的帮助信息
print(testFun())#8
```



### 定义类

无构造方法：

```python
#coding=utf-8
class Person:
    def sayHello(self):
        print("hello chen")

person01=Person()
person01.sayHello()
```

有构造方法：

```python
#coding=utf-8
class Person:
    def __init__(self,name): ##构造方法
        self._name=name
        
    def sayHello(self):
        print("hello {0}".format(self._name))

person01=Person("chen")
person01.sayHello()
```

继承类：

```python
class Chinese(Person): ##集成父类Person
    def __init__(self,name):
        Person.__init__(self,name)  ##调用父类构造方法

    def sayChinese(self):
        print("你好，{0}".format(self._name))

person02=Chinese("陈铁锋")
person02.sayChinese()
```

### 引入外部文件

创建`person.py`:

```python
#coding=utf-8
class Person:
    def __init__(self,name):
        self._name=name
    def sayHello(self):
        print("hello {0}".format(self._name))
```

创建另一个文件引入`person.py`：

```python
#coding=utf-8
import person

person01=person.Person("chen")
person01.sayHello()
```

或者：

```python
#coding=utf-8
from person import Person
#from person import *  ##导入所有方法

person01=Person("chen")
person01.sayHello()
```

### 字节编译

- 字节编译：将模块编译成二进制语言程序的过程叫做字节编译，这个过程会参数一个与编译的模块对应的`.pyc`文件。
- `.pyc`文件就是经过编译后的模块对应的二进制文件。
- `import 模块名`便会编译模块为`.pyc`文件
  - 还可以使用`python -m compilrall 模块名.py`编译

### 常见数据结构

#### 栈

```python
#栈的实现
class Stack():
    def __init__(st,size):
        st.stack=[];
        st.size=size;
        st.top=-1;

    def push(st,content):
        if st.Full():
            print "Stack is Full!"
        else:
            st.stack.append(content)
            st.top=st.top+1

    def out(st):
        if st.Empty():
            print "Stack is Empty!"
        else:
            st.top=st.top-1
    def Full(st):
        if  st.top==st.size:
            return True
        else:
            return False
    def Empty(st):
        if st.top==-1:
            return True
        else:
            return False
```

#### 队列

```python
#队列的实现
class Queue():
    def __init__(qu,size):
        qu.queue=[];
        qu.size=size;
        qu.head=-1;
        qu.tail=-1;
    def Empty(qu):
        if qu.head==qu.tail:
            return True
        else:
            return False
    def Full(qu):
        if qu.tail-qu.head+1==qu.size:
            return True
        else:
            return False
    def enQueue(qu,content):
        if qu.Full():
            print "Queue is Full!"
        else:
            qu.queue.append(content)
            qu.tail=qu.tail+1
    def outQueue(qu):
        if qu.Empty():
            print "Queue is Empty!"
        else:
            qu.head=qu.head+1
```

### 文件操作



## 常用模块

模块分为主模块和非主模块：

- 主模块：如果模块的`__name__`属性是`__main__`，则该模块是主模块；
- 非主模块：

示例：

```python
#coding=utf-8

print __name__

if __name__=="__main__":
    print("运行主程序")
else:
    print("被调用运行") #import导入时会执行
```

### 系统自带函数

- `len(str)`：获取字符串长度
- `str.split(char)`：实现字符串的切割
- `help(函数名)`：查看函数的文档字符串，还可以通过`函数名.__doc__`
- `dir(模块名)`：查看模块中的属性或函数，如`dir(sys)`
- 

### 随机数

- `random.random() `：返回 [0.0, 1.0) 之间的浮点数，注意，这是一个左闭右开的区间，随机数可能会是 0 但不可能为 1 

- `random.randint(a , b)`:生成一个 a 与 b 之间的随机整数，也就是 [a, b] 

- `random.randrange()`生成的随机整数不会包含 b ，也即 [a, b) 

- `random.uniform(a, b)`生成 [a, b] 之间的随机浮点数

- `random.choice(items)`从序列类型（列表、元祖、字符串）items中随机取出一个元素，注意参数非空，否则会抛出 IndexError 

- `random.shuffle(items) `把序列 items 中的元素随机打乱

  注意：如果不想修改原来的列表，可以使用 copy 模块先拷贝一份原来的列表

  ```python
  import random
  import copy
  items=['one','two','three','four','five']
  items_copy=copy.copy(items)
  random.shuffle(items_copy)
  print(items_copy) #['three', 'four', 'two', 'one', 'five']
  ```

- ` random.sample(items, n)`从序列 items 中随机取出 n 个元素

示例：

```python
import random

print(random.random()) #0.6622050896867065
print(random.randint(1,10)) #10
print(random.randrange(1,10)) #9
print(random.uniform(1,10)) #3.501100423266598
items=['one','two','three','four','five']
print(random.choice(items)) #four
print(random.sample(items,2)) #['four', 'one']
```

#### 生成密码学安全的伪随机数

通过 `random.SystemRandom` 类的实例化对象，可以用于生成密码学安全的随机数。

`random.SystemRandom`实例化后的对象拥有与 random 类似的方法。

示例：



### pickle(序列化与反序列化)

- 序列化：`pickle.dumps(序列化对象)`或者`pickle.dump(序列化对象,file)`
- 反序列化：`pickle.loads(读取文件逐行记录)`或者`pickle.load(读取文件逐行记录,file)`

示例1：内存中 操作

```python
import pickle

#dumps
li = [11,22,33]
r = pickle.dumps(li)
print(r)


#loads
result = pickle.loads(r)
print(result)
```

示例2：文本中操作

```python
import pickle

#dump：
li = [11,22,33]
pickle.dump(li,open('db','wb'))

#load
ret = pickle.load(open('db','rb'))
print(ret)
```

### 正则表达式

引入正则表达式库有一下几种方法：

- `import re`
- `form re import *` 不推荐
- `form re import findall,search,sub,S` 不推荐 

#### 常用符号

- `.`可以匹配任意字符，换行符`\n`除外
- `\d`配置数字
- `*`匹配前一个字符0次或无限次
- `?`匹配前一个字符0次或1次
- `.*`贪心算法
- `.?`非贪心算法
- `()`括号内的数据作为结果返回

#### 常用方法

- `findall`匹配所有符号规律的内容，返回包含结果的列表
  - `re.S`可以让点匹配换行符
- `search`配置并提前第一个符合规律的内容，返回一个正则表达式对象（object）
- `sub`替换符合规律的内容，返回替换后的值

示例：

```python
#coding=utf-8

#导入re库文件
import re

secret_code='testxxIxxtestxxlovexx12345xxyouxx66'
testArr=re.findall('xx(.*?)xx',secret_code)
print(testArr) #['I', 'love', 'you']

testStr='''abcxxhello
    xxtestettestxxworldxxabc'''
print(re.findall('xx(.*?)xx',testStr)) #['testettest']
print(re.findall('xx(.*?)xx',testStr,re.S)) #['hello\n    ', 'world']

#对比findall与search的区别
s2 = 'asdfxxIxx123xxlovexxdfd'
test01 = re.search('xx(.*?)xx123xx(.*?)xx',s2)
print(test01.groups()) #('I', 'love')
f = re.search('xx(.*?)xx123xx(.*?)xx',s2).group(2) #love
print f
f2 = re.findall('xx(.*?)xx123xx(.*?)xx',s2)
print(f2) #[('I', 'love')]
print f2[0][1] #love

#sub的使用举例
s = '123rrrrr123'
output = re.sub('123(.*?)123','123%d123'%789,s)
print output ##123789123
output = re.sub('123(.*?)123','12%d12'%789,s)
print output ##1278912

#匹配数字
a = 'asdfasf1234567fasd555fas'
b = re.findall('(\d+)',a)
print b #['1234567', '555']
```

#### 爬虫测试

测试文本内容test.html：

```html
<html>
  <head>
    <title>极客学院爬虫测试</title>
  </head>
  <body>
    <div class="topic"><a href="http://jikexueyuan.com/welcome.html">欢迎参加《Python定向爬虫入门课程》</a>
      <div class="list">
        <ul>
          <li><a href="http://jikexueyuan.com/1.html">这是第一条</a></li>
          <li><a href="http://jikexueyuan.com/2.html">这是第二条</a></li>
          <li><a href="http://jikexueyuan.com/3.html">这是第三条</a></li>
        </ul>
      </div>
    </div>
  </body>
</html>
```

爬虫测试代码：

```python
#coding=utf-8

#导入re库文件
import re

old_url = 'http://www.test.com/htmls/id=11111?pageNum=2'
total_page = 20

f = open('test.html','r')
html = f.read()
f.close()

#爬取标题
title = re.search('<title>(.*?)</title>',html,re.S).group(1)
print title

#爬取链接
links = re.findall('href="(.*?)"',html,re.S)
for each in links:
    print each

#抓取部分文字,先大再小
text_fied = re.findall('<ul>(.*?)</ul>',html,re.S)[0]
the_text = re.findall('">(.*?)</a>',text_fied,re.S)
for every_text in the_text:
    print every_text

#sub实现翻页
for i in range(2,total_page+1):
    new_link = re.sub('pageNum=\d+','pageNum=%d'%i,old_url,re.S)
    print new_link
```

实战下载图片：

```python
#coding=utf-8
import re
##安装requests: pip install requests 或者 pip install requests -U
import requests

testURL='http://www.testclass.net/'
htmlStr=requests.get(testURL)
#匹配图片网址
pic_urls = re.findall('img class="card-img-top" src="(.*?)"',htmlStr.text,re.S)
i = 0
for item in pic_urls:
    print( 'now downloading:' + item)
    pic = requests.get(item)
    fp = open('pic\\' + str(i) + '.gif','wb')
    fp.write(pic.content)
    fp.close()
    i += 1
```

# Python读取配置文件

参考： https://mp.weixin.qq.com/s?__biz=MzU1OTI0NjI1NQ==&mid=2247486592&idx=1&sn=78718ab892a8e22b8ac334ed773f450d&chksm=fc1b7240cb6cfb56884827ab62f2c403fb733a24dd25d1dae2d16dd6c18e259389a838c94f15&scene=126&sessionid=1602035721&key=1394210e71e96a226a13d2cb12844a0033fec0b8986d767289fe9d44481b74c515cd88fcdc51a348def4f06b2563d95640d749ea60be73bf9edddbef4ab9ffcbec2b8daa2a07d5d1c375d219bad20b98c01dd12bf3cdd8cb90d51cf50b42bd374de767b8578cfb13e25df0a98010837312fa5c7aba0a9460aab894f2bc81780f&ascene=1&uin=MTQxMzIwMDI4Mg%3D%3D&devicetype=Windows+10+x64&version=62090538&lang=zh_CN&exportkey=AYSjogrvWAR1PLp0Zoz4fgg%3D&pass_ticket=ogGQgS6L2jMXeiqZKeAnUzATDV%2BPmfHAs6dQa94MPXFNPm2PRJ%2FKBKKXrTXpKYI8&wx_header=0 

​		在实际项目中，经常会接触到各种各样的配置文件，它可以增强项目的可维护性，常用配件文件的处理方式，包含：JSON、ini / config、YAML、XML 等。

## JSON

Python 内置了 JSON 模块，可以非常方便操作 JSON 数据

常见的 4 个方法分别是：

- json.load(json_file)

  解析 JSON 文件，转换为 Python 中对应的数据类型

- json.loads(json_string)

  解析 JSON 格式的字符串，结果为 Python 中的字典

- json.dump(python_content,file_path)

  将 Python 数据，包含：dict、list 写入到文件中

- json.dumps(python_dict)

  将 Python 中 dict 转为 JSON 格式的字符串

以下面这段 JSON 配置文件为例：

```json
#config.json
{
  "mysql": {
    "host": "198.0.0.1",
    "port": 3306,
    "db": "xh",
    "username": "root",
    "password": "123456",
    "desc": "Mysql配置文件"
  }
}
```

### 1、读取配置文件

读取配置文件有两种方式，分别是：

使用 json.load() 直接读取配置文件

或者，先读取配置文件中的内容，然后使用 json.loads() 转换为 Python 数据类型

需要指出的是，面对复杂层级的 JSON 配置文件，可以利用 jsonpath 进行读取；jsonpath 类似于 xpath，可以通过正则表达式快速读取数据

```python
import json

def read_json_file(file_path):
    """
    读取json文件
    :param file_path:
    :return:
    """
    with open(file_path, 'r', encoding='utf-8') as file:

        # 读取方式二选一
        # 方式一
        result = json.load(file)

        # 方式二
        # result = json.loads(file.read())

        # 解析数据
        host_mysql = result['mysql']['host']
        port_mysql = result['mysql']['port']
        db = result['mysql']['db']

        print('Mysql地址：', host_mysql, ",端口号:", port_mysql, ",数据库:", db)

    return result
```

### 2、保存配置文件

使用 json 中的 json.dump() 方法，可以将一个字典写入到 JSON 文件中

```python
def write_content_to_json_file(output_file, content):
    """
    写入到json文件中
    :param output_file:
    :param content:
    :return:
    """
    with open(output_file, 'w') as file:
        # 写入到文件中
        # 注意：为了保证中文能正常显示，需要设置ensure_ascii=False
        json.dump(content, file, ensure_ascii=False)

content_dict = {
    'mysql': {
        'host': '127.0.0.1',
        'port': 3306,
        'db': 'xh',
        'username': 'admin',
        'password': '123456',
        'desc': 'Mysql数据库'
    }
}

write_content_to_json_file('./output.json', content_dict)
```

### 3、修改配置文件

如果需要修改配置文件，只需要先从配置文件中读出内容，然后修改内容，最后将修改后的内容保存的配置文件中即可

```python
def modify_json_file():
    """
    修改json配置文件
    :return:
    """
    result = read_json_file('./config.json')

    # 修改
    result['mysql']['host'] = '198.0.0.1'

    write_content_to_json_file('./config.json', result)
```

## ini/config

ini 配置文件和 config 配置文件的解析方式类似，仅仅是文件后缀不一致

这里我们以 ini 配置文件为例：

``` ini
# config.ini
[mysql]
host = 139.199.1.1
username = root
password = 123456
port = 3306
```

ini 文件由 3 部分组成，分别是：节点（Section）、键（Key）、值（Value）

常见的 Python 处理 ini 文件有两种方式，包含：

- 使用内置的 configparser 标准模块
- 使用 configobj 第三方依赖库

我们先来看看内置的 configparser 模块

###  1.1、读取配置文件

 实例化一个 ConfigParser 解析对象，使用 read() 方法读取 ini 配置文件 

``` python
from configparser import ConfigParser

# 实例化解析对象
cfg = ConfigParser()

# 读取ini文件内容
cfg.read(file_path)
```

 使用 sections() 函数，可以获取所有的节点列表 

``` python
# sections() 得到所有的section，并以列表的形式返回
sections = cfg.sections()
print(sections)
```

 要获取某一个节点下的所有键，可以使用 options(section_name) 函数 

``` python
# 获取某一个区域的所有key
# cfg.options(section_name)
keys = cfg.options('mysql')
print(keys)
```

 通过 items(section_name) 函数，可以获取某一个节点下的所有键值对 

``` python
# 获取某一个区域下的键值对
items = cfg.items("mysql")
print(items)
```

 如果要获取某一个节点下，某一个键下的值，使用 get(section_name,key_name) 函数即可 

``` python
# 读取某一个区域下的某一个键值
host = cfg.get("mysql", "host")
print(host)
```

### 1.2、写入配置文件

和读取配置文件类似，需要先实例化一个 ConfigParser 解析对象

首先，使用 add_section(section_name) 函数添加一个节点

```python
# 加入节点和键值对
# 添加一个节点
cfg.add_section("redis")
```

然后，就可以使用 set(section_name,key,value) 函数往某一个节点添加键值对

```python
# 往节点内，添加键值对
cfg.set("redis", "host", "127.0.0.1")
cfg.set("redis", "port", "12345")
```

最后，使用 write() 函数写入到配置文件中去

```python
# 写入到文件中
cfg.write(open('./raw/output.ini', 'w'))
```

### 1.3、修改配置文件

修改配置文件的步骤是，读取配置文件，然后通过 set(section_name,key,value) 进行修改操作，最后使用 write() 函数写入到文件中即可

```python
def modify_ini_file(file_path):
    """
    修改ini文件
    :return:
    """
    cfg.read(file_path)

    cfg.set("mysql", "host", "139.199.11.11")

    # 写入
    cfg.write(open(file_path, "w"))
```

接着，我们聊聊使用 configobj 操作 ini 配置文件的流程

首先安装 configobj 依赖库

```shell
# 依赖# pip3 install configobj
```

### 2.1、读取配置文件

 直接将 ini 配置文件路径作为参数，使用 ConfigObj 类构造一个对象 

``` python
from configobj import ConfigObj

# 实例化对象
config = ConfigObj(file_path, encoding='UTF8')
```

 查看源码可以发现，ConfigObj 是 Section 节点的子类，而 Section 是 Dict 字典的子类 , 所以，可以直接通过键名 Key 获取节点和键值 。

``` python
# <class 'configobj.ConfigObj'>
print(type(config))

# <class 'configobj.Section'>
print(type(config['mysql']))

# 节点
print(config['mysql'])

# 某一个键对应的值
print(config['mysql']
```

### 2.2、修改配置文件

只需要读取配置文件，然后直接修改 ConfigObj 对象，最后使用 write() 方法，即可以达到修改配置文件的目的

``` python
def modify_ini_file(file_path):
    """
    修改ini文件
    :param file_path:
    :return:
    """
    # 读取配置文件
    config = read_ini_file(file_path)

    # 直接修改
    config['mysql']['host'] = '139.199.1.1'

    # 删除某个键值对
    try:
        del config['mysql']['db']
    except Exception as e:
        print('键不存在')
        pass

    # 写入
    config.write()
```

### 2.3、 写入配置文件 

写入配置文件，首先需要实例化一个 ConfigObj 对象，传入文件路径

然后，设置节点、针对节点设置键值对

最后，调用 write() 方法，写入到配置文件中

``` python
def write_to_ini_file(output):
    """
    写入到ini文件中
    :param output:
    :return:
    """
    config = ConfigObj(output, encoding='UTF8')
    config['website'] = {}
    config['website']['url'] = "www.baidu.com"
    config['website']['name'] = "百度"

    # 保存
    config.write()
```

## YAML

Python 操作 YAML 文件，常见的 2 种方式分别是：pyyaml、ruamel.yaml

使用 pip 安装依赖

``` shell
# 安装依赖
# 方式一
pip3 install pyyaml

# 方式二
pip3 install ruamel.yaml
```

 下面以一个简单的 YAML 配置文件为例，通过两种方式进行说明 

``` yaml
# 水果
Fruits:
  # 苹果
  - Apple:
      name: apple
      price:  1
      address:  广东
  # 桔子
  - Orange:
      name: orange
      price:  3
      address:  湖南
  # 香蕉
  - Banana:
      name: banana
      price:  2
      address:  海南
```

 我们先来看看 pyyaml 

### 1.1、读取配置文件 

 首先，读取配置文件，使用 yaml.safe_load() 加载数据，获取的数据类型是字典 

``` python
import yaml

with open(file_path, "r") as file:
    data = file.read()

    # safe_load() 读取配置文件
    # 结果数据类型：dict
    result = yaml.safe_load(data)

    print(result)
```

 接着，就可以通过 YAML 配置文件的层级关系，获取键值 

``` python
# 3、获取yaml中的值
name = result['Fruits'][0]['Apple']['name']
price = result['Fruits'][0]['Apple']['price']
address = result['Fruits'][0]['Apple']['address']
print("名称:", name, ",price:", price, ",address:", address)
```

### 1.2、 写入配置文件 

使用 YAML 中的 dump() 方法，可以将一个字典写入到 YAML 配置文件中

需要注意的是，为了保证中文写入能正常显示，需要配置 allow_unicode=True

``` python
def write_to_yaml_file(content, file_path):
    """
    写入到yaml文件中
    :param content:
    :param file_path:
    :return:
    """

    # 写入到文件中
    with open(file_path, 'w', encoding='utf-8') as file:
        yaml.dump(content, file, default_flow_style=False, encoding='utf-8', allow_unicode=True)

# 定义一个字典
content = {
   "websites": [{"baidu": {'url': "www.baidu.com", 'name': '百度', "price": 100}},{"alibaba": {'url': "www.taobao.com", 'name': '淘宝', "price": 200}},{"tencent": {'url': "www.tencent.com", 'name': '腾讯', "price": 300}},]
}

write_to_yaml_file(content, "./raw/new.yaml")
```

### 1.3、 修改配置文件 

 和修改 ini 文件类型，先读取配置文件，然后修改字典中的内容，最后使用上面的写入方法，即可以达到修改配置文件的目的 

``` python
def modify_yaml_file():
    """
    修改yaml文件
    :return:
    """
    content = read_yaml_file('./raw/norm.yaml')
    print(content)

    # 修改dict
    content['Fruits'][0]['Apple']['price'] = 10086

    # 重新写入到一个新的yaml文件中
    write_to_yaml_file(content, './raw/output.yaml')
```

接着，我们来聊聊使用 ruamel 操作 YAML 配置文件的流程

ruamel 是 pyyaml 的衍生版本，在传统 pyyaml 的基础上，增加了 RoundTrip 模式，保证 YAML 配置文件的读写顺序一致

所以，在读取、修改、写入方式上和 pyyaml 类似

### 2.1、读取配置文件

``` python
from ruamel import yaml

def read_yaml_file(file_path):
    """
    读取yaml文件
    :param file_path:
    :return:
    """
    with open(file_path, 'r', encoding='utf-8') as file:
        data = file.read()

        # 解析yaml文件
        # 类型：ordereddict
        result = yaml.load(data, Loader=yaml.RoundTripLoader)

        name = result['Fruits'][0]['Apple']['name']
        price = result['Fruits'][0]['Apple']['price']
        address = result['Fruits'][0]['Apple']['address']
        print("名称:", name, ",price:", price, ",address:", address)

    return result
```

###  2.2、写入配置文件 

``` python
def write_to_yaml_file(filepath, data):
    """
    写入到yaml文件中
    :param filepath:
    :param data:
    :return:
    """
    with open(filepath, 'w', encoding='utf-8') as file:
        yaml.dump(data, file, Dumper=yaml.RoundTripDumper, allow_unicode=True)
```

### 2.3、 修改配置文件 

``` python
def modify_yaml_file():
    """
    修改yaml文件
    :return:
    """
    content = read_yaml_file('./raw/norm.yaml')

    print(content)

    # 修改dict
    content['Fruits'][0]['Apple']['price'] = 10086

    # 重新写入到一个新的yaml文件中
    write_to_yaml_file('./raw/output.yaml', content)
```

## XML

XML 作为一种标记语言，被用来设计存储和传输数据，很多项目经常使用 XML 作为配置文件和数据传输类型

Python 内置的 xml 模块 可以很方便地处理 XML 配置文件

以下面这段配置文件为例：

``` xml
<?xml version="1.0" encoding="utf-8"?>
<dbconfig>
    <mysql>
        <host>127.0.0.1</host>
        <port>3306</port>
        <dbname>test</dbname>
        <username>root</username>
        <password>4355</password>
    </mysql>
</dbconfig>
```

 首先，使用 xml.dom.minidom.parser(file_path) 解析配置文件，利用 documentElement 属性获取 XML 根节点 

``` python
import xml.dom.minidom

# 读取配置文件
dom = xml.dom.minidom.parse("./raw.xml")

# 利用 documentElement 属性获取 XML 根节点
# 根节点
root = dom.documentElement
```

 接着，使用 getElementsByTagName(tag_name) 方法，获取某一节点 

``` python
# 获取mysql节点
node_mysql = root.getElementsByTagName('mysql')[0]
```

 最后，使用 childNodes 属性，遍历节点的子 Node 节点，获取节点的名称和值 

``` python
# 遍历子节点，获取名称和值
for node in node_mysql.childNodes:
    # 节点类型
    # 1：Element
    # 2：Attribute
    # 3：Text
    # print(node.nodeType)
    if node.nodeType == 1:
        print(node.nodeName, node.firstChild.data)
```

# Python数据处理

[聊聊 Python 数据处理全家桶（Mysql 篇）](http://mp.weixin.qq.com/s?__biz=MzU1OTI0NjI1NQ==&mid=2247486468&idx=2&sn=7c8cdfb478e5801496dad8b0ddf2061e&chksm=fc1b72c4cb6cfbd2e71861ddb8aa5d6c2ba3d4266ede047cfaf0558e6cd4e5b05a9ecca4dcdc&scene=21#wechat_redirect)

[聊聊 Python 数据处理全家桶（Sqlite 篇）](http://mp.weixin.qq.com/s?__biz=MzU1OTI0NjI1NQ==&mid=2247486487&idx=1&sn=78b344b45366c4b8ffd195b2ce300191&chksm=fc1b72d7cb6cfbc106f1a820db3d0f7ee07f22f0a7af0525952fb82a9ed87ef00675a2c18b3a&scene=21#wechat_redirect)

[聊聊 Python 数据处理全家桶（Redis 篇）](http://mp.weixin.qq.com/s?__biz=MzU1OTI0NjI1NQ==&mid=2247486517&idx=1&sn=01d43b1cd83c7caef94f06614fd42c02&chksm=fc1b72f5cb6cfbe39cef5a63b64c332c6b9cd2e329cc4373a72d460ca84d44e7637d5c6ae06b&scene=21#wechat_redirect)

[聊聊 Python 数据处理全家桶（Memc 篇）](http://mp.weixin.qq.com/s?__biz=MzU1OTI0NjI1NQ==&mid=2247486553&idx=1&sn=06c97f43e68be307a6826e74ec126f29&chksm=fc1b7299cb6cfb8f8a6e99598327c240f9147dffeb79627d8848d465727ae8c3081e818b204d&scene=21#wechat_redirect)

[聊聊 Python 数据处理全家桶（Mongo 篇）](http://mp.weixin.qq.com/s?__biz=MzU1OTI0NjI1NQ==&mid=2247486582&idx=2&sn=a1ec46619e543046994823d82df48dd7&chksm=fc1b72b6cb6cfba0a875a4931f4ce482307725076c285c6a3210a0e75b8082041ea17ae7abab&scene=21#wechat_redirect)

# 爬虫

## requests的介绍与安装

`requests`是Python的HTTP库，完美替换了Python的`urllib2`模块。

`requests`的安装：`pip install requests 或者 pip install requests -U`

## requests使用示例

### 获取网页的源代码

核心：`requests.get`

```python
#coding=utf-8
import requests

testURL='http://www.testclass.net/'
htmlStr=requests.get(testURL)
print(htmlStr.text)

headers={'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.75 Safari/537.36'}
##带headers的请求
htmlStr2=requests.get(testURL,headers=headers)
htmlStr2.encoding='utf-8'
print(htmlStr2.text)
```

### `requests`表单提交

核心：`requests.post`

```python
#coding=utf-8
import requests
import re
import random

testURL='http://www.quanshuwang.com/register.php?do=submit'

num=random.randint(1,100000)
formData={
    'email':'test{0}@test.com'.format(num),
    'username':'test{0}'.format(num),
    'password':'123456',
    'repassword':'123456',
    'action':'newuser'
}
responseData=requests.post(testURL,data=formData)
responseData.encoding='gbk'
responseInfo=re.findall('<div class="blocktitle">(.*?)</div>',responseData.text,re.S)
print(responseInfo) #['注册成功']
```





## 单线程爬虫

```python
#coding=utf-8
import requests
import re

testURL='http://www.testclass.net/'
htmlStr=requests.get(testURL)
titles=re.findall('<a class="text-dark" href=".*?">(.*?)</a>',htmlStr.text,re.S)
createTimes=re.findall('<small>(.*?)</small>',htmlStr.text,re.S)
for index in range(len(titles)):
    print(titles[index]+'({0})'.format(createTimes[index]))
```

### 实战：爬取

```python
#-*_coding:utf8-*-
import requests
import re
import sys
reload(sys)
sys.setdefaultencoding("utf-8")

class spider(object):
    def __init__(self):
        print u'开始爬取内容。。。'

#getsource用来获取网页源代码
    def getsource(self,url):
        html = requests.get(url)
        return html.text

#changepage用来生产不同页数的链接
    def changepage(self,url,total_page):
        now_page = int(re.search('pageNum=(\d+)',url,re.S).group(1))
        page_group = []
        for i in range(now_page,total_page+1):
            link = re.sub('pageNum=\d+','pageNum=%s'%i,url,re.S)
            page_group.append(link)
        return page_group
#geteveryclass用来抓取每个课程块的信息
    def geteveryclass(self,source):
        everyclass = re.findall('(<li deg="".*?</li>)',source,re.S)
        return everyclass
#getinfo用来从每个课程块中提取出我们需要的信息
    def getinfo(self,eachclass):
        info = {}
        info['title'] = re.search('target="_blank">(.*?)</a>',eachclass,re.S).group(1)
        info['content'] = re.search('</h2><p>(.*?)</p>',eachclass,re.S).group(1)
        timeandlevel = re.findall('<em>(.*?)</em>',eachclass,re.S)
        info['classtime'] = timeandlevel[0]
        info['classlevel'] = timeandlevel[1]
        info['learnnum'] = re.search('"learn-number">(.*?)</em>',eachclass,re.S).group(1)
        return info
#saveinfo用来保存结果到info.txt文件中
    def saveinfo(self,classinfo):
        f = open('info.txt','a')
        for each in classinfo:
            f.writelines('title:' + each['title'] + '\n')
            f.writelines('content:' + each['content'] + '\n')
            f.writelines('classtime:' + each['classtime'] + '\n')
            f.writelines('classlevel:' + each['classlevel'] + '\n')
            f.writelines('learnnum:' + each['learnnum'] +'\n\n')
        f.close()

if __name__ == '__main__':

    classinfo = []
    url = 'http://www.jikexueyuan.com/course/?pageNum=1'
    jikespider = spider()
    all_links = jikespider.changepage(url,20)
    for link in all_links:
        print u'正在处理页面：' + link
        html = jikespider.getsource(link)
        everyclass = jikespider.geteveryclass(html)
        for each in everyclass:
            info = jikespider.getinfo(each)
            classinfo.append(info)
    jikespider.saveinfo(classinfo)
```









# web开发

### web2py框架

官网：http://www.web2py.com/

启动：`python ./web2py.py`

- 所有应用都存放在`applications`路径下。

- 应用目录下的`static`路径存放静态资源（`html、css、js、images`）

  - 静态资源可以通过`ip:prot/应用名/static/资源名`访问。

- `controllers`：控制器存放路径

  - `default.py`：是默认处理Controller，用于首页

  - py文件的`index()`方法是默认处理路径的方法

  - 测试：创建一个`hello.py`文件

    ```python
    #coding=utf-8
    
    def index():
        return "hello chen"
    ```

    - 可以通过：`ip:prot/应用名/hello`访问到，得到结果是“hello chen”。

    - 可以通过`ip:prot/应用名/hello/方法名`访问到特定的方法。






































































