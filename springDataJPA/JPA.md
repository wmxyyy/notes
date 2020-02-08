

## JPA

### 第一章 orm思想

<img src="C:\Users\wang1\Desktop\mdSource\7_springDataJPA\img\01-回顾jdbc操作以及引入orm.png" alt="01-回顾jdbc操作以及引入orm" style="zoom:67%;" />

1.1 主要目的：

操作实体类就相当于操作数据库表，不再重点关注sql语句

> 当实现一个应用程序时（不使用O/R Mapping），我们可能会写特别多数据访问层的代码，从数据库保存数据、修改数据、删除数据，而这些代码都是重复的。而使用ORM则会大大减少重复性代码。对象关系映射（Object Relational Mapping，简称ORM），主要实现程序对象到关系数据库数据的映射。

1.2 建立两个映射关系：

- 实体类和表的映射关系
- 实体类中属性和表中字段的映射关系

> 实现了ORM思想的框架：mybatis，hibernate

### 第二章 hibernate框架介绍

Hibernate是一个开放源代码的对象关系映射框架，它对JDBC进行了非常轻量级的对象封装，它将POJO与数据库表建立映射关系，是一个全自动的orm框架

### 第三章 JPA规范

JPA的全称是Java Persistence API， 即Java 持久化API，是SUN公司推出的一套基于ORM的规范，内部是由一系列的接口和抽象类构成。

<img src="C:\Users\wang1\Desktop\mdSource\7_springDataJPA\img\02-jpa.png" alt="02-jpa" style="zoom:50%;" />

### 第四章 jpa的基本操作

#### 4.1 搭建开发环境

- ##### maven工程导入坐标

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.hibernate.version>5.0.7.Final</project.hibernate.version>
</properties>

<dependencies>
    <!-- junit -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>

    <!-- hibernate对jpa的支持包 -->
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-entitymanager</artifactId>
        <version>${project.hibernate.version}</version>
    </dependency>

    <!-- c3p0 -->
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-c3p0</artifactId>
        <version>${project.hibernate.version}</version>
    </dependency>

    <!-- log日志 -->
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>

    <!-- Mysql and MariaDB -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.6</version>
    </dependency>
