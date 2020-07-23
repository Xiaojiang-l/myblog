## 1、简介

### 1.1、什么是Mybatis

![image-20200528181938030](.\mybatis.assets\image-20200528181938030.png)

- MyBatis 是一款优秀的**持久层框架**
- 它支持定制化 SQL、存储过程以及高级映射。
- MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。
- MyBatis 可以使用简单的 XML 或注解来配置和映射原生类型、接口和 Java 的 POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。
- MyBatis 本是[apache](https://baike.baidu.com/item/apache/6265)的一个开源项目[iBatis](https://baike.baidu.com/item/iBatis), 2010年这个项目由apache software foundation 迁移到了google code，并且改名为MyBatis 。
- 2013年11月迁移到Github。



如何获得Mybatis？

- maven仓库：

  ```xml
  <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
  <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.2</version>
  </dependency>
  ```

- Github ： https://github.com/mybatis/mybatis-3/releases

- 中文文档：https://mybatis.org/mybatis-3/zh/index.html

## 2、第一个Mybatis程序

步骤：创建数据表-->搭建环境-->编写代码-->测试程序

### 2.1、创建数据表

在数据库中新建一个表格并插入数据

```sql
CREATE DATABASE `mybatis`;
USE `mybatis`;
CREATE TABLE `user`(
	`id` INT(20) NOT NULL PRIMARY KEY,
	`name` VARCHAR(30) DEFAULT NULL,
	`pwd` VARCHAR(30) DEFAULT NULL
)DEFAULT CHARSET= utf8;

INSERT INTO `user` (`id`,`name`,`pwd`) VALUES
(1,'晓江','12345'),
(2,'张三','12345'),
(3,'李四','23123')
```

### 2.2、搭建环境

1、新建maven项目

2、导入maven依赖

```xml
<dependencies>
	<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
	<dependency>
		<groupId>org.mybatis</groupId>
		<artifactId>mybatis</artifactId>
		<version>3.5.2</version>
	</dependency>
	<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>8.0.11</version>
	</dependency>
     <!--junit-->
     <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.12</version>
     </dependency>
</dependencies>
```

3、编写mybatis的核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--configuration核心配置文件 -->
<configuration>
	<!-- default默认选择下面id为 development的环境-->
	<environments default="development">
		<environment id="development">
			<transactionManager type="JDBC" />
			<dataSource type="POOLED">
				<!-- 连接Mybatis数据库 -->
				<property name="driver" value="com.mysql.jdbc.Driver" />
				<property name="url"
					value="jdbc:mysql://localhost:3306/test?serverTimezone=GMT&amp;createDatabaseIfNotExist=true&amp;useUnicode=true&amp;characterEncoding=utf-8&amp;autoReconnect=true" />
				<property name="username" value="root" />
				<property name="password" value="1234" />
			</dataSource>
		</environment>
	</environments>
	<!-- 设置映射文件 -->
	<mappers>
        <!-- 路径必须用"/"分隔开 -->
		<mapper resource="com/xj/mybatis/mapper/UserMapper.xml"/>
	</mappers>
</configuration>
```



### 2.3、编写代码

1、编写实体类User

```java
package com.xj.mybatis.pojo;

public class User {
	private int id;
	private String name;
	private String pwd;
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getPwd() {
		return pwd;
	}
	public void setPwd(String pwd) {
		this.pwd = pwd;
	}
	public User() {
	}
	@Override
	public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pwd='" + pwd + '\'' +
                '}';
	} 
}
```

2、关联映射

- 编写映射接口UserMappe

  Mybatis框架中的映射接口类似于Hibernate中的Dao层接口，唯一不同的是，Mybatis中只需要声明接口即可，不需要实现

  ```java
  package com.xj.mybatis.pojo;
  
  public interface UserMapper {
  	public void insertUser(User user);
  	public User getUser(int id);
  }
  ```

- 编写对应的映射配置文件UserMapper.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
  	PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  	"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <!--namespace=绑定一个对应的Dao/Mapper接口 -->
  <mapper namespace="com.xj.mybatis.mapper.UserMapper">
  
  	<!--insert插入语句 -->
  	<insert id="insertUser" parameterType="com.xj.mybatis.pojo.User"> 
  		insert into mybatis.user(id,name,pwd) values (#{id},#{name},#{pwd})
  	</insert>
  	
  	<!-- 这里的id必须与接口中的方法名一致 -->
  	<select id="getUser" resultType="com.xj.mybatis.pojo.User" parameterType="int">
  		select * from mybatis.user where id = #{id}
  	</select>
  
  </mapper>
  ```

3、编写工具类

- 编写mybatis工具类

  ```java
  package com.xj.mybatis.util;
  
  import java.io.IOException;
  import java.io.InputStream;
  
  import org.apache.ibatis.io.Resources;
  import org.apache.ibatis.session.SqlSession;
  import org.apache.ibatis.session.SqlSessionFactory;
  import org.apache.ibatis.session.SqlSessionFactoryBuilder;
  
  public class MybatisUtil {
  	private static SqlSessionFactory sqlSessionFactory;
  	static {
  		try {
  			// 使用Mybatis第一步：获取sqlSessionFactory对象
  			String resource = "mybatis-config.xml";	// 加载配置文件
  			InputStream inputStream = Resources.getResourceAsStream(resource);
  			sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
  		} catch (IOException e) {
  			e.printStackTrace();
  		}
  	}
  	// 既然有了 SqlSessionFactory，顾名思义，我们就可以从中获得 SqlSession 的实例了。
  	// SqlSession 完全包含了面向数据库执行 SQL 命令所需的所有方法。
  	public static SqlSession getSqlSession() {
  		return sqlSessionFactory.openSession();
  	}
  }
  
  ```

  

