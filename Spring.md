## Spring

### 第一章 Spring 概述 

#### 1.1 spring概述

Spring 是分层的 Java SE/EE 应用 full-stack 轻量级开源框架，以 IoC（Inverse Of Control：
反转控制）和 AOP（Aspect Oriented Programming：面向切面编程）为内核，提供了展现层 Spring
MVC 和持久层 Spring JDBC 以及业务层事务管理等众多的企业级应用技术，还能整合开源世界众多
著名的第三方框架和类库，逐渐成为使用最多的 Java EE 企业应用开源框架。 

#### 1.2 spring优势

- 方便解耦，简化开发
  通过 Spring 提供的 IoC 容器，可以将对象间的依赖关系交由 Spring 进行控制，避免硬编码所造
  成的过度程序耦合。用户也不必再为单例模式类、属性文件解析等这些很底层的需求编写代码，可
  以更专注于上层的应用。
- AOP 编程的支持
  通过 Spring 的 AOP 功能，方便进行面向切面的编程，许多不容易用传统 OOP 实现的功能可以 通过 AOP 轻松应付。
- 声明式事务的支持
  可以将我们从单调烦闷的事务管理代码中解脱出来，通过声明式方式灵活的进行事务的管理，
  提高开发效率和质量。
- 方便程序的测试
  可以用非容器依赖的编程方式进行几乎所有的测试工作，测试不再是昂贵的操作，而是随手可
  做的事情。
- 方便集成各种优秀框架
  Spring 可以降低各种框架的使用难度，提供了对各种优秀框架（Struts、Hibernate、Hessian、 Quartz等）的直接支持。
- 降低 JavaEE API 的使用难度
  Spring 对 JavaEE API（如 JDBC、 JavaMail、远程调用等）进行了薄薄的封装层，使这些 API 的
  使用难度大为降低。
- Java 源码是经典学习范例
  Spring 的源代码设计精妙、结构清晰、匠心独用，处处体现着大师对 Java 设计模式灵活运用以
  及对 Java 技术的高深造诣。它的源代码无意是 Java 技术的最佳实践的范例 

#### 1.3 spring 的体系结构 

<img src="C:\Users\wang1\Desktop\mdSource\spring\Snipaste_2020-01-15_17-35-46.png" style="zoom:67%;" />

### 第二章 Ioc的概念和作用

#### 2.1 程序的耦合 

耦合性(Coupling)，也叫耦合度，是对模块间关联程度的度量。耦合的强弱取决于模块间接口的复杂性、调用模块的方式以及通过界面传送数据的多少。模块间的耦合度是指模块之间的依赖关系，包括控制关系、调用关系、数据传递关系。 

#### 2.2 解决程序耦合的思路 



#### 2.3 工厂模式解耦 

在实际开发中我们可以把三层的对象都使用配置文件配置起来，当启动服务器应用加载的时候， 让一个类中的方法通过读取配置文件，把这些对象创建出来并存起来。在接下来的使用的时候，直接拿过来用就好了。那么，这个读取配置文件， 创建和获取三层对象的类就是工厂 

#### 2.4 控制反转-Inversion Of Control 

控制反转把创建对象的权利交给框架，是框架的重要特征，并非面向对象编程的专用术语，它包括依赖注入（Dependency，DI）和依赖查找（Dependency Lookup）

> 工厂就是负责给我们从容器中获取指定对象的类。这时候我们获取对象的方式发生了改变。
>
> 原来：我们在获取对象时，都是采用 new 的方式。 是主动的。 
>
> 现在：我们获取对象时，同时跟工厂要，有工厂为我们查找或者创建对象。 是被动的 

#### 2.5 作用

降低计算机程序的耦合性(解除我们代码中的依赖关系) 

### 第三章 Ioc解决程序耦合

#### 3.1 Spring 基于 XML 的 IOC  

1. ##### pom.xml

   ```xml
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-context</artifactId>
       <version>5.0.2.RELEASE</version>
   </dependency>
   ```

2. ##### bean.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:jee="http://www.springframework.org/schema/jee"
          xsi:schemaLocation="
           http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/jee
           http://www.springframework.org/schema/jee/spring-jee.xsd">
   
       <!--把对象的创建交给spring来管理-->
       <bean id="accountService" class="com.wmxyyy.service.imp.AccountServiceImpl"></bean>
       <bean id="accountDao" class="com.wmxyyy.dao.impl.AccountDaoImpl"></bean>
       
   </beans>
   ```

3. ##### 测试类

   ```java
   public static void main(String[] args) {
           //1.获取核心容器对象
           ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
           //2.根据id获取Bean对象
           IAccountService as = (IAccountService) ac.getBean("accountService");
           IAccountDao ad = ac.getBean("accountDao", IAccountDao.class);
   
           System.out.println(as);
           System.out.println(ad);
           as.saveAccount();
       
           Resource resource = new ClassPathResource("bean.xml");
           BeanFactory factory = new XmlBeanFactory(resource);
           IAccountService as = (IAccountService) 				factory.getBean("accountService");
           System.out.println(as);
       }
   ```

   > BeanFactory 才是 Spring 容器中的顶层接口。
   > ApplicationContext 是它的子接口。
   >
   > BeanFactory 和 ApplicationContext 的区别：创建对象的时间点不一样。
   > ApplicationContext：在构建核心容器时，创建对象采取的策略时采用立即加载的方式
   > BeanFactory：创建对象采取的策略时采用延迟加载的方法
   >
   > 
   >
   > ApplicationContext的三个常用实现类：   
   >
   > - ClassPathXmlApplicationContext（常用）:加载类类路径下的配置文件
   >
   > - FileSystemXmlApplication：可以加载磁盘路径下的配置文件（访问权限）
   >
   > - AnnotationConfigApplicationContext：用于读取注解创建容器

   

#### 3.2 IOC 中 bean 标签和管理对象细节 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:jee="http://www.springframework.org/schema/jee"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/jee
        http://www.springframework.org/schema/jee/spring-jee.xsd">

    <!--把对象的创建交给spring来管理-->
    <!--创建Bean的三种方式-->

    <!--第一种方式：使用默认构造函数创建
        在spring的配置文件中使用bean标签，里面只含有id和class属性采用就是默认函数构造方法
    -->
    <bean id="accountService" class="com.wmxyyy.service.imp.AccountServiceImpl"></bean>

    <!--第二种方式：使用工厂中的方法创建对象（使用某个类中的方法创建对象，存入spring容器）-->
    <bean id="instanceFactory" class="com.wmxyyy.factory.InstanceFactory"></bean>
    <bean id="accountService" factory-bean="instanceFactory" factory-method="getAccountService"></bean>

    <!--第三种方式：使用工厂中的静态方法创建对象（使用某个类中的静态方法创建对象，存入spring容器）-->
    <bean id="accountService" class="com.wmxyyy.factory.StaticFactory" factory-method="getAccountService"></bean>

    <!-- bean的作用范围调整
         bean标签的scope属性：
            作用：用于指定bean的作用范围
            取值：常用的就是单例和多例
                singleton：单例
                prototype：多例
                request：作用于web应用的请求范围
                session：作用于web应用的会话范围
                global-session：作用于集群环境的会话范围（全局会话范围），当不是集群时它就是session
    -->
    <!--bean对象的声明周期
        单例对象
            出生：当容器创建时对象出生
            活着：只要容器还在，对象一直活着
            死亡：容器销毁，对象消亡
            总结：单例对象的声明周期和容器相同

        多例对象
            出生：当我们使用对象时spring框架为我们创建
            活着：对象只要是在使用过程中就一直活着
            死亡：当对象长时间不用，且没有别的对象引用时，由gc回收
    -->
    <bean id="accountService" class="com.wmxyyy.service.imp.AccountServiceImpl"
          scope="prototype" init-method="init" destroy-method="destroy"></bean>

</beans>

```

#### 3.3 spring 的依赖注入（DI）

1. ##### 概念

   依赖注入： Dependency Injection。 它是 spring 框架核心 ioc 的具体实现。

   > 我们的程序在编写时通过控制反转把对象的创建交给了 spring，但是代码中不可能出现没有依赖的情况。Ioc 解耦只是降低他们的依赖关系，但不会消除。 例如：我们的业务层仍会调用持久层的方法。
   > 那这种业务层和持久层的依赖关系， 在使用 spring 之后， 就让 spring 来维护了。简单的说，就是坐等框架把持久层对象传入业务层，而不用我们自己去获取 
   >
   > ```xml
   > <!-- spring的依赖注入 Dependency Injection
   >       - IOC的作用是降低程序间的耦合（依赖关系）
   >       - 依赖关系的管理交给spring维护，依赖关系的维护称之为依赖注入
   >       - 在当前类需要用到其他类的对象由spring为我们提供，我们只需要在配置文件中说明
   > 
   >       - 能注入的数据有三类
   >             基本类型和String
   >             其他bean类型（在配置文件中或者注解配置过的bean）
   >             复杂类型/集合类型
   >       - 注入的方式有三种
   >             - 使用构造函数提供
   >             - 使用set方法提供
   >             - 使用注解提供
   > -->
   > ```

2. ##### 构造函数注入 

   ```java
   public class AccountServiceImpl implements IAccountService {
       //如果是经常变化的数据，并不适用于注入的方式
       private String name;
       private Integer age;
       private Date birthday;
   
       public AccountServiceImpl(String name, Integer age, Date birthday) {
           this.name = name;
           this.age = age;
           this.birthday = birthday;
       }
       public void saveAccount() {
           System.out.println("service中的saveAccount方法执行了..." + name + age + birthday);
       }
   }
   ```

   ```xml
       <!--构造函数注入
           使用标签：constructor-arg
           标签属性：
                type：指定参数在构造函数中的数据类型
                index：指定参数在构造函数参数列表的索引位置
                name：用于指定给构造函数中指定名称的参数赋值
                =======================================
                value：用于提供基本类型和String类型的数据
                ref：用于指定其他的bean类型数据
           优势：
               在获取bean对象时注入数据时必须的操作，否则对象无法创建成功
           弊端：
               改变bean对象的实例化方法使我们在创建对象时如果用不到的数据，也必须提供
       -->
       <bean id="accountService" class="com.wmxyyy.service.imp.AccountServiceImpl">
           <constructor-arg name="name" value="王明"></constructor-arg>
           <constructor-arg name="age" value="15"></constructor-arg>
           <constructor-arg name="birthday" ref="now"></constructor-arg>
       </bean>
   
       <bean id="now" class="java.util.Date"></bean>
   ```

3. ##### set 方法注入 

   ```java
   public class AccountServiceImpl2 implements IAccountService {
       //如果是经常变化的数据，并不适用于注入的方式
       private String name;
       private Integer age;
       private Date birthday;
   
       public void setName(String name) {
           this.name = name;
       }
   
       public void setAge(Integer age) {
           this.age = age;
       }
   
       public void setBirthday(Date birthday) {
           this.birthday = birthday;
       }
   
       public void saveAccount() {
           System.out.println("service中的saveAccount方法执行了..." + name + age + birthday);
       }
   }
   ```

   ```xml
   <!--set方法注入 （常用方法）
           使用标签：property
           标签属性：
               name:用于指定注入时所调用的set方法名称
               value：用于提供基本类型和String类型的数据
               ref：用于指定其他的bean类型数据。
           优势：
               创建对象时没有明确的限制，可以直接使用默认的构造函数
           弊端：
               如果有某成员必须有值，则获取对象是有可能set方法没有执行
       -->
       <bean id="accountService2" class="com.wmxyyy.service.imp.AccountServiceImpl2">
           <property name="name" value="wmxyyy"></property>
           <property name="age" value="32"></property>
           <property name="birthday" ref="now"></property>
       </bean>
   
   	<bean id="now" class="java.util.Date"></bean>
   ```

   

4. ##### 注入集合属性 

   ```java
   public class AccountServiceImpl3 implements IAccountService {
   
       private String[] myStrs;
       private List<String> myList;
       private Set<String> mySet;
       private Map<String,String> myMap;
       private Properties myProps;
   
       public String[] getMyStrs() {
           return myStrs;
       }
   
       public void setMyStrs(String[] myStrs) {
           this.myStrs = myStrs;
       }
   
       public List<String> getMyList() {
           return myList;
       }
   
       public void setMyList(List<String> myList) {
           this.myList = myList;
       }
   
       public Set<String> getMySet() {
           return mySet;
       }
   
       public void setMySet(Set<String> mySet) {
           this.mySet = mySet;
       }
   
       public Map<String, String> getMyMap() {
           return myMap;
       }
   
       public void setMyMap(Map<String, String> myMap) {
           this.myMap = myMap;
       }
   
       public Properties getMyProps() {
           return myProps;
       }
   
       public void setMyProps(Properties myProps) {
           this.myProps = myProps;
       }
   
       public void saveAccount() {
           System.out.println(Arrays.toString(myStrs));
           System.out.println(myList);
           System.out.println(mySet);
           System.out.println(myMap);
           System.out.println(myProps);
       }
   
   }
   ```

   ```xml
   <!--复杂类型的注入/集合类型的注入
           - 用于给List结构集合注入的标签
               list array set
   
           - 用于给map结构集合注入的标签
               map props
   
           - 结构相同，标签可以互换
       -->
       <bean id="accountService3" class="com.wmxyyy.service.imp.AccountServiceImpl3">
           <property name="myStrs">
               <array>
                   <value>AAA</value>
                   <value>BBB</value>
                   <value>CCC</value>
               </array>
           </property>
           <property name="myList">
               <list>
                   <value>DDD</value>
                   <value>EEE</value>
                   <value>FFF</value>
               </list>
           </property>
           <property name="mySet">
               <set>
                   <value>GGG</value>
                   <value>HHH</value>
                   <value>III</value>
               </set>
           </property>
           <property name="myMap"> 
               <map>
                   <entry key="key1" value="aaa"></entry>
                   <entry key="key2">
                       <value>bbb</value>
                   </entry>
               </map>
           </property>
           <property name="myProps">
               <props>
                   <prop key="pro1">ccc</prop>
                   <prop key="pro2">ddd</prop>
               </props>
           </property>
       </bean>
   ```

#### 3.4 基于XML的 IoC 的实现账户的CRUD 

1. ##### pom.xml

   ```xml
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-context</artifactId>
       <version>5.0.2.RELEASE</version>
   </dependency>
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>5.1.6</version>
   </dependency>
   <dependency>
       <groupId>commons-dbutils</groupId>
       <artifactId>commons-dbutils</artifactId>
       <version>1.4</version>
   </dependency>
   <dependency>
       <groupId>c3p0</groupId>
       <artifactId>c3p0</artifactId>
       <version>0.9.1.2</version>
   </dependency>
   <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.12</version>
   </dependency>
   ```

2. ##### 业务层实现类

   ```java
   public class AccountServiceImpl implements IAccountService{
       private IAccountDao accountDao;
   
       public void setAccountDao(IAccountDao accountDao) {
           this.accountDao = accountDao;
       }
   
       public List<Account> findAllAccount() {
           return accountDao.findAllAccount();
       }
       public Account findAccountById(Integer accountId) {
           return accountDao.findAccountById(accountId);
       }
       public void saveAccount(Account account) {
           accountDao.saveAccount(account);
       }
       public void updateAccount(Account account) {
           accountDao.updateAccount(account);
       }
       public void deleteAccount(Integer accountId) {
           accountDao.deleteAccount(accountId);
       }
   }
   ```

3. ##### 持久层实现类

   ```java
   public class AccountDaoImpl implements IAccountDao {
       private QueryRunner runner;
   
       public void setRunner(QueryRunner runner) {
           this.runner = runner;
       }
   
       public List<Account> findAllAccount() {
           try {
               return runner.query("select * from account", new BeanListHandler<Account>(Account.class));
           } catch (Exception e) {
               throw new RuntimeException(e);
           }
       }
   
       public Account findAccountById(Integer accountId) {
           try {
               return runner.query("select * from account where id = ?", new BeanHandler<Account>(Account.class), accountId);
           } catch (Exception e) {
               throw new RuntimeException(e);
           }
       }
   
       public void saveAccount(Account account) {
           try {
               runner.update("insert into account(name,money) values (?,?)",account.getName(),account.getMoney());
           } catch (Exception e) {
               throw new RuntimeException(e);
           }
       }
   
       public void updateAccount(Account account) {
           try {
               runner.update("update account set name=?, money=? where id=? ",account.getName(),account.getMoney(),account.getId());
           } catch (Exception e) {
               throw new RuntimeException(e);
           }
       }
   
       public void deleteAccount(Integer accountId) {
           try {
               runner.update("delete from account where id=?",accountId);
           } catch (Exception e) {
               throw new RuntimeException(e);
           }
       }
   }
   ```

4. ##### bean.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--配置Service对象-->
       <bean id="accountService" class="com.wmxyyy.service.impl.AccountServiceImpl">
           <!--注入dao对象-->
           <property name="accountDao" ref="accountDao"></property>
       </bean>
   
       <!--配置Dao对象-->
       <bean id="accountDao" class="com.wmxyyy.dao.impl.AccountDaoImpl">
           <!--注入QueryRunner-->
           <property name="runner" ref="runner"></property>
       </bean>
   
       <!--配置QueryRunner-->
       <bean id="runner" class="org.apache.commons.dbutils.QueryRunner" scope="prototype">
           <!--注入数据源-->
           <constructor-arg name="ds" ref="dataSource"></constructor-arg>
       </bean>
   
       <!--配置数据源-->
       <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
           <!--连接数据库的必备信息-->
           <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
           <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/eesy?characterEncoding=utf8"></property>
           <property name="user" value="root"></property>
           <property name="password" value="123456"></property>
       </bean>
   </beans>
   ```

#### 3.5 Spring 基于注解 的 IOC  

1. ##### bean.xml

   ```xml
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context.xsd">
   
       <!--告知spring在创建容器时要扫描的包，配置所需要的标签不是在beans的约束中，
       而是一个名称为context名称空间和约束中-->
       <context:component-scan base-package="com.wmxyyy"></context:component-scan>
   </beans>
   ```

2. ##### 持久层实现类

   ```java
   /**
    * @author wmxyyy
    * @date 2020/1/10 10:30
    * @state 账户的持久层实现类
    */
   @Repository("accountDao1")
   public class AccountDaoImpl1 implements IAccountDao {
       public void saveAccount() {
           System.out.println("保存了账户1111111111");
       }
   }
   
   @Repository("accountDao2")
   public class AccountDaoImpl2 implements IAccountDao {
       public void saveAccount() {
           System.out.println("保存了账户22222222222");
       }
   }
   ```

3. ##### 业务层实现类

   ```java
   /**
    * @author wmxyyy
    * @date 2020/1/10 10:28
    * @state 账户的业务层实现类
    * - 注解用于创建对象（相当于xml的<bean>标签）：
    *      Component:
    *          作用：用于把当前类对象存入spring容器中
    *          属性：
    *              value：用于指定bean的id。当不写value时默认是当前首字母小写类名
    *      ==========================
    *      Controller:一般用在表现层
    *      Service：一般用用在业务层
    *      Repository：一般用在持久层
    *      以上三个注解它们的作用和属性与Component时一样的，只是spring为我们提供明确的三层		使用的注解，使我们的三层对象更加清晰
   =============================================================================
    * - 注解用于注入数据（相当于xml的<bean>标签里的<property>标签）
    *      Autowired：
    *          作用：自动按照类型注入。只要容器中有唯一的一个bean对象类型和要注入的变量类型匹配就可以注入成功。
    *                - 如果ioc容器中没有任何bean的类型和要注入的变量类型匹配则报错
    *                - 如果ioc容器中有多个类型匹配时.......
    *           位置：变量或者方法
    *           细节：在使用注解注入时，set方法就不是必须的了
    *      Qualifier：
    *           作用：在按照类中注入的基础上再按照名称注入。它在给类成员注入时不能单独使用。
    *                  但是在给方法参数注入时可以
    *           属性：
    *              value：用于指定注入bean的id
    *      ==============================================
    *      Resource：
    *           作用：直接按照bean的id注入，它可以独立使用
    *           属性：
    *              name：用于指定bean的id
    *      - 以上三个注入只能注入其他bean类型的数据，而基本类型和String类型无法使用上述注解
    *      - 集合类型的注入只能通过XML来实现
    *
    *      Value
    *           作用：用于注入基本类型和String类型的数据
    *           属性：
    *              value：用于指定数据的值。它可以使用spring中SpEL
    *                      ${表达式}
   =============================================================================
    * - 注解用于改变作用范围（相当于xml的<bean>标签里的scope标签）
    *      Scope
    *           作用：用于指定bean的作用范围
    *           属性：
    *              value：指定范围的取值
    *                  - singleton
    *                  - prototype
    * =============================================================================
    * - 注解用于生命周期（相当于xml的<bean>标签使用init-method和destroy-method）
    *      PostConstruct：用于指定初始化方法
    *      PreDestroy：用于指定销毁方法
    */
   @Service("accountService")
   @Scope("prototype")
   public class AccountServiceImpl implements IAccountService {
       /*@Autowired
       @Qualifier("accountDao2")*/
       @Resource(name = "accountDao2")
       private IAccountDao accountDao = null;
   
       public AccountServiceImpl() {
           System.out.println("对象被创建");
       }
   
       @PostConstruct
       public void init(){
           System.out.println("初始化方法执行了");
       }
       @PreDestroy
       public void destroy(){
           System.out.println("销毁方法执行了");
       }
       public void saveAccount() {
           accountDao.saveAccount();
       }
   }
   ```

4. ##### 测试类

   ```java
   /**
    * @author wmxyyy
    * @date 2020/1/10 10:31
    * @state 模拟表现层，调用业务层
    */
   public class Client{
       public static void main(String[] args) {
           //1.获取核心容器对象
           ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
           //2.根据id获取Bean对象
           IAccountService as = (IAccountService) ac.getBean("accountService");
           IAccountDao ad1 = (IAccountDao) ac.getBean("accountDao1");
           IAccountDao ad2 = (IAccountDao) ac.getBean("accountDao2");
   
           System.out.println(as);
           System.out.println(ad1);
           System.out.println(ad2);
   
           as.saveAccount();
       }
   }
   ```

#### 3.4 基于注解的 IoC 的实现账户的CRUD 

1. 业务层实现类

   ```java
   @Service("accountService")
   public class AccountServiceImpl implements IAccountService{
       @Autowired
       private IAccountDao accountDao;
   
       public List<Account> findAllAccount() {
           return accountDao.findAllAccount();
       }
   
       public Account findAccountById(Integer accountId) {
           return accountDao.findAccountById(accountId);
       }
   
       public void saveAccount(Account account) {
           accountDao.saveAccount(account);
       }
   
       public void updateAccount(Account account) {
           accountDao.updateAccount(account);
       }
   
       public void deleteAccount(Integer accountId) {
           accountDao.deleteAccount(accountId);
       }
   }
   ```

2. 持久层实现类

   ```java
   @Repository("accountDao")
   public class AccountDaoImpl implements IAccountDao {
       @Autowired
       private QueryRunner runner;
   
       public List<Account> findAllAccount() {
           try {
               return runner.query("select * from account", new BeanListHandler<Account>(Account.class));
           } catch (Exception e) {
               throw new RuntimeException(e);
           }
       }
   
       public Account findAccountById(Integer accountId) {
           try {
               return runner.query("select * from account where id = ?", new BeanHandler<Account>(Account.class), accountId);
           } catch (Exception e) {
               throw new RuntimeException(e);
           }
       }
   
       public void saveAccount(Account account) {
           try {
               runner.update("insert into account(name,money) values (?,?)",account.getName(),account.getMoney());
           } catch (Exception e) {
               throw new RuntimeException(e);
           }
       }
   
       public void updateAccount(Account account) {
           try {
               runner.update("update account set name=?, money=? where id=? ",account.getName(),account.getMoney(),account.getId());
           } catch (Exception e) {
               throw new RuntimeException(e);
           }
       }
   
       public void deleteAccount(Integer accountId) {
           try {
               runner.update("delete from account where id=?",accountId);
           } catch (Exception e) {
               throw new RuntimeException(e);
           }
       }
   }
   
   ```

3. bean.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context.xsd">
   
       <!-- 告知spring在创建容器时要扫描的包 -->
       <context:component-scan base-package="com.wmxyyy"></context:component-scan>
   
       <!--配置QueryRunner-->
       <bean id="runner" class="org.apache.commons.dbutils.QueryRunner" scope="prototype">
           <!--注入数据源-->
           <constructor-arg name="ds" ref="dataSource"></constructor-arg>
       </bean>
   
       <!--配置数据源-->
       <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
           <!--连接数据库的必备信息-->
           <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
           <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/eesy?characterEncoding=utf8"></property>
           <property name="user" value="root"></property>
           <property name="password" value="123456"></property>
       </bean>
   </beans>
   ```

   > 基于注解的 IoC 配置已经完成，但是大家都发现了一个问题：我们依然离不开 spring 的 xml 配置文件，那么能不能不写这个 bean.xml，所有配置都用注解来实现呢？
   > 当然，我们选择哪种配置的原则是简化开发和配置方便，而非追求某种技术

#### 3.5 改进：基于纯注解的配置

1. SpringConfiguration

   ```java
   /**
    * @author wmxyyy
    * @date 2020/1/12 10:02
    * @state 配置类，作用和bean.xml一样
    * spring的新注解
    *  - Configuration
    *      作用：指定当前类是一个配置类
    *		用于指定当前类是一个 spring 配置类， 当创建容器时会从该类上加载注解。获取容器时需要使用AnnotationApplicationContext(有@Configuration 注解的类.class)
    *      细节：当配置类作为AnnotationConfigApplicationContext对象创建参数时，可省略不写
    ============================================================================
    *  - ComponentScan
    *      作用：用于通过注解指定spring在创建容器时要扫描的包
    *      属性：
    *          value：它和basePackages的作用是一样的都是用于指定创建容器时要扫描的包
    *            相当于xml配置的<context:component-scan base-package="com.wmxyyy"></context:component-scan>
    ============================================================================
    *  - Bean
    *      作用：用于把当前方法的返回值作为bean对象存入spring的ioc容器中
    *      属性：
    *          name：用于指定bean的id。当省略不写时，默认值是当前方法的名称
    *      细节：
    *          当我们使用注解配置方法时，如果方法有参数spring会去容器中查找有没有可用的bean对象
    *          查找的方式和Autowired注解的作用时一样的
    ============================================================================
    *  - Import
    *      作用：用于导入其他的配置类
    *      属性：
    *          value：用于指定其他配置类的字节码
    *              当我们使用Import的注解之后，有Import注解的类就是父配置类，导入的是子配置类
    ============================================================================
    *  - PropertySource
    *      作用：用于指定properties文件的位置
    *      属性：
    *          value：指定文件的名称和路径
    *              关键字：classpath：表示类路径下
    *
    */
   @Configuration
   @ComponentScan("com.wmxyyy")
   @Import(JdbcConfig.class)
   @PropertySource("classpath:jdbcConfig.properties")
   public class SpringCongiguration {
   
   }
   ```

2. JdbConfig

   ```java
   /**
    * @author wmxyyy
    * @date 2020/1/12 10:17
    * @state 用于和spring连接数据库的相关配置类
    */
   
   public class JdbcConfig {
       @Value("${jdbc.driver}")
       private String driver;
   
       @Value("${jdbc.url}")
       private String url;
   
       @Value("${jdbc.username}")
       private String username;
   
       @Value("${jdbc.password}")
       private String password;
       /**
        * 用于创建一个QueryRunner对象
        * @param dataSource
        * @return
        */
       @Bean(name = "runner")
       @Scope("prototype")
       public QueryRunner createQueryRunner(DataSource dataSource){
           return new QueryRunner(dataSource);
       }
   
       /**
        * 创建数据源对象
        * @return
        */
       @Bean(name = "dataSource")
       public DataSource createDataSource(){
           ComboPooledDataSource ds = new ComboPooledDataSource();
           try{
               ds.setDriverClass(driver);
               ds.setJdbcUrl(url);
               ds.setUser(username);
               ds.setPassword(password);
               return ds;
           }catch (Exception e){
               throw new RuntimeException(e);
           }
       }
   }
   ```

3. jdbcConfig.properties

   ```properties
   jdbc.driver=com.mysql.jdbc.Driver
   jdbc.url=jdbc:mysql://localhost:3306/eesy
   jdbc.username=root
   jdbc.password=123456
   ```

#### 3.6 Spring 整合 Junit 

1. #### 问题

   在测试类中，每个测试方法都要获取容器，如果不写的话，直接会提示空指针异常。所以又不能轻易删掉 

2. #### 解决思路分析

   针对上述问题，我们需要的是程序能自动帮我们创建容器。一旦程序能自动为我们创建 spring 容器，我们就无须手动创建了，问题也就解决了。
   我们都知道junit 单元测试的原理，因为它自己都无法知晓我们是否使用了 spring 框架，更不用说帮我们创建 spring 容器了。不过好在 junit 给我们暴露了一个注解，可以让我们替换掉它的运行器。这时，我们需要依靠 spring 框架，因为它提供了一个运行器，可以读取配置文件（或注解）来创建容器。我们只需要告诉它配置文件在哪就行了。 

   1. pom.xml

      ```xml
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-test</artifactId>
          <version>5.0.2.RELEASE</version>
      </dependency>
      ```

   2. 测试类

      ```java
      /**
       * @author wmxyyy
       * @date 2020/1/11 13:57
       * @state Spring整合junit的配置
       *  1.导入spring整合junit的jar
       *  2.使用junit提供的一个注解把原有的main方法替换了，替换成spring提供的
       *      -- @RunWith
       *  3.告知spring的运行器，spring和ioc创建是基于xml还是注解，并且说明位置
       *      -- @ContextConfiguration
       *              locations：指定xml的位置，加上classpath关键字，表示在类路径下
       *              classes：指定注解类所在的位置
       */
      @RunWith(SpringJUnit4ClassRunner.class)
      @ContextConfiguration(classes = SpringCongiguration.class)
      public class AccountServiceTest {
          @Autowired
          private IAccountService as = null;
          /**
           * 测试查找所有账户信息
           */
          @Test
          public void testFindAll(){
              List<Account> accounts = as.findAllAccount();
              for(Account account : accounts){
                  System.out.println(account);
              }
          }
          /**
           * 测试根据id查找账户信息
           */
          @Test
          public void testFindOne(){
              Account account = as.findAccountById(2);
              System.out.println(account);
          }
          /**
           * 测试保存账户信息
           */
          @Test
          public void testSave(){
              Account account = new Account();
              account.setName("中文");
              account.setMoney(9999f);
      
              as.saveAccount(account);
          }
          /**
           * 测试修改账户
           */
          @Test
          public void testUpdate(){
              Account account = as.findAccountById(5);
              account.setMoney(234234f);
              as.updateAccount(account);
          }
          /**
           * 测试删除账户
           */
          @Test
          public void testDelete(){
              as.deleteAccount(1);
          }
      }
      ```

      > 为什么不把测试类配到 xml 中 
      >
      > 
      >
      > 答案是肯定的，可以使用。
      > 那么为什么不采用配置到 xml 中的方式呢？
      > 这个原因是这样的：
      > 第一：当我们在 xml 中配置了一个 bean， spring 加载配置文件创建容器时，就会创建对象。
      > 第二：测试类只是我们在测试功能时使用，而在项目中它并不参与程序逻辑，也不会解决需求上的问题，所以创建完了，并没有使用。那么存在容器中就会造成资源的浪费。所以，基于以上两点，我们不应该把测试配置到 xml 文件中。 

   

### 第四章 AOP面向切面编程

#### 4.1 概念

全称是 Aspect Oriented Programming 即： 面向切面编程 。简单的说它就是把我们程序重复的代码抽取出来，在需要执行的时候，使用动态代理的技术，在不修改源码的基础上，对我们的已有方法进行增强 

#### 4.2 AOP 的作用 

- 在程序运行期间，不修改源码对已有方法进行增强 

- 减少重复代码
- 提高开发效率
- 维护方便 

#### 4.3 AOP 的实现方式 

使用动态代理基数

#### 4.4 案例中问题 

当我们执行时，由于执行有异常，转账失败。但是因为我们是每次执行持久层方法都是独立事务，导致无法实现事务控制（不符合事务的一致性） 

#### 4.5 解决办法 

让业务层来控制事务的提交和回滚 

#### 4.6 新的问题

通过对业务层改造，已经可以实现事务控制了，但是由于我们添加了事务控制，也产生了一
个新的问题：业务层方法变得臃肿了，里面充斥着很多重复代码。并且业务层方法和事务控制方法耦合了。试想一下，如果我们此时提交，回滚，释放资源中任何一个方法名变更，都需要修改业务层的代码，况且这还只是一个业务层实现类，而实际的项目中这种业务层实现类可能有十几个甚至几十个。 

#### 4.7 动态代理常用的有两种方式 

```java
/**
 * 基于接口的动态代理
 */
public class Client {
    public static void main(String[] args) {
        final Producer producer = new Producer();
        producer.saleProduct(1000f);

        /**
         * 动态代理
         *  特点：字节码随用随创建，随用随加载
         *  作用：不修改源码的基础上对方法增强
         *  分类：
         *      基于接口的动态代理
         *      基于子类的动态代理
         *
         *  基于接口的动态代理：Proxy
         *      创建代理对象：使用Proxy类中的newProxyInstance方法
         *      被代理类最少实现一个接口，如果没有则不能使用
         *
         * newProxyInstance方法的参数
         *  - ClassLoader:类加载器
         *  - Class[]:字节码数组
         *  - InvocationHandler:用于提供增强的代码
         */
        IProducer proxyProduce = (IProducer) Proxy.newProxyInstance(producer.getClass().getClassLoader(),
                producer.getClass().getInterfaces(),
                new InvocationHandler() {
                    /**
                     * 作用：执行被代理对象的任何接口方法都会经过该方法
                     * @param proxy  代理对象的引用
                     * @param method 当前执行的方法
                     * @param args   当前执行方法的所需的参数
                     * @return 和被代理对象方法有相同的返回值
                     * @throws Throwable
                     */
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        Object returnValue = null;
                        //1.获取方法执行的参数
                        Float money = (Float) args[0];
                        //2.判断当前方法是不是销售
                        if ("saleProduct".equals(method.getName())) {
                            returnValue = method.invoke(producer, money * 0.8f);
                        }
                        return returnValue;
                    }
                });
        proxyProduce.saleProduct(1000f);
    }
}
```

```java
/**
 * 基于子类的动态代理
 */
public class Client {
    public static void main(String[] args) {
        final Producer producer = new Producer();
        /**
         * 基于子类的动态代理：Enhancer
         *     创建代理对象：使用Enhancer类中的create方法
         *          create方法参数
         *              - Class：字节码，用于指定被代理对象的字节码
         *              - Callback：用于提供增强的代码
         *                  一般写的都是该接口的子接口实现类：MethodInterceptor
         */
        Producer cglibProducer = (Producer) Enhancer.create(producer.getClass(),
                new MethodInterceptor() {
                    /**
                     *
                     * @param proxy
                     * @param method
                     * @param args
                     * @param methodProxy 当前执行方法的代理对象
                     * @return
                     * @throws Throwable
                     */
                    public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
                        Object returnValue = null;
                        //1.获取方法的参数
                        Float money = (Float) args[0];
                        //2.判断当前方法是不是销售
                        if ("saleProduct".equals(method.getName())) {
                            returnValue = method.invoke(producer, money * 0.8f);
                        }
                        return returnValue;
                    }
                });
        cglibProducer.saleProduct(1200f);
    }
}
```

```xml
<dependency>
    <groupId>org.sonatype.sisu.inject</groupId>
    <artifactId>cglib</artifactId>
    <version>2.2.1-v20090111</version>
</dependency>
```

#### 4.8 解决案例中的问题 

```java
public class BeanFactory {
    private IAccountService accountService;
    private TransactionManager txManager;

    public void setTxManager(TransactionManager txManager) {
        this.txManager = txManager;
    }

    public final void setAccountService(IAccountService accountService) {
        this.accountService = accountService;
    }

    public IAccountService getAccountService() {
        return (IAccountService) Proxy.newProxyInstance(accountService.getClass().getClassLoader(),
                accountService.getClass().getInterfaces(),
                new InvocationHandler() {
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        Object rtValue = null;
                        try {
                            //1.开启事务
                            txManager.beginTransaction();
                            //2.执行操作
                            method.invoke(accountService, args);
                            //3.提交事务
                            txManager.commit();
                            return rtValue;
                        } catch (Exception e) {
                            //4.回滚操作
                            txManager.rollback();
                            throw new RuntimeException(e);
                        } finally {
                            //5.释放连接
                            txManager.release();
                        }
                    }
                });
    }
}
```

> 当我们改造完成之后，业务层用于控制事务的重复代码就都可以删掉了。

#### 4.9 AOP 相关术语 

- Joinpoint(连接点): 指那些被拦截到的点。在 spring 中,这些点指的是方法,因为 spring 只支持方法类型的连接点 (业务层所有方法)

- Pointcut(切入点):指我们要对哪些 Joinpoint 进行拦截的定义（被增强方法）

  

- Advice(通知/增强):指拦截到 Joinpoint 之后所要做的事情就是通知

- 通知的类型： 前置通知,后置通知,异常通知,最终通知,环绕通知

  

- Introduction(引介):
  引介是一种特殊的通知在不修改类代码的前提下, Introduction 可以在运行期为类动态地添加一些方法或 Field

- Target(目标对象):代理的目标对象

- Weaving(织入):
  是指把增强应用到目标对象来创建新的代理对象的过程。
  spring 采用动态代理织入，而 AspectJ 采用编译期织入和类装载期织入

- Proxy（代理） :
  一个类被 AOP 织入增强后，就产生一个结果代理类

  

- Aspect(切面):
  是切入点和通知（引介）的结合 

  > 运行阶段（Spring 框架完成的）
  > Spring 框架监控切入点方法的执行。一旦监控到切入点方法被运行，使用代理机制，动态创建目标对象的代理对象，根据通知类别，在代理对象的对应位置，将通知对应的功能织入，完成完整的代码逻辑运行。 

#### 4.10 spring中基于XML的AOP

1. ##### pom.xml

   ```xml
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-context</artifactId>
       <version>5.0.2.RELEASE</version>
   </dependency>
   <dependency>
       <groupId>org.aspectj</groupId>
       <artifactId>aspectjweaver</artifactId>
       <version>1.8.7</version>
   </dependency>
   ```

2. ##### 工具类

   ```java
   /**
    * @author wmxyyy
    * @date 2020/1/13 10:26
    * @state 用于记录日志的工具类，它里面提供了公共的代码
    */
   public class Logger {
       /**
        * 用于打印日志：计划让其在切入点方法执行之前执行（切入点方法就是业务层方法）
        */
       public  void printLog(){
           System.out.println("Logger类中的printLog方法开始记录日志了。。。");
       }
   }
   ```

3. ##### 业务层实现类

   ```java
   /**
    * 账户的业务层实现类
    */
   public class AccountServiceImpl implements IAccountService{
   
       @Override
       public void saveAccount() {
           System.out.println("执行了保存");
       }
       @Override
       public void updateAccount(int i) {
           System.out.println("执行了更新");
       }
       @Override
       public int deleteAccount() {
           System.out.println("执行了删除");
           return 0;
       }
   }
   ```

4. ##### bean.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/aop
           http://www.springframework.org/schema/aop/spring-aop.xsd">
   
       <!--配置spring的IOC，配置service对象-->
       <bean id="accountService" class="com.wmxyyy.service.impl.AccountServiceImpl"></bean>
   
       <!--配置Logger类-->
       <bean id="logger" class="com.wmxyyy.utils.Logger"></bean>
   
       <!--spring中基于XML的AOP配置步骤
           1、把通知Bean也交给spring来管理
           2、使用aop:config标签表明开始AOP的配置
           3、使用aop:aspect标签表明配置切面
                   id属性：是给切面提供一个唯一标识
                   ref属性：是指定通知类bean的Id。
           4、在aop:aspect标签的内部使用对应标签来配置通知的类型
                  我们现在示例是让printLog方法在切入点方法执行之前之前：所以是前置通知
                  aop:before：表示配置前置通知
                       method属性：用于指定Logger类中哪个方法是前置通知
                       pointcut属性：用于指定切入点表达式，该表达式的含义指的是对业务层中哪些方法增强
           -->
   
       <!--切入点表达式的写法：
                   关键字：execution(表达式)
                   表达式：
                       访问修饰符  返回值  包名.包名.包名...类名.方法名(参数列表)
                   标准的表达式写法：
                       public void com.wmxyyy.service.impl.AccountServiceImpl.saveAccount()
                   访问修饰符可以省略
   
                   返回值可以使用通配符，表示任意返回值
                       * com.wmxyyy.service.impl.AccountServiceImpl.saveAccount()
                   包名可以使用通配符，表示任意包。但是有几级包，就需要写几个*.
                       * *.*.*.*.AccountServiceImpl.saveAccount())
                   包名可以使用..表示当前包及其子包
                       * *..AccountServiceImpl.saveAccount()
                   类名和方法名都可以使用*来实现通配
                       * *..*.*()
                   参数列表：
                       可以直接写数据类型：
                           基本类型直接写名称           int
                           引用类型写包名.类名的方式   java.lang.String
                       可以使用通配符表示任意类型，但是必须有参数
                       可以使用..表示有无参数均可，有参数可以是任意类型
                   全通配写法：
                       * *..*.*(..)
   
                   实际开发中切入点表达式的通常写法：
                       切到业务层实现类下的所有方法
                           * com.wmxyyy.service.impl.*.*(..)-->
       <!--配置AOP-->
       <aop:config>
           <!--配置切面-->
           <aop:aspect id="logAdvice" ref="logger">
               <!--配置通知的类型，并且建立通知方法和切入点方法的关联-->
               <aop:before method="printLog"
                           pointcut="execution(* com.wmxyyy.service.impl.*.*(..))"></aop:before>
           </aop:aspect>
       </aop:config>
   </beans>
   ```

   #### 4.11 通知类型 

   ```xml
   <!--配置AOP-->
       <aop:config>
           <!--配置切入点表达式-->
           <aop:pointcut id="pt1" expression="execution(* com.wmxyyy.service.impl.*.*(..))"></aop:pointcut>
           <!--配置切面-->
           <aop:aspect id="logAdvice" ref="logger">
               <!--&lt;!&ndash;配置前置通知的类型，并且建立通知方法和切入点方法的关联&ndash;&gt;
               <aop:before method="beforePrintLog" pointcut-ref="pt1"></aop:before>
               &lt;!&ndash;配置后置通知的类型&ndash;&gt;
               <aop:after-returning method="afterReturningPrintLog" pointcut-ref="pt1"></aop:after-returning>
               &lt;!&ndash;配置异常通知的类型&ndash;&gt;
               <aop:after-throwing method="afterThrowingPrintLog" pointcut-ref="pt1"></aop:after-throwing>
               &lt;!&ndash;配置最终通知的类型&ndash;&gt;
               <aop:after method="afterPrintLog" pointcut-ref="pt1"></aop:after>-->
   
               <aop:around method="aroundPringLog" pointcut-ref="pt1"></aop:around>
           </aop:aspect>
       </aop:config>
   ```

#### 4.10 spring中基于注解的AOP

1. pom.xml

   ```xml
   <dependencies>
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-context</artifactId>
           <version>5.0.2.RELEASE</version>
       </dependency>
       <dependency>
           <groupId>org.aspectj</groupId>
           <artifactId>aspectjweaver</artifactId>
           <version>1.8.7</version>
       </dependency>
   </dependencies>
   ```

2. bean.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/aop
           http://www.springframework.org/schema/aop/spring-aop.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context.xsd">
   
       <!-- 配置spring创建容器时要扫描的包-->
       <context:component-scan base-package="com.wmxyyy"></context:component-scan>
   
       <!-- 配置spring开启注解AOP的支持 -->
       <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
   
   </beans>
   ```

3. 工具类

   ```java
   /**
    * 用于记录日志的工具类，它里面提供了公共的代码
    */
   @Component("logger")
   @Aspect//切面类
   public class Logger {
   
       @Pointcut("execution(* com.wmxyyy.service.impl.*.*(..))")
       private void pt1(){}
   
       /**
        * 前置通知
        */
   //    @Before("pt1()")
       public  void beforePrintLog(){
           System.out.println("前置通知Logger类中的beforePrintLog方法开始记录日志了。。。");
       }
   
       /**
        * 后置通知
        */
   //    @AfterReturning("pt1()")
       public  void afterReturningPrintLog(){
           System.out.println("后置通知Logger类中的afterReturningPrintLog方法开始记录日志了。。。");
       }
       /**
        * 异常通知
        */
   //    @AfterThrowing("pt1()")
       public  void afterThrowingPrintLog(){
           System.out.println("异常通知Logger类中的afterThrowingPrintLog方法开始记录日志了。。。");
       }
   
       /**
        * 最终通知
        */
   //    @After("pt1()")
       public  void afterPrintLog(){
           System.out.println("最终通知Logger类中的afterPrintLog方法开始记录日志了。。。");
       }
   
       /**
        * 环绕通知
        * 问题：
        *      当我们配置了环绕通知之后，切入点方法没有执行，而通知方法执行了。
        * 分析：
        *      通过对比动态代理中的环绕通知代码，发现动态代理的环绕通知有明确的切入点方法调用，而我们的代码中没有。
        * 解决：
        *      Spring框架为我们提供了一个接口：ProceedingJoinPoint。该接口有一个方法proceed()，此方法就相当于明确调用切入点方法。
        *      该接口可以作为环绕通知的方法参数，在程序执行时，spring框架会为我们提供该接口的实现类供我们使用。
        *
        * spring中的环绕通知：
        *      它是spring框架为我们提供的一种可以在代码中手动控制增强方法何时执行的方式。
        */
       @Around("pt1()")
       public Object aroundPringLog(ProceedingJoinPoint pjp){
           Object rtValue = null;
           try{
               Object[] args = pjp.getArgs();//得到方法执行所需的参数
   
               System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。前置");
   
               rtValue = pjp.proceed(args);//明确调用业务层方法（切入点方法）
   
               System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。后置");
   
               return rtValue;
           }catch (Throwable t){
               System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。异常");
               throw new RuntimeException(t);
           }finally {
               System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。最终");
           }
       }
   }
   
   ```

4. 业务层实现类

   ```java
   /**
    * 账户的业务层实现类
    */
   @Service("accountService")
   public class AccountServiceImpl implements IAccountService{
   
       @Override
       public void saveAccount() {
           System.out.println("执行了保存");
       }
   
       @Override
       public void updateAccount(int i) {
           System.out.println("执行了更新"+i);
   
       }
   
       @Override
       public int deleteAccount() {
           System.out.println("执行了删除");
           return 0;
       }
   }
   ```

   

### 第五章 Spring 中的 JdbcTemplate 



### 第六章 Spring 中的事务控制 

#### 6.1 Spring 中事务控制的 API 介绍 

- ##### PlatformTransactionManager 

  此接口是 spring 的事务管理器，它里面提供了我们常用的操作事务的方法

  	- TransactionStatus getTransaction(TransactionDefinition definition) ：获取事务状态信息
  	- void commit(TransactionStatus status) ：提交事务
  	- void rollback(TransactionStatus status)：回滚事务

  > 我们在开发中都是使用它的实现类 
  >
  > 真正管理事务的对org.springframework.jdbc.datasource.DataSourceTransactionManager 使用 SpringJDBC 或 iBatis 进行持久化数据时使用

- ##### TransactionDefinition 

  它是事务的定义信息对象 

  - ##### String getName() ：获取事务对象名称

  - ##### int get getIsolationLevel()：获取事务隔离级

    > ISOLATION_DEFAULT：默认级别，归属下列某一种
    >
    > 
    >
    > - ISOLATION_READ_UNCOMMITTED：可以读取未提交的数据（可能出现脏读、不可重读、幻读）
    >
    > - ISOLATION_READ_COMMITTED：只能读取已提交数据，解决脏读问题（Oracle默认级别）（可能出现不可重读、幻读）
    >
    > - ISOLATION_REPEATABLE_READ：是否读取其他事务提交修改后的数据，解决不可重复读问题（MySQL默认级别）（可能出现幻读）
    >
    > - ISOLATION_SERIALIZABLE：是否读取其他事务提交添加后的数据，解决幻读问题

  - ##### int getPropagationBehavior()：获取事务传播行为

    > **REQUIRED:如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中。一般的选择（默认值）**
    > **SUPPORTS:支持当前事务，如果当前没有事务，就以非事务方式执行（没有事务）**
    > MANDATORY：使用当前的事务，如果当前没有事务，就抛出异常
    > REQUERS_NEW:新建事务，如果当前在事务中，把当前事务挂起。
    > NOT_SUPPORTED:以非事务方式执行操作，如果当前存在事务，就把当前事务挂起
    > NEVER:以非事务方式运行，如果当前存在事务，抛出异常
    > NESTED:如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行 REQUIRED 类似的操作 

  - ##### int getTimeout():获取事务超时时间

    > 默认值是-1，没有超时限制。如果有，以秒为单位进行设置。

  - ##### boolean is ReadOnly：获取事务是否只读

    > 只读型事务：执行查询时，也会开启事务
    >
    > 读写型事务：增加、删除、修改开启事务

#### 6.2 基于 XML 的声明式事务控制 

1. pom.xml

   ```xml
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-context</artifactId>
       <version>5.0.2.RELEASE</version>
   </dependency>
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-jdbc</artifactId>
       <version>5.0.2.RELEASE</version>
   </dependency>
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-tx</artifactId>
       <version>5.0.2.RELEASE</version>
   </dependency>
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-test</artifactId>
       <version>5.0.2.RELEASE</version>
   </dependency>
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>5.1.6</version>
   </dependency>
   <dependency>
       <groupId>org.aspectj</groupId>
       <artifactId>aspectjweaver</artifactId>
       <version>1.8.7</version>
   </dependency>
   <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.12</version>
   </dependency>
   ```

2. ```java
   /**
    * 账户的业务层实现类
    * 事务控制应该都是在业务层
    */
   public class AccountServiceImpl implements IAccountService {
   
       private IAccountDao accountDao;
   
       public void setAccountDao(IAccountDao accountDao) {
           this.accountDao = accountDao;
       }
   
       @Override
       public Account findAccountById(Integer accountId) {
           return accountDao.findAccountById(accountId);
   
       }
   
       @Override
       public void transfer(String sourceName, String targetName, Float money) {
           System.out.println("transfer....");
               //2.1根据名称查询转出账户
               Account source = accountDao.findAccountByName(sourceName);
               //2.2根据名称查询转入账户
               Account target = accountDao.findAccountByName(targetName);
               //2.3转出账户减钱
               source.setMoney(source.getMoney()-money);
               //2.4转入账户加钱
               target.setMoney(target.getMoney()+money);
           
               //2.5更新转出账户
               accountDao.updateAccount(source);
               //2.6更新转入账户
               accountDao.updateAccount(target);
       }
   }
   ```

3. ```java
   /**
    * 账户的持久层实现类
    */
   public class AccountDaoImpl extends JdbcDaoSupport implements IAccountDao {
   
       @Override
       public Account findAccountById(Integer accountId) {
           List<Account> accounts = super.getJdbcTemplate().query("select * from account where id = ?",new BeanPropertyRowMapper<Account>(Account.class),accountId);
           return accounts.isEmpty()?null:accounts.get(0);
       }
   
       @Override
       public Account findAccountByName(String accountName) {
           List<Account> accounts = super.getJdbcTemplate().query("select * from account where name = ?",new BeanPropertyRowMapper<Account>(Account.class),accountName);
           if(accounts.isEmpty()){
               return null;
           }
           if(accounts.size()>1){
               throw new RuntimeException("结果集不唯一");
           }
           return accounts.get(0);
       }
       @Override
       public void updateAccount(Account account) {
           super.getJdbcTemplate().update("update account set name=?,money=? where id=?",account.getName(),account.getMoney(),account.getId());
       }
   }
   ```

4. ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xmlns:tx="http://www.springframework.org/schema/tx"
          xsi:schemaLocation="
           http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/tx
           http://www.springframework.org/schema/tx/spring-tx.xsd
           http://www.springframework.org/schema/aop
           http://www.springframework.org/schema/aop/spring-aop.xsd">
       <!-- 配置业务层-->
       <bean id="accountService" class="com.wmxyyy.service.impl.AccountServiceImpl">
           <property name="accountDao" ref="accountDao"></property>
       </bean>
   
       <!-- 配置账户的持久层-->
       <bean id="accountDao" class="com.wmxyyy.dao.impl.AccountDaoImpl">
           <property name="dataSource" ref="dataSource"></property>
       </bean>
   
       <!-- 配置数据源-->
       <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
           <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
           <property name="url" value="jdbc:mysql://localhost:3306/eesy"></property>
           <property name="username" value="root"></property>
           <property name="password" value="123456"></property>
       </bean>
   
       <!--spring中基于XML的声明式事务控制配置步骤
           1.配置事务管理器
           2.配置事务通知,导入事务的约束
           3.配置AOP的通用切入点表达式
   
           4.建立事务通知和切入点表达式的对应关系
           5.配置事务的属性
       -->
   
       <!--配置事务管理器-->
       <bean id="transcationManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
           <property name="dataSource" ref="dataSource"></property>
       </bean>
   
       <!--配置事务的通知-->
       <tx:advice id="txAdvice" transaction-manager="transcationManager">
           <tx:attributes>
               <!-- 配置事务的属性
                   isolation：用于指定事务的隔离级别。默认值是DEFAULT，表示使用数据库的默认隔离级别。
                   propagation：用于指定事务的传播行为。
                                - 默认值是REQUIRED，表示一定会有事务，增删改的选择。
                                - 查询方法可以选择SUPPORTS。
                   read-only：用于指定事务是否只读。
                                - 只有查询方法才能设置为true。
                                - 默认值是false，表示读写。
                   timeout：用于指定事务的超时时间，默认值是-1，表示永不超时。如果指定了数值，以秒为单位。
                   rollback-for：
                                - 用于指定一个异常，当产生该异常时，事务回滚
                                - 产生其他异常时，事务不回滚。
                                - 没有默认值。表示任何异常都回滚。
                   no-rollback-for：用于指定一个异常，当产生该异常时，事务不回滚
                                - 产生其他异常时事务回滚。
                                - 没有默认值。表示任何异常都回滚。
           -->
               <tx:method name="*" propagation="REQUIRED" read-only="false"></tx:method>
               <tx:method name="find*" propagation="SUPPORTS" read-only="true"></tx:method>
           </tx:attributes>
       </tx:advice>
   
       <!--配置aop-->
       <aop:config>
           <!--配置切入点表达式-->
           <aop:pointcut id="pt1" expression="execution(* com.wmxyyy.service.impl.*.*(..))"></aop:pointcut>
           <!--建立切入点表达式和事务通知的对应关系-->
           <aop:advisor advice-ref="txAdvice" pointcut-ref="pt1"></aop:advisor>
       </aop:config>
   </beans>
   ```

5. ```java
   /**
    * 使用Junit单元测试：测试我们的配置
    */
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration(locations = "classpath:bean.xml")
   public class AccountServiceTest {
   
       @Autowired
       private IAccountService as;
   
       @Test
       public  void testTransfer(){
           as.transfer("aaa","bbb",100f);
   
       }
   }
   ```

#### 6.3 基于注解的声明式事务控制 

1. ```java
   /**
    * 账户的业务层实现类
    * 事务控制应该都是在业务层
    */
   @Service("accountService")
   @Transactional(propagation = Propagation.SUPPORTS,readOnly = true)//只读型事务的配置
   public class AccountServiceImpl implements IAccountService {
   
       @Autowired
       private IAccountDao accountDao;
   
       @Override
       public Account findAccountById(Integer accountId) {
           return accountDao.findAccountById(accountId);
   
       }
   
       @Transactional(propagation = Propagation.REQUIRED,readOnly = false)//只读写型事务的配置
       @Override
       public void transfer(String sourceName, String targetName, Float money) {
           System.out.println("transfer....");
               //2.1根据名称查询转出账户
               Account source = accountDao.findAccountByName(sourceName);
               //2.2根据名称查询转入账户
               Account target = accountDao.findAccountByName(targetName);
               //2.3转出账户减钱
               source.setMoney(source.getMoney()-money);
               //2.4转入账户加钱
               target.setMoney(target.getMoney()+money);
               //2.5更新转出账户
               accountDao.updateAccount(source);
   
               //2.6更新转入账户
               accountDao.updateAccount(target);
       }
   }
   
   ```

2. ```java
   /**
    * 账户的持久层实现类
    */
   @Repository("accountDao")
   public class AccountDaoImpl implements IAccountDao {
   
       @Autowired
       private JdbcTemplate jdbcTemplate;
   
       @Override
       public Account findAccountById(Integer accountId) {
           List<Account> accounts = jdbcTemplate.query("select * from account where id = ?",new BeanPropertyRowMapper<Account>(Account.class),accountId);
           return accounts.isEmpty()?null:accounts.get(0);
       }
   
       @Override
       public Account findAccountByName(String accountName) {
           List<Account> accounts = jdbcTemplate.query("select * from account where name = ?",new BeanPropertyRowMapper<Account>(Account.class),accountName);
           if(accounts.isEmpty()){
               return null;
           }
           if(accounts.size()>1){
               throw new RuntimeException("结果集不唯一");
           }
           return accounts.get(0);
       }
   
       @Override
       public void updateAccount(Account account) {
          jdbcTemplate.update("update account set name=?,money=? where id=?",account.getName(),account.getMoney(),account.getId());
       }
   }
   ```

3. ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xmlns:tx="http://www.springframework.org/schema/tx"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="
           http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/tx
           http://www.springframework.org/schema/tx/spring-tx.xsd
           http://www.springframework.org/schema/aop
           http://www.springframework.org/schema/aop/spring-aop.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context.xsd">
   
       <!-- 配置spring创建容器时要扫描的包-->
       <context:component-scan base-package="com.wmxyyy"></context:component-scan>
   
       <!-- 配置JdbcTemplate-->
       <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
           <property name="dataSource" ref="dataSource"></property>
       </bean>
   
       <!-- 配置数据源-->
       <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
           <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
           <property name="url" value="jdbc:mysql://localhost:3306/eesy"></property>
           <property name="username" value="root"></property>
           <property name="password" value="123456"></property>
       </bean>
   
       <!-- spring中基于注解 的声明式事务控制配置步骤
           1、配置事务管理器
           2、开启spring对注解事务的支持
           3、在需要事务支持的地方使用@Transactional注解
       -->
   
       <!--配置事务管理器-->
       <bean id="transcationManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
           <property name="dataSource" ref="dataSource"></property>
       </bean>
   
       <!--开启spring对注解事务的支持-->
       <tx:annotation-driven transaction-manager="transcationManager"></tx:annotation-driven>
   
   </beans>
   ```
   
4. ```java
   /**
    * 使用Junit单元测试：测试我们的配置
    */
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration(locations = "classpath:bean.xml")
   public class AccountServiceTest {
   
       @Autowired
       private IAccountService as;
   
       @Test
       public  void testTransfer(){
           as.transfer("aaa","bbb",100f);
   
       }
   }
   
   ```

#### 6.4 基于纯注解的声明式事务控制 

1. ```java
   /**
    * @author wmxyyy
    * @date 2020/1/14 10:18
    * @state
    */
   @Configuration
   @ComponentScan("com.wmxyyy")
   @Import({JdbcConfig.class,TransactionConfig.class})
   @PropertySource("jdbcConfig.properties")
   @EnableTransactionManagement
   public class SpringConfiguration {
   }
   ```

2. ```java
   /**
    * @author wmxyyy
    * @date 2020/1/14 10:20
    * @state
    */
   
   public class JdbcConfig {
       @Value("${jdbc.driver}")
       private String driver;
   
       @Value("${jdbc.url}")
       private String url;
   
       @Value("${jdbc.username}")
       private String username;
   
       @Value("${jdbc.password}")
       private String password;
   
       /**
        * 创建JdbcTemplate
        * @param dataSource
        * @return
        */
       @Bean(name = "jdbcTemplate")
       public JdbcTemplate creteJdbcTemplate(DataSource dataSource){
           return new JdbcTemplate(dataSource);
       }
   
       /**
        * 创建数据源对象
        * @return
        */
       @Bean(name = "dataSource")
       public DataSource createDataSource(){
           DriverManagerDataSource ds = new DriverManagerDataSource();
           ds.setDriverClassName(driver);
           ds.setUrl(url);
           ds.setUsername(username);
           ds.setPassword(password);
           return ds;
       }
   }
   
   ```

3. ```java
   /**
    * 和事务相关的配置类
    */
   public class TransactionConfig {
   
       /**
        * 用于创建事务管理器对象
        * @param dataSource
        * @return
        */
       @Bean(name="transactionManager")
       public PlatformTransactionManager createTransactionManager(DataSource dataSource){
           return new DataSourceTransactionManager(dataSource);
       }
   }
   
   ```

4. ```java
   /**
    * 使用Junit单元测试：测试我们的配置
    */
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration(classes = SpringConfiguration.class)
   public class AccountServiceTest {
   
       @Autowired
       private IAccountService as;
   
       @Test
       public  void testTransfer(){
           as.transfer("aaa","bbb",100f);
   
       }
   
   }
   ```

   

