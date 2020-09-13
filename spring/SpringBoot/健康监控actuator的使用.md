# 健康监控actuator的使用

官方教程地址： https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#production-ready 

## Actuator简介

​		 在Spring Boot的众多Starter POMs中有一个特殊的模块，它不同于其他模块那样大多用于开发业务功能或是连接一些其他外部资源。它完全是一个用于暴露自身信息的模块，所以很明显，它的主要作用是用于监控与管理，它就是：`spring-boot-starter-actuator`。 
​		 `spring-boot-starter-actuato`模块的实现对于实施微服务的中小团队来说，可以有效地减少监控系统在采集应用指标时的开发量。当然，它也并不是万能的，有时候我们也需要对其做一些简单的扩展来帮助我们实现自身系统个性化的监控需求。 
​		 在这种框架下，微服务的监控显得尤为重要，只需要引入`spring-boot-starter-actuator`即可。 

## 引入`spring-boot-starter-actuator`

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

## 监控和管理端点

默认的查看所有监控端点的地址： http://localhost:8080/actuator ，如：要访问健康信息，可以访问：` http://localhost:8080/actuator/health `

| Id                 | 描述                                                         |
| :----------------- | :----------------------------------------------------------- |
| `auditevents`      | 显示当前应用程序的审计事件                                   |
| `beans`            | 显示应用程序中所有 Spring bean 的完整列表。                  |
| `caches`           | 显示可用的缓存。                                             |
| `conditions`       | 显示在配置和自动配置类上计算的条件，以及它们不匹配或不匹配的原因。 |
| `configprops`      | 显示所有配置属性的列表。`@ConfigurationProperties`           |
| `env`              | 显示当前环境信息。`ConfigurableEnvironment`                  |
| `flyway`           | 显示已应用的任何 Flyway 数据库迁移。需要一个或多个豆类。`Flyway` |
| `health`           | 显示应用程序的健康指标，这些值由`HealthIndicator`的实现类提供。 |
| `httptrace`        | 显示 HTTP 跟踪信息（默认情况下，最后 100 个 HTTP 请求-响应交换）。需要一个豆子。`HttpTraceRepository` |
| `info`             | 显示当前应用信息。                                           |
| `integrationgraph` | 显示弹簧集成图。需要对 的依赖性。`spring-integration-core`   |
| `loggers`          | 显示和修改应用程序中记录器的配置。                           |
| `liquibase`        | 显示已应用的任何 Liquibase 数据库迁移。需要一个或多个豆类。`Liquibase` |
| `metrics`          | 显示当前应用程序的"指标"信息。                               |
| `mappings`         | 显示所有映射路径的列表。`@RequestMapping`                    |
| `scheduledtasks`   | 显示应用程序中的计划任务。                                   |
| `sessions`         | 允许从 Spring 会话支持的会话存储中检索和删除用户会话。需要使用 Spring 会话的基于 Servlet 的 Web 应用程序。 |
| `shutdown`         | 让应用程序正常关闭。默认情况下禁用。                         |
| `threaddump`       | 执行线程转储。                                               |

 如果应用程序是 Web 应用程序（Spring MVC、Spring WebFlux  ），您可以使用以下附加端点： 

| Id           | 描述                                                         |
| :----------- | :----------------------------------------------------------- |
| `heapdump`   | 返回堆转储文件。`hprof`                                      |
| `jolokia`    | 在 HTTP 上公开 JMX bean（当 Jolokia 在类路径上时，WebFlux 不可用）。需要对 的依赖性。`jolokia-core` |
| `logfile`    | 返回日志文件的内容（如果 已设置 或 属性）。支持使用 HTTP 标头检索日志文件的部分内容。`logging.file.name``logging.file.path``Range` |
| `prometheus` | 可以由普罗米修斯服务器收集格式的公开指标。需要添加`micrometer-registry-prometheus`依赖。 |

## 启用端点

  默认情况下， 除个别端点外，其他端点都没有启动。若要配置终结点的启用，请使用其属性。下面的示例启用终结点： 

``` properties
management.endpoint.shutdown.enabled=true
```

 如果希望逐个端点启动，可以通过关闭默认端点启动状态，逐个启动，如下： 

``` properties
management.endpoints.enabled-by-default=false
management.endpoint.info.enabled=true
```

 `*`可用于选择所有终结点。例如，若要通过 HTTP 公开除`env`和 `beans`终结点之外的所有内容，请使用以下属性：

```properties
management.endpoints.web.exposure.include=*          ## 暴露
management.endpoints.web.exposure.exclude=env,beans  ## 不暴露
```

在yml中配置如下：

``` yaml
management:
  endpoints:
    web:
      exposure:
        include: '*'
```

访问 http://localhost:8080/actuator ，可以看到所有开放端口。

## 公开端点

 由于端点可能包含敏感信息，因此应仔细考虑何时公开它们。下表显示了内置终结点的默认曝光情况： 

| Id                 | Jmx  | Web  |
| :----------------- | :--- | :--- |
| `auditevents`      | 是的 | 不   |
| `beans`            | 是的 | 不   |
| `caches`           | 是的 | 不   |
| `conditions`       | 是的 | 不   |
| `configprops`      | 是的 | 不   |
| `env`              | 是的 | 不   |
| `flyway`           | 是的 | 不   |
| `health`           | 是的 | 是的 |
| `heapdump`         | 不和 | 不   |
| `httptrace`        | 是的 | 不   |
| `info`             | 是的 | 是的 |
| `integrationgraph` | 是的 | 不   |
| `jolokia`          | 不和 | 不   |
| `logfile`          | 不和 | 不   |
| `loggers`          | 是的 | 不   |
| `liquibase`        | 是的 | 不   |
| `metrics`          | 是的 | 不   |
| `mappings`         | 是的 | 不   |
| `prometheus`       | 不和 | 不   |
| `scheduledtasks`   | 是的 | 不   |
| `sessions`         | 是的 | 不   |
| `shutdown`         | 是的 | 不   |
| `threaddump`       | 是的 | 不   |

 若要更改公开的终结点，请使用以下特定于技术的`include`和`exclude` 属性：

| 配置                                        | 默认           |
| :------------------------------------------ | :------------- |
| `management.endpoints.jmx.exposure.exclude` |                |
| `management.endpoints.jmx.exposure.include` | `*`            |
| `management.endpoints.web.exposure.exclude` |                |
| `management.endpoints.web.exposure.include` | `info, health` |

## 端点安全配置

为了端口的访问安全性，可以通过 Spring Security 进行管理，如下：

1、添加Spring Security依赖：

``` xml
 <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
 </dependency>
```

2、配置安全策略

``` java
@Configuration(proxyBeanMethods = false)
public class ActuatorSecurity extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.requestMatcher(EndpointRequest.toAnyEndpoint()).authorizeRequests((requests) ->
                requests.anyRequest().hasRole("ENDPOINT_ADMIN"));
        http.httpBasic();
    }

}
```

注意：以上如果公开所有端点，需要配置如下：

```properties
management.endpoints.web.exposure.include=*
```

``` java
@Configuration(proxyBeanMethods = false)
public class ActuatorSecurity extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.requestMatcher(EndpointRequest.toAnyEndpoint()).authorizeRequests((requests) ->
            requests.anyRequest().permitAll());
    }

}
```

## CORS 支持

 默认情况下，CORS 支持处于禁用状态，并且仅在设置属性后才启用。以下配置允许和调用域： 

``` properties
management.endpoints.web.cors.allowed-origins=https://example.com
management.endpoints.web.cors.allowed-methods=GET,POST
```



 