## 	1、Spring

### 1.1、简介

![image-20200527135321085](.\Spring.assets\image-20200527135321085.png)

- Spring : 春天 --->给软件行业带来了春天

- 2002年，Rod Jahnson首次推出了Spring框架雏形interface21框架。

- 2004年3月24日，Spring框架以interface21框架为基础，经过重新设计，发布了1.0正式版。

- Spring理念 : 使现有技术更加实用 . 本身就是一个大杂烩 , 整合现有的框架技术

**两个java框架**

- SSH：Struct2 + Spring + Hibernate
- SSM：SpringMVC + Spring + Mybatis

官网 : http://spring.io/

官方下载地址 : https://repo.spring.io/libs-release-local/org/springframework/spring/

GitHub : https://github.com/spring-projects

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.2.5.RELEASE</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.5.RELEASE</version>
</dependency>
```



### 1.2、优点

- Spring是一个开源免费的框架 , 容器  

- Spring是一个轻量级的框架 , 非侵入式的 .

- **控制反转 IoC  , 面向切面 Aop**

- 对事物的支持 , 对框架的支持



==一句话概括：**Spring是一个轻量级的控制反转(IoC)和面向切面(AOP)的容器（框架）。**==



### 1.3、组成

![image-20200527141010854](.\Spring.assets\image-20200527141010854.png)

### 1.4、扩展

在Spring的官网的介绍：现代化的Java开发！说白就是基于Spring的开发！

![image-20200527141236423](.\Spring.assets\image-20200527141236423.png)

- Spring Boot
  - 一个快速开发的脚手架
  - 基于Spring Boot可以快速的开发单个微服务
  - 约定大于配置！（类似Mybatis）
- Spring Cloud
  - 基于Spring Boot实现的

现在大多数公司都在使用SpringBoot进行快速开发，学习前提是掌握Spring以及SpringMVC！

**弊端：发展太久之后，违背原来的理念！配置十分繁琐，“配置地狱！”**

---



### 1.5、Spring官方文档的查看：

- ![image-20200527161514828](.\Spring.assets\image-20200527161514828.png)

---

- ![image-20200527161633018](.\Spring.assets\image-20200527161633018.png)

- ![image-20200527161722731](.\Spring.assets\image-20200527161722731.png)

### 1.6、Spring下载的官方页面

- ![image-20200527161905091](.\Spring.assets\image-20200527161905091.png)

- ![image-20200527162011730](.\Spring.assets\image-20200527162011730.png)

- ![image-20200527162047021](.\Spring.assets\image-20200527162047021.png)
- ![image-20200527162123562](.\Spring.assets\image-20200527162123562.png)
- ![image-20200527162141239](.\Spring.assets\image-20200527162141239.png)

- 这里就可以下载各个版本的Spring了，链接：https://repo.spring.io/release/org/springframework/spring/



## 2、Ioc理论推导

==控制反转，将实现类的控制权交到用户手里，使程序员更加专注于处理service==

### 2.1、IoC基础

新建一个空白的maven项目

> 分析实现

我们先用我们原来的方式写一段代码 .

1、先写一个UserDao接口

```java
public interface UserDao {
   public void getUser();
}
```

2、再去写Dao的实现类

```java
public class UserDaoImpl implements UserDao {
   @Override
   public void getUser() {
       System.out.println("获取用户数据");
  }
}
```

3、然后去写UserService的接口

```java
public interface UserService {
   public void getUser();
}
```

4、最后写Service的实现类

```java
public class UserServiceImpl implements UserService {
   private UserDao userDao = new UserDaoImpl();

   @Override
   public void getUser() {
       userDao.getUser();
  }
}
```

5、测试一下

```java
@Test
public void test(){
   UserService service = new UserServiceImpl();
   service.getUser();
}
```

这是我们原来的方式 , 开始大家也都是这么去写的对吧 . 那我们现在修改一下 .

> 把Userdao的实现类增加一个 实现类UserDaoMySqlImpl
>

```java
public class UserDaoMySqlImpl implements UserDao {
   @Override
   public void getUser() {
       System.out.println("MySql获取用户数据");
  }
}
```

紧接着我们要去使用MySql的话 , 我们就需要去service实现类里面修改对应的实现

```java
public class UserServiceImpl implements UserService {
   private UserDao userDao = new UserDaoMySqlImpl();

   @Override
   public void getUser() {
       userDao.getUser();
  }
}
```

> 在假设, 我们再增加一个Userdao的实现类 UserDaoOracleImpl
>

```java
public class UserDaoOracleImpl implements UserDao {
   @Override
   public void getUser() {
       System.out.println("Oracle获取用户数据");
  }
}
```

==那么我们要使用Oracle , 又需要去service实现类里面修改对应的实现 . 假设我们的这种需求非常大 , 这种方式就根本不适用了, 甚至反人类对吧 , 每次变动 , 都需要修改大量代码 . 这种设计的耦合性太高了, 牵一发而动全身 .==



**那我们如何去解决呢 ?** （面向接口编程）

我们可以在需要用到他的地方 , 不去实现它 , 而是留出一个接口 , 利用set , 我们去代码里修改下 .

```java
public class UserServiceImpl implements UserService {
   private UserDao userDao;
// 利用set实现
   public void setUserDao(UserDao userDao) {
       this.userDao = userDao;
  }

   @Override
   public void getUser() {
       userDao.getUser();
  }
}
```

现在去我们的测试类里 , 进行测试 ;

```java
@Test
public void test(){
   UserServiceImpl service = new UserServiceImpl();
   service.setUserDao( new UserDaoMySqlImpl() );
   service.getUser();
   //那我们现在又想用Oracle去实现呢
   service.setUserDao( new UserDaoOracleImpl() );
   service.getUser();
}
```

大家发现了区别没有 ? 可能很多人说没啥区别 . 但是同学们 , 他们已经发生了根本性的变化 , 很多地方都不一样了 . 仔细去思考一下 , 以前所有东西都是由程序去进行控制创建 , 而现在是由我们自行控制创建对象 , 把主动权交给了调用者 . 程序不用去管怎么创建,怎么实现了 . 它只负责提供一个接口 .

这种思想 , 从本质上解决了问题 , 我们程序员不再去管理对象的创建了 , 更多的去关注业务的实现 . 耦合性大大降低 . 这也就是IOC的原型 !

#### ==两种区别如下：==

- 控制权在程序手里

![image-20200527151129580](.\Spring.assets\image-20200527151129580.png)

- 控制权在用户手里

![image-20200527151143463](.\Spring.assets\image-20200527151143463.png)

### 2.2、IOC本质

**控制反转IoC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现IoC的一种方法**，也有人认为DI只是IoC的另一种说法。没有IoC的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。



![image-20200530155539586](.\Spring.assets\image-20200530155539586.png)

采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

**控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IoC容器，其实现方法是依赖注入（Dependency Injection,DI）。**

## 3、第一个Spring程序

### 3.1、Hello Spring

注意：有时候需要修改文件夹类型，否则无法创建java类，以及配置文件无效

- ![image-20200527162502782](.\Spring.assets\image-20200527162502782.png)
- ![image-20200527162540164](.\Spring.assets\image-20200527162540164.png)

1. 导入maven依赖，放在主项目的pom.xml中，所有模块就不用再次导入了

   ```xml
   <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-webmvc</artifactId>
       <version>5.2.5.RELEASE</version>
   </dependency>
   <!-- AOP织入 -->
   <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.9.4</version>
   </dependency>
   ```

2. 创建实体类

   ```java
   package com.xj.pojo;
   
   public class Hello {
       private String name;
       public String getName() {
           return name;
       }
       public void setName(String name) {
           this.name = name;
       }
       @Override
       public String toString() {
           return "Hello{" +
                   "name='" + name + '\'' +
                   '}';
       }
   }
   ```

3. 编写Spring配置文件bean.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
       <!--
           id = 类名
           class = new 对象
           property： 为对象赋值
       -->
       <bean id = "Hello" class = "com.xj.pojo.Hello">
           <property name = "name" value="Spring"/>
       </bean>
   </beans>
   ```

   - ![image-20200527170108924](.\Spring.assets\image-20200527170108924.png)

   - ![image-20200527170149895](.\Spring.assets\image-20200527170149895.png)

   - 关于配置文件中的内容，直接在官方文档中复制

   - https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/core.html#beans-factory-instantiation

     ![image-20200527161252806](.\Spring.assets\image-20200527161252806.png)

4. 测试

   ```java
   public class mytest {
       public static void main(String[] args) {
           //解析beans.xml文件 , 生成管理相应的Bean对象 CPX
           ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
           //getBean : 参数即为spring配置文件中bean的id
           Hello hello = (Hello) context.getBean("Hello");
           System.out.println(hello.toString());
       }
   }
   ```

