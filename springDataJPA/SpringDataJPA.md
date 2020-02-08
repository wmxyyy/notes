## SpringDataJPA

### 第一章 springDataJpa

#### 1.1 概述

Spring Data JPA 是 Spring 基于 ORM 框架、JPA 规范的基础上封装的一套JPA应用框架，可使开发者用极简的代码即可实现对数据库的访问和操作。它提供了包括增删改查等在内的常用功能，且易于扩展！学习并使用 Spring Data JPA 可以极大提高开发效率！

> Spring Data JPA 让我们解脱了DAO层的操作，基本上所有CRUD都可以依赖于它来实现,在实际的工作工程中，推荐使用Spring Data JPA + ORM（如：hibernate）完成操作，这样在切换不同的ORM框架时提供了极大的方便，同时也使数据库层操作更加简单，方便解耦

#### 1.2 特性

使用了SpringDataJpa，我们的dao层中只需要写接口就**自动**具有了增删改查、分页查询等方法。

#### 1.3 Spring Data JPA 与 JPA和hibernate之间的关系

JPA是一套规范，内部是有接口和抽象类组成的。hibernate是一套成熟的ORM框架，而且Hibernate实现了JPA规范，所以也可以称hibernate为JPA的一种实现方式，我们使用JPA的API编程，意味着站在更高的角度上看待问题（**面向接口编程**）

Spring Data JPA是Spring提供的一套对JPA操作更加高级的封装，是在JPA规范下的专门用来进行数据持久化的解决方案。

![01-springDataJpa，jpa，hibernate关系](C:\Users\wang1\Desktop\mdSource\7_springDataJPA\img\01-springDataJpa，jpa，hibernate关系.png)

### 第二章 springDataJpa的入门操作

#### 2.1 搭建Spring Data JPA的开发环境

- ##### 引入Spring Data JPA的坐标

  ```xml

  <properties>
      <spring.version>5.0.2.RELEASE</spring.version>
      <hibernate.version>5.0.7.Final</hibernate.version>
      <slf4j.version>1.6.6</slf4j.version>
      <log4j.version>1.2.12</log4j.version>
      <c3p0.version>0.9.1.2</c3p0.version>
      <mysql.version>5.1.6</mysql.version>
  </properties>
  
  <dependencies>
      <!-- junit单元测试 -->
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.12</version>
          <scope>test</scope>
      </dependency>
  
      <!-- spring beg -->
      <dependency>
          <groupId>org.aspectj</groupId>
          <artifactId>aspectjweaver</artifactId>
          <version>1.6.8</version>
      </dependency>
  
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-aop</artifactId>
          <version>${spring.version}</version>
      </dependency>
  
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>${spring.version}</version>
      </dependency>
  
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context-support</artifactId>
          <version>${spring.version}</version>
      </dependency>
  
      <!-- spring对orm框架的支持包-->
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-orm</artifactId>
          <version>${spring.version}</version>
      </dependency>
  
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-beans</artifactId>
          <version>${spring.version}</version>
      </dependency>
  
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-core</artifactId>
          <version>${spring.version}</version>
      </dependency>
  
      <!-- spring end -->
  
      <!-- hibernate beg -->
      <dependency>
          <groupId>org.hibernate</groupId>
          <artifactId>hibernate-core</artifactId>
          <version>${hibernate.version}</version>
      </dependency>
      <dependency>
          <groupId>org.hibernate</groupId>
          <artifactId>hibernate-entitymanager</artifactId>
          <version>${hibernate.version}</version>
      </dependency>
      <dependency>
          <groupId>org.hibernate</groupId>
          <artifactId>hibernate-validator</artifactId>
          <version>5.2.1.Final</version>
      </dependency>
      <!-- hibernate end -->
  
      <!-- c3p0 beg -->
      <dependency>
          <groupId>c3p0</groupId>
          <artifactId>c3p0</artifactId>
          <version>${c3p0.version}</version>
      </dependency>
      <!-- c3p0 end -->
  
      <!-- log end -->
      <dependency>
          <groupId>log4j</groupId>
          <artifactId>log4j</artifactId>
          <version>${log4j.version}</version>
      </dependency>
  
      <dependency>
          <groupId>org.slf4j</groupId>
          <artifactId>slf4j-api</artifactId>
          <version>${slf4j.version}</version>
      </dependency>
  
      <dependency>
          <groupId>org.slf4j</groupId>
          <artifactId>slf4j-log4j12</artifactId>
          <version>${slf4j.version}</version>
      </dependency>
      <!-- log end -->
  
  
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>${mysql.version}</version>
      </dependency>
  
      <!-- spring data jpa 的坐标-->
      <dependency>
          <groupId>org.springframework.data</groupId>
          <artifactId>spring-data-jpa</artifactId>
          <version>1.9.0.RELEASE</version>
      </dependency>
  
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-test</artifactId>
          <version>${spring.version}</version>
      </dependency>
  
      <!-- el beg 使用spring data jpa 必须引入 -->
      <dependency>
          <groupId>javax.el</groupId>
          <artifactId>javax.el-api</artifactId>
          <version>2.2.4</version>
      </dependency>
  
      <dependency>
          <groupId>org.glassfish.web</groupId>
          <artifactId>javax.el</artifactId>
          <version>2.2.4</version>
      </dependency>
      <!-- el end -->
  </dependencies>
  ```

