## SpringBoot

## 1、简介

### 1.1、什么是Spring

Spring是一个开源框架，2003 年兴起的一个轻量级的Java 开发框架，作者：Rod Johnson 

**Spring是为了解决企业级应用开发的复杂性而创建的，简化开发。**



### 1.2、Spring是如何简化Java开发的

为了降低Java开发的复杂性，Spring采用了以下4种关键策略：

1. 基于POJO的轻量级和最小侵入性编程，所有东西都是bean；
2. 通过IOC，依赖注入（DI）和面向接口实现松耦合；
3. 基于切面（AOP）和惯例进行声明式编程；
4. 通过切面和模版减少样式代码，RedisTemplate，xxxTemplate；



### 1.3、什么是SpringBoot

​	学过javaweb的同学就知道，开发一个web应用，从最初开始接触Servlet结合Tomcat, 跑出一个Hello Wolrld程序，是要经历特别多的步骤；后来就用了框架Struts，再后来是SpringMVC，到了现在的SpringBoot，过一两年又会有其他web框架出现；你们有经历过框架不断的演进，然后自己开发项目所有的技术也在不断的变化、改造吗？建议都可以去经历一遍；

​	言归正传，什么是SpringBoot呢，就是一个javaweb的开发框架，和SpringMVC类似，对比其他javaweb框架的好处，官方说是简化开发，**约定大于配置**，  you can "just run"，能迅速的开发web应用，几行代码开发一个http接口。

​	所有的技术框架的发展似乎都遵循了一条主线规律：从一个复杂应用场景 衍生 一种规范框架，人们只需要进行各种配置而不需要自己去实现它，这时候强大的配置功能成了优点；发展到一定程度之后，人们根据实际生产应用情况，选取其中实用功能和设计精华，重构出一些轻量级的框架；之后为了提高开发效率，嫌弃原先的各类配置过于麻烦，于是开始提倡“约定大于配置”，进而衍生出一些一站式的解决方案。

> 是的这就是Java企业级应用->J2EE->spring->springboot的过程。

​	随着 Spring 不断的发展，涉及的领域越来越多，项目整合开发需要配合各种各样的文件，慢慢变得不那么易用简单，违背了最初的理念，甚至人称配置地狱。Spring Boot 正是在这样的一个背景下被抽象出来的开发框架，目的为了让大家更容易的使用 Spring 、更容易的集成各种常用的中间件、开源软件；

​	Spring Boot 基于 Spring 开发，Spirng Boot 本身并不提供 Spring 框架的核心特性以及扩展功能，只是用于快速、敏捷地开发新一代基于 Spring 框架的应用程序。也就是说，它并不是用来替代 Spring 的解决方案，而是和 Spring 框架紧密结合用于提升 Spring 开发者体验的工具。Spring Boot 以**约定大于配置的核心思想**，默认帮我们进行了很多设置，多数 Spring Boot 应用只需要很少的 Spring 配置。同时它集成了大量常用的第三方库配置（例如 Redis、MongoDB、Jpa、RabbitMQ、Quartz 等等），Spring Boot 应用中这些第三方库几乎可以零配置的开箱即用。

​	简单来说就是SpringBoot其实不是什么新的框架，它默认配置了很多框架的使用方式，就像maven整合了所有的jar包，spring boot整合了所有的框架 。

​	Spring Boot 出生名门，从一开始就站在一个比较高的起点，又经过这几年的发展，生态足够完善，Spring Boot 已经当之无愧成为 Java 领域最热门的技术。

**Spring Boot的主要优点：**

- 为所有Spring开发者更快的入门
- **开箱即用**，提供各种默认配置来简化项目配置
- 内嵌式容器简化Web项目
- 没有冗余代码生成和XML配置的要求



## 2、第一个Spring Boot

> 准备工作

我们将学习如何快速的创建一个Spring Boot应用，并且实现一个简单的Http请求处理。通过这个例子对Spring Boot有一个初步的了解，并体验其结构简单、开发快速的特性。

我的环境准备：

- java version "1.8.0_181"
- Maven-3.6.1
- SpringBoot 2.x 最新版

开发工具：

- IDEA



### 2.1、创建基础项目说明

Spring官方提供了非常方便的工具让我们快速构建应用

Spring Initializr：https://start.spring.io/

#### 1、项目创建方式一

使用Spring Initializr 的 Web页面创建项目