5. 结果

   当配置成功之后，在实体类中会出现两个标志，点击可以跳转到配置文件中的相应位置

   （如果没出现，请到配置文件中设置）

   ![image-20200527165902710](.\Spring.assets\image-20200527165902710.png)

---



### 3.2、修改上面的Ioc代码

1. 编写配置文件bean.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
       <bean id="DaoImpl" class="com.xj.dao.UserDaoImpl"/>
       <bean id="MysqlImpl" class="com.xj.dao.UserDaoMysqlImpl"/>
       <bean id="OracleImpl" class="com.xj.dao.UserDaoOracleImpl"/>
       <!-- 注意这里必须使用ref -->
       <bean id="service" class="com.xj.service.UserServiceImpl">
           <property name="userDao" ref="OracleImpl"/>
       </bean>
   </beans>
   ```

2. 测试

   ```java
   public class Mytest_02 {
       public static void main(String[] args) {
           ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
           // 获取bean
           UserServiceImpl service = (UserServiceImpl) context.getBean("service");
           service.getUser();
       }
   }
   ```

3. 结果（现在只需要修改配置文件就可以实现不同类型）

   ![image-20200527164943824](.\Spring.assets\image-20200527164943824.png)

---



### 3.3、property标签

1. 两种类型，但参数为常量的时候，使用value

   ```xml
   <bean id = "Hello" class = "com.xj.pojo.Hello">
       <property name = "name" value="Spring"/>
   </bean>
   ```

2. 但参数为自定义类型时，使用ref

   ```xml
   <bean id = "service" class = "com.xj.service.UserServiceImpl">
       <property name="userDao" ref="OracleImpl"/>
   </bean>
   ```

OK , 到了现在 , 我们彻底不用在程序中去改动了 , 要实现不同的操作 , 只需要在xml配置文件中进行修改 , 所谓的IoC,一句话搞定 : 对象由Spring 来创建 , 管理 , 装配 ! 

---



## 4、IOC创建对象方式

1. 通过无参构造方法来创建

   ![image-20200527180012820](.\Spring.assets\image-20200527180012820.png)

   - 修改为有参构造的话，配置文件就需要相应修改，不然报错

2. **如果想通过有参来创建**

   - 通过**constructor-arg**标签的使用**构造函数参数名称**

   - 创建实体类

     ```java
     public class Hello {
         private String name;
         // 有参构造方法
         public Hello(String name){
             this.name = name;
         }
     }
     ```

   1. **构造函数参数索引**

      ```xml
      <!--构造函数参数索引-->
      <bean id = "Hello" class = "com.xj.pojo.Hello">
          <constructor-arg index="0" value="晓江"/>
      </bean>
      ```

   2. **构造函数参数类型匹配**（当有两种同类型不适用）

      ```xml
      <!--构造函数参数类型匹配-->
      <bean id="Hello" class="com.xj.pojo.Hello">
          <constructor-arg type="String" value="晓江"/>
      </bean>
      ```

   3. **构造函数参数名称**（推荐使用）

      ```xml
      <!--构造函数参数类型匹配-->
      <bean id="Hello" class="com.xj.pojo.Hello">
          <constructor-arg name="name" value="晓江"/>
      </bean>
      ```

   结论：在配置文件加载的时候。其中管理的对象都已经初始化了！



## 5、Spring的配置

### 5.1、别名

> 不实用

alias 设置别名 , 为bean设置别名 , 可以设置多个别名，但原来的名字仍可以使用

```xml
<!--设置别名：在获取Bean的时候可以使用别名获取-->
<alias name="userT" alias="userNew"/>
```



### 5.2、Bean的配置（也可以设置别名）

```xml
<!--bean就是java对象,由Spring创建和管理-->

<!--
   id: 是bean的标识符,要唯一,如果没有配置id,name就是默认标识符
   如果配置id,又配置了name,那么name是别名

   name: 可以设置多个别名,可以用逗号,分号,空格隔开
   如果不配置id和name,可以根据applicationContext.getBean(.class)获取对象;

	class是bean的全限定名=包名+类名
-->
<bean id="hello" name="hello2 h2,h3;h4" class="com.xj.pojo.Hello">
   <property name="name" value="Spring"/>
</bean>
```



### 5.3、import

团队的合作通过import来实现 .

当项目由多个人来开发，每个人负责不同的Bean.xml，最后可以利用import将所有的bean.xml合并在一个总的配置文件中！

```xml
<import resource="bean1.xml"/>
<import resource="bean2.xml"/>
<import resource="bean3.xml"/>
```

使用的时候使用总的配置文件就好！

---



## 6、依赖注入

### 6.1、构造器注入

> 即前面的构造方法注入。
>

无参构造和有参构造

### 6.2、Set方式注入【重点】

- 依赖注入：Set注入
  - 依赖：bean对象的创建依赖于容器
  - 注入：bean对象中的所有属性，由容器来注入

#### 【环境搭建】

1. 复杂类型

   ```java
    public class Address {
    
        private String address;
    
        public String getAddress() {
            return address;
       }
    
        public void setAddress(String address) {
            this.address = address;
       }
    }
   ```

   

2. 真实测试对象

   ```java
    package com.xj.pojo;
    
    import java.util.List;
    import java.util.Map;
    import java.util.Properties;
    import java.util.Set;
    
    public class Student {
    
        private String name;
        private Address address;
        private String[] books;
        private List<String> hobbys;
        private Map<String,String> card;
        private Set<String> games;
        private String wife;
        private Properties info;
    
        public void setName(String name) {
            this.name = name;
       }
    
        public void setAddress(Address address) {
            this.address = address;
       }
    
        public void setBooks(String[] books) {
            this.books = books;
       }
    
        public void setHobbys(List<String> hobbys) {
            this.hobbys = hobbys;
       }
    
        public void setCard(Map<String, String> card) {
            this.card = card;
       }
    
        public void setGames(Set<String> games) {
            this.games = games;
       }
    
        public void setWife(String wife) {
            this.wife = wife;
       }
    
        public void setInfo(Properties info) {
            this.info = info;
       }
    
        public void show(){
            System.out.println("name="+ name
                    + ",address="+ address.getAddress()
                    + ",books="
           );
            for (String book:books){
                System.out.print("<<"+book+">>\t");
           }
            System.out.println("\n爱好:"+hobbys);
    
            System.out.println("card:"+card);
    
            System.out.println("games:"+games);
    
            System.out.println("wife:"+wife);
    
            System.out.println("info:"+info);
    
       }
    }
   ```

#### 【完善代码】

- ```xml
  <!--引用类型-->
  <bean id="address" class="com.xj.pojo.Address">
      <property name="address" value="潮州"/>
  </bean>
  <bean id="student" class="com.xj.pojo.Student">
      <property name="name" value="晓江"/>
      <!--引用类型-->
      <property name="address" ref="address"/>
      <!--数组类型-->
      <property name="books">
          <array>
              <value>西游记</value>
              <value>红楼梦</value>
              <value>水浒传</value>
              <value>三国演义</value>
          </array>
      </property>
      <!--List类型-->
      <property name="hobbys">
          <list>
              <value>跑步</value>
              <value>听歌</value>
              <value>编程</value>
          </list>
      </property>
      <!--Map类型-->
      <property name="card">
          <map>
              <entry key="学号" value="201841312124"/>
              <entry key="班级" value="18光电1班"/>
              <entry key="姓名" value="晓江"/>
          </map>
      </property>
      <!--Set类型-->
      <property name="games">
          <set>
              <value>王者荣耀</value>
              <value>吃鸡</value>
          </set>
      </property>
      <!--Null类型-->
      <property name="wife">
          <null></null>
      </property>
      <!--Properties类型-->
      <property name="info">
          <props>
              <prop key="driver">com.mysql.jdbc.Driver</prop>
              <prop key="url">jdbc:mysql://localhost:3306/test?serverTimezone=GMT&amp;createDatabaseIfNotExist=true&amp;useUnicode=true&amp;characterEncoding=utf-8&amp;autoReconnect=true</prop>
              <prop key="username">root</prop>
              <prop key="password">1234</prop>
          </props>
      </property>
  </bean>
  ```

  

### 6.3、拓展方式注入

可以使用**p命名空间**和**c命名空间**进行注入

- **p命名注入属性值**
- **c命名注入构造方法的值**

官方解释：

![image-20200528114638993](.\Spring.assets\image-20200528114638993.png)

1. 使用

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:p="http://www.springframework.org/schema/p"
          xmlns:c="http://www.springframework.org/schema/c"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
       <!--需要无参构造-->
       <bean id="user" class="com.xj.pojo.User" p:name="晓江" p:age="20"/>
       <!--需要有参构造-->
       <bean id="user2" class="com.xj.pojo.User" c:name="晓江" c:age="20"/>
   </beans>
   ```

