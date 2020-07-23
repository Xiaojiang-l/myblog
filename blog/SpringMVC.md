# SpringMVC

## 基本配置

1. 新建普通Maven项目作为父类

2. 在项目中创建模块，并添加web框架支持

3. 配置web.xml文件

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

4. 创建SpringMVC-Servlet.xml配置文件（一般使用注解版）

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
   
       <!-- 视图解析器 -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
           <!-- 前缀 -->
           <property name="prefix" value="/WEB-INF/jsp/" />
           <!-- 后缀 -->
           <property name="suffix" value=".jsp" />
       </bean>
   </beans>
   ```

5. 编写控制类Controller

   ```java
   package com.xj.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.ui.Model;
   import org.springframework.web.bind.annotation.RequestMapping;
   
   @Controller
   public class MyTest {
   
       @RequestMapping("h1")
       public String test(Model model){
           model.addAttribute("msg", "hello, SpringMVC");
           return "hello";
       }
   
       @RequestMapping("h2")
       public String test2(Model model){
           model.addAttribute("msg", "MyTest02");
           // 转发到/WEB-INF/jsp/目录下
           return "hello";
       }
   }
   ```

6. 配置Tomcat，**并在WEB-INF目录下创建lib并导入相应的包（不然报404错误）**

**如果使用注解@ResponseBody会不经过视图解析器处理**

## 1、回顾MVC

### 1.1、什么是MVC

- MVC是模型(Model)、视图(View)、控制器(Controller)的简写，是一种软件设计规范。

- 是将业务逻辑、数据、显示分离的方法来组织代码。

- MVC主要作用是降低了视图与业务逻辑间的双向偶合。

- MVC不是一种设计模式，MVC是一种架构模式。当然不同的MVC存在差异。

**Model（模型）**：数据模型，提供要展示的数据，因此包含数据和行为，可以认为是领域模型或 JavaBean组件（包含数据和行为），不过现在一般都分离开来：Value Object（数据Dao） 和 服务层
（行为Service）。也就是模型提供了模型数据查询和模型数据的状态更新等功能，包括数据和业务。

 **View（视图）**：负责进行模型的展示，一般就是我们见到的用户界面，客户想看到的东西。
 **Controller（控制器）**：接收用户请求，委托给模型进行处理（状态改变），处理完毕后把返回的模型数据返回给视图，由视图负责展示。 也就是说控制器做了个调度员的工作。

**最典型的MVC就是JSP + servlet + javabean的模式。**

###   1.2、Model1时代

- 在web早期的开发中，通常采用的都是Model1。

- Model1中，主要分为两层，视图层和模型层。

![image-20200613180014634](.\SpringMVC.assets\image-20200613180014634.png)

### 1.3、Model2时代

Model2把一个项目分成三部分，包括视图、控制、模型。

1. 用户发请求
2. Servlet接收请求数据，并调用对应的业务逻辑方法
3. 业务处理完毕，返回更新后的数据给servlet
4. servlet转向到JSP，由JSP来渲染页面
5. 响应给前端更新后的页面

**职责分析：**
**Controller：控制器**

1. 取得表单数据
2. 调用业务逻辑
3. 转向指定的页面

**Model：模型**

1. 业务逻辑

2. 保存数据的状态


**View：视图**

1. 显示页面

Model2这样不仅提高的代码的复用率与项目的扩展性，且大大降低了项目的维护成本。Model 1模式的实现比较简单，适用于快速开发小规模项目，Model1中JSP页面身兼View和Controller两种角色，将控制逻辑和表现逻辑混杂在一起，从而导致代码的重用性非常低，增加了应用的扩展性和维护的难度。Model2消除了Model1的缺点。



### 1.4、回顾Servlet

1. 新建一个普通Maven工程当父项目，导入pom依赖

   ```xml
   <dependencies>
       <dependency>
           <groupId>junit</groupId>
           <artifactId>junit</artifactId>
           <version>4.12</version>
       </dependency>
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-webmvc</artifactId>
           <version>5.1.9.RELEASE</version>
       </dependency>
       <dependency>
           <groupId>javax.servlet</groupId>
           <artifactId>servlet-api</artifactId>
           <version>2.5</version>
       </dependency>
       <dependency>
           <groupId>javax.servlet.jsp</groupId>
           <artifactId>jsp-api</artifactId>
           <version>2.2</version>
       </dependency>
       <dependency>
           <groupId>javax.servlet</groupId>
           <artifactId>jstl</artifactId>
           <version>1.2</version>
       </dependency>
   </dependencies>
   ```

2. 建立一个Model，添加Web app的支持！

   ![image-20200613182259569](.\SpringMVC.assets\image-20200613182259569.png)

3. 需要有导入servlet 和 jsp 的 jar 依赖

   ```xml
   <dependency>
       <groupId>javax.servlet</groupId>
       <artifactId>servlet-api</artifactId>
       <version>2.5</version>
   </dependency>
   <dependency>
       <groupId>javax.servlet.jsp</groupId>
       <artifactId>jsp-api</artifactId>
       <version>2.2</version>
   </dependency>
   ```

4. 编写Servlet类，处理用户请求

   ```java
   package com.xj.servlet;
   
   import javax.servlet.ServletException;
   import javax.servlet.http.HttpServlet;
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   
   public class HelloServlet extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           //取得参数
           String method = req.getParameter("method");
           if (method.equals("add")){
               req.getSession().setAttribute("msg","执行了add方法");
           }
           if (method.equals("delete")){
               req.getSession().setAttribute("msg","执行了delete方法");
           }
           //业务逻辑
           //视图跳转
           req.getRequestDispatcher("/WEB-INF/jsp/hello.jsp").forward(req,resp);
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req, resp);
       }
   }
   ```

5. 编写Hello.jsp，在WEB-INF目录下新建一个jsp的文件夹，新建hello.jsp

   (在WEB-INF目录下新建的文件属于安全的，不公开)

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
       <head>
           <title>lxj</title>
       </head>
       <body>
           ${msg}
       </body>
   </html>
   ```

