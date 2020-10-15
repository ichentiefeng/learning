## 安装

### Docker环境安装

1. 下载postgresql镜像

   ```shell
   docker pull postgres
   ```

2. 创建数据文件夹

   ```shell
   mkdir -p /opt/data/pgdata
   ```

3. 创建postgres容器

   ```shell
   docker run -d --name postgres -e POSTGRES_PASSWORD=123456 -p 5432:5432  -v /opt/data/pgdata:/var/lib/postgresql/data postgres
   ```

4. 进入postgres容器

   ```shell
   docker exec -it postgres /bin/bash
   ```

5. 测试

   ```
   su - postgres
   psql
   ```

   查看所有库：

   ```
   postgres=# \l
   ```

   进入postgres数据库：

   ```
   postgres=# \c postgres
   ```

   查看所有表：

   ```
   postgres=# \dt
   ```

   































