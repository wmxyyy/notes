## SpringMVC

### 第一章 SpringMVC 的基本概念 

#### 1.1 三层架构

![](C:\Users\wang1\Desktop\md笔记\框架\springMVC\img\Snipaste_2020-02-05_19-32-05.png)

1. 咱们开发服务器端程序，一般都基于两种形式，一种C/S架构程序，一种B/S架构程序
2. 使用Java语言基本上都是开发B/S架构的程序，B/S架构又分成了三层架构
3. 三层架构
   1. 表现层：WEB层，用来和客户端进行数据交互的。表现层一般会采用MVC的设计模型
   2. 业务层：处理公司具体的业务逻辑的
   3. 持久层：用来操作数据库的 

#### 1.2 MVC模型

MVC 全名是 Model View Controller，是模型(model)－视图(view)－控制器(controller)的缩写，
是一种用于设计创建 Web 应用程序表现层的模式。

- Model（模型） ：通常指的就是我们的数据模型。作用一般情况下用于封装数据。 
- View（视图） ：通常指的就是我们的 jsp 或者 html。作用一般就是展示数据的。通常视图是依据模型数据创建的 
- Controller（控制器） ：是应用程序中处理用户交互的部分。 作用一般就是处理程序逻辑的。

#### 1.3 SpringMVC 概述 

1. 是一种基于Java实现的MVC设计模型的请求驱动类型的轻量级WEB框架。
2. Spring MVC属于SpringFrameWork的后续产品，已经融合在Spring Web Flow里面。Spring 框架提供了构建 Web 应用程序的全功能 MVC 模块。
3. 使用 Spring 可插入的 MVC 架构，从而在使用Spring进行WEB开发时，可以选择使用Spring的
SpringMVC框架或集成其他MVC开发框架，如Struts1(现在一般不用)，Struts2等 

#### 1.4 SpringMVC 在三层架构的位置 

<img src="C:\Users\wang1\Desktop\md笔记\框架\springMVC\img\Snipaste_2020-02-05_19-35-09.png" style="zoom: 80%;" />

#### 1.5 SpringMVC 的优势 

1、清晰的角色划分：
			前端控制器（DispatcherServlet）
			请求到处理器映射（HandlerMapping）
			处理器适配器（HandlerAdapter）
			视图解析器（ViewResolver）
			处理器或页面控制器（Controller）
			验证器（ Validator）
			命令对象（Command 请求参数绑定到的对象就叫命令对象）
			表单对象（Form Object 提供给表单展示和提交到的对象就叫表单对象）

<img src="C:\Users\wang1\Desktop\md笔记\框架\springMVC\img\springmvc执行流程原理.jpg" alt="springmvc执行流程原理" style="zoom:67%;" />

2、分工明确，而且扩展点相当灵活，可以很容易扩展，虽然几乎不需要。
3、由于命令对象就是一个 POJO，无需继承框架特定 API，可以使用命令对象直接作为业务对象。
4、和 Spring 其他框架无缝集成，是其它 Web 框架所不具备的。
5、可适配，通过 HandlerAdapter 可以支持任意的类作为处理器。
6、可定制性， HandlerMapping、 ViewResolver 等能够非常简单的定制。
7、功能强大的数据验证、格式化、绑定机制。
8、利用 Spring 提供的 Mock 对象能够非常简单的进行 Web 层单元测试。
9、本地化、主题的解析的支持，使我们更容易进行国际化和主题的切换。
10、强大的 JSP 标签库，使 JSP 编写更容易 

#### 1.6 Spring 和 Struts2 的优略分析 

- 共同点：
  它们都是表现层框架，都是基于 MVC 模型编写的。
  它们的底层都离不开原始 ServletAPI。
  它们处理请求的机制都是一个核心控制器。