- ##### 整合Spring Data JPA与Spring

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:jdbc="http://www.springframework.org/schema/jdbc" xmlns:tx="http://www.springframework.org/schema/tx"
         xmlns:jpa="http://www.springframework.org/schema/data/jpa" xmlns:task="http://www.springframework.org/schema/task"
         xsi:schemaLocation="
  		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
  		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
  		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
  		http://www.springframework.org/schema/jdbc http://www.springframework.org/schema/jdbc/spring-jdbc.xsd
  		http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
  		http://www.springframework.org/schema/data/jpa
  		http://www.springframework.org/schema/data/jpa/spring-jpa.xsd">
  
      <!--spring 和 spring data jpa的配置-->
  
      <!-- 1.创建entityManagerFactory对象交给spring容器管理-->
      <bean id="entityManagerFactoty" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
          <property name="dataSource" ref="dataSource" />
          <!--配置的扫描的包（实体类所在的包） -->
          <property name="packagesToScan" value="cn.itcast.domain" />
          <!-- jpa的实现厂家 -->
          <property name="persistenceProvider">
              <bean class="org.hibernate.jpa.HibernatePersistenceProvider"/>
          </property>
  
          <!--jpa的供应商适配器 -->
          <property name="jpaVendorAdapter">
              <bean class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter">
                  <!--配置是否自动创建数据库表 -->
                  <property name="generateDdl" value="false" />
                  <!--指定数据库类型 -->
                  <property name="database" value="MYSQL" />
                  <!--数据库方言：支持的特有语法 -->
                  <property name="databasePlatform" value="org.hibernate.dialect.MySQLDialect" />
                  <!--是否显示sql -->
                  <property name="showSql" value="true" />
              </bean>
          </property>
  
          <!--jpa的方言 ：高级的特性 -->
          <property name="jpaDialect" >
              <bean class="org.springframework.orm.jpa.vendor.HibernateJpaDialect" />
          </property>
  
      </bean>
  
      <!--2.创建数据库连接池 -->
      <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
          <property name="user" value="root"></property>
          <property name="password" value="111111"></property>
          <property name="jdbcUrl" value="jdbc:mysql:///jpa" ></property>
          <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
      </bean>
  
      <!--3.整合spring dataJpa-->
      <jpa:repositories base-package="cn.itcast.dao" transaction-manager-ref="transactionManager"
                     entity-manager-factory-ref="entityManagerFactoty" ></jpa:repositories>
  
      <!--4.配置事务管理器 -->
      <bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
          <property name="entityManagerFactory" ref="entityManagerFactoty"></property>
      </bean>
  
      <!-- 4.txAdvice-->
      <tx:advice id="txAdvice" transaction-manager="transactionManager">
          <tx:attributes>
              <tx:method name="save*" propagation="REQUIRED"/>
              <tx:method name="insert*" propagation="REQUIRED"/>
              <tx:method name="update*" propagation="REQUIRED"/>
              <tx:method name="delete*" propagation="REQUIRED"/>
              <tx:method name="get*" read-only="true"/>
              <tx:method name="find*" read-only="true"/>
              <tx:method name="*" propagation="REQUIRED"/>
          </tx:attributes>
      </tx:advice>
  
      <!-- 5.aop-->
      <aop:config>
          <aop:pointcut id="pointcut" expression="execution(* cn.itcast.service.*.*(..))" />
          <aop:advisor advice-ref="txAdvice" pointcut-ref="pointcut" />
      </aop:config>
  
      <!--5.声明式事务 -->
  
      <!-- 6. 配置包扫描-->
      <context:component-scan base-package="cn.itcast" ></context:component-scan>
  </beans>
  ```

- 使用JPA注解配置映射关系

  ```java
  /**
   * 1.实体类和表的映射关系
   *      @Eitity
   *      @Table
   * 2.类中属性和表中字段的映射关系
   *      @Id
   *      @GeneratedValue
   *      @Column
   */
  @Entity
  @Table(name="cst_customer")
  public class Customer {
  
      @Id
      @GeneratedValue(strategy = GenerationType.IDENTITY)
      @Column(name="cust_id")
      private Long custId;
      @Column(name="cust_address")
      private String custAddress;
      @Column(name="cust_industry")
      private String custIndustry;
      @Column(name="cust_level")
      private String custLevel;
      @Column(name="cust_name")
      private String custName;
      @Column(name="cust_phone")
      private String custPhone;
      @Column(name="cust_source")
      private String custSource;
  ```

#### 2.2 编写符合Spring Data JPA规范的Dao层接口

> Spring Data JPA是spring提供的一款对于数据访问层（Dao层）的框架，使用Spring Data JPA，只需要按照框架的规范提供dao接口，不需要实现类就可以完成数据库的增删改查、分页查询等方法的定义，极大的简化了我们的开发过程。

在Spring Data JPA中，对于定义符合规范的Dao层接口，我们只需要：

- **创建一个Dao层接口，并继承JpaRepository和JpaSpecificationExecutor**

- **提供相应的泛型**

```java
/**
 * 符合SpringDataJpa的dao层接口规范
 *      JpaRepository<操作的实体类类型，实体类中主键属性的类型>
 *          * 封装了基本CRUD操作
 *      JpaSpecificationExecutor<操作的实体类类型>
 *          * 封装了复杂查询（分页）
 */
public interface CustomerDao extends JpaRepository<Customer,Long> ,JpaSpecificationExecutor<Customer> {
    
}
```

#### 2.3 客户的基本CRUD

```java
@RunWith(SpringJUnit4ClassRunner.class) //声明spring提供的单元测试环境
@ContextConfiguration(locations = "classpath:applicationContext.xml")//指定spring容器的配置信息
public class CustomerDaoTest {
    @Autowired
    private CustomerDao customerDao;

    /**
     * 根据id查询
     */
    @Test
    public void testFindOne() {
        Customer customer = customerDao.findOne(4l);
        System.out.println(customer);
    }

    /**
     * save : 保存或者更新
     *      根据传递的对象是否存在主键id，
     *      如果没有id主键属性：保存
     *      存在id主键属性，根据id查询数据，更新数据
     */
    @Test
    public void testSave() {
        Customer customer  = new Customer();
        customer.setCustName("黑马程序员");
        customer.setCustLevel("vip");
        customer.setCustIndustry("it教育");
        customerDao.save(customer);
    }

    @Test
    public void testUpdate() {
        Customer customer  = new Customer();
        customer.setCustId(4l);
        customer.setCustName("黑马程序员很厉害");
        customerDao.save(customer);
    }

    @Test
    public void testDelete () {
        customerDao.delete(3l);
    }


    /**
     * 查询所有
     */
    @Test
    public void testFindAll() {
        List<Customer> list = customerDao.findAll();
        for(Customer customer : list) {
            System.out.println(customer);
        }
    }

    /**
     * 测试统计查询：查询客户的总数量
     *      count:统计总条数
     */
    @Test
    public void testCount() {
        long count = customerDao.count();//查询全部的客户数量
        System.out.println(count);
    }

    /**
     * 测试：判断id为4的客户是否存在
     *      1. 可以查询以下id为4的客户
     *          如果值为空，代表不存在，如果不为空，代表存在
     *      2. 判断数据库中id为4的客户的数量
     *          如果数量为0，代表不存在，如果大于0，代表存在
     */
    @Test
    public void  testExists() {
        boolean exists = customerDao.exists(4l);
        System.out.println("id为4的客户 是否存在："+exists);
    }


    /**
     * 根据id从数据库查询
     *      @Transactional : 保证getOne正常运行
     *
     *  findOne：
     *      em.find()           :立即加载
     *  getOne：
     *      em.getReference     :延迟加载
     *      * 返回的是一个客户的动态代理对象
     *      * 什么时候用，什么时候查询
     */
    @Test
    @Transactional
    public void  testGetOne() {
        Customer customer = customerDao.getOne(4l);
        System.out.println(customer);
    }

}
```



### 第三章 Spring Data JPA的内部原理剖析

<img src="C:\Users\wang1\Desktop\mdSource\7_springDataJPA\img\03-springdatajpa的运行过程.png" alt="03-springdatajpa的运行过程"  />
1.通过JdkDynamicAopProxy的invoke方法创建了一个动态代理对象
2.SimpleJpaRepository当中封装了JPA的操作（借助JPA的api完成数据库的CRUD）
3.通过hibernate完成数据库操作（封装了jdbc）

### 第四章 复杂查询

#### 4.1 使用JPQL方式查询

使用Spring Data JPA提供的查询方法已经可以解决大部分的应用场景，但是对于某些业务来说，我们还需要灵活的构造查询条件，这时就可以使用@Query注解，结合JPQL的语句方式完成查询 

**@Query** 注解的使用非常简单，只需在方法上面标注该注解，同时提供一个**JPQL**查询语句

```java
 /**
     * 案例：根据客户名称查询客户
     *      使用jpql的形式查询
     *  jpql：from Customer where custName = ?
     *
     *  配置jpql语句，使用的@Query注解
     */
    @Query(value="from Customer where custName = ?")
    public Customer findJpql(String custName);


    /**
     * 案例：根据客户名称和客户id查询客户
     *      jpql： from Customer where custName = ? and custId = ?
     *
     *  对于多个占位符参数
     *      赋值的时候，默认的情况下，占位符的位置需要和方法参数中的位置保持一致
     *
     *  可以指定占位符参数的位置
     *      ? 索引的方式，指定此占位的取值来源
     */
    @Query(value = "from Customer where custName = ?2 and custId = ?1")
    public Customer findCustNameAndId(Long id,String name);

    /**
     * 使用jpql完成更新操作
     *      案例 ： 根据id更新，客户的名称
     *          更新4号客户的名称，将名称改为“黑马程序员”
     *
     *  sql  ：update cst_customer set cust_name = ? where cust_id = ?
     *  jpql : update Customer set custName = ? where custId = ?
     *
     *  @Query : 代表的是进行查询
     *      * 声明此方法是用来进行更新操作
     *  @Modifying
     *      * 当前执行的是一个更新操作
     *
     */
    @Query(value = " update Customer set custName = ?2 where custId = ?1 ")
    @Modifying
    public void updateCustomer(long custId,String custName);

```

```java
@RunWith(SpringJUnit4ClassRunner.class) //声明spring提供的单元测试环境
@ContextConfiguration(locations = "classpath:applicationContext.xml")//指定spring容器的配置信息
public class JpqlTest {
    @Autowired
    private CustomerDao customerDao;

    @Test
    public void  testFindJPQL() {
        Customer customer = customerDao.findJpql("传智播客");
        System.out.println(customer);
    }

    @Test
    public void testFindCustNameAndId() {
       // Customer customer =  customerDao.findCustNameAndId("传智播客",1l);
        Customer customer =  customerDao.findCustNameAndId(1l,"传智播客");
        System.out.println(customer);
    }

    /**
     * 测试jpql的更新操作
     *  * springDataJpa中使用jpql完成 更新/删除操作
     *         * 需要手动添加事务的支持
     *         * 默认会执行结束之后，回滚事务
     *   @Rollback : 设置是否自动回滚
     *          false | true
     */
    @Test
    @Transactional //添加事务的支持
    @Rollback(value = false)
    public void testUpdateCustomer() {
        customerDao.updateCustomer(4l,"黑马程序员");
    }

```

#### 4.2 使用SQL语句查询

```java
    /**
     * 使用sql的形式查询：
     *     查询全部的客户
     *  sql ： select * from cst_customer;
     *  Query : 配置sql查询
     *      value ： sql语句
     *      nativeQuery ： 查询方式
     *          true ： sql查询
     *          false：jpql查询
     *
     */
    //@Query(value = " select * from cst_customer" ,nativeQuery = true)
    @Query(value="select * from cst_customer where cust_name like ?1",nativeQuery = true)
    public List<Object [] > findSql(String name);

```

```java
    //测试sql查询
    @Test
    public void testFindSql() {
        List<Object[]> list = customerDao.findSql("传智播客%");
        for(Object [] obj : list) {
            System.out.println(Arrays.toString(obj));
        }
    }
```

#### 4.3 方法命名规则查询

方法命名规则查询就是根据方法的名字，就能创建查询。只需要按照Spring Data JPA提供的方法命名规则定义方法的名称，就可以完成查询工作。Spring Data JPA在程序执行的时候会根据方法名称进行解析，并自动生成查询语句进行查询

按照Spring Data JPA 定义的规则，查询方法以findBy开头，涉及条件查询时，条件的属性用条件关键字连接，要注意的是：条件属性首字母需大写。框架在进行方法名解析时，会先把方法名多余的前缀截取掉，然后对剩下部分进行解析。

```java
    /**
     * 方法名的约定：
     *      findBy : 查询
     *            对象中的属性名（首字母大写） ： 查询的条件
     *            CustName
     *            * 默认情况 ： 使用 等于的方式查询
     *                  特殊的查询方式
     *
     *  findByCustName   --   根据客户名称查询
     *
     *  再springdataJpa的运行阶段
     *          会根据方法名称进行解析  findBy    from  xxx(实体类)
     *                                      属性名称      where  custName =
     *
     *      1.findBy  + 属性名称 （根据属性名称进行完成匹配的查询=）
     *      2.findBy  + 属性名称 + “查询方式（Like | isnull）”
     *          findByCustNameLike
     *      3.多条件查询
     *          findBy + 属性名 + “查询方式”   + “多条件的连接符（and|or）”  + 属性名 + “查询方式”
     */
    public Customer findByCustName(String custName);


    public List<Customer> findByCustNameLike(String custName);

    //使用客户名称模糊匹配和客户所属行业精准匹配的查询
    public Customer findByCustNameLikeAndCustIndustry(String custName,String custIndustry);
}
```

```java
    //测试方法命名规则的查询
    @Test
    public void testNaming() {
        Customer customer = customerDao.findByCustName("传智播客");
        System.out.println(customer);
    }

    //测试方法命名规则的查询
    @Test
    public void testFindByCustNameLike() {
        List<Customer> list = customerDao.findByCustNameLike("传智播客%");
        for (Customer customer : list) {
            System.out.println(customer);
        }
    }

    //测试方法命名规则的查询
    @Test
    public void testFindByCustNameLikeAndCustIndustry() {
        Customer customer = customerDao.findByCustNameLikeAndCustIndustry("传智播客1%", "it教育");
        System.out.println(customer);
    }
}
```

### 第四章  Specifications动态查询

有时我们在查询某个实体的时候，给定的条件是不固定的，这时就需要动态构建相应的查询语句，在Spring Data JPA中可以通过JpaSpecificationExecutor接口查询。相比JPQL,其优势是类型安全,更加的面向对象。



```java
//JpaSpecificationExecutor 方法列表	

	T findOne(Specification<T> spec);  //查询单个对象
	List<T> findAll(Specification<T> spec);  //查询列表

	//查询全部，分页
	//pageable：分页参数
	//返回值：分页pageBean（page：是springdatajpa提供的）
	Page<T> findAll(Specification<T> spec, Pageable pageable);

	//查询列表
	//Sort：排序参数
	List<T> findAll(Specification<T> spec, Sort sort);

	long count(Specification<T> spec);//统计查询
	