### 2.4、编写测试代码

可以使用Junit进行测试，但感觉在eclipse中使用Junit查错真的不方便

- 编写demo测试代码

- 主要的两步是 获取和关闭，中间填写填写相应执行语句

  `SqlSession sqlSession = MybatisUtil.getSqlSession();`

  和`sqlSession.close();`

  ```java
  package com.xj.mybatis.demo;
  
  import org.apache.ibatis.session.SqlSession;
  import com.xj.mybatis.mapper.UserMapper;
  import com.xj.mybatis.pojo.User;
  import com.xj.mybatis.util.MybatisUtil;
  
  public class MybatisDemo {
  	public static void main(String[] args) {
          // 获取SqlSession
  		SqlSession sqlSession = MybatisUtil.getSqlSession();
  		//方式一：getMapper
  	    UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
          // 填写相应执行语句
  	    User rs = userMapper.getUser(1);
  	    System.out.println(rs);
          
  	    //关闭SqlSession
  	    sqlSession.close();
  	}
  }
  ```

### 2.5、Eclipse中的文件

- 如果出现找不到mybatis-config.xml的错误时，将文件放到源文件夹中即可。

![image-20200528181958478](.\mybatis.assets\image-20200528181958478.png)

- 在web项目中的文件

- 需要手动导入两个jar包

  ![image-20200528182009694](.\mybatis.assets\image-20200528182009694.png)

### 2.6、易错点

- UserMapper.xml文件中的配置

  在此代码中注意对应好 id（映射接口中的方法）以及 parameterType的拼写

  ```xml
  <!--insert插入语句 -->
  <insert id="insertUser" parameterType="com.xj.mybatis.pojo.User"> 
  	insert into mybatis.user(id,name,pwd) values (#{id},#{name},#{pwd})
  	<!-- 注意sql结尾不能加分号，否则报"ORA-00911"的错误 -->
  </insert>
  ```

- 以及标签的区分 <insert>

- 以及sql错误：` Table 'test.user' doesn't exist`

  解决方法：在user前面增加mybatis.  完整的数据表位置

  ```xml
   insert into mybatis.user(id,name,pwd) values (#{id},#{name},#{pwd})
  ```

### 2.7、总结：

#### 步骤：

1. 创建数据表
2. 导入maven依赖（或jar包）
3. 编写mybatis的核心配置文件
4. 编写实体类pojo
5. 编写映射接口mapper和映射文件mapper.xml
6. 编写工具类（获取SqlSession对象）
7. 编写测试代码

- 完成之后程序只需要修改三部分

## 3、CRUD

### 1、select

选择，查询语句;

- id : 就是对应的namespace中的方法名；
- resultType：Sql语句执行的返回值！
- parameterType ： 参数类型！

1、编写接口

```java
// 根据id查询用户
public User getUser(int id);
```

2、编写映射文件中对应的mapper中的sql语句

```xml
<!-- 这里的id必须与接口中的方法名一致 -->
<select id="getUser" resultType="com.xj.mybatis.pojo.User" parameterType="int">
	select * from mybatis.user where id = #{id}
	<!-- 注意sql结尾不能加分号，否则报"ORA-00911"的错误 -->
</select>
```

3、测试

```java
// 查询
public static void selectUser() {
    //方式一：getMapper
    UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
    User rs = userMapper.getUser(1);
    System.out.println(rs);
    //关闭SqlSession
    sqlSession.close();
}
```

### 2、insert

1. 编写接口

   ```java
   public int insertUser(User user);
   ```

2. 编写映射文件

   ```xml
   <!--insert插入语句 -->
   <insert id="insertUser" parameterType="com.xj.mybatis.pojo.User"> 
       insert into mybatis.user(id,name,pwd) values (#{id},#{name},#{pwd})
       <!-- 注意sql结尾不能加分号，否则报"ORA-00911"的错误 -->
   </insert>
   ```

3. 测试（必须提交事务，不然插入语句不会存在）

   ```java
   // 插入
   public static void insertUser() {
       // 获取mapper
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       // 执行插入语句
       int rs = mapper.insertUser(new User(4, "王五", "12345"));
       System.out.println(rs);
       // 提交事务
       sqlSession.commit();
       // 关闭SqlSession
       sqlSession.close();
   }
   ```

### 3、update

1. 编写接口

   ```java
   // 修改数据
   public int updataUser(User user);
   ```

2. 编写映射文件

   ```java
   <update id="updataUser" parameterType="com.xj.mybatis.pojo.User">
       update mybatis.user set name=#{name},pwd=#{pwd}  where id = #{id} 
   </update>
   ```

3. 测试（同样需要提交事务）

   ```java
   public static void updataUser() {
       // 获取mapper
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       // 执行修改语句
       int rs = mapper.updataUser(new User(2, "李二", "32141"));
       System.out.println(rs);
       // 提交事务
       sqlSession.commit();
       // 关闭SqlSession
       sqlSession.close();
   }
   ```

### 4、delect

```xml
<!-- 删除数据 -->
<delete id="deleteUser" parameterType="int">
    delete from mybatis.user where id = #{id} 
</delete>
```



### 5、万能Map

假设，我们的实体类，或者数据库中的表，字段或者参数过多，我们应当考虑使用Map！

```java
//万能的Map
int addUser2(Map<String,Object> map);
```

