

## Mycat安装

### 准备

```
yum install -y wget
```

### 安装JDK

参考：[Centos安装JDK](https://github.com/ichentiefeng/learning/blob/master/java/Centos7%E5%AE%89%E8%A3%85JDK8.md)

### 安装Postgresql

参考：[Docker环境安装Postgresql](https://github.com/ichentiefeng/learning/blob/master/postgresql/Postgresql.md#docker%E7%8E%AF%E5%A2%83%E5%AE%89%E8%A3%85)

我这里启动了4个容器，作为两主两从。

1. 拉取镜像

   ```
   docker pull postgres
   ```

2. 创建数据文件夹

   ```
   mkdir -p data/pgdata1 data/pgdata2 data/pgdata3 data/pgdata4
   ```

3. 启动4个postgresql容器

   ```shell
   docker run -d --name postgres -e POSTGRES_PASSWORD=123456 -p 55431:5432  -v /home/chen/data/pgdata1:/var/lib/postgresql/data postgres
   docker run -d --name postgres2 -e POSTGRES_PASSWORD=123456 -p 55432:5432  -v /home/chen/data/pgdata2:/var/lib/postgresql/data postgres
   docker run -d --name postgres3 -e POSTGRES_PASSWORD=123456 -p 55433:5432  -v /home/chen/data/pgdata3:/var/lib/postgresql/data postgres
   docker run -d --name postgres4 -e POSTGRES_PASSWORD=123456 -p 55434:5432  -v /home/chen/data/pgdata4:/var/lib/postgresql/data postgres
   ```

4. 通过`docker ps`已经可以看到我们启动的postgres的容器了，这时已经可以连接这4个postgres数据库了。

### 安装Mycat

1. 下载

   ```shell
   wget http://dl.mycat.org.cn/1.6.7.5/2020-4-10/Mycat-server-1.6.7.5-release-20200410174409-linux.tar.gz
   ```

2. 解压

   ```
   tar -zxvf Mycat-server-1.6.7.5-release-20200410174409-linux.tar.gz
   ```

3. 配置环境变量

   ```shell
   vi .bash_profile
   ```

   添加如下内容：

   ```shell
   export MYCAT_HOME=/home/chen/soft/mycat
   export PATH=$PATH:$MYCAT_HOME/bin
   ```

   使配置生效：

   ```shell
    source .bash_profile
   ```

4. 启动

   ```
   mycat start
   ```

5. 可以通过日志查看启动情况：

   ```shell
   cd $MYCAT_HOME/logs
   ```

   可以看到：

   - mycat.log：Mycat的运行期间日志
   - mycat.pid：Mycat进程Id
   - switch.log：Mycat与数据库交互的日志
   - wrapper.log：Mycat的启动日志。如果Mycat启动失败，可以查看这个文件。

6. 

### 安装Mycat-web

Mycat-web需要依赖Zookeeper，所以我们先安装Zookeeper。



























































