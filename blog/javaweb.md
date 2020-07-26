# JavaWeb

## 1、基本概念

### 1.2、静态web

- *.htm, *.html,这些都是网页的后缀，如果服务器上一直存在这些东西，我们就可以直接进行读取。通络；

![1567822802516](.\javaweb.assets\1567822802516.png)

- 静态web存在的缺点
  - Web页面无法动态更新，所有用户看到都是同一个页面
    - 轮播图，点击特效：伪动态
    - JavaScript [实际开发中，它用的最多]
    - VBScript
  - 它无法和数据库交互（数据无法持久化，用户无法交互）



### 1.2、动态web

页面会动态展示： “Web的页面展示的效果因人而异”；

![1567823191289](.\javaweb.assets\1567823191289.png)

缺点：

- 加入服务器的动态web资源出现了错误，我们需要重新编写我们的**后台程序**,重新发布；
  - 停机维护

优点：

- Web页面可以动态更新，所有用户看到都不是同一个页面
- 它可以与数据库交互 （数据持久化：注册，商品信息，用户信息........）

![1567823350584](.\javaweb.assets\1567823350584.png)



## 2、web服务器

### 2.1、技术讲解

**php：**

- PHP开发速度很快，功能很强大，跨平台，代码很简单 （70% , WP）
- 无法承载大访问量的情况（局限性）



**JSP/Servlet : ** 

B/S：浏览和服务器

C/S:  客户端和服务器

- sun公司主推的B/S架构
- 基于Java语言的 (所有的大公司，或者一些开源的组件，都是用Java写的)
- 可以承载三高问题带来的影响；
- 语法像ASP ， ASP-->JSP , 加强市场强度；

.....

### 2.2、web服务器

**Tomcat**

![1567824446428](.\javaweb.assets\1567824446428.png)

面向百度编程；

Tomcat是Apache 软件基金会（Apache Software Foundation）的Jakarta 项目中的一个核心项目，最新的Servlet 和JSP 规范总是能在Tomcat 中得到体现，因为Tomcat 技术先进、性能稳定，而且**免费**，因而深受Java 爱好者的喜爱并得到了部分软件开发商的认可，成为目前比较流行的Web 应用服务器。