* Specification ：查询条件
	自定义我们自己的Specification实现类
		实现
			//root：查询的根对象（查询的任何属性都可以从根对象中获取）
			//CriteriaQuery：顶层查询对象，自定义查询方式（了解：一般不用）
			//CriteriaBuilder：查询的构造器，封装了很多的查询条件
			Predicate toPredicate(Root<T> root, CriteriaQuery<?> query, CriteriaBuilder cb); //封装查询条件
```
```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:applicationContext.xml")
public class SpecTest {

    @Autowired
    private CustomerDao customerDao;

    /**
     * 根据条件，查询单个对象
     *
     */
    @Test
    public void testSpec() {
        //匿名内部类
        /**
         * 自定义查询条件
         *      1.实现Specification接口（提供泛型：查询的对象类型）
         *      2.实现toPredicate方法（构造查询条件）
         *      3.需要借助方法参数中的两个参数（
         *          root：获取需要查询的对象属性
         *          CriteriaBuilder：构造查询条件的，内部封装了很多的查询条件（模糊匹配，精准匹配）
         *       ）
         *  案例：根据客户名称查询，查询客户名为传智播客的客户
         *          查询条件
         *              1.查询方式
         *                  cb对象
         *              2.比较的属性名称
         *                  root对象
         *
         */
        Specification<Customer> spec = new Specification<Customer>() {
            @Override
            public Predicate toPredicate(Root<Customer> root, CriteriaQuery<?> query, CriteriaBuilder cb) {
                //1.获取比较的属性
                Path<Object> custName = root.get("custId");
                //2.构造查询条件  ：    select * from cst_customer where cust_name = '传智播客'
                /**
                 * 第一个参数：需要比较的属性（path对象）
                 * 第二个参数：当前需要比较的取值
                 */
                Predicate predicate = cb.equal(custName, "传智播客");//进行精准的匹配  （比较的属性，比较的属性的取值）
                return predicate;
            }
        };
        Customer customer = customerDao.findOne(spec);
        System.out.println(customer);
    }

