## 1、SpringSecurity

- shiro、SpringSecurity
- 认证、授权

### 1.1、环境搭建

1. 新建项目`spring-05-security`

2. 导入web模块，thymeleaf模块

   ![image-20200724151057862](.\SpringSecurity.assets\image-20200724151057862.png)

3. 导入素材

4. 关闭模板引擎的缓存`application.properties`

   ```properties
   # 关闭缓存
   spring.thymeleaf.cache=false
   ```

5. 创建controller文件`RouterController`

   ```java
   @Controller
   public class RouterController {
   
       @RequestMapping({"/", "/index"})
       public String index(){
           return "index";
       }
       @RequestMapping("/login")
       public String login(){
           return "/views/login";
       }
       @RequestMapping("/level1/{id}")
       public String level1(@PathVariable("id") int id){
           return "/views/level1/" + id;
       }
       @RequestMapping("/level2/{id}")
       public String level2(@PathVariable("id") int id){
           return "/views/level2/" + id;
       }
       @RequestMapping("/level3/{id}")
       public String level3(@PathVariable("id") int id){
           return "/views/level3/" + id;
       }
   }
   ```

6. 打开浏览器进行测试

   ![image-20200724155447222](.\SpringSecurity.assets\image-20200724155447222.png)

### 1.2、认识SpringSecurity

Spring Security 是针对Spring项目的安全框架，也是Spring Boot底层安全模块默认的技术选型，他可以实现强大的Web安全控制，对于安全控制，我们仅需要引入spring-boot-starter-security 模块，进行少量的配置，即可实现强大的安全管理！

记住几个类：

- WebSecurityConfigurerAdapter：自定义Security策略
- AuthenticationManagerBuilder：自定义认证策略
- @EnableWebSecurity：开启WebSecurity模式，@EnableXXX就是开启某个模式

Spring Security的两个主要目标是 “认证” 和 “授权”（访问控制）。

**“认证”（Authentication）**

身份验证是关于验证您的凭据，如用户名/用户ID和密码，以验证您的身份。

身份验证通常通过用户名和密码完成，有时与身份验证因素结合使用。

 **“授权” （Authorization）**

授权发生在系统成功验证您的身份后，最终会授予您访问资源（如信息，文件，数据库，资金，位置，几乎任何内容）的完全权限。

这个概念是通用的，而不是只在Spring Security 中存在。

> 参考官网：https://spring.io/projects/spring-security 
>
> 查看我们自己项目中的版本，找到对应的帮助文档：
>
> https://docs.spring.io/spring-security/site/docs/5.3.0.RELEASE/reference/html5  #servlet-applications 8.16.4

**测试**

1. 引入 Spring Security 模块

   ```xml
   <!-- security -->
   <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-security</artifactId>
   </dependency>
   ```

   如果报了如下错误

   ![image-20200724164902647](.\SpringSecurity.assets\image-20200724164902647.png)

   在导入下面这个版本设置即可

   ```xml
   <properties>
       <spring-security.version>5.3.2.RELEASE</spring-security.version>
   </dependencies>
   ```

2. 创建一个`config`，创建`SecurityConfig`，这是基本框架

   ```java
   import org.springframework.security.config.annotation.web.builders.HttpSecurity;
   import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
   import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
   
   @EnableWebSecurity // 开启WebSecurity模式
   public class SecurityConfig extends WebSecurityConfigurerAdapter {
   
      @Override
      protected void configure(HttpSecurity http) throws Exception {
          
     }
   }
   ```

3. 定制请求的授权规则

   而定义为`.antMatchers("/level1/**").hasAnyRole("vip1", "vip2", "vip3")`就可以设置多个值

   ```java
   @Override
   protected void configure(HttpSecurity http) throws Exception {
       // 定制请求的授权规则
       // 首页所有人可以访问
       http.authorizeRequests().antMatchers("/").permitAll()
           .antMatchers("/level1/**").hasRole("vip1")
           .antMatchers("/level2/**").hasRole("vip2")
           .antMatchers("/level3/**").hasRole("vip3");
   }
   ```

