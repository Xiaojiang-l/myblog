## 1、Shiro简介

### 1.1、什么是Shiro

- Apache Shiro是一个强大且易用的Java安全框架,执行身份验证、授权、密码和会话管理。

- 使用Shiro的易于理解的API,您可以快速、轻松地获得任何应用程序,从最小的移动应用程序到最大的网络和企业应用程序。

- 下载地址：https://shiro.apache.org/

- 文档地址：https://shiro.apache.org/tutorial.html

  ![image-20200724185658710](.\Shiro.assets\image-20200724185658710.png)

### 1.2、功能

![image-20200724185922358](.\Shiro.assets\image-20200724185922358.png)





### 1.3、Shiro架构

![image-20200725180525995](.\Shiro.assets\image-20200725180525995.png)

![img](.\Shiro.assets\9825bc315c6034a8f93c7d0cce13495408237665.jpg)



## 2、shiro程序

### 2.1、环境搭建

快速入门源代码地址：https://github.com/apache/shiro/tree/master/samples/quickstart

1. 新建一个普通的Maven项目，`springboot-06-shiro`

2. 删除src目录，创建一个模块`hello-shiro`

3. 导入maven依赖

   ```xml
   <dependencies>
       <dependency>
           <groupId>org.apache.shiro</groupId>
           <artifactId>shiro-core</artifactId>
           <version>1.4.1</version>
       </dependency>
       <!-- SLF4J -->
       <dependency>
           <groupId>org.slf4j</groupId>
           <artifactId>slf4j-simple</artifactId>
           <version>1.7.21</version>
       </dependency>
       <dependency>
           <groupId>org.slf4j</groupId>
           <artifactId>jcl-over-slf4j</artifactId>
           <version>1.7.21</version>
       </dependency>
       <dependency>
           <groupId>log4j</groupId>
           <artifactId>log4j</artifactId>
           <version>1.2.17</version>
       </dependency>
   </dependencies>
   ```

4. 配置`log4j.properties`文件

   ```properties
   log4j.rootLogger=INFO, stdout
   
   log4j.appender.stdout=org.apache.log4j.ConsoleAppender
   log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
   log4j.appender.stdout.layout.ConversionPattern=%d %p [%c] - %m %n
   
   # General Apache libraries
   log4j.logger.org.apache=WARN
   
   # Spring
   log4j.logger.org.springframework=WARN
   
   # Default Shiro logging
   log4j.logger.org.apache.shiro=INFO
   
   # Disable verbose logging
   log4j.logger.org.apache.shiro.util.ThreadContext=WARN
   log4j.logger.org.apache.shiro.cache.ehcache.EhCache=WARN
   ```

5. 创建`shiro.ini`文件

   ```ini
   [users]
   root = secret, admin
   guest = guest, guest
   presidentskroob = 12345, president
   darkhelmet = ludicrousspeed, darklord, schwartz
   lonestarr = vespa, goodguy, schwartz
   
   # -----------------------------------------------------------------------------
   # Roles with assigned permissions
   # roleName = perm1, perm2, ..., permN
   # -----------------------------------------------------------------------------
   [roles]
   admin = *
   schwartz = lightsaber:*
   goodguy = winnebago:drive:eagle5
   ```

6. 安装ini插件

   ![image-20200725165609447](.\Shiro.assets\image-20200725165609447.png)

   ![image-20200725165617050](.\Shiro.assets\image-20200725165617050.png)

   完成之后重启即可

   如果还是不亮，就需要进行如下设置

   ![image-20200725172856203](.\Shiro.assets\image-20200725172856203.png)