    /**
     * 多条件查询
     *      案例：根据客户名（传智播客）和客户所属行业查询（it教育）
     *
     */
    @Test
    public void testSpec1() {
        /**
         *  root:获取属性
         *      客户名
         *      所属行业
         *  cb：构造查询
         *      1.构造客户名的精准匹配查询
         *      2.构造所属行业的精准匹配查询
         *      3.将以上两个查询联系起来
         */
        Specification<Customer> spec = new Specification<Customer>() {
            @Override
            public Predicate toPredicate(Root<Customer> root, CriteriaQuery<?> query, CriteriaBuilder cb) {
                Path<Object> custName = root.get("custName");//客户名
                Path<Object> custIndustry = root.get("custIndustry");//所属行业

                //构造查询
                //1.构造客户名的精准匹配查询
                Predicate p1 = cb.equal(custName, "传智播客");//第一个参数，path（属性），第二个参数，属性的取值
                //2..构造所属行业的精准匹配查询
                Predicate p2 = cb.equal(custIndustry, "it教育");
                //3.将多个查询条件组合到一起：组合（满足条件一并且满足条件二：与关系，满足条件一或满足条件二即可：或关系）
                Predicate and = cb.and(p1, p2);//以与的形式拼接多个查询条件
                // cb.or();//以或的形式拼接多个查询条件
                return and;
            }
        };
        Customer customer = customerDao.findOne(spec);
        System.out.println(customer);
    }


