git地址：https://github.com/ehang-io/nps



## 服务端

```
docker run -it -d --name nps -p 58080:8080 -p 50080:80 -p 58024:8024 centos:centos7 /bin/bash
docker attach nps
```

1、按照wget

```
yum install -y wget
```

2、下载nps

```
wget https://github.com/ehang-io/nps/releases/download/v0.26.9/linux_amd64_server.tar.gz
```

解压：

```
tar -zxvf linux_amd64_server.tar.gz
```

3、执行安装命令

```
./nps install
```

4、启动

```
./nps start
```

5、访问

```
http://49.233.207.213:58080/
```

6、修改配置文件

```
cd /etc/nps
cd conf
vi nps.conf
```

7、重启

```
./nps restart
```





## 客户端

安装ifconfig工具

```
yum install -y net-tools
```

```
ip addr
```



```
yum install -y wget
```



```
wget https://github.com/ehang-io/nps/releases/download/v0.26.9/linux_amd64_client.tar.gz
```

```
mkdir npc
mv linux_amd64_client.tar.gz npc/
cd npc/
```



```
tar -zxvf linux_amd64_client.tar.gz
```

```
(./npc -server=49.233.207.213:58024 -vkey=6dtl1iv8lrk1ijo1 -type=tcp &)
```



```

```