1. 打开  https://start.spring.io/

   或直接进行Spring官网，点击Spring Boot

   ![image-20200719180427482](.\SpringBoot.assets\image-20200719180427482.png)

   点击下方的快速开始

   ![image-20200719180513214](.\SpringBoot.assets\image-20200719180513214.png)

   这里有官方用法教程，同样也是点击[start.spring.io](https://start.spring.io/)网页进行创建

   ![image-20200719180541966](.\SpringBoot.assets\image-20200719180541966.png)

2. 填写项目信息

   ![image-20200719181054653](.\SpringBoot.assets\image-20200719181054653.png)

   导入Web模板

   ![image-20200719181538488](.\SpringBoot.assets\image-20200719181538488.png)

3. 点击`Generate Project`按钮生成项目；下载此项目

4. 解压项目包，并用IDEA以Maven项目导入，一路下一步即可，直到项目导入完毕。

   ![image-20200719181255497](.\SpringBoot.assets\image-20200719181255497.png)

   使用IDEA导入项目（注意不是打开），选择Maven，然后其他的默认即可

   ![image-20200719181642130](.\SpringBoot.assets\image-20200719181642130.png)

5. 如果是第一次使用，可能速度会比较慢，包比较多、需要耐心等待一切就绪。(真的好慢！)

   ![image-20200719181734041](.\SpringBoot.assets\image-20200719181734041.png)

6. 完成之后，可以将多余的项目删除，这就成为一个简洁的`Maven`项目了

   ![image-20200719184007159](.\SpringBoot.assets\image-20200719184007159.png)



#### 2、项目创建方式二

使用 IDEA 直接创建项目

1. 创建一个新项目

2. 选择`spring initalizr` ， 可以看到默认就是去官网的快速构建工具那里实现

   ![image-20200719202324251](.\SpringBoot.assets\image-20200719202324251.png)

3. 填写项目信息

   ![image-20200719202434088](.\SpringBoot.assets\image-20200719202434088.png)

   **然后报错了**（Artifact contains illegal characters即包含非法字符，**将名称中的字母改为小写即可**）

   ![image-20200719202936439](.\SpringBoot.assets\image-20200719202936439.png)

4. 选择初始化的组件（初学勾选 Web 即可）

   ![image-20200719203444139](.\SpringBoot.assets\image-20200719203444139.png)

5. 填写项目路径

6. 等待项目构建成功，第二次创建的项目就快很多了！然后将不需要的文件夹删除简化一下。

   ![image-20200719203551546](.\SpringBoot.assets\image-20200719203551546.png)![image-20200719203708881](.\SpringBoot.assets\image-20200719203708881.png)



### 2.2、项目结构分析

通过上面步骤完成了基础项目的创建。就会自动生成以下文件。

![image-20200719184106890](.\SpringBoot.assets\image-20200719184106890.png)

- 程序的主启动类
- 一个 application.properties 配置文件
- 一个 测试类
- 一个 pom.xml

#### 1、pom.xml分析

```xml
<!-- 父依赖 -->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.5.RELEASE</version>
    <relativePath/>
</parent>

<dependencies>
    <!-- web场景启动器 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!-- springboot单元测试 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
        <!-- 剔除依赖 -->
        <exclusions>
            <exclusion>
                <groupId>org.junit.vintage</groupId>
                <artifactId>junit-vintage-engine</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>

<build>
    <plugins>
        <!-- 打包插件 -->
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```



### 2.3、编写一个http接口

1. 在主程序的同级目录下，新建一个controller包，一定要在同级目录下，否则识别不到

2. 在包中新建一个HelloController类，跟`SpringMVC`的作用一样，但是省略了很多配置

   ```java
   // 不经过视图解析器
   @RestController
   public class HelloController {
       @RequestMapping("/hello")
       public String hello(){
           return "Hello World!";
       }
   }
   ```

3. 编写完毕后，**从主程序启动项目**，浏览器发起请求，看页面返回；控制台输出了 Tomcat 访问的端口号！

   ![image-20200719184329128](.\SpringBoot.assets\image-20200719184329128.png)

   运行效果

   ![image-20200719184418387](.\SpringBoot.assets\image-20200719184418387.png)

4. 打开浏览器，访问成功

   ![image-20200719184447831](.\SpringBoot.assets\image-20200719184447831.png)

简单几步，就完成了一个web接口的开发，SpringBoot就是这么简单。所以我们常用它来建立我们的微服务项目！



### 2.4、将项目打成jar包

1. 双击 maven的 package

   ![image-20200719184533256](.\SpringBoot.assets\image-20200719184533256.png)

2. 如果打包成功，则会在target目录下生成一个 jar 包

   ![image-20200719184649839](.\SpringBoot.assets\image-20200719184649839.png)

   ![image-20200719184701953](.\SpringBoot.assets\image-20200719184701953.png)

4. 如果打包错误，可以配置打包时 跳过项目运行测试用例

   ```xml
   <!--
       在工作中,很多情况下我们打包是不想执行测试用例的
       可能是测试用例不完事,或是测试用例会影响数据库数据
       跳过测试用例执
       -->
   <plugin>
       <groupId>org.apache.maven.plugins</groupId>
       <artifactId>maven-surefire-plugin</artifactId>
       <configuration>
           <!--跳过项目运行测试用例-->
           <skipTests>true</skipTests>
       </configuration>
   </plugin>
   ```

5. 打成了jar包后，就可以在任何地方运行了，不需要开启Tomcat。

   运行jar包：

   - 切换到文件路径
   - 输入 java -jar 文件名.jar

   ![image-20200719201453591](.\SpringBoot.assets\image-20200719201453591.png)



### 2.5、修改启动时的字符

> 如何更改启动时显示的字符拼成的字母，SpringBoot呢？也就是 banner 图案

只需一步：到项目下的 `resources `目录下新建一个`banner.txt` 即可，注意图标右下角有符号。

![image-20200719201710543](.\SpringBoot.assets\image-20200719201710543.png)

图案可以到：https://www.bootschool.net/ascii 这个网站生成，然后拷贝到文件中即可！

像这样：

```tex
                       _____________________________________________________
                      |                                                     |
             _______  |                                                     |
            / _____ | |                       ACME MOO-VERS                 |
           / /(__) || |                                                     |
  ________/ / |OO| || |                                                     |
 |         |-------|| |                                                     |
(|         |     -.|| |_______________________                              |
 |  ____   \       ||_________||____________  |             ____      ____  |
/| / __ \   |______||     / __ \   / __ \   | |            / __ \    / __ \ |\
\|| /  \ |_______________| /  \ |_| /  \ |__| |___________| /  \ |__| /  \|_|/
   | () |                 | () |   | () |                  | () |    | () |
    \__/                   \__/     \__/                    \__/      \__/
```

**运行效果**

![image-20200719201751896](.\SpringBoot.assets\image-20200719201751896.png)

### 2.6、application.properties配置文件

> 参考网站：application.properties

修改端口号：修改之后必须访问8081端口才有效

```properties
server.port=8081
```

系统属性/变量

```properties
spring.profiles.active=xxx
```

配置首页的目录

```properties

```





## 3、SpringBoot运行原理初探

参考网站：https://www.cnblogs.com/milicool/p/11718099.html

> 我们之前写的HelloSpringBoot，到底是怎么运行的呢，Maven项目，我们一般从pom.xml文件探究起

### 3.1、pom.xml

> #### 父项目

其中它主要是依赖一个父项目，主要是管理项目的资源过滤及插件！

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.3.1.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

点进去，发现还有一个父依赖

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.3.1.RELEASE</version>
</parent>
```

这里才是真正管理SpringBoot应用里面所有依赖版本的地方，SpringBoot的版本控制中心；

**以后我们导入依赖默认是不需要写版本；但是如果导入的包没有在依赖中管理着就需要手动配置版本了；**



> #### 启动器 spring-boot-starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**springboot-boot-starter-xxx**：就是spring-boot的场景启动器

**spring-boot-starter-web**：帮我们导入了web模块正常运行所依赖的组件；

SpringBoot将所有的功能场景都抽取出来，做成一个个的starter （启动器），只需要在项目中引入这些starter即可，所有相关的依赖都会导入进来 ， 我们要用什么功能就导入什么样的场景启动器即可 ；我们未来也可以自己自定义 starter；



### 3.2、主启动类

> 分析完了 pom.xml 来看看这个启动类

```java
//@SpringBootApplication 来标注一个主程序类
//说明这是一个Spring Boot应用
@SpringBootApplication
public class SpringbootApplication {

   public static void main(String[] args) {
     //以为是启动了一个方法，没想到启动了一个服务
      SpringApplication.run(SpringbootApplication.class, args);
   }

}
```

但是**一个简单的启动类并不简单！**我们来分析一下这些注解都干了什么

> `@SpringBootApplication`

作用：标注在某个类上说明这个类是SpringBoot的**主配置类** ， SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；

进入这个注解：可以看到上面还有很多其他注解！

```java
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
    // ......
}
```

- `@ComponentScan`

  这个注解在Spring中很重要 ,它对应XML配置中的元素。

  作用：自动扫描并加载符合条件的组件或者bean ， 将这个bean定义加载到IOC容器中

- `@SpringBootConfiguration`

  作用：SpringBoot的配置类 ，标注在某个类上 ， 表示这是一个SpringBoot的配置类；

  我们继续进去这个注解查看

  ```java
  // 点进去得到下面的 @Component
  @Configuration
  public @interface SpringBootConfiguration {}
  
  @Component
  public @interface Configuration {}
  ```

  这里的 `@Configuration`，说明这是一个配置类 ，配置类就是对应Spring的xml 配置文件；

  里面的` @Component` 这就说明，启动类本身也是Spring中的一个组件而已，负责启动应用！

  我们回到 `SpringBootApplication `注解中继续看。

- `@EnableAutoConfiguration`：开启自动配置功能

  以前我们需要自己配置的东西，而现在SpringBoot可以自动帮我们配置 ；

  `@EnableAutoConfiguration`告诉SpringBoot开启自动配置功能，这样自动配置才能生效；

  点进注解接续查看：

  - `@AutoConfigurationPackage` ：自动配置包

  - `@import` ：Spring底层注解@import ， 给容器中导入一个组件

    Registrar.class 作用：将主启动类的所在包及包下面所有子包里面的所有组件扫描到Spring容器 ；

    这个分析完了，退到上一步，继续看

    `@Import({AutoConfigurationImportSelector.class})` ：给容器导入组件 ；

    - `AutoConfigurationImportSelector` ：自动配置导入选择器，那么它会导入哪些组件的选择器呢？我们点击去这个类看源码：

      1、这个类中有一个这样的方法

      ```java
      // 获得候选的配置
      protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
          //这里的getSpringFactoriesLoaderFactoryClass（）方法
          //返回的就是我们最开始看的启动自动导入配置文件的注解类；EnableAutoConfiguration
          List<String> configurations = SpringFactoriesLoader.loadFactoryNames(this.getSpringFactoriesLoaderFactoryClass(), this.getBeanClassLoader());
          Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you are using a custom packaging, make sure that file is correct.");
          return configurations;
      }
      ```

      ![image-20200719233041862](.\SpringBoot.assets\image-20200719233041862.png)

      2、这个方法又调用了 ` SpringFactoriesLoader` 类的静态方法！我们进入`SpringFactoriesLoader`类`loadFactoryNames() `方法

      ```java
      public static List<String> loadFactoryNames(Class<?> factoryClass, @Nullable ClassLoader classLoader) {
          String factoryClassName = factoryClass.getName();
          //这里它又调用了 loadSpringFactories 方法
          return (List)loadSpringFactories(classLoader).getOrDefault(factoryClassName, Collections.emptyList());
      }
      ```

      3、我们继续点击查看` loadSpringFactories `方法

      ```java
      private static Map<String, List<String>> loadSpringFactories(@Nullable ClassLoader classLoader) {
          //获得classLoader ， 我们返回可以看到这里得到的就是EnableAutoConfiguration标注的类本身
          MultiValueMap<String, String> result = (MultiValueMap)cache.get(classLoader);
          if (result != null) {
              return result;
          } else {
              try {
                  //去获取一个资源 "META-INF/spring.factories"
                  Enumeration<URL> urls = classLoader != null ? classLoader.getResources("META-INF/spring.factories") : ClassLoader.getSystemResources("META-INF/spring.factories");
                  LinkedMultiValueMap result = new LinkedMultiValueMap();
      
                  //将读取到的资源遍历，封装成为一个Properties
                  while(urls.hasMoreElements()) {
                      URL url = (URL)urls.nextElement();
                      UrlResource resource = new UrlResource(url);
                      Properties properties = PropertiesLoaderUtils.loadProperties(resource);
                      Iterator var6 = properties.entrySet().iterator();
      
                      while(var6.hasNext()) {
                          Entry<?, ?> entry = (Entry)var6.next();
                          String factoryClassName = ((String)entry.getKey()).trim();
                          String[] var9 = StringUtils.commaDelimitedListToStringArray((String)entry.getValue());
                          int var10 = var9.length;
      
                          for(int var11 = 0; var11 < var10; ++var11) {
                              String factoryName = var9[var11];
                              result.add(factoryClassName, factoryName.trim());
                          }
                      }
                  }
      
                  cache.put(classLoader, result);
                  return result;
              } catch (IOException var13) {
                  throw new IllegalArgumentException("Unable to load factories from location [META-INF/spring.factories]", var13);
              }
          }
      }
      ```

      其中 `FACTORIES_RESOURCE_LOCATION = "META-INF/spring.factories";`

      4、发现一个多次出现的文件：spring.factories，全局搜索它

      ![image-20200719234658500](.\SpringBoot.assets\image-20200719234658500.png)

      ![image-20200719234733885](.\SpringBoot.assets\image-20200719234733885.png)

      我们在上面的自动配置类随便找一个打开看看，比如 ：`WebMvcAutoConfiguration`

      ![image-20200719234824109](.\SpringBoot.assets\image-20200719234824109.png)

      可以看到这些一个个的都是JavaConfig配置类，而且都注入了一些Bean，可以找一些自己认识的类，看着熟悉一下！

      所以，自动配置真正实现是从`classpath`中搜寻所有的`META-INF/spring.factories`配置文件 ，并将其中对应的 `org.springframework.boot.autoconfigure` 包下的配置项，通过反射实例化为对应标注了 `@Configuration`的JavaConfig形式的IOC容器配置类 ， 然后将这些都汇总成为一个实例并加载到IOC容器中。

- **结论：**

  1. SpringBoot在启动的时候从类路径下的`META-INF/spring.factories`中获取`EnableAutoConfiguration`指定的值
  2. 将这些值作为自动配置类导入容器 ， 自动配置类就生效 ， 帮我们进行自动配置工作；
  3. 整个J2EE的整体解决方案和自动配置都在`springboot-autoconfigure`的jar包中；
  4. 它会给容器中导入非常多的自动配置类 `（xxxAutoConfiguration）`, 就是给容器中导入这个场景需要的所有组件 ， 并配置好这些组件 ；
  5. 有了自动配置类 ， 免去了我们手动编写配置注入功能组件等的工作；

  **现在大家应该大概的了解了下，SpringBoot的运行原理，后面我们还会深化一次！**

  

### 3.3、SpringApplication

> 不简单的方法

我最初以为就是运行了一个main方法，没想到却开启了一个服务；

```java
@SpringBootApplication
public class SpringbootApplication {
    public static void main(String[] args) {
        SpringApplication.run(SpringbootApplication.class, args);
    }
}
```

**SpringApplication.run分析**

分析该方法主要分两部分，一部分是SpringApplication的实例化，二是run方法的执行;

> SpringApplication

**这个类主要做了以下四件事情：**

1. 推断应用的类型是普通的项目还是Web项目
2. 查找并加载所有可用初始化器 ， 设置到initializers属性中
3. 找出所有的应用程序监听器，设置到listeners属性中
4. 推断并设置main方法的定义类，找到运行的主类

查看构造器：

```java
public SpringApplication(ResourceLoader resourceLoader, Class... primarySources) {
    // ......
    this.webApplicationType = WebApplicationType.deduceFromClasspath();
    this.setInitializers(this.getSpringFactoriesInstances();
    this.setListeners(this.getSpringFactoriesInstances(ApplicationListener.class));
    this.mainApplicationClass = this.deduceMainApplicationClass();
}
```

> run方法流程分析

跟着源码和这幅图就可以一探究竟了！

![image-20200719235554613](.\SpringBoot.assets\image-20200719235554613.png)



## 4、yaml配置注入

### 4.1、yaml语法学习

> 配置文件

SpringBoot使用一个全局的配置文件 ， 配置文件名称是固定的

- application.properties

- - 语法结构 ：key=value

- application.yml

- - 语法结构 ：key：空格 value

**配置文件的作用 ：**修改SpringBoot自动配置的默认值，因为SpringBoot在底层都给我们自动配置好了；

比如我们可以在配置文件中修改Tomcat 默认启动的端口号！测试一下！

```properties
server.port=8081
```



#### 1、yaml概述

> YAML是 "YAML Ain't a Markup Language" （YAML不是一种标记语言）的递归缩写。在开发的这种语言时，YAML 的意思其实是："Yet Another Markup Language"（仍是一种标记语言）

**这种语言以数据作为中心，而不是以标记语言为重点！**

以前的配置文件，大多数都是使用xml来配置；比如一个简单的端口配置，我们来对比下yaml和xml

- 传统xml配置：

  ```xml
  <server>
      <port>8081<port>
  </server>
  ```

- yaml配置：(**冒号后必须有空格！**)

  ```yaml
  server: 
    prot: 8080
  ```

  

#### 2、yaml基础语法

说明：语法要求严格！

1. 空格不能省略
2. 以缩进来控制层级关系，只要是左边对齐的一列数据都是同一个层级的。
3. 属性和值的大小写都是十分敏感的。

> ##### 字面量：普通的值  [ 数字，布尔值，字符串  ]

字面量直接写在后面就可以 ， 字符串默认不用加上双引号或者单引号；

```yaml
K: v
```

注意：

- “ ” 双引号，不会转义字符串里面的特殊字符 ， 特殊字符会作为本身想表示的意思；

  比如 ：name: "xj \n shen"  输出 ：xj 换行  shen

- ' ' 单引号，会转义特殊字符 ， 特殊字符最终会变成和普通字符一样输出

  比如 ：name: ‘xj \n shen’  输出 ：xj \n  shen



> ##### 对象、Map（键值对）

```yaml
#对象、Map格式
k: 
    v1:
    v2:
```

在下一行来写对象的属性和值得关系，注意缩进；比如：

```yaml
student:
    name: xj
    age: 3
```

行内写法

```yaml
student: {name: xj,age: 3}
```



> ##### 数组（ List、set ）

用 - 值表示数组中的一个元素,比如：

```yaml
pets:
 - cat
 - dog
 - pig
```

行内写法

```yaml
pets: [cat,dog,pig]
```

**修改SpringBoot的默认端口号**

配置文件中添加，端口号的参数，就可以切换端口；

```yaml
server: 
 port: 8082
```



### 4.2、注入配置文件

> yaml文件更强大的地方在于，他可以给我们的实体类直接注入匹配值！

#### 1、yaml注入配置文件

​	java程序都需要注册bean到容器中，即添加`@Component`注解

1. 在springboot项目中的resources目录下新建一个文件 application.yml

2. 编写一个实体类 Dog；

   ```java
   package com.xj.pojo;
   
   @Component  //注册bean到容器中
   public class Dog {
       private String name;
       private Integer age;
       
       //有参无参构造、get、set方法、toString()方法  
   }
   ```

3. 思考，我们原来是如何给bean注入属性值的！@Value，给狗狗类测试一下：

   ```java
   @Component //注册bean
   public class Dog {
       @Value("阿黄")
       private String name;
       @Value("18")
       private Integer age;
   }
   ```

4. 在SpringBoot的测试类下注入狗狗输出一下；

   ```java
   @SpringBootTest
   class DemoApplicationTests {
   
       @Autowired //将狗狗自动注入进来
       Dog dog;
   
       @Test
       public void contextLoads() {
           System.out.println(dog); //打印看下狗狗对象
       }
   
   }
   ```

   结果成功输出，@Value注入成功，这是我们原来的办法

5. 我们在编写一个复杂一点的实体类：Person 类

   ```java
   @Component //注册bean到容器中
   public class Person {
       private String name;
       private Integer age;
       private Boolean happy;
       private Date birth;
       private Map<String,Object> maps;
       private List<Object> lists;
       private Dog dog;
       
       //有参无参构造、get、set方法、toString()方法  
   }
   ```

6. 我们来使用yaml配置的方式进行注入，大家写的时候注意区别和优势，我们编写一个yaml配置！

   ```yaml
   person:
     name: xj
     age: 3
     happy: false
     birth: 2020/07/20
     maps: {k1: v1,k2: v2}
     lists:
      - code
      - girl
      - music
     dog:
       name: 旺财
       age: 1
   ```

7. 我们刚才已经把person这个对象的所有值都写好了，我们现在来注入到我们的类中！

   ```java
   /*
   @ConfigurationProperties作用：
   将配置文件中配置的每一个属性的值，映射到这个组件中；
   告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定
   参数 prefix = “person” : 将配置文件中的person下面的所有属性一一对应
   */
   @Component //注册bean
   @ConfigurationProperties(prefix = "person")
   public class Person {
       private String name;
       private Integer age;
       private Boolean happy;
       private Date birth;
       private Map<String,Object> maps;
       private List<Object> lists;
       private Dog dog;
   }
   ```

8. IDEA 提示，springboot配置注解处理器没有找到，让我们看文档，我们可以查看文档，找到一个依赖！

   ```xml
   <!-- 导入配置文件处理器，配置文件进行绑定就会有提示，需要重启 -->
   <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-configuration-processor</artifactId>
     <optional>true</optional>
   </dependency>
   ```

9. 确认以上配置都OK之后，我们去测试类中测试一下：

   ```java
   @SpringBootTest
   class DemoApplicationTests {
   
       @Autowired
       Person person; //将person自动注入进来
   
       @Test
       public void contextLoads() {
           System.out.println(person); //打印person信息
       }
   
   }
   ```

   结果：所有值全部注入成功！


**yaml配置注入到实体类完全OK！**

课堂测试：

1. 将配置文件的key 值 和 属性的值设置为不一样，则结果输出为null，注入失败
2. 在配置一个person2，然后将 @ConfigurationProperties(prefix = "person2") 指向我们的person2；



#### 2、加载指定的配置文件

- **@PropertySource ：**加载指定的配置文件；
- **@configurationProperties**：默认从全局配置文件中获取值；

1. 我们去在resources目录下新建一个**person.properties**文件

   ```properties
   name=xj
   ```

2. 然后在我们的代码中指定加载person.properties文件

    ```java
   @PropertySource(value = "classpath:person.properties")
   @Component // 注册bean
   public class Person {
   
       @Value("${name}") // 使用EL表达式
       private String name;
   
       ......  
   }
   ```

3. 再次输出测试一下：指定配置文件绑定成功！



#### 3、配置文件占位符

> 配置文件还可以编写占位符生成随机数

```yaml
person:
    name: qinjiang${random.uuid} # 随机uuid
    age: ${random.int}  # 随机int
    happy: false
    birth: 2000/01/01
    maps: {k1: v1,k2: v2}
    lists:
      - code
      - girl
      - music
    dog:
      name: ${person.hello:other}_旺财
      age: 1
```



#### 4、回顾properties配置

> 我们上面采用的yaml方法都是最简单的方式，开发中最常用的；也是springboot所推荐的！那我们来唠唠其他的实现方式，道理都是相同的；写还是那样写；配置文件除了yml还有我们之前常用的properties 。

【注意】properties配置文件在写中文的时候，会有乱码 ， 我们需要去IDEA中设置编码格式为UTF-8；

settings-->FileEncodings 中配置；

![image-20200720174242762](.\SpringBoot.assets\image-20200720174242762.png)

测试步骤：

1. 新建一个实体类User

    ```java
   @Component //注册bean
   public class User {
       private String name;
       private int age;
       private String sex;
   }
   ```

2. 编辑配置文件 user.properties

   ```properties
   user1.name=xj
   user1.age=18
   user1.sex=男
   ```

3. 我们在User类上使用@Value来进行注入！

   ```java
   @Component //注册bean
   @PropertySource(value = "classpath:user.properties")
   public class User {
       //直接使用@value
       @Value("${user.name}") //从配置文件中取值
       private String name;
       @Value("#{9*2}")  // #{SPEL} Spring表达式
       private int age;
       @Value("男")  // 字面量
       private String sex;
   }
   ```

4. Springboot测试

   ```java
   @SpringBootTest
   class DemoApplicationTests {
   
       @Autowired
       User user;
   
       @Test
       public void contextLoads() {
           System.out.println(user);
       }
   
   }
   ```

   结果正常输出：



### 4.3、对比小结

> @Value这个使用起来并不友好！我们需要为每个属性单独注解赋值，比较麻烦；我们来看个功能对比图

![image-20200720123346097](.\SpringBoot.assets\image-20200720123346097.png)

1. @ConfigurationProperties只需要写一次即可 ， @Value则需要每个字段都添加
2. 松散绑定：这个什么意思呢? 比如我的yml中写的last-name，这个和lastName是一样的， - 后面跟着的字母默认是大写的。这就是松散绑定。可以测试一下
3. JSR303数据校验 ， 这个就是我们可以在字段是增加一层过滤器验证 ， 可以保证数据的合法性
4. 复杂类型封装，yml中可以封装对象 ， 使用value就不支持

**结论：**

- 配置yml和配置properties都可以获取到值，强烈推荐 yml；
- 如果我们在某个业务中，只需要获取配置文件中的某个值，可以使用一下 @value；
- 如果说，我们专门编写了一个JavaBean来和配置文件进行一一映射，就直接@configurationProperties，不要犹豫！



## 5、JSR303数据校验及多环境切换

> Springboot中可以用`@validated`来校验数据，如果数据异常则会统一抛出异常，方便异常中心统一处理。我们这里来写个注解让我们的name只能支持Email格式

```java
@Component //注册bean
@ConfigurationProperties(prefix = "person")
@Validated  //数据校验
public class Person {

    @Email(message="邮箱格式错误") //name必须是邮箱格式
    private String name;
}
```

运行结果 ：default message [不是一个合法的电子邮件地址];

**使用数据校验，可以保证数据的正确性；** 



### 5.1、常见参数

```java
@NotNull(message="名字不能为空")
private String userName;
@Max(value=120,message="年龄最大不能查过120")
private int age;
@Email(message="邮箱格式错误")
private String email;

// 空检查
@Null       // 验证对象是否为null
@NotNull    // 验证对象是否不为null, 无法查检长度为0的字符串
@NotBlank   // 检查约束字符串是不是Null还有被Trim的长度是否大于0,只对字符串,且会去掉前后空格.
@NotEmpty   // 检查约束元素是否为NULL或者是EMPTY.
    
// Booelan检查
@AssertTrue     // 验证 Boolean 对象是否为 true  
@AssertFalse    // 验证 Boolean 对象是否为 false  
    
// 长度检查
@Size(min=, max=) // 验证对象（Array,Collection,Map,String）长度是否在给定的范围之内  
@Length(min=, max=) // string is between min and max included.

// 日期检查
@Past       // 验证 Date 和 Calendar 对象是否在当前时间之前  
@Future     // 验证 Date 和 Calendar 对象是否在当前时间之后  
@Pattern    // 验证 String 对象是否符合正则表达式的规则

// .......等等
// 除此以外，我们还可以自定义一些数据校验规则
```



### 5.2、多环境切换

> profile是Spring对不同环境提供不同配置功能的支持，可以通过激活不同的环境版本，实现快速切换环境；

#### 1、多配置文件

我们在主配置文件编写的时候，文件名可以是 application-{profile}.properties/yml , 用来指定多个环境版本；

**例如：**

- **application-test.properties** 代表测试环境配置
- **application-dev.properties** 代表开发环境配置

但是Springboot并不会直接启动这些配置文件，它**默认使用application.properties主配置文件**；

我们需要通过一个配置来选择需要激活的环境：

```properties
#比如在配置文件中指定使用dev环境，我们可以通过设置不同的端口号进行测试；
#我们启动SpringBoot，就可以看到已经切换到dev下的配置了；
spring.profiles.active=dev
```



#### 2、yaml的多文档块

> 和properties配置文件中一样，但是使用yml去实现不需要创建多个配置文件，更加方便了 !

```yaml
server:
  port: 8081
#选择要激活那个环境块
spring:
  profiles:
    active: prod

---
server:
  port: 8083
spring:
  profiles: dev #配置环境的名称


---
server:
  port: 8084
spring:
  profiles: prod  #配置环境的名称
```

**注意：如果yml和properties同时都配置了端口，并且没有激活其他环境 ， 默认会使用properties配置文件的！**



### 5.3、配置文件加载位置

> 外部加载配置文件的方式十分多，我们选择最常用的即可，在开发的资源文件中进行配置！

springboot 启动会扫描以下位置的`application.properties`或者`application.yml`文件作为SpringBoot的默认配置文件：

```properties
优先级1：项目路径下的config文件夹配置文件
优先级2：项目路径下配置文件
优先级3：资源路径下的config文件夹配置文件
优先级4：资源路径下配置文件
```

优先级由高到底，高优先级的配置会覆盖低优先级的配置；

**SpringBoot会从这四个位置全部加载主配置文件；互补配置；**

我们在最低级的配置文件中设置一个项目访问路径的配置来测试互补问题；

```properties
#配置项目的访问路径
server.servlet.context-path=/xj
```



### 5.4、拓展，运维小技巧

指定位置加载配置文件

我们还可以通过`spring.config.location`来改变默认的配置文件位置

项目打包好以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；这种情况，一般是后期运维做的多，相同配置，外部指定的配置文件优先级最高

```cmd
java -jar spring-boot-config.jar --spring.config.location=D:/application.properties
```



## 6、自动配置原理

### 6.1、自动配置原理

> 我们以**HttpEncodingAutoConfiguration（Http编码自动配置）**为例解释自动配置原理；

```java
//表示这是一个配置类，和以前编写的配置文件一样，也可以给容器中添加组件；
@Configuration 

//启动指定类的ConfigurationProperties功能；
  //进入这个HttpProperties查看，将配置文件中对应的值和HttpProperties绑定起来；
  //并把HttpProperties加入到ioc容器中
@EnableConfigurationProperties({HttpProperties.class}) 

//Spring底层@Conditional注解
  //根据不同的条件判断，如果满足指定的条件，整个配置类里面的配置就会生效；
  //这里的意思就是判断当前应用是否是web应用，如果是，当前配置类生效
@ConditionalOnWebApplication(
    type = Type.SERVLET
)

//判断当前项目有没有这个类CharacterEncodingFilter；SpringMVC中进行乱码解决的过滤器；
@ConditionalOnClass({CharacterEncodingFilter.class})

//判断配置文件中是否存在某个配置：spring.http.encoding.enabled；
  //如果不存在，判断也是成立的
  //即使我们配置文件中不配置pring.http.encoding.enabled=true，也是默认生效的；
@ConditionalOnProperty(
    prefix = "spring.http.encoding",
    value = {"enabled"},
    matchIfMissing = true
)

public class HttpEncodingAutoConfiguration {
    //他已经和SpringBoot的配置文件映射了
    private final Encoding properties;
    //只有一个有参构造器的情况下，参数的值就会从容器中拿
    public HttpEncodingAutoConfiguration(HttpProperties properties) {
        this.properties = properties.getEncoding();
    }
    
    //给容器中添加一个组件，这个组件的某些值需要从properties中获取
    @Bean
    @ConditionalOnMissingBean //判断容器没有这个组件？
    public CharacterEncodingFilter characterEncodingFilter() {
        CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
        filter.setEncoding(this.properties.getCharset().name());
        filter.setForceRequestEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.REQUEST));
        filter.setForceResponseEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.RESPONSE));
        return filter;
    }
    //。。。。。。。
}
```

**一句话总结 ：根据当前不同的条件判断，决定这个配置类是否生效！**

- 一但这个配置类生效；这个配置类就会给容器中添加各种组件；
- 这些组件的属性是从对应的properties类中获取的，这些类里面的每一个属性又是和配置文件绑定的；
- 所有在配置文件中能配置的属性都是在xxxxProperties类中封装着；
- 配置文件能配置什么就可以参照某个功能对应的这个属性类

```java
//从配置文件中获取指定的值和bean的属性进行绑定
@ConfigurationProperties(prefix = "spring.http") 
public class HttpProperties {
    // .....
}
```

我们去配置文件里面试试前缀，看提示！

![image-20200720125329518](.\SpringBoot.assets\image-20200720125329518.png)

**这就是自动装配的原理！**



### 6.2、了解@Conditional

了解完自动装配的原理后，我们来关注一个细节问题，**自动配置类必须在一定的条件下才能生效；**

- **@Conditional派生注解（Spring注解版原生的@Conditional作用）**

  作用：必须是@Conditional指定的条件成立，才给容器中添加组件，配置配里面的所有内容才生效；

![image-20200720125519708](.\SpringBoot.assets\image-20200720125519708.png)

**那么多的自动配置类，必须在一定的条件下才能生效；也就是说，我们加载了这么多的配置类，但不是所有的都生效了。**

我们怎么知道哪些自动配置类生效？

- **我们可以通过启用 debug=true属性；来让控制台打印自动配置报告，这样我们就可以很方便的知道哪些自动配置类生效；**

  ```properties
  #开启springboot的调试类
  debug=true
  ```

  - Positive matches:（自动配置类启用的：正匹配）

  - Negative matches:（没有启动，没有匹配成功的自动配置类：负匹配）

  - Unconditional classes: （没有条件的类）

  【演示：查看输出的日志】

  掌握吸收理解原理，即可以不变应万变！



### 6.3、小结

1. SpringBoot启动会加载大量的自动配置类
2. 我们看我们需要的功能有没有在SpringBoot默认写好的自动配置类当中；
3. 我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件存在在其中，我们就不需要再手动配置了）
4. 给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们只需要在配置文件中指定这些属性的值即可；

**xxxxAutoConfigurartion：自动配置类；**给容器中添加组件

**xxxxProperties:封装配置文件中相关属性；**



## 7、SpringBoot Web开发

> 一般都是可以直接双击`Shift`按键，进行XXX Properties进行搜索查看

### 7.1、导入静态资源

#### 1、初始化

1. 新建项目`springboot-03-web`
2. 环境测试

#### 2、分析源码

进入`WebMvvcAutoConfiguration.java`程序，找到读取资源的方法

##### 方法一

> SpringBoot中，SpringMVC的web配置都在 WebMvcAutoConfiguration 这个配置类里面；
>
> 我们可以去看看 WebMvcAutoConfigurationAdapter 中有很多配置方法；

有一个方法：`addResourceHandlers `添加资源处理

```java
@Override
public void addResourceHandlers(ResourceHandlerRegistry registry) {
    if (!this.resourceProperties.isAddMappings()) {
        // 已禁用默认资源处理
        logger.debug("Default resource handling disabled");
        return;
    }
    // 缓存控制
    Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
    CacheControl cacheControl = this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl();
    // webjars 配置
    if (!registry.hasMappingForPattern("/webjars/**")) {
        customizeResourceHandlerRegistration(registry.addResourceHandler("/webjars/**")
                                             .addResourceLocations("classpath:/META-INF/resources/webjars/")
                                             .setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
    }
    // 静态资源配置
    String staticPathPattern = this.mvcProperties.getStaticPathPattern();
    if (!registry.hasMappingForPattern(staticPathPattern)) {
        customizeResourceHandlerRegistration(registry.addResourceHandler(staticPathPattern)
                                             .addResourceLocations(getResourceLocations(this.resourceProperties.getStaticLocations()))
                                             .setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
    }
}
```

![image-20200720180309065](.\SpringBoot.assets\image-20200720180309065.png)

读一下源代码：比如所有的` /webjars/**` ， 都需要去 classpath:/META-INF/resources/webjars/ 找对应的资源；

> 什么是webjars 呢？

`Webjars`本质就是以jar包的方式引入我们的静态资源 ， 我们以前要导入一个静态资源文件，直接导入即可。

使用SpringBoot需要使用Webjars，我们可以去搜索一下：

网站：https://www.webjars.org 

要使用jQuery，我们只要要引入jQuery对应版本的pom依赖即可！

```xml
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>3.4.1</version>
</dependency>
```

导入完毕，查看webjars目录结构，并访问Jquery.js文件！

![image-20200720163323208](.\SpringBoot.assets\image-20200720163323208.png)

访问：只要是静态资源，SpringBoot就会去对应的路径寻找资源，我们这里访问：

```
http://localhost:8080/webjars/jquery/3.4.1/jquery.js
```

![image-20200720163335697](.\SpringBoot.assets\image-20200720163335697.png)



##### 方法二

> 那我们项目中要是使用自己的静态资源该怎么导入呢？

我们去找`staticPathPattern`发现第二种映射规则 ：/** , 访问当前的项目任意资源，它会去找 `resourceProperties `这个类，我们可以点进去看一下分析：

（也可以直接双击`Shift`按键，搜索`ResourceProperties`进行查看）

```java
// 进入方法
public String[] getStaticLocations() {
    return this.staticLocations;
}
// 找到对应的值
private String[] staticLocations = CLASSPATH_RESOURCE_LOCATIONS;
// 找到路径
private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { 
    "classpath:/META-INF/resources/",
 	"classpath:/resources/", 
    "classpath:/static/", 
    "classpath:/public/" 
};
```

![image-20200720180445254](.\SpringBoot.assets\image-20200720180445254.png)

`ResourceProperties `可以设置和我们静态资源有关的参数；这里面指向了它会去寻找资源的文件夹，即上面数组的内容。

所以得出结论，以下四个目录存放的静态资源可以被我们识别：

```java
"classpath:/META-INF/resources/"
"classpath:/resources/"
"classpath:/static/"
"classpath:/public/"
```

我们可以在resources根目录下新建对应的文件夹，都可以存放我们的静态文件；

比如我们访问 http://localhost:8080/1.js , 他就会去这些文件夹中寻找对应的静态资源文件；

> #### 即`resoures`在目录下的（可以在每个目录下创建1.js 进行访问测试一下）

![image-20200720154331009](.\SpringBoot.assets\image-20200720154331009.png)

- public （优先级最低）
- resources （优先级最高）
- static  （优先级第二）

优先级：resources > static  > public 



### 7.2、设置首页和图标

> 首页

1. 依然进入`WebMvvcAutoConfiguration.java`程序查找

2. 寻找到关于`index`的代码块

   ![image-20200720180801273](.\SpringBoot.assets\image-20200720180801273.png)

3. 然后点进去`ResourceLoader`进行查看，只需要放在**静态资源文件夹下即可**

   ![image-20200720180942141](.\SpringBoot.assets\image-20200720180942141.png)

4. **所以只需要在静态资源文件夹上创建`index.html`页面即可**

   ![image-20200720181757995](.\SpringBoot.assets\image-20200720181757995.png)

   ![image-20200720181807117](.\SpringBoot.assets\image-20200720181807117.png)

   

> 图标（新版本已经不存在了）

图标需要在2.1.7版本中使用，在新版本中找不到需要降级

![image-20200720155229678](.\SpringBoot.assets\image-20200720155229678.png)

1. 依然进入`WebMvvcAutoConfiguration.java`程序，找到关于`icon`的代码块

   ![image-20200720155340531](.\SpringBoot.assets\image-20200720155340531.png)

2. 所以只需要在静态资源文件夹下面创建一个名为`favicon.icon`图片文件即可

3. 然后关闭使用默认的图标

   ```properties
   # 关闭默认图标
   spring.mvc.favicon.enable=false
   ```

4. 启动浏览器进行查看



> 图标新方法

1. 将一个图片命名为favicon.ico
2. 将ico图标放在`resources/static`目录下，名字是favicon.ico

2. 在html文件中引入

   ```html
   <link rel="shortcut icon" href="static/favicon.ico" type="image/x-icon">
   ```

3. 搞定。现在发现还挺简单的（在配置文件中更改spring.mvc.favicon.enbaled=false现在已经不能用了）

   ![image-20200720182327589](.\SpringBoot.assets\image-20200720182327589.png)

   

### 7.3、模板引擎Thymeleaf

#### 1、引入Thymeleaf

怎么引入呢，对于springboot来说，什么事情不都是一个start的事情嘛，我们去在项目中引入一下。给大家三个网址：

Thymeleaf 官网：https://www.thymeleaf.org/

Thymeleaf 在Github 的主页：https://github.com/thymeleaf/thymeleaf

Spring官方文档：找到我们对应的版本

https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter 

找到对应的pom依赖：可以适当点进源码看下本来的包！

```xml
<!--thymeleaf-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```



#### 2、Thymeleaf分析

> 通过全局搜索 `ThymeleafProperties`进入对应程序查看

1. 查看源码

   ```java
   @ConfigurationProperties(
       prefix = "spring.thymeleaf"
   )
   public class ThymeleafProperties {
       private static final Charset DEFAULT_ENCODING;
       public static final String DEFAULT_PREFIX = "classpath:/templates/";
       public static final String DEFAULT_SUFFIX = ".html";
       private boolean checkTemplate = true;
       private boolean checkTemplateLocation = true;
       private String prefix = "classpath:/templates/";
       private String suffix = ".html";
       private String mode = "HTML";
       private Charset encoding;
   }
   ```

   ![image-20200720183252414](.\SpringBoot.assets\image-20200720183252414.png)

   我们只需要把我们的html页面放在类路径下的templates下，thymeleaf就可以帮我们自动渲染了。

2. 模板放在`templatees`目录下即可



#### 3、跳转测试

1. 编写一个TestController

   ```java
   @Controller
   public class TestController {
       @RequestMapping("/t1")
       public String test1(){
           //classpath:/templates/test.html
           return "test";
       }
   }
   ```
   
2. 编写一个测试页面  test.html 放在 templates 目录下

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   <h1>测试页面</h1>
   
   </body>
   </html>
   ```

3. 启动项目请求测试

   ![image-20200720184621181](.\SpringBoot.assets\image-20200720184621181.png)

   



#### 4、Thymeleaf 语法学习

> Thymeleaf 官网：https://www.thymeleaf.org/ ， 简单看一下官网！我们去下载Thymeleaf的官方文档！

![image-20200721124028997](.\SpringBoot.assets\image-20200721124028997-1595341657328.png)

1. 使用方法`th:元素类`

   ```html
   <!DOCTYPE html>
   <html xmlns:th="http://www.thymeleaf.org">
       <head>
           <title>Good Thymes Virtual Grocery</title>
           <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
       </head>
       <body>
           <p th:text="${home.welcome}">Welcome to our grocery store!</p>
       </body>
   </html>
   ```

![image-20200720162358140](.\SpringBoot.assets\image-20200720162358140.png)

**类别：**

1. th:text  输出字符串

2. th:utext  可以转换html标签

3. 遍历：

   ```html
   <h3 th:each="user:${users}" th:text="${user}"></h3>
   ```

4. 行外写法

   ```html
   <h3 th:each="user:${users}">[[ ${user} ]]</h3>
   ```
   
5. 国际语言

   ```html
   #{login.remember}
   ```

6. 判断是否为空

   ```html
   th:if="${not #strings.isEmpty(msg)}"
   ```

7. 提取公共列表

   - `th:fragment="sidebar"`

   - `th:replace="~{commons/commons::sidebar}"`

     `~{路径/文件名 ::  定义的名称}`

8. 设置时间格式`${#dates.format(emp.getBirth(), 'yyyy-MM-dd HH:mm:ss')}`



#### 5、带参数测试

1. 只带一个参数

   ```java
   @RequestMapping("/t2")
   public String test2(Model model){
       model.addAttribute("msg", "你好");
       //classpath:/templates/test.html
       return "test";
   }
   ```

2. 获取参数

   ```html
   <!DOCTYPE html>
   <html lang="en" xmlns:th="http://www.thymeleaf.org">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
       <h1>测试页面</h1>
       <h3 th:text="${msg}"></h3>
   </body>
   </html>
   ```

3. 带数组

   ```java
   @RequestMapping("/t3")
   public String test3(Model model){
       model.addAttribute("users", Arrays.asList("小明", "小红", "小黄"));
       //classpath:/templates/test.html
       return "test";
   }
   ```

4. 获取数组

   ```html
   <h3 th:each="user:${users}" th:text="${user}"></h3>
   ```

   



### 7.4、装配扩展SpringMVC

#### 1、官网阅读

在进行项目编写前，我们还需要知道一个东西，就是SpringBoot对我们的SpringMVC还做了哪些配置，包括如何扩展，如何定制。

只有把这些都搞清楚了，我们在之后使用才会更加得心应手。途径一：源码分析，途径二：官方文档！

地址 ：https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#boot-features-spring-mvc-auto-configuration

![image-20200720165111519](.\SpringBoot.assets\image-20200720165111519.png)

#### 2、测试

1. 在`config`目录下创建一个`MyMvcConfig.java`

   ```java
   // 扩展SpringMVC
   @Configuration
   public class MyMvcConfig {
   }
   ```

2. 写一个视图解析器，实现`ViewResolver`方法

   ```java
   // 扩展SpringMVC
   @Configuration
   public class MyMvcConfig {
       @Bean //放到bean中
       public ViewResolver myViewResolver(){
           return new MyViewResolver();
       }
   
       //我们写一个静态内部类，视图解析器就需要实现ViewResolver接口
       private static class MyViewResolver implements ViewResolver{
           @Override
           public View resolveViewName(String s, Locale locale) throws Exception {
               return null;
           }
       }
   }
   ```

3. 怎么看我们自己写的视图解析器有没有起作用呢？

   我们给 DispatcherServlet 中的 doDispatch方法 加个断点进行调试一下，因为所有的请求都会走到这个方法中

   ![image-20200720212047046](.\SpringBoot.assets\image-20200720212047046.png)

4. 我们启动我们的项目，然后随便访问一个页面，看一下Debug信息；找到this

   ![image-20200720170148662](.\SpringBoot.assets\image-20200720170148662.png)

   找到视图解析器，我们看到我们自己定义的就在这里了；在`viewResolvers`目录下

   ![image-20200720212359063](.\SpringBoot.assets\image-20200720212359063.png)

   所以说，我们如果想要使用自己定制化的东西，我们只需要给容器中添加这个组件就好了！剩下的事情SpringBoot就会帮我们做了！



#### 3、转换器和格式化器

1. 找到格式化转换器：

   ```java
   @Bean
   @Override
   public FormattingConversionService mvcConversionService() {
       // 拿到配置文件中的格式化规则
       WebConversionService conversionService = 
           new WebConversionService(this.mvcProperties.getDateFormat());
       addFormatters(conversionService);
       return conversionService;
   }
   ```

   ![image-20200720220132963](.\SpringBoot.assets\image-20200720220132963.png)

   ![image-20200720220428144](.\SpringBoot.assets\image-20200720220428144.png)

2. 点击去：

   ```java
   @Deprecated
   @DeprecatedConfigurationProperty(replacement = "spring.mvc.format.date")
   public String getDateFormat() {
       return this.format.getDate();
   }
   
   @Deprecated
   public void setDateFormat(String dateFormat) {
       this.format.setDate(dateFormat);
   }
   
   /**
   * Date-time format to use, for example `yyyy-MM-dd HH:mm:ss`.
   */
   private String dateTime;
   ```

   ![image-20200720220526096](.\SpringBoot.assets\image-20200720220526096.png)

   ![image-20200720220352754](.\SpringBoot.assets\image-20200720220352754.png)

   

   可以看到在我们的Properties文件中，我们可以进行自动配置它！

   如果配置了自己的格式化方式，就会注册到Bean中生效，我们可以在配置文件中配置日期格式化的规则：(奇怪的是这没有生效)

   ![image-20200720222511746](.\SpringBoot.assets\image-20200720222511746.png)



#### 4、视图跳转

> 我们要做的就是编写一个`@Configuration`注解类，并且类型要为WebMvcConfigurer，还不能标注@EnableWebMvc注解；我们去自己写一个；我们新建一个包叫config，写一个类MyMvcConfig；

1. 使用`@Configuration`注解
2. 实现`WebMvcConfigurer`接口
3. 实现`addViewControllers`方法

```java
//应为类型要求为WebMvcConfigurer，所以我们实现其接口
//可以使用自定义类扩展MVC的功能
@Configuration
public class MyMvcConfig02 implements WebMvcConfigurer {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        // 浏览器发送/test,就会跳转到test页面；
        registry.addViewController("/test").setViewName("test");
    }
}
```

我们去浏览器访问一下：

![image-20200720231843551](.\SpringBoot.assets\image-20200720231843551.png)

**确实也跳转过来了！所以说，我们要扩展SpringMVC，官方就推荐我们这么去使用，既保SpringBoot留所有的自动配置，也能用我们扩展的配置！**



#### 5、全面接管SpringMVC

官方文档：

```tex
If you want to take complete control of Spring MVCyou can add your own @Configuration annotated with @EnableWebMvc.
```

全面接管即：SpringBoot对SpringMVC的自动配置不需要了，所有都是我们自己去配置！

只需在我们的配置类中要加一个`@EnableWebMvc`。

我们看下如果我们全面接管了SpringMVC了，我们之前SpringBoot给我们配置的静态资源映射一定会无效，我们可以去测试一下；

> #### 不加注解之前，访问首页：

![image-20200720232107383](.\SpringBoot.assets\image-20200720232107383.png)



> #### 给配置类加上注解：`@EnableWebMvc`

![image-20200720232021334](.\SpringBoot.assets\image-20200720232021334.png)

**当然，我们开发中，不推荐使用全面接管SpringMVC**

**原因：**

1. 点进去`@EnableWebMvc`这里发现它是导入了一个类，我们可以继续进去看

   ```java
   @Import({DelegatingWebMvcConfiguration.class})
   public @interface EnableWebMvc {
   }
   ```

2. 它继承了一个父类 `WebMvcConfigurationSupport`

   ```java
   public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
     // ......
   }
   ```

3. 我们来回顾一下Webmvc自动配置类

   ```java
   @Configuration(proxyBeanMethods = false)
   @ConditionalOnWebApplication(type = Type.SERVLET)
   @ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
   
   // 这个注解的意思就是：容器中没有这个组件的时候，这个自动配置类才生效
   @ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
   @AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
   @AutoConfigureAfter({ DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class,
       ValidationAutoConfiguration.class })
   public class WebMvcAutoConfiguration {
       
   }
   ```

   ![image-20200720232502878](.\SpringBoot.assets\image-20200720232502878.png)

   总结一句话：`@EnableWebMvc`将`WebMvcConfigurationSupport`组件导入进来了；所以自动配置就失效了。

   而导入的WebMvcConfigurationSupport只是SpringMVC最基本的功能！

   **在SpringBoot中会有非常多的扩展配置，只要看见了这个，我们就应该多留心注意~**



## 8、整合JDBC

### 8.1、SpringData简介

对于数据访问层，无论是 SQL(关系型数据库) 还是 NOSQL(非关系型数据库)，Spring Boot 底层都是采用 Spring Data 的方式进行统一处理。

Spring Boot 底层都是采用 Spring Data 的方式进行统一处理各种数据库，Spring Data 也是 Spring 中与 Spring Boot、Spring Cloud 等齐名的知名项目。

Sping Data 官网：https://spring.io/projects/spring-data

数据库相关的启动器 ：可以参考官方文档：

https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter

### 8.2、整合JDBC

1. 新建SpringBoot项目`springboot-04-jdbc`，导入相应模块

   ![image-20200724123329817](.\SpringBoot.assets\image-20200724123329817.png)

   ![image-20200724123353460](.\SpringBoot.assets\image-20200724123353460.png)

2. 清理项目文件

   ![image-20200724123602683](.\SpringBoot.assets\image-20200724123602683.png)

3. 编写`application.yml`文件接数据库，报了如下错误

   ![image-20200724124804713](.\SpringBoot.assets\image-20200724124804713.png)

   其实这个问题是由于MySQL 这个jar 包依赖类型默认是runtime ，也就是说只有运行时生效，所以虽然这里报错，但是不影响你代码运行。参考网站：https://blog.csdn.net/hadues/article/details/82354658

   解决方法：修改导入的依赖

   ![image-20200724125302673](.\SpringBoot.assets\image-20200724125302673.png)

   ```xml
   <dependency>
   	  <groupId>mysql</groupId>
   	  <artifactId>mysql-connector-java</artifactId>
   	  <scope>compile</scope>
   </dependency>
   ```

4. 配置完这一些东西后，我们就可以直接去使用了，因为SpringBoot已经默认帮我们进行了自动配置；去测试类测试一下

   ```java
   @SpringBootTest
   class Springboot04JdbcApplicationTests {
       //DI注入数据源
       @Autowired
       DataSource dataSource;
   
       @Test
       void contextLoads() throws SQLException {
           // 看一下默认的数据源
           System.out.println(dataSource.getClass());
           // 获取连接
           Connection connection = dataSource.getConnection();
           System.out.println(connection);
           // 关闭连接
           connection.close();
       }
   
   }
   ```

   ![image-20200724125715138](.\SpringBoot.assets\image-20200724125715138.png)

   默认给我们配置的数据源为 : class com.zaxxer.hikari.HikariDataSource

5. 全局搜索进入`DataSourceAutoConfiguration`，查看`PooledDataSourceConfiguration`

   ```java
   @Import({ DataSourceConfiguration.Hikari.class, DataSourceConfiguration.Tomcat.class,
            DataSourceConfiguration.Dbcp2.class, DataSourceConfiguration.Generic.class,
            DataSourceJmxConfiguration.class })
   protected static class PooledDataSourceConfiguration {
   
   }
   ```

   > 可以看到，这里默认实用的是`DataSourceConfiguration.Hikari.class`数据源，HikariDataSource 号称 Java WEB 当前速度最快的数据源，相比于传统的 C3P0 、DBCP、Tomcat jdbc 等连接池更加优秀；
   >
   > 可以使用 spring.datasource.type 指定自定义的数据源类型

### 8.3、JDBCTemplate

1. 有了数据源(com.zaxxer.hikari.HikariDataSource)，然后可以拿到数据库连接(java.sql.Connection)，有了连接，就可以使用原生的 JDBC 语句来操作数据库；
2. 即使不使用第三方第数据库操作框架，如 MyBatis等，Spring 本身也对原生的JDBC 做了轻量级的封装，即JdbcTemplate。
3. 数据库操作的所有 CRUD 方法都在 JdbcTemplate 中。
4. SpringBoot 不仅提供了默认的数据源，同时默认已经配置好了 JdbcTemplate 放在了容器中，程序员只需自己注入即可使用
5. JdbcTemplate 的自动配置是依赖 org.springframework.boot.autoconfigure.jdbc 包下的 JdbcTemplateConfiguration 类

**JdbcTemplate主要提供以下几类方法：**

- execute方法：可以用于执行任何SQL语句，一般用于执行DDL语句；
- update方法及batchUpdate方法：update方法用于执行新增、修改、删除等语句；batchUpdate方法用于执行批处理相关语句；
- query方法及queryForXXX方法：用于执行查询相关语句；
- call方法：用于执行存储过程、函数相关语句。 

**测试**

1. 编写一个Controllerr，注入 jdbcTemplate，编写测试方法进行访问测试

   ```java
   @RestController
   public class jdbcController {
       /**
        * Spring Boot 默认提供了数据源，默认提供了 org.springframework.jdbc.core.JdbcTemplate
        * JdbcTemplate 中会自己注入数据源，用于简化 JDBC操作
        * 还能避免一些常见的错误,使用起来也不用再自己来关闭数据库连接
        */
       @Autowired
       JdbcTemplate jdbcTemplate;
       // 查询全部用户
       @GetMapping("/list")
       public List<Map<String,Object>> userList(){
           String sql = "select * from user";
           List<Map<String, Object>> list = jdbcTemplate.queryForList(sql);
           return list;
       }
       // 新增一个用户
       @GetMapping("/add")
       public String addUser(){
           String sql = "insert into user(id, name, pwd) values(9, '晓江', '1234')";
           jdbcTemplate.update(sql);
           return "insert ok";
       }
       // 修改用户信息
       @GetMapping("update/{id}")
       public String updateUser(@PathVariable("id") int id){
           String sql = "update user set name=?,pwd=? where id=" + id;
           Object[] objects = new Object[2];
           objects[0] = "晓江2";
           objects[1] = "123456";
           jdbcTemplate.update(sql, objects);
           return "update ok";
       }
       // 删除用户
       @GetMapping("delete/{id}")
       public String deleteUser(@PathVariable("id") int id){
           String sql = "delete from user where id=?";
           jdbcTemplate.update(sql, id);
           return "delete ok";
       }
   }
   ```

2. 测试请求，结果正常

   ![image-20200724131917857](.\SpringBoot.assets\image-20200724131917857.png)

   到此，CURD的基本操作，使用 JDBC 就搞定了。



## 9、整合Druid

### 9.1、Druid简介

> Druid 是阿里巴巴开源平台上一个数据库连接池实现，结合了 C3P0、DBCP 等 DB 池的优点，同时加入了日志监控。
>
> Druid 可以很好的监控 DB 池连接和 SQL 的执行情况，天生就是针对监控而生的 DB 连接池。
>
> Github地址：https://github.com/alibaba/druid/

com.alibaba.druid.pool.DruidDataSource 基本配置参数如下：


![img](.\SpringBoot.assets\640.webp)

![img](.\SpringBoot.assets\640-1595568918456.webp)

![img](.\SpringBoot.assets\640-1595568924094.webp)



### 9.2、配置数据源

1. 添加上Druid数据源依赖

   ```xml
   <!--druid -->
   <dependency>
       <groupId>com.alibaba</groupId>
       <artifactId>druid</artifactId>
       <version>1.1.23</version>
   </dependency>
   ```

2. 切换数据源，因为Spring Boot 2.0 以上默认使用 com.zaxxer.hikari.HikariDataSource 数据源，但可以通过 `spring.datasource.type` 指定数据源

   ```yaml
   spring:
     datasource:
       type: com.alibaba.druid.pool.DruidDataSource # 自定义数据源
   ```

3. 数据源切换之后，运行刚才的测试程序，获取`DataSource`

   ![image-20200724134253131](.\SpringBoot.assets\image-20200724134253131.png)

   可以看到已经切换到Druid

4. 设置数据源连接初始化大小、最大连接数、等待时间、最小连接数 等设置项；可以查看源码

   ```yaml
   spring:
     datasource:
       username: root
       password: 1234
       url: jdbc:mysql://localhost:3306/mybatis?serverTimezone=GMT&createDatabaseIfNotExist=true&useUnicode=true&characterEncoding=utf-8&autoReconnect=true
       driver-class-name: com.mysql.cj.jdbc.Driver
       type: com.alibaba.druid.pool.DruidDataSource # 自定义数据源
       
       #Spring Boot 默认是不注入这些属性值的，需要自己绑定
       #druid 数据源专有配置
       initialSize: 5
       minIdle: 5
       maxActive: 20
       maxWait: 60000
       timeBetweenEvictionRunsMillis: 60000
       minEvictableIdleTimeMillis: 300000
       validationQuery: SELECT 1 FROM DUAL
       testWhileIdle: true
       testOnBorrow: false
       testOnReturn: false
       poolPreparedStatements: true
   
       #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
       #如果允许时报错  java.lang.ClassNotFoundException: org.apache.log4j.Priority
       #则导入 log4j 依赖即可，Maven 地址：https://mvnrepository.com/artifact/log4j/log4j
       filters: stat,wall,log4j
       maxPoolPreparedStatementPerConnectionSize: 20
       useGlobalDataSourceStat: true
       connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
   ```

5. 导入Log4j 的依赖

   ```xml
   <!-- https://mvnrepository.com/artifact/log4j/log4j -->
   <dependency>
       <groupId>log4j</groupId>
       <artifactId>log4j</artifactId>
       <version>1.2.17</version>
   </dependency>
   ```

6. 现在需要程序员自己为 DruidDataSource 绑定全局配置文件中的参数，再添加到容器中，而不再使用 Spring Boot 的自动生成了；我们需要 自己添加 DruidDataSource 组件到容器中，并绑定属性；

   ```java
   package com.xj.config;
   
   import com.alibaba.druid.pool.DruidDataSource;
   import org.springframework.boot.context.properties.ConfigurationProperties;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   
   import javax.sql.DataSource;
   
   @Configuration
   public class DruidConfig {
       /*
       将自定义的 Druid数据源添加到容器中，不再让 Spring Boot 自动创建
       绑定全局配置文件中的 druid 数据源属性到 com.alibaba.druid.pool.DruidDataSource从而让它们生效
       @ConfigurationProperties(prefix = "spring.datasource")：作用就是将 全局配置文件中
       前缀为 spring.datasource的属性值注入到 com.alibaba.druid.pool.DruidDataSource 的同名参数中
     */
       @ConfigurationProperties(prefix = "spring.datasource")
       @Bean
       public DataSource druidDataSource() {
           return new DruidDataSource();
       }
   }
   ```

7. 去测试类中测试一下；看是否成功！

   ```java
   @SpringBootTest
   class Springboot04JdbcApplicationTests {
       //DI注入数据源
       @Autowired
       DataSource dataSource;
   
       @Test
       void contextLoads() throws SQLException {
           // 看一下默认的数据源
           System.out.println(dataSource.getClass());
           // 获取连接
           Connection connection = dataSource.getConnection();
           System.out.println(connection);
   
           DruidDataSource druidDataSource = (DruidDataSource)dataSource;
           System.out.println("druidDataSource 数据源最大连接数：" + druidDataSource.getMaxActive());
           System.out.println("druidDataSource 数据源初始化连接数：" + druidDataSource.getInitialSize());
   
           // 关闭连接
           connection.close();
       }
   }
   ```

   ![image-20200724135806742](.\SpringBoot.assets\image-20200724135806742.png)



### 9.3、配置Druid数据源监控

> Druid 数据源具有监控的功能，并提供了一个 web 界面方便用户查看，类似安装 路由器 时，人家也提供了一个默认的 web 页面。

1. 所以第一步需要在`config`中设置 Druid 的后台管理页面，比如 登录账号、密码 等；配置后台管理；

   ```java
   //配置 Druid 监控管理后台的Servlet；
   //内置 Servlet 容器时没有web.xml文件，所以使用 SpringBoot 的注册 Servlet 方式
   @Bean
   public ServletRegistrationBean servletRegistrationBean(){
       ServletRegistrationBean bean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");
   
       // 这些参数可以在 com.alibaba.druid.support.http.StatViewServlet
       // 的父类 com.alibaba.druid.support.http.ResourceServlet 中找到
       Map<String, String> initParams = new HashMap<>();
       initParams.put("loginUsername", "admin"); //后台管理界面的登录账号
       initParams.put("loginPassword", "123456"); //后台管理界面的登录密码
   
       //后台允许谁可以访问
       //initParams.put("allow", "localhost")：表示只有本机可以访问
       //为空或者为null时，表示允许所有访问
       initParams.put("allow", "");
       //deny：Druid 后台拒绝谁访问
       //initParams.put("lxj", "192.168.1.20");表示禁止此ip访问
   
       //设置初始化参数
       bean.setInitParameters(initParams);
       return bean;
   }
   ```

2. 配置完毕后，我们可以访问 ：http://localhost:8080/druid/login.html

   ![image-20200724140727180](.\SpringBoot.assets\image-20200724140727180.png)

   ![image-20200724140807518](.\SpringBoot.assets\image-20200724140807518.png)

3. 当我们再次运行sql查询的时候，这里可以检测到

   ![image-20200724140929304](.\SpringBoot.assets\image-20200724140929304.png)

4. 配置 Druid web 监控 filter 过滤器

   ```java
   //配置 Druid 监控 之  web 监控的 filter
   //WebStatFilter：用于配置Web和Druid数据源之间的管理关联监控统计
   @Bean
   public FilterRegistrationBean webStatFilter() {
       FilterRegistrationBean bean = new FilterRegistrationBean();
       bean.setFilter(new WebStatFilter());
   
       //exclusions：设置哪些请求进行过滤排除掉，从而不进行统计
       Map<String, String> initParams = new HashMap<>();
       initParams.put("exclusions", "*.js,*.css,/druid/*,/jdbc/*");
       bean.setInitParameters(initParams);
   
       //"/*" 表示过滤所有请求
       bean.setUrlPatterns(Arrays.asList("/*"));
       return bean;
   }
   ```

   

## 10、整合MyBatis

> 官方文档：http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/
>
> Maven仓库地址：https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter/2.1.1

![image-20200724141412283](.\SpringBoot.assets\image-20200724141412283.png)

**整合测试**

1. 导入 MyBatis 的依赖

   ```xml
   <dependency>
       <groupId>org.mybatis.spring.boot</groupId>
       <artifactId>mybatis-spring-boot-starter</artifactId>
       <version>2.1.3</version>
   </dependency>
   ```

2. 配置数据库连接信息

   ```yaml
   spring:
     datasource:
       username: root
       password: 1234
       url: jdbc:mysql://localhost:3306/mybatis?serverTimezone=GMT&createDatabaseIfNotExist=true&useUnicode=true&characterEncoding=utf-8&autoReconnect=true
   ```

3. 测试数据库是否连接成功

4. 创建实体类，导入 Lombok

   ```xml
   <!-- Lombok -->
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <version>1.18.12</version>
       <scope>provided</scope>
   </dependency>
   ```

   ```java
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class User {
       private int id;
       private String name;
       private String pwd;
   }
   ```

5. 创建`mapper`目录以及对应的 Mapper 接口，需要导入两个注解@Mapper、@Repository

   ```java
   @RestController
   @Mapper
   public class UserController {
       // 先获取Mapper
       @Autowired
       UserMapper userMapper;
   
       // 查询全部用户
       @GetMapping("/getUsers")
       public List<User> getUsers(){
           List<User> users = userMapper.getUsers();
           return users;
       }
       // 根据id查询用户
       @GetMapping("/getUserById/{id}")
       public User getUserById(@PathVariable int id){
           User user = userMapper.getUserById(id);
           return user;
       }
       // 删除用户
       @GetMapping("/deleteUserById/{id}")
       public String deleteUserById(@PathVariable("id") int id){
           userMapper.deleteUserById(id);
           return "delete ok";
       }
       // 增加用户
       @GetMapping("/addUser")
       public String addUser(){
           // 创建一个用户
           User user = new User();
           user.setName("晓江3");
           user.setId(9);
           user.setPwd("12345");
           userMapper.addUser(user);
           return "addUser ok";
       }
       // 修改用户
       @GetMapping("/updateUser")
       public String updateUser(){
           // 创建一个用户
           User user = new User();
           user.setName("晓江4");
           user.setId(9);
           user.setPwd("1234567");
           userMapper.updateUser(user);
           return "update ok";
       }
   }
   ```

6. 在`resources`目录下创建`mybatis`文件夹，然后创建`mapper`文件夹，最后在里面创建对应的`UserMapper.xml`映射文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.xj.mapper.UserMapper">
       <select id="getUsers" resultType="User">
           select * from user
       </select>
       <select id="getUserById" resultType="User">
           select * from user where id = #{id}
       </select>
       <delete id="deleteUserById" parameterType="int">
           delete from user where id = #{id}
       </delete>
       <insert id="addUser" parameterType="User">
           insert into user(id, name, pwd) values (#{id}, #{name}, #{pwd})
       </insert>
       <update id="updateUser" parameterType="User">
           update user set name = #{name}, pwd = #{pwd} where id = #{id}
       </update>
   
   </mapper>
   ```