    /**
     * 案例：完成根据客户名称的模糊匹配，返回客户列表
     *      客户名称以 ’传智播客‘ 开头
     *
     * equal ：直接的到path对象（属性），然后进行比较即可
     * gt，lt,ge,le,like : 得到path对象，根据path指定比较的参数类型，再去进行比较
     *      指定参数类型：path.as(类型的字节码对象)
     */
    @Test
    public void testSpec3() {
        //构造查询条件
        Specification<Customer> spec = new Specification<Customer>() {
            @Override
            public Predicate toPredicate(Root<Customer> root, CriteriaQuery<?> query, CriteriaBuilder cb) {
                //查询属性：客户名
                Path<Object> custName = root.get("custName");
                //查询方式：模糊匹配
                Predicate like = cb.like(custName.as(String.class), "传智播客%");
                return like;
            }
        };
//        List<Customer> list = customerDao.findAll(spec);
//        for (Customer customer : list) {
//            System.out.println(customer);
//        }
        //添加排序
        //创建排序对象,需要调用构造方法实例化sort对象
        //第一个参数：排序的顺序（倒序，正序）
        //   Sort.Direction.DESC:倒序
        //   Sort.Direction.ASC ： 升序
        //第二个参数：排序的属性名称
        Sort sort = new Sort(Sort.Direction.DESC,"custId");
        List<Customer> list = customerDao.findAll(spec, sort);
        for (Customer customer : list) {
            System.out.println(customer);
        }
    }

    /**
     * 分页查询
     *      Specification: 查询条件
     *      Pageable：分页参数
     *          分页参数：查询的页码，每页查询的条数
     *          findAll(Specification,Pageable)：带有条件的分页
     *          findAll(Pageable)：没有条件的分页
     *  返回：Page（springDataJpa为我们封装好的pageBean对象，数据列表，共条数）
     */
    @Test
    public void testSpec4() {

        Specification spec = null;
        //PageRequest对象是Pageable接口的实现类
        /**
         * 创建PageRequest的过程中，需要调用他的构造方法传入两个参数
         *      第一个参数：当前查询的页数（从0开始）
         *      第二个参数：每页查询的数量
         */
        Pageable pageable = new PageRequest(0,2);
        //分页查询
        Page<Customer> page = customerDao.findAll(null, pageable);
        System.out.println(page.getContent()); //得到数据集合列表
        System.out.println(page.getTotalElements());//得到总条数
        System.out.println(page.getTotalPages());//得到总页数
    }
}
```

#### 方法对应关系

| 方法名称                    | Sql对应关系            |
| --------------------------- | ---------------------- |
| equle                       | filed =   value        |
| gt（greaterThan ）          | filed   > value        |
| lt（lessThan ）             | filed   < value        |
| ge（greaterThanOrEqualTo ） | filed   >= value       |
| le（ lessThanOrEqualTo）    | filed   <= value       |
| notEqule                    | filed   != value       |
| like                        | filed   like value     |
| notLike                     | filed   not like value |

### 第五章 多表设计

#### 5.1 表关系

- 一对一

- 一对多：

  > 一：主表
  > 多：从表
  > 外键：需要再从表上新建一列作为外键，他的取值来源于主表的主键	

- 多对多：

  > 中间表：中间表中最少应该由两个字段组成，这两个字段做为外键指向两张表的主键，又组成了联合主键

#### 5.2 实体类中的关系

- 包含关系：可以通过实体类中的包含关系描述表关系
- 继承关系

#### 5.3 分析步骤

1. 明确表关系
2. 确定表关系（描述外键|中间表）
3. 编写实体类，再实体类中描述表关系（包含关系）
4. 配置映射关系

### 第六章 JPA中的一对多

#### 6.1 实例分析

 客户：指的是一家公司，我们记为A。

 联系人：指的是A公司中的员工。

 在不考虑兼职的情况下，公司和员工的关系即为一对多。

#### 6.2 表关系建立

#### 6.3 实体类关系建立以及映射配置

```java
/**
 * 1.实体类和表的映射关系
 *      @Eitity
 *      @Table
 * 2.类中属性和表中字段的映射关系
 *      @Id
 *      @GeneratedValue
 *      @Column
 */
