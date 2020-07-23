## 1、JSON

### 1.1、什么是JSON

- JSON(JavaScript Object Notation, JS 对象标记) 是一种轻量级的数据交换格式，目前使用特别广泛。
- 采用完全独立于编程语言的文本格式来存储和表示数据。
- 简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。
- 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。

**在 JavaScript 语言中，一切都是对象。因此，任何JavaScript 支持的类型都可以通过 JSON 来表示，例如字符串、数字、对象、数组等。看看他的要求和语法格式：**

- 对象表示为键值对，数据由逗号分隔
- 花括号保存对象
- 方括号保存数组

**JSON 键值对**是用来保存 JavaScript 对象的一种方式，和JavaScript 对象的写法也大同小异，键/值对组合中的键名写在前面并用双引号 "" 包裹，使用冒号 : 分隔，然后紧接着值：

```json
{"name": "QinJiang"}
{"age": "3"}
{"sex": "男"}
```



**JSON 和 JavaScript 对象**

- Json本质是字符串

  ```js
  var obj = {a: 'Hello', b: 'World'}; //这是一个对象，注意键名也是可以使用引号包裹的
  var json = '{"a": "Hello", "b": "World"}'; //这是一个 JSON 字符串，本质是一个字符串
  ```

- JSON 和 JavaScript 对象互转

  ```js
  //要实现从JSON字符串转换为JavaScript 对象，使用 JSON.parse() 方法
  var obj = JSON.parse('{"a": "Hello", "b": "World"}');
  //结果是 {a: 'Hello', b: 'World'}
  
  //要实现从JavaScript对象转换为JSON字符串，使用 JSON.stringify() 方法
  var json = JSON.stringify({a: 'Hello', b: 'World'});
  //结果是 '{"a": "Hello", "b": "World"}'
  ```



#### **代码测试**

1. 新建一个module ，SpringMVC_04_Json ， 添加web的支持

2. 在web目录下新建一个 json-1.html ， 编写测试内容

   ```html
   <!DOCTYPE html>
   <html lang="en">
       <head>
           <meta charset="UTF-8">
           <title>JSON_晓江</title>
       </head>
       <body>
           <script type="text/javascript">
               //编写一个js的对象
               var user = {
                   name:"晓江",
                   age:3,
                   sex:"男"
               };
               //将js对象转换成json字符串
               var str = JSON.stringify(user);
               console.log(str);
               //将json字符串转换为js对象
               var user2 = JSON.parse(str);
               console.log(user2.age,user2.name,user2.sex);
           </script>
       </body>
   </html>
   ```

   

### 1.2、Controller返回JSON数据

1. Jackson应该是目前比较好的json解析工具了

2. 当然工具不止这一个，比如还有阿里巴巴的 fastjson 等等。

3. 我们这里使用Jackson，使用它需要导入它的jar包；

   ```xml
   <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
   <dependency>
       <groupId>com.fasterxml.jackson.core</groupId>
       <artifactId>jackson-databind</artifactId>
       <version>2.10.2</version>
   </dependency>
   ```

