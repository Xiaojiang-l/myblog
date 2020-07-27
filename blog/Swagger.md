## Swagger

## 1、Swagger简介

### 1.1、前后端分离

Vue + SpringBoot

后端时代：前端只管理静态页面；

前后端分离时代

1. 后端：后端控制层，服务层，数据访问层
2. 前端：前端控制层，视图层
3. 前后端相对独立，松耦合
4. 前后端甚至可以部署在不同的服务器上

问题

- 前端后集尘协调，沟通不利

解决方法

- 制定计划提纲，实时更新API，降低集尘的风险
- 早些年：指定word计划文档
- 前后端分离：
  - 前端测试后端接口：postman
  - 后端提供接口，需要实时更新最新的消息改动



### 2.2、Swagger

> 号称世界上最流行的API框架
>
> RestFul Api 文档在线自动生成工具 =》API文档与API定义同步更新
>
> 直接运行，可以在线测试API接口

官网：https://swagger.io/

文档：https://swagger.io/docs/open-source-tools/swagger-editor/

![image-20200727110051606](.\Swagger.assets\image-20200727110051606.png)



## 2、Swagger的使用

> 在项目中使用swagger需要Springfox

- swagger2
- swagger-ui

### 2.1、SpringBoot集成Swagger

1. 新建SpringBoot项目`springboot-07-swagger`，选择`web`模板

2. 导入依赖

   ```xml
   <!-- springfox-swagger2 -->
   <dependency>
       <groupId>io.springfox</groupId>
       <artifactId>springfox-swagger2</artifactId>
       <version>2.9.2</version>
   </dependency>
   <!-- springfox-swagger-ui -->
   <dependency>
       <groupId>io.springfox</groupId>
       <artifactId>springfox-swagger-ui</artifactId>
       <version>2.9.2</version>
   </dependency>
   ```

3. 编写控制类`MyController`，进行环境测试

   ```java
   @RestController
   public class MyController {
       @RequestMapping("/hello")
       public String hello(){
           return "hello swagger!";
       }
   }
   ```

   ![image-20200727124120007](.\Swagger.assets\image-20200727124120007.png)

4. 配置Swagger ==> 编写配置类`SwaggerConfig`

   ```java
   @Configuration //配置类
   @EnableSwagger2// 开启Swagger2的自动配置
   public class SwaggerConfig {  
   }
   ```

5. 测试运行，访问http://localhost:8080/swagger-ui.html，可以看到swagger的界面；(如果访问出现404页面，那就降低依赖版本)

   ![image-20200727131828697](.\Swagger.assets\image-20200727131828697.png)

### 2.2、配置Swagger

1. Swagger实例Bean是Docket，所以通过配置Docket实例来配置Swaggger。

   ```java
   @Bean //配置docket以配置Swagger具体参数
   public Docket docket() {
      return new Docket(DocumentationType.SWAGGER_2);
   }
   ```

2. 可以通过apiInfo()属性配置文档信息

   ```java
   //配置文档信息
   private ApiInfo apiInfo() {
      Contact contact = new Contact("联系人名字", "http://xxx.xxx.com/联系人访问链接", "联系人邮箱");
      return new ApiInfo(
              "Swagger学习", // 标题
              "学习演示如何配置Swagger", // 描述
              "v1.0", // 版本
              "http://terms.service.url/组织链接", // 组织链接
              contact, // 联系人信息
              "Apach 2.0 许可", // 许可
              "许可链接", // 许可连接
              new ArrayList<>()// 扩展
     );
   }
   ```

3. Docket 实例关联上 apiInfo()

   ```java
   @Bean
   public Docket docket() {
      return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo());
   }
   ```

4. 重启项目，访问测试 http://localhost:8080/swagger-ui.html  看下效果；

   ![image-20200727132813371](.\Swagger.assets\image-20200727132813371.png)

### 2.3、配置扫描接口及开关

> 使用Docket.select()，以build()结尾

1. 编写`SwaggerConfig`代码

   ```java
   @Bean
   public Docket docket() {
       return new Docket(DocumentationType.SWAGGER_2)
           .apiInfo(apiInfo())
           // 通过.select()方法，去配置扫描接口
           .select()
           // RequestHandlerSelectors配置如何扫描接口
           .apis(RequestHandlerSelectors.basePackage("com.xj.controller"))
           .build();
   }
   ```

2. 重启项目测试，由于我们配置根据包的路径扫描接口，所以我们只能看到一个类

   ![image-20200727133218848](.\Swagger.assets\image-20200727133218848.png)