4. 测试一下：发现除了首页，其他页面进不去了！因为我们目前没有登录的角色，因为请求需要登录的角色拥有对应的权限才可以！

   ![image-20200724165108454](.\SpringSecurity.assets\image-20200724165108454.png)

5. 在`configure()`方法中加入以下配置，开启自动配置的登录功能！

   ```java
   // 开启自动配置的登录功能
   // /login 请求来到登录页
   // /login?error 重定向到这里表示登录失败
   http.formLogin();
   ```

   点击进去查看源码，下载源码之后可以看到一堆注释，其中就有说明会跳转到 login

   ![image-20200724165622433](.\SpringSecurity.assets\image-20200724165622433.png)

6. 、测试一下：发现，没有权限的时候，会跳转到登录的页面（这个页面是自动生成的）

   ![image-20200724165232494](.\SpringSecurity.assets\image-20200724165232494.png)

7. 重写`configure(AuthenticationManagerBuilder auth)`方法

   ```java
   //定义认证规则
   @Override
   protected void configure(AuthenticationManagerBuilder auth) throws Exception {
      //在内存中定义，也可以在jdbc中去拿....
      //Spring security 5.0中新增了多种加密方式，也改变了密码的格式。
      //要想我们的项目还能够正常登陆，需要修改一下configure中的代码。我们要将前端传过来的密码进行某种方式加密
      //spring security 官方推荐的是使用bcrypt加密方式。
      
      auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
             .withUser("xj").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2","vip3")
             .and()
             .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2","vip3")
             .and()
             .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2");
   }
   ```

   注意：我们要将前端传过来的密码进行某种方式加密，否则就无法登录，会爆出`There is no PasswordEncoder mapped for the id “null”`错误

8. 测试，发现，登录成功，并且每个角色只能访问自己认证下的规则



### 1.3、权限控制和注销

1. 开启自动配置的注销的功能

   ```java
   //定制请求的授权规则
   @Override
   protected void configure(HttpSecurity http) throws Exception {
       //....
       //开启自动配置的注销的功能
       // /logout 注销请求
       http.logout();
   }
   ```

2. 我们在前端，增加一个注销的按钮，index.html 导航栏中

   `/logout`路径就是注销的

   ```html
   <a class="item" th:href="@{/logout}">
      <i class="address card icon"></i> 注销
   </a>
   ```

3. 测试：登录成功后点击注销，发现注销完毕会跳转到登录页面！

   ![image-20200724173618260](.\SpringSecurity.assets\image-20200724173618260.png)

   ![image-20200724173624863](.\SpringSecurity.assets\image-20200724173624863.png)

4. 但是，我们想让他注销成功后，依旧可以跳转到首页，该怎么处理呢？

   ```java
   // .logoutSuccessUrl("/"); 注销成功来到首页
   http.logout().logoutSuccessUrl("/");
   ```

5. 测试，注销完毕后，发现跳转到首页



### 1.4、显示内容的控制

> 用户没有登录的时候，导航栏上只显示登录按钮，用户登录之后，导航栏可以显示登录的用户信息及注销按钮！
>
> 我们需要结合thymeleaf中的一些功能
>
> sec：authorize="isAuthenticated()":是否认证登录！来显示不同的页面

1. 导入Maven依赖

   ```xml
   <!-- thymeleaf-extras-springsecurity4 -->
   <dependency>
      <groupId>org.thymeleaf.extras</groupId>
      <artifactId>thymeleaf-extras-springsecurity4</artifactId>
      <version>3.0.4.RELEASE</version>
   </dependency>
   ```

2. 导入命名空间

   ```html
   xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity5"
   ```

