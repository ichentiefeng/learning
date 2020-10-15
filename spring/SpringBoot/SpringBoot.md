## pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.chen</groupId>
    <artifactId>ElkTest</artifactId>
    <version>1.0-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.4.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
    </dependencies>
    
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```



# Springboot模块



## ` spring-boot-maven-plugin `

To create an executable jar, we need to add the `spring-boot-maven-plugin` to our `pom.xml`. To do so, insert the following lines just below the `dependencies` section:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

## `spring-boot-starter-actuator`

https://mp.weixin.qq.com/s?__biz=Mzg2MDIxODU2Nw==&mid=2247483719&idx=1&sn=c442adc684ede60da82465a33a6dad55&chksm=ce28f4eff95f7df90f871358766d0813d47133394f1340c6c8bab7cd2882a8cc8f8bff192089&mpshare=1&scene=1&srcid=1014AgmbVKFQC57TKTmAfOxT&sharer_sharetime=1602652534472&sharer_shareid=1d5d84b5f7aecbed6f8e41d989dae9ce&key=b6b2c5109aba53e43eaa633b3a8b81259827bf7dd245dfd72e1cbc8b712e38a35d6bf4c9dcbb5a5344774be4f0b17c149ae77ecf3a0246763aa4a8627dc34980db63e2da2e8d2266c436a62afd074fa05de2bd955e0f97e1d426f389e2321a58ef4fa075d4bbebab7533741ac704f873ce0c7fdffd8999fd3453b86b8cd689b8&ascene=1&uin=MTQxMzIwMDI4Mg%3D%3D&devicetype=Windows+10+x64&version=6300002f&lang=zh_CN&exportkey=AcGvpDrTAlPDPwQGmoZXgJo%3D&pass_ticket=RLxQSfBOwaGchmI%2B%2BG5lyfJeOb13ja2roWg%2B%2BwK2z1dgMpQXSODivxUBYnq027fZ&wx_header=0

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```



## `spring-boot-properties-migrator`

https://segmentfault.com/a/1190000021113056?utm_source=tag-newest

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-properties-migrator</artifactId>
    <scope>runtime</scope>
</dependency>
```

































