2. 测试

   ```java
   @Test
   public void getUser(){
       ApplicationContext context = new ClassPathXmlApplicationContext("userbean.xml");
       // 这样子就不用强制类型转换
       User user = context.getBean("user", User.class);
       System.out.println(user);
   }
   ```

   

- ==注意：p命名和c命名空间不能直接使用，需要导入xml约束,==

```xml
xmlns:p="http://www.springframework.org/schema/p"
xmlns:c="http://www.springframework.org/schema/c"
```



### 6.4、bean作用域

![image-20200528120646558](.\Spring.assets\image-20200528120646558.png)

1. 单例模式（Spring默认机制）

   ```xml
   <bean id="accountService" class="com.something.DefaultAccountService" scope="singleton"/>
   ```

2. 原型模式：每次都会从容器中get新的对象

   ```xml
   <bean id="accountService" class="com.something.DefaultAccountService" scope="prototype"/>
   ```

3. 其余的request、session、application只能再web开发中使用！



## 7、Bean的自动装配

- 自动装配是使用spring满足bean依赖的一种方法
- spring会在应用上下文中为某个bean寻找其依赖的bean。

Spring中bean有三种装配机制，分别是：

1. 在xml中显式配置；
2. 在java中显式配置；
3. 隐式的bean发现机制和自动装配。【重点】

**推荐不使用自动装配xml配置 , 而使用注解 .**

![image-20200528121444576](.\Spring.assets\image-20200528121444576.png)

### 7.1、测试（未使用自动装配）

1. 新建项目 Spring_05_autowired

2. 编写实体类

   ```java
   public class Cat {
       public void shout() {
           System.out.println("miao~");
       }
   }
   ```

   ```java
   public class Dog {
       public void shout() {
           System.out.println("wang~");
       }
   }
   ```

   ```java
   public class User {
       private Cat cat;
       private Dog dog;
       private String name;
       public Cat getCat() {
           return cat;
       }
       public void setCat(Cat cat) {
           this.cat = cat;
       }
       public Dog getDog() {
           return dog;
       }
       public void setDog(Dog dog) {
           this.dog = dog;
       }
       public String getName() {
           return name;
       }
       public void setName(String name) {
           this.name = name;
       }
   }
   ```

3. 映射文件

   ```xml
   <bean id="cat" class="com.xj.pojo.Cat"/>
   <bean id="dog" class="com.xj.pojo.Dog"/>
   <bean id="user" class="com.xj.pojo.User">
       <property name="cat" ref="cat"/>
       <property name="dog" ref="dog"/>
       <property name="name" value="晓江"/>
   </bean>
   ```

4. 测试

   ```java
   @Test
   public void getUser(){
       ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
       User user = context.getBean("user", User.class);
       user.getCat().shout();
       user.getDog().shout();
   }
   ```

   输出正常

### 7.2、byName自动装配

**autowire byName (按名称自动装配)**

由于在手动配置xml过程中，常常发生字母缺漏和大小写等错误，而无法对其进行检查，使得开发效率降低。

采用自动装配将避免这些错误，并且使配置简单化。

测试：修改bean配置，增加一个属性  autowire="byName"

```xml
<bean id="cat" class="com.xj.pojo.Cat"/>
<bean id="dog" class="com.xj.pojo.Dog"/>

<!---
	byName:会在容器上下文中查找，和自己对象set方法后面的值对应的bean id
	byType:会在容器上下文中查找，和自己属性类型相同的bean
-->
<bean id="user" class="com.xj.pojo.User" autowire="byName">
    <property name="name" value="晓江"/>
</bean>
```

- ==注意：User类中的属性名必须和定义的id对应，否则输出不了==

### 7.3、byType自动装配

```xml
<bean id="cat" class="com.xj.pojo.Cat"/>
<bean id="dog" class="com.xj.pojo.Dog"/>
<bean id="user" class="com.xj.pojo.User" autowire="byType">
    <property name="name" value="晓江"/>
</bean>
```

- ==使用autowire byType首先需要保证：同一类型的对象，在spring容器中唯一。如果不唯一，会报不唯一的异常。==

### 7.4、使用自动装配注解

- **注解是使用反射获取的，所以可以不用set方法**

![image-20200528132055338](.\Spring.assets\image-20200528132055338.png)

- jdk1.5开始支持注解，spring2.5开始全面支持注解。
- The introduction of annotation-based configuration raised the question of whether this approach is “better” than XML.

要使用注解须知：

1. 导入约束context约束

2. ==配置注解的支持：`<context:annotation-config>`==

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           https://www.springframework.org/schema/context/spring-context.xsd">
   
       <context:annotation-config/>
   
   </beans>
   ```

#### @Autowired

1. 再User类中使用@Autowired注解

   ```java
   public class User {
       @Autowired
       private Cat cat;
       @Autowired
       private Dog dog;
       private String name;
       ...
   }
   ```

2. 此时配置文件内容

   ```xml
   <context:annotation-config/>
   <bean id="cat" class="com.xj.pojo.Cat"/>
   <bean id="dog" class="com.xj.pojo.Dog"/>
   <bean id="user" class="com.xj.pojo.User">
       <property name="name" value="晓江"/>
   </bean>
   ```

3. 测试，成功输出结果！

【科普】

- @Autowired(required=false)  说明：false，对象可以为null；true，对象必须存对象，不能为null。

- ```java
  //如果允许对象为null，设置required = false,默认为true
  @Autowired(required = false)
  private Cat cat;
  ```

#### @Qualifier

- **@Autowired是根据类型自动装配的**，加上**@Qualifier则可以根据byName的方式自动装配**
- **@Qualifier不能单独使用**

**测试实验步骤：**

1、配置文件修改内容，保证类型存在对象。且名字不为类的默认名字！

```xml
<bean id="cat1" class="com.xj.pojo.Cat"/>
<bean id="cat2" class="com.xj.pojo.Cat"/>
<bean id="dog1" class="com.xj.pojo.Dog"/>
<bean id="dog2" class="com.xj.pojo.Dog"/>
```

2、没有加Qualifier测试，直接报错

3、在属性上添加Qualifier注解

```java
@Autowired
@Qualifier(value = "cat1")
private Cat cat;
@Autowired
@Qualifier(value = "dog1")
private Dog dog;
private String name;
```

![image-20200528140419412](.\Spring.assets\image-20200528140419412.png)

测试，成功输出！



#### @Resource

- @Resource如有**指定的name属性**，先按该属性进行byName方式查找装配；
- 其次再进行默认的byName方式进行装配；
- 如果以上都不成功，则按byType的方式自动装配。
- 都不成功，则报异常。

实体类：

```java
public class User {
   //如果允许对象为null，设置required = false,默认为true
   @Resource(name = "cat2")
   private Cat cat;
   @Resource
   private Dog dog;
   private String str;
}
```

beans.xml

```xml
<bean id="dog" class="com.xj.pojo.Dog"/>
<bean id="cat1" class="com.xj.pojo.Cat"/>
<bean id="cat2" class="com.xj.pojo.Cat"/>

<bean id="user" class="com.xj.pojo.User"/>
```

**测试：结果OK，删掉cat2依然可以**

结论：先进行byName查找，失败；再进行byType查找，成功。



### 7.5、小结

1. @Autowired与@Resource都可以用来装配bean。都可以写在字段上，或写在setter方法上。
2. 它们的作用相同都是用注解方式注入对象，但执行顺序不同。@Autowired先byType，@Resource先byName。



## 8、使用注解开发

在Spring4之后，要使用注解开发，必须保证aop的包导入了，一般使用spring-webmvc会自动导入

![image-20200528142612746](.\Spring.assets\image-20200528142612746.png)

在配置文件当中，还得要引入一个context约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:context="http://www.springframework.org/schema/context"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd">

</beans>
```



### 8.1、bean

- 配置扫描哪些包下的注解

```xml
<!--指定注解扫描包-->
<context:component-scan base-package="com.xj.pojo"/>
```

- 在指定包下编写类，增加注解

```java
@Component("user")
// 相当于配置文件中 <bean id="user" class="com.xj.pojo.User"/>
public class User {
    private String name = "晓江";
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}
```

- 测试

```java
@Test
public void getUser(){
    ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
    User user = (User) context.getBean("user");
    System.out.println(user.getName());
}
```

### 8.2、属性注入

使用注解注入属性

- 可以不用提供set方法，直接在直接名上添加@value("值")

```java
@Component("user")
// 相当于  <bean id="user" class="com.xj.pojo.User"/>
public class User {
    @Value("晓江")
    // 相当于配置文件中 <property name="name" value="晓江"/>
    private String name ;
```