3. 除了通过包路径配置扫描接口外，还可以通过配置其他方式扫描接口，这里注释一下所有的配置方式：

   ```java
   any() // 扫描所有，项目中的所有接口都会被扫描到
   none() // 不扫描接口
   // 通过方法上的注解扫描，如withMethodAnnotation(GetMapping.class)只扫描get请求
   withMethodAnnotation(final Class<? extends Annotation> annotation)
   // 通过类上的注解扫描，如.withClassAnnotation(Controller.class)只扫描有controller注解的类中的接口
   withClassAnnotation(final Class<? extends Annotation> annotation)
   basePackage(final String basePackage) // 根据包路径扫描接口
   ```

   ![image-20200727115430698](.\Swagger.assets\image-20200727115430698.png)

4. 除此之外，对指定显示的内容进行再**过滤**

   ```java
   @Bean
   public Docket docket() {
       return new Docket(DocumentationType.SWAGGER_2)
           .apiInfo(apiInfo())
           .select()// 通过.select()方法，去配置扫描接口,RequestHandlerSelectors配置如何扫描接口
           .apis(RequestHandlerSelectors.basePackage("com.xj.controller"))
           // 配置如何通过path过滤,即这里只扫描请求以/xj开头的接口
           .paths(PathSelectors.ant("/xj/**"))
         	  /* 
      	    	any() // 任何请求都扫描
   			none() // 任何请求都不扫描
   			regex(final String pathRegex) // 通过正则表达式控制
   			ant(final String antPattern) // 通过ant()控制
      	    */
           .build();
   }
   ```

> 配置Swagger开关

- enable是否启动Swagger，如果为false，则不能在浏览器中访问swagger

  ```java
  @Bean
  public Docket docket() {
      return new Docket(DocumentationType.SWAGGER_2)
          .apiInfo(apiInfo())
          .enable(false) //配置是否启用Swagger，如果是false，在浏览器将无法访问
          .select()// 通过.select()方法，去配置扫描接口,RequestHandlerSelectors配置如何扫描接口
          .apis(RequestHandlerSelectors.basePackage("com.xj.controller"))
          // 配置如何通过path过滤,即这里只扫描请求以/xj开头的接口
          .paths(PathSelectors.ant("/xj/**"))
          .build();
  }
  ```

让Swagger在生产环境中使用，在发布的时候不使用

- 判断是不是生产环境 flag=false
- 注入enable()

步骤：

1. 编写两个配置文件`application-dev.properties`和`application-pro.properties`，分别为8081和8082端口，然后在`application.properties`中指定默认环境

   ```properties
   # 分别指定端口号
   # application-dev.properties中的
   server.port=8081
   
   # application-pro.properties中的
   server.port=8082
   ```

   ```properties
   # 在application.properties中指定默认环境
   spring.profiles.active=dev
   ```

2. 修改配置类，获取项目的环境

   ```java
   @Bean
   public Docket docket(Environment environment) {
       // 设置要显示swagger的环境
       Profiles profiles = Profiles.of("dev", "test");
       // 判断当前是否处于该环境
       // 通过 enable() 接收此参数判断是否要显示
       boolean flag = environment.acceptsProfiles(profiles);
   
       return new Docket(DocumentationType.SWAGGER_2)
           .apiInfo(apiInfo())
           .enable(flag) //配置是否启用Swagger，如果是false，在浏览器将无法访问
           .select()// 通过.select()方法，去配置扫描接口,RequestHandlerSelectors配置如何扫描接口
           .apis(RequestHandlerSelectors.basePackage("com.xj.controller"))
           // 配置如何通过path过滤,即这里只扫描请求以/xj开头的接口
           .paths(PathSelectors.ant("/xj/**"))
           .build();
   }
   ```

   测试的时候注意修改地址的端口号，如设置了默认dev就需要变为8081

   ![image-20200727134554985](.\Swagger.assets\image-20200727134554985.png)



### 2.4、配置API分组

1. 如果没有配置分组，默认是default。通过groupName()方法即可配置分组：

   ```java
   @Bean
   public Docket docket(Environment environment) {
       return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo())
           .groupName("晓江") // 配置分组
           // 省略配置....
   }
   ```

2. 重启项目查看分组

   ![image-20200727134808228](.\Swagger.assets\image-20200727134808228.png)