- 区别：
  Spring MVC 的入口是 **Servlet**, 而 Struts2 是 **Filter**
  Spring MVC 是**基于方法**设计的，而 Struts2 是**基于类**， Struts2 每次执行都会创建一个动作类。所以 Spring MVC 会稍微比 Struts2 快些。
  Spring MVC 使用更加简洁,同时还支持 JSR303, 处理 ajax 的请求更方便
  (JSR303 是一套 JavaBean 参数校验的标准，它定义了很多常用的校验注解，我们可以直接将这些注解加在我们 JavaBean 的属性上面，就可以在需要校验的时候进行校验了。 )
  Struts2 的 OGNL 表达式使**页面的开发效率**相比 Spring MVC 更高些，但**执行效率**并没有比 JSTL 提
  升，尤其是 struts2 的表单标签，远没有 html 执行效率高。 

### 第二章 SpringMVC 的入门 

#### 2.1 环境搭建

- ##### 导入坐标

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <!-- 版本锁定 -->
    <spring.version>5.0.2.RELEASE</spring.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>${spring.version}</version>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>${spring.version}</version>
    </dependency>

    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>servlet-api</artifactId>
        <version>2.5</version>
        <scope>provided</scope>
    </dependency>

    <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>jsp-api</artifactId>
        <version>2.0</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.11</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

- ##### web.xml配置核心控制器

  ```xml
  <!DOCTYPE web-app PUBLIC
   "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
   "http://java.sun.com/dtd/web-app_2_3.dtd" >
  
  <web-app>
    <display-name>Archetype Created Web Application</display-name>
  
    <!--配置前端控制器-->
    <servlet>
      <servlet-name>dispatcherServlet</servlet-name>
      <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
      <!-- 配置初始化参数，用于读取 SpringMVC 的配置文件 -->
      <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:springmvc.xml</param-value>
      </init-param>
      <load-on-startup>1</load-on-startup>
    </servlet>
      
    <servlet-mapping>
      <servlet-name>dispatcherServlet</servlet-name>
      <url-pattern>/</url-pattern>
    </servlet-mapping>
  
    <!--配置解决中文乱码的过滤器-->
    <filter>
      <filter-name>characterEncodingFilter</filter-name>
      <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
      <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
      </init-param>
    </filter>
    <filter-mapping>
      <filter-name>characterEncodingFilter</filter-name>
      <url-pattern>/*</url-pattern>
    </filter-mapping>
  </web-app>
  
  ```

- ##### 创建 spring mvc 的配置文件 

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:mvc="http://www.springframework.org/schema/mvc"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="
          http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/mvc
          http://www.springframework.org/schema/mvc/spring-mvc.xsd
          http://www.springframework.org/schema/context
          http://www.springframework.org/schema/context/spring-context.xsd">
  
      <!--开启注解扫描包-->
      <context:component-scan base-package="com.wmxyyy"></context:component-scan>
  
      <!--视图解析器-->
      <bean id = "internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
          <property name="prefix" value="/WEB-INF/pages/"></property>
          <property name="suffix" value=".jsp"></property>
      </bean>
  
      <!-- 开启SpringMVC框架注解的支持 -->
      <mvc:annotation-driven conversion-service="conversionService"/>
  </beans>
  ```

- ##### 编写控制器并使用注解配置

  ```java
  /**
   * @author wmxyyy
   * @date 2020/1/16 15:53
   * @state 控制器类
   */
  @Controller
  @RequestMapping(path = "/user")
  public class HelloController {
      /**
       * 入门案例
       * @return
       */
      @RequestMapping(path = "/hello")
      public String sayHello(){
          System.out.println("Hello SpringMVC");
          return "success";
      }
  ```

#### 2.2 执行过程及原理分析 

#### <img src="C:\Users\wang1\Desktop\md笔记\框架\springMVC\img\03.bmp" alt="03" style="zoom: 50%;" />

1. 当启动Tomcat服务器的时候，因为配置了load-on-startup标签，所以会创建DispatcherServlet对象，就会加载springmvc.xml配置文件
2. 开启了注解扫描，那么HelloController对象就会被创建
3. 从index.jsp发送请求，请求会先到达DispatcherServlet核心控制器，根据配置@RequestMapping注解找到执行的具体方法
4. 根据执行方法的返回值，再根据配置的视图解析器，去指定的目录下查找指定名称的JSP文件
5. Tomcat服务器渲染页面，做出响应  

#### 2.3 入门案例中涉及的组件 

- DispatcherServlet：前端控制器
  用户请求到达前端控制器，它就相当于 mvc 模式中的 c， dispatcherServlet 是整个流程控制的中心，由它调用其它组件处理用户的请求， dispatcherServlet 的存在降低了组件之间的耦合性。

- **HandlerMapping：处理器映射器**

  HandlerMapping 负责根据用户请求找到 Handler 即处理器， SpringMVC 提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。 

- Handler：处理器
  它就是我们开发中要编写的具体业务控制器。由 DispatcherServlet 把用户请求转发到 Handler。由Handler 对具体的用户请求进行处理。

- **HandlAdapter：处理器适配器**
  通过 HandlerAdapter 对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行。

- **View Resolver：视图解析器**
  View Resolver 负责将处理结果生成 View 视图， View Resolver 首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成 View 视图对象，最后对 View 进行渲染将处理结果通过页面展示给用户。

-  View：视图
  SpringMVC 框架提供了很多的 View 视图类型的支持，包括： jstlView、 freemarkerView、 pdfView等。我们最常用的视图就是 jsp。
  一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由程序员根据业务需求开发具体的页面。 

#### 2.4  springmvc配置文件 <mvc:annotation-driven >的说明

在 SpringMVC 的各个组件中，处理器映射器、处理器适配器、视图解析器称为 SpringMVC 的三大组件。使 用 <mvc:annotation-driven > 自 动 加 载 RequestMappingHandlerMapping （ 处 理 映 射 器 ） 和RequestMappingHandlerAdapter （ 处 理 适 配 器 ） ， 可 用 在 SpringMVC.xml 配 置 文 件 中 使 用<mvc:annotation-driven >替代注解处理器和适配器的配置 

#### 2.5  RequestMapping 注解 

> @Target({ElementType.METHOD, ElementType.TYPE})
> @Retention(RetentionPolicy.RUNTIME)
> @Documented
> @Mapping
> public @interface RequestMapping {
> } 

- 作用用于建立请求 URL 和处理请求方法之间的对应关系 

- 作用位置： 
  - 类上：
    请求 URL 的第一级访问目录。此处不写的话，就相当于应用的根目录。 写的话需要以/开头。
    它出现的目的是为了使我们的 URL 可以按照**模块化管理**:
    账户模块：                    订单模块：
    /account/add               /order/add
    /account/update        /order/update
    /account/delete         /order/delete
  - 方法上：
    请求 URL 的第二级访问目录 

- 属性： 

  value：用于指定请求的 URL。 它和 path 属性的作用是一样的。
  method：用于指定请求的方式。
  params：用于指定限制请求参数的条件。 它支持简单的表达式。 要求请求参数的 key 和 value 必须和配置的一模一样。

  例如：
  params = {"accountName"}，表示请求参数必须有 accountName
  params = {"moeny!100"}，表示请求参数中 money 不能是 100。
  headers：用于指定限制请求消息头的条件。

### 第三章 请求参数的绑定 

#### 3.1 绑定的机制

1. 表单提交的数据都是k=v格式的 username=haha&password=123
2. SpringMVC的参数绑定过程是把表单提交的请求参数，作为控制器中方法的参数进行绑定的
3. 要求：提交表单的name和参数的名称是相同的 

```java
@RequestMapping("/testParam")
    public String testParam(String username, String password){
        System.out.println("执行了...");
        System.out.println("用户名：" + username);
        System.out.println("密码：" + password);
        return "success";
    }
```

#### 3.2 支持的数据类型

- 基本类型参数：
  包括基本类型和 String 类型

  > 要求我们的参数名称必须和控制器中方法的形参名称保持一致。 (严格区分大小写) 

- POJO 类型参数：
  包括实体类，以及关联的实体类

  > 要求表单中参数名称和 POJO 类的属性名称保持一致。并且控制器方法的参数类型是 POJO 类型。

  ```java
      /**
       * 请求参数绑定:把数据封装到JavaBean类
       * @param account
       * @return
       */
      @RequestMapping("/saveAccount")
      public String testParam(Account account){
          System.out.println("执行了...");
          System.out.println(account);
          return "success";
      }
  ```

- 数组和集合类型参数：
  包括 List 结构和 Map 结构的集合（包括数组）

  > 第一种：
  > 要求集合类型的请求参数必须在 POJO 中。在表单中请求参数名称要和 POJO 中集合属性名称相同。
  > 给 List 集合中的元素赋值， 使用下标。
  > 给 Map 集合中的元素赋值， 使用键值对。
  >
  > 第二种：
  > 接收的请求参数是 json 格式数据。需要借助一个注解实现。 

#### 3.3 自定义类型转换器 

第一步：定义一个类，实现 Converter 接口，该接口有两个泛型。

```java
/**
 * @author wmxyyy
 * @date 2020/1/16 20:34
 * @state 把字符串转换日期
 */
public class StringToDateConverter implements Converter<String, Date> {
    /**
     * @param source
     * @return
     */
    @Override
    public Date convert(String source) {
        //判断
        if (source == null){
            throw new RuntimeException("请您传入数据");
        }
        DateFormat df = new SimpleDateFormat("yyyy-MM-dd");
        try {
            //字符串转换日期
            return df.parse(source);
        } catch (ParseException e) {
            e.printStackTrace();
        }
        throw new RuntimeException("数据类型转换出现错误");
    }
}
```

第二步：在 spring 配置文件中配置类型转换器。 

```java
<!--配置自定义类型转换器-->
    <bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
        <property name="converters">
            <set>
                <bean class="com.wmxyyy.utils.StringToDateConverter"></bean>
            </set>
        </property>
    </bean>
```

第三步：在 annotation-driven 标签中引用配置的类型转换服务 

```java
<mvc:annotation-driven conversion-service="conversionService"/>
```

#### 3.4 使用 ServletAPI 对象作为方法参数 

SpringMVC 还支持使用原始 ServletAPI 对象作为控制器方法的参数。支持原始 ServletAPI 对象有：
HttpServletRequest
HttpServletResponse
HttpSession
...

```java
    /**
     * 原生的API
     * @return
     */
    @RequestMapping("/testServlet")
    public String testServlet(HttpServletRequest request, HttpServletResponse response){
        System.out.println("执行了...");
        System.out.println(request);
        System.out.println(response);

        HttpSession session = request.getSession();
        System.out.println(session);

        ServletContext servletContext = session.getServletContext();
        System.out.println(servletContext);
        return "success";
    }
```

### 第四章 常用注解 

#### 4.1 RequestParam 

##### 作用：

​	把请求中指定名称的参数给控制器中的形参赋值。

##### 属性：

​	value： 请求参数中的名称。
​	required：请求参数中是否必须提供此参数。 默认值： true。表示必须提供，如果不提供将报错。 

```java
@RequestMapping("/testRequestParam")
    public String testRequestParam(@RequestParam("name") String username){
        System.out.println("执行了...");
        System.out.println(username);
        return "success";
    }
```

#### 4.2 RequestBody 

##### 作用：

用于获取请求体内容。 直接使用得到是 key=value&key=value...结构的数据。
get 请求方式不适用。

##### 属性：

required：是否必须有请求体。默认值是:true。当取值为 true 时,get 请求方式会报错。如果取值
为 false， get 请求得到是 null。 

```java
    /**
     * 获取到请求体的内容
     * @return
     */
    @RequestMapping("/testRequestBody")
    public String testRequestBody(@RequestBody String body){
        System.out.println("执行了...");
        System.out.println(body);
        return "success";
    }
```

#### 4.3 PathVaribale 

##### 作用：

用于绑定 url 中的占位符。 例如：请求 url 中 /delete/{id}， 这个{id}就是 url 占位符。
url 支持占位符是 spring3.0 之后加入的。是 springmvc 支持 rest 风格 URL 的一个重要标志。

##### 属性：

value： 用于指定 url 中占位符名称。
required：是否必须提供占位符 

```java
/**
     * PathVariable注解
     * @return
     */
    @RequestMapping(value="/testPathVariable/{sid}")
    public String testPathVariable(@PathVariable(name="sid") String id){
        System.out.println("执行了...");
        System.out.println(id);
        return "success";
    }
```

#### 4.4 RequestHeader 

##### 作用：

用于获取请求消息头

##### 属性：

value：提供消息头名称
required：是否必须有此消息头

> 注：在实际开发中一般不怎么用

```java
    /**
     * 获取请求头的值
     * @param header
     * @return
     */
    @RequestMapping(value="/testRequestHeader")
    public String testRequestHeader(@RequestHeader(value="Accept") String header){
        System.out.println("执行了...");
        System.out.println(header);
        return "success";
    }
```

#### 4.5 CookieValue 

##### 作用：

用于把指定 cookie 名称的值传入控制器方法参数。

##### 属性：

value：指定 cookie 的名称。
required：是否必须有此 cookie。

```java
    /**
     * 获取Cookie的值
     * @return
     */
    @RequestMapping(value="/testCookieValue")
    public String testCookieValue(@CookieValue(value="JSESSIONID") String cookieValue){
        System.out.println("执行了...");
        System.out.println(cookieValue);
        return "success";
    }
```

#### 4.6 ModelAttribute 

##### 作用：

1. 出现在方法上：表示当前方法会在控制器方法执行前线执行。
2. 出现在参数上：获取指定的数据给参数赋值 

##### 属性：

value：用于获取数据的 key。 key 可以是 POJO 的属性名称，也可以是 map 结构的 key。

##### 应用场景：

当提交表单数据不是完整的实体数据时，保证没有提交的字段使用数据库原来的数据 

1. 修饰的方法有返回值 

   ```java
   /**
   * 作用在方法，先执行
   * @param name
   * @return
   */
   @ModelAttribute
   public User showUser(String name) {
   	System.out.println("showUser执行了...");
   	// 模拟从数据库中查询对象
   	User user = new User();
   	user.setName("哈哈");
   	user.setPassword("123");
   	user.setMoney(100d);
   	return user;
   } 
   /
   **
   * 修改用户的方法
   * @param cookieValue
   * @return
   */
   @RequestMapping(path="/updateUser")
   public String updateUser(User user) {
   	System.out.println(user);
   	return "success";
   }
   ```

2. 修饰的方法没有返回值 

   ```java
   /
   **
   * 作用在方法，先执行
   * @param name
   * @return
   */
   @ModelAttribute
   public void showUser(String name,Map<String, User> map) {
   	System.out.println("showUser执行了...");
   	// 模拟从数据库中查询对象
   	User user = new User();
   	user.setName("哈哈");
   	user.setPassword("123");
   	user.setMoney(100d);
   	map.put("abc", user);
   } 
   /
   **
   * 修改用户的方法
   * @param cookieValue
   * @return
   */
   @RequestMapping(path="/updateUser")
   public String updateUser(@ModelAttribute(value="abc") User user) {
   	System.out.println(user);
   	return "success";
   }
   ```

#### 4.7 SessionAttribute 

##### 作用：

用于多次执行控制器方法间的参数共享。

##### 属性：

value：用于指定存入的属性名称
type：用于指定存入的数据类型 

```java
@Controller
@RequestMapping(path="/user")
@SessionAttributes(value= {"username","password","age"},types=
{String.class,Integer.class}) // 把数据存入到session域对象中
public class HelloController{
	/**
     * SessionAttributes的注解
     * @return
     */
    @RequestMapping(value="/testSessionAttributes")
    public String testSessionAttributes(Model model){
        System.out.println("testSessionAttributes...");
        // 底层会存储到request域对象中
        model.addAttribute("msg","美美");
        return "success";
    }

    /**
     * 获取值
     * @param modelMap
     * @return
     */
    @RequestMapping(value="/getSessionAttributes")
    public String getSessionAttributes(ModelMap modelMap){
        System.out.println("getSessionAttributes...");
        String msg = (String) modelMap.get("msg");
        System.out.println(msg);
        return "success";
    }

    /**
     * 清除
     * @param status
     * @return
     */
    @RequestMapping(value="/delSessionAttributes")
    public String delSessionAttributes(SessionStatus status){
        System.out.println("getSessionAttributes...");
        status.setComplete();
        return "success";
    }
}

```