- 如果提供了set方法，在set方法上添加@value("值");

```java
@Component("user")
// 相当于  <bean id="user" class="com.xj.pojo.User"/>
public class User {
    private String name ;

    public String getName() {
        return name;
    }
    @Value("晓江")
    // 相当于配置文件中 <property name="name" value="晓江"/>
    public void setName(String name) {
        this.name = name;
    }
}
```

### 8.3、衍生的注解

我们这些注解，就是替代了在配置文件当中配置步骤而已！更加的方便快捷！

**@Component三个衍生注解**

为了更好的进行分层，Spring可以使用其它三个注解，功能一样，目前使用哪一个功能都一样。

- **@Controller：control层**
- **@Service：service层**
- **@Repository：dao层**

**写上这些注解，就相当于将这个类交给Spring管理装配了！**



### 8.4、自动装配

- 在7.4中有详细介绍

### 8.5、作用域

@scope

- singleton：单例模式。默认的，Spring会采用单例模式创建这个对象。关闭工厂 ，所有的对象都会销毁。
- prototype：多例模式。关闭工厂 ，所有的对象不会销毁。内部的垃圾回收机制会回收

```java
@Controller("user")
@Scope("prototype")
public class User {
   @Value("晓江")
   public String name;
}
```



### 8.6、小结

**XML与注解比较**

- XML可以适用任何场景 ，结构清晰，维护方便
- 注解不是自己提供的类使用不了，开发简单方便

**xml与注解整合开发** ：推荐最佳实践

- xml管理Bean
- 注解完成属性注入
- 使用过程中， 可以不用扫描，扫描是为了类上的注解

- 自动装配需要`<context:annotation-config/>`
- 注解开发需要`<context:component-scan base-package="com.xj.pojo"/>`



## 9、使用java的方式配置Spring

- 不使用xml配置文件

- JavaConfig 原来是 Spring 的一个子项目，它通过 Java 类的方式提供 Bean 的定义信息，在 Spring4 的版本， JavaConfig 已正式成为 Spring4 的核心功能 。

### 9.1、实体类

```java
// @Component将这个类注解为Spring的一个组件，放到容器中！
@Component
public class User {
    public String name = "晓江";
}
```

### 9.2、配置类

```java
// 这个也会被Spring容器托管，注册到容器中，因为它本来就是一个@Component
// @Configuration代表这是一个配置类，就和我们之前看的bean.xml一样
@Configuration
@ComponentScan("com.xj.pojo")
// 可以导入其他文件
@Import(MyConfig2.class)
public class MyConfig1 {
    // 注册一个bean，就相当于我们之前写的一个bean标签
    // 这个方法的名字，就相当于bean标签中的id属性
    // 这个方法的返回值，就相当于bean标签中class属性
    @Bean
    public User user(){
        // 就是返回要注入到bean中的对象！
        return new User();
    }
}
```

### 9.3、测试类

（使用AnnotationConfigApplicationContext获取配置类）

```java
@Test
public void getUser(){
    ApplicationContext context = new AnnotationConfigApplicationContext(MyConfig1.class);
    User user = (User) context.getBean("user");
    System.out.println(user.name);
}
```

测试成功！

==当有多个配置类的话，可以使用@Import(MyConfig2.class)导入==

1. 效果图（不需要xml配置文件）

   ![image-20200528155246750](.\Spring.assets\image-20200528155246750.png)

这种纯java 的配置方式，在SpringBoot中随处可见

### 9.4、将主动权放到pojo中

- 将config类中的@Bean标签取消掉
- 在pojo中设置@Component("user")，并对属性赋值
- 效果跟==8==中的使用注解开发效果一样

1. 实体类

   ```java
   @Component("user")
   public class User {
       public String name = "晓江";
   }
   ```

2. 配置类

   ```java
   // 只需要这几部分
   // @Configuration代表这是一个配置类
   @Configuration	
   // 指定注解扫描包
   // 类似于 <context:component-scan base-package="com.xj.pojo"/>
   @ComponentScan("com.xj.pojo")	
   public class MyConfig1 {
   }
   ```

3. 测试与上方一致



## 10、代理模式

![image-20200528214649380](.\Spring.assets\image-20200528214649380.png)



### 10.1、静态代理

**静态代理角色分析**

- 抽象角色 : 一般使用接口或者抽象类来实现（出租这件事）
- 真实角色 : 被代理的角色（房东）
- 代理角色 : 代理真实角色 ; 代理真实角色后 , 一般会做一些附属的操作 .（中介）
- 客户  :  使用代理角色来进行一些操作 （ 客户）

> 通过代理可以在不修改 “房东” 的情况下增加一些操作，在代理代码中获取 “房东” 对象，在使用其方法的基础上增加操作

- **代码实现**

  1. Rent.java 事务接口

     ```java
     public interface Rent {
         // 租房
         public void rent();
     }
     ```

  2. Host.java 房东

     ```java
     public class Host implements Rent {
         @Override
         public void rent() {
             System.out.println("房东出租房子");
         }
     }
     ```

  3. Proxy.java 代理中介

     ```java
     public class Proxy implements Rent {
         // 中介可以联系到房东
         private Host host;
         public void setHost(Host host) {
             this.host = host;
         }
         @Override
         public void rent() {
             showHost();
             fare();
             host.rent();
         }
         // 中介自己的功能
         public void showHost(){
             System.out.println("带客看房");
         }
         public void fare(){
             System.out.println("收取中介费");
         }
     }
     ```

  4. Client.java 客户

     ```java
     public class Client {
         public static void main(String[] args) {
             // 代理帮客户联系房东，并收取相应服务费
             Proxy proxy = new Proxy();
             proxy.setHost(new Host());
             proxy.rent();
         }
     }
     ```

  5. 结果

     ![image-20200528225451878](.\Spring.assets\image-20200528225451878.png)

**静态代理的好处:**

- 可以使得我们的真实角色更加纯粹 . 不再去关注一些公共的事情 
- 公共的业务由代理来完成 . 实现了业务的分工 
- 公共业务发生扩展时变得更加集中和方便 

**缺点 :**

- 类多了 , 多了代理类 , 工作量变大了 . 开发效率降低 .



> 我们想要静态代理的好处，又不想要静态代理的缺点，所以 , 就有了动态代理 !
>
> 我们在不改变原来的代码的情况下，实现了对原有功能的增强，这是AOP中最核心的思想

- 纵向开发，横向开发

![image-20200530160030207](.\Spring.assets\image-20200530160030207.png)



---



### 10.2、动态代理

- 动态代理和静态代理角色一样
- 动态代理的代理类是动态生成的
- 动态代理分为两大类：一类是基于接口动态代理 , 一类是基于类的动态代理
  - 基于接口的动态代理----JDK动态代理
  - 基于类的动态代理--cglib
  - java字节码实现--javasist 
- **JDK的动态代理需要了解两个类**
  - 核心 : InvocationHandler   和   Proxy  

【InvocationHandler：调用处理程序 invoke】

![image-20200530160050119](.\Spring.assets\image-20200530160050119.png)

```java
Object invoke(Object proxy, 方法 method, Object[] args)；
//参数
//proxy - 调用该方法的代理实例
//method -所述方法对应于调用代理实例上的接口方法的实例。方法对象的声明类将是该方法声明的接口，它可以是代理类继承该方法的代理接口的超级接口。
//args -包含的方法调用传递代理实例的参数值的对象的阵列，或null如果接口方法没有参数。原始类型的参数包含在适当的原始包装器类的实例中，例如java.lang.Integer或java.lang.Boolean 。
```

【Proxy  : 代理】

```java
//生成代理类
public Object getProxy(){
   return Proxy.newProxyInstance(this.getClass().getClassLoader(),
                                 rent.getClass().getInterfaces(),this);
}
```



- 代码实现

  1. 编写抽像接口

     ```java
     //抽象角色：租房
     public interface Rent {
         public void rent();
     }
     ```

  2. 真实角色

     ```java
     //真实角色: 房东，房东要出租房子
     public class Host implements Rent{
         @Override
         public void rent() {
             System.out.println("房东出租房子");
         }
     }
     ```

  3. 代理角色

     ```java
     public class ProxyInvocationHandler implements InvocationHandler {
         private Rent rent;
         public void setRent(Rent rent){
             this.rent = rent;
         }
     
         //生成代理类，重点是第二个参数，获取要代理的抽象角色！之前都是一个角色，现在可以代理一类角色
         public Object getProxy(){
             return Proxy.newProxyInstance(this.getClass().getClassLoader(),
                     rent.getClass().getInterfaces(),this);
         }
     
         // proxy : 代理类      method : 代理类的调用处理程序的方法对象.
         // 处理代理实例上的方法调用并返回结果
         @Override
         public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
             //核心：本质利用反射实现！
             Object result = method.invoke(rent, args);
             return result;
         }
     }
     ```

  4. 使用（租客）

     ```java
     // 租客
     public class Client {
         public static void main(String[] args) {
             // 真实角色
             Host host = new Host();
             // 代理实例的调用处理程序
             ProxyInvocationHandler pih = new ProxyInvocationHandler();
             pih.setRent(host); // 将真实角色放进去
             Rent proxy = (Rent) pih.getProxy();
             proxy.rent();
         }
     }
     ```

     核心：**一个动态代理 , 一般代理某一类业务 , 一个动态代理可以代理多个类，代理的是接口！**

     ---

     