@Entity
@Table(name="cst_customer")
public class Customer {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name="cust_id")
    private Long custId;
    @Column(name="cust_address")
    private String custAddress;
    @Column(name="cust_industry")
    private String custIndustry;
    @Column(name="cust_level")
    private String custLevel;
    @Column(name="cust_name")
    private String custName;
    @Column(name="cust_phone")
    private String custPhone;
    @Column(name="cust_source")
    private String custSource;

    //配置客户和联系人之间的关系（一对多关系）
    /**
     * 使用注解的形式配置多表关系
     *      1.声明关系
     *          @OneToMany : 配置一对多关系
     *              targetEntity ：对方对象的字节码对象
     *      2.配置外键（中间表）
     *              @JoinColumn : 配置外键
     *                  name：外键字段名称
     *                  referencedColumnName：参照的主表的主键字段名称
     *
     *  * 在客户实体类上（一的一方）添加了外键了配置，所以对于客户而言，也具备了维护外键的作用
     *
     */

//    @OneToMany(targetEntity = LinkMan.class)
//    @JoinColumn(name = "lkm_cust_id",referencedColumnName = "cust_id")
    /**
     * 放弃外键维护权
     *      mappedBy：对方配置关系的属性名称
     * cascade : 配置级联（可以配置到设置多表的映射关系的注解上）
     *      CascadeType.all         : 所有
     *                  MERGE       ：更新
     *                  PERSIST     ：保存
     *                  REMOVE      ：删除
     *
     * fetch : 配置关联对象的加载方式
     *          EAGER   ：立即加载
     *          LAZY    ：延迟加载

      */
    @OneToMany(mappedBy = "customer",cascade = CascadeType.ALL)
    private Set<LinkMan> linkMans = new HashSet<>();

```

```java
@Entity
@Table(name = "cst_linkman")
public class LinkMan {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "lkm_id")
    private Long lkmId; //联系人编号(主键)
    @Column(name = "lkm_name")
    private String lkmName;//联系人姓名
    @Column(name = "lkm_gender")
    private String lkmGender;//联系人性别
    @Column(name = "lkm_phone")
    private String lkmPhone;//联系人办公电话
    @Column(name = "lkm_mobile")
    private String lkmMobile;//联系人手机
    @Column(name = "lkm_email")
    private String lkmEmail;//联系人邮箱
    @Column(name = "lkm_position")
    private String lkmPosition;//联系人职位
    @Column(name = "lkm_memo")
    private String lkmMemo;//联系人备注

    /**
     * 配置联系人到客户的多对一关系
     *     使用注解的形式配置多对一关系
     *      1.配置表关系
     *          @ManyToOne : 配置多对一关系
     *              targetEntity：对方的实体类字节码
     *      2.配置外键（中间表）
     *
     * * 配置外键的过程，配置到了多的一方，就会在多的一方维护外键
     *
     */
    @ManyToOne(targetEntity = Customer.class,fetch = FetchType.LAZY)
    @JoinColumn(name = "lkm_cust_id",referencedColumnName = "cust_id")
    private Customer customer;