7. 在`application.yml`配置文件中，增加配置

   ```yaml
   # 整合MyBatis
   mybatis:
     type-aliases-package: com.xj.pojo
     mapper-locations: classpath:mybatis/mapper/*.xml
   ```

8. 编写Controller类进行测试

   ```java
   @RestController
   public class UserController {
       // 先获取Mapper
       @Autowired
       UserMapper userMapper;
   
       // 查询全部用户
       @GetMapping("/getUsers")
       public List<User> getUsers(){
           List<User> users = userMapper.getUsers();
           return users;
       }
       // 根据id查询用户
       @GetMapping("/getUserById/{id}")
       public User getUserById(@PathVariable int id){
           User user = userMapper.getUserById(id);
           return user;
       }
       // 删除用户
       @GetMapping("/deleteUserById/{id}")
       public String deleteUserById(@PathVariable("id") int id){
           userMapper.deleteUserById(id);
           return "delete ok";
       }
       // 增加用户
       @GetMapping("/addUser")
       public String addUser(){
           // 创建一个用户
           User user = new User();
           user.setName("晓江3");
           user.setId(9);
           user.setPwd("12345");
           userMapper.addUser(user);
           return "addUser ok";
       }
       // 修改用户
       @GetMapping("/updateUser")
       public String updateUser(){
           // 创建一个用户
           User user = new User();
           user.setName("晓江4");
           user.setId(9);
           user.setPwd("1234567");
           userMapper.updateUser(user);
           return "update ok";
       }
   }
   ```

9. 项目结构

   ![image-20200724150106195](.\SpringBoot.assets\image-20200724150106195.png)