4. 配置SpringMVC需要的配置
   web.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <!--1.注册DispatcherServlet-->
       <servlet>
           <servlet-name>SpringMVC</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--关联一个SpringMVC的配置文件:【servlet-name】-servlet.xml-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:SpringMVC-Servlet.xml</param-value>
           </init-param>
           <!--启动级别-1-->
           <load-on-startup>1</load-on-startup>
       </servlet>
       <!--/ 匹配所有的请求；（不包括.jsp）-->
       <!--/* 匹配所有的请求；（包括.jsp）-->
       <servlet-mapping>
           <servlet-name>SpringMVC</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
       <!--过滤器，处理Web乱码问题-->
       <filter>
           <filter-name>encoding</filter-name>
           <filter-class>
               org.springframework.web.filter.CharacterEncodingFilter</filter-class>
           <init-param>
               <param-name>encoding</param-name>
               <param-value>utf-8</param-value>
           </init-param>
       </filter>
       <filter-mapping>
           <filter-name>encoding</filter-name>
           <url-pattern>/</url-pattern>
       </filter-mapping>
   </web-app>
   ```

   springmvc-servlet.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/mvc
           http://www.springframework.org/schema/mvc/spring-mvc.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context.xsd">
   
       <!-- 自动扫描包，让指定包下的注解生效,由IOC容器统一管理 -->
       <context:component-scan base-package="com.xj.controller"/>
       <!-- 让Spring MVC不处理静态资源 -->
       <mvc:default-servlet-handler />
       <mvc:annotation-driven />
   
       <!-- 视图解析器 -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
           <!-- 前缀 -->
           <property name="prefix" value="/WEB-INF/jsp/" />
           <!-- 后缀 -->
           <property name="suffix" value=".jsp" />
       </bean>
   </beans>
   ```

5. 编写一个实体类

   ```java
   package com.xj.pojo;
   
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class User {
       private String name;
       private int age;
       private String sex;
   }
   ```

6. 这里我们需要两个新东西，一个是@ResponseBody（不走视图解析器），一个是ObjectMapper对象，我们看下具体的用法编写一个Controller；

   ```java
   package com.xj.controller;
   
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.xj.pojo.User;
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.ResponseBody;
   
   @Controller
   public class UserController {
   
       @RequestMapping("json1")
       @ResponseBody
       public String json1() throws JsonProcessingException {
           //创建一个jackson的对象映射器，用来解析数据
           ObjectMapper mapper = new ObjectMapper();
           // 创建一个对象
           User user = new User("晓江", 20, "男");
           // 将对象转为JSON格式
           String str = mapper.writeValueAsString(user);
           // 不会经过视图解析器，直接显示出来
           // 由于@ResponseBody注解，这里会将str转成json格式返回
           return str;
       }
   }
   ```

7. 测试（出现乱码）

   ![image-20200614144143284](.\Ajax和JSON.assets\image-20200614144143284-1595425401176.png)

   - 发现出现了乱码问题，我们需要设置一下他的编码格式为utf-8，以及它返回的类型；
   - 通过@RequestMaping的produces属性来实现，修改下代码

   ```java
   //produces:指定响应体返回类型和编码
   @RequestMapping(value = "/json1",produces ="application/json;charset=utf-8")
   @ResponseBody
   ```

   - 测试结果正常

     ![image-20200614144502259](.\Ajax和JSON.assets\image-20200614144502259-1595425401177.png)

   - 如果没有使用注解@ResponseBody会经过视图解析器处理

     ![image-20200614144613903](.\Ajax和JSON.assets\image-20200614144613903-1595425401177.png)

**【注意：使用json记得处理乱码问题】**



### 1.3、代码优化

#### 乱码统一解决

上一种方法比较麻烦，如果项目中有许多请求则每一个都要添加，可以通过Spring配置统一指定，这样
就不用每次都去处理了！

- **我们可以在springmvc的配置文件上添加一段消息StringHttpMessageConverter转换配置！**

  ```xml
  <!--统一处理JSON乱码-->
  <mvc:annotation-driven>
      <mvc:message-converters register-defaults="true">
          <bean class="org.springframework.http.converter.StringHttpMessageConverter">
              <constructor-arg value="UTF-8"/>
          </bean>
          <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
              <property name="objectMapper">
                  <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                      <property name="failOnEmptyBeans" value="false"/>
                  </bean>
              </property>
          </bean>
      </mvc:message-converters>
  </mvc:annotation-driven>
  ```

- **返回json字符串统一解决**
  在类上直接使用 **@RestController** ，这样子，里面所有的方法都只会返回 json 字符串了，不用再每一
  个都添加**@ResponseBody** ！我们在前后端分离开发中，一般都使用 @RestController ，十分便捷！

  ```java
  @RestController
  public class UserController2 {
      @RequestMapping("json2")
      public String json2() throws JsonProcessingException {
          // 创建一个jackson的对象映射器来解析数据
          ObjectMapper mapper = new ObjectMapper();
          // 创建对象
          User user = new User("晓江2", 20, "男");
          // 将对象解析为JSON格式
          String str = mapper.writeValueAsString(user);
          return str;
      }
  }
  ```

  重新启动tomcat测试，结果都正常输出！



### 1.4、测试集合输出

- 添加一个新方法

  ```java
  @RequestMapping("json3")
  public String json3() throws JsonProcessingException {
      //创建一个jackson的对象映射器，用来解析数据
      ObjectMapper mapper = new ObjectMapper();
      //创建一个对象
      User user1 = new User("晓江1号", 3, "男");
      User user2 = new User("晓江2号", 3, "男");
      User user3 = new User("晓江3号", 3, "男");
      User user4 = new User("晓江4号", 3, "男");
      List<User> list = new ArrayList<User>();
      list.add(user1);
      list.add(user2);
      list.add(user3);
      list.add(user4);
      //将我们的对象解析成为json格式
      String str = mapper.writeValueAsString(list);
      return str;
  }
  ```

- 测试正常

  ![image-20200614145904789](.\Ajax和JSON.assets\image-20200614145904789-1595425401177.png)



### 1.5、输出时间对象

- 增加一个新方法

  ```java
  @RequestMapping("json4")
  public String json4() throws JsonProcessingException {
      ObjectMapper mapper = new ObjectMapper();
      //创建时间一个对象，java.util.Date
      Date date = new Date();
      //将我们的对象解析成为json格式
      String str = mapper.writeValueAsString(date);
      return str;
  }
  ```

- 测试

  ![image-20200614150154130](.\Ajax和JSON.assets\image-20200614150154130-1595425401177.png)

- 默认日期格式会变成一个数字，**是1970年1月1日到当前日期的毫秒数**！Jackson 默认是会把时间转成timestamps形式

- **解决方案**：取消timestamps形式 ， 自定义时间格式

  ```java
  @RequestMapping("json4")
  public String json4() throws JsonProcessingException {
      ObjectMapper mapper = new ObjectMapper();
      //不使用时间戳的方式
      mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
      //自定义日期格式对象
      SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
      //指定日期格式
      mapper.setDateFormat(sdf);
  
      //创建时间一个对象
      Date date = new Date();
      //将我们的对象解析成为json格式
      String str = mapper.writeValueAsString(date);
      return str;
  }
  ```

- 测试

  ![image-20200614150639474](.\Ajax和JSON.assets\image-20200614150639474-1595425401177.png)



### 1.6、抽取为工具类

```java
package com.xj.utils;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;
import java.text.SimpleDateFormat;

public class JsonUtils {
    // 重构方法，只有一个参数的格式
    public static String getJson(Object object) {
        return getJson(object,"yyyy-MM-dd HH:mm:ss");
    }
    public static String getJson(Object object,String dateFormat) {
        ObjectMapper mapper = new ObjectMapper();
        //不使用时间差的方式
        mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
        //自定义日期格式对象
        SimpleDateFormat sdf = new SimpleDateFormat(dateFormat);
        //指定日期格式
        mapper.setDateFormat(sdf);
        try {
            return mapper.writeValueAsString(object);
        } catch (JsonProcessingException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

使用工具类

```java
@RequestMapping("/json5")
public String json5() throws JsonProcessingException {
    Date date = new Date();
    String json = JsonUtils.getJson(date);
    return json;
}
```

测试

![image-20200614151100556](.\Ajax和JSON.assets\image-20200614151100556-1595425401177.png)



### 7.7、FastJson

​	fastjson.jar是阿里开发的一款专门用于Java开发的包，可以方便的实现json对象与JavaBean对象的转
换，实现JavaBean对象与json字符串的转换，实现json对象与json字符串的转换。实现json的转换方法
很多，最后的实现结果都是一样的。

- **fastjson 的 pom依赖！**

  ```xml
  <!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
  <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>fastjson</artifactId>
      <version>1.2.70</version>
  </dependency>
  ```

- **fastjson 三个主要的类：**

  1. 【JSONObject 代表 json 对象 】
     - JSONObject实现了Map接口, 猜想 JSONObject底层操作是由Map实现的。
     - JSONObject对应json对象，通过各种形式的get()方法可以获取json对象中的数据，也可利用
       诸如size()，isEmpty()等方法获取"键：值"对的个数和判断是否为空。其本质是通过实现Map
       接口并调用接口中的方法完成的。
  2. 【JSONArray 代表 json 对象数组】
     - 内部是有List接口中的方法来完成操作的。
  3. 【JSON 代表 JSONObject和JSONArray的转化】
     - JSON类源码分析与使用
     - 仔细观察这些方法，主要是实现json对象，json对象数组，javabean对象，json字符串之间
       的相互转化。

- **使用**

  1. 我们新建一个FastJsonDemo 类

     ```java
     package com.xj.demo;
     import com.alibaba.fastjson.JSON;
     import com.alibaba.fastjson.JSONObject;
     import com.xj.pojo.User;
     
     import java.util.ArrayList;
     import java.util.List;
     
     public class FastJsonDemo {
         public static void main(String[] args) {
             //创建一个对象
             User user1 = new User("晓江1号", 3, "男");
             User user2 = new User("晓江2号", 3, "男");
             User user3 = new User("晓江3号", 3, "男");
             User user4 = new User("晓江4号", 3, "男");
             List<User> list = new ArrayList<User>();
             list.add(user1);
             list.add(user2);
             list.add(user3);
             list.add(user4);
             System.out.println("*******Java对象 转 JSON字符串*******");
             String str1 = JSON.toJSONString(list);
             System.out.println("JSON.toJSONString(list)==>"+str1);
             String str2 = JSON.toJSONString(user1);
             System.out.println("JSON.toJSONString(user1)==>"+str2);
             System.out.println("\n****** JSON字符串 转 Java对象*******");
             User jp_user1=JSON.parseObject(str2,User.class);
             System.out.println("JSON.parseObject(str2,User.class)==>"+jp_user1);
             System.out.println("\n****** Java对象 转 JSON对象 ******");
             JSONObject jsonObject1 = (JSONObject) JSON.toJSON(user2);
             System.out.println("(JSONObject)SON.toJSON(user2)==>"+jsonObject1.getString("name"));
             System.out.println("\n****** JSON对象 转 Java对象 ******");
             User to_java_user = JSON.toJavaObject(jsonObject1, User.class);
             System.out.println("JSON.toJavaObject(jsonObject1,User.class)==>"+to_java_user);
         }
     }
     ```

  2. 控制台输出

     ```java
     *******Java对象 转 JSON字符串*******
     JSON.toJSONString(list)==>[{"age":3,"name":"晓江1号","sex":"男"},{"age":3,"name":"晓江2号","sex":"男"},{"age":3,"name":"晓江3号","sex":"男"},{"age":3,"name":"晓江4号","sex":"男"}]
     JSON.toJSONString(user1)==>{"age":3,"name":"晓江1号","sex":"男"}
     
     ****** JSON字符串 转 Java对象*******
     JSON.parseObject(str2,User.class)==>User(name=晓江1号, age=3, sex=男)
     
     ****** Java对象 转 JSON对象 ******
     (JSONObject)SON.toJSON(user2)==>晓江2号
     
     ****** JSON对象 转 Java对象 ******
     JSON.toJavaObject(jsonObject1,User.class)==>User(name=晓江2号, age=3, sex=男)
     ```

## 2、Ajax

### 2.1、简介

- **A JAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。**

- A JAX 是一种在**无需重新加载整个网页**的情况下，能够更新部分网页的技术。

- **Ajax 不是一种新的编程语言，而是一种用于创建更好更快以及交互性更强的Web应用程序的技术。**

- 在 2005 年，Google 通过其 Google Suggest 使 A JAX 变得流行起来。Google Suggest能够自动帮你完成搜索单词。

- Google Suggest 使用 A JAX 创造出动态性极强的 web 界面：当您在谷歌的搜索框输入关键字时，

- JavaScript 会把这些字符发送到服务器，然后服务器会返回一个搜索建议的列表。

- 就和国内百度的搜索框一样：

  ![image-20200620181845202](.\Ajax和JSON.assets\image-20200620181845202-1595425401177.png)

- 传统的网页(即不用ajax技术的网页)，想要更新内容或者提交一个表单，都需要重新加载整个网页。

- 使用ajax技术的网页，通过在后台服务器进行少量的数据交换，就可以实现异步局部更新。

- 使用Ajax，用户可以创建接近本地桌面应用的直接、高可用、更丰富、更动态的Web用户界面。

### 2.2、伪造Ajax

> 我们可以使用前端的一个标签来伪造一个ajax的样子，使用 iframe标签制作出一个窗口。

1. 新建一个module ： sspringmvc-05-ajax ， 导入web支持！

2. 编写一个 ajax-frame.html 使用 iframe 测试，感受下效果

   ```html
   <!DOCTYPE html>
   <html>
       <head lang="en">
           <meta charset="UTF-8">
           <title>kuangshen</title>
       </head>
       <body>
           <script type="text/javascript">
               window.onload = function(){
                   var myDate = new Date();
                   document.getElementById('currentTime').innerText =
                       myDate.getTime();
               };
               function LoadPage(){
                   var targetUrl = document.getElementById('url').value;
                   console.log(targetUrl);
                   document.getElementById("iframePosition").src = targetUrl;
               }
           </script>
           <div>
               <p>请输入要加载的地址：<span id="currentTime"></span></p>
               <p>
                   <input id="url" type="text" value="https://www.baidu.com/"/>
                   <input type="button" value="提交" onclick="LoadPage()">
               </p>
           </div>
           <div>
               <h3>加载页面位置：</h3>
               <iframe id="iframePosition" style="width: 100%;height: 500px;">
               </iframe>
           </div>
       </body>
   </html>
   ```

3. 使用浏览器进行测试

   ![image-20200620210854240](.\Ajax和JSON.assets\image-20200620210854240-1595425401177.png)

**利用AJAX可以做：**

- 注册时，输入用户名自动检测用户是否已经存在。
- 登陆时，提示用户名密码错误
- 删除数据行时，将行ID发送到后台，后台在数据库中删除，数据库删除成功后，在页面DOM中将数据行也删除等等。



### 2.3、jQuery.ajax

- 纯JS原生实现Ajax我们不去讲解这里，直接使用jquery提供的，方便学习和使用，避免重复造轮子，有兴趣的同学可以去了解下JS原生XMLHttpRequest ！
- Ajax的核心是XMLHttpRequest对象(XHR)。XHR为向服务器发送请求和解析服务器响应提供了接口。能够以异步方式从服务器获取新数据。
- **jQuery 提供多个与 A JAX 有关的方法。**
- 通过 jQuery A JAX 方法，您能够使用 HTTP Get 和 HTTP Post 从远程服务器上请求文本、HTML、XML 或 JSON – 同时您能够把这些外部数据直接载入网页的被选元素中。
- **jQuery 不是生产者，而是大自然搬运工。**
- jQuery Ajax本质就是 XMLHttpRequest，对他进行了封装，方便调用！

```java
jQuery.ajax(...)
部分参数：
url：请求地址
type：请求方式，GET、POST（1.9.0之后用method）
headers：请求头
data：要发送的数据
contentType：即将发送信息至服务器的内容编码类型(默认: "application/x-wwwform-urlencoded; charset=UTF-8")
async：是否异步
timeout：设置请求超时时间（毫秒）
beforeSend：发送请求前执行的函数(全局)
complete：完成之后执行的回调函数(全局)
success：成功之后执行的回调函数(全局)
error：失败之后执行的回调函数(全局)
accepts：通过请求头发送给服务器，告诉服务器当前客户端课接受的数据类型
dataType：将服务器端返回的数据转换成指定类型
"xml": 将服务器端返回的内容转换成xml格式
"text": 将服务器端返回的内容转换成普通文本格式
"html": 将服务器端返回的内容转换成普通文本格式，在插入DOM中时，如果包含
JavaScript标签，则会尝试去执行。
"script": 尝试将返回值当作JavaScript去执行，然后再将服务器端返回的内容转换成
普通文本格式
"json": 将服务器端返回的内容转换成相应的JavaScript对象
"jsonp": JSONP 格式使用 JSONP 形式调用函数时，如 "myurl?callback=?"
jQuery 将自动替换 ? 为正确的函数名，以执行回调函数
```

1. **jQuery下载**

   下载地址：https://jquery.com/

   ![image-20200620182720053](.\Ajax和JSON.assets\image-20200620182720053-1595425401177.png)

   ![image-20200620183045913](.\Ajax和JSON.assets\image-20200620183045913-1595425401177.png)

2. 配置web.xml 和 springmvc的配置文件 【记得静态资源过滤和注解驱动配置上】

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <!--1.注册DispatcherServlet-->
       <servlet>
           <servlet-name>SpringMVC</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--关联一个SpringMVC的配置文件:【servlet-name】-servlet.xml-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:SpringMVC-Servlet.xml</param-value>
           </init-param>
           <!--启动级别-1-->
           <load-on-startup>1</load-on-startup>
       </servlet>
       <!--/ 匹配所有的请求；（不包括.jsp）-->
       <!--/* 匹配所有的请求；（包括.jsp）-->
       <servlet-mapping>
           <servlet-name>SpringMVC</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
       <!--过滤器，处理web乱码问题-->
       <filter>
           <filter-name>encoding</filter-name>
           <filter-class>
               org.springframework.web.filter.CharacterEncodingFilter</filter-class>
           <init-param>
               <param-name>encoding</param-name>
               <param-value>utf-8</param-value>
           </init-param>
       </filter>
       <filter-mapping>
           <filter-name>encoding</filter-name>
           <url-pattern>/</url-pattern>
       </filter-mapping>
   </web-app>
   ```

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/mvc
           http://www.springframework.org/schema/mvc/spring-mvc.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context.xsd">
   
       <!-- 自动扫描包，让指定包下的注解生效,由IOC容器统一管理 -->
       <context:component-scan base-package="com.xj.controller"/>
       <!-- 让Spring MVC处理静态资源 -->
       <mvc:default-servlet-handler />
       <!--统一处理JSON乱码-->
       <mvc:annotation-driven>
           <mvc:message-converters register-defaults= "true">
               <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                   <constructor-arg value="UTF-8"/>
               </bean>
               <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
                   <property name="objectMapper">
                       <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                           <property name="failOnEmptyBeans" value="false"/>
                       </bean>
                   </property>
               </bean>
           </mvc:message-converters>
       </mvc:annotation-driven>
   
       <!-- 视图解析器 -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
           <!-- 前缀 -->
           <property name="prefix" value="/WEB-INF/jsp/" />
           <!-- 后缀 -->
           <property name="suffix" value=".jsp" />
       </bean>
   </beans>
   ```

3. 编写一个AjaxController

   ```java
   @Controller
   public class AjaxController {
       @RequestMapping("/a1")
       public void ajax1(String name , HttpServletResponse response) throws
           IOException {
           if ("admin".equals(name)){
               response.getWriter().print("true");
           }else{
               response.getWriter().print("false");
           }
       }
   }
   ```

4. 导入jquery ，可以使用在线的CDN ，也可以下载导入(**将刚才下载的文件放到statics/js目录下)**

   ![image-20200620212650079](.\Ajax和JSON.assets\image-20200620212650079-1595425401177.png)

   ```html
   <script src="https://code.jquery.com/jquery-3.5.1.js"></script>
   <script src="${pageContext.request.contextPath}/statics/js/jquery-3.5.1.js"></script>
   ```

5. 编写index.jsp测试

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
       <head>
           <title>$Title$</title>
           <%--<script src="https://code.jquery.com/jquery-3.1.1.min.js">
       </script>--%>
           <script src="${pageContext.request.contextPath}/statics/js/jquery-
                        3.1.1.min.js"></script>
           <script>
               function a1(){
                   $.post({
                       url:"${pageContext.request.contextPath}/a1",
                       data:{'name':$("#txtName").val()},
                       success:function (data,status) {
                           alert(data);
                           alert(status);
                       }
                   });
               }
           </script>
       </head>
       <body>
           <%--onblur：失去焦点触发事件--%>
           用户名:<input type="text" id="txtName" onblur="a1()"/>
       </body>
   </html>
   ```

6. 启动tomcat测试！ 

   ![image-20200620212159832](.\Ajax和JSON.assets\image-20200620212159832-1595425401177.png)

7. 打开浏览器的控制台，当我们鼠标离开输入框的时候，可以看到发出了一个ajax的请求！是后台返回给我们的结果！测试成功！

   ![image-20200620212634151](.\Ajax和JSON.assets\image-20200620212634151-1595425401177.png)

### 2.4、Springmvc实现

1. 实体类user

   ```java
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class User {
       private String name;
       private int age;
       private String sex;
   }
   ```

2. 我们来获取一个集合对象，展示到前端页面

   ```java
   @RequestMapping("a2")
   public List<User> ajax2(){
       List<User> users = new ArrayList<User>();
       users.add(new User("晓江1号", 20, "男"));
       users.add(new User("晓江2号", 20, "男"));
       users.add(new User("晓江3号", 20, "男"));
       users.add(new User("晓江4号", 20, "男"));
       //由于@RestController注解，将list转成json格式返回
       return users;
   }
   ```

3. 前端页面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
       <script src="${pageContext.request.contextPath}/statics/js/jquery-3.5.1.js"></script>
       <script>
           $(function () {
               $("#btn").click(function () {
                   $.post("${pageContext.request.contextPath}/a2",function (data) {
                       console.log(data)
                       var html="";
                       for (var i = 0; i <data.length ; i++) {
                           html+= "<tr>" +
                               "<td>" + data[i].name + "</td>" +
                               "<td>" + data[i].age + "</td>" +
                               "<td>" + data[i].sex + "</td>" +
                               "</tr>"
                       }
                       $("#content").html(html);
                   });
               })
           })
       </script>
   </head>
   <body>
       <input type="button" id="btn" value="获取数据" />
       <table width="80%" align="center">
           <tr>
               <td>姓名</td>
               <td>年龄</td>
               <td>性别</td>
           </tr>
           <tbody id="content">
           </tbody>
       </table>
   </body>
   </html>
   ```

4. 测试

   ![image-20200620220239744](.\Ajax和JSON.assets\image-20200620220239744-1595425401177.png)

### 2.5、注册提示效果

我们再测试一个小Demo，思考一下我们平时注册时候，输入框后面的实时提示怎么做到的；如何优化

1. 我们写一个Controller

   ```java
   @RequestMapping("/a3")
   public String ajax3(String name,String pwd){
       String msg = "";
       //模拟数据库中存在数据
       if (name!=null){
           if ("admin".equals(name)){
               msg = "OK";
           }else {
               msg = "用户名输入错误";
           }
       }
       if (pwd!=null){
           if ("123456".equals(pwd)){
               msg = "OK";
           }else {
               msg = "密码输入有误";
           }
       }
       return msg; //由于@RestController注解，将msg转成json格式返回
   }
   ```

2. 前端页面 login.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
       <head>
           <title>ajax</title>
           <script src="${pageContext.request.contextPath}/statics/js/jquery-
                        3.1.1.min.js"></script>
           <script>
               function a1(){
                   $.post({
                       url:"${pageContext.request.contextPath}/a3",
                       data:{'name':$("#name").val()},
                       success:function (data) {
                           if (data.toString()=='OK'){
                               $("#userInfo").css("color","green");
                           }else {
                               $("#userInfo").css("color","red");
                           }
                           $("#userInfo").html(data);
                       }
                   });
               }
               function a2(){
                   $.post({
                       url:"${pageContext.request.contextPath}/a3",
                       data:{'pwd':$("#pwd").val()},
                       success:function (data) {
                           if (data.toString()=='OK'){
                               $("#pwdInfo").css("color","green");
                           }else {
                               $("#pwdInfo").css("color","red");
                           }
                           $("#pwdInfo").html(data);
                       }
                   });
               }
           </script>
       </head>
       <body>
           <p>
               用户名:<input type="text" id="name" onblur="a1()"/>
               <span id="userInfo"></span>
           </p>
           <p>
               密码:<input type="text" id="pwd" onblur="a2()"/>
               <span id="pwdInfo"></span>
           </p>
       </body>
   </html>
   ```

   【记得处理JSON的乱码问题，在SpringMVC中导入处理】

3. 测试效果

### 2.6、获取baidu接口Demo

```html
<!DOCTYPE HTML>
<html>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
        <title>JSONP百度搜索</title>
        <style>
            #q{
                width: 500px;
                height: 30px;
                border:1px solid #ddd;
                line-height: 30px;
                display: block;
                margin: 0 auto;
                padding: 0 10px;
                font-size: 14px;
            }
            #ul{
                width: 520px;
                list-style: none;
                margin: 0 auto;
                padding: 0;
                border:1px solid #ddd;
                margin-top: -1px;
                display: none;
            }
            #ul li{
                line-height: 30px;
                padding: 0 10px;
            }
            #ul li:hover{
                background-color: #f60;
                color: #fff;
            }
        </style>
        <script>
            // 2.步骤二
            // 定义demo函数 (分析接口、数据)
            function demo(data){
                var Ul = document.getElementById('ul');
                var html = '';
                // 如果搜索数据存在 把内容添加进去
                if (data.s.length) {
                    // 隐藏掉的ul显示出来
                    Ul.style.display = 'block';
                    // 搜索到的数据循环追加到li里
                    for(var i = 0;i<data.s.length;i++){
                        html += '<li>'+data.s[i]+'</li>';
                    }
                    // 循环的li写入ul
                    Ul.innerHTML = html;
                }
            }
            // 1.步骤一
            window.onload = function(){
                // 获取输入框和ul
                var Q = document.getElementById('q');
                var Ul = document.getElementById('ul');
                // 事件鼠标抬起时候
                Q.onkeyup = function(){
                    // 如果输入框不等于空
                    if (this.value != '') {
                        // ☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆JSONPz重点☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆
                        // 创建标签
                        var script = document.createElement('script');
                        //给定要跨域的地址 赋值给src
                        //这里是要请求的跨域的地址 我写的是百度搜索的跨域地址
                        script.src =
                            'https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?
                        wd='+this.value+'&cb=demo';
                        // 将组合好的带src的script标签追加到body里
                        document.body.appendChild(script);
                    }
                }
            }
        </script>
    </head>
    <body>
        <input type="text" id="q" />
        <ul id="ul">
        </ul>
    </body>
</html>
```