```

> **@OneToMany:**
>
> ​     作用：建立一对多的关系映射
>
> ​    属性：
>
> ​            targetEntityClass：指定多的多方的类的字节码
>
> ​            mappedBy：指定从表实体类中引用主表对象的名称。
>
> ​            cascade：指定要使用的级联操作
>
> ​            fetch：指定是否采用延迟加载
>
> ​            orphanRemoval：是否使用孤儿删除
>
> **@ManyToOne**
>
> ​    作用：建立多对一的关系
>
> ​    属性：
>
> ​            targetEntityClass：指定一的一方实体类字节码
>
> ​            cascade：指定要使用的级联操作
>
> ​            fetch：指定是否采用延迟加载
>
> ​            optional：关联是否可选。如果设置为false，则必须始终存在非空关系。
>
> **@JoinColumn**
>
> ​     作用：用于定义主键字段和外键字段的对应关系。
>
> ​     属性：
>
> ​            name：指定外键字段的名称
>
> ​            referencedColumnName：指定引用主表的主键字段名称
>
> ​            unique：是否唯一。默认值不唯一
>
> ​            nullable：是否允许为空。默认值允许。
>
> ​            insertable：是否允许插入。默认值允许。
>
> ​            updatable：是否允许更新。默认值允许。
>
> ​            columnDefinition：列的定义信息。

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:applicationContext.xml")
public class OneToManyTest {

    @Autowired
    private CustomerDao customerDao;

    @Autowired
    private LinkManDao linkManDao;

    /**
     * 保存一个客户，保存一个联系人
     *  效果：客户和联系人作为独立的数据保存到数据库中
     *      联系人的外键为空
     *  原因？
     *      实体类中没有配置关系
     */
    @Test
    @Transactional //配置事务
    @Rollback(false) //不自动回滚
    public void testAdd() {
        //创建一个客户，创建一个联系人
        Customer customer = new Customer();
        customer.setCustName("百度");

        LinkMan linkMan = new LinkMan();
        linkMan.setLkmName("小李");

        /**
         * 配置了客户到联系人的关系
         *      从客户的角度上：发送两条insert语句，发送一条更新语句更新数据库（更新外键）
         * 由于我们配置了客户到联系人的关系：客户可以对外键进行维护
         */
        customer.getLinkMans().add(linkMan);
        customerDao.save(customer);
        linkManDao.save(linkMan);
    }


    @Test
    @Transactional //配置事务
    @Rollback(false) //不自动回滚
    public void testAdd1() {
        //创建一个客户，创建一个联系人
        Customer customer = new Customer();
        customer.setCustName("百度");

        LinkMan linkMan = new LinkMan();
        linkMan.setLkmName("小李");

        /**
         * 配置联系人到客户的关系（多对一）
         *    只发送了两条insert语句
         * 由于配置了联系人到客户的映射关系（多对一）
         *
         *
         */
        linkMan.setCustomer(customer);

        customerDao.save(customer);
        linkManDao.save(linkMan);
    }

    /**
     * 会有一条多余的update语句
     *      * 由于一的一方可以维护外键：会发送update语句
     *      * 解决此问题：只需要在一的一方放弃维护权即可
     *
     */
    @Test
    @Transactional //配置事务
    @Rollback(false) //不自动回滚
    public void testAdd2() {
        //创建一个客户，创建一个联系人
        Customer customer = new Customer();
        customer.setCustName("百度");

        LinkMan linkMan = new LinkMan();
        linkMan.setLkmName("小李");


        linkMan.setCustomer(customer);//由于配置了多的一方到一的一方的关联关系（当保存的时候，就已经对外键赋值）
        customer.getLinkMans().add(linkMan);//由于配置了一的一方到多的一方的关联关系（发送一条update语句）

        customerDao.save(customer);
        linkManDao.save(linkMan);
    }

    /**
     * 级联添加：保存一个客户的同时，保存客户的所有联系人
     *      需要在操作主体的实体类上，配置casacde属性
     */
    @Test
    @Transactional //配置事务
    @Rollback(false) //不自动回滚
    public void testCascadeAdd() {
        Customer customer = new Customer();
        customer.setCustName("百度1");

        LinkMan linkMan = new LinkMan();
        linkMan.setLkmName("小李1");

        linkMan.setCustomer(customer);
        customer.getLinkMans().add(linkMan);

        customerDao.save(customer);
    }


    /**
     * 级联删除：
     *      删除1号客户的同时，删除1号客户的所有联系人
     */
    @Test
    @Transactional //配置事务
    @Rollback(false) //不自动回滚
    public void testCascadeRemove() {
        //1.查询1号客户
        Customer customer = customerDao.findOne(1l);
        //2.删除1号客户
        customerDao.delete(customer);
    }
}
```

### 第七章 JPA中的一对多

#### 7.1 示例分析

#### 7.2 表关系建立

#### 7.3 实体类关系建立以及映射配置

```java
@Entity
@Table(name = "sys_user")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name="user_id")
    private Long userId;
    @Column(name="user_name")
    private String userName;
    @Column(name="age")
    private Integer age;

    /**
     * 配置用户到角色的多对多关系
     *      配置多对多的映射关系
     *          1.声明表关系的配置
     *              @ManyToMany(targetEntity = Role.class)  //多对多
     *                  targetEntity：代表对方的实体类字节码
     *          2.配置中间表（包含两个外键）
     *                @JoinTable
     *                  name : 中间表的名称
     *                  joinColumns：配置当前对象在中间表的外键
     *                      @JoinColumn的数组
     *                          name：外键名
     *                          referencedColumnName：参照的主表的主键名
     *                  inverseJoinColumns：配置对方对象在中间表的外键
     */
    @ManyToMany(targetEntity = Role.class,cascade = CascadeType.ALL)
    @JoinTable(name = "sys_user_role",
            //joinColumns,当前对象在中间表中的外键
            joinColumns = {@JoinColumn(name = "sys_user_id",referencedColumnName = "user_id")},
            //inverseJoinColumns，对方对象在中间表的外键
            inverseJoinColumns = {@JoinColumn(name = "sys_role_id",referencedColumnName = "role_id")}
    )
    private Set<Role> roles = new HashSet<Role>();
```

```java
@Entity
@Table(name = "sys_role")
public class Role {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "role_id")
    private Long roleId;
    @Column(name = "role_name")
    private String roleName;

    //配置多对多
    @ManyToMany(mappedBy = "roles")  //配置多表关系
    private Set<User> users = new HashSet<User>();
```

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:applicationContext.xml")
public class ManyToManyTest {