#### 动态代理深入理解

1. 抽象角色

   ```java
   //抽象角色：增删改查业务
   public interface UserService {
       void add();
       void delete();
       void updata();
       void query();
   }
   ```

2. 真实对象

   ```java
   //真实对象，完成增删改查操作的人
   public class UserServiceImpl implements  UserService {
       public void add() {
           System.out.println("增加了一个用户");
       }
   
       public void delete() {
           System.out.println("删除了一个用户");
       }
   
       public void updata() {
           System.out.println("更新了一个用户");
       }
   
       public void query() {
           System.out.println("查询了一个用户");
       }
   }
   ```

3. 代理角色

   ```java
   public class ProxyInvocationHandler implements InvocationHandler {
       // 抽象角色
       private UserService userService;
       public void setUserService(UserService userService) {
           this.userService = userService;
       }
   
       // 生成代理类
       public Object getProxy(){
           return Proxy.newProxyInstance(this.getClass().getClassLoader(),
                   userService.getClass().getInterfaces(),this);
       }
   
       // proxy : 代理类
       // method : 代理类的调用处理程序的方法对象.
       // 用来设置代理的方法
       @Override
       public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
           // method.getName()获取调用方法的方法名
           log(method.getName());
           // 核心：本质利用反射实现！
           Object result = method.invoke(userService, args);
           return result;
       }
       public void log(String methodName){
           System.out.println("执行了" + methodName + "方法");
       }
   }
   ```

4. 使用（调用者）

   ```java
   public class Client {
       public static void main(String[] args) {
           // 真实对象
           UserServiceImpl userService = new UserServiceImpl();
           // 代理对象的调用处理程序
           ProxyInvocationHandler pih = new ProxyInvocationHandler();
           // 设置要代理的对象
           pih.setUserService(userService);
           // 动态生成代理类
           UserService proxy = (UserService) pih.getProxy();
           proxy.add();
       }
   }
   ```

#### ==动态代理优化==

- 修改代理角色，使其具有通用性（将抽象角色设置为Object）

  ```java
  public class ProxyInvocationHandler implements InvocationHandler {
      // 将抽象角色设置为Object
      private Object target;
  
      public void setTarget(Object target) {
          this.target = target;
      }
      // 返回的代理类是接口类型的
      public Object getProxy(){
          return Proxy.newProxyInstance(this.getClass().getClassLoader(),
                  target.getClass().getInterfaces(),this);
      }
      public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
          log(method.getName());
          // 核心
          Object result = method.invoke(target,args);
          return result;
      }
      public void log(String methodName){
          System.out.println("执行了" + methodName + "方法");
      }
  }
  ```

- 在"租房"实例中使用

  ```java
  @Test
  public void rentProxy(){
      // 真实对象
      Host host = new Host();
      // 获取代理对象的调用程序
      ProxyInvocationHandler pih = new ProxyInvocationHandler();
      // 设置代理对象
      pih.setTarget(host);
      // 获取接口代理
      Rent proxy = (Rent) pih.getProxy();
      proxy.rent();
  }
  ```

- 在调用方法中使用

  ```java
  @Test
  public void userServiceProxy(){
      // 真实对象
      UserServiceImpl userService = new UserServiceImpl();
      // 获取代理对象的调用程序
      ProxyInvocationHandler pih = new ProxyInvocationHandler();
      // 设置代理对象
      pih.setTarget(userService);
      // 获取接口代理
      UserService proxy = (UserService) pih.getProxy();
      proxy.add();
  }
  ```

以上测试结果均正常输出，并添加上了日志信息，使用method.getName()获取每个方法的方法名。

---



#### 小结：

1. 动态代理的角色：
   - 抽象角色
   - 真实对象（实现抽象角色）
   - 代理角色（实现InvocationHandler）
   - 使用者（租客）
2. 动态代理没有代理类，只有用来生成代理类的代理角色
3. 代理角色需要实现`InvocationHandler`接口
   - 并在其中定义`抽象角色`
   - 定义获取代理类的方法`getProxy`
   - 实现设置代理类方法的方法`invoke`
   - 方法`invoke`的核心是`Object result = method.invoke(target,args);`
4. 在测试程序中需要定义`真实对象`，并获取到`接口代理`
5. 注意：代理类是使用==接口类型==

---



## 11、AOP

### 11.1、什么是AOP

AOP（Aspect Oriented Programming）意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的**耦合度降低**，提高程序的可重用性，同时提高了开发的效率。



![image-20200530160106524](.\Spring.assets\image-20200530160106524.png)



### 11.2、Aop在Spring中的作用

提供声明式事务；允许用户自定义切面

以下名词需要了解下：

- 横切关注点：跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志 , 安全 , 缓存 , 事务等等 ....
- 切面（ASPECT）：横切关注点 被模块化 的特殊对象。即 它是一个类。
- 通知（Advice）：切面必须要完成的工作。即，它是类中的一个方法。
- 目标（Target）：被通知对象。
- 代理（Proxy）：向目标对象应用通知之后创建的对象。
- 切入点（PointCut）：切面通知 执行的 “地点”的定义。
- 连接点（JointPoint）：与切入点匹配的执行点

![image-20200530160121216](.\Spring.assets\image-20200530160121216.png)

SpringAOP中，通过Advice定义横切逻辑，Spring中支持5种类型的Advice:

- 前置通知
- 后置通知
- 环绕通知
- 异常抛出通知
- 引介通知



![image-20200530160132321](.\Spring.assets\image-20200530160132321.png)



### 11.3、使用Spring实现AOP

**【重点】使用AOP织入，需要导入一个依赖包！**

```xml
<!-- AOP织入 -->
<dependency>
   <groupId>org.aspectj</groupId>
   <artifactId>aspectjweaver</artifactId>
   <version>1.9.4</version>
</dependency>
```



#### 第一种方式

【主要SpringAPI接口实现】

​	**通过 Spring API 实现**

1. 编写业务接口和实现类

   ```java
   public interface UserService {
       public void add();
       public void delete();
       public void updata();
       public void search();
   }
   ```

   ```java
   public class UserServiceImpl implements UserService {
       public void add() {
           System.out.println("增加用户");
       }
   
       public void delete() {
           System.out.println("删除用户");
       }
   
       public void updata() {
           System.out.println("更新用户");
       }
   
       public void search() {
           System.out.println("更新用户");
       }
   }
   ```

2. 编写增强类（前置和后置）

   - 前置增强（实现MethodBeforeAdvice 接口 ）

   ```java
   public class Log implements MethodBeforeAdvice {
       //method : 要执行的目标对象的方法
       //objects : 被调用的方法的参数
       //Object : 目标对象
       public void before(Method method, Object[] objects, Object target) throws Throwable {
           System.out.println(target.getClass() + "的" + method.getName() + "方法被执行了");
       }
   }
   ```

   - 后置增强（实现AfterReturningAdvice 接口）

   ```java
   public class AfterLog implements AfterReturningAdvice {
       //returnValue 返回值
       //method 被调用的方法
       //args 被调用的方法的对象的参数
       //target 被调用的目标对象
       public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
           System.out.println("执行了" + target.getClass().getName() + "的" + method.getName() + "方法" +
                   "返回值：" + returnValue);
       }
   }
   ```

3. 在spring的文件中注册 , 并实现aop切入实现 , 注意导入约束 

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/aop
          http://www.springframework.org/schema/aop/spring-aop.xsd">
       
       <!--注册bean-->
       <bean id="userServuce" class="com.xj.service.UserServiceImpl"/>
       <bean id="log" class="com.xj.log.Log"/>
       <bean id="afterlog" class="com.xj.log.AfterLog"/>
       
       <!--aop的配置-->
       <aop:config>
           <!--切入点 pointcut; expression:表达式匹配要执行的方法-->
           <aop:pointcut id="pointcut" expression="execution(* com.xj.service.UserServiceImpl.*(..))"/>
           <!--执行环绕；advice-ref执行方法，pointcut-ref切入点-->
           <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
           <aop:advisor advice-ref="afterlog" pointcut-ref="pointcut"/>
       </aop:config>
   
   </beans>
   ```

4. 测试

   ```java
   public class MyTest {
       @Test
       public void test(){
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserService userService = (UserService) context.getBean("userService");
           userService.add();
       }
   }
   ```

5. 结果

   ![image-20200529151048628](.\Spring.assets\image-20200529151048628.png)

##### 小结：

1. 使用aop的核心在于增强和配置文件

2. 配置文件使用：

   - 增加aop约束

   - 注册bean： 真实对象 以及 增强类

   - aop的配置<aop:congfig>：切入点、增强方法的配置

     ```xml
     <aop:config>
         <!--切入点 pointcut; expression:表达式匹配要执行的方法-->
         <aop:pointcut id="pointcut" expression="execution(* com.xj.service.UserServiceImpl.*(..))"/>
         <!--执行环绕；advice-ref执行方法，pointcut-ref切入点-->
         <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
         <aop:advisor advice-ref="afterlog" pointcut-ref="pointcut"/>
     </aop:config>
     ```



#### 第二种方式（自定义切面）aop:aspect

【主要是切面定义，简单点】

1. 定义一个自己的切入类

   ```java
   public class DiyPointcut {
       public void before(){
           System.out.println("=============方法执行前=========");
       }
       public void after(){
           System.out.println("==============方法执行后==========");
       }
   }
   ```
   
2. 在Spring中配置

   ```xml
   <!--第二种方式自定义实现-->
   <!--注册bean-->
   <bean id="diy" class="com.xj.diy.DiyPointcut"/>
   <!--aop的配置-->
   <aop:config>
       <!--第二种方式：使用AOP的标签使用-->
       <aop:aspect ref="diy">
           <aop:pointcut id="diyPointcut" expression="execution(* com.xj.service.UserServiceImpl.*(..))"/>
           <aop:before method="before" pointcut-ref="diyPointcut"/>
           <aop:after method="after" pointcut-ref="diyPointcut"/>
       </aop:aspect>
   </aop:config>
   ```

3. 测试（跟方式一的一样）

   ```java
   @Test
   public void test(){
       ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
       UserService userService = (UserService) context.getBean("userService");
       userService.updata();
   }
   ```

4. 结果

   ![image-20200529153338154](.\Spring.assets\image-20200529153338154.png)



#### 第三种方式（注解）@Aspect

【使用注解】

1. 编写一个注解实现的增强类

   ```java
   package com.xj.diy;
   
   import org.aspectj.lang.ProceedingJoinPoint;
   import org.aspectj.lang.annotation.After;
   import org.aspectj.lang.annotation.Around;
   import org.aspectj.lang.annotation.Aspect;
   import org.aspectj.lang.annotation.Before;
   
   @Aspect
   public class AnnotationPointcut {
   
       @Before("execution(* com.xj.service.UserServiceImpl.*(..))")
       public void before(){
           System.out.println("--------方法执行前---------");
       }
   
       @After("execution(* com.xj.service.UserServiceImpl.*(..))")
       public void after(){
           System.out.println("--------方法执行后---------");
       }
   
       @Around("execution(* com.xj.service.UserServiceImpl.*(..))")
       public void around(ProceedingJoinPoint jp) throws Throwable {
           System.out.println("环绕前");
           System.out.println("签名:"+jp.getSignature());
           //执行目标方法proceed
           Object proceed = jp.proceed();
           System.out.println("环绕后");
           System.out.println(proceed);
       }
   }
   ```

   ![image-20200529155346412](.\Spring.assets\image-20200529155346412.png)

2. 在Spring配置文件中，注册bean，并增加支持注解的配置

   ```xml
   <!--第三种方式:注解实现-->
   <bean id="annotationPointcut" class="com.xj.diy.AnnotationPointcut"/>
   <aop:aspectj-autoproxy/>
   ```

   <aop:aspectj-autoproxy>标签说明：

   ```xml
   通过aop命名空间的<aop:aspectj-autoproxy />声明自动为spring容器中那些配置@Aspect切面的bean创建代理，织入切面。当然，spring 在内部依旧采用AnnotationAwareAspectJAutoProxyCreator进行自动代理的创建工作，但具体实现的细节已经被<aop:aspectj-autoproxy />隐藏起来了
   
   <aop:aspectj-autoproxy />有一个proxy-target-class属性，默认为false，表示使用jdk动态代理织入增强，当配为<aop:aspectj-autoproxy  poxy-target-class="true"/>时，表示使用CGLib动态代理技术织入增强。不过即使proxy-target-class设置为false，如果目标类没有声明接口，则spring将自动使用CGLib动态代理。
   ```

3. 测试方法不变（Spring中要有注册userService的bean）

   ```java
   @Test
   public void test(){
       ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
       UserService userService = (UserService) context.getBean("userService");
       userService.updata();
   }
   ```

4. 结果

   ![image-20200529155951329](.\Spring.assets\image-20200529155951329.png)





## 12、整合MyBatis

### 12.1、基本配置

1. 导入相关jar包

   ```xml
   <dependencies>
       <!--junit-->
       <dependency>
           <groupId>junit</groupId>
           <artifactId>junit</artifactId>
           <version>4.12</version>
       </dependency>
       <!--mybatis-->
       <dependency>
           <groupId>org.mybatis</groupId>
           <artifactId>mybatis</artifactId>
           <version>3.5.2</version>
       </dependency>
       <!--mysql-connector-java-->
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>5.1.47</version>
       </dependency>
       <!--spring相关-->
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-webmvc</artifactId>
           <version>5.2.5.RELEASE</version>
       </dependency>
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-jdbc</artifactId>
           <version>5.1.10.RELEASE</version>
       </dependency>
       <!--aspectJ AOP 织入器-->
       <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
       <dependency>
           <groupId>org.aspectj</groupId>
           <artifactId>aspectjweaver</artifactId>
           <version>1.9.4</version>
       </dependency>
       <!--mybatis-spring整合包 【重点】-->
       <dependency>
           <groupId>org.mybatis</groupId>
           <artifactId>mybatis-spring</artifactId>
           <version>2.0.2</version>
       </dependency>
       <!--Lombok-->
       <dependency>
           <groupId>org.projectlombok</groupId>
           <artifactId>lombok</artifactId>
           <version>1.18.10</version>
       </dependency>
   </dependencies>
   ```

2. 配置Maven静态资源过滤问题！

   ```xml
   <build>
       <resources>
           <resource>
               <directory>src/main/java</directory>
               <includes>
                   <include>**/*.properties</include>
                   <include>**/*.xml</include>
               </includes>
               <filtering>true</filtering>
           </resource>
       </resources>
   </build>
   ```

3. 编写配置文件

4. 代码实现



### 12.2、回忆MyBatis的使用

1. 编写实体类pojo

   ```java
   @Data
   public class User {
       private int id;
       private String name;
       private String pwd;
   }
   ```

2. 实现mybatis配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
   
       <typeAliases>
           <package name="com.xj.pojo"/>
       </typeAliases>
   
       <environments default="development">
           <environment id="development">
               <transactionManager type="JDBC"/>
               <dataSource type="POOLED">
                   <property name="driver" value="com.mysql.jdbc.Driver"/>
                   <property name="url" value="jdbc:mysql://localhost:3306/test?serverTimezone=GMT&amp;createDatabaseIfNotExist=true&amp;useUnicode=true&amp;characterEncoding=utf-8&amp;autoReconnect=true"/>
                   <property name="username" value="root"/>
                   <property name="password" value="1234"/>
               </dataSource>
           </environment>
       </environments>
   
       <mappers>
           <package name="com.xj.mapper"/>
       </mappers>
   </configuration>
   ```

3. 编写mapper接口

   ```java
   public interface UserMapper {
       List<User> selectUser();
   }
   ```

   

4. 编写mapper.xml映射文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper>
       <select id="selectUser" resultType="User">
           select * from mybatis.user
       </select>
   </mapper>
   ```

   

5. 测试

   ```java
   @Test
   public void test() throws IOException {
       // 省略了工具类
       String resource = "mybatis-config.xml";
       InputStream in = Resources.getResourceAsStream(resource);
       SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(in);
       // 开启自动提交事务
       SqlSession sqlSession = sqlSessionFactory.openSession(true); 
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       List<User> userList = mapper.selectUser();
       for (User user : userList) {
           System.out.println(user);
       }
       sqlSession.close();
   }
   ```

   成功读取出数据，配置成功，注意mysql的url会有不同，因为我使用的是**mysql8.0版本**

---



### 12.3、MyBatis-Spring概念

![image-20200529174714149](.\Spring.assets\image-20200529174714149.png)

- 官网：http://mybatis.org/spring/zh/index.html

- 什么是 MyBatis-Spring？

  - MyBatis-Spring 会帮助你将 MyBatis 代码无缝地整合到 Spring 中。

- **知识基础**

  - 在开始使用 MyBatis-Spring 之前，你需要先熟悉 Spring 和 MyBatis 这两个框架和有关它们的术语。这很重要
  - MyBatis-Spring 需要对应以下版本：

  ![image-20200529174902347](.\Spring.assets\image-20200529174902347.png)

- 如果使用 Maven 作为构建工具，仅需要在 pom.xml 中加入以下代码即可：

  ```xml
  <dependency>
     <groupId>org.mybatis</groupId>
     <artifactId>mybatis-spring</artifactId>
     <version>2.0.2</version>
  </dependency>
  ```

- 要和 Spring 一起使用 MyBatis，需要在 Spring 应用上下文中定义至少两样东西：一个 SqlSessionFactory 和至少一个数据映射器类。


- 在 MyBatis-Spring 中，可使用SqlSessionFactoryBean来创建 SqlSessionFactory。要配置这个工厂 bean，只需要把下面代码放在 Spring 的 XML 配置文件中：


  ```xml
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource" />
</bean>
  ```

  **注意**：SqlSessionFactory需要一个 DataSource（数据源）。这可以是任意的 DataSource，只需要和配置其它 Spring 数据库连接一样配置它就可以了。

-   **在 MyBatis-Spring 中，则使用 SqlSessionFactoryBean 来创建。**


  在 MyBatis 中，你可以使用 SqlSessionFactory 来创建 SqlSession。一旦你获得一个 session 之后，你可以使用它来执行映射了的语句，提交或回滚连接，最后，当不再需要它的时候，你可以关闭 session。

  SqlSessionFactory有一个唯一的必要属性：用于 JDBC 的 DataSource。这可以是任意的 DataSource 对象，它的配置方法和其它 Spring 数据库连接是一样的。

  一个常用的属性是 configLocation，它用来指定 MyBatis 的 XML 配置文件路径。它在需要修改 MyBatis 的基础配置非常有用。通常，基础配置指的是 < settings> 或 < typeAliases>元素。

  需要注意的是，**这个配置文件并不需要是一个完整的 MyBatis 配置**。确切地说，任何环境配置（<environments>），数据源（<DataSource>）和 MyBatis 的事务管理器（<transactionManager>）都会被忽略。SqlSessionFactoryBean 会创建它自有的 MyBatis 环境配置（Environment），并按要求设置自定义环境的值。

  **SqlSessionTemplate（模板） 是 MyBatis-Spring 的核心。作为 SqlSession 的一个实现，这意味着可以使用它无缝代替你代码中已经在使用的 SqlSession。**

  **模板可以参与到 Spring 的事务管理中，并且由于其是线程安全的**，可以供多个映射器类使用，你应该总是用 SqlSessionTemplate 来替换 MyBatis 默认的 DefaultSqlSession 实现。在同一应用程序中的不同类之间混杂使用可能会引起数据一致性的问题。

  **可以使用 SqlSessionFactory 作为构造方法的参数来创建 SqlSessionTemplate 对象。**

  ```xml
<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
    <constructor-arg index="0" ref="sqlSessionFactory" />
</bean>
  ```

  现在，这个 bean 就可以直接注入到你的 DAO bean 中了。你需要在你的 bean 中添加一个 SqlSession 属性，就像下面这样：

  ```java
public class UserDaoImpl implements UserMapper {

    private SqlSession sqlSession;

    public void setSqlSession(SqlSession sqlSession) {
        this.sqlSession = sqlSession;
    }

    public User getUser(String userId) {
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        return mapper.selectUser();
    }
}
  ```

  按下面这样，注入 SqlSessionTemplate：

  ```xml
<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
    <constructor-arg index="0" ref="sqlSessionFactory" />
</bean>

<bean id="userDao" class="org.mybatis.spring.sample.dao.UserDaoImpl">
    <property name="sqlSession" ref="sqlSession" />
</bean>
  ```

---



### 12.4、整合实现一

1. 引入Spring配置文件Spring-beans.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
       
   </beans>
   ```

2. 配置数据源替换mybaits的数据源（注意修改url的版本参数【这是mysql8.0的】，以及用户名、密码）

   ```xml
   <!--配置数据源：数据源有非常多，可以使用第三方的，也可使使用Spring的-->
   <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
       <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
       <property name="url" value="jdbc:mysql://localhost:3306/test?serverTimezone=GMT&amp;createDatabaseIfNotExist=true&amp;useUnicode=true&amp;characterEncoding=utf-8&amp;autoReconnect=true"/>
       <property name="username" value="root"/>
       <property name="password" value="1234"/>
   </bean>
   ```

3. Spring-bean.xml代替之后需要将mybatis-config.xml中重复的部分删掉，不然会报错（这是现在的mybatis-config.xml配置文件，只保留别名设置）

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
       <typeAliases>
           <package name="com.xj.pojo"/>
       </typeAliases>
   </configuration>
   ```

4. 配置SqlSessionFactory，关联MyBatis（在Spring-beans.xml中配置）

   ```xml
   <!--配置SqlSessionFactory-->
   <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
       <property name="dataSource" ref="dataSource"/>
       <!--关联Mybatis-->
       <property name="configLocation" value="classpath:mybatis-config.xml"/>
       <property name="mapperLocations" value="classpath:com/xj/mapper/*.xml"/>
   </bean>
   ```

5. 注册sqlSessionTemplate，关联sqlSessionFactory；

   ```xml
   <!--注册sqlSessionTemplate , 关联sqlSessionFactory-->
   <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
       <!--利用构造器注入-->
       <constructor-arg index="0" ref="sqlSessionFactory"/>
   </bean>
   ```

6. 增加Mapper接口的实现类；私有化sqlSessionTemplate

   ```java
   public class UserMapperImpl implements UserMapper{
       // sqlSession不用我们自己创建了，Spring创建
       private SqlSessionTemplate sqlSession;
   
       public void setSqlSession(SqlSessionTemplate sqlSession) {
           this.sqlSession = sqlSession;
       }
   
       public List<User> selectUser() {
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
           return mapper.selectUser();
       }
   }
   ```

7. 注册bean实现

   ```xml
   <!--注册bean实现-->
   <bean id="userMapper" class="com.xj.mapper.UserMapperImpl">
       <property name="sqlSession" ref="sqlSession"/>
   </bean>
   ```

8. 测试

   ```java
   public void test2(){
       // 这部分跟Spring中的获取一样
       ApplicationContext context = new ClassPathXmlApplicationContext("Spring-bean.xml");
       UserMapper mapper = (UserMapper) context.getBean("userMapper");
       // 这部分跟mybatis的部分一样
       List<User> users = mapper.selectUser();
       for (User user : users) {
           System.out.println(user);
       }
   }
   ```

   读取出数据库的信息，测试成功！

9. 结果图

   ![image-20200529183102165](.\Spring.assets\image-20200529183102165.png)

#### 小结：

1. 在Spring中整合MyBatis关键在于Spring-bean.xml的配置

   - 以下部分为相对固定的内容（配置之后不需要修改）

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://www.springframework.org/schema/beans
                                http://www.springframework.org/schema/beans/spring-beans.xsd">
     
         <!--配置数据源：数据源有非常多，可以使用第三方的，也可使使用Spring的-->
         <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
             <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
             <property name="url" value="jdbc:mysql://localhost:3306/test?serverTimezone=GMT&amp;createDatabaseIfNotExist=true&amp;useUnicode=true&amp;characterEncoding=utf-8&amp;autoReconnect=true"/>
             <property name="username" value="root"/>
             <property name="password" value="1234"/>
         </bean>
     
         <!--配置SqlSessionFactory-->
         <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
             <property name="dataSource" ref="dataSource"/>
             <!--关联Mybatis-->
             <property name="configLocation" value="classpath:mybatis-config.xml"/>
             <property name="mapperLocations" value="classpath:com/xj/mapper/*.xml"/>
         </bean>
     
         <!--注册sqlSessionTemplate,关联sqlSessionFactory-->
         <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
             <!--利用构造器注入-->
             <constructor-arg index="0" ref="sqlSessionFactory"/>
         </bean>
     
     </beans>
     ```

   - 然后再将新增的`UserMapperImpl`实现类进行注册（需要修改）

     ```xml
     <!--注册bean实现-->
     <bean id="userMapper" class="com.xj.mapper.UserMapperImpl">
         <property name="sqlSession" ref="sqlSession"/>
     </bean>
     ```

2. 新增Mapper接口的实现类 `UserMapperImpl`

3. 测试（使用方法和Spring中的相似）

   只需要修改接口和实现类

---



### 12.5、整合实现二

mybatis-spring1.2.3版以上的才有这个 

