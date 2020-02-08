## Mybatis

### 第一章 JDBC问题分析

1.  数据库链接创建、释放频繁造成系统资源浪费从而影响系统性能，如果使用数据库链接池可解决此问题
2. Sql 语句在代码中硬编码，造成代码不易维护，实际应用 sql 变化的可能较大， sql 变动需要改变 java
   代码
3.  使用 preparedStatement 向占有位符号传参数存在硬编码，因为 sql 语句的 where 条件不一定，可能
   多也可能少，修改 sql 还要修改代码，系统不易维护
4.  对结果集解析存在硬编码（查询列名 ), sql 变化导致解析代码变化，系统不易维护，如果能将数据库记
   录封装成 pojo 对象解析比较方便

### 第二章 Mybatis框架入门

#### 2.1 搭建Mybatis开发环境(基于xml)

1. ##### **创建**maven工程

2. ##### pom.xml添加相关坐标

   ```
   <dependency>
       <groupId>org.mybatis</groupId>
       <artifactId>mybatis</artifactId>
       <version>3.4.5</version>
   </dependency>
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>5.1.6</version>
   </dependency>
   <dependency>
       <groupId>log4j</groupId>
       <artifactId>log4j</artifactId>
       <version>1.2.12</version>
   </dependency>
   <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.10</version>
   </dependency>
   ```

3. ##### 编写实体类

   ```java
   public class User implements Serializable {
       private Integer id;
       private String username;
       private Date birthday;
       private String sex;
       private String address;
       
       ...
   }
   ```

4. ##### 编写持久层接口

   ```java
   public interface IUserDao {
       /**
        * 查询所有用户
        * @return
        */
       List<User> findAll();
   }
   ```

5. ##### 编写持久层接口的映射配置文件

   ​	<img src="C:\Users\wang1\Desktop\mdSource\mybatis\微信图片_20200114110203.png" style="zoom:67%;" />

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   
   <mapper namespace="com.wmxyyy.dao.IUserDao">
       <!-- 配置查询所有用户的方法-->
       <select id="findAll" resultType="com.wmxyyy.domain.User">
           select * from user
       </select>
   </mapper>
   ```

6. ##### 编写SqlMapConfig.xml配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   
   <!-- mybatis的主配置文件 -->
   <configuration>
       <!-- 配置环境-->
       <environments default="mysql">
           <!-- 配置mysql的环境 -->
           <environment id="mysql">
               <!-- 配置事务的类型 -->
               <transactionManager type="JDBC"></transactionManager>
               <!-- 配置数据源(连接池) -->
               <dataSource type="POOLED">
                   <!-- 配置连接数据库的4个基本信息-->
                   <property name="driver" value="com.mysql.jdbc.Driver"/>
                   <property name="url" value="jdbc:mysql://localhost:3306/eesy_mybatis"/>
                   <property name="username" value="root"/>
                   <property name="password" value="123456"/>
               </dataSource>
           </environment>
       </environments>
   
       <!-- 指定映射配置文件的位置，映射配置文件指的是每个dao独立的配置文件 -->
       <mappers>
           <mapper resource="com/wmxyyy/dao/IUserDao.xml"/>
       </mappers>
   </configuration>
   ```

7. 编写测试类

   ```java
   public class MybatisTest {
       public static void main(String[] args) throws Exception {
           //1.读取配置文件
           InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
           //2.创建SqlSessionFactory工厂
           SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
           SqlSessionFactory factory = builder.build(in);
           //3.使用工厂生产SqlSession对象
           SqlSession sqlSession = factory.openSession();
           //4.使用SqlSession对象创建Dao接口的代理对象
           IUserDao userDao = sqlSession.getMapper(IUserDao.class);
           //5.使用代理对象执行方法
           List<User> users = userDao.findAll();
           for (User user : users){
               System.out.println(user);
           }
           //6.释放资源
           sqlSession.close();
           in.close();
       }
   }
   ```



#### 2.2 搭建Mybatis开发环境(基于注解)

1. ##### 在持久层中添加注解， 删除IUserDao.xml

   ```java
   public interface IUserDao {
       /**
        * 查询所有用户
        * @return
        */
       @Select("select * from user ")
       List<User> findAll();
   }
   ```

2. ##### 修改SqlMapConfig.xml

   ```xml
       <!-- 指定映射配置文件的位置，映射配置文件指的是每个dao独立的配置文件 -->
       <!-- 如果使用注解配置的话，此处应该使用class属性指定被注解的dao全限定类名-->
       <mappers>
           <!--<mapper resource="com/wmxyyy/dao/IUserDao.xml"/>-->
           <mapper class="com.wmxyyy.dao.IUserDao"></mapper>
       </mappers>
   ```

   

### 第三章 自定义Mybatis框架

#### 3.1 基于XML的自定义Mybatis框架

#### 3.2 基于注解的自定义Mybatis框架

#### 3.3 自定义Mybatis的设计模式说明

- 工厂模式

- 代理模式

- 构建者模式

  

### 第四章 基于代理Dao实现CRUD操作

##### 4.1 持久层接口