</dependencies>
```
- ##### 创建客户的数据库表

```sql
 /*创建客户表*/
    CREATE TABLE cst_customer (
      cust_id bigint(32) NOT NULL AUTO_INCREMENT COMMENT '客户编号(主键)',
      cust_name varchar(32) NOT NULL COMMENT '客户名称(公司名称)',
      cust_source varchar(32) DEFAULT NULL COMMENT '客户信息来源',
      cust_industry varchar(32) DEFAULT NULL COMMENT '客户所属行业',
      cust_level varchar(32) DEFAULT NULL COMMENT '客户级别',
      cust_address varchar(128) DEFAULT NULL COMMENT '客户联系地址',
      cust_phone varchar(64) DEFAULT NULL COMMENT '客户联系电话',
      PRIMARY KEY (`cust_id`)
    ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

```

- ##### 创建客户的实体类

```java
public class Customer implements Serializable {
    
	private Long custId;
	private String custName;
	private String custSource;
	private String custIndustry;
	private String custLevel;
	private String custAddress;
	private String custPhone;
    ...
}
```

- ##### 编写实体类和数据库表的映射配置[重点]

  ```java
  /**
   * 客户的实体类配置映射关系
   *
   *   1.实体类和表的映射关系
   *      @Entity:声明实体类
   *      @Table : 配置实体类和表的映射关系
   *          name : 配置数据库表的名称
   *   2.实体类中属性和表中字段的映射关系
   */
  @Entity
  @Table(name = "cst_customer")
  public class Customer {
  
      /**
       * @Id：声明主键的配置
       * @GeneratedValue:配置主键的生成策略
       *      strategy
       *          GenerationType.IDENTITY ：自增，mysql
       *                 * 底层数据库必须支持自动增长（底层数据库支持的自动增长方式，对id自增）
       *          GenerationType.SEQUENCE : 序列，oracle
       *                  * 底层数据库必须支持序列
       *          GenerationType.TABLE : jpa提供的一种机制，通过一张数据库表的形式帮助我们完成主键自增
       *          GenerationType.AUTO ： 由程序自动的帮助我们选择主键生成策略
       * @Column:配置属性和字段的映射关系
       *      name：数据库表中字段的名称
       */
      @Id
      @GeneratedValue(strategy = GenerationType.IDENTITY)
      @Column(name = "cust_id")
      private Long custId; //客户的主键
  
      @Column(name = "cust_name")
      private String custName;//客户名称
  
      @Column(name="cust_source")
      private String custSource;//客户来源
  
      @Column(name="cust_level")
      private String custLevel;//客户级别
  
      @Column(name="cust_industry")
      private String custIndustry;//客户所属行业
  
      @Column(name="cust_phone")
      private String custPhone;//客户的联系方式
  
      @Column(name="cust_address")
      private String custAddress;//客户地址
  ```

  > @Entity：指定当前类是实体类。
  >
  > @Table：指定实体类和表之间的对应关系。
  >
  > ​       属性：name：指定数据库表的名称
  >
  > @Id：指定当前字段是主键。
  >
  > @GeneratedValue：指定主键的生成方式。。
  >
  > ​        属性：strategy ：指定主键生成策略。
  >
  >  @Column：指定实体类属性和数据库表之间的对应关系
  >
  > ​        属性：
  >
  > ​              name：指定数据库表的列名称。
  >
  > ​              unique：是否唯一  
  >
  > ​              nullable：是否可以为空  
  >
  > ​              inserttable：是否可以插入  
  >
  > ​              updateable：是否可以更新  
  >
  > ​              columnDefinition: 定义建表时创建此列的DDL  
  >
  > ​              secondaryTable: 从表名。如果此列不建在主表上（默认建在主表），该属性定义该列所在从表的名字搭建开发环境[重点]

- ##### 1.1.1    配置JPA的核心配置文件

  在java工程的src路径下创建一个名为META-INF的文件夹，在此文件夹下创建一个名为persistence.xml的配置文件

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <persistence xmlns="http://java.sun.com/xml/ns/persistence" version="2.0">
      <!--需要配置persistence-unit节点
          持久化单元：
              name：持久化单元名称
              transaction-type：事务管理的方式
                      JTA：分布式事务管理
                      RESOURCE_LOCAL：本地事务管理
      -->
      <persistence-unit name="myJpa" transaction-type="RESOURCE_LOCAL">
          <!--jpa的实现方式 -->
          <provider>org.hibernate.jpa.HibernatePersistenceProvider</provider>
          <!--可选配置：配置jpa实现方的配置信息-->
          <properties>
              <!-- 数据库信息
                  用户名，javax.persistence.jdbc.user
                  密码，  javax.persistence.jdbc.password
                  驱动，  javax.persistence.jdbc.driver
                  数据库地址   javax.persistence.jdbc.url
              -->
              <property name="javax.persistence.jdbc.user" value="root"/>
              <property name="javax.persistence.jdbc.password" value="111111"/>
              <property name="javax.persistence.jdbc.driver" value="com.mysql.jdbc.Driver"/>
              <property name="javax.persistence.jdbc.url" value="jdbc:mysql:///jpa"/>
  
              <!--配置jpa实现方(hibernate)的配置信息
                  显示sql           ：   false|true
                  自动创建数据库表    ：  hibernate.hbm2ddl.auto
                          create   : 程序运行时创建数据库表（如果有表，先删除表再创建）
                          update   :程序运行时创建表（如果有表，不会创建表）
                          none     :不会创建表
              -->
              <property name="hibernate.show_sql" value="true" />
              <property name="hibernate.hbm2ddl.auto" value="update" />
          </properties>
      </persistence-unit>
  </persistence>
  ```

#### 4.2 实现保存操作

```java
    /**
     * 测试jpa的保存
     *      案例：保存一个客户到数据库中
     *  Jpa的操作步骤
     *     1.加载配置文件创建工厂（实体管理器工厂）对象
     *     2.通过实体管理器工厂获取实体管理器
     *     3.获取事务对象，开启事务
     *     4.完成增删改查操作
     *     5.提交事务（回滚事务）
     *     6.释放资源
     */
    @Test
    public void testSave() {
        //1.加载配置文件创建工厂（实体管理器工厂）对象
        EntityManagerFactory factory = Persistence.createEntityManagerFactory("myJpa");
        //2.通过实体管理器工厂获取实体管理器
        EntityManager em = factory.createEntityManager();
        //3.获取事务对象，开启事务
        EntityTransaction tx = em.getTransaction(); //获取事务对象
        tx.begin();//开启事务
        //4.完成增删改查操作：保存一个客户到数据库中
        Customer customer = new Customer();
        customer.setCustName("传智播客");
        customer.setCustIndustry("教育");
        em.persist(customer); //保存操作
        //5.提交事务
        tx.commit();
        //6.释放资源
        em.close();
        factory.close();
    }
```

> EntityManagerFactory的创建过程比较浪费资源
> 特点：线程安全的对象
> 多个线程访问同一个EntityManagerFactory不会有线程安全问题
>
> 如何解决EntityManagerFactory的创建过程浪费资源（耗时）的问题？
> 思路：创建一个公共的EntityManagerFactory的对象
>
> 静态代码块的形式创建EntityManagerFactory

```java
/**
 * 解决实体管理器工厂的浪费资源和耗时问题
 *      通过静态代码块的形式，当程序第一次访问此工具类时，创建一个公共的实体管理器工厂对象
 *
 * 第一次访问getEntityManager方法：经过静态代码块创建一个factory对象，再调用方法创建一个EntityManager对象
 * 第二次方法getEntityManager方法：直接通过一个已经创建好的factory对象，创建EntityManager对象
 */
 
public class JpaUtils {

    private static EntityManagerFactory factory;

    static  {
        //1.加载配置文件，创建entityManagerFactory
        factory = Persistence.createEntityManagerFactory("myJpa");
    }

    /**
     * 获取EntityManager对象
     */
    public static EntityManager getEntityManager() {
       return factory.createEntityManager();
    }
}
```
#### 4.3 使用JPA完成增删改查操作

```java
public class JpaTest {

    @Test
    public void testSave() {
        EntityManager em = JpaUtils.getEntityManager();
        //3.获取事务对象，开启事务
        EntityTransaction tx = em.getTransaction(); //获取事务对象
        tx.begin();//开启事务
        //4.完成增删改查操作：保存一个客户到数据库中
        Customer customer = new Customer();
        customer.setCustName("传智播客");
        customer.setCustIndustry("教育");
        //保存，
        em.persist(customer); //保存操作
        //5.提交事务
        tx.commit();
        //6.释放资源
        em.close();

    }

    /**
     * 根据id查询客户
     *  使用find方法查询：
     *      1.查询的对象就是当前客户对象本身
     *      2.在调用find方法的时候，就会发送sql语句查询数据库
     *  立即加载
     */
    @Test
    public  void testFind() {
        //1.通过工具类获取entityManager
        EntityManager entityManager = JpaUtils.getEntityManager();
        //2.开启事务
        EntityTransaction tx = entityManager.getTransaction();
        tx.begin();
        //3.增删改查 -- 根据id查询客户
        /**
         * find : 根据id查询数据
         *      class：查询数据的结果需要包装的实体类类型的字节码
         *      id：查询的主键的取值
         */
        Customer customer = entityManager.find(Customer.class, 1l);
       // System.out.print(customer);
        //4.提交事务
        tx.commit();
        //5.释放资源
        entityManager.close();
    }

    /**
     * 根据id查询客户
     *      getReference方法
     *          1.获取的对象是一个动态代理对象
     *          2.调用getReference方法不会立即发送sql语句查询数据库
     *              * 当调用查询结果对象的时候，才会发送查询的sql语句：什么时候用，什么时候发送sql语句查询数据库
     *
     * 延迟加载（懒加载）
     *      * 得到的是一个动态代理对象
     *      * 什么时候用，什么使用才会查询
     */
    @Test
    public  void testReference() {
        //1.通过工具类获取entityManager
        EntityManager entityManager = JpaUtils.getEntityManager();
        //2.开启事务
        EntityTransaction tx = entityManager.getTransaction();
        tx.begin();
        //3.增删改查 -- 根据id查询客户
        /**
         * getReference : 根据id查询数据
         *      class：查询数据的结果需要包装的实体类类型的字节码
         *      id：查询的主键的取值
         */
        Customer customer = entityManager.getReference(Customer.class, 1l);
        System.out.print(customer);
        //4.提交事务
        tx.commit();
        //5.释放资源
        entityManager.close();
    }

    /**
     * 删除客户的案例
     *
     */
    @Test
    public  void testRemove() {
        //1.通过工具类获取entityManager
        EntityManager entityManager = JpaUtils.getEntityManager();
        //2.开启事务
        EntityTransaction tx = entityManager.getTransaction();
        tx.begin();
        //3.增删改查 -- 删除客户
        //i 根据id查询客户
        Customer customer = entityManager.find(Customer.class,1l);
        //ii 调用remove方法完成删除操作
        entityManager.remove(customer);
        //4.提交事务
        tx.commit();
        //5.释放资源
        entityManager.close();
    }

    /**
     * 更新客户的操作
     *      merge(Object)
     */
    @Test
    public  void testUpdate() {
        //1.通过工具类获取entityManager
        EntityManager entityManager = JpaUtils.getEntityManager();
        //2.开启事务
        EntityTransaction tx = entityManager.getTransaction();
        tx.begin();
        //3.增删改查 -- 更新操作
        //i 查询客户
        Customer customer = entityManager.find(Customer.class,1l);
        //ii 更新客户
        customer.setCustIndustry("it教育");
        entityManager.merge(customer);
        //4.提交事务
        tx.commit();
        //5.释放资源
        entityManager.close();
    }
}
iii.jpql查询
	sql：查询的是表和表中的字段
	jpql：查询的是实体类和类中的属性
```

### 第五章 JPA中的复杂查询

#### 5.1 JPQL（ Java Persistence Query Language）

> Java持久化查询语言(JPQL)是一种可移植的查询语言，旨在以面向对象表达式语言的表达式，将SQL语法和简单查询语义绑定在一起·使用这种语言编写的查询是可移植的，可以被编译成所有主流数据库服务器上的SQL。
>
> 其特征与原生SQL语句类似，并且完全面向对象，通过类名和属性访问，而不是表名和表的属性。

#### 5.2 查询全部

```java
/**
     * 查询全部
     *      jqpl：from cn.itcast.domain.Customer
     *      sql：SELECT * FROM cst_customer
     */
    @Test
    public void testFindAll() {
        //1.获取entityManager对象
        EntityManager em = JpaUtils.getEntityManager();
        //2.开启事务
        EntityTransaction tx = em.getTransaction();
        tx.begin();
        //3.查询全部
        String jpql = "from Customer ";
        Query query = em.createQuery(jpql);//创建Query查询对象，query对象才是执行jqpl的对象
        //发送查询，并封装结果集
        List list = query.getResultList();
        for (Object obj : list) {
            System.out.print(obj);
        }
        //4.提交事务
        tx.commit();
        //5.释放资源
        em.close();
    }
```



#### 5.3 分页查询

```java
/**
     * 分页查询
     *      sql：select * from cst_customer limit 0,2
     *      jqpl : from Customer
     */
    @Test
    public void testPaged() {
        //1.获取entityManager对象
        EntityManager em = JpaUtils.getEntityManager();
        //2.开启事务
        EntityTransaction tx = em.getTransaction();
        tx.begin();
        //3.查询全部
        //i.根据jpql语句创建Query查询对象
        String jpql = "from Customer";
        Query query = em.createQuery(jpql);
        //ii.对参数赋值 -- 分页参数
        //起始索引
        query.setFirstResult(0);
        //每页查询的条数
        query.setMaxResults(2);
        //iii.发送查询，并封装结果
        /**
         * getResultList ： 直接将查询结果封装为list集合
         * getSingleResult : 得到唯一的结果集
         */
        List list = query.getResultList();
        for(Object obj : list) {
            System.out.println(obj);
        }
        //4.提交事务
        tx.commit();
        //5.释放资源
        em.close();
    }
```

#### 5.4 统计查询

```java
    /**
     * 使用jpql查询，统计客户的总数
     *      sql：SELECT COUNT(cust_id) FROM cst_customer
     *      jpql：select count(custId) from Customer
     */
    @Test
    public void testCount() {
        //1.获取entityManager对象
        EntityManager em = JpaUtils.getEntityManager();
        //2.开启事务
        EntityTransaction tx = em.getTransaction();
        tx.begin();
        //3.查询全部
        //i.根据jpql语句创建Query查询对象
        String jpql = "select count(custId) from Customer";
        Query query = em.createQuery(jpql);
        //ii.对参数赋值
        //iii.发送查询，并封装结果

        /**
         * getResultList ： 直接将查询结果封装为list集合
         * getSingleResult : 得到唯一的结果集
         */
        Object result = query.getSingleResult();

        System.out.println(result);

        //4.提交事务
        tx.commit();
        //5.释放资源
        em.close();
    }
```

#### 5.5 条件查询

```java
	/**
     * 条件查询
     *     案例：查询客户名称以‘传智播客’开头的客户
     *          sql：SELECT * FROM cst_customer WHERE cust_name LIKE  ?
     *          jpql : from Customer where custName like ?
     */
    @Test
    public void testCondition() {
        //1.获取entityManager对象
        EntityManager em = JpaUtils.getEntityManager();
        //2.开启事务
        EntityTransaction tx = em.getTransaction();
        tx.begin();
        //3.查询全部
        //i.根据jpql语句创建Query查询对象
        String jpql = "from Customer where custName like ? ";
        Query query = em.createQuery(jpql);
        //ii.对参数赋值 -- 占位符参数
        //第一个参数：占位符的索引位置（从1开始），第二个参数：取值
        query.setParameter(1,"传智播客%");

        //iii.发送查询，并封装结果
        /**
         * getResultList ： 直接将查询结果封装为list集合
         * getSingleResult : 得到唯一的结果集
         */
        List list = query.getResultList();

        for(Object obj : list) {
            System.out.println(obj);
        }

        //4.提交事务
        tx.commit();
        //5.释放资源
        em.close();
    }
```

#### 5.6 排序

```java
/**
     * 排序查询： 倒序查询全部客户（根据id倒序）
     *      sql：SELECT * FROM cst_customer ORDER BY cust_id DESC
     *      jpql：from Customer order by custId desc
     *
     * 进行jpql查询
     *      1.创建query查询对象
     *      2.对参数进行赋值
     *      3.查询，并得到返回结果
     */
    @Test
    public void testOrders() {
        //1.获取entityManager对象
        EntityManager em = JpaUtils.getEntityManager();
        //2.开启事务
        EntityTransaction tx = em.getTransaction();
        tx.begin();
        //3.查询全部
        String jpql = "from Customer order by custId desc";
        Query query = em.createQuery(jpql);//创建Query查询对象，query对象才是执行jqpl的对象

        //发送查询，并封装结果集
        List list = query.getResultList();
        for (Object obj : list) {
            System.out.println(obj);
        }
        //4.提交事务
        tx.commit();
        //5.释放资源
        em.close();
    }
```

