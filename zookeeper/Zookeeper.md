## 安装

### 离线安装

1. 下载

   ```shell
   wget http://dl.mycat.org.cn/zookeeper-3.4.6.tar.gz
   ```

2. 解压

   ```shell
   tar -zxvf zookeeper-3.4.6.tar.gz
   ```

3. 移动

   ```shell
   mv zookeeper-3.4.6 /usr/local/zookeeper
   ```

4. 配置环境变量

   ```shell
    vi /etc/profile
   ```

   添加如下内容：

   ```shell
   export ZOOKEEPER_HOME=/usr/local/zookeeper
   export PATH=$PATH:$ZOOKEEPER_HOME/bin
   ```

   使配置文件生效

   ```shell
   source /etc/profile
   ```

5. 配置

   ```shell
   cd $ZOOKEEPER_HOME/conf
   cp zoo_sample.cfg zoo.cfg
   ```

   通过修改`zoo.cfg`可以改变Zookeeper的相关配置。

6. 启动

   ```shell
   zkServer.sh start
   ```

   查看启动状态：

   ```shell
   zkServer.sh statusc
   ```

7. 测试

   ```
   zkCli.sh
   ```

   