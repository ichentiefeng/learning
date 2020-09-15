# Swagger

​		Swagger 是一个规范和完整的框架，用于生成、描述、调用和可视化 RESTful 风格的 Web 服务。 

官网： https://swagger.io/ 

​		Spring将Swagger规范纳入自身的标准，建立了`Spring-swagger`项目，后面改成了现在的Springfox。通过在项目中引入`Springfox`，可以扫描相关的代码，生成该描述文件，进而生成与代码一致的接口文档和客户端代码。

## 基于Spring框架的Swagger流程应用

在Spring项目开发中，我们一般有两种流程使用 Swagger ，具体如下：

流程一：

![](images/swagger使用流程一.png)

流程二：

![](images/swagger使用流程二 .png)

## 注解详解

- @ApiOperation：请求接口注解
  - value
  - notes
  - httpMethod ：Http请求方式，分为`POST`和`GET`
- @ApiParam：请求参数注解
  - name
  - value
  - required：是否必须填写，默认是false
- @ApiModel：对象注解
  - value
  - description
- @ApiModelProperty：对象属性注解
  - value：
  - example：示例值
  - required：是否必须填写，默认是false
- 

## 使用示例

### Controller层代码

``` java
    @Override
    @ApiOperation(value = "post请求调用示例", notes = "invokePost说明", httpMethod = "POST")
    public FFResponseModel<DemoOutputDto> invokePost(@ApiParam(name="传入对象",value="传入json格式",required=true) @RequestBody @Valid DemoDto input) {
        log.info("/testPost is called. input=" + input.toString());
        return new FFResponseModel(Errcode.SUCCESS_CODE, Errcode.SUCCESS_MSG);
    }
```

### 接口请求参对象

``` java
@Data
@ApiModel(value="演示类",description="请求参数类" )
public class DemoDto implements Serializable {

    private static final long serialVersionUID = 1L;

    @NotNull
    @ApiModelProperty(value = "defaultStr",example="mockStrValue")
    private String strDemo;

    @NotNull
    @ApiModelProperty(example="1234343523",required = true)
    private Long longNum;

    @NotNull
    @ApiModelProperty(example="111111.111")
    private Double doubleNum;

    @NotNull
    @ApiModelProperty(example="2018-12-04T13:46:56.711Z")
    private Date date;
    
}
```

### 接口响应公共参类

``` java
@ApiModel(value="基础返回类",description="基础返回类")
public class FFResponseModel<T> implements Serializable {

    private static final long serialVersionUID = -2215304260629038881L;
    // 状态码
    @ApiModelProperty(example="成功")
    private String code;
    // 业务提示语
    @ApiModelProperty(example="000000")
    private String msg;
    // 数据对象
    private T data;

...
}
```

### 接口响应实际数据对象

``` java
@Data
public class DemoOutputDto {

    private String res;

    @NotNull
    @ApiModelProperty(value = "defaultOutputStr",example="mockOutputStrValue")
    private String outputStrDemo;

    @NotNull
    @ApiModelProperty(example="6666666",required = true)
    private Long outputLongNum;

    @NotNull
    @ApiModelProperty(example="88888.888")
    private Double outputDoubleNum;

    @NotNull
    @ApiModelProperty(example="2018-12-12T11:11:11.111Z")
    private Date outputDate;
    
}
```

### 效果图

 模拟请求数据报文 ：

![模拟请求数据报文](images/模拟请求数据报文.png)

 模拟返回数据报文： 

![模拟返回数据报文](images/模拟返回数据报文.png)



## 总结

​		其实归根到底，使用Swagger，就是把相关的信息存储在它定义的描述文件里面（yml或json格式），再通过维护这个描述文件可以去更新接口文档，以及生成各端代码。

​		而`Springfox-swagger`,则可以通过扫描代码去生成这个描述文件，连描述文件都不需要再去维护了。所有的信息，都在代码里面了。代码即接口文档，接口文档即代码。