```java
public interface IUserDao {
    /**
     * 查询所有用户
     * @return
     */
    List<User> findAll();
    /**
     * 保存用户
     * @param user
     */
    void saveUser(User user);

    /**
     * 更新用户
     * @param user
     */
    void updateUser(User user);

    /**
     * 根据Id删除用户
     * @param userId
     */
    void deleteUser(Integer userId);

    /**
     * 根据Id查询用户信息
     * @param userId
     */
    User findById(Integer userId);

    /**
     * 根据名称模糊查询用户信息
     * @param username
     */
    List<User> findByName(String username);

    /**
     * 查询总用户数
     * @return
     */
    int findTotal();
}
```

##### 4.2 持久层接口的映射配置文件

> 持久层接口和持久层接口的映射配置必须在相同的包下
>
> 持久层映射配置中 mapper 标签的 namespace 属性取值必须是持久层接口的全限定类名
>
> SQL 语句的配置标签<select>,<insert>,<delete>,<update>的 id 属性必须和持久层接口的方法名相同
>
> resultTyle属性：指定的结果集类型
>
> parameterType属性：指定传入参数类型
>
> #{ } ：它代表占位符，#{ } 它用的是 ognl 表达式。 相当于原来 jdbc的 ?，都是用于执行语句时替换实际的数据。
>
> ognl 表达式：
> 它是 apache 提供的一种表达式语言， 全称是：Object Graphic Navigation Language 对象图导航语言。它是按照一定的语法格式来获取数据的。语法格式就是使用 #{对象.对象}的方式 
> 
> #{user.username}它会先去找 user 对象，然后在 user 对象中找到 username 属性，并调用
> getUsername()方法把值取出来。但是我们在 parameterType 属性上指定了实体类名称，所以可以省略 user，而直接写 username 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
 