3. 修改导航栏，增加认证判断

   ```html
   <!--登录注销-->
   <div class="right menu">
   
      <!--如果未登录-->
      <div sec:authorize="!isAuthenticated()">
          <a class="item" th:href="@{/login}">
              <i class="address card icon"></i> 登录
          </a>
      </div>
   
      <!--如果已登录-->
      <div sec:authorize="isAuthenticated()">
          <a class="item">
              <i class="address card icon"></i>
             用户名：<span sec:authentication="principal.username"></span>
             角色：<span sec:authentication="principal.authorities"></span>
          </a>
      </div>
   
      <div sec:authorize="isAuthenticated()">
          <a class="item" th:href="@{/logout}">
              <i class="address card icon"></i> 注销
          </a>
      </div>
   </div>
   ```

4. 重启测试，显示了所有的标签

   ![image-20200724174441109](.\SpringSecurity.assets\image-20200724174441109.png)

   这是因为Thymeleaf的版本原因，只能对应较低版本的SpringBoot，降低SpringBoot的版本

   ![image-20200724174642081](.\SpringSecurity.assets\image-20200724174642081.png)

   也有可能是URI错误

   ![image-20200724182929391](.\SpringSecurity.assets\image-20200724182929391.png)

   点击设置

   ![image-20200724183143139](.\SpringSecurity.assets\image-20200724183143139.png)


5. 重新进行测试，害 原来是忘记导入上面的依赖了`thymeleaf-extras-springsecurity4`

   ![image-20200724220656712](.\SpringSecurity.assets\image-20200724220656712.png)

   ![image-20200724220721574](.\SpringSecurity.assets\image-20200724220721574.png)

6. 如果注销404了，就是因为它默认防止`csrf`跨站请求伪造，因为会产生安全问题，我们可以将请求改为post表单提交，或者在springsecurity中关闭csrf功能

   ![image-20200724220739529](.\SpringSecurity.assets\image-20200724220739529.png)

7. 在配置中增加 `http.csrf().disable();`关闭csrf功能

   ```java
   //关闭csrf功能:跨站请求伪造,默认只能通过post方式提交logout请求
   http.csrf().disable();
   ```

8. 继续将下面的角色功能块认证完成

   在对应的模块前面增加`sec:authorize="hasRole('vip1')"`判断条件，`hasAnyRole`可以加多个判断

   ```html
   <div class="column" sec:authorize="hasAnyRole('vip1','vip2','vip3')">
       <div class="ui raised segment">
           <div class="ui">
               <div class="content">
                   <h5 class="content">Level 1</h5>
                   <hr>
                   <div><a th:href="@{/level1/1}"><i class="bullhorn icon"></i> Level-1-1</a></div>
                   <div><a th:href="@{/level1/2}"><i class="bullhorn icon"></i> Level-1-2</a></div>
                   <div><a th:href="@{/level1/3}"><i class="bullhorn icon"></i> Level-1-3</a></div>
               </div>
           </div>
       </div>
   </div>
   ```

9. 测试

   ![image-20200724221519267](.\SpringSecurity.assets\image-20200724221519267.png)



### 1.5、记住我功能

> 现在的情况，我们只要登录之后，关闭浏览器，再登录，就会让我们重新登录，但是很多网站的情况，就是有一个记住密码的功能，这个该如何实现呢？

1. 开启记住我功能

   ```java
   //定制请求的授权规则
   @Override
   protected void configure(HttpSecurity http) throws Exception {
   
      //记住我
      http.rememberMe();
   }
   ```

2. 我们再次启动项目测试一下，发现登录页多了一个记住我功能，我们登录之后关闭 浏览器，然后重新打开浏览器访问，发现用户依旧存在！这是因为存放到了Cookie中了

   ![image-20200724221740963](.\SpringSecurity.assets\image-20200724221740963.png)

3. 我们点击注销的时候，可以发现，springsecurity 帮我们自动删除了这个 cookie

   ![image-20200724221835131](.\SpringSecurity.assets\image-20200724221835131.png)

   ![image-20200724221953214](.\SpringSecurity.assets\image-20200724221953214.png)

