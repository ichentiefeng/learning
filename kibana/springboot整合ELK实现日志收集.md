# **1.什么是ELK？**

ELK 就是Elasticsearch，logstash，kibana的缩写哈哈。

> - Elasticsearch 是一个搜索和分析引擎。
> - Logstash 是服务器端数据处理管道，能够同时从多个来源采集数据，转换数据，然后将数据发送到诸如 Elasticsearch 等“存储库”中。
> - Kibana 则可以让用户在 Elasticsearch 中使用图形和图表对数据进行可视化。

ELK该怎么实现日志收集呢？
logstash就相当于一个管道，日志通过这个管道传输给Elasticsearch来保存，最后在kibana上通过图表的形式就可以看见日志信息啦！

# **2.需要准备的环境**

这里是使用的Docker来创建elasticsearch,logstash,kibana；Docker compose来启动的（也可以用Docker运行，看个人选择），所以需要安装Docker和Docker-Compose环境

### 2.1Docker环境安装

**1. 安装yum-utils：**

```
yum install -y yum-utils device-mapper-persistent-data lvm2
```

**2. 为yum源添加docker仓库位置：**

```
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

**3. 安装docker：**

```
yum install docker-ce
```

**4. 启动docker：**

```
systemctl start docker
```

### 2.2Docker Compose环境安装

> Docker Compose是一个用于定义和运行多个docker容器应用的工具。使用Compose你可以用YAML文件来配置你的应用服务，然后使用一个命令，你就可以部署你配置的所有服务了。
> **使用Docker Compose的步骤**：
>
> - 使用Dockerfile定义应用程序环境，一般需要修改初始镜像行为时才需要使用；
> - 使用docker-compose.yml定义需要部署的应用程序服务，以便执行脚本一次性部署；
> - 使用docker-compose up命令将所有应用服务一次性部署起来。

**1. 安装下载Docker Compose**

```
curl -L https://get.daocloud.io/docker/compose/releases/download/1.24.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```

**2. 修改该文件的权限为可执行**

```
chmod +x /usr/local/bin/docker-compose
```

**3. 查看是否已经安装成功**

```
docker-compose --version
```