6. 在web.xml中注册Servlet

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <servlet>
           <servlet-name>HelloServlet</servlet-name>
           <servlet-class>com.xj.servlet.HelloServlet</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>HelloServlet</servlet-name>
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
   </web-app>
   ```

7. 配置Tomcat，并启动测试

   - ![image-20200613182403983](.\SpringMVC.assets\image-20200613182403983.png)
   - http://localhost:8080/hello?method=add
   - http://localhost:8080/hello?method=delete



**MVC框架要做哪些事情**

1. 将url映射到java类或java类的方法 .
2. 封装用户提交的数据 .
3. 处理请求--调用相关的业务处理--封装响应数据 .
4. 将响应的数据进行渲染 . jsp / html 等表示层数据 .

**说明：**
    常见的服务器端MVC框架有：Struts、Spring MVC、ASP.NET MVC、Zend Framework、JSF；常见前端
MVC框架：vue、angularjs、react、backbone；由MVC演化出了另外一些模式如：MVP、MVVM 等
等....



## 2、什么是SpringMVC

### 2.1、概述

![image-20200613183016033](.\SpringMVC.assets\image-20200613183016033.png)

Spring MVC是Spring Framework的一部分，是基于Java实现MVC的轻量级Web框架。
**查看官方文档**：https://docs.spring.io/spring/docs/5.2.7.RELEASE/spring-framework-reference/web.html#spring-web

我们为什么要学习SpringMVC呢?
**Spring MVC的特点**：

1. 轻量级，简单易学

2. 高效 , 基于请求响应的MVC框架

3. 与Spring兼容性好，无缝结合

4. 约定优于配置

5. 功能强大：RESTful、数据验证、格式化、本地化、主题等

6. 简洁灵活

Spring的web框架围绕**DispatcherServlet** [ 调度Servlet ] 设计。

DispatcherServlet的作用是将请求分发到不同的处理器。从Spring 2.5开始，使用Java 5或者以上版本的
用户可以采用基于注解形式进行开发，十分简洁；
正因为SpringMVC好 , 简单 , 便捷 , 易学 , 天生和Spring无缝集成(使用SpringIoC和Aop, 使用约定优于
配置 . 能够进行简单的junit测试 . 支持Restful风格 .异常处理 , 本地化 , 国际化 , 数据验证 , 类型转换 , 拦
截器 等等......所以我们要学习 。
**最重要的一点还是用的人多 , 使用的公司多 。**



### 2.2、中心控制器

Spring的web框架围绕**DispatcherServlet**设计。 DispatcherServlet的作用是将请求分发到不同的处理
器。从Spring 2.5开始，使用Java 5或者以上版本的用户可以采用**基于注解的controller**声明方式。
Spring MVC框架像许多其他MVC框架一样, **以请求为驱动 , 围绕一个中心Servlet分派请求及提供其他功**
**能，DispatcherServlet是一个实际的Servlet (它继承自HttpServlet 基类)。**



### 2.3、SpringMVC执行原理

![image-20200613183445431](.\SpringMVC.assets\image-20200613183445431.png)

图为SpringMVC的一个较完整的流程图，实线表示SpringMVC框架提供的技术，不需要开发者实现，虚
线表示需要开发者实现。

**简要分析执行流程**

1. DispatcherServlet表示前置控制器，是整个SpringMVC的控制中心。用户发出请求，
    DispatcherServlet接收请求并拦截请求。

  我们假设请求的url为 : http://localhost:8080/SpringMVC/hello
  如上url拆分成三部分：
  http://localhost:8080服务器域名
  SpringMVC部署在服务器上的web站点
  hello表示控制器
  通过分析，如上url表示为：请求位于服务器localhost:8080上的SpringMVC站点的hello控制器。

2. ==HandlerMapping为处理器映射==。DispatcherServlet调用HandlerMapping,HandlerMapping根据
    请求url查找Handler。

3. HandlerExecution表示具体的Handler,其主要作用是根据url查找控制器，如上url被查找控制器
    为：hello。

4. HandlerExecution将解析后的信息传递给DispatcherServlet,如解析控制器映射等。

5. ==HandlerAdapter表示处理器适配器==，其按照特定的规则去执行Handler。

6. Handler让具体的Controller执行。

7. Controller将具体的执行信息返回给HandlerAdapter,如ModelAndView。

8. HandlerAdapter将视图逻辑名或模型传递给DispatcherServlet。

9. DispatcherServlet调用视图解析器(ViewResolver)来解析HandlerAdapter传递的逻辑视图名。

10. ==视图解析器==将解析的逻辑视图名传给DispatcherServlet。

11. DispatcherServlet根据视图解析器解析的视图结果，调用具体的视图。

12. 最终视图呈现给用户。



## 3、HelloSpring

### 3.1、配置版

1. 新建一个Moudle ， springmvc_02_hello ， 添加web的支持！

2. 确定导入了SpringMVC 的依赖！(父项目中导入即可)

   ```xml
   <dependencies>
       <dependency>
           <groupId>junit</groupId>
           <artifactId>junit</artifactId>
           <version>4.12</version>
       </dependency>
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-webmvc</artifactId>
           <version>5.1.9.RELEASE</version>
       </dependency>
       <dependency>
           <groupId>javax.servlet</groupId>
           <artifactId>servlet-api</artifactId>
           <version>2.5</version>
       </dependency>
       <dependency>
           <groupId>javax.servlet.jsp</groupId>
           <artifactId>jsp-api</artifactId>
           <version>2.2</version>
       </dependency>
       <dependency>
           <groupId>javax.servlet</groupId>
           <artifactId>jstl</artifactId>
           <version>1.2</version>
       </dependency>
   </dependencies>
   ```

3. 配置web.xml ， 注册DispatcherServlet

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
   </web-app>
   ```

4. 编写SpringMVC 的 配置文件！名称：springmvc-servlet.xml : [servletname]-servlet.xml
   说明，这里的名称要求是按照官方来的

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">
       
   </beans>
   ```

5. 添加 处理映射器

   ```xml
   <!--添加处理映射器-->
   <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
   ```

6. 添加 处理器适配器

   ```xml
   <!--添加处理适配器-->
   <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
   ```

7. 添加视图解析器

   ```xml
   <!--视图解析器:DispatcherServlet给他的ModelAndView-->
   <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
       <!--前缀-->
       <property name="prefix" value="/WEB-INF/jsp/"/>
       <!--后缀-->
       <property name="suffix" value=".jsp"/>
   </bean>
   ```

8. 编写我们要操作业务Controller ，要么实现Controller接口，要么增加注解；需要返回一个
   ModelAndView，装数据，封视图；

   ```java
   package com.xj.controller;
   
   import org.springframework.web.servlet.ModelAndView;
   import org.springframework.web.servlet.mvc.Controller;
   
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   
   //注意：这里我们先导入Controller接口
   public class HelloController implements Controller {
       public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
           //ModelAndView 模型和视图
           ModelAndView mv = new ModelAndView();
           //封装对象，放在ModelAndView中。Model
           mv.addObject("msg","HelloSpringMVC!");
           //封装要跳转的视图，放在ModelAndView中
           mv.setViewName("hello"); //: /WEB-INF/jsp/hello.jsp
           return mv;
       }
   }
   ```

   ![image-20200613190316535](.\SpringMVC.assets\image-20200613190316535.png)

9. 将自己的类交给SpringIOC容器，注册bean

   ```xml
   <!--Handler-->
   <bean id="/hello" class="com.xj.controller.HelloController"/>
   ```

10. 写要跳转的jsp页面，显示ModelandView存放的数据，以及我们的正常页面；

    ```jsp
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
        <head>
            <title>Title</title>
        </head>
        <body>
            ${msg}
        </body>
    </html>
    ```

11. 配置Tomcat 启动测试！

    ![image-20200613191403510](.\SpringMVC.assets\image-20200613191403510.png)

**可能遇到的问题：访问出现404，排查步骤：**

1. 查看控制台输出，看一下是不是缺少了什么jar包。
2. 如果jar包存在，显示无法输出，就在IDEA的项目发布中，添加lib依赖！
3. 重启Tomcat 即可解决！

![image-20200613191715216](.\SpringMVC.assets\image-20200613191715216.png)

![image-20200613194922117](.\SpringMVC.assets\image-20200613194922117.png)

**小结**：实际开发才不会这么写，不然就疯了.注解版实现，才是SpringMVC的精髓。



### 3.2、注解版

1. 新建一个Moudle，springmvc_03_annotation。添加web支持！

2. 由于Maven可能存在资源过滤的问题，我们将配置完善

   ```xml
   <build>
       <resources>
           <resource>
               <directory>src/main/java</directory>
               <includes>
                   <include>**/*.properties</include>
                   <include>**/*.xml</include>
               </includes>
               <filtering>false</filtering>
           </resource>
           <resource>
               <directory>src/main/resources</directory>
               <includes>
                   <include>**/*.properties</include>
                   <include>**/*.xml</include>
               </includes>
               <filtering>false</filtering>
           </resource>
       </resources>
   </build>
   ```

3. 在pom.xml文件引入相关的依赖：主要有Spring框架核心库、Spring MVC、servlet , JSTL等。我们
   在父依赖中已经引入了！

4. 配置web.xml

   **注意点：**

   - 注意web.xml版本问题，要最新版！
   - 注册DispatcherServlet
   - 关联SpringMVC的配置文件
   - 启动级别为1
   - 映射路径为 /【不要使用/*不然会报错404】

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <!--1.注册servlet-->
       <servlet>
           <servlet-name>SpringMVC</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:SpringMVC-Servlet.xml</param-value>
           </init-param>
           <!-- 启动顺序，数字越小，启动越早 -->
           <load-on-startup>1</load-on-startup>
       </servlet>
       <!--所有请求都会被SpringMVC拦截(除了.jsp) -->
       <servlet-mapping>
           <servlet-name>SpringMVC</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   </web-app>
   ```
   