7. 创建`Quickstart.java`文件

   ```java
   import org.apache.shiro.SecurityUtils;
   import org.apache.shiro.authc.*;
   import org.apache.shiro.ini.IniSecurityManagerFactory;
   import org.apache.shiro.mgt.SecurityManager;
   import org.apache.shiro.session.Session;
   import org.apache.shiro.subject.Subject;
   import org.apache.shiro.lang.util.Factory;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   
   
   /**
    * Simple Quickstart application showing how to use Shiro's API.
    *
    * @since 0.9 RC2
    */
   public class Quickstart {
   
       private static final transient Logger log = LoggerFactory.getLogger(Quickstart.class);
   
   
       public static void main(String[] args) {
   
           // The easiest way to create a Shiro SecurityManager with configured
           // realms, users, roles and permissions is to use the simple INI config.
           // We'll do that by using a factory that can ingest a .ini file and
           // return a SecurityManager instance:
   
           // Use the shiro.ini file at the root of the classpath
           // (file: and url: prefixes load from files and urls respectively):
           Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
           SecurityManager securityManager = factory.getInstance();
   
           // for this simple example quickstart, make the SecurityManager
           // accessible as a JVM singleton.  Most applications wouldn't do this
           // and instead rely on their container configuration or web.xml for
           // webapps.  That is outside the scope of this simple quickstart, so
           // we'll just do the bare minimum so you can continue to get a feel
           // for things.
           SecurityUtils.setSecurityManager(securityManager);
   
           // Now that a simple Shiro environment is set up, let's see what you can do:
   
           // get the currently executing user:
           Subject currentUser = SecurityUtils.getSubject();
   
           // Do some stuff with a Session (no need for a web or EJB container!!!)
           Session session = currentUser.getSession();
           session.setAttribute("someKey", "aValue");
           String value = (String) session.getAttribute("someKey");
           if (value.equals("aValue")) {
               log.info("Retrieved the correct value! [" + value + "]");
           }
   
           // let's login the current user so we can check against roles and permissions:
           if (!currentUser.isAuthenticated()) {
               UsernamePasswordToken token = new UsernamePasswordToken("lonestarr", "vespa");
               token.setRememberMe(true);
               try {
                   currentUser.login(token);
               } catch (UnknownAccountException uae) {
                   log.info("There is no user with username of " + token.getPrincipal());
               } catch (IncorrectCredentialsException ice) {
                   log.info("Password for account " + token.getPrincipal() + " was incorrect!");
               } catch (LockedAccountException lae) {
                   log.info("The account for username " + token.getPrincipal() + " is locked.  " +
                           "Please contact your administrator to unlock it.");
               }
               // ... catch more exceptions here (maybe custom ones specific to your application?
               catch (AuthenticationException ae) {
                   //unexpected condition?  error?
               }
           }
   
           //say who they are:
           //print their identifying principal (in this case, a username):
           log.info("User [" + currentUser.getPrincipal() + "] logged in successfully.");
   
           //test a role:
           if (currentUser.hasRole("schwartz")) {
               log.info("May the Schwartz be with you!");
           } else {
               log.info("Hello, mere mortal.");
           }
   
           //test a typed permission (not instance-level)
           if (currentUser.isPermitted("lightsaber:wield")) {
               log.info("You may use a lightsaber ring.  Use it wisely.");
           } else {
               log.info("Sorry, lightsaber rings are for schwartz masters only.");
           }
   
           //a (very powerful) Instance Level permission:
           if (currentUser.isPermitted("winnebago:drive:eagle5")) {
               log.info("You are permitted to 'drive' the winnebago with license plate (id) 'eagle5'.  " +
                       "Here are the keys - have fun!");
           } else {
               log.info("Sorry, you aren't allowed to drive the 'eagle5' winnebago!");
           }
   
           //all done - log out!
           currentUser.logout();
   
           System.exit(0);
       }
   }
   ```

8. 启动项目，只打印出日志信息。如果报错了，检查一下maven依赖中是否存在`<scope>test</scope>`，删掉即可。

   ![image-20200725173253571](.\Shiro.assets\image-20200725173253571.png)



### 2.2、代码分析