- mapper继承SqlSessionDaoSupport  , 直接利用 getSqlSession() 获得 , 然后直接注入`SqlSessionFactory `，比起方式1 , 不需要管理SqlSessionTemplate , 而且对事务的支持更加友好 . 可跟踪源码查看
- 相比方式一，只是少了使用`SqlSessionTemplate`私有化sqlSession的步骤
- 同时在bean中注入的是`sqlSessionFactory`

#### 测试

1. 将我们上面写的UserMapperImpl修改一下变成UserMapperImpl2

   ```java
   public class UserMapperImpl2 extends SqlSessionDaoSupport implements UserMapper {
       public List<User> selectUser() {
           // 一句话搞定
           return getSqlSession().getMapper(UserMapper.class).selectUser();
       }
   }
   ```

2. 修改bean的配置

   ```xml
   <bean id="userMapper2" class="com.xj.mapper.UserMapperImpl2">
       <property name="sqlSessionFactory" ref="sqlSessionFactory" />
   </bean>
   ```

3. 测试

   ```java
   @Test
   public void test3(){
       ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
       UserMapper mapper = (UserMapper) context.getBean("userMapper2");
       List<User> users = mapper.selectUser();
       for (User user : users) {
           System.out.println(user);
       }
   }
   ```

   测试成功！

   ---

   

### 12.6、整合框架优化

- 将Spring 的配置文件分为两个 `applicationContext.xml`和 `Spring-bean.xml`方便管理

  -  `Spring-bean.xml`用于配置连接信息，一般不需要再修改

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">
    
        <!--配置数据源：数据源有非常多，可以使用第三方的，也可使使用Spring的-->
        <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
            <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql://localhost:3306/test?serverTimezone=GMT&amp;createDatabaseIfNotExist=true&amp;useUnicode=true&amp;characterEncoding=utf-8&amp;autoReconnect=true"/>
            <property name="username" value="root"/>
            <property name="password" value="1234"/>
        </bean>
        <!--配置SqlSessionFactory-->
        <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
            <property name="dataSource" ref="dataSource"/>
            <!--关联Mybatis-->
            <property name="configLocation" value="classpath:mybatis-config.xml"/>
            <property name="mapperLocations" value="classpath:com/xj/mapper/*.xml"/>
        </bean>
        <!--注册sqlSessionTemplate,关联sqlSessionFactory-->
        <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
            <!--利用构造器注入-->
            <constructor-arg index="0" ref="sqlSessionFactory"/>
        </bean>
    </beans>
    ```

  - `applicationContext.xml`用于注册bean的实现

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">
        <!--导入另一个配置文件-->
        <import resource="Spring-bean.xml"/>
        
        <!--注册bean实现-->
        <bean id="userMapper" class="com.xj.mapper.UserMapperImpl">
            <property name="sqlSession" ref="sqlSession"/>
        </bean>
    
        <bean id="userMapper2" class="com.xj.mapper.UserMapperImpl2">
            <property name="sqlSessionFactory" ref="sqlSessionFactory" />
        </bean>
    </beans>
    ```

**总结 : 整合到spring以后可以完全不要mybatis的配置文件，除了这些方式可以实现整合之外，我们还可以使用注解来实现，这个等我们后面学习SpringBoot的时候还会测试整合！**



## 13、事务

- 事务在项目开发过程非常重要，涉及到数据的一致性的问题，不容马虎！
- 事务管理是企业级应用程序开发中必备技术，用来确保数据的完整性和一致性。

事务就是把一系列的动作当成一个独立的工作单元，这些动作要么全部完成，要么全部不起作用。

**事务的四个属性ACID**

1. 原子性（atomicity）
   - 事务是原子性操作，由一系列动作组成，事务的原子性确保动作要么全部完成，要么完全不起作用

2. 一致性（consistency）
   - 一旦所有事务动作完成，事务就要被提交。数据和资源处于一种满足业务规则的一致性状态中

3. 隔离性（isolation）
   - 可能多个事务会同时处理相同的数据，因此每个事务都应该与其他事务隔离开来，防止数据损坏

4. 持久性（durability）
   - 事务一旦完成，无论系统发生什么错误，结果都不会受到影响。通常情况下，事务的结果被写到持久化存储器中


### 13.1、测试：

将上面的代码拷贝到一个新项目中

在之前的案例中，我们给UserMapper接口新增两个方法，删除和增加用户；

1. 增加方法

   ```java
   //添加一个用户
   int addUser(User user);
   //根据id删除用户
   int deleteUser(int id);
   ```

2. 修改映射文件

   ```xml
   <insert id="addUser" parameterType="user">
       insert into mybatis.user (id, name, pwd) values (#{id}, #{name}, #{pwd});
   </insert>
   
   <delete id="deleteUser" parameterType="int">
    	<!-- 这里故意写错delete -->   
       deletes from mybatis.user where id = #{id}
   </delete>
   ```

3. 编写接口的实现类，在实现类中，我们去操作一波

   ```java
   public class UserMapperImpl extends SqlSessionDaoSupport implements UserMapper{
   
       public List<User> selectUser() {
           User user = new User(8,"小明","123456");
           UserMapper mapper = getSqlSession().getMapper(UserMapper.class);
           mapper.addUser(user);
           mapper.deleteUser(8);
           return mapper.selectUser();
       }
   
       public int addUser(User user) {
           return getSqlSession().getMapper(UserMapper.class).addUser(user);
       }
   
       public int deleteUser(int id) {
           return getSqlSession().getMapper(UserMapper.class).deleteUser(id);
       }
   }
   ```

4. 测试

   ```java
   @Test
   public void test(){
       ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
       UserMapper mapper = context.getBean("userMapper", UserMapper.class);
       List<User> users = mapper.selectUser();
       for (User user : users) {
           System.out.println(user);
       }
   }
   ```

   程序报错，但仍然会插入数据

   ![image-20200530144048743](.\Spring.assets\image-20200530144048743.png)

说明默认是没有开启事务的，我们想让他们都成功才成功，有一个失败，就都失败，我们就应该需要**事务！**

### 13.2、使用Spring管理事务

- 在Spring-bean.xml中配置

1. **注意头文件的约束导入 : tx**

   (这部分可以在配置好事务的通知之后 按alt+Enter自动生成)

   ```xml
   xmlns:tx="http://www.springframework.org/schema/tx"
   http://www.springframework.org/schema/tx
   http://www.springframework.org/schema/tx/spring-tx.xsd">
   ```

2. **JDBC事务**

   ```xml
   <!--配置事务管理器-->
   <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
       <property name="dataSource" ref="dataSource" />
   </bean>
   ```

3. **配置好事务管理器后我们需要去配置事务的通知**

   ```xml
   <!--配置事务通知-->
   <tx:advice id="txAdvice" transaction-manager="transactionManager">
       <tx:attributes>
           <!--配置哪些方法使用什么样的事务,配置事务的传播特性-->
           <!--propagation="REQUIRED" 为默认的传播特性-->
           <tx:method name="add" propagation="REQUIRED"/>
           <tx:method name="delete" propagation="REQUIRED"/>
           <tx:method name="update" propagation="REQUIRED"/>
           <tx:method name="search*" propagation="REQUIRED"/>
           <tx:method name="get" read-only="true"/>
           <!--以上可直接省略为下面一句-->
           <tx:method name="*" propagation="REQUIRED"/>
       </tx:attributes>
   </tx:advice>
   ```

4. **配置AOP**

   导入aop的头文件！

   ```xml
   <!--配置aop织入事务-->
   <aop:config>
       <aop:pointcut id="txPointcut" expression="execution(* com.xj.dao.*.*(..))"/>
       <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut"/>
   </aop:config>
   ```

4. **进行测试**

   删掉刚才插入的数据，再次测试！事务创建成功！
   
   只要将UserMapper.xml中的deletes改成delete即可正常运行！
   
   ![image-20200530145443356](.\Spring.assets\image-20200530145443356.png)

---



### 13.4、为什么需要配置事务？

- 如果不配置，就需要我们手动提交控制事务；
- 事务在项目开发过程非常重要，涉及到数据的一致性的问题，不容马虎！

## 14、异常：

-  Cause: org.springframework.jdbc.CannotGetJdbcConnectionException: Failed to obtain JDBC Connection; nested exception is com.mysql.jdbc.exceptions.jdbc4.MySQLNonTransientConnectionException: Could not create connection to database server. Attempted reconnect 3 times. Giving up.

  **解决方法：**在服务中将Mysql启动

  ![image-20200530132354579](.\Spring.assets\image-20200530132354579.png)



> Spring的内容就这么多了，需要掌握使用注解开发，因为到了SpringBoot之后就很少使用配置文件了

![image-20200530150742827](.\Spring.assets\image-20200530150742827.png)



> 本页学习资源来自b站：遇见狂神说 