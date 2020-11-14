

### Python中的数学操作符

| 操作符 | 操作          | 示例          |
| ------ | ------------- | ------------- |
| **     | 指数          | 2 ** 3   = 8  |
| %      | 取余数        | 22 % 8 = 6    |
| //     | 整除/商取整数 | 22 //8 = 2    |
| /      | 除法          | 22 / 8 = 2.75 |
| *      | 乘法          | 2 * 3 = 6     |
| -      | 减法          | 2 - 1 = 1     |
| +      | 加法          | 1 + 1 = 2     |

### 字符串操作

| 操作符      | 操作           | 示例                              | 结果               |
| ----------- | -------------- | --------------------------------- | ------------------ |
| +           | 字符串连接     | 'i am' + ' chentiefeng'           | i am chentiefeng   |
| *           | 字符串复制     | 'love ' * 2                       | love love          |
| len(str)    | 字符串长度     | len('chentiefeng')                | 11                 |
| str(number) | 数字转字符串   | 'I am ' + str(25) + ' years old.' | I am 25 years old. |
| int(str)    | 字符串转整数   | int('25')                         | 25                 |
| float(str)  | 字符串转浮点数 | float('3.1415')                   | 3.1415             |
|             |                |                                   |                    |

### 交互式环境

- 输出

  ```python
  print('What is your name ?')
  ```

- 输入

  ``` python
  myName = input()
  ```



### 布尔值

- `True`
- `False`

### 布尔操作符

- and

  ``` python
  True and True
  ```

- or

  ``` python
  True or False
  ```

- not

  ```python
  not True
  ```

- 布尔加比较操作符

  ```python
  (4 > 5) and (6 > 5)
  ```

注意：Python 先求值 not 操作符，然后是 and 操作符，然后是 or 操作符。

### 条件表达式

```python
print('Please input your username:')
username = input()

print('Please input your password:')
password = input()

if password == '123456':
    print('Access granted')
elif username == 'admin':
    print('Admin access granted')
else:
    print('Wrong password')
```

### while循环语句

``` python
count = 3
while count >0:
    print('呵呵')
    count = count - 1
```

或者：

```python
name = ''
while name != 'admin':
    print('Please input your name:')
    name = input()
    if name != 'root':
        continue
    print('Please input your password:')
    password = input()
    if password == '123456':
        break
print('Welcome ' + name)
```

### for循环

```python
count = 3
for i in range(count):
    print('呵呵')
```

- `i `分别被设置为 0、1、2，变量` i` 将递增到（但不包括） 传递给 `range()`函数的整数。

### `range()`函数

格式：

``` python
range(start,end,step)
```

- strat：代表开始值
- end：代表上限值，但不包含它
- step：代表步长。步长是每次迭代后循环变量增加的值。

示例：

```python
for i in range(1, 10, 2):
    print(i)
```

结果是：

``` te
1
3
5
7
9
```









































