1. 源代码

   ```java
   import org.apache.shiro.SecurityUtils;
   import org.apache.shiro.authc.*;
   import org.apache.shiro.config.IniSecurityManagerFactory;
   import org.apache.shiro.mgt.SecurityManager;
   import org.apache.shiro.session.Session;
   import org.apache.shiro.subject.Subject;
   import org.apache.shiro.util.Factory;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   
   public class Quickstart {
       private static final transient Logger log = LoggerFactory.getLogger(Quickstart.class);
   
       public static void main(String[] args) {
   
           // 这三步固定的
           Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
           SecurityManager securityManager = factory.getInstance();
           SecurityUtils.setSecurityManager(securityManager);
   
           // get the currently executing user:
           // 获取当前用户对象 Subject
           Subject currentUser = SecurityUtils.getSubject();
   
           // Do some stuff with a Session (no need for a web or EJB container!!!)
           // 通过当前用户获取session，然后存值，取值
           Session session = currentUser.getSession();
           session.setAttribute("someKey", "aValue");
           String value = (String) session.getAttribute("someKey");
           if (value.equals("aValue")) {
               log.info("Subject==>session中的取值[" + value + "]");
           }
   
           // let's login the current user so we can check against roles and permissions:
           // 判断当前用户是否被认证
           if (!currentUser.isAuthenticated()) {
               // 生成Token令牌
               UsernamePasswordToken token = new UsernamePasswordToken("lonestarr", "vespa");
               // 设置记住我
               token.setRememberMe(true);
   
               try {
                   // 执行登录操作
                   currentUser.login(token);
               } catch (UnknownAccountException uae) {
                   // 用户不存在
                   log.info("There is no user with username of " + token.getPrincipal());
               } catch (IncorrectCredentialsException ice) {
                   // 密码错误
                   log.info("Password for account " + token.getPrincipal() + " was incorrect!");
               } catch (LockedAccountException lae) {
                   // 用户被锁定
                   log.info("The account for username " + token.getPrincipal() + " is locked.  " +
                           "Please contact your administrator to unlock it.");
               }
               // ... catch more exceptions here (maybe custom ones specific to your application?
               catch (AuthenticationException ae) {
                   // 认证异常
                   //unexpected condition?  error?
               }
           }
   
           /* 以下为权限判断 */
           //say who they are:
           //print their identifying principal (in this case, a username):
           // 打印用户信息
           log.info("User [" + currentUser.getPrincipal() + "] logged in successfully.");
   
           //test a role:
           // 判断角色
           if (currentUser.hasRole("schwartz")) {
               log.info("May the Schwartz be with you!");
           } else {
               log.info("Hello, mere mortal.");
           }
   
           // 粗粒度，简单的认证
           //test a typed permission (not instance-level)
           if (currentUser.isPermitted("lightsaber:wield")) {
               log.info("You may use a lightsaber ring.  Use it wisely.");
           } else {
               log.info("Sorry, lightsaber rings are for schwartz masters only.");
           }
           // 细粒度
           //a (very powerful) Instance Level permission:
           if (currentUser.isPermitted("winnebago:drive:eagle5")) {
               log.info("You are permitted to 'drive' the winnebago with license plate (id) 'eagle5'.  " +
                       "Here are the keys - have fun!");
           } else {
               log.info("Sorry, you aren't allowed to drive the 'eagle5' winnebago!");
           }
   
           //all done - log out!
           // 注销
           currentUser.logout();
   
           System.exit(0);
       }
   }
   ```

2. 主要方法：

   ```java
   // 获取当前用户对象 Subject
   Subject currentUser = SecurityUtils.getSubject();
   // 判断当前用户是否被认证
   if (!currentUser.isAuthenticated())
   // 判断角色
   if (currentUser.hasRole("schwartz"))
   // 粗粒度，简单的认证
   if (currentUser.isPermitted("lightsaber:wield")
   // 注销
   currentUser.logout();
   ```

   

## 3、在SpringBoot中集成

### 3.1、环境搭建

1. 在当前项目中新建一个`module`，创建`shiro-springboot`，必须选择Springboot的模板

   ![image-20200725175251192](.\Shiro.assets\image-20200725175251192.png)

