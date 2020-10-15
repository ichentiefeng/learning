# Mysql安装

## Docker环境安装

1. 拉取官方镜像（我们这里选择5.7，如果不写后面的版本号则会自动拉取最新版）

   ```shell
   docker pull mysql:5.7   # 拉取 mysql 5.7
   docker pull mysql       # 拉取最新版mysql镜像
   ```

   [MySQL文档地址](https://hub.docker.com/_/mysql/)

2. 检查是否拉取成功

   ```
   sudo docker images
   ```

3. 一般来说数据库容器不需要建立目录映射

   ```shell
   docker run -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7
   ```

   - –name：容器名，此处命名为`mysql`
   - -e：配置信息，此处配置mysql的root用户的登陆密码
   - -p：端口映射，此处映射 主机3306端口 到 容器的3306端口

4. 如果要建立目录映射

   ```shell
    docker run -p 3306:3306 --name mysql \
   -v /usr/local/docker/mysql/conf:/etc/mysql \
   -v /usr/local/docker/mysql/logs:/var/log/mysql \
   -v /usr/local/docker/mysql/data:/var/lib/mysql \
   -e MYSQL_ROOT_PASSWORD=123456 \
   -d mysql:5.7
   ```

   - -v：主机和容器的目录映射关系，":"前为主机目录，之后为容器目录

5. 检查容器是否正确运行

   ```shell
   docker container ls
   ```

   - 可以看到容器ID，容器的源镜像，启动命令，创建时间，状态，端口映射信息，容器名字

6. 进入docker本地连接mysql客户端

   ```shell
   docker exec -it mysql bash
   mysql -uroot -p123456
   ```

7. 测试

   ```mysql
   mysql> show databases;
   ```

   