```xml
<!--对象中的属性，可以直接取出来    传递map的key-->
<insert id="addUser" parameterType="map">
    insert into mybatis.user (id, pwd) values (#{userid},#{passWord});
</insert>
```

```java
@Test
public void addUser2(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);

    Map<String, Object> map = new HashMap<String, Object>();
    map.put("userid",5);
    map.put("passWord","2222333");
    mapper.addUser2(map);
    sqlSession.close();
}
```



Map传递参数，直接在sql中取出key即可！    【parameterType="map"】

对象传递参数，直接在sql中取对象的属性即可！【parameterType="Object"】

只有一个基本类型参数的情况下，可以直接在sql中取到！

多个参数用Map，**或者注解！**

### 6、模糊匹配

Java代码执行的时候，传递通配符 % %

```sql
select * from mybatis.user where name like #{value}
```

```java
List<User> userList = mapper.getUserLike("%李%");
```



## 4、配置文件的设置

### 属性顺序：

- **properties**
- **settings**
- **typeAliases**
- **typeHandlers**
- **objectFactory**
- **objectWrapperFactory**
- **reflectorFactory**
- **plugins**
- **environments**
- **databaseIdProvider**
- **mappers**

### 1、属性的设置

- mybatis-config.xml中配置的数据库mysql的数值可以通过引用配置文件设置【db.properties】

  > 发现我这个版本的mysql必须指定下面这段url (MySQL 8.0)
  
  ```properties
  driver=com.mysql.jdbc.Driver
  url=jdbc:mysql://localhost:3306/mybatis?serverTimezone=GMT&createDatabaseIfNotExist=true&useUnicode=true&characterEncoding=utf-8&autoReconnect=true
  username=root
  password=1234
  ```

- 在配置文件中导入

  ```xml
  <!--引入外部配置文件 -->
  <properties resource="db.properties">
      <!-- 这里面也可以增加配置信息，当相同时，外部文件优先级更高 -->
      <property name="username" value="root"/>
      <property name="pwd" value="11111"/>
  </properties>
  ```

- 修改environments环境中的属性值`${driver}`

  ```xml
  <!-- default默认选择下面id为 development的环境 -->
  <environments default="development">
      <environment id="development">
          <transactionManager type="JDBC" />
          <dataSource type="POOLED">
              <!-- 连接数据库 -->
              <property name="driver" value="${driver}" />
              <property name="url" value="${url}" />
              <property name="username" value="${username}" />
              <property name="password" value="${password}" />
          </dataSource>
      </environment>
  </environments>
  ```
  
- **注意**：在配置文件中增加属性需要按照指定顺序：

  The content of element type "configuration" must match "(**properties?,settings?,typeAliases?,typeHandlers?,objectFactory?,objectWrapperFactory?,reflectorFactory?,plugins?,environments?,databaseIdProvider?,mappers?**)".

### 2、类别名（typeAliases）

- 为类型名起一个简短的名字

- 存在的意义仅在于用来减少类完全限定名的冗余。

  ```xml
  <!--可以给实体类起别名-->
  <typeAliases>
      <typeAlias type="com.xj.mybatis.pojo.User" alias="User"/>
  </typeAliases>
  ```

- 在映射文件mapper.xml中

  ```xml
  <!-- 这里的id必须与接口中的方法名一致 -->
  <select id="getUser" resultType="User">
      select * from mybatis.user
  </select>
  ```

  

  也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean，比如：

  扫描实体类的包，它的默认别名就为这个类的 类名，**建议首字母小写！**

  ```xml
  <!--可以给实体类起别名-->
  <typeAliases>
      <package name="com.xj.mybatis.pojo.User"/>
  </typeAliases>
  ```

  在实体类比较少的时候，使用第一种方式。

  **如果实体类十分多，建议使用第二种**。

  第一种可以DIY别名，第二种则·不行·，如果非要改，需要在实体上增加注解

  ```java
  @Alias("user")
  public class User {}
  ```


### 3、映射器（mappers）

MapperRegistry：注册绑定我们的Mapper文件；

方式一： 【推荐使用】

```xml
<!--每一个Mapper.XML都需要在Mybatis核心配置文件中注册！-->
<mappers>
    <mapper resource="com/xj/mybatis/dao/UserMapper.xml"/>
</mappers>
```

方式二：使用class文件绑定注册

```xml
<!--每一个Mapper.XML都需要在Mybatis核心配置文件中注册！-->
<mappers>
    <mapper class="com.xj.mybatis.mapper.UserMapper"/>
</mappers>
```

注意点：

- 接口和他的Mapper配置文件必须同名！
- 接口和他的Mapper配置文件必须在同一个包下！

方式三：使用扫描包进行注入绑定

```xml
<!--每一个Mapper.XML都需要在Mybatis核心配置文件中注册！-->
<mappers>
    <package name="com.xj.mybatis.mapper"/>
</mappers>
```

注意点：

- 接口和他的Mapper配置文件必须同名！
- 接口和他的Mapper配置文件必须在同一个包下！

### 4、生命周期

**SqlSessionFactoryBuilder：**

- 一旦创建了 SqlSessionFactory，就不再需要它了
- 局部变量

**SqlSessionFactory：**

- 说白了就是可以想象为 ：数据库连接池
- SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在，**没有任何理由丢弃它或重新创建另一个实例。** 
- 因此 SqlSessionFactory 的最佳作用域是应用作用域。 
- 最简单的就是使用**单例模式**或者静态单例模式。

**SqlSession**

- 连接到连接池的一个请求！
- SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域。
- 用完之后需要赶紧关闭，否则资源被占用！