2. 导入web模板和Thymeleaf

3. 在`template`目录下创建一个index首页

4. 创建controller，`MyController`

   ```java
   @Controller
   public class MyController {
       @RequestMapping({"/index", "/"})
       public String toIndex(){
           return "index";
       }
   }
   ```

5. 启动浏览器，可以正常访问就环境搭建完成了

   ![image-20200725200314124](.\Shiro.assets\image-20200725200314124.png)

   

### 3.2、整合shiro

#### 1、测试环境

1. 导入shiro整合包

   ```xml
   <!-- shiro-spring -->
   <dependency>
       <groupId>org.apache.shiro</groupId>
       <artifactId>shiro-spring</artifactId>
       <version>1.5.3</version>
   </dependency>
   ```

2. 自定义一个`UserRealm`，需要继承`AuthorizingRealm`

   ```java
   public class UserRealm extends AuthorizingRealm {
       // 认证
       @Override
       protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
           System.out.println("执行了授权====》doGetAuthorizationInfo");
           return null;
       }
   
       // 授权
       @Override
       protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
           System.out.println("执行了认证====》doGetAuthenticationInfo");
           return null;
       }
   }
   ```

3. 编写配置类`ShiroConfig`

   ```java
   @Configuration
   public class ShiroConfig {
   
       // ShiroFilterFactoryBean3
       @Bean
       public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("getDefaultWebSecurityManager") DefaultWebSecurityManager defaultWebSecurityManager){
           ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
           // 设置安全管理器
           bean.setSecurityManager(defaultWebSecurityManager);
           return bean;
       }
   
       // DefaultWebSecurityManage2
       @Bean
       public DefaultWebSecurityManager getDefaultWebSecurityManager(@Qualifier("userRealm") UserRealm userRealm){
           DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
           // 关联userRealm
           securityManager.setRealm(userRealm);
           return securityManager;
       }
   
       // 创建realm对象，需要自定义1
       @Bean
       public UserRealm userRealm(){
           return new UserRealm();
       }
   }
   ```

4. 在`template`目录下创建`user`文件夹

5. 创建`add.html`，`update.html`

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
       <h1>执行了add</h1>
   </body>
   </html>
   ```

6. 修改controller进行跳转

   ```java
   @Controller
   public class MyController {
       @RequestMapping({"/index", "/"})
       public String toIndex(){
           return "index";
       }
       @RequestMapping("user/add")
       public String toAdd(){
           return "user/add";
       }
       @RequestMapping("user/update")
       public String toUpdate(){
           return "user/update";
       }
   }
   ```

7. 在首页增加跳转

   ```html
   <!DOCTYPE html>
   <html lang="en" xmlns:th="http://www.thymeleaf.org">
   <head>
       <meta charset="UTF-8">
       <title>首页</title>
   </head>
   <body>
   <h1>首页</h1>
   <a th:href="@{/user/add}" >add</a> | <a th:href="@{/user/update}">update</a>
   </body>
   </html>
   ```

#### 2、进行拦截

1. 修改`ShiroConfig`的代码

   ```java
   // ShiroFilterFactoryBean3
   @Bean
   public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("getDefaultWebSecurityManager") DefaultWebSecurityManager defaultWebSecurityManager){
       ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
       // 设置安全管理器
       bean.setSecurityManager(defaultWebSecurityManager);
       // 添加shiro的内置过滤器
       /*
               anon：无需认证就可以访问
               authc：必须认证了才能访问
               user：必须拥有 记住我 功能才能用
               perms：拥有对某个资源的权限才能访问
               role：拥有某个角色权限才能访问
           */
       Map<String, String> filterMap = new LinkedHashMap<>();
       filterMap.put("/user/*", "authc");
       bean.setFilterChainDefinitionMap(filterMap);
   
       // 设置登录的请求
       bean.setLoginUrl("/toLogin");
       return bean;
   }
   ```

2. 由于没有默认的登录页面，需要自己进行创建

   ```html
   <!DOCTYPE html>
   <html lang="en" xmlns:th="http://www.thymeleaf.org">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
       <h1>登录</h1>
       <hr>
       <form th:action="@{/login}">
           <p>用户名：<input type="text" name="username"></p>
           <p>密码：<input type="password" name="password"></p>
           <p><input type="submit" value="登录"></p>
       </form>
   </body>
   </html>
   ```

3. 修改控制类跳转到登录页

   ```java
   @RequestMapping("/toLogin")
   public String tuLogin(){
       return "login";
   }
   ```

4. 测试：现在点击任意一个都会跳转到登录页面

   ![image-20200725203025834](.\Shiro.assets\image-20200725203025834.png)



#### 3、认证

在`UserRealm`中设置

1. 修改控制类，接收登录的信息

   ```java
   @RequestMapping("/toLogin")
   public String tuLogin(String username, String password, Model model){
       // 获取当前用户
       Subject subject = SecurityUtils.getSubject();
       // 封装用户的登陆数据
       UsernamePasswordToken token = new UsernamePasswordToken(username, password);
       // 进行提交测试
       try{
           // 执行登录操作
           subject.login(token);
           return "index";
       }catch (UnknownAccountException uae) {
           // 用户不存在
           model.addAttribute("msg", "用户名不存在");
           return "login";
       } catch (IncorrectCredentialsException ice) {
           // 密码错误
           model.addAttribute("msg", "密码错误");
           return "login";
       }
   }
   ```

2. 前端获取错误信息

   ```html
   <p th:text="${msg}" style="color: red;"></p>
   ```

   ![image-20200725204111937](.\Shiro.assets\image-20200725204111937.png)

3. 测试，发现会经过`UserRealm`代码的认证功能

   ![image-20200725204558526](.\Shiro.assets\image-20200725204558526.png)

4. 修改`UserRealm`代码，进行认证，密码不需要我们进行判断

   ```java
   // 认证
   @Override
   protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
       System.out.println("执行了认证====》doGetAuthenticationInfo");
   
       // 用户名，密码的设置
       String name = "root";
       String password = "123456";
       UsernamePasswordToken userToken = (UsernamePasswordToken)token;
       // 用户名认证
       if(!userToken.getUsername().equals(name)){
           // 抛出UnknownAccountException未知用户异常
           return null;
       }
       // 密码认证交给Shiro进行操作
       return new SimpleAuthenticationInfo("",password,"");
   }
   }
   ```

5. 测试，输入`UserRealm`定义的用户名和密码就可以跳到首页



### 3.3、shiro整合Mybatis

1. 导入依赖

   ```xml
   <!--mysql驱动-->
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
   </dependency>
   
   <!--Druid数据源-->
   <dependency>
       <groupId>com.alibaba</groupId>
       <artifactId>druid</artifactId>
       <version>1.1.23</version>
   </dependency>
   
   <!--Mybatis-->
   <dependency>
       <groupId>org.mybatis.spring.boot</groupId>
       <artifactId>mybatis-spring-boot-starter</artifactId>
       <version>2.1.3</version>
   </dependency>
   
   <!--log4j-->
   <dependency>
       <groupId>log4j</groupId>
       <artifactId>log4j</artifactId>
       <version>1.2.17</version>
   </dependency>
   
   <!--Lombok-->
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <version>1.18.12</version>
   </dependency>
   ```

2. 编写配置文件`application.yml`

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

3. 在IDEA中连接数据库

   ![image-20200725211424225](.\Shiro.assets\image-20200725211424225.png)

   ![image-20200725211507612](.\Shiro.assets\image-20200725211507612.png)

   ![image-20200725211544973](.\Shiro.assets\image-20200725211544973.png)

4. 在`application.properties`配置文件中配置Mybatis

   ```properties
   mybatis.type-aliases-package=com.xj.pojo
   mybatis.mapper-locations=classpath:mapper/*.xml
   ```

5. 编写实体类`User`

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

6. 编写`UserMapper`接口，需要导入两个注解@Repository，@Mapper

   ```java
   @Repository
   @Mapper
   public interface UserMapper {
       User queryUserByName(String name);
   }
   ```

7. 在`resources`目录下创建`mapper`，然后创建`UserMapper.xml`，然后编写映射文件

   ```java
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.xj.mapper.UserMapper">
       <select id="queryUserByName" parameterType="String" resultType="User">
           SELECT * from user where name = #{name}
       </select>
   
   </mapper>
   ```

8. 编写业务层`Userservice`接口

   ```java
   package com.xj.service;
   
   import com.xj.pojo.User;
   
   public interface UserService {
       User queryUserByName(String name);
   }
   ```

9. 业务接口实现类`UserserviceImpl`

   ```java
   @Service
   public class UserserviceImpl implements UserService {I
       @Autowired
       private UserMapper userMapper;
       @Override
       public User queryUserByName(String name) {
           return userMapper.queryUserByName(name);
       }
   }
   ```

   需要加上注解@Service才会被Spring接管![image-20200725213203936](.\Shiro.assets\image-20200725213203936.png)

10. 编写测试类测试

    ```java
    @SpringBootTest
    class ShiroSpringbootApplicationTests {
        @Autowired
        private UserServiceImpl userService;
        @Test
        void contextLoads() {
            System.out.println(userService.queryUserByName("晓江"));
        }
    }
    ```

    ![image-20200725213714132](.\Shiro.assets\image-20200725213714132.png)

11. 修改配置类`UserRealm`，将用户名认证从数据库获取

    ```java
    public class UserRealm extends AuthorizingRealm {
        @Autowired
        private UserServiceImpl userService;
        // 授权
        @Override
        protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
            System.out.println("执行了授权====》doGetAuthorizationInfo");
            return null;
        }
    
        // 认证
        @Override
        protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
            System.out.println("执行了认证====》doGetAuthenticationInfo");
            // 获取登录令牌
            UsernamePasswordToken userToken = (UsernamePasswordToken)token;
    
            // 从数据库中获取用户信息
            User user = userService.queryUserByName(userToken.getUsername());
            // 用户名认证
            if(user == null){
                // 抛出UnknownAccountException未知用户异常
                return null;
            }
            // 密码认证交给Shiro进行操作
            return new SimpleAuthenticationInfo("",user.getPwd(),"");
        }
    }
    ```

12. 登录测试：这个时候要启动的是整个SpringBoot服务

    ![image-20200725214218619](.\Shiro.assets\image-20200725214218619.png)

    可以使用数据库中的用户信息登录即可

**注意点：**

1. 在`UserMapper`接口需要加上：两个注解@Repository，@Mapper
2. 业务接口实现类`UserserviceImpl`需要加上：@Service注解
3. 配置完`mapper.xml`文件之后需要在总的配置文件中进行声明



### 3.4、shiro请求授权的实现

> 授权是在config中进行的操作

#### 1、授权拦截

1. 编写`ShiroConfig`的代码

   ```java
   Map<String, String> filterMap = new LinkedHashMap<>();
   // 授权拦截
   filterMap.put("/user/add", "perms[user:add]");
   filterMap.put("/user/update", "perms[user:update]");
   filterMap.put("/user/*", "authc");
   bean.setFilterChainDefinitionMap(filterMap);
   ```

   ![image-20200726143733094](.\Shiro.assets\image-20200726143733094.png)

2. 此时进入add页面会显示401未授权错误

   ![image-20200726143823324](.\Shiro.assets\image-20200726143823324.png)

3. 设置未授权页面

   编写控制类

   ```java
   @RequestMapping("/unaut")
   @ResponseBody
   public String unaunt(){
       return "未经授权无法访问此页面";
   }
   ```

   编写配置类

   ```java
   // 为授权的请求页面
   bean.setUnauthorizedUrl("/unaut");
   ```

   ![image-20200726144243475](.\Shiro.assets\image-20200726144243475.png)

   

#### 2、给用户进行授权

> 进入需要授权的页面就会经过这里

![image-20200726144325168](.\Shiro.assets\image-20200726144325168.png)

1. 在`UserRealm`中进行编写

   ```java
   // 授权
   @Override
   protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
       System.out.println("执行了授权====》doGetAuthorizationInfo");
   
       // 给所有用户授权
       SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();
       info.addStringPermission("user:add");
   
       return info;
   }
   ```

2. 修改数据库，增加用户对应的权限，修改后上传即可

   ![image-20200726144715955](.\Shiro.assets\image-20200726144715955.png)

   ![image-20200726144929639](.\Shiro.assets\image-20200726144929639.png)

3. 修改实体类，增加权限属性即可

4. 获取用户的权限

   修改认证的代码，将用户信息提交到当前用户

   ```java
   // 密码认证交给Shiro进行操作
   return new SimpleAuthenticationInfo(user,user.getPwd(),"");
   ```

   在授权那里进行获取

   ```java
   @Override
   protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
       System.out.println("执行了授权====》doGetAuthorizationInfo");
   
       // 获取用户信息
       Subject subject = SecurityUtils.getSubject();
       // 拿到用户对象
       User currentUser = (User) subject.getPrincipal();
   
       // 设置当前用户的权限
       SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();
       info.addStringPermission(currentUser.getPermissions());
       // 记得返回新的信息
       return info;
   }
   ```

   ![image-20200726141255589](.\Shiro.assets\image-20200726141255589.png)

### 3.5、Shiro整合Thymeleaf

1. 导入整合包

   ```xml
   <!-- thymeleaf-extras-shiro -->
   <dependency>
       <groupId>com.github.theborakompanioni</groupId>
       <artifactId>thymeleaf-extras-shiro</artifactId>
       <version>2.0.0</version>
   </dependency>
   ```

2. 编写`ShiroConfig`代码

   ```java
   // ShiroDialect：用来整合shiro和thymeleaf
   @Bean
   public ShiroDialect getShiroDialect(){
       return new ShiroDialect();
   }
   ```

3. 修改首页代码，增加命名空间

   ```html
   xmlns:shiro="http://www.thymeleaf.org/thymeleaf-extras-shiro"
   ```

4. 执行判断

   ```html
   <!DOCTYPE html>
   <html lang="en" xmlns:th="http://www.thymeleaf.org"
         xmlns:shiro="http://www.thymeleaf.org/thymeleaf-extras-shiro">
   <head>
       <meta charset="UTF-8">
       <title>首页</title>
   </head>
   <body>
   <h1>首页</h1>
   <div shiro:hasPermission="user:add">
       <a th:href="@{/user/add}" >add</a>
   </div>
   <div shiro:hasPermission="user:update">
       <a th:href="@{/user/update}">update</a>
   </div>
   </body>
   </html>
   ```

   ![image-20200726150449148](.\Shiro.assets\image-20200726150449148.png)

5. 登录成功之后在`UserRealm`获取Session

   ```java
   // 存储认证信息
   Subject subject = SecurityUtils.getSubject();
   Session session = subject.getSession();
   session.setAttribute("loginUser", user);
   ```

   ![image-20200726142950179](.\Shiro.assets\image-20200726142950179.png)

6. 前端判断登录按钮

   ```html
   <div th:if="${session.loginUser == null}">
       <a th:href="@{/toLogin}">登录</a>
   </div>
   ```

7. 增加注销按钮

   修改控制类，增加注销路由

   ```java
   @RequestMapping("/logout")
   public String logout(){
       Subject subject = SecurityUtils.getSubject();
       // 注销
       subject.logout();
       return "index";
   }
   ```

   修改主页，增加注销按钮

   ```html
   <div th:if="${session.loginUser != null}">
       <a th:href="@{/logout}">注销</a>
   </div>
   ```

现在就可以给不同权限的用户展示不同的内容了~





















​    

​    