Tomcat 服务器是一个免费的开放源代码的Web 应用服务器，属于轻量级应用[服务器](https://baike.baidu.com/item/服务器)，在中小型系统和并发访问用户不是很多的场合下被普遍使用，是开发和调试JSP 程序的首选。对于一个Java初学web的人来说，它是最佳的选择



## 3、Tomcat

### 3.1、安装tomcat

tomcat官网：http://tomcat.apache.org/

![1567825600842](.\javaweb.assets\1567825600842.png)

![1567825627138](.\javaweb.assets\1567825627138.png)



### 3.2、Tomcat启动和配置

文件夹作用：

![1567825763180](.\javaweb.assets\1567825763180.png)

**启动。关闭Tomcat**

![1567825840657](.\javaweb.assets\1567825840657.png)

访问测试：http://localhost:8080/

可能遇到的问题：

1. Java环境变量没有配置
2. 闪退问题：需要配置兼容性
3. 乱码问题：配置文件中设置

   tomcat窗口出现中文乱码，把tomcat的编码修改和系统一致就解决乱码问题，方法如下：
   - 找到apache-tomcat-7.0.92/conf/logging.properties
   - 添加语句：java.util.logging.ConsoleHandler.encoding = GBK

### 3.3、配置

![1567825967256](.\javaweb.assets\1567825967256.png)

可以配置启动的端口号

- tomcat的默认端口号为：8080
- mysql：3306
- http：80
- https：443

```xml
<Connector port="8081" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

可以配置主机的名称

- 默认的主机名为：localhost->127.0.0.1
- 默认网站应用存放的位置为：webapps

```xml
<Host name="www.qinjiang.com"  appBase="webapps"
      unpackWARs="true" autoDeploy="true">
```

#### 高难度面试题：

请你谈谈网站是如何进行访问的！

1. 输入一个域名；回车

2. 检查本机的 C:\Windows\System32\drivers\etc\hosts配置文件下有没有这个域名映射；

   1. 有：直接返回对应的ip地址，这个地址中，有我们需要访问的web程序，可以直接访问

      ```java
      127.0.0.1       www.qinjiang.com
      ```

   2. 没有：去DNS服务器找，找到的话就返回，找不到就返回找不到；

   ![1567827057913](.\javaweb.assets\1567827057913.png)

3. 可以配置一下环境变量（可选性）

### 3.4、在IDEA中配置Tomcat

![image-20200608134548079](.\javaweb.assets\image-20200608134548079.png)

![image-20200608134635700](.\javaweb.assets\image-20200608134635700.png)

![image-20200608134715000](.\javaweb.assets\image-20200608134715000.png)

![image-20200608135250006](.\javaweb.assets\image-20200608135250006.png)

解决警告问题

必须要的配置：**为什么会有这个问题：我们访问一个网站，需要指定一个文件夹名字；**

![image-20200608135544764](.\javaweb.assets\image-20200608135544764.png)

![image-20200608135615437](.\javaweb.assets\image-20200608135615437.png)

![image-20200608135808333](.\javaweb.assets\image-20200608135808333.png)

到这里就配置完成，进行运行测试

![image-20200608140219378](.\javaweb.assets\image-20200608140219378.png)

![image-20200608140541709](.\javaweb.assets\image-20200608140541709.png)

## 4、Http

### 4.1、什么是HTTP

HTTP（超文本传输协议）是一个简单的请求-响应协议，它通常运行在TCP之上。

- 文本：html，字符串，~ ….
- 超文本：图片，音乐，视频，定位，地图…….
- 80端口

Https：安全的

- 443端口

### 4.2、两个时代

- http1.0

  - HTTP/1.0：客户端可以与web服务器连接后，只能获得一个web资源，断开连接

- http2.0

  - HTTP/1.1：客户端可以与web服务器连接后，可以获得多个web资源。

### 4.3、Http请求

- 客户端---发请求（Request）---服务器

百度：

```java
Request URL:https://www.baidu.com/   请求地址
Request Method:GET    get方法/post方法
Status Code:200 OK    状态码：200
Remote（远程） Address:14.215.177.39:443
```

```java
Accept:text/html  
Accept-Encoding:gzip, deflate, br
Accept-Language:zh-CN,zh;q=0.9    语言
Cache-Control:max-age=0
Connection:keep-alive
```

#### 1、请求行

- 请求行中的请求方式：GET
- 请求方式：**Get，Post**，HEAD,DELETE,PUT,TRACT…
  - get：请求能够携带的参数比较少，大小有限制，会在浏览器的URL地址栏显示数据内容，不安全，但高效
  - post：请求能够携带的参数没有限制，大小没有限制，不会在浏览器的URL地址栏显示数据内容，安全，但不高效。

#### 2、消息头

```java
Accept：告诉浏览器，它所支持的数据类型
Accept-Encoding：支持哪种编码格式  GBK   UTF-8   GB2312  ISO8859-1
Accept-Language：告诉浏览器，它的语言环境
Cache-Control：缓存控制
Connection：告诉浏览器，请求完成是断开还是保持连接
HOST：主机..../.
```

### 4.4、Http响应

- 服务器---响应-----客户端

百度：

```java
Cache-Control:private    缓存控制
Connection:Keep-Alive    连接
Content-Encoding:gzip    编码
Content-Type:text/html   类型
```

#### 1.响应体

```java
Accept：告诉浏览器，它所支持的数据类型
Accept-Encoding：支持哪种编码格式  GBK   UTF-8   GB2312  ISO8859-1
Accept-Language：告诉浏览器，它的语言环境
Cache-Control：缓存控制
Connection：告诉浏览器，请求完成是断开还是保持连接
HOST：主机..../.
Refresh：告诉客户端，多久刷新一次；
Location：让网页重新定位；
```

#### 2、响应状态码 

200：请求响应成功  200

3xx：请求重定向 

- 重定向：你重新到我给你新位置去；

4xx：找不到资源   404

- 资源不存在；

5xx：服务器代码错误   500       502:网关错误



**常见面试题：**

当你的浏览器中地址栏输入地址并回车的一瞬间到页面能够展示回来，经历了什么？









## 5、Maven

### 5.1 Maven项目架构管理工具

我们目前用来就是方便导入jar包的！

Maven的核心思想：**约定大于配置**

- 有约束，不要去违反。

Maven会规定好你该如何去编写我们的Java代码，必须要按照这个规范来；

### 5.2 下载安装Maven

官网;https://maven.apache.org/

![1567842350606](.\javaweb.assets\1567842350606.png)

下载完成后，解压即可；



### 5.3 配置环境变量

在我们的系统环境变量中

配置如下配置：

- M2_HOME     maven目录下的bin目录
- MAVEN_HOME      maven的目录
- 在系统的path中配置  %MAVEN_HOME%\bin

![1567842882993](.\javaweb.assets\1567842882993.png)

测试Maven是否安装成功，保证必须配置完毕！

### 5.4 阿里云镜像

![1567844609399](.\javaweb.assets\1567844609399.png)

- 镜像：mirrors
  - 作用：加速我们的下载
- 国内建议使用阿里云的镜像

```xml
<mirror>
    <id>nexus-aliyun</id>  
    <mirrorOf>*,!jeecg,!jeecg-snapshots</mirrorOf>  
    <name>Nexus aliyun</name>  
    <url>http://maven.aliyun.com/nexus/content/groups/public</url> 
</mirror>
```

### 5.5 本地仓库

在本地的仓库，远程仓库；

**建立一个本地仓库：**localRepository

```xml
<localRepository>D:\Environment\apache-maven-3.6.2\maven-repo</localRepository>
```

### 5.6 在IDEA中使用Maven

1. 启动IDEA

2. 创建一个MavenWeb项目

   ![1567844785602](.\javaweb.assets\1567844785602.png)

   ![1567844841172](.\javaweb.assets\1567844841172.png)

   ![1567844917185](.\javaweb.assets\1567844917185.png)

3. 等待项目初始化完毕

   ![1567845105970](.\javaweb.assets\1567845105970-1591591735908.png)

4. IDEA中的Maven设置

   注意：IDEA项目创建成功后，看一眼Maven的配置

   - 但可以在开始界面进行全局设置

   

   ![1567905247201](.\javaweb.assets\1567905247201.png)

   ![1567845341956](D:/文档/近期文件/常用文件/笔记/java资料/JavaWeb笔记/JavaWeb/JavaWeb.assets/1567845341956.png)

   ![1567845413672](D:/文档/近期文件/常用文件/笔记/java资料/JavaWeb笔记/JavaWeb/JavaWeb.assets/1567845413672.png)

5. 修改web.xml文件，替换webapp版本和tomcat一致(查看tomcat中webapps\ROOT\WEB-INF下面的web.xml)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                         http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
     version="3.1"
     metadata-complete="true">
   
   </web-app>
   ```

==到这里所有的配置工作就完成了，可以开始进行javaweb开发==

## 6、Servlet

### 6.1、HelloServlet

Serlvet接口Sun公司有两个默认的实现类：HttpServlet，GenericServlet

1. 创建一个普通的Maven项目

2. 删除src目录，并创建一个MavenWeb模块

   ![image-20200608144340809](.\javaweb.assets\image-20200608144340809.png)

   ![image-20200608144540543](.\javaweb.assets\image-20200608144540543.png)

3. 增加目录

   ![image-20200608144821335](.\javaweb.assets\image-20200608144821335.png)

4. 在父项目中导入相应maven依赖

```xml
<!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
    <scope>provided</scope>
</dependency>
<!-- https://mvnrepository.com/artifact/javax.servlet.jsp/javax.servlet.jsp-api -->
<dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>javax.servlet.jsp-api</artifactId>
    <version>2.3.3</version>
    <scope>provided</scope>
</dependency>
```

5. Maven环境优化

   1. 修改web.xml为最新的

      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                            http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
        version="3.1"
        metadata-complete="true">
      
      </web-app>
      ```

   2. 将maven的结构搭建完整

6. 编写普通的java类并继承HttpServlet

   ```java
   public class myservlet_01 extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           PrintWriter writer = resp.getWriter(); //响应流
           writer.print("Hello,Serlvet");
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req, resp);
       }
   ```

7. 编写Servlet的映射(在web.xml文件中配置)

   为什么需要映射：我们写的是JAVA程序，但是要通过浏览器访问，而浏览器需要连接web服务器，所以我们需要再web服务中注册我们写的Servlet，还需给他一个浏览器能够访问的路径；

   ```xml
   <web-app>
     <display-name>Archetype Created Web Application</display-name>
     <!--注册Servlet-->
     <servlet>
       <servlet-name>hello</servlet-name>
       <servlet-class>com.xj.servlet.myservlet_01</servlet-class>
     </servlet>
     <!--Servlet的请求路径-->
     <servlet-mapping>
       <servlet-name>hello</servlet-name>
       <url-pattern>/hello</url-pattern>
     </servlet-mapping>
   </web-app>
   ```

8. 配置Tomcat

   注意：配置项目发布的路径就可以了

9. 测试

   ![image-20200608150808585](.\javaweb.assets\image-20200608150808585.png)

### 6.2、Servlet原理

Servlet是由Web服务器调用，web服务器在收到浏览器请求之后，会：

![1567913793252](.\javaweb.assets\1567913793252.png)

## 6.3、Mapping问题

1. 一个Servlet可以指定一个映射路径

   ```xml
   <servlet>
       <servlet-name>hello</servlet-name>
       <servlet-class>com.xj.servlet.myservlet_01</servlet-class>
   <servlet-mapping>
       <servlet-name>hello</servlet-name>
       <url-pattern>/hello</url-pattern>
   </servlet-mapping>
   </servlet>
   ```

2. 一个Servlet可以指定多个映射路径

   ```xml
   <servlet-mapping>
       <servlet-name>hello</servlet-name>
       <url-pattern>/hello</url-pattern>
   </servlet-mapping>
   <servlet-mapping>
       <servlet-name>hello</servlet-name>
       <url-pattern>/hello2</url-pattern>
   </servlet-mapping>
   <servlet-mapping>
       <servlet-name>hello</servlet-name>
       <url-pattern>/hello3</url-pattern>
   </servlet-mapping>
   <servlet-mapping>
       <servlet-name>hello</servlet-name>
       <url-pattern>/hello4</url-pattern>
   </servlet-mapping>
   <servlet-mapping>
       <servlet-name>hello</servlet-name>
       <url-pattern>/hello5</url-pattern>
   </servlet-mapping>
   
   ```

3. 一个Servlet可以指定通用映射路径

   ```xml
   <servlet-mapping>
       <servlet-name>hello</servlet-name>
       <url-pattern>/hello/*</url-pattern>
   </servlet-mapping>
   ```

4. 默认请求路径(这将取代默认页面)

   ```xml
   <!--默认请求路径-->
   <servlet-mapping>
       <servlet-name>hello</servlet-name>
       <url-pattern>/*</url-pattern>
   </servlet-mapping>
   ```

5. 指定一些后缀或者前缀等等….

   ```xml
   <!--可以自定义后缀实现请求映射
       注意点，*前面不能加项目映射的路径
       hello/sajdlkajda.xj
       -->
   <servlet-mapping>
       <servlet-name>hello</servlet-name>
       <url-pattern>*.xj</url-pattern>
   </servlet-mapping>
   ```

6. 优先级问题(先匹配现有的，没有就走默认的)
   指定了固有的映射路径优先级最高，如果找不到就会走默认的处理请求；

   ```xml
   <!--404-->
   <servlet>
       <servlet-name>error</servlet-name>
       <servlet-class>com.xj.servlet.myservlet_01</servlet-class>
   </servlet>
   <servlet-mapping>
       <servlet-name>error</servlet-name>
       <url-pattern>/*</url-pattern>
   </servlet-mapping>
   
   ```

   

## 6.5、ServletContext

### 1、共享数据

在这个Servlet中保存的数据，可以在另外一个servlet中拿到。

1. 设置ServletContext中的属性和值

   ```java
   public class MyServlet_02 extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           //this.getInitParameter()   初始化参数
           //this.getServletConfig()   Servlet配置
           //this.getServletContext()  Servlet上下文
           ServletContext context = this.getServletContext();
           String userName = "晓江";
           // 将一个数据保存在了ServletContext中，名字为：username, 值 username
           context.setAttribute("userName", userName);
       }
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req, resp);
       }
   }
   ```

2. 获取ServletContex中的值

   ```java
   public class MyServlet_03 extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           ServletContext context = this.getServletContext();
           String userName = (String) context.getAttribute("userName");
           // 设置编码方式
           resp.setContentType("text/html");
           resp.setCharacterEncoding("UTF-8");
           resp.getWriter().print("名字: " + userName);
       }
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req, resp);
       }
   }
   ```

3. 编写Servlet映射

   - **web.xml中配置属性也需要按照顺序**

   ```xml
   <web-app>
       <!--注册Servlet-->
   	<servlet>
           <servlet-name>setd</servlet-name>
           <servlet-class>com.xj.servlet.MyServlet_02</servlet-class>
       </servlet>
       <servlet>
           <servlet-name>getd</servlet-name>
           <servlet-class>com.xj.servlet.MyServlet_03</servlet-class>
       </servlet>
       <!--Servlet的请求路径-->
       <servlet-mapping>
           <servlet-name>setd</servlet-name>
           <url-pattern>/setd</url-pattern>
       </servlet-mapping>
       <servlet-mapping>
           <servlet-name>getd</servlet-name>
           <url-pattern>/getd</url-pattern>
       </servlet-mapping>
   </web-app>
   ```

4. 启动tomcat测试

   需要先进入setc页面设置数值之后，getc页面才能得到数值（否则显示null）

   ![image-20200608200227073](.\javaweb.assets\image-20200608200227073.png)

### 2、获取初始化数据

1. 设置初始数据

```xml
<!--配置一些web应用初始化参数-->
<context-param>
    <param-name>url</param-name>
    <param-value>jdbc:mysql://localhost:3306/mybatis</param-value>
</context-param>
```

2. 获取初始化数据
   - **注意是使用context.getInitParameter("url");**

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    // 设置编码
    resp.setCharacterEncoding("UTF-8");
    resp.setContentType("text/html");
    ServletContext context = this.getServletContext();
    String url = context.getInitParameter("url");
    resp.getWriter().print("获取到的数据为：" + url);
}
```

3. 编写servlet映射

4. 测试

   ![image-20200608201449597](.\javaweb.assets\image-20200608201449597.png)

   读取不到初始化数据。

   ==错误原因：获取初始化数据是使用`context.getInitParameter("url");`，而原来使用了`context.getAttribute("url");`==

   ![image-20200608221329503](.\javaweb.assets\image-20200608221329503.png)

### 3、请求转发

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    ServletContext context = this.getServletContext();
    System.out.println("进入了MyServlet_05");
    //RequestDispatcher requestDispatcher = context.getRequestDispatcher("/gp"); // 转发的请求路径
    //requestDispatcher.forward(req,resp);  // 调用forward实现请求转发；
    context.getRequestDispatcher("/hello").forward(req,resp);
}
```



### 4、读取资源文件

1. 在resources目录下新建properties

   或者在java目录下

2. 启动Tomcat的时候会自动加载文件到tartget目录下

   - 在resource下创建：导出到/WEB-INF/classes
   - 在java/com/xj/servlet/中创建：导出到/WEB-INF/classes/com/xj/servlet/

   ![image-20200608225403945](.\javaweb.assets\image-20200608225403945.png)

3. 编写servlet

   ```java
   @Override
   protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
       // 读取文件
       InputStream is = this.getServletContext().getResourceAsStream("/WEB-INF/classes/mysql.properties");
       Properties properties = new Properties();
       properties.load(is);
       // 获取数值
       String user = properties.getProperty("username");
       String pwd = properties.getProperty("password");
       resp.getWriter().print(user+":"+pwd);
   }
   ```

4. 编写servlet映射

5. 测试



## 6.6、HttpServletResponse

web服务器接收到客户端的http请求，针对这个请求，分别创建一个代表请求的HttpServletRequest对象，代表响应的一个HttpServletResponse；

- 如果要获取客户端请求过来的参数：找HttpServletRequest
- 如果要给客户端响应一些信息：找HttpServletResponse

**response对浏览器的操作**

1. 让浏览器能够支持(Content-Disposition)下载我们需要的东西

   ```java
   resp.setHeader("Content-Disposition","attachment;filename=" + 
                  URLEncoder.encode(fileName,"UTF-8"));
   ```

2. 让浏览器每3秒刷新一次

   ```java
   resp.setHeader("refresh", "3");
   ```

3. 把图片写给浏览器

   ```java
   ImageIO.write(image,"jpg", resp.getOutputStream());
   ```

### 1、下载文件

1. 要获取下载文件的路径（从target目录下复制路径）

2. 下载的文件名是啥？(最后一个“\\\”的序号再加一)

3. 设置想办法让浏览器能够支持下载我们需要的东西

   ```java
   resp.setHeader("Content-Disposition","attachment;filename=" + URLEncoder.encode(fileName,"UTF-8"));
   ```

4. 获取下载文件的输入流

5. 创建缓冲区

6. 获取OutputStream对象

7. 将FileOutputStream流写入到buffer缓冲区

8. 使用OutputStream将缓冲区中的数据输出到客户端！

- 测试

  1. 新建模块（response），需要配置一下Tomcat
  2. 修改web.xml文件
  3. 编写servlet

  ```java
  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
      //1. 要获取下载文件的路径（从target目录下复制路径）
      String realPath = "D:\\IDEA2019\\IdeaProjects\\JavaWeb_02\\Response\\target\\classes\\电脑.jpg";
      //2. 下载的文件名是啥？(最后一个\\的序号再加一)
      String fileName = realPath.substring(realPath.lastIndexOf("\\") + 1);
      // 3. 设置想办法让浏览器能够支持(Content-Disposition)下载我们需要的东西,中文文件名URLEncoder.encode编码，否则有可能乱码
      resp.setHeader("Content-Disposition","attachment;filename=" + URLEncoder.encode(fileName,"UTF-8"));
      //4. 获取下载文件的输入流
      FileInputStream in = new FileInputStream(realPath);
      //5. 创建缓冲区
      int len = 0;
      byte[] buffer = new byte[1024];
      //6. 获取OutputStream对象
      ServletOutputStream out = resp.getOutputStream();
      //7. 将FileOutputStream流写入到buffer缓冲区,使用OutputStream将缓冲区中的数据输出到客户端！
      while((len = in.read(buffer)) != -1){
          out.write(buffer, 0, len);
      }
      //8.关闭流
      in.close();
      out.close();
  }
  ```

  4. 编写servlet映射

  ```xml
  <servlet>
      <servlet-name>down</servlet-name>
      <servlet-class>com.xj.servlet.download</servlet-class>
  </servlet>
  <servlet-mapping>
      <servlet-name>down</servlet-name>
      <url-pattern>/down</url-pattern>
  </servlet-mapping>
  ```

  5. 测试

  ![image-20200609115441521](.\javaweb.assets\image-20200609115441521.png)

### 2、验证码功能

验证怎么来的？

- 前端实现
- 后端实现，需要用到 Java 的图片类，生产一个图片

1. 实现代码

```java
package com.xj.servlet;

import javax.imageio.ImageIO;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.IOException;
import java.util.Random;

public class ImageServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 让浏览器每3秒刷新一次
        resp.setHeader("refresh", "3");
        // 在内存中创建一张图片
        BufferedImage image = new BufferedImage(80, 20, BufferedImage.TYPE_INT_RGB);
        // 得到图片的画笔
        Graphics2D g = (Graphics2D) image.getGraphics();
        // 设置背景颜色
        g.setColor(Color.white);
        g.fillRect(0,0,80,20);
        //给图片写数据
        g.setColor(Color.blue);
        g.setFont(new Font(null, Font.BOLD, 20));
        g.drawString(makeNum(),0,20);
        //告诉浏览器，这个请求用图片的方式打开
        resp.setContentType("image/jpeg");
        //网站存在缓存，不让浏览器缓存
        resp.setDateHeader("expires",-1);
        resp.setHeader("Cache-Control","no-cache");
        resp.setHeader("Pragma","no-cache");
        //把图片写给浏览器
        ImageIO.write(image,"jpg", resp.getOutputStream());
    }
    //生成随机数
    private String makeNum(){
        Random random = new Random();
        String num = random.nextInt(9999999) + "";
        StringBuffer sb = new StringBuffer();
        // 保证产生的随机数是7为，不是的话就前面加 0
        for (int i = 0; i < 7-num.length() ; i++) {
            sb.append("0");
        }
        num = sb.toString() + num;
        return num;
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

2. 测试

   ![image-20200609122825999](.\javaweb.assets\image-20200609122825999.png)



### 3、实现重定向



B一个web资源收到客户端A请求后，B他会通知A客户端去访问另外一个web资源C，这个过程叫重定向

常见场景：

![1567931587955](.\javaweb.assets\1567931587955.png)

```java
resp.sendRedirect("/r/img");	// 重定向
```

面试题：请你聊聊重定向和转发的区别？

相同点

- 页面都会实现跳转

不同点

- 请求转发的时候，url不会产生变化；307

  resp.sendRedirect("/r/img");

- 重定向时候，url地址栏会发生变化；302

   req.getRequestDispatcher("/success.jsp").forward(req,resp);

![1567932163430](.\javaweb.assets\1567932163430.png)

 

### 6.7、HttpServletRequest

HttpServletRequest代表客户端的请求，用户通过Http协议访问服务器，HTTP请求中的所有信息会被封装到HttpServletRequest，通过这个HttpServletRequest的方法，获得客户端的所有信息；

**获取参数，请求转发**

- 请求转发：` req.getRequestDispatcher("/success.jsp").forward(req,resp);`

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
	// 设置接收和发送的编码
    req.setCharacterEncoding("utf-8");
    resp.setCharacterEncoding("utf-8");

    String username = req.getParameter("username");
    String password = req.getParameter("password");
    String[] hobbys = req.getParameterValues("hobbys");
    System.out.println("=============================");
    // 后台接收中文乱码问题
    System.out.println(username);
    System.out.println(password);
    System.out.println(Arrays.toString(hobbys));
    System.out.println("=============================");


    System.out.println(req.getContextPath());
    // 通过请求转发
    // 这里的 / 代表当前的web应用
    req.getRequestDispatcher("/success.jsp").forward(req,resp);

}
```



## 7、Cookie、Session

### 7.1、会话

**会话**：用户打开一个浏览器，点击了很多超链接，访问多个web资源，关闭浏览器，这个过程可以称之为会话；开启浏览器到关闭的这个过程。

**有状态会话**：一个同学来过教室，下次再来教室，我们会知道这个同学，曾经来过，称之为有状态会话；

**一个网站，怎么证明你来过？**

客户端              服务端

1. 服务端给客户端一个 信件，客户端下次访问服务端带上信件就可以了； cookie
2. 服务器登记你来过了，下次你来的时候我来匹配你； seesion



### 7.2、保存会话的两种技术

**cookie**

- 客户端技术   （响应，请求）

**session**

- 服务器技术，利用这个技术，可以保存用户的会话信息？ 我们可以把信息或者数据放在Session中！

### 7.3、Cookie

![1568344447291](.\javaweb.assets\1568344447291.png)

1. 从请求中拿到cookie信息
2. 服务器响应给客户端cookie

```java
// 获得Cookie
Cookie[] cookies = req.getCookies();
// 获得cookie中的key
cookie.getName(); 
// 获得cookie中的vlaue
cookie.getValue(); 
// 新建一个cookie， System.currentTimeMillis()+""获取当前时间
new Cookie("lastLoginTime", System.currentTimeMillis()+"");
// 设置cookie的有效期
cookie.setMaxAge(24*60*60); 
// 响应给客户端一个cookie
resp.addCookie(cookie); 
```

**cookie：一般会保存在本地的 用户目录下 appdata；**



一个网站cookie是否存在上限！

- 一个Cookie只能保存一个信息（字符串）；
- 一个web站点可以给浏览器发送多个cookie，最多存放20个cookie；
- Cookie大小有限制4kb；
- 300个cookie为浏览器上限

**删除Cookie；**

- **不设置有效期，关闭浏览器，自动失效；**
- 设置有效期时间为 0 ；

**编码解码(防止中文乱码)：**

```java
URLEncoder.encode("晓江","utf-8")
URLDecoder.decode(cookie.getValue(),"UTF-8")
```



**测试**

1. 保存上次访问的时间（不设置有效期的话，浏览器关闭就失效）

   ```java
   @Override
   protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
       req.setCharacterEncoding("utf-8");
       resp.setCharacterEncoding("utf-8");
       PrintWriter out = resp.getWriter();
       // 服务器从客户端获取cookie
       Cookie[] cookies = req.getCookies();
   
       // 判断cookie是否存在
       if(cookies != null){
           // 如果存在
           out.write("你的上次访问时间：");
           // 遍历所有cookie
           for (Cookie cookie : cookies) {
               if(cookie.getName().equals("lastTime")){
                   // 获取cookie中的值，将字符串转为Date类型
                   long lastTime = Long.parseLong(cookie.getValue());
                   Date date = new Date(lastTime);
                   out.write(date.toLocaleString());
               }
           }
       }else {
           out.write("这是你第一次访问该网站");
       }
       // 服务器给客户一个cookie
       Cookie cookie = new Cookie("lastTime", System.currentTimeMillis() + "");
       resp.addCookie(cookie);
       // 设置有效期，一分钟
       cookie.setMaxAge(60);
   }
   ```

2. 保存中文用户名

   ```java
   @Override
   protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       // 设置编码
       req.setCharacterEncoding("utf-8");
       resp.setCharacterEncoding("utf-8");
       resp.setContentType("text/html; charset=utf-8");
   
       PrintWriter out = resp.getWriter();
       // 创建一个cookie
       // URLEncoder.encode() 为URL编码
       Cookie cookie = new Cookie("username", URLEncoder.encode("晓江"));
       resp.addCookie(cookie);
       cookie.setMaxAge(60);
       // 获取出信息
       Cookie[] cookies = req.getCookies();
       for (Cookie cookie1 : cookies) {
           if(cookie1.getName().equals("username")){
               // URLDecoder.decode() 为URL解码
               out.write("用户名：" + URLDecoder.decode(cookie1.getValue()));
               System.out.println(URLEncoder.encode("晓江"));
           }
       }
   }
   ```



### 7.4、Session（重点）

![1568344560794](.\javaweb.assets\1568344560794.png)

什么是Session：

- 服务器会给每一个用户（浏览器）创建一个Seesion对象；
- 一个Seesion独占一个浏览器，只要浏览器没有关闭，这个Session就存在；
- 用户登录之后，整个网站它都可以访问！--> 保存用户的信息；保存购物车的信息…..

![1568342773861](.\javaweb.assets\1568342773861.png)

Session和cookie的区别：

- Cookie是把用户的数据写给用户的浏览器，浏览器保存 （可以保存多个）
- Session把用户的数据写到用户独占Session中（只有一个），服务器端保存  （保存重要的信息，减少服务器资源的浪费）
- Session对象由服务创建
- 设置值的时候cookie只能存放字符串类型，而session可以存放任意类型



使用场景：

- 保存一个登录用户的信息；
- 购物车信息；
- 在整个网站中经常会使用的数据，我们将它保存在Session中；



**测试**

1. 创建一个Session

   ```java
   @Override
   protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       // 设置编码
       req.setCharacterEncoding("utf-8");
       resp.setContentType("utf-8");
       resp.setContentType("text/html; charset=utf-8");
   
       // 获取session
       HttpSession session = req.getSession();
   
       // 给session存数据
       session.setAttribute("username", "晓江");
   
       // 获取sessionID
       String sessionID = session.getId();
   
       // 判断session是不是新建的
       if(session.isNew()){
           resp.getWriter().write("Session创建成功，ID：" + sessionID);
       }else {
           resp.getWriter().write("Session已经创建，ID：" + sessionID);
       }
   }
   ```

   其中`session.setAttribute("username", "晓江");`可以存放Object类型

   ```java
   // 给session存自定义类
   session.setAttribute("user", new person("晓江", 20));
   // 获取session中的数据
   person user = (person) session.getAttribute("user");
   ```

   

   **Session创建的时候做了什么事情？**

   - 将session中的值添加到cookie中

   ```java
Cookie cookie = new Cookie("JSESSIONID",sessionId);
   resp.addCookie(cookie);
   ```
   
2. Session中的主要内容

   ```java
   //得到Session
   HttpSession session = req.getSession();
   
   Person person = (Person) session.getAttribute("username");
   
   System.out.println(person.toString());
   
   HttpSession session = req.getSession();
   session.removeAttribute("name");
   //手动注销Session
   session.invalidate();
   ```

3. **会话手动过期**

   ```java
   // 获取Session
   HttpSession session = req.getSession();
   // 使Session失效
   session.invalidate();
   ```

4. **会话自动过期：web.xml配置**

   ```xml
   <!--设置Session默认的失效时间-->
   <session-config>
       <!--15分钟后Session自动失效，以分钟为单位-->
       <session-timeout>15</session-timeout>
   </session-config>
   ```

   ![1568344679763](.\javaweb.assets\1568344679763-1591774212672.png)

## 8、JSP

### 8.1、什么是JSP

Java Server Pages ： Java服务器端页面，也和Servlet一样，用于动态Web技术！

最大的特点：

- 写JSP就像在写HTML

- 区别：

  - HTML只给用户提供静态的数据
  - JSP页面中可以嵌入JAVA代码，为用户提供动态数据；

- **浏览器向服务器发送请求，不管访问什么资源，其实都是在访问Servlet！**

  JSP最终也会被转换成为一个Java类！

  **JSP 本质上就是一个Servlet**

![1568347078207](.\javaweb.assets\1568347078207.png)

### 8.2、JSP基础语法

#### **JSP表达式**

```jsp
  <%--JSP表达式
  作用：用来将程序的输出，输出到客户端
  <%= 变量或者表达式%>
  --%>
  <%= new java.util.Date()%>
```



#### **jsp脚本片段**

```jsp
<%--jsp脚本片段--%>
<%
	int sum = 0;
	for (int i = 1; i <=100 ; i++) {
	    sum+=i;
	}
	out.println("<h1>Sum="+sum+"</h1>");
%>
```



**脚本片段的再实现**

```jsp
<%
	int x = 10;
	out.println(x);
%>
	<p>这是一个JSP文档</p>
<%
	int y = 2;
	out.println(y);
%>

<hr>


<%--在代码嵌入HTML元素--%>
<%
	for (int i = 0; i < 5; i++) {
%>
	<h1>Hello,World  <%= i %> </h1>
<%
}
%>
```



#### JSP声明

```jsp
  <%!
    static {
      System.out.println("Loading Servlet!");
    }

    private int globalVar = 0;

    public void kuang(){
      System.out.println("进入了方法Kuang！");
    }
  %>
```



JSP声明：会被编译到JSP生成Java的类中！其他的，就会被生成到_jspService方法中！

在JSP，嵌入Java代码即可！

```jsp
<% %>
<%= %>
<%! %>

<%--注释--%>
```

JSP的注释，不会在客户端显示，HTML就会！



### 8.3、9大内置对象

- PageContext    存东西
- Request     存东西
- Response
- Session      存东西
- Application   【SerlvetContext】   存东西
- config    【SerlvetConfig】
- out
- page ，不用了解
- exception

```java
pageContext.setAttribute("name1","晓江1号"); //保存的数据只在一个页面中有效
request.setAttribute("name2","晓江2号"); //保存的数据只在一次请求中有效，请求转发会携带这个数据
session.setAttribute("name3","晓江3号"); //保存的数据只在一次会话中有效，从打开浏览器到关闭浏览器
application.setAttribute("name4","晓江4号");  //保存的数据只在服务器中有效，从打开服务器到关闭服务器
```

request：客户端向服务器发送请求，产生的数据，用户看完就没用了，比如：新闻，用户看完没用的！

session：客户端向服务器发送请求，产生的数据，用户用完一会还有用，比如：购物车；

application：客户端向服务器发送请求，产生的数据，一个用户用完了，其他用户还可能使用，比如：聊天数据；

### 8.6、JSP标签、JSTL标签、EL表达式

```xml
<!-- JSTL表达式的依赖 -->
<dependency>
    <groupId>javax.servlet.jsp.jstl</groupId>
    <artifactId>jstl-api</artifactId>
    <version>1.2</version>
</dependency>
<!-- standard标签库 -->
<dependency>
    <groupId>taglibs</groupId>
    <artifactId>standard</artifactId>
    <version>1.1.2</version>
</dependency>
```

EL表达式：  ${ }

- **获取数据**
- **执行运算**
- **获取web开发的常用对象**



**JSP标签**

```jsp
<%--jsp:include--%>

<%--
等价于
http://localhost:8080/jsptag.jsp?name=xj&age=12
--%>

<jsp:forward page="/jsptag2.jsp">
    <jsp:param name="name" value="xj"></jsp:param>
    <jsp:param name="age" value="12"></jsp:param>
</jsp:forward>
```



**JSTL表达式**

JSTL标签库的使用就是为了弥补HTML标签的不足；它自定义许多标签，可以供我们使用，标签的功能和Java代码一样！

**格式化标签**

**SQL标签**

**XML 标签**

**核心标签** （掌握部分）

- 需要添加

  ```xml
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  ```

![1568362473764](.\javaweb.assets\1568362473764.png)

**JSTL标签库使用步骤**

- 引入对应的 taglib
- 使用其中的方法
- **在Tomcat 也需要引入 jstl的包，否则会报错：JSTL解析错误**

c：if

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<head>
    <title>Title</title>
</head>
<body>


<h4>if测试</h4>

<hr>

<form action="coreif.jsp" method="get">
    <%--
    EL表达式获取表单中的数据
    ${param.参数名}
    --%>
    <input type="text" name="username" value="${param.username}">
    <input type="submit" value="登录">
</form>

<%--判断如果提交的用户名是管理员，则登录成功--%>
<c:if test="${param.username=='admin'}" var="isAdmin">
    <c:out value="管理员欢迎您！"/>
</c:if>

<%--自闭合标签--%>
<c:out value="${isAdmin}"/>

</body>
```

c:choose   c:when

```jsp
<body>

<%--定义一个变量score，值为85--%>
<c:set var="score" value="55"/>

<c:choose>
    <c:when test="${score>=90}">
        你的成绩为优秀
    </c:when>
    <c:when test="${score>=80}">
        你的成绩为一般
    </c:when>
    <c:when test="${score>=70}">
        你的成绩为良好
    </c:when>
    <c:when test="${score<=60}">
        你的成绩为不及格
    </c:when>
</c:choose>

</body>
```

c:forEach

```jsp
<%

    ArrayList<String> people = new ArrayList<>();
    people.add(0,"张三");
    people.add(1,"李四");
    people.add(2,"王五");
    people.add(3,"赵六");
    people.add(4,"田六");
    request.setAttribute("list",people);
%>


<%--
var , 每一次遍历出来的变量
items, 要遍历的对象
begin,   哪里开始
end,     到哪里
step,   步长
--%>
<c:forEach var="people" items="${list}">
    <c:out value="${people}"/> <br>
</c:forEach>

<hr>

<c:forEach var="people" items="${list}" begin="1" end="3" step="1" >
    <c:out value="${people}"/> <br>
</c:forEach>

```

## 9、JavaBean

实体类

JavaBean有特定的写法：

- 必须要有一个无参构造
- 属性必须私有化
- 必须有对应的get/set方法；

一般用来和数据库的字段做映射  ORM；

ORM ：对象关系映射

- 表--->类
- 字段-->属性
- 行记录---->对象

**people表**

| id   | name    | age  | address |
| ---- | ------- | ---- | ------- |
| 1    | 晓江1号 | 3    | 潮州    |
| 2    | 晓江2号 | 18   | 潮州    |
| 3    | 晓江3号 | 100  | 潮州    |

```java
class People{
    private int id;
    private String name;
    private int id;
    private String address;
}

class A{
    new People(1,"晓江1号",3，"潮州");
    new People(2,"晓江2号",3，"潮州");
    new People(3,"晓江3号",3，"潮州");
}
```



## 10、MVC三层架构

什么是MVC：  Model     view     Controller  模型、视图、控制器

### 10.1、早些年

![1568423664332](.\javaweb.assets\1568423664332.png)

用户直接访问控制层，控制层就可以直接操作数据库；

```java
servlet--CRUD-->数据库
弊端：程序十分臃肿，不利于维护  
servlet的代码中：处理请求、响应、视图跳转、处理JDBC、处理业务代码、处理逻辑代码

架构：没有什么是加一层解决不了的！
程序猿调用
|
JDBC
|
Mysql Oracle SqlServer ....
```

### 10.2、MVC三层架构

![1568424227281](.\javaweb.assets\1568424227281.png)



Model

- 业务处理 ：业务逻辑（Service）
- 数据持久层：CRUD   （Dao）

View

- 展示数据
- 提供链接发起Servlet请求 （a，form，img…）

Controller  （Servlet）

- 接收用户的请求 ：（req：请求参数、Session信息….）

- 交给业务层处理对应的代码 

- 控制视图的跳转  

  ```java
  登录--->接收用户的登录请求--->处理用户的请求（获取用户登录的参数，username，password）---->交给业务层处理登录业务（判断用户名密码是否正确：事务）--->Dao层查询用户名和密码是否正确-->数据库
  ```



## 11、Filter （重点）

Filter：过滤器 ，用来过滤网站的数据；

- 处理中文乱码
- 登录验证….

![1568424858708](.\javaweb.assets\1568424858708.png)

Filter开发步骤：

1. 导包

2. 编写过滤器

   1. 导包不要错

      ![1568425162525](.\javaweb.assets\1568425162525.png)

      实现Filter接口，重写对应的方法即可

      ```java
      public class CharacterEncodingFilter implements Filter {
      
          //初始化：web服务器启动，就以及初始化了，随时等待过滤对象出现！
          public void init(FilterConfig filterConfig) throws ServletException {
              System.out.println("CharacterEncodingFilter初始化");
          }
      
          //Chain : 链
          /*
          1. 过滤中的所有代码，在过滤特定请求的时候都会执行
          2. 必须要让过滤器继续同行
              chain.doFilter(request,response);
           */
          public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
              request.setCharacterEncoding("utf-8");
              response.setCharacterEncoding("utf-8");
              response.setContentType("text/html;charset=UTF-8");
      
              System.out.println("CharacterEncodingFilter执行前....");
              chain.doFilter(request,response); //让我们的请求继续走，如果不写，程序到这里就被拦截停止！
              System.out.println("CharacterEncodingFilter执行后....");
          }
      
          //销毁：web服务器关闭的时候，过滤会销毁
          public void destroy() {
              System.out.println("CharacterEncodingFilter销毁");
          }
      }
      
      ```

3. 在web.xml中配置 Filter

   ```xml
   <filter>
       <filter-name>CharacterEncodingFilter</filter-name>
       <filter-class>com.xj.filter.CharacterEncodingFilter</filter-class>
   </filter>
   <filter-mapping>
       <filter-name>CharacterEncodingFilter</filter-name>
       <!--只要是 /servlet的任何请求，会经过这个过滤器-->
       <url-pattern>/servlet/*</url-pattern>
       <!--<url-pattern>/*</url-pattern>-->
   </filter-mapping>
   ```

   

## 12、监听器

实现一个监听器的接口；（有N种）

1. 编写一个监听器

   实现监听器的接口…

   ```java
   //统计网站在线人数 ： 统计session
   public class OnlineCountListener implements HttpSessionListener {
   
       //创建session监听： 看你的一举一动
       //一旦创建Session就会触发一次这个事件！
       public void sessionCreated(HttpSessionEvent se) {
           ServletContext ctx = se.getSession().getServletContext();
   
           System.out.println(se.getSession().getId());
   
           Integer onlineCount = (Integer) ctx.getAttribute("OnlineCount");
   
           if (onlineCount==null){
               onlineCount = new Integer(1);
           }else {
               int count = onlineCount.intValue();
               onlineCount = new Integer(count+1);
           }
   
           ctx.setAttribute("OnlineCount",onlineCount);
   
       }
   
       //销毁session监听
       //一旦销毁Session就会触发一次这个事件！
       public void sessionDestroyed(HttpSessionEvent se) {
           ServletContext ctx = se.getSession().getServletContext();
   
           Integer onlineCount = (Integer) ctx.getAttribute("OnlineCount");
   
           if (onlineCount==null){
               onlineCount = new Integer(0);
           }else {
               int count = onlineCount.intValue();
               onlineCount = new Integer(count-1);
           }
   
           ctx.setAttribute("OnlineCount",onlineCount);
   
       }
   
   
       /*
       Session销毁：
       1. 手动销毁  getSession().invalidate();
       2. 自动销毁
        */
   }
   
   ```

2. web.xml中注册监听器

   ```xml
   <!--注册监听器-->
   <listener>
       <listener-class>com.xj.listener.OnlineCountListener</listener-class>
   </listener>
   ```

3. 看情况是否使用！



## 13、过滤器、监听器常见应用

**监听器：GUI编程中经常使用；**

```java
public class TestPanel {
    public static void main(String[] args) {
        Frame frame = new Frame("快乐");  //新建一个窗体
        Panel panel = new Panel(null); //面板
        frame.setLayout(null); //设置窗体的布局

        frame.setBounds(300,300,500,500);
        frame.setBackground(new Color(0,0,255)); //设置背景颜色

        panel.setBounds(50,50,300,300);
        panel.setBackground(new Color(0,255,0)); //设置背景颜色

        frame.add(panel);

        frame.setVisible(true);

        //监听事件，监听关闭事件
        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                super.windowClosing(e);
            }
        });


    }
}
```



用户登录之后才能进入主页！用户注销后就不能进入主页了！

1. 用户登录之后，向Sesison中放入用户的数据

2. 进入主页的时候要判断用户是否已经登录；要求：在过滤器中实现！

   ```java
   // 获取session和request
   HttpServletRequest request = (HttpServletRequest) req;
   HttpServletResponse response = (HttpServletResponse) resp;
   
   if (request.getSession().getAttribute(Constant.USER_SESSION)==null){
       response.sendRedirect("/error.jsp");
   }
   
   chain.doFilter(request,response);
   ```



## 14、JDBC

   什么是JDBC ： Java连接数据库！

   ![1568439601825](.\javaweb.assets\1568439601825.png)

   需要jar包的支持：

   - java.sql
   - javax.sql
   - mysql-conneter-java…  连接驱动（必须要导入）

   

   **实验环境搭建**

   ```sql
   CREATE TABLE users(
       id INT PRIMARY KEY,
       `name` VARCHAR(40),
       `password` VARCHAR(40),
       email VARCHAR(60),
       birthday DATE
   );

   INSERT INTO users(id,`name`,`password`,email,birthday)
   VALUES(1,'张三','123456','zs@qq.com','2000-01-01');
   INSERT INTO users(id,`name`,`password`,email,birthday)
   VALUES(2,'李四','123456','ls@qq.com','2000-01-01');
   INSERT INTO users(id,`name`,`password`,email,birthday)
   VALUES(3,'王五','123456','ww@qq.com','2000-01-01');


   SELECT	* FROM users;

   ```

   

   导入数据库依赖

   ```xml