## 5、结果集映射 resultMap

- 当实体类的属性名与数据表不一致时

  ```java
  public class User {
  	private int id;
  	private String name;
  	private String password;
  }
  ```

- 查询之后password的值为null（需要进行结果集映射）

- 在mapper.xml中进行配置的

  ```xml
  <!--结果集映射 -->
  <resultMap id="UserMap" type="user">
      <!--column数据库中的字段，property实体类中的属性 -->
      <!-- 数据库属性名 - 类属性名 -->
      <result column="id" property="id" />
      <result column="name" property="name" />
      <result column="pwd" property="password" />
  </resultMap>
  <!-- 注意使用的时resultMap -->
  <select id="getUser" resultMap="UserMap">
      select * from mybatis.user
  </select>
  ```

  

## 6、日志（setting）

### 1、日志工厂类型：

- SLF4J 
- LOG4J  【掌握】
- LOG4J2
- JDK_LOGGING
- COMMONS_LOGGING
- STDOUT_LOGGING   【掌握】
- NO_LOGGING

### 2、STDOUT_LOGGING标准日志输出

- 在mybatis核心配置文件中，配置我们的日志！

```xml
<settings>
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

![image-20200528182029771](.\mybatis.assets\image-20200528182029771.png)



### 3、Log4j的使用

1. 先导入log4j的包

   ```xml
   <!-- https://mvnrepository.com/artifact/log4j/log4j -->
   <dependency>
       <groupId>log4j</groupId>
       <artifactId>log4j</artifactId>
       <version>1.2.17</version>
   </dependency>
   ```

2. log4j.properties（可以控制输出类型和存放文件）

   ```properties
   #将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
   log4j.rootLogger=DEBUG,console,file
   
   #控制台输出的相关设置
   log4j.appender.console = org.apache.log4j.ConsoleAppender
   log4j.appender.console.Target = System.out
   log4j.appender.console.Threshold=DEBUG
   log4j.appender.console.layout = org.apache.log4j.PatternLayout
   log4j.appender.console.layout.ConversionPattern=[%c]-%m%n
   
   #文件输出的相关设置
   log4j.appender.file = org.apache.log4j.RollingFileAppender
   #设置日志的输出文件
   log4j.appender.file.File=./log/xj.log
   log4j.appender.file.MaxFileSize=10mb
   log4j.appender.file.Threshold=DEBUG
   log4j.appender.file.layout=org.apache.log4j.PatternLayout
   #可以修改时间格式
   log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n
   
   #日志输出级别
   log4j.logger.org.mybatis=DEBUG
   log4j.logger.java.sql=DEBUG
   log4j.logger.java.sql.Statement=DEBUG
   log4j.logger.java.sql.ResultSet=DEBUG
   log4j.logger.java.sql.PreparedStatement=DEBUG
   ```

3. 配置log4j为日志的实现

   ```xml
   <settings>
       <setting name="logImpl" value="log4j"/>
   </settings>
   ```

4. 输出结果

   ![image-20200528182040356](.\mybatis.assets\image-20200528182040356.png)

### 4、log4j的简单使用

1. 在要使用Log4j 的类中，导入包  import org.apache.log4j.Logger;

2. 日志对象，参数为当前类的class

   ```java
   static Logger logger = Logger.getLogger(UserDaoTest.class);
   ```

3. 日志级别

   ```java
   logger.info(rs);
   logger.debug(rs);
   logger.error(rs);
   ```

4. 输出结果

   在当前目录下产生./log/xj.log文件，里面存放控制台的所有输出信息

   ![image-20200528182050076](.\mybatis.assets\image-20200528182050076.png)



## 7、分页

- 使用新的项目编写（Mybatis_02）
  1. 导包
  2. 配置文件
  3. 编写实体类
  4. 编写接口和映射文件

### 7.1、使用Limit分页

```sql
语法：SELECT * from user limit startIndex,pageSize;
SELECT * from user limit 3;  #[0,n]
```



使用Mybatis实现分页，核心SQL

1. 接口

   ```java
   //分页
   List<User> getUserByLimit(Map<String,Integer> map);
   ```

2. Mapper.xml

   ```xml
   <!--结果集映射 -->
   <resultMap id="UserMap" type="user">
       <!--column数据库中的字段，property实体类中的属性 -->
       <result column="id" property="id" />
       <result column="name" property="name" />
       <result column="pwd" property="password" />
   </resultMap>
   <!-- 这里的id必须与接口中的方法名一致 -->
   <select id="getUserByLimit" resultMap="UserMap" parameterType="map">
       <!-- 分页查询语法：SELECT * from user limit startIndex,pageSize; -->
       select * from mybatis.user limit #{startIndex},#{pageSize}
   </select>
   ```

3. 测试

   ```java
   // 分页查询
   public static void selectUser() {
       // 方式一：getMapper
       UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
       Map<String,Integer> map = new HashMap<String, Integer>();
       map.put("startIndex", 0);
       map.put("pageSize", 2);
       
       List<User> rs = userMapper.getUserByLimit(map);
       for (User result : rs) {
           System.out.println(result);
       }
       // 关闭SqlSession
       sqlSession.close();
   }
   ```


### 7.2、RowBounds分页

1. 接口

   ```java
   //分页2
   List<User> getUserByRowBounds();
   ```

2. 映射文件

   ```xml
   <!--结果集映射 -->
   <resultMap id="UserMap" type="user">
       <!--column数据库中的字段，property实体类中的属性 -->
       <result column="id" property="id" />
       <result column="name" property="name" />
       <result column="pwd" property="password" />
   </resultMap>
   <!--分页2 -->
   <select id="getUserByRowBounds" resultMap="UserMap">
       select * from mybatis.user
   </select>
   ```

3. 测试

   ```java
   // 分页查询2
   public static void getUserByRowBounds() {
       //RowBounds实现（从0开始，查询2个数据）
       RowBounds rowBounds = new RowBounds(0, 2);
       //通过Java代码层面实现分页，不需要获取mapper了
       List<User> userList = sqlSession.
           selectList("com.xj.mybatis.mapper.UserMapper.getUserByRowBounds",null,rowBounds);
       // 输出结果
       for (User result : userList) {
           System.out.println(result);
       }
       // 关闭SqlSession
       sqlSession.close();
   }
   ```

- **注意**：使用RowBounds分页查询，不需要使用

  ```java
  UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
  ```

  而是使用

  ```java
  List<User> userList = sqlSession.
  selectList("com.xj.mybatis.mapper.UserMapper.getUserByRowBounds",null,rowBounds);
  ```

## 8、注解开发

-**面向接口开发**（不需要在配置映射文件mapper.xml）

\- **作用 :  ==解耦== , 可拓展 , 提高复用 , 分层开发中 , 上层不用管具体的实现 , 大家都遵守共同的标准 , 使得开发变得容易 , 规范性更好**

- **我们可以在工具类创建的时候实现自动提交事务！**

```java
public static SqlSession  getSqlSession(){
    return sqlSessionFactory.openSession(true);
}
```

### 8.1、select

1. 在接口中

   ```java
   // 注解开发
   @Select("select * from mybatis.user")
   List<User> getUsers();
   ```

2. 测试

   ```java
   // 使用注解查询
   public static void getUsers() {
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       List<User> list = mapper.getUsers();
       for(User user : list) {
           System.out.println(user);
       }
       sqlSession.close();
   }
   ```

   本质：反射机制实现

   底层：动态代理！

### 8.2、CRUD

- 其他的接口注解
- ==方法存在多个参数，所有的参数前面必须加上 @Param("id")注解==

```java
// 方法存在多个参数，所有的参数前面必须加上 @Param("id")注解
@Select("select * from mybatis.user where id = #{id}")
User getUserByID(@Param("id") int id);

@Insert("insert into mybatis.user(id,name,pwd) values (#{id},#{name},#		  	{password})")
int addUser(User user);

@Update("update mybatis.user set name=#{name},pwd=#{password} where id = #{id}")
int updateUser(User user);

@Delete("delete from mybatis.user where id = #{uid}")
int deleteUser(@Param("uid") int id);
```

**关于@Param() 注解**

- 基本类型的参数或者String类型，需要加上
- 引用类型不需要加
- 如果只有一个基本类型的话，可以忽略，但是建议大家都加上！
- 我们在SQL中引用的就是我们这里的 @Param() 中设定的属性名！

> **#{}     ${} 区别**：#{}比使用 ${}更加安全，作用一样

## 9、Lombok

- 使用注解来简化实体层的代码（set、get、构造方法等）

使用步骤：

1. **在项目中导入lombok的jar包**

   ```xml
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <version>1.18.10</version>
   </dependency>
   ```

2. **在编译器中安装Lombok插件！**

   - 运行Lombok包

   ![image-20200524170600801](.\mybatis.assets\image-20200524170600801.png)

   ![image-20200524170751035](.\mybatis.assets\image-20200524170751035.png)

   - 点击确定之后选择 Specify Location，选择eclipse的安装路径

   ![image-20200524170913594](.\mybatis.assets\image-20200524170913594.png)

   - 点击安装之后会发现目录下多了lombx.jar，此时重启eclipse即可

   ![image-20200524171009774](.\mybatis.assets\image-20200524171009774.png)

   - **注意：**如果出现无法重启eclipse的情况，请修改eclipse.ini中的配置路径（这里路径中的中文名被分隔开了）

   ![image-20200524171152198](.\mybatis.assets\image-20200524171152198.png)

   

3. 在实体类上加注解即可！

   ```java
   @Data ：get、set、无参构造方法等
   @AllArgsConstructor ：有参构造方法
   @NoArgsConstructor ：无参构造方法
   ```

4. 总的注解有：

   ```java
   @Getter and @Setter
   @FieldNameConstants
   @ToString
   @EqualsAndHashCode
   @AllArgsConstructor, @RequiredArgsConstructor and @NoArgsConstructor
   @Log, @Log4j, @Log4j2, @Slf4j, @XSlf4j, @CommonsLog, @JBossLog, @Flogger
   @Data
   @Builder
   @Singular
   @Delegate
   @Value
   @Accessors
   @Wither
   @SneakyThrows
   ```

说明：

```java
@Data：无参构造，get、set、tostring、hashcode，equals
@AllArgsConstructor
@NoArgsConstructor
@EqualsAndHashCode
@ToString
@Getter
```



## 10、多对一

- SQL（多条语句手动依次进行）

```sql
CREATE TABLE `teacher` (
  `id` INT(10) NOT NULL,
  `name` VARCHAR(30) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO teacher(`id`, `name`) VALUES (1, '卢晓江'); 

CREATE TABLE `student` (
  `id` INT(10) NOT NULL,
  `name` VARCHAR(30) DEFAULT NULL,
  `tid` INT(10) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `fktid` (`tid`),
  CONSTRAINT `fktid` FOREIGN KEY (`tid`) REFERENCES `teacher` (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8


INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('1', '小明', '1'); 
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('2', '小红', '1'); 
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('3', '小张', '1'); 
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('4', '小李', '1'); 
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('5', '小王', '1');

```

- 新建Maven项目Mybatis_03（第一个记得打勾）

  ![image-20200524185521741](.\mybatis.assets\image-20200524185521741.png)

- ### 测试环境搭建

  1. 导入lombok.jar（已安装好lombox插件）

  2. 新建实体类 Teacher，Student

  3. 建立Mapper接口

  4. 建立Mapper.XML文件

  5. 在核心配置文件中绑定注册我们的Mapper接口或者文件！【方式很多，随心选】

     以及类别名的设置

  6. 测试查询是否能够成功！

### 10.1、步骤：

### 按照结果嵌套处理

1. 编写实体类

   ```java
   @Data
   public class Student {
   	private int id;
   	private String name;
   	private Teacher teacher;
   	public String toString() {
   		return "id=" + id + ",name=" +
   				name + ",teacher=" + 
   				teacher.getName();
   	}
   }
   ```

2. 编写接口

   ```java
   public interface StudentMapper {
   	List<Student> getStudents();
   }
   ```

3. 编写配置文件

   ```xml
   <!--按照结果嵌套处理-->
   <select id="getStudents" resultMap="StudentTeacher">
       select 
       s.id sid,
       s.name sname,
       t.name tname
       from
       mybatis.student s,
       mybatis.teacher t
       where
       s.tid = t.id
   </select>
   <!-- 结果集映射 -->
   <!-- type中的类Student是使用类别名的 -->
   <resultMap type="Student" id="StudentTeacher">
       <result property="id" column="sid"/>
       <result property="name" column="sname"/>
       <!-- 关联一个 -->
       <association property="teacher" javaType="Teacher">
           <result property="name" column="tname"/>
       </association>
   </resultMap>
   ```

4. 测试

   ```java
   public class GetStudentsTest {
   	public static void main(String[] args) {
   		SqlSession sqlSession = MybatisUtil.getSqlSession();
   		StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
   		
   		List<Student> stus = mapper.getStudents();
   		for(Student stu : stus) {
   			System.out.println(stu);
   		}
   		sqlSession.close();
   	}
   }
   ```

- ![image-20200524185457446](.\mybatis.assets\image-20200524185457446.png)

- 文件图：

  ![image-20200524183325594](.\mybatis.assets\image-20200524183325594.png)



### 按照查询嵌套处理

- 修改映射文件（不需要编写长的SQL语句，但需要增加查询Teacher的select标签语句）

```xml
	<!-- 思路: 
	1. 查询所有的学生信息
	2. 根据查询出来的学生的tid，寻找对应的老师！ 子查询 -->
	<select id="getStudents2" resultMap="StudentTeacher2">
		select * from mybatis.student
	</select>

	<resultMap id="StudentTeacher2" type="Student">
		<result property="id" column="id" />
		<result property="name" column="name" />
		<!--复杂的属性，我们需要单独处理 对象： association 集合： collection -->
		<association property="teacher" column="tid"
			javaType="Teacher" select="getTeacher" />
	</resultMap>

	<select id="getTeacher" resultType="Teacher">
		select * from mybatis.teacher where id = #{id}
	</select>
```

- 结果

  ![image-20200524185422540](.\mybatis.assets\image-20200524185422540.png)



## 11、一对多

### 按结果嵌套查询

- 修改实体类

  ```java
  @Data
  public class Student {
  	private int id;
  	private String name;
  	private int tid;
  }
  ```

  ```java
  @Data
  public class Teacher {
  	private int id;
  	private String name;
      //一个老师拥有多个学生
      private List<Student> students;
  }
  ```

- 接口

  ```java
  List<Teacher> getTeacher(int id);
  ```

- 映射文件

  ```xml
  <!--按结果嵌套查询-->
  <select id="getTeacher" resultMap="TeacherStudent">
      select 
     		s.id sid,
      	s.name sname, 
      	t.name tname,
      	t.id tid
      from 
      	mybatis.student s,
      	mybatis.teacher t
      where 
      	s.tid = t.id 
      	and t.id = #{tid}
  </select>
  <resultMap id="TeacherStudent" type="Teacher">
      <result property="id" column="tid"/>
      <result property="name" column="tname"/>
      <!--复杂的属性，我们需要单独处理 
  			对象： association 
  			集合： collection
          javaType="" 指定属性的类型！
          集合中的泛型信息，我们使用ofType获取
          -->
      <collection property="students" ofType="Student">
          <result property="id" column="sid"/>
          <result property="name" column="sname"/>
          <result property="tid" column="tid"/>
      </collection>
  </resultMap>
  ```

- ![image-20200524191334549](.\mybatis.assets\image-20200524191334549.png)

### 按照查询嵌套处理

```xml
<select id="getTeacher2" resultMap="TeacherStudent2">
    select * from mybatis.teacher where id = #{tid}
</select>

<resultMap id="TeacherStudent2" type="Teacher">
    <collection property="students" javaType="ArrayList" ofType="Student" select="getStudentByTeacherId" column="id"/>
</resultMap>

<select id="getStudentByTeacherId" resultType="Student">
    select * from mybatis.student where tid = #{tid}
</select>
```



### 小结

1. 关联 - association   【多对一】
2. 集合 - collection   【一对多】
3. javaType    &   ofType
   1. JavaType  用来指定实体类中属性的类型
   2. ofType  用来指定映射到List或者集合中的 pojo类型，泛型中的约束类型！



注意点：

- 保证SQL的可读性，尽量保证通俗易懂
- 注意一对多和多对一中，属性名和字段的问题！
- 如果问题不好排查错误，可以使用日志 ， 建议使用 Log4j



## 12、动态SQL

> 从这里开始，使用IDEA进行开发

==**什么是动态SQL：动态SQL就是指根据不同的条件生成不同的SQL语句**==

### 搭建环境

```sql
CREATE TABLE `blog` (
  `id` varchar(50) NOT NULL COMMENT '博客id',
  `title` varchar(100) NOT NULL COMMENT '博客标题',
  `author` varchar(30) NOT NULL COMMENT '博客作者',
  `create_time` datetime NOT NULL COMMENT '创建时间',
  `views` int(30) NOT NULL COMMENT '浏览量'
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

创建一个基础工程

1. 导包

2. 编写配置文件

   开启驼峰命名设置==即从经典数据库列名 A_COLUMN 映射到经典 Java 属性名 aColumn。==

   ```xml
   <setting name="mapUnderscoreToCamelCase" value="true"/>
   ```

3. 编写实体类

   ```java
   @Data
   public class Blog {
       private int id;
       private String title;
       private String author;
       private Date createTime;
       private int views;
   }
   ```

4. 编写实体类对应Mapper接口 和 Mapper.XML文件

### IF

1. 接口中

   ```java
   // 使用IF动态查询
   List<Blog> queryBlogIF(Map map);
   ```

2. 映射文件中

   ```xml
   <select id="queryBlogIF" resultType="Blog" parameterType="map">
       select * from mybatis.blog
       <where>
           <if test="title != null">
               title = #{title}
           </if>
           <if test="author != null">
               and author = #{author}
           </if>
       </where>
   </select>
   ```

3. 测试

   ```java
   @Test
   public void queryBlogIF(){
       SqlSession sqlSession = MybatisUtil.getSqlSession();
       BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);
       HashMap map = new HashMap();
       map.put("title", "测试1");
       map.put("author", "晓江");
       
       List<Blog> blogs = mapper.queryBlogIF(map);
       for (Blog blog : blogs) {
           System.out.println(blog);
       }
       sqlSession.close();
   }
   ```

### where

- 在追加的语句上面自动添加where
- 并自动对**第一条语句进行删除and的操作（如果存在and）**
- 但**不会自动生成and**（即第二条语句需要加上and）

```xml
select * from mybatis.blog 
<where>
    <if test="title != null">
        title = #{title}
    </if>
    <if test="author != null">
        and author = #{author}
    </if>
</where>
```

### choose (when, otherwise)

```xml
<!--
    1、<choose> 配合<when>、<otherwise>使用
    2、相当于switch语句，只会检索出符合的一个条件
    3、但<when>中都不符合，检索出<otherwise>中的
-->
```

```xml
<select id="queryBlogChoose" parameterType="map" resultType="blog">
    select * from mybatis.blog
    <where>
        <choose>
            <when test="title != null">
                title = #{title}
            </when>
            <when test="author != null">
                and author = #{author}
            </when>
            <otherwise>
                and views = #{views}
            </otherwise>
        </choose>
    </where>
</select>
```

- 运行结果
![image-20200525170530327](.\mybatis.assets\image-20200525170530327.png)

### set

- ==用于执行动态修改语句==，

  前面的语句必须加 “ , ”，而最后的语句加上逗号也没问题

  ```xml
  <update id="updateBlog" parameterType="map">
      update mybatis.blog
      <set>
          <if test="title != null">
              title = #{title},
          </if>
          <if test="author != null">
              author = #{author}
          </if>
      </set>
      where id = #{id}
  </update>
  ```
 ### SQL片段

  ==有的时候，我们可能会将一些功能的部分抽取出来，方便复用！==

  1. 使用SQL标签抽取公共的部分

     ```xml
     <sql id="if-title-author">
         <if test="title != null">
             title = #{title}
         </if>
         <if test="author != null">
             and author = #{author}
         </if>
     </sql>
     ```

  2. 在需要使用的地方使用==<Include>==标签引用即可

     ```xml
     <select id="queryBlogIF" parameterType="map" resultType="blog">
         select * from mybatis.blog
         <where>
             <include refid="if-title-author"></include>
         </where>
     </select>
     ```




### Foreach

- sql语句（查询出前三个）

  ```sql
  select * from mybatis.blog where 1=1 and (id=1 or id = 2 or id=3)
  ```

- ==用法：传一个集合给他，他依次返回集合中的数值==

  1. item：返回值
  2. collection：传入的集合
  3. open：开始符号
  4. separator：分隔符
  5. close：结束符号

  ```xml
  select * from user where 1=1 and 
  <foreach item="id" collection="ids" open="(" separator="or" close=")">
      #{id}
  </foreach>
  ```

- 编写映射文件

```xml
<!--
        select * from mybatis.blog where 1=1 and (id=1 or id = 2 or id=3)
        collection：传入的集合        item：返回的数值
        我们现在传递一个万能的map ， 这map中可以存在一个集合，传入一个ids的集合！
-->
<select id="queryBlogForeach" parameterType="map" resultType="blog">
    select * from mybatis.blog
    <where>
        <foreach collection="ids" item="id" open="and (" close=")" separator="or">
            id = #{id}
        </foreach>
    </where>
</select>
```

- 测试

```java
@Test
public void queryBlogForeach(){
    SqlSession sqlSession = MybatisUtil.getSqlSession();
    BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);
    HashMap map = new HashMap();
    // 因为map要传递一个 ids 的值，是集合类型
    ArrayList<Integer> ids = new ArrayList<Integer>();
    ids.add(1);
    ids.add(2);
    ids.add(3);
    map.put("ids", ids);
    List<Blog> blogs = mapper.queryBlogForeach(map);
    sqlSession.close();
}
```

## 13、缓存 （了解即可）

### 13.1、简介

```
查询  ：  连接数据库 ，耗资源！
	一次查询的结果，给他暂存在一个可以直接取到的地方！--> 内存 ： 缓存
	
我们再次查询相同数据的时候，直接走缓存，就不用走数据库了
```

1. 什么是缓存 [ Cache ]？
   - 存在内存中的临时数据。
   - 将用户经常查询的数据放在缓存（内存）中，用户去查询数据就不用从磁盘上(关系型数据库数据文件)查询，**从缓存中查询**，从而提高查询效率，解决了高并发系统的性能问题。
2. 为什么使用缓存？

   - 减少和数据库的交互次数，减少系统开销，**提高系统效率。**
3. 什么样的数据能使用缓存？

   - **经常查询并且不经常改变的数据**。【可以使用缓存】

### 13.2、Mybatis缓存

- MyBatis包含一个非常强大的查询缓存特性，它可以非常方便地定制和配置缓存。缓存可以极大的提升查询效率。
- MyBatis系统中默认定义了两级缓存：**一级缓存**和**二级缓存**
  - 默认情况下，只有一级缓存开启。（SqlSession级别的缓存，也称为本地缓存）

  - 二级缓存需要手动开启和配置，他是基于namespace级别的缓存。

  - 为了提高扩展性，MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存

### 13.3、一级缓存（作用于SqlSession）

- 一级缓存也叫本地缓存：  SqlSession
  - 与数据库同一次会话期间查询到的数据会放在本地缓存中。
  - 以后如果需要获取相同的数据，直接从缓存中拿，没必须再去查询数据库；



测试步骤：

1. 开启日志！
2. 测试在一个Sesion中查询两次相同记录
3. 查看日志输出（查询相同数据使用缓存）

![1569983650437](.\mybatis.assets\1569983650437.png)



**缓存失效**的情况：

1. 查询**不同**的东西

2. **增删改**操作，可能会改变原来的数据，所以必定会刷新缓存！

   ![1569983952321](.\mybatis.assets\1569983952321.png)

3. 查询**不同的Mapper.xml**

4. 手动清理缓存！`sqlSession.clearCache`

   ![1569984008824](.\mybatis.assets\1569984008824.png)



小结：一级缓存默认是开启的，只在一次SqlSession中有效，也就是拿到连接到关闭连接这个区间段！

一级缓存就是一个Map。

### 13.4、二级缓存（作用于Mapper）

- 二级缓存也叫**全局缓存**，一级缓存作用域太低了，所以诞生了二级缓存
- 基于namespace级别的缓存，一个名称空间，对应一个二级缓存；
- 工作机制
  - 一个会话查询一条数据，这个数据就会被放在当前会话的一级缓存中；
  - 如果**当前会话关闭了**，这个会话对应的一级缓存就没了；但是我们想要的是，会话关闭了，一级缓存中的数据被**保存到二级缓存中**；
  - 新的会话查询信息，就可以从二级缓存中获取内容；
  - 不同的mapper查出的数据会放在自己对应的缓存（map）中；

步骤：

1. 开启全局缓存

   ```xml
   <!--在配置文件中显示的开启全局缓存-->
   <setting name="cacheEnabled" value="true"/>
   ```

2. 在要使用二级缓存的Mapper中开启

   ```xml
   <!--在当前Mapper.xml中使用二级缓存-->
   <cache/>
   ```

   也可以自定义参数（60秒）

   ```xml
   <!--在当前Mapper.xml中使用二级缓存-->
   <cache  eviction="FIFO"
          flushInterval="60000"
          size="512"
          readOnly="true"/>
   ```

3. 测试

   1. 问题:我们需要将实体类序列化！否则就会报错！

      ```
      Caused by: java.io.NotSerializableException: com.kuang.pojo.User
      ```

   2. 在实体类中实现**java.io.Serializable接口**



小结：

- 只要开启了二级缓存，在同一个Mapper下就有效
- 所有的数据都会先放在一级缓存中；
- 只有当会话提交，或者关闭的时候，才会提交到二级缓冲中！





### 13.5、缓存原理

1. 先查看二级缓存中有没有
2. 查看一级缓存有没有
3. 如果都没有就直接查询数据库

![1569985541106](.\mybatis.assets\1569985541106.png)





## 14、遇到的问题

### 1、MalformedByteSequenceException:3 字节的 UTF-8 序列的字节 3 无效。

- 解决方法：在pom.xml中的相应部分增加如下代码

  ```xml
  <build>
      <resources>
          <resource>
              <directory>src/main/resources</directory>
              <filtering>true</filtering>
          </resource>
      </resources>
  
      <plugins>
          <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-resources-plugin</artifactId>
              <configuration>
                  <encoding>UTF-8</encoding>
              </configuration>
          </plugin>
      </plugins>
  </build>
  ```




### 2、Caused by: java.io.NotSerializableException: com.xj.pojo.User

- 解决方法：在实体类中实现java.io.Serializable接口

  ```java
  public class Blog implements Serializable {
      private String id;
      private String title;
      private String author;
      private Date createTime;
      private int views;
  }
  ```




> **备注：本文学习资源来自b站：狂神说**