<mapper namespace="com.wmxyyy.dao.IUserDao">
    <!--查询所有用户-->
    <select id="findAll" resultType="com.wmxyyy.domain.User">
        select * from user;
    </select>
	
    <!--添加用户-->
    <insert id="saveUser" parameterType="com.wmxyyy.domain.User">
        insert into user(username,address,sex,birthday) value (#{username},#{address},#{sex},#{birthday});    
        <!--配置插入操作后，获取插入数据的id-->
        <!-- 新增用户后，同时还要返回当前新增用户的 id 值，因为 id 是由数据库的自动增长来实		现的，所以就相当于我们要在新增后将自动增长 auto_increment 的值返回
		-->
        <selectKey keyProperty="id" keyColumn="id" resultType="int" order="AFTER">
            select last_insert_id();
        </selectKey>
    </insert>

    <!--更新用户-->
    <update id="updateUser" parameterType="com.wmxyyy.domain.User">
        update user set username=#{username},address=#{address},sex=#{sex},birthday=#{birthday} where id=#{id};
    </update>

    <!--根据id删除用户-->
    <delete id="deleteUser" parameterType="Integer">
        delete from user where id=#{uid};
    </delete>

    <!--根据id查询用户-->
    <select id="findById" parameterType="INT" resultType="com.wmxyyy.domain.User">
        select * from USER where id = #{uid};
    </select>

    <!--根据名称模糊查询用户信息-->
    <select id="findByName" parameterType="String" resultType="com.wmxyyy.domain.User">
        select * from user where username like #{name};
        <!--select * from user where username like '%${value}%';-->
    </select>

    <!--获取用户的总记录条数-->
    <select id="findTotal" resultType="int">
        select count(id) from user;
    </select>
</mapper>
```

> #{ }与 ${ }
>
> 1. #{}表示一个占位符号
>    通过#{}可以实现 preparedStatement 向占位符中设置值，自动进行 java 类型和 jdbc 类型转换，#{}可以有效防止 sql 注入。 #{}可以接收简单类型值或 pojo 属性值。 如果 parameterType 传输单个简单类型值， #{}括号中可以是 value 或其它名称。
>
> 2. ${}表示拼接 sql 串
>    通过${}可以将 parameterType 传入的内容拼接在 sql 中且不进行 jdbc 类型转换， ${}可以接收简单类型值或 pojo 属性值，如果 parameterType 传输单个简单类型值， ${}括号中只能是 value。  

##### 4.3 测试方法

```java
public class MybatisTest {
    private InputStream is;
    private SqlSessionFactory factory;
    private SqlSession sqlSession;
    private IUserDao userDao;

    @Before
    public void init() throws IOException {
        //1.读取配置文件，生成字节输入流
        is = Resources.getResourceAsStream("SqlMapConfig.xml");
        //2.获取SqlSessionFactory
        factory =  new SqlSessionFactoryBuilder().build(is);
        //3.获取SqlSession对象
        sqlSession = factory.openSession();
        //4.获取dao的代理对象
        userDao = sqlSession.getMapper(IUserDao.class);
    }
    @After
    public void destroy() throws Exception{
        //提交事务
        sqlSession.commit();
        //关闭资源
        sqlSession.close();
        is.close();
    }
    
    @Test
    ...
}
```



### 第五章 Mybatis和JDBC的比较

| JDBC                                                         | Mybatis                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 数据库链接创建、释放频繁造成系统资源浪费从而影响系统性能，如果使用数据库链接池可解决此问题 | 在 SqlMapConfig.xml 中配置数据链接池，使用连接池管理数据库链接 |
| sql 语句写在代码中造成代码不易维护，实际应用 sql 变化的可能较大， sql 变动需要改变 java 代码 | 将 Sql 语句配置在 XXXXmapper.xml 文件中与 java 代码分离      |
| 向 sql 语句传参数麻烦，因为 sql 语句的 where 条件不一定，可能多也可能少，占位符需要和参数对应 | Mybatis 自动将 java 对象映射至 sql 语句，通过 statement 中的 parameterType 定义输入参数的类型 |
| 对结果集解析麻烦， sql 变化导致解析代码变化，且解析前需要遍历，如果能将数据库记录封装成 pojo 对象解析比较方便 | Mybatis 自动将 sql 执行结果映射至 java 对象，通过 statement 中的 resultType 定义输出结果的类型 |

### 第六章 Mybatis的参数深入

#### 6.1 parameterType 配置参数 

parameterType属性的取值可以是基本类型，引用类型（例如:String 类型），还可以是实体类类型（POJO 类）。同时也可以使用实体类的包装类

> 基 本 类 型 和 String 我 们 可 以 直 接 写 类 型 名 称 ， 也 可 以 使 用 包 名 . 类 名 的 方 式 ， 例 如 ：java.lang.String。 
>
> 实体类类型，目前我们只能使用全限定类名。
> 究其原因，是 mybaits 在加载时已经把常用的数据类型注册了别名，从而我们在使用时可以不写包名，而我们的是实体类并没有注册别名，所以必须写全限定类名。

#### 6.2 传递pojo包装对象

开发中通过 pojo 传递查询条件 ，查询条件是综合的查询条件，不仅包括用户查询条件还包括其它的查
询条件（比如将用户购买商品信息也作为查询条件），这时可以使用包装对象传递输入参数 

1. ##### 需求：根据用户名查询用户信息，查询条件放到 QueryVo 的 user 属性中 

2. ##### 编写QueryVo

   ```java
   public class QueryVo {
       private User user;
   
       public User getUser() {
           return user;
       }
   
       public void setUser(User user) {
           this.user = user;
       }
   }
   ```

3. ##### 持久层接口

   ```java
      	 /**
         * 根据queryVo中的条件查询用户
         * @param vo
         * @return
         */
       List<User> findUserByVo(QueryVo vo);
   ```

4. 持久层接口的映射配置文件

   ```xml
   <!--根据queryVo的条件查询用户-->
       <select id="findUserByVo" parameterType="com.wmxyyy.domain.QueryVo" resultType="com.wmxyyy.domain.User">
           select * from user where username like #{user.username};
           <!--select * from user where username like '%${value}%';-->
       </select>
   ```

5. 测试包装类作为参数

   ```java
   	/**
        * 使用QueryVo作为查询条件
        */
       @Test
       public void testFindByVo(){
           QueryVo vo = new QueryVo();
           User user = new User();
           user.setUsername("%王%");
           vo.setUser(user);
           List<User> users = userDao.findUserByVo(vo);
           for (User u : users){
               System.out.println(u);
           }
       }
   ```

   

### 第七章 Mybatis 的输出结果封装 

#### 7.1 resultType配置结果类型

- resultType 属性可以指定结果集的类型，它支持基本类型和实体类类型。 

- 它和 parameterType 一样，如果注册过类型别名的，可以直接使用别名。没有注册过的必须
  使用全限定类名。例如：我们的实体类此时必须是全限定类名（后面会讲解如何配置实体类的别名）
- 实体类中的属性名称必须和查询语句中的列名保持一致，否则无法实现封装 

#### 7.2 基本类型实例



#### 7.3 实体类类型实例



#### 7.4 特殊情况实例

1. ##### 实体类

   ```java
   //实体类属性和数据库表的列名不一致
   public class User implements Serializable {
       private Integer userId;
       private String userName;
       private Date userBirthday;
       private String userSex;
       private String userAddress;
   
       ...
   }
   ```

2. ##### 持久层接口

   ```java
   /**
   * 查询所有用户
   * @return
   */
   List<User> findAll();
   ```

3. ##### 持久层接口映射配置

   ```xml
   <!-- 配置查询所有操作 -->
   <select id="findAll" resultType="com.itheima.domain.User">
   	select * from user
   </select>
   ```

4. ##### 查询结果

   null值

5. ##### 修改映射配置

   ```xml
   <!--使用别名查询-->
   <!-- 配置查询所有操作 -->
   <select id="findAll" resultType="com.itheima.domain.User">
   	select id as userId,username as userName,birthday as userBirthday,
   sex as userSex,address as userAddress from user
   </select>
   ```

   > 如果我们的查询很多，都使用别名的话写起来岂不是很麻烦，有没有别的解决办法呢？ 

#### 7.5 resultMap 结果类型

resultMap 标签可以建立查询的列名和实体类的属性名称不一致时建立对应关系。从而实现封装。
在 select 标签中使用 resultMap 属性指定引用即可。

同时 resultMap 可以实现将查询结果映射为复杂类型的 pojo，比如在查询结果映射对象中包括 pojo 和 list 实现一对一查询和一对多查询。 

1. 持久层接口映射配置文件

   ```xml
   <!--建立 User 实体和数据库表的对应关系
   	type 属性：指定实体类的全限定类名
   	id 属性：给定一个唯一标识，是给查询 select 标签引用用的。
   -->
   <resultMap type="com.itheima.domain.User" id="userMap">
       <id column="id" property="userId"/> 		    
       <result column="username" property="userName"/> 
       <result column="sex" property="userSex"/> 		
       <result column="address" property="userAddress"/>
       <result column="birthday" property="userBirthday"/>
   </resultMap>
   
   -- id标签:		指定主键字段
   -- result标签:	指定非主键字段
   -- column属性:	指定数据库列名
   -- property属性:	指定实体类名称
   
   <!-- 配置查询所有操作 -->
   <select id="findAll" resultType="com.itheima.domain.User">
   	select * from user
   </select>
   ```

2. 测试结果

   正常
   
   

### 第八章 Mybatis传统DAO层开发（了解）



### 第九章 SqlMapConfig.xml配置文件

#### 9.1 配置内容和顺序

```xml
-properties（属性）
	--property
	
-settings（全局配置参数）
	--setting
	
-typeAliases（类型别名）
	--typeAliase
	
--package
-typeHandlers（类型处理器）
-objectFactory（对象工厂）
-plugins（插件）

-environments（环境集合属性对象）
	--environment（环境子属性对象）
		---transactionManager（事务管理）
		---dataSource（数据源）
		
		
-mappers（映射器）
	--mapper
	--package
```

#### 9.2 properties（属性）

- ```xml
  <!--第一种方式-->
  <properties>
      <property name="jdbc.driver" value="com.mysql.jdbc.Driver"/>
      <property name="jdbc.url" value="jdbc:mysql://localhost:3306/eesy"/>
      <property name="jdbc.username" value="root"/>
      <property name="jdbc.password" value="123456"/>
  </properties>
  ```

- ```xml
  <!--第二种方式-->
  <!-- 配置properties-->
      <properties resource="jdbcConfig.properties"></properties>
  
  <!--配置环境-->
      <environments default="mysql">
          <!--配置mysql的环境-->
          <environment id="mysql">
              <!--配置事务-->
              <transactionManager type="JDBC"></transactionManager>
              <!--配置连接池-->
              <dataSource type="POOLED">
                  <property name="driver" value="${jdbc.driver}"></property>
                  <property name="url" value="${jdbc.url}"></property>
                  <property name="username" value="${jdbc.username}"></property>
                  <property name="password" value="${jdbc.password}"></property>
              </dataSource>
          </environment>
      </environments>
  ```

- ```properties
    jdbc.driver=com.mysql.jdbc.Driver
    jdbc.url=jdbc:mysql://localhost:3306/eesy_mybatis?characterEncoding=utf-8
    jdbc.username=root
    jdbc.password=123456
  ```

#### 9.3 typeAliases（类型别名）

```xml
<typeAliases>
	<!-- 单个别名定义 -->
	<typeAlias alias="user" type="com.itheima.domain.User"/>
	
</typeAliases>
```

```xml
<typeAliases>
    <!-- 批量别名定义，扫描整个包下的类，别名为类名（首字母大写或小写都可以） -->
    <package name="com.wmxyyy.domain"></package>
</typeAliases>
```

#### 9.4 mappers（映射器）

```xml
<!--配置映射文件的位置-->
<mappers>
    <!-- <mapper resource="com/wmxyyy/dao/IUserDao.xml"></mapper> -->
    <package name="com.wmxyyy.dao"></package>
</mappers>
```

> 1. <mapper resource=" " />  使用相对于类路径的资源
>    如： <mapper resource="com/wmxyyy/dao/IUserDao.xml" /> 
>
> 2. <mapper class=" " /> 使用 mapper 接口类路径
>    如： <mapper class="com.itheima.dao.UserDao"/>
>
> 3. <package name=""/> 注册指定包下的所有 mapper 接口 
>
>    如：<package name="com.wmxyyy.dao"></package>

### 第十章 Mybatis 连接池与事务深入 

#### 10.1 Mybatis 的连接池技术 

在 Mybatis 中也有连接池技术但是它采用的是自己的连接池技术。在 Mybatis 的 SqlMapConfig.xml 配置文件中， 通过<dataSource type="pooled">来实现 Mybatis 中连接池的配置 

1. ##### Mybatis 连接池的分类 

   - UNPOOLED 不使用连接池的数据源
   - POOLED 使用连接池的数据源
   - JNDI 使用 JNDI 实现的数据源 

2. ##### Mybatis 中数据源的配置 

   ```xml
   <!-- 配置数据源（连接池）信息 -->
   <dataSource type="POOLED">
       <property name="driver" value="${jdbc.driver}"/>
       <property name="url" value="${jdbc.url}"/>
       <property name="username" value="${jdbc.username}"/>
       <property name="password" value="${jdbc.password}"/>
   </dataSource>
   <!--MyBatis 在初始化时， 根据<dataSource>的 type 属性来创建相应类型的的数据源 		  DataSource
   	type=”POOLED”： MyBatis 会创建 PooledDataSource 实例
   	type=”UNPOOLED” ： MyBatis 会创建 UnpooledDataSource 实例
   	type=”JNDI”： MyBatis 会从 JNDI 服务上查找 DataSource 实例，然后返回使用
   -->
   ```

3. ##### Mybatis 中 DataSource 的存取 

   MyBatis 是 通 过 工 厂 模 式 来 创 建 数 据 源 DataSource 对 象 的 ， MyBatis 定 义 了 抽 象 的 工 厂 接口: **org.apache.ibatis.datasource.DataSourceFactory**，通过其 getDataSource()方法返回数据源DataSource。

4. ##### Mybatis 中连接的获取过程分析 

   当我们需要创建 SqlSession 对象并需要执行 SQL 语句时，这时候 MyBatis 才会去调用 dataSource 对象来创建 **java.sql.Connection**对象。也就是说， **java.sql.Connection**对象的创建一直延迟到执行 SQL语句的时候。 

### 第十一章 Mybatis 的事务控制 

#### 11.1 JDBC 中事务的回顾 

#### 11.2 Mybatis 中事务提交方式 

#### 11.3 Mybatis 自动提交事务的设置 



### 第十二章 Mybatis 的动态 SQL 语句 

#### 12.1 if 标签

```xml
<!--根据条件查询-->
<select id="findUserByCondition" parameterType="User" resultType="User">
        select * from user where 1=1
        <if test="username != null">
            and username = #{username}
        </if>
        <if test="sex != null">
            and sex = #{sex}
        </if>
</select> 
```

#### 12.2 where标签

```xml
<select id="findUserByCondition" parameterType="User" resultType="User">
        select * from user
        <where>
            <if test="username != null">
                and username = #{username}
            </if>
            <if test="sex != null">
                and sex = #{sex}
            </if>
        </where>
    </select>
```

#### 12.3 foreach标签

```xml
<!-- 根据queryvo中的Id集合实现查询用户列表 -->
    <select id="findUserInIds" parameterType="queryvo" resultType="User">
        select * from user
        <where>
            <if test="ids != null and ids.size() > 0">
                <foreach collection="ids" open="and id in (" close=")" item="uid" separator=",">
                    #{uid}
                </foreach>
            </if>
        </where>
    </select>
```

> SQL 语句：select 字段 from user where id in (?)
>
> <foreach>标签用于遍历集合，它的属性：
> 	collection:代表要遍历的集合元素，注意编写时不要写#{}
> 	open:代表语句的开始部分
> 	close:代表结束部分 
>
> ​	item:代表遍历集合的每个元素，生成的变量名
> ​	sperator:代表分隔符 



### 第十三章 Mybatis 多表查询之一对多 

用户为 User 表，账户为 Account表。一个用户（User）可以有多个账户（Account） 

#### 13.1 查询所有账户信息，关联查询下单用户信息 

1. ##### 实体类

   ```java
   public class User implements Serializable {
       private Integer id;
       private String username;
       private String address;
       private String sex;
       private Date birthday;
   
       ...
   }
   
   public class Account implements Serializable {
       private Integer id;
       private Integer uid;
       private Double money;
   
       private User user;//从表实体类应该包含一个主表的实体类对象的引用
       
       ...
   }
   ```

2. ##### 账户接口的映射配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    
   <mapper namespace="com.wmxyyy.dao.IAccountDao">
       <!-- 定义封装account和user的resultMap -->
       <resultMap id="accountUserMap" type="account">
           <id property="id" column="aid"></id>
           <result property="uid" column="uid"></result>
           <result property="money" column="money"></result>
           <!-- 一对一的关系映射：配置封装user的内容-->
           <association property="user" column="uid" javaType="user">
               <id property="id" column="id"></id>
               <result property="username" column="username"></result>
               <result property="address" column="address"></result>
               <result property="sex" column="sex"></result>
               <result property="birthday" column="birthday"></result>
           </association>
       </resultMap>
   
       <!--查询所有账户-->
       <select id="findAll" resultMap="accountUserMap">
           <!--select * from account;-->
           SELECT u.*,
                  a.id as aid,
                  a.uid,
                  a.money
           FROM account a,
                user u
           WHERE
               a.uid = u.id
       </select>
       <!--根据id查询账户-->
       <select id="findById" parameterType="INT" resultType="Account">
           select *
           from account
           where id = #{aid};
       </select>
   </mapper>
   ```

#### 13.2 查询所有用户信息及用户关联的账户信息 

1. ##### 实体类

   ```java
   public class User implements Serializable {
       private Integer id;
       private String username;
       private String address;
       private String sex;
       private Date birthday;
      
       private List<Account> accounts;//主表实体应该包含从表实体的集合引用
       ...
   }
   
   public class Account implements Serializable {
       private Integer id;
       private Integer uid;
       private Double money;
   
       private User user;//从表实体类应该包含一个主表的实体类对象的引用
       
       ...
   }
   ```

2. ##### 用户接口的映射配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    
   <mapper namespace="com.wmxyyy.dao.IUserDao">
       <!-- 定义User的resultMap-->
       <resultMap id="userAccountMap" type="user">
           <id property="id" column="id"></id>
           <result property="username" column="username"></result>
           <result property="address" column="address"></result>
           <result property="sex" column="sex"></result>
           <result property="birthday" column="birthday"></result>
           <!--配置user对象中accounts集合的映射-->
           <collection property="accounts" ofType="account">
               <id property="id" column="aid"></id>
               <result property="uid" column="uid"></result>
               <result property="money" column="money"></result>
           </collection>
       </resultMap>
       <!--查询所有用户-->
       <select id="findAll" resultMap="userAccountMap">
           <!-- select * from user;-->
           SELECT *
           FROM
               user u
           LEFT JOIN
               account a
           ON
               u.id = a.uid;
       </select>
   
       <!--根据id查询用户-->
       <select id="findById" parameterType="INT" resultType="User">
           select * from USER where id = #{uid};
       </select>
   </mapper>
   ```

   

### 第十四章 Mybatis 多表查询之多对多 

用户为 user 表，角色为 role表。一个用户（User）可以有多个角色（Role），一个角色（Role）可以有多个用户 （User）

```java
public class Role implements Serializable {
    private Integer roleId;
    private String roleName;
    private String roleDesc;
    
    private List<User> users;//一个角色可以赋予多个用户
    ...
}
public class User implements Serializable {
    private Integer id;
    private String username;
    private String address;
    private String sex;
    private Date birthday;

    private List<Role> roles;//一个用户可以具备多个角色
    ...
}
```

#### 14.1 实现 Role 到 User 多对多关系

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
 
<mapper namespace="com.wmxyyy.dao.IRoleDao">
    <!-- 定义Role的resultMap-->
    <resultMap id="roleMap" type="role">
        <id property="roleId" column="rid"></id>
        <result property="roleName" column="role_name"></result>
        <result property="roleDesc" column="role_desc"></result>
       
        <collection property="users" ofType="user">
            <id property="id" column="id"></id>
            <result property="username" column="username"></result>
            <result property="address" column="address"></result>
            <result property="sex" column="sex"></result>
            <result property="birthday" column="birthday"></result>
        </collection>
    </resultMap>

    <!--查询所有角色，包括每个角色下的所有用户-->
    <select id="findAll" resultMap="roleMap">
        SELECT
            u.*,
			r.id as rid,
			r.ROLE_NAME,
			r.ROLE_DESC
        FROM
		    role r
        LEFT JOIN
		    user_role ur
        ON
		    r.id = ur.rid
        LEFT JOIN
		    user u
        ON
		    ur.uid = u.id
    </select>

</mapper>
```



#### 14.2 实现User到Role多对多关系

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
 
<mapper namespace="com.wmxyyy.dao.IUserDao">

    <resultMap id="userMap" type="user">
        <id property="id" column="id"></id>
        <result property="username" column="username"></result>
        <result property="address" column="address"></result>
        <result property="sex" column="sex"></result>
        <result property="birthday" column="birthday"></result>

        <collection property="roles" ofType="role">
            <id property="roleId" column="rid"></id>
            <result property="roleName" column="role_name"></result>
            <result property="roleDesc" column="role_desc"></result>
        </collection>
    </resultMap>
    <!--查询所有用户，包括角色信息-->
    <select id="findAll" resultMap="userMap">
        SELECT
            u.*,
            r.id as rid,
            r.ROLE_NAME,
            r.ROLE_DESC
        FROM
            user u
        LEFT JOIN
            user_role ur
        ON
            u.id = ur.uid

        LEFT JOIN
            role r
        ON
            ur.rid = r.id
    </select>

    <!--根据id查询用户-->
    <select id="findById" parameterType="INT" resultType="User">
        select * from USER where id = #{uid};
    </select>
</mapper>
```

### 第十五章 Mybatis 延迟加载策略 

#### 15.1 延迟加载

就是在需要用到数据时才进行加载，不需要用到数据时就不加载数据。延迟加载也称懒加载.
**好处：** 先从单表查询，需要时再从关联表去关联查询，大大提高数据库性能，因为查询单表要比关联查询多张表速度要快。 

**坏处：**因为只有当需要用到数据时，才会进行数据库查询，这样在大批量数据查询时，因为查询工作也要消耗时间，所以可能造成用户等待时间变长，造成用户体验下降。 

#### 15.2 使用 assocation 和  collection  实现延迟加载 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
 
<mapper namespace="com.wmxyyy.dao.IAccountDao">
    <!-- 定义封装account和user的resultMap -->
    <resultMap id="accountUserMap" type="account">
        <id property="id" column="id"></id>
        <result property="uid" column="uid"></result>
        <result property="money" column="money"></result>
        <!-- 一对一的关系映射：配置封装user的内容
             select属性指定的内容:查询用户的唯一标识
             column属性指定的内容:用户根据id查询时所需要的参数的值
        -->
        <association property="user" column="uid" javaType="user" select="com.wmxyyy.dao.IUserDao.findById">
        </association>
    </resultMap>

    <!--查询所有账户-->
    <select id="findAll" resultMap="accountUserMap">
        select * from account;

    </select>

    <!--根据id查询账户-->
    <select id="findById" parameterType="INT" resultType="Account">
        select *
        from account
        where id = #{aid};
    </select>

</mapper>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <!-- 配置properties-->
    <properties resource="jdbcConfig.properties"></properties>
    <settings>
        <setting name="lazyLoadingEnabled" value="true"/>
        <setting name="aggressiveLazyLoading" value="false"></setting>
    </settings>

    <!--使用typeAliases配置别名，它只能配置domain中类的别名 -->
    <typeAliases>
        <package name="com.wmxyyy.domain"></package>
    </typeAliases>

    <!--配置环境-->
    <environments default="mysql">
        <!--配置mysql的环境-->
        <environment id="mysql">
            <!--配置事务-->
            <transactionManager type="JDBC"></transactionManager>
            <!--配置连接池-->
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"></property>
                <property name="url" value="${jdbc.url}"></property>
                <property name="username" value="${jdbc.username}"></property>
                <property name="password" value="${jdbc.password}"></property>
            </dataSource>
        </environment>
    </environments>

    <!--配置映射文件的位置-->
    <mappers>
        <package name="com.wmxyyy.dao"></package>
    </mappers>
</configuration>
```

```java
	/**
     * 测试查询所有账户,包括所属账户
     */
    @Test
    public void testFindAll(){
        List<Account> accounts = accountDao.findAll();
        for (Account account : accounts){
            System.out.println(account);
            System.out.println(account.getUser());
        }
    }
```

![](C:\Users\wang1\Desktop\mdSource\mybatis\Snipaste_2020-01-14_20-48-50.png)

------

​	<img src="C:\Users\wang1\Desktop\mdSource\mybatis\Snipaste_2020-01-14_20-55-50.png" style="zoom: 67%;" />

### 十六章 Mybatis缓存

#### 16.1 Mybatis 一级缓存 

一级缓存是 SqlSession 范围的缓存，默认开启。当调用 SqlSession 的修改，添加，删除， commit()， close()等方法时，就会清空一级缓存。  

如果 sqlSession 去执行 commit 操作（执行插入、更新、删除），清空 SqlSession 中的一级缓存，这样做的目的为了让缓存中存储的是最新的信息，避免脏读 

```xml
 <!--根据id查询用户-->
    <select id="findById" parameterType="INT" resultType="User">
        select * from USER where id = #{uid};
    </select>
```

```java
	/**
     * 测试一级缓存的同步
     */
    @Test
    public void testClearCache(){
        //1.根据id查询用户
        User user = userDao.findById(41);
        System.out.println(user);
        //2.更新用户信息
        user.setUsername("update user clear cache");
        user.setAddress("佛山");
        userDao.findById(41);

        //3.再次查询这个用户
        User user2 = userDao.findById(41);
        System.out.println(user2);

        System.out.println(user == user2);
    }
```

#### 16.2 Mybatis 二级缓存 

二级缓存是 mapper 映射级别的缓存，多个 SqlSession 去操作同一个 Mapper 映射的 sql 语句，多个
SqlSession 可以共用二级缓存，二级缓存是跨 SqlSession 的 

1. 在 SqlMapConfig.xml 文件开启二级缓存 

   ```xml
   <settings>
           <!--开启支持二级缓存-->
           <setting name="cacheEnabled" value="true"/>
   </settings>
   ```

2. 配置相关的 Mapper 映射文件 

   ```xml
   <mapper namespace="com.wmxyyy.dao.IUserDao">
       <!--开启user支持二级缓存-->
       <cache/>
   ```

3. 配置 statement 上面的 useCache 属性 

   ```xml
       <!--根据id查询用户-->
       <select id="findById" parameterType="INT" resultType="User" useCache="true">
           select * from USER where id = #{uid};
       </select>
   ```

#### 16.3 二级缓存注意事项 

当我们在使用二级缓存时，所缓存的类一定要实现 java.io.Serializable 接口，这种就可以使用序列化
方式来保存对象。 

### 第十七章 Mybatis 注解开发 

#### 17.1 mybatis 的常用注解说明 

```java
@Insert:实现新增
@Update:实现更新
@Delete:实现删除
@Select:实现查询

@Result:实现结果集封装
@Results:可以与@Result 一起使用，封装多个结果集
@ResultMap:实现引用@Results 定义的封装

@One:实现一对一结果集封装
@Many:实现一对多结果集封装

@SelectProvider: 实现动态SQL映射

@CacheNamespace:实现注解二级缓存的使用
```

#### 17.2 使用 Mybatis 注解实现基本 CRUD 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <!--引入外部配置-->
    <properties resource="jdbcConfig.properties"></properties>
    <!--配置别名-->
    <typeAliases>
        <package name="com.wmxyyy.domain"></package>
    </typeAliases>
    <!--环境搭建-->
    <environments default="mysql">
        <environment id="mysql">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"></property>
                <property name="url" value="${jdbc.url}"></property>
                <property name="username" value="${jdbc.username}"></property>
                <property name="password" value="${jdbc.password}"></property>
            </dataSource>
        </environment>
    </environments>

    <!--指定带有注解的dao接口所在位置-->
    <mappers>
        <package name="com.wmxyyy.dao"></package>
    </mappers>
</configuration>
```

```java
/**
 * CRUD四个注解: @Select @Insert @Update @Delete
 */
public interface IUserDao {
    /**
     * 查询所有用户
     * @return
     */
    @Select("select * from user")
    List<User> findAll();

    /**
     * 保存用户
     * @param user
     */
    @Insert("insert into user(username,address,sex,birthday) " +
            "values(#{username},#{address},#{sex},#{birthday})")
    void saveUser(User user);

    /**
     * 更新用户信息
     * @param user
     */
    @Update("update user set " +
            "username=#{username}," +
            "address=#{address}," +
            "sex=#{sex}," +
            "birthday=#{birthday}")
    void updateUser(User user);

    /**
     * 根据id删除用户
     * @param userId
     */
    @Delete("delete from user " +
            "where id=#{id}")
    void deleteUser(Integer userId);

    /**
     * 根据id查询用户
     * @param userId
     * @return
     */
    @Select("select * from user " +
            "where id = #{id}")
    User findById(Integer userId);

    /**
     * 根据用户名称模糊查询
     * @param username
     * @return
     */
    @Select("select * from user where username like '% ${value} %'")
    List<User> findUserByName(String username);

    /**
     * 查询总用户数量
     * @return
     */
    @Select("select count(*) from user")
    int findTotalUser();
}
```

### 第十八章 使用注解实现复杂关系映射开发 

实现复杂关系映射之前我们可以在映射文件中通过配置<resultMap>来实现， 在使用注解开发时我们需要借助@Results 注解， @Result 注解， @One 注解， @Many 注解 

#### 18.1 复杂关系映射的注解说明 

```java
@Results 注解
代替的是标签<resultMap>
该注解中可以使用单个@Result 注解，也可以使用@Result 集合
@Results（{@Result（）， @Result（） }）或@Results（@Result（））

@Resutl 注解
代替了 <id>标签和<result>标签
@Result 中 属性介绍：
id 是否是主键字段
column 数据库的列名
property 需要装配的属性名
one 需要使用的@One 注解（@Result（one=@One）（）））
many 需要使用的@Many 注解（@Result（many=@many）（）））

@One 注解（一对一）
代替了<assocation>标签，是多表查询的关键，在注解中用来指定子查询返回单一对象。
@One 注解属性介绍：
select 指定用来多表查询的 sqlmapper
fetchType 会覆盖全局的配置参数 lazyLoadingEnabled。。
使用格式：
@Result(column=" ",property="",one=@One(select=""))
    
@Many 注解（多对一）
代替了<Collection>标签,是是多表查询的关键，在注解中用来指定子查询返回对象集合。
注意：聚集元素用来处理“一对多”的关系。需要指定映射的 Java 实体类的属性，属性的 javaType
（一般为 ArrayList）但是注解中可以不定义；
使用格式：
@Result(property="",column="",many=@Many(select=""))
```

```java
/**
 * @author wmxyyy
 * @date 2020/1/7 22:04
 * @state 用户的持久层接口
 */
@CacheNamespace(blocking = true)
public interface IUserDao {
    /**
     * 查询所有用户,同时获取到用户下所有的账户信息
     * @return
     */
    @Select("select * from user")
    @Results(id="userMap",value = {
            @Result(id = true,property = "userId",column = "id"),
            @Result(property = "userName",column = "username"),
            @Result(property = "userAddress",column = "address"),
            @Result(property = "userSex",column = "sex"),
            @Result(property = "userBirthday",column = "birthday")
    })
    List<User> findAll();

    /**
     * 根据Id查询用户信息
     * @param userId
     */
    @Select("select * from user  where id=#{id} ")
    @ResultMap(value={"userMap"})
    User findById(Integer userId);

    /**
     * 根据用户名称模糊查询
     * @param username
     * @return
     */
    @Select("select * from user where username like #{username}")
    @ResultMap("userMap")
    List<User> findUserByName(String username);
}

```

```java
/**
 * @author wmxyyy
 * @date 2020/1/7 22:04
 * @state 用户的持久层接口
 */
@CacheNamespace(blocking = true)//mybatis 基于注解方式实现配置二级缓存
public interface IUserDao {
    /**
     * 查询所有用户,同时获取到用户下所有的账户信息
     * @return
     */
    @Select("select * from user")
    @Results(id="userMap",value = {
            @Result(id = true,property = "userId",column = "id"),
            @Result(property = "userName",column = "username"),
            @Result(property = "userAddress",column = "address"),
            @Result(property = "userSex",column = "sex"),
            @Result(property = "userBirthday",column = "birthday"),
            @Result(property = "accounts",column = "id",
                    many = @Many(select = "com.wmxyyy.dao.IAccountDao.findAccountByUid",
                            fetchType = FetchType.LAZY))
    })
    List<User> findAll();

    /**
     * 根据Id查询用户信息
     * @param userId
     */
    @Select("select * from user  where id=#{id} ")
    @ResultMap(value={"userMap"})
    User findById(Integer userId);

    /**
     * 根据用户名称模糊查询
     * @param username
     * @return
     */
    @Select("select * from user where username like #{username}")
    @ResultMap("userMap")
    List<User> findUserByName(String username);
}

```