**/ 和 /* 的区别**：

-  < url-pattern > / </ url-pattern > 不会匹配到.jsp， 只针对我们编写的请求；
     即：.jsp 不会进入spring的 DispatcherServlet类 。 
   
- < url-pattern > /* </ url-pattern > 会匹配*.jsp， 会出现返回 jsp视图 时再次进入spring的DispatcherServlet 类，导致找不到对应的controller所以报404错。
  
5. 添加Spring MVC配置文件

   - 让IOC的注解生效
   - 静态资源过滤 ：HTML . JS . CSS . 图片 ， 视频 .....
   - MVC的注解驱动
   - 配置视图解析器

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
       <!--
           支持mvc注解驱动
           在spring中一般采用@RequestMapping注解来完成映射关系
           要想使@RequestMapping注解生效
           必须向上下文中注册DefaultAnnotationHandlerMapping
           和一个AnnotationMethodHandlerAdapter实例
           这两个实例分别在类级别和方法级别处理。
           而annotation-driven配置帮助我们自动完成上述两个实例的注入。
       -->
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

   在视图解析器中我们把所有的视图都存放在/WEB-INF/目录下，这样可以**保证视图安全，因为这个**
   **目录下的文件，客户端不能直接访问。**

6. 创建Controller

   ```java
   @Controller
   @RequestMapping("/HelloController")
   public class HelloController {
       @RequestMapping("/hello")
       public String sayHello(Model model){
           // 向模型中添加属性msg与值，可以在jsp页面中取出并渲染
           model.addAttribute("msg", "hello,SpringMVC");
           // 转发到web-inf/jsp/hello.jsp
           return "hello";
       }
   }
   ```

   - @Controller是为了让Spring IOC容器初始化时自动扫描到；
   - @RequestMapping是为了映射请求路径，这里因为类与方法上都有映射所以访问时应该
     是/HelloController/hello；
   - 方法中声明Model类型的参数是为了把Action中的数据带到视图中；
   - 方法返回的结果是视图的名称hello，加上配置文件中的前后缀变成WEB-INF/jsp/**hello**.jsp。

7. 创建视图层

   在WEB-INF/ jsp目录中创建hello.jsp ， 视图可以直接取出并展示从Controller带回的信息；
   可以通过EL表示取出Model中存放的值，或者对象；

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
       <head>
           <title>Title</title>
       </head>
       <body>
           ${msg}
       </body>
   </html>
   
   ```

8. 配置Tomcat运行
   配置Tomcat ， 开启服务器 ， 访问对应的请求路径！

   ![image-20200613211448132](.\SpringMVC.assets\image-20200613211448132.png)

### 3.3、小结

实现步骤其实非常的简单：

1. 新建一个web项目
2. 导入相关jar包
3. 编写web.xml , 注册DispatcherServlet
4. 编写springmvc配置文件
5. 接下来就是去创建对应的控制类 , controller
6. 最后完善前端视图和controller之间的对应
7. 测试运行调试.
使用springMVC必须配置的三大件：
**处理器映射器**、**处理器适配器**、**视图解析器**
通常，我们只需要**手动配置视图解析器**，而**处理器映射器**和**处理器适配器**只需要**开启注解驱动**即可，而
省去了大段的xml配置



## 4、Controller 及 RestFul

### 4.1、控制器Controller

- 控制器复杂提供访问应用程序的行为，通常通过接口定义或注解定义两种方法实现。
- 控制器负责解析用户的请求并将其转换为一个模型。
- 在Spring MVC中一个控制器类可以包含多个方法
- 在Spring MVC中，对于Controller的配置方式有很多种

我们来看看有哪些方式可以实现：

1. 实现Controller接口

   - 需要实现接口方法，获得ModelAndView，并返回给解析器

   - 并且需要在springmvc-servlet.xml中配置虚拟域名

   ```xml
   <bean id="/hello" class="com.xj.controller.HelloController"/>
   ```

   ```java
   public class HelloController implements Controller {
       public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
           //ModelAndView 模型和视图
           ModelAndView mv = new ModelAndView();
           //封装对象，放在ModelAndView中。Model
           mv.addObject("msg","HelloSpringMVC!");
           //封装要跳转的视图，放在ModelAndView中
           mv.setViewName("hello"); //: /WEB-INF/jsp/hello.jsp
           // 需要返回模型和视图给解析器
           return mv;
       }
   }
   ```

2. 使用Controller注解

   - 可以自定义方法（可复用），需要`@Controller`和`@RequestMapping`注解

   - 同时可以定义虚拟域名localhost8080：/hello
   - `@Controller`定义该类为Controller
   - `@RequestMapping`用于映射url到控制器类或一个特定的处理程序方法。可用于类或方法上。
     用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。
   
   ```java
   @Controller
   public class HelloController {
       @RequestMapping("/hello")
       public String sayHello(Model model){
           // 向模型中添加属性msg与值，可以在jsp页面中取出并渲染
           model.addAttribute("msg", "hello,SpringMVC");
           // 转发到web-inf/jsp/hello.jsp
           return "hello";
       }
   }
   ```



### 4.2、RestFul 风格

- **概念**

  Restful就是一个资源定位及资源操作的风格。不是标准也不是协议，只是一种风格。基于这个风格
  设计的软件可以更简洁，更有层次，更易于实现缓存等机制。

- **功能资源**：互联网所有的事物都可以被抽象为资源 资源操作：使用POST、DELETE、PUT、GET，使用
    不同方法对资源进行操作。 分别对应 添加、 删除、修改、查询。

- **传统方式操作资源** ：通过不同的参数来实现不同的效果！方法单一，post 和 get

  ```java
  http://127.0.0.1/item/queryItem.action?id=1 查询,GET 
  http://127.0.0.1/item/saveItem.action 新增,POST 
  http://127.0.0.1/item/updateItem.action 更新,POST 
  http://127.0.0.1/item/deleteItem.action?id=1 删除,GET或POST
  ```

- **使用RESTful操作资源** ： 可以通过不同的请求方式来实现不同的效果！如下：请求地址一样，但是功能可以不同！(对url进行复用)；**使用不同的请求方法访问同一个url会有不同效果**

  ```java
  // 带参数的
  http://127.0.0.1/item/1 查询,GET 
  	@GetMapping("/item/{p}")
  http://127.0.0.1/item/1 删除,DELETE
  	@DeleteMapping("/item/{p}")
  
  // 不带参数的
  http://127.0.0.1/item 新增,POST 
  	@PostMapping("/item")
  http://127.0.0.1/item 更新,PUT
  	@PutMapping("/item")
  ```

- **作用**：隐藏了url中提交的属性名，降低辨别度，更加安全！



#### **普通使用**

1. 在新建一个类 RestFulController

   ```java
   @Controller
   public class RestFulController {
       @RequestMapping("/commit/{p1}/{p2}")
       public String index(@PathVariable int p1,@PathVariable int p2, Model model){
           int result = p1 + p2;
           model.addAttribute("msg", "结果：" + result);
           // 返回视图位置
           return "hello";
       }
   }
   ```
   

注意：需要对方法中的参数使用注解@PathVariable修饰

2. 测试

   ![image-20200613231244135](.\SpringMVC.assets\image-20200613231244135.png)

**思考：使用路径变量的好处？**

- 使路径变得更加简洁；
- 获得参数更加方便，框架会自动进行类型转换。
- 通过路径变量的类型可以约束访问参数，如果类型不一样，则访问不到对应的请求方法，如这里访问是的路径是/commit/1/a，**则路径与方法不匹配，而不会是参数转换失败**。

![image-20200613231938701](.\SpringMVC.assets\image-20200613231938701.png)



#### **使用method属性指定请求类型**

用于约束请求的类型，可以收窄请求范围。指定请求谓词的类型如GET, POST, HEAD, OPTIONS, PUT,
PATCH, DELETE, TRACE等
我们来测试一下：

- 增加一个方法

  ```java
  //映射访问路径,必须是POST请求
  @RequestMapping(value = "/hello",method = {RequestMethod.POST})
  public String index2(Model model){
      model.addAttribute("msg", "hello!");
      return "hello";
  }
  ```

- 我们使用浏览器地址栏进行访问默认是Get请求，会出现405错误

  ![image-20200613232316342](.\SpringMVC.assets\image-20200613232316342.png)

- 如果将POST修改为GET则正常了

  ```java
  //映射访问路径,必须是GET请求
  @RequestMapping(value = "/hello",method = {RequestMethod.GET})
  public String index2(Model model){
      model.addAttribute("msg", "hello!");
      return "hello";
  }
  ```



#### **小结：**

- Spring MVC 的 @RequestMapping 注解能够处理 HTTP 请求的方法, 比如 GET, PUT, POST, DELETE 以
  及 PATCH。

- 所有的地址栏请求默认都会是 HTTP GET 类型的。

- 方法级别的注解变体有如下几个： 组合注解

  ```java
  @GetMapping			// 设置该地址为GET方法
  @PostMapping		// 设置该地址为Post方法
  @PutMapping			// 设置该地址为Put方法
  @DeleteMapping		// 设置该地址为Delete方法
  @PatchMapping		// 设置该地址为Patch方法
  ```



## 5、结果跳转方式

### 5.1、ModelAndView

设置ModelAndView对象 , 根据view的名称 , 和视图解析器跳到指定的页面 .
**页面** : {视图解析器前缀} + viewName +{视图解析器后缀}

```xml
<!-- 视图解析器 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
    <!-- 前缀 -->
    <property name="prefix" value="/WEB-INF/jsp/" />
    <!-- 后缀 -->
    <property name="suffix" value=".jsp" />
</bean>
```

**对应的Controller类**

```java
public class ControllerTest1 implements Controller {
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        //返回一个模型视图对象
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","ControllerTest1");
        // 设置跳转的页面
        mv.setViewName("hello");
        return mv;
    }
}
```

**并需要注册Handler**

```xml
<!--注册Handler-->
<bean id="/t2" class="com.xj.controller.MyTest02"/>
```



### 5.2、ServletAPI

通过设置ServletAPI , 不需要视图解析器 

1. 通过**HttpServletResponse**进行输出

2. 通过**HttpServletResponse**实现重定向

3. 通过**HttpServletRequest**实现转发

   需要获取HttpServletRequest req, HttpServletResponse rsp对象

```java
@Controller
public class ResultGo {
    @RequestMapping("/result/t1")
    public void test1(HttpServletRequest req, HttpServletResponse rsp)
        throws IOException {
        // 输出
        rsp.getWriter().println("Hello,Spring BY servlet API");
    }
    @RequestMapping("/result/t2")
    public void test2(HttpServletRequest req, HttpServletResponse rsp)
        throws IOException {
        // 重定向
        rsp.sendRedirect("/index.jsp");
    }
    @RequestMapping("/result/t3")
    public void test3(HttpServletRequest req, HttpServletResponse rsp)
        throws Exception {
        //转发
        req.setAttribute("msg","/result/t3");
        req.getRequestDispatcher("/WEB-INF/jsp/test.jsp").forward(req,rsp);
    }
}
```

### 5.3、SpringMVC

**通过SpringMVC来实现转发和重定向 - 无需视图解析器；**

- 但没有视图解析器的时候，需要写出完整的页面路径（自行拼接）
- renturn默认功能为请求转发，但加上`redirect:`可以实现重定向
- 转发：url不会改变
- 重定向：url会改变

```java
@Controller
public class ResultSpringMVC {
    @RequestMapping("/rsm/t1")
    public String test1(){
        //转发
        return "/index.jsp";
    }
    @RequestMapping("/rsm/t2")
    public String test2(){
        //转发二
        return "forward:/index.jsp";
    }
    @RequestMapping("/rsm/t3")
    public String test3(){
        //重定向
        return "redirect:/index.jsp";
    }
}
```



**通过SpringMVC来实现转发和重定向 - 有视图解析器；**

- 有视图解析器的时候，转发必须简写，不能增加前缀和后缀
- 重定向 , 不需要视图解析器 , 本质就是重新请求一个新地方嘛 , 所以**注意路径问题**

```java
@Controller
public class ResultSpringMVC2 {
    @RequestMapping("/rsm2/t1")
    public String test1(){
        //转发
        return "test";
    }
    @RequestMapping("/rsm2/t2")
    public String test2(){
        //重定向
        return "redirect:/index.jsp";
        //return "redirect:hello.do"; //hello.do为另一个请求/
    }
}
```



## 6、数据处理

### 6.1、处理提交数据

1. **提交的域名称和处理方法的参数名一致**

   提交数据 : http://localhost:8080/hello?name=xj
   处理方法 :

   ```java
   @RequestMapping("/hello")
   public String hello(String name){
       System.out.println(name);
       return "hello";
   }
   ```

   后台输出 : xj

   

2. **提交的域名称和处理方法的参数名不一致**

   提交数据 : http://localhost:8080/hello?username=xj
   处理方法 :需要在参数前面使用注解`@RequestParam("XXX")`修饰，**页面会提示参数名称**

   ```java
   //@RequestParam("username") : username提交的域的名称 .
   @RequestMapping("/hello")
   public String hello(@RequestParam("username") String name){
       System.out.println(name);
       return "hello";
   }
   ```

   ![image-20200614112623818](.\SpringMVC.assets\image-20200614112623818.png)

3. **提交的是一个对象**

   要求提交的表单域和对象的属性名一致 , 参数使用对象即可

   1. 实体类

      ```java
      @Data
      @NoArgsConstructor
      @AllArgsConstructor
      public class User {
          private int id;
          private String name;
          private int age;
      }
      ```

   2. 提交数据 : http://localhost:8080/mvc04/user?name=xj&id=1&age=15

   3. 处理方法 :参数直接使用实体类对象

      ```java
      @RequestMapping("/user")
      public String user(User user){
          System.out.println(user);
          return "hello";
      }
      ```

      后台输出 : User { id=1, name=xj, age=15 }

   **说明：如果使用对象的话，前端传递的参数名和对象名必须一致，否则String类型输出null，int类型就输出0**

   

### 6.2、数据显示到前端

- **第一种 : 通过ModelAndView**

  ```java
  public class ControllerTest1 implements Controller {
      public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
          //返回一个模型视图对象
          ModelAndView mv = new ModelAndView();
          mv.addObject("msg","ControllerTest1");
          mv.setViewName("test");
          return mv;
      }
  }
  ```

- **第二种 : 通过ModelMap**

  ```java
  @RequestMapping("/hello")
  public String hello(@RequestParam("username") String name, ModelMap model){
      //封装要显示到视图中的数据
      //相当于req.setAttribute("name",name);
      model.addAttribute("name",name);
      System.out.println(name);
      return "hello";
  }
  ```

- **第三种 : 通过Model**

  ```java
  @RequestMapping("/ct2/hello")
  public String hello(@RequestParam("username") String name, Model model){
      //封装要显示到视图中的数据
      //相当于req.setAttribute("name",name);
      model.addAttribute("msg",name);
      System.out.println(name);
      return "test";
  }
  ```

- **对比**

  ```properties
  Model 只有寥寥几个方法只适合用于储存数据，简化了新手对于Model对象的操作和理解；
  
  ModelMap 继承了 LinkedMap ，除了实现了自身的一些方法，同样的继承 LinkedMap 的方法和特性；
  
  ModelAndView 可以在储存数据的同时，可以进行设置返回的逻辑视图，进行控制展示层的跳转。
  ```



### 6.3、乱码问题

测试步骤：

1. 编写一个提交表单

   ```jsp
   <form action="/e/t" method="post">
       <input type="text" name="name">
       <input type="submit">
   </form>
   ```

2. 编写处理类

   ```java
   @Controller
   public class Encoding {
       @RequestMapping("/e/t")
       public String test(Model model,String name){
           model.addAttribute("msg",name); //获取表单提交的值
           return "test"; //跳转到test页面显示输入的值
       }
   }
   ```

3. 输入中文测试，发现乱码

   ![image-20200614115144426](.\SpringMVC.assets\image-20200614115144426.png)

#### **解决方法：**

**(优先使用SpringMVC的方法)**

1. **自己创建过滤器，并在Web.xml中进行配置**

   ```java
   package com.xj.filter;
   
   import javax.servlet.*;
   import java.io.IOException;
   
   public class EncodingFilter implements Filter {
       public void init(FilterConfig filterConfig) throws ServletException {
   
       }
   
       public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
           servletRequest.setCharacterEncoding("utf-8");
           servletResponse.setCharacterEncoding("utf-8");
           // 放行
           filterChain.doFilter(servletRequest,servletResponse);
       }
   
       public void destroy() {
   
       }
   }
   ```

   ```xml
   <!--设置编码过滤器-->
   <filter>
       <filter-name>encoding</filter-name>
       <filter-class>com.xj.filter.EncodingFilter</filter-class>
   </filter>
   <filter-mapping>
       <filter-name>encoding</filter-name>
       <!--注意这里是拦截所有，包括jsp-->
       <url-pattern>/*</url-pattern>
   </filter-mapping>
   ```

2. **==如果自定义过滤器不能解决，使用SpringMVC自带的过滤器==**（**一般直接使用该方法**）

   ```xml
   <filter>
       <filter-name>encoding</filter-name>
       <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
       <init-param>
           <param-name>encoding</param-name>
           <param-value>utf-8</param-value>
       </init-param>
   </filter>
   <filter-mapping>
       <filter-name>encoding</filter-name>
       <url-pattern>/*</url-pattern>
   </filter-mapping>
   ```

   但是有些极端情况下,这个过滤器对get的支持不好 .

3. **当SpringMVC的过滤器使用不了时，使用该自定义过滤器**

   ```java
   package com.xj.filter;
   import javax.servlet.*;
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletRequestWrapper;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   import java.io.UnsupportedEncodingException;
   import java.util.Map;
   /**
    * 解决get和post请求 全部乱码的过滤器
    */
   public class GenericEncodingFilter implements Filter {
       public void destroy() {
       }
       public void doFilter(ServletRequest request, ServletResponse
               response, FilterChain chain) throws IOException, ServletException {
           //处理response的字符编码
           HttpServletResponse myResponse=(HttpServletResponse) response;
           myResponse.setContentType("text/html;charset=UTF-8");
           // 转型为与协议相关对象
           HttpServletRequest httpServletRequest = (HttpServletRequest)request;
           // 对request包装增强
           HttpServletRequest myrequest = new MyRequest(httpServletRequest);
           chain.doFilter(myrequest, response);
       }
       public void init(FilterConfig filterConfig) throws
               ServletException {
       }
   }
   //自定义request对象，HttpServletRequest的包装类
   class MyRequest extends HttpServletRequestWrapper {
       private HttpServletRequest request;
       //是否编码的标记
       private boolean hasEncode;
       //定义一个可以传入HttpServletRequest对象的构造函数，以便对其进行装饰
       public MyRequest(HttpServletRequest request) {
           super(request);// super必须写
           this.request = request;
       }
       // 对需要增强方法 进行覆盖
       public Map getParameterMap() {
           // 先获得请求方式
           String method = request.getMethod();
           if (method.equalsIgnoreCase("post")) {
               // post请求
               try {
                   // 处理post乱码
                   request.setCharacterEncoding("utf-8");
                   return request.getParameterMap();
               } catch (UnsupportedEncodingException e) {
                   e.printStackTrace();
               }
           } else if (method.equalsIgnoreCase("get")) {
               // get请求
               Map<String, String[]> parameterMap =
                       request.getParameterMap();
               if (!hasEncode) { // 确保get手动编码逻辑只运行一次
                   for (String parameterName : parameterMap.keySet()) {
                       String[] values = parameterMap.get(parameterName);
                       if (values != null) {
                           for (int i = 0; i < values.length; i++) {
                               try {
                                   // 处理get乱码
                                   values[i] = new String(values[i]
                                           .getBytes("ISO-8859-1"), "utf-8");
                               } catch (UnsupportedEncodingException e) {
                                   e.printStackTrace();
                               }
                           }
                       }
                   }
                   hasEncode = true;
               }
               return parameterMap;
           }
           return super.getParameterMap();
       }
       //取一个值
       public String getParameter(String name) {
           Map<String, String[]> parameterMap = getParameterMap();
           String[] values = parameterMap.get(name);
           if (values == null) {
               return null;
           }
           return values[0]; // 取回参数的第一个值
       }
       //取所有值
       public String[] getParameterValues(String name) {
           Map<String, String[]> parameterMap = getParameterMap();
           String[] values = parameterMap.get(name);
           return values;
       }
   }
   ```

   ```xml
   <filter>
       <filter-name>encoding</filter-name>
       <filter-class>com.xj.filter.GenericEncodingFilter</filter-class>
   </filter>
   <filter-mapping>
       <filter-name>encoding</filter-name>
       <url-pattern>/*</url-pattern>
   </filter-mapping>
   ```

   这是网上大神写的，**一般情况下，SpringMVC默认的乱码处理就已经能够很好的解决了！然后在web.xml中配置这个过滤器即可！**
   乱码问题，需要平时多注意，在尽可能能设置编码的地方，都设置为统一编码 UTF-8！

   - 同时Tomcat也需要有正确的设置

     ```xml
     <Connector URIEncoding="utf-8" port="8080" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" />
     ```



## 7、JSON

### 7.1、什么是JSON

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

   

### 7.2、Controller返回JSON数据

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

   ![image-20200614144143284](.\SpringMVC.assets\image-20200614144143284.png)

   - 发现出现了乱码问题，我们需要设置一下他的编码格式为utf-8，以及它返回的类型；
   - 通过@RequestMaping的produces属性来实现，修改下代码

   ```java
   //produces:指定响应体返回类型和编码
   @RequestMapping(value = "/json1",produces ="application/json;charset=utf-8")
   @ResponseBody
   ```

   - 测试结果正常

     ![image-20200614144502259](.\SpringMVC.assets\image-20200614144502259.png)

   - **如果没有使用注解@ResponseBody会经过视图解析器处理**

     ![image-20200614144613903](.\SpringMVC.assets\image-20200614144613903.png)

**【注意：使用json记得处理乱码问题】**



### 7.3、代码优化

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



### 7.4、测试集合输出

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

  ![image-20200614145904789](.\SpringMVC.assets\image-20200614145904789.png)



### 7.5、输出时间对象

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

  ![image-20200614150154130](.\SpringMVC.assets\image-20200614150154130.png)

- 默认日期格式会变成一个数字，**是1970年1月1日到当前日期的秒数**！Jackson 默认是会把时间转成timestamps形式

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

  ![image-20200614150639474](.\SpringMVC.assets\image-20200614150639474.png)



### 7.6、抽取为工具类

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

![image-20200614151100556](.\SpringMVC.assets\image-20200614151100556.png)



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

















