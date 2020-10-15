## Centos安装JDK

1. 下载

   ``` shell
   wget http://dl.mycat.org.cn/jdk-8u20-linux-x64.tar.gz
   ```

2. 解压

   ``` shell
   tar -zxvf jdk-8u20-linux-x64.tar.gz
   ```

3. 移动

   ```shell
   mv jdk1.8.0_20 /usr/local/java
   ```

4. 配置环境变量

   ``` shell
    vi /etc/profile
   ```

   添加如下内容：

   ```shell
   export JAVA_HOME=/usr/local/java
   export PATH=$PATH:$JAVA_HOME/bin
   ```

5. 使配置文件生效

   ```shell
   source /etc/profile
   ```

6. 测试

   ```
   java -version
   ```

   