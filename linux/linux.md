### -bash: make: command not found的解决办法

Centos中无法使用make，make install，命令 make: command not found

一般出现这个-bash: make: command not found提示，

是因为安装系统的时候使用的是最小化mini安装，

系统没有安装make、vim等常用命令，直接yum安装下即可。

安装：

```
yum -y install gcc automake autoconf libtool make
```

安装g++:

```
yum -y install gcc gcc-c++
```



### bash: wget: command not found

```
yum -y install wget
```



