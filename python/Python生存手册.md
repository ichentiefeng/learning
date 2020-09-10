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






































