4. 登录成功后，将cookie发送给浏览器保存，以后登录带上这个cookie，只要通过检查就可以免登录了。如果点击注销，则会删除这个cookie



### 1.6、定制登录页

> 现在这个登录页面都是spring security 默认的，怎么样可以使用我们自己写的Login界面呢？

1. 在刚才的登录页配置后面指定 loginpage

   ```java
   http.formLogin().loginPage("/toLogin");
   ```

2. 然后前端也需要指向我们自己定义的 login请求

   ```html
   <a class="item" th:href="@{/toLogin}">
      <i class="address card icon"></i> 登录
   </a>
   ```

3. 测试失败：`IllegalArgumentException: 'toLogin?error' is not a valid redirect URL`

   ![image-20200724222650135](.\SpringSecurity.assets\image-20200724222650135.png)

   这是因为我刚才少写了斜杠了。。`loginPage("toLogin");`

#### 设置登录页

1. 我们登录，需要将这些信息发送到哪里，我们也需要配置，login.html 配置提交请求及方式，方式必须为post:在 loginPage()源码中的注释上有写明：

   主要修改`th:action="@{/login}" method="post"`

   ```html
   <form th:action="@{/login}" method="post">
      <div class="field">
          <label>Username</label>
          <div class="ui left icon input">
              <input type="text" placeholder="Username" name="username">
              <i class="user icon"></i>
          </div>
      </div>
      <div class="field">
          <label>Password</label>
          <div class="ui left icon input">
              <input type="password" name="password">
              <i class="lock icon"></i>
          </div>
      </div>
      <input type="submit" class="ui blue submit button"/>
   </form>
   ```

2. 这个请求提交上来，我们还需要验证处理，怎么做呢？我们可以查看formLogin()方法的源码！我们配置接收登录的用户名和密码的参数！

   ```java
   http.formLogin()
     .usernameParameter("username")
     .passwordParameter("password")
     .loginPage("/toLogin")
     .loginProcessingUrl("/login"); // 登陆表单提交请求
   ```

3. 在登录页增加记住我的多选框

   ```html
   <div class="field">
       <input type="checkbox" name="remember"> 记住我
   </div>
   ```

4. 后端验证处理

   ```java
   //定制记住我的参数！
   http.rememberMe().rememberMeParameter("remember");
   ```

5. 测试

   ![image-20200724225243718](.\SpringSecurity.assets\image-20200724225243718.png)

### 完整的配置代码

```java
package com.xj.config;

import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

@EnableWebSecurity // 开启WebSecurity模式
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // 定制请求的授权规则
        // 首页所有人可以访问
        http.authorizeRequests().antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasAnyRole("vip1", "vip2", "vip3")
                .antMatchers("/level2/**").hasAnyRole("vip2", "vip3")
                .antMatchers("/level3/**").hasRole("vip3");
        // 开启自动配置的登录功能
        // /login 请求来到登录页
        // /login?error 重定向到这里表示登录失败
        // .loginPage("toLogin");指定自己的登录页
        http.formLogin()
                .usernameParameter("username")
                .passwordParameter("password")
                .loginPage("/toLogin")
                .loginProcessingUrl("/login"); // 登陆表单提交请求
        //开启自动配置的注销的功能
        // /logout 注销请求
        http.logout().logoutSuccessUrl("/");
        //关闭csrf功能:跨站请求伪造,默认只能通过post方式提交logout请求
        http.csrf().disable();
        //记住我
        http.rememberMe().rememberMeParameter("remember");
    }

    //定义认证规则
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        //在内存中定义，也可以在jdbc中去拿....
        //Spring security 5.0中新增了多种加密方式，也改变了密码的格式。
        //要想我们的项目还能够正常登陆，需要修改一下configure中的代码。我们要将前端传过来的密码进行某种方式加密
        //spring security 官方推荐的是使用bcrypt加密方式
        auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
                .withUser("xj").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2")
                .and()
                .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip3")
                .and()
                .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1");
    }
}
```