![img](https://mmbiz.qpic.cn/mmbiz_png/3kMZppQa5zD0yjqM7iaxGM5BF5uiaDOwbMXFF6MWU70dTBiaXI5MXu57W0WfMUrM0EqFQf1JQp6KnF9rzuU0SibQoQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 2.3获取Elasticsearch，logstash，kibana的镜像

```
docker pull elasticsearch:6.4.0
docker pull logstash:6.4.0
docker pull kibana:6.4.0
```

### 2.4部署前准备

##### Elasticsearch配置

需要设置系统内核参数，否则会因为内存不足无法启动；

- 改变设置

```
sysctl -w vm.max_map_count=262144
```

- 使之立即生效

```
sysctl -p
```

需要创建/mydata/elasticsearch/data目录并设置权限，否则会因为无权限访问而启动失败。

- 创建目录

```
mkdir -p ~/mydata/elasticsearch/data/
```

- 创建并改变该目录权限

```
chmod 777 ~/mydata/elasticsearch/data
```

##### Logstash配置

创建配置文件存放目录logstash

```
mkdir -p mydata/logstash
```

- 创建一个存放logstash配置的文件**logstash-springboot.conf**

  ```
  cd mydata/logstash
  vi logstash-springboot.conf
  ```

  文件内容：

```

input {
  tcp {
    mode => "server"
    host => "0.0.0.0"
    port => 4560
    codec => json_lines
  }
}
output {
  elasticsearch {
    hosts => "elasticsearch:9200"
    index => "springboot-logstash-%{+YYYY.MM.dd}"
  }
}

```

### 2.5使用docker-compose.yml脚本启动ELK服务

创建docker-compose.yml，内容如下

```
version: '3'
services:
  elasticsearch:
    image: elasticsearch:7.0.1
    container_name: elasticsearch
    environment:
      - "cluster.name=elasticsearch" #设置集群名称为elasticsearch
      - "discovery.type=single-node" #以单一节点模式启动
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m" #设置使用jvm内存大小
    volumes:
      - /home/chen/mydata/elasticsearch/plugins:/usr/share/elasticsearch/plugins #插件文件挂载
      - /home/chen/mydata/elasticsearch/data:/usr/share/elasticsearch/data #数据文件挂载
    ports:
      - 59200:9200
      - 59300:9300
  kibana:
    image: kibana:7.0.1
    container_name: kibana
    links:
      - elasticsearch:es #可以用es这个域名访问elasticsearch服务
    depends_on:
      - elasticsearch #kibana在elasticsearch启动之后再启动
    environment:
      - "elasticsearch.hosts=http://es:9200" #设置访问elasticsearch的地址
    ports:
      - 55601:5601
  logstash:
    image: logstash:7.0.1
    container_name: logstash
    volumes:
      - /home/chen/mydata/logstash/logstash-springboot.conf:/usr/share/logstash/pipeline/logstash.conf #挂载logstash的配置文件
    depends_on:
      - elasticsearch #kibana在elasticsearch启动之后再启动
    links:
      - elasticsearch:es #可以用es这个域名访问elasticsearch服务
    ports:
      - 54560:4560
```

# **3.部署Elasticsearch，Logstash，Kibana**

在docker-compose.yml目录下，使用命令`docker-compose up -d`启动
![img](https://mmbiz.qpic.cn/mmbiz_png/3kMZppQa5zD0yjqM7iaxGM5BF5uiaDOwbMiacjX0Mic2R2Uo6X490Jd4InicXmpYjmWbEXUicRlXog1ZanBRxx1zQb7Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
启动成功！！！

### 3.1elasticsearch需要安装中文分词器IKAnalyzer

- 进入容器

```
docker exec -it elasticsearch /bin/bash
```

- 此命令需要在容器中运行

```
elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.4.0/elasticsearch-analysis-ik-6.4.0.zip
```

- 退出

```
exit
```

- 重启

```
docker restart elasticsearch
```

### 3.2在logstash中安装json_lines插件

- 进入logstash容器

```
docker exec -it logstash /bin/bash
```

- 安装插件

```
logstash-plugin install logstash-codec-json_lines
```

- 退出容器

```
exit
```

- 重启logstash服务

```
docker restart logstash
```

都重新启动好之后，这三个容器都是运行好的
![img](https://mmbiz.qpic.cn/mmbiz_png/3kMZppQa5zD0yjqM7iaxGM5BF5uiaDOwbMrGPpCJ2p08iaNeWWcow4fuibuRsK3kqqzxpoj6FhmcPT34FI7PGiaUdRg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

查看kibana
http://192.168.56.150:55601/
![img](https://mmbiz.qpic.cn/mmbiz_png/3kMZppQa5zD0yjqM7iaxGM5BF5uiaDOwbMPDKocLoXLkPmhKZxoyIn7ObalIY4aoKdZFVKzZ0jDy52qZUr1Mje5g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
查看elasticsearch是否成功启动
http://192.168.56.150:59200
![img](https://mmbiz.qpic.cn/mmbiz_png/3kMZppQa5zD0yjqM7iaxGM5BF5uiaDOwbMUwj9HoHdyYvtaT0k9L3ZYRqU5goT3MGgNoDzCgDY80G5NRRNpCljKQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

# **4.springboot应用集成logstash**

### 4.1.导入pom依赖

```xml
<!--集成logstash-->
<dependency>
    <groupId>net.logstash.logback</groupId>
    <artifactId>logstash-logback-encoder</artifactId>
    <version>5.3</version>
</dependency>
```

### 4.2添加配置文件logback-spring.xml让logback的日志输出到logstash

将下面的ip地址改成自己的

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration>
<configuration>
    <include resource="org/springframework/boot/logging/logback/defaults.xml"/>
    <include resource="org/springframework/boot/logging/logback/console-appender.xml"/>
    <!--应用名称-->
    <property name="APP_NAME" value="mall-admin"/>
    <!--日志文件保存路径-->
    <property name="LOG_FILE_PATH" value="${LOG_FILE:-${LOG_PATH:-${LOG_TEMP:-${java.io.tmpdir:-/tmp}}}/logs}"/>
    <contextName>${APP_NAME}</contextName>
    <!--每天记录日志到文件appender-->
    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${LOG_FILE_PATH}/${APP_NAME}-%d{yyyy-MM-dd}.log</fileNamePattern>
            <maxHistory>30</maxHistory>
        </rollingPolicy>
        <encoder>
            <pattern>${FILE_LOG_PATTERN}</pattern>
        </encoder>
    </appender>
    <!--输出到logstash的appender-->
    <appender name="LOGSTASH" class="net.logstash.logback.appender.LogstashTcpSocketAppender">
        <!--可以访问的logstash日志收集端口-->
        <destination>ip:4560</destination>
        <encoder charset="UTF-8" class="net.logstash.logback.encoder.LogstashEncoder"/>
    </appender>
    <root level="INFO">
        <appender-ref ref="CONSOLE"/>
        <appender-ref ref="FILE"/>
        <appender-ref ref="LOGSTASH"/>
    </root>
</configuration>
```

### 4.3配置yml文件

添加logstash配置文件路径

```
logging:
  config: classpath:logback-spring.xml
```

### 4.4测试

写了一个方法测试了一下
![img](https://mmbiz.qpic.cn/mmbiz_png/3kMZppQa5zD0yjqM7iaxGM5BF5uiaDOwbMQYZajniarPtQoXpibzN7akNyDJHFczZvJd1Aicr35qPwpSmK8ew9Fasyg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
访问路径：http://localhost:8081/agv/agvdevdao/selectAll
控制台输出
![img](https://mmbiz.qpic.cn/mmbiz_png/3kMZppQa5zD0yjqM7iaxGM5BF5uiaDOwbMT7eG4rPDBibPgnh2e2PuNxyN5D5qF2OicCOAdM3dr2DrjemjibQDkHbog/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
在kibana中的日志信息
![img](https://mmbiz.qpic.cn/mmbiz_png/3kMZppQa5zD0yjqM7iaxGM5BF5uiaDOwbMmc33dgiboMC5sGYcPwyCmicBG1VCPEBQYLJ4aq6EqWFibbXKgeOn9L9CA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
成功！！！

> 最后总结：
> 1.安装docker环境
> 2.获取elasticsearch,logstash,kibana的镜像
> 3.运行这三个容器
> 4.springboot集成logstash，将日志传输到elasticsearch





# 管理



```
docker stop logstash kibana elasticsearch
```



```
docker rm logstash kibana elasticsearch
```



```
docker start elasticsearch
```

```
 docker logs -f elasticsearch
```



# 问题

es：AccessDeniedException: /usr/share/elasticsearch/data/nodes

 挂载的目录没有读写权限  需要赋予它读写权限： 

```
chmod 777 chmod 777 /home/chen/mydata/elasticsearch
```

