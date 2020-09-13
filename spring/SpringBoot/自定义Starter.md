# 自定义Starter

## 背景

 		Spring Boot大量使用了starter模式，比如`spring-boot-starter-redis`，`spring-boot-starter-jdbc`，`spring-boot-starter-data-jpa`，`spring-boot-starter-amqp` ，使用starter极大的简化了开发人员的配置 。

​		有时，我们需要自研中间件，然后被其他项目依赖 ，为了做到对其他项目代码无污染入侵， 这个时候我们需要自定义starter，实现自动化配置，供其他项目依赖 。

## 自定义Starter

### 1、模块：

 		在springboot官方文档中，建议创建两个module ，其中一个是`autoconfigure module` ,一个是 `starter module` ，其中 `starter module `依赖 `autoconfigure module`。 

​		 不过，**如果不需要将自动配置代码和依赖项管理分离开来，则可以将它们组合到一个模块中**。 我们这里只创建一个module。

### 2、 命名规范 

​		**springboot 官方建议springboot官方推出的starter 以`spring-boot-starter-xxx`的格式来命名，第三方开发者自定义的starter则以`xxxx-spring-boot-starter`的规则来命名**。

### 3、参考官方的starter

​		按照springboot官方给的思路，starter的核心module应该是autoconfigure，所以我们直接去看`spring-boot-autoconfigure`里面的内容。 

​		 当Spring Boot启动时，它会在类路径中查找名为`spring.factories`的文件。该文件位于`META-INF`目录中。打开spring.factories文件，文件内容太多了，为了避免我水篇幅，我们只看其中的一部分： 

``` properties
# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
org.springframework.boot.autoconfigure.amqp.RabbitAutoConfiguration,\
org.springframework.boot.autoconfigure.batch.BatchAutoConfiguration,\
org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration,\
```

 我们可以发现一些比较眼熟的单词，比如Aop，Rabbit，Cache ，当springboot启动的时候，将会尝试加载这些配置类，如果该路径下存在该类的话，则将运行它，并初始化与该配置类相关的bean。 

 点开一个看看： 

``` java
@Configuration
@ConditionalOnClass({RabbitTemplate.class, Channel.class})
@EnableConfigurationProperties({RabbitProperties.class})
@Import({RabbitAnnotationDrivenConfiguration.class})
public class RabbitAutoConfiguration {
  
  //...代码略..
}
```

我们先来了解一下这几个注解:

- `@ConditionalOnClass` :条件注解，当classpath下发现该类的情况下进行自动配置。

- `@EnableConfigurationProperties`：外部化配置

- `@Import` ：引入其他的配置类

 当然，这并不是一种通用的套路，查看其他的配置类，我们会发现其标注的注解往往也是有所区别的。 

### 4、自定义starter

####  4.1、新建一个maven项目
pom.xml文件如下: 
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.chen.springboot</groupId>
    <artifactId>my-spring-boot-starter</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-autoconfigure</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
    </dependencies>
    <dependencyManagement>
        <!-- 我们是基于Springboot的应用 -->
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.2.2.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>
```

#### 4.2、 创建HostInfo类

然后我们创建一个HostInfo类，用作后期我们测试的bean 

```java
package com.chen.springboot.entity;

import lombok.Data;

/**
 * 主机信息
 *
 * @author chentiefeng
 * @create 2020-09-13 18:50
 */
@Data
public class HostInfo {
    /**
     * 主机地址
     */
    private String host;
    /**
     * 主机编码
     */
    private String code;
    /**
     * 开放端口
     */
    private int port;

}
```

4.3、创建 HostInfoConfigProperties 类

 然后我们也创建一个HostInfoConfigProperties 来完成我们属性的注入 

``` java
package com.chen.springboot.entity;

import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;

/**
 * 主机信息属性配置类
 *
 * @author chentiefeng
 * @create 2020-09-13 18:56
 */
@Data
@ConfigurationProperties(prefix = "mystarter.config.host")
public class HostInfoConfigProperties {
    /**
     * 主机地址
     */
    private String host;
    /**
     * 主机编码
     */
    private String code;
    /**
     * 开放端口
     */
    private int port;

}
```

####  4.3、创建自动配置类MyStarterAutoConfiguration

最后创建我们的自动配置类MyStarterAutoConfiguration.java 

```java
package com.chen.springboot.config;

import com.chen.springboot.entity.HostInfo;
import com.chen.springboot.entity.HostInfoProperties;
import org.springframework.boot.autoconfigure.condition.ConditionalOnClass;
import org.springframework.boot.autoconfigure.condition.ConditionalOnProperty;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * 自定义Starter的自动配置类
 *
 * @author chentiefeng
 * @create 2020-09-13 18:59
 */
@Configuration
@EnableConfigurationProperties(HostInfoProperties.class)
@ConditionalOnClass(HostInfo.class)
public class MyStarterAutoConfiguration {
    @Bean
    @ConditionalOnProperty(prefix = "mystarter.config", name = "enable", havingValue = "true")
    public HostInfo defaultHostInfo(HostInfoProperties personConfigProperties) {
        HostInfo hostInfo = new HostInfo();
        hostInfo.setHost(personConfigProperties.getHost());
        hostInfo.setCode(personConfigProperties.getCode());
        hostInfo.setPort(personConfigProperties.getPort());
        return hostInfo;
    }
}
```

 注意：`@ConditionalOnProperty`是用来控制是否启用配置，它的name或者value属性意义相同，区别是name是数组，value只绑定一个属性。当name或者value的属性为havingValue指定值的时候配置才会生效，否则配置不生效。即在下面的配置中，只有当enabled为true的时候配置才生效。 

#### 4.4、 创建 spring.factories 

##### 4.4.1、 创建 spring.factories 

 在`resources`文件目录下创建`META-INF`，并创建我们自己的`spring.factories`，并把我们的 `MyStarterAutoConfiguration`添加进去 

``` properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.chen.springboot.config.MyStarterAutoConfiguration
```

 最后打包成jar包，就可以在新的项目里面测试了

**注意**：除了通过spring.factories文件激活配置类，还可以通过注解的形式，只需要在主类上添加`@EnableHostClient`注解，不需要编写spring.factories文件，就可以激活配置类，注解定义如下：

> 关键在于`@Import`注解，它也申明了自动配置类`MyStarterAutoConfiguration`的路径

``` java
package com.chen.springboot.annotation;

import com.chen.springboot.config.MyStarterAutoConfiguration;
import org.springframework.context.annotation.Import;

import java.lang.annotation.*;

/**
 * 启动配置的注解
 *
 * @author chentiefeng
 * @create 2020-09-14 00:26
 */
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@Import(value = {MyStarterAutoConfiguration.class})
public @interface EnableHostClient {
    
}
```

##### 4.4.2、配置application.yml提示信息

​		配置application.yml提示信息，我们更倾向于在添加配置的时候能够看到配置信息或者默认值之类的，可以按如下方法配置：

- 在META-INF下创建文件`spring-configuration-metadata.json`，内容如下：

``` json
{
  "properties": [
    {
      "name": "mystarter.config.enabled",
      "type": "java.lang.Boolean",
      "defaultValue": false
    },
    {
      "name": "mystarter.config.host.host",
      "defaultValue": "127.0.0.1"
    },
    {
      "name": "mystarter.config.host.code",
      "defaultValue": "2020"
    },
    {
      "name": "mystarter.config.host.port",
      "type": "java.lang.Integer",
      "defaultValue": 8888
    }
  ]
}
```

#### 4.5、测试 

 引入我们的starter，当然也可以在本地直接引入我们的my-spring-boot-starter项目 

##### 4.5.1、引入starter依赖

新建maven项目，引入依赖：

```xml
        <dependency>
            <groupId>com.chen.springboot</groupId>
            <artifactId>my-spring-boot-starter</artifactId>
            <version>1.0-SNAPSHOT</version>
            <scope>system</scope>
            <systemPath>${project.basedir}/src/main/resources/lib/my-spring-boot-starter-1.0-SNAPSHOT.jar</systemPath>
        </dependency>
```

注：完整的pom.xml文件

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.chen.test</groupId>
    <artifactId>starter</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.2.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>com.chen.springboot</groupId>
            <artifactId>my-spring-boot-starter</artifactId>
            <version>1.0-SNAPSHOT</version>
            <scope>system</scope>
            <systemPath>${project.basedir}/src/main/resources/lib/my-spring-boot-starter-1.0-SNAPSHOT.jar</systemPath>
        </dependency>
    </dependencies>
</project>
```

##### 4.5.2、添加配置

 在`application.yml`配置文件中添加我们相应的配置 

``` yaml
mystarter:
  config:
    enable: true
    host:
      host: 192.168.1.1
      code: 2020
      port: 8888
```

#####  4.5.3、新建一个测试的Controller： 

``` java
package com.chen.test.web;

import com.chen.springboot.entity.HostInfo;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * 测试接口
 *
 * @author chentiefeng
 * @create 2020-09-13 19:17
 */
@RestController
public class TestController {

    @Autowired
    private HostInfo hostInfo;

    @RequestMapping("/getHostInfo")
    private HostInfo getHostInfo() {
        return hostInfo;
    }

}
```

##### 4.5.4、创建启动类

``` java
package com.chen.test;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * 启动类
 *
 * @author chentiefeng
 * @create 2020-09-13 19:18
 */
@SpringBootApplication
public class AppStarter {
    public static void main(String[] args) {
        SpringApplication.run(AppStarter.class,args);
    }
}
```

##### 4.5.5、启动测试

启动后，访问 http://localhost:8080/getHostInfo ，可以得到如下：

``` json
{
    "host": "192.168.1.1",
    "code": "2020",
    "port": 8888
}
```






