<!--mysql的驱动-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
</dependency>
   ```

   IDEA中连接数据库：

   ![1568440926845](.\javaweb.assets\1568440926845.png)

   

   **JDBC 固定步骤：**

   1. 加载驱动
   2. 连接数据库,代表数据库
   3. 向数据库发送SQL的对象Statement : CRUD
   4. 编写SQL （根据业务，不同的SQL）
   5. 执行SQL
   6. 关闭连接

   ```java
   public class TestJdbc {
       public static void main(String[] args) throws ClassNotFoundException, SQLException {
           //配置信息
           //useUnicode=true&characterEncoding=utf-8 解决中文乱码
           String url="jdbc:mysql://localhost:3306/jdbc?useUnicode=true&characterEncoding=utf-8";
           String username = "root";
           String password = "123456";
   
           //1.加载驱动
           Class.forName("com.mysql.jdbc.Driver");
           //2.连接数据库,代表数据库
           Connection connection = DriverManager.getConnection(url, username, password);
           //3.向数据库发送SQL的对象Statement,PreparedStatement : CRUD
           Statement statement = connection.createStatement();
   
           //4.编写SQL
           String sql = "select * from users";
   
           //5.执行查询SQL，返回一个 ResultSet  ： 结果集
           ResultSet rs = statement.executeQuery(sql);
   
           while (rs.next()){
               System.out.println("id="+rs.getObject("id"));
               System.out.println("name="+rs.getObject("name"));
               System.out.println("password="+rs.getObject("password"));
               System.out.println("email="+rs.getObject("email"));
               System.out.println("birthday="+rs.getObject("birthday"));
           }
   
           //6.关闭连接，释放资源（一定要做） 先开后关
           rs.close();
           statement.close();
           connection.close();
       }
   }
   
   ```

   

   **预编译SQL**

- 使用preparedStatement比Statement安全

   ```java
   public class TestJDBC2 {
       public static void main(String[] args) throws Exception {
           //配置信息
           //useUnicode=true&characterEncoding=utf-8 解决中文乱码
           String url="jdbc:mysql://localhost:3306/jdbc?useUnicode=true&characterEncoding=utf-8";
           String username = "root";
           String password = "123456";
   
           //1.加载驱动
           Class.forName("com.mysql.jdbc.Driver");
           //2.连接数据库,代表数据库
           Connection connection = DriverManager.getConnection(url, username, password);
   
           //3.编写SQL
           String sql = "insert into  users(id, name, password, email, birthday) values (?,?,?,?,?);";
   
           //4.预编译
           PreparedStatement preparedStatement = connection.prepareStatement(sql);
   
           preparedStatement.setInt(1,2);//给第一个占位符？ 的值赋值为1；
           preparedStatement.setString(2,"Java");//给第二个占位符？ 的值赋值为狂神说Java；
           preparedStatement.setString(3,"123456");//给第三个占位符？ 的值赋值为123456；
           preparedStatement.setString(4,"24736743@qq.com");//给第四个占位符？ 的值赋值为1；
           preparedStatement.setDate(5,new Date(new java.util.Date().getTime()));//给第五个占位符？ 的值赋值为new Date(new java.util.Date().getTime())；
   
           //5.执行SQL
           int i = preparedStatement.executeUpdate();
   
           if (i>0){
               System.out.println("插入成功@");
           }
   
           //6.关闭连接，释放资源（一定要做） 先开后关
           preparedStatement.close();
           connection.close();
       }
   }
   
   ```

   