    @Autowired
    private UserDao userDao;
    @Autowired
    private RoleDao roleDao;

    /**
     * 保存一个用户，保存一个角色
     *
     *  多对多放弃维护权：被动的一方放弃
     */
    @Test
    @Transactional
    @Rollback(false)
    public void  testAdd() {
        User user = new User();
        user.setUserName("小李");

        Role role = new Role();
        role.setRoleName("java程序员");

        //配置用户到角色关系，可以对中间表中的数据进行维护     1-1
        user.getRoles().add(role);

        //配置角色到用户的关系，可以对中间表的数据进行维护     1-1
        //在保存时，会出现主键重复的错误，因为都是要往中间表中保存数据造成的
        role.getUsers().add(user);

        userDao.save(user);
        roleDao.save(role);
    }


    //测试级联添加（保存一个用户的同时保存用户的关联角色）
    @Test
    @Transactional
    @Rollback(false)
    public void  testCasCadeAdd() {
        User user = new User();
        user.setUserName("小李");

        Role role = new Role();
        role.setRoleName("java程序员");

        //配置用户到角色关系，可以对中间表中的数据进行维护     1-1
        user.getRoles().add(role);

        //配置角色到用户的关系，可以对中间表的数据进行维护     1-1
        role.getUsers().add(user);

        userDao.save(user);
    }

    /**
     * 案例：删除id为1的用户，同时删除他的关联对象
     */
    @Test
    @Transactional
    @Rollback(false)
    public void  testCasCadeRemove() {
        //查询1号用户
        User user = userDao.findOne(1l);
        //删除1号用户
        userDao.delete(user);

    }
}
/*
	在多对多（保存）中，如果双向都设置关系，意味着双方都维护中间表，都会往中间表插	入数据，中间表的2个字段又作为联合主键，所以报错，主键重复，解决保存失败的问		题：只需要在任意一方放弃对中间表的维护权即可，推荐在被动的一方放弃
	//放弃对中间表的维护权，解决保存中主键冲突的问题
	@ManyToMany(mappedBy="roles")
	private Set<User> users = new HashSet<User>();
*/
```

> **@ManyToMany**
>
> ​        作用：用于映射多对多关系
>
> ​        属性：
>
> ​                 cascade：配置级联操作。
>
> ​                 fetch：配置是否采用延迟加载。
>
> ​            targetEntity：配置目标的实体类。映射多对多的时候不用写。
>
> **@JoinTable**
>
> ​    作用：针对中间表的配置
>
> ​    属性：
>
> ​            nam：配置中间表的名称
>
> ​            joinColumns：中间表的外键字段关联当前实体类所对应表的主键字段                                                
>
> ​            inverseJoinColumn：中间表的外键字段关联对方表的主键字段           
>
> **@JoinColumn**
>
> ​    作用：用于定义主键字段和外键字段的对应关系。
>
> ​    属性：
>
> ​            name：指定外键字段的名称
>
> ​            referencedColumnName：指定引用主表的主键字段名称
>
> ​            unique：是否唯一。默认值不唯一
>
> ​            nullable：是否允许为空。默认值允许。
>
> ​            insertable：是否允许插入。默认值允许。
>
> ​            updatable：是否允许更新。默认值允许。
>
> ​            columnDefinition：列的定义信息。

### 第八章 Spring Data JPA中的多表查询

#### 8.1 对象导航查询

对象图导航检索方式是根据已经加载的对象，导航到他的关联对象。它利用类与类之间的关系来检索对象。例如：我们通过ID查询方式查出一个客户，可以调用Customer类中的getLinkMans()方法来获取该客户的所有联系人。对象导航查询的使用要求是：两个对象之间必须存在关联关系。

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:applicationContext.xml")
public class ObjectQueryTest {
    @Autowired
    private CustomerDao customerDao;

    @Autowired
    private LinkManDao linkManDao;

    //could not initialize proxy - no Session
    //测试对象导航查询（查询一个对象的时候，通过此对象查询所有的关联对象）
    @Test
    @Transactional // 解决在java代码中的no session问题
    public void  testQuery1() {
        //查询id为1的客户
        Customer customer = customerDao.getOne(1l);
        //对象导航查询，此客户下的所有联系人
        Set<LinkMan> linkMans = customer.getLinkMans();

        for (LinkMan linkMan : linkMans) {
            System.out.println(linkMan);
        }
    }

    /**
     * 对象导航查询：
     *      默认使用的是延迟加载的形式查询的
     *          调用get方法并不会立即发送查询，而是在使用关联对象的时候才会差和讯
     *      延迟加载！
     * 修改配置，将延迟加载改为立即加载
     *      fetch，需要配置到多表映射关系的注解上
     */

    @Test
    @Transactional // 解决在java代码中的no session问题
    public void  testQuery2() {
        //查询id为1的客户
        Customer customer = customerDao.findOne(1l);
        //对象导航查询，此客户下的所有联系人
        Set<LinkMan> linkMans = customer.getLinkMans();

        System.out.println(linkMans.size());
    }

    /**
     * 从联系人对象导航查询他的所属客户
     *      * 默认 ： 立即加载
     *  延迟加载：
     */
    @Test
    @Transactional // 解决在java代码中的no session问题
    public void  testQuery3() {
        LinkMan linkMan = linkManDao.findOne(2l);
        //对象导航查询所属的客户
        Customer customer = linkMan.getCustomer();
        System.out.println(customer);
    }

}
```

#### 8.2 使用Specification查询