3. 如何配置多个分组？配置多个分组只需要配置多个docket即可：

   ```java
   @Bean
   public Docket docket1(){
      return new Docket(DocumentationType.SWAGGER_2).groupName("group1");
   }
   @Bean
   public Docket docket2(){
      return new Docket(DocumentationType.SWAGGER_2).groupName("group2");
   }
   @Bean
   public Docket docket3(){
      return new Docket(DocumentationType.SWAGGER_2).groupName("group3");
   }
   ```

4. 重启项目查看即可

   ![image-20200727135004239](.\Swagger.assets\image-20200727135004239.png)

### 2.5、实体配置

> 主要是控制类进行增加上去的，而实体类中的注解是说明作用，只影响显示中的说明
>
> @ApiModel：修饰类
>
> @ApiModelProperty：修饰属性

1. 创建pojo实体类

   ```java
   @ApiModel("用户实体")
   public class User {
       @ApiModelProperty("用户名")
       public String username;
       @ApiModelProperty("密码")
       public String password;
   }
   ```

2. 修改控制类，增加如下代码

   ```java
   @RequestMapping("/getUser")
   public User getUser(){
       return new User();
   }
   ```

3. 重启查看测试，注意切换到我们配置的组内

   ![image-20200727135228045](.\Swagger.assets\image-20200727135228045.png)



### 2.6、常用注解

Swagger的所有注解定义在io.swagger.annotations包下

下面列一些经常用到的，未列举出来的可以另行查阅说明：

| Swagger注解                                            | 简单说明                                             |
| ------------------------------------------------------ | ---------------------------------------------------- |
| @Api(tags = "xxx模块说明")                             | 作用在模块类上                                       |
| @ApiOperation("xxx接口说明")                           | 作用在接口方法上                                     |
| @ApiModel("xxxPOJO说明")                               | 作用在模型类上：如VO、BO                             |
| @ApiModelProperty(value = "xxx属性说明",hidden = true) | 作用在类方法和属性上，hidden设置为true可以隐藏该属性 |
| @ApiParam("xxx参数说明")                               | 作用在参数、方法和字段上，类似@ApiModelProperty      |

在控制类中进行修饰，都是修饰方法的

```java
@ApiOperation("获取用户名")
@GetMapping("/user/{username}")
public String getUsername(@PathVariable("username") @ApiParam("用户名") String username){
    return username;
}
```

![image-20200727141921819](.\Swagger.assets\image-20200727141921819.png)

![image-20200727143606867](.\Swagger.assets\image-20200727143606867.png)

这样的话，可以给一些比较难理解的属性或者接口，增加一些配置信息，让人更容易阅读和测试！



### 2.7、拓展：其他皮肤

我们可以导入不同的包实现不同的皮肤定义：如果修改了端口号记得访问**对应端口**，里面的配置信息还是一致。

1. 默认的  **访问 http://localhost:8080/swagger-ui.html**

   ```xml
   <dependency> 
      <groupId>io.springfox</groupId>
      <artifactId>springfox-swagger-ui</artifactId>
      <version>2.9.2</version>
   </dependency>
   ```

   ![image-20200727144645271](.\Swagger.assets\image-20200727144645271.png)

2. bootstrap-ui  **访问 http://localhost:8080/doc.html**

   ```xml
   <!-- 引入swagger-bootstrap-ui包 /doc.html-->
   <dependency>
      <groupId>com.github.xiaoymin</groupId>
      <artifactId>swagger-bootstrap-ui</artifactId>
      <version>1.9.1</version>
   </dependency>
   ```

   ![image-20200727144547432](.\Swagger.assets\image-20200727144547432.png)

3. Layui-ui  **访问 http://localhost:8080/docs.html**，这个暂时访问不了

   ```xml
   <!-- 引入swagger-ui-layer包 /docs.html-->
   <dependency>
      <groupId>com.github.caspar-chen</groupId>
      <artifactId>swagger-ui-layer</artifactId>
      <version>1.1.3</version>
   </dependency>
   ```

   ![image-20200727151615208](.\Swagger.assets\image-20200727151615208.png)

4. mg-ui  **访问 http://localhost:8080/document.html**

   ```xml
   <!-- 引入swagger-mg-ui包 /document.html-->
   <dependency>
      <groupId>com.zyplayer</groupId>
      <artifactId>swagger-mg-ui</artifactId>
      <version>1.0.6</version>
   </dependency>
   ```

   ![image-20200727150936383](.\Swagger.assets\image-20200727150936383.png)





### 总结：

1. 我们可以通过Swagger给一些比较难理解的属性或者接口增加注释信息
2. 接口文档实时更新
3. 可以在线测试

【注意】在正式发布的时候，需要关闭Swagger！出于安全也节省空间！

























