# 第一章：

## 入门Spring：

Spring IoC

• Inversion of Control 

\- 控制反转，是一种面向对象编程的设计思想。

• Dependency Injection

\- 依赖注入，是IoC思想的实现方式。

• IoC Container 

\- IoC容器，是实现依赖注入的关键，本质上是一个工厂。

Spring容器最基本的接口就是BeanFactory。BeanFactory负责配置、创建、管理Bean；

ApplicationContext由BeanFactory派生而来；BeanFactory的许多功能需要编程实现，而在ApplicationContext中则可以通过配置的方式实现；

ApplicationContext.getBean(类.class)-->获取某一个对象

----

其实就是

Spring容器里面会主动实例化Bean类，然后可以通过反射机制->拿到类。
要容器扫到Bean并且加载进去的话，那么就要加上注解(有四个)：
1.@Component、@Controller、@Service、@Repository

2.依赖注入@Autowired->就不需要通过手动ApplicationContext.getBean(类.class)-->获取某一个对象。就会自动给我们的类注入实例。一般来说就是声明 接口或者父类 注入的是子类。这样的话就使用了多态的性质。

---

### 一、@RequestMapping 基础用法

用于将任意HTTP 请求映射到控制器方法上。

@RequestMapping表示共享映射，如果没有指定请求方式，将接收GET、POST、HEAD、OPTIONS、PUT、PATCH、DELETE、TRACE、CONNECT所有的HTTP请求方式。@GetMapping、@PostMapping、@PutMapping、@DeleteMapping、@PatchMapping 都是HTTP方法特有的快捷方式@RequestMapping的变体，分别对应具体的HTTP请求方式的映射注解。

@RequestMapping 注解可以在控制器类上和控制器类中的方法上使用。
在类的级别上的注解会将一个特定请求或者请求模式映射到一个控制器之上。之后你还可以另外添加方法级别的注解来进一步指定到处理方法的映射关系。

需要注意的是，控制器方法都应该映射到一个特定的HTTP方法，而不是使用@RequestMapping共享映射。

在控制器类上和控制器类中的方法上使用
将映射都放到方法上：

```java
@RestController
public class UserController {
		// 映射到方法上
		// localhost:8080/user/login
		// 此处通常用 @GetMapping("/user/login") 表明GET请求方式的映射，因为login登录只需向服务器获取用户数据。
    @RequestMapping("/user/login")  
    public String login() {
        return "user login";
    }
    
    // 映射到方法上
    // localhost:8080/user/register
    // 此处通常用 @PostMapping("/user/login") 表明POST请求方式的映射，因为register注册需要向服务器提交用户数据。
    @RequestMapping("/user/register")
    public String register() {
        return "user register";
    }
}

```

如上述代码所示，到 /user/login 的请求会由 login() 方法来处理，而到 /user/register的请求会由 register() 来处理。

下面是一个同时在类和方法上应用了 @RequestMapping 注解的示例，上述代码与如下代码等价：

```java
@RestController
// 映射到类上
// localhost:8080/user
@RequestMapping("/user")
public class UserController {
		// 映射到方法上
		// localhost:8080/user/login
		// 此处通常用 @GetMapping("/user/login") 表明GET请求方式的映射
    @RequestMapping("/login") 
    public String login() {
        return "user login";
    }
    
    // 映射到方法上
    // localhost:8080/user/register
    // 此处通常用 @PostMapping("/user/login") 表明POST请求方式的映射
    @RequestMapping("/register")
    public String register() {
        return "user register";
    }
}

```

### 二、@RequestMapping 返回值

### 1.4Spring MVC

![image-20231118155547020](C:\Users\Rutong\AppData\Roaming\Typora\typora-user-images\image-20231118155547020.png)

浏览器给服务器发送请求：

1.Controller接受数据，然后调用业务层，业务层调用数据库，拿到“数据”。

2.把数据封装到Model层。

3.最后再给到视图层。

两两的三层其实并没有什么关系。

-----

![image-20231118160528649](C:\Users\Rutong\AppData\Roaming\Typora\typora-user-images\image-20231118160528649.png)

这里多加了一个前端控制器。

![image-20231118161621463](E:\Typroa_Picture\image-20231118161621463.png)



1. **DispatcherServlet：**

   - `DispatcherServlet` 是 Spring MVC 框架中的一个关键组件，用于接收客户端的请求并将请求分派给合适的处理器（Controller）进行处理。
   - 它是前端控制器（Front Controller）的实现，负责协调整个请求处理过程。

2. **Thymeleaf：**

   - `Thymeleaf` 是一种用于Web和独立环境的现代服务器端Java模板引擎。它允许开发人员构建自然模板，这些模板在浏览器中查看时会以有效的方式显示，也可以用作静态建模。

   - 在 Spring MVC 中，Thymeleaf 可以作为视图层的一部分，负责渲染模板并将动态数据插入到 HTML 页面中.

     

     - `DispatcherServlet` 负责处理请求的分发，它通过配置和注解找到合适的 `Controller` 处理请求。
     - 当 `Controller` 处理完请求并确定了要返回的视图时，通常它会返回一个视图名称（例如 "home"）。
     - 这个视图名称会被 `DispatcherServlet` 传递给视图解析器（View Resolver）。
     - 视图解析器将视图名称解析为实际的视图对象，而在使用 Thymeleaf 作为视图技术时，这个实际的视图对象就是 Thymeleaf 视图。
     - Thymeleaf 视图负责渲染 HTML 模板，将模型数据插入到模板中，最终生成客户端需要的 HTML 响应。

     综言之，`DispatcherServlet` 与 Thymeleaf 之间的关系是协同工作，`DispatcherServlet` 负责请求分发和处理器的调度，而 Thymeleaf 负责渲染视图模板。在 Spring MVC 中，它们共同构建了一个完整的请求处理和视图渲染流程。



### 1.5Mybatis

1.安装数据库，确保数据库能够使用

```java

```

2.需要去maven repository里面找出Mysql依赖

```xml
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>5.1.49</version>
	</dependency>
	
	
```
3.导入mybatis依赖

```xml
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>2.0.1</version>
		</dependency>
```

4.给Springboot（application.properties）配置Mysql与Mybatis属性（）

```properties
# DataSourceProperties
spring.datasource.driver-class-name=com.mysql.jdbc.Driver#数据库驱动
spring.datasource.url=jdbc:mysql://localhost:3306/community?characterEncoding=utf-8&useSSL=false&serverTimezone=Hongkong//community是数据库名字
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.type=com.zaxxer.hikari.HikariDataSource//线程池
spring.datasource.hikari.maximum-pool-size=15
spring.datasource.hikari.minimum-idle=5
spring.datasource.hikari.idle-timeout=30000

# MybatisProperties
mybatis.mapper-locations=classpath:mapper/*.xml//配置映射文件路径
mybatis.type-aliases-package=com.tong.community.entity//实体类路径，以后就写XML文件里面，实体类直接就用就行
mybatis.configuration.useGeneratedKeys=true
mybatis.configuration.mapUnderscoreToCamelCase=true //支持驼峰命名
```

![image-20231118224024911](E:\Typroa_Picture\image-20231118224024911.png)

注意：

```
你使用的 MySQL 数据库驱动依赖是 mysql-connector-java，版本为 5.1.49。在 Spring Boot 项目中，你可以将数据库驱动类名配置为：
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
请注意，这是针对 MySQL Connector/J 版本 5.x 的配置。如果你将 MySQL Connector/J 升级到版本 8.x，则需要使用新的驱动类名：


spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
在新版本的 MySQL Connector/J 中，驱动类名变更为 com.mysql.cj.jdbc.Driver。确保根据你使用的 MySQL Connector/J 版本选择正确的驱动类名。
```

4.使用

- 创建entity包，里面几乎都是放数据库中拿来数据的实体

  ![image-20231118224520024](E:\Typroa_Picture\image-20231118224520024.png)

  

- 创建方法接口(Mapper)：需要对这个类进行什么操作。具体SQL语句在mybatis.XML配置。

  ![image-20231118224600631](E:\Typroa_Picture\image-20231118224600631.png)

- 在mapper资源中写具体某一个类的映射文件

  ![image-20231118224708257](E:\Typroa_Picture\image-20231118224708257.png)

```
@Autowired//自动装载在容器中就可以拿对象使用
UserMapper userMapper
```

```java
编写UserMapper接口
package com.nowcoder.community.dao;

import com.nowcoder.community.entity.User;
import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface UserMapper {

    User selectById(int id);

    User selectByName(String username);

    User selectByEmail(String email);

    int insertUser(User user);

    int updateStatus(int id, int status);

    int updateHeader(int id, String headerUrl);

    int updatePassword(int id, String password);

}

```

```java
使用UserMapper接口，当一个对象使用就行了，因为他已经主动在容器中构建出一个对象了。并不需要成需要手动实现一个类。
package com.nowcoder.community;

import com.nowcoder.community.dao.UserMapper;
import com.nowcoder.community.entity.User;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.Date;

@RunWith(SpringRunner.class)
@SpringBootTest
@ContextConfiguration(classes = CommunityApplication.class)
public class MapperTests {

    @Autowired
    private UserMapper userMapper;

    @Test
    public void testSelectUser() {
        User user = userMapper.selectById(101);
        System.out.println(user);

        user = userMapper.selectByName("liubei");
        System.out.println(user);

        user = userMapper.selectByEmail("nowcoder101@sina.com");
        System.out.println(user);
    }

    @Test
    public void testInsertUser() {
        User user = new User();
        user.setUsername("test");
        user.setPassword("123456");
        user.setSalt("abc");
        user.setEmail("test@qq.com");
        user.setHeaderUrl("http://www.nowcoder.com/101.png");
        user.setCreateTime(new Date());

        int rows = userMapper.insertUser(user);
        System.out.println(rows);
        System.out.println(user.getId());
    }

    @Test
    public void updateUser() {
        int rows = userMapper.updateStatus(150, 1);
        System.out.println(rows);

        rows = userMapper.updateHeader(150, "http://www.nowcoder.com/102.png");
        System.out.println(rows);

        rows = userMapper.updatePassword(150, "hello");
        System.out.println(rows);
    }

}

```

### 1.6首页构建

1.一般是先构建Dao层：用于实现对数据库信息查询->返回信息。

2.构造service层，调用Dao层类型，拿出结果。

3.Controller层 调用service层，取到了结果。与前端控制器进行交互。

```java

@Controller
public class HomeController {

    @Autowired
    private DiscussPostService discussPostService;

    @Autowired
    private UserService userService;

    @RequestMapping(path = "/index", method = RequestMethod.GET)
    public String getIndexPage(Model model, Page page) {
        // 方法调用钱,SpringMVC会自动实例化Model和Page,并将Page注入Model.
        // 所以,在thymeleaf中可以直接访问Page对象中的数据.
        /**
        	model只是一个容器，把返回值discussPosts，与输入的参数Page装载，送到前端控制器。
        	那么在前端就可以直接当对象使用了。
        **/
        page.setRows(discussPostService.findDiscussPostRows(0));
        page.setPath("/index");

        List<DiscussPost> list = discussPostService.findDiscussPosts(0, page.getOffset(), page.getLimit());
        List<Map<String, Object>> discussPosts = new ArrayList<>();
        if (list != null) {
            for (DiscussPost post : list) {
                Map<String, Object> map = new HashMap<>();
                map.put("post", post);
                User user = userService.findUserById(post.getUserId());
                map.put("user", user);
                discussPosts.add(map);
            }
        }
        model.addAttribute("discussPosts", discussPosts);
        return "/index";
    }

}

```

```XML
//前端交互代码
<li th:class="|page-item ${page.current==page.total?'disabled':''}|">
	<a class="page-link" th:href="@{${page.path}(current=${page.current+1})}">下一页</a>
</li>

	当你点击下一页并触发了/index的GET请求时，Spring MVC会尝试通过请求参数来填充Page对象。具体来说，在Thymeleaf模板中，你通过th:href="@{${page.path}(current=${page.current+1})}"传递了一个名为 current 的参数，这个参数会被Spring MVC尝试绑定到Page对象的current属性上。
	在Controller方法中，Spring MVC会自动创建一个Page对象，并尝试将请求中的参数绑定到该对象的属性上。因此，page.current 的值应该在点击下一页后被更新为原来的值加一。
```

### 1.7调试

1.F7进入函数体，F8下一条语句，F9跳转到下一个断点



2.创建Logger类

```java
public class LoggerTests {
    //用工厂对象创建Logger对象，传入的是一个名字。给logger赋名。一般都取类的名字
    private static final Logger logger= LoggerFactory.getLogger(LoggerTests.class);
    @Test
    public void logertests(){
        System.out.println(logger.getName());
        logger.debug("debug-logger");
        logger.info("info-logger");
        logger.warn("warn-logger");
        logger.error("error-logger");
    }
}
```

3.配置一个XML文件

```Xml

<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <contextName>community</contextName>
    <property name="LOG_PATH" value="E:/work/java_log"/>
    <property name="APPDIR" value="community"/>

    <!-- error file -->
    <appender name="FILE_ERROR" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${LOG_PATH}/${APPDIR}/log_error.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${LOG_PATH}/${APPDIR}/error/log-error-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>5MB</maxFileSize> 
            </timeBasedFileNamingAndTriggeringPolicy>
            <maxHistory>30</maxHistory>
        </rollingPolicy>
        <append>true</append>
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <pattern>%d %level [%thread] %logger{10} [%file:%line] %msg%n</pattern>
            <charset>utf-8</charset>
        </encoder>
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>error</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch> 
        </filter>
    </appender>

    <!-- warn file -->
    <appender name="FILE_WARN" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${LOG_PATH}/${APPDIR}/log_warn.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${LOG_PATH}/${APPDIR}/warn/log-warn-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>5MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
            <maxHistory>30</maxHistory>
        </rollingPolicy>
        <append>true</append>
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <pattern>%d %level [%thread] %logger{10} [%file:%line] %msg%n</pattern>
            <charset>utf-8</charset>
        </encoder>
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>warn</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>

    <!-- info file -->
    <appender name="FILE_INFO" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${LOG_PATH}/${APPDIR}/log_info.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${LOG_PATH}/${APPDIR}/info/log-info-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>5MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
            <maxHistory>30</maxHistory>
        </rollingPolicy>
        <append>true</append>
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <pattern>%d %level [%thread] %logger{10} [%file:%line] %msg%n</pattern>
            <charset>utf-8</charset>
        </encoder>
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>info</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>

    <!-- console -->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d %level [%thread] %logger{10} [%file:%line] %msg%n</pattern>
            <charset>utf-8</charset>
        </encoder>
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>debug</level>
        </filter>
    </appender>

    <logger name="com.nowcoder.community" level="debug"/>

    <root level="info">
        <appender-ref ref="FILE_ERROR"/>
        <appender-ref ref="FILE_WARN"/>
        <appender-ref ref="FILE_INFO"/>
        <appender-ref ref="STDOUT"/>
    </root> 

</configuration>
```

### 1.8Git

```
配置：用户名
git config --global user.name "wenrutong"
配置：邮箱
git config --global user.email "1158346590@qq.com"

需要把本地代码存到本地仓库才行：你要管理哪个项目就需要用Git工具进入改项目

```

![image-20231120163059301](E:\Typroa_Picture\image-20231120163059301.png)

```
1.要给git工具管理，必须进行git初始化
 git init
2.把文件添加到本地仓库(只是临时存储)
git add* (所有文件)

```

![image-20231207162547345](E:\Typroa_Picture\image-20231207162547345.png)

# 第二章

### 2.1发送邮件

用到的技术：

• 邮箱设置

​		启用客户端SMTP服务

• Spring Email 

​		导入 jar 包

​		邮箱参数配置

​		使用 JavaMailSender 发送邮件

• 模板引擎

​		使用 Thymeleaf 发送 HTML 邮件

```xml
导入jar包依赖
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-mail</artifactId>
			<version>2.2.6.RELEASE</version>
		</dependency>

```

```properties
#配置application配置，设置邮箱参数
# MailProperties
#使用新浪客户端帮我们发送消息
spring.mail.host=smtp.sina.com
spring.mail.port=465 
spring.mail.username=wrt1158346590@sina.com
spring.mail.password=“页面生成的校验码”
spring.mail.protocol=smtps
spring.mail.properties.mail.smtp.ssl.enable=true
```

创建工具了，因为他不属于SpringMVC的3层的基本用例



```java
JavaMailSender 是 Spring Framework 提供的一个邮件发送的接口，用于简化 JavaMail API 的使用。它允许你发送邮件，包括纯文本、HTML 和带有附件的邮件。
JavaMailSender:

JavaMailSender 是 Spring 框架对 JavaMail API 的封装，提供了一组简化邮件发送的方法。
通过 JavaMailSender，你可以配置邮件服务器的连接信息，包括主机、端口、用户名、密码等。
MimeMessage:

MimeMessage 是 JavaMail API 中的一个类，用于表示一封 MIME 格式的邮件。MIME（Multipurpose Internet Mail Extensions）是一种扩展邮件标准，支持在邮件中包含多媒体和非ASCII文本。
通过 mailSender.createMimeMessage() 创建了一个 MimeMessage 对象，这个对象可以用来构建邮件内容，包括设置收件人、发件人、主题、正文等。
MimeMessageHelper:

MimeMessageHelper 是 Spring 提供的一个辅助类，用于更方便地构建 MimeMessage。
通过 new MimeMessageHelper(message) 创建了一个 MimeMessageHelper 对象，这个对象提供了一些便捷的方法来设置邮件的各个部分，例如设置收件人、发件人、主题、正文，以及添加附件等。
@Component
public class MailClient {
    //一般发送邮件需要添加日志
    private static final Logger logger= LoggerFactory.getLogger(MailClient.class);

    @Autowired
    private JavaMailSender mailSender;
    //这个值是邮件发送方
    @Value("${spring.mail.username}")
    private String from;

    public void sendMail(String to, String subject, String content) {
        try {
            MimeMessage message = mailSender.createMimeMessage();
            MimeMessageHelper helper = new MimeMessageHelper(message);
            helper.setFrom(from);
            helper.setTo(to);
            helper.setSubject(subject);
            helper.setText(content, true);
            mailSender.send(helper.getMimeMessage());
        } catch (MessagingException e) {
            logger.error("发送邮件失败:" + e.getMessage());
        }
    }
}

```

### 2.2注册页面

1.Service层开发：插入一个User用户的业务逻辑

```java
@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    //用来发邮件的
    @Autowired
    private MailClient mailClient;

    //可以使得邮件的模板带上HTML
    @Autowired
    private TemplateEngine templateEngine;

    @Value("${community.path.domain}")
    private String domin;

    @Value("${server.servlet.context-path}")
    private String contextPath;

    //注册功能
    public Map<String,Object> register(User user){
        Map<String,Object> map=new HashMap<>();
        //空值处理-------------------------
        if(user==null){
            throw new IllegalArgumentException("参数不能为空");
        }
        if (StringUtils.isBlank(user.getUsername())) {
            map.put("usernameMsg", "账号不能为空!");
            return map;
        }
        if (StringUtils.isBlank(user.getPassword())) {
            map.put("passwordMsg", "密码不能为空!");
            return map;
        }
        if (StringUtils.isBlank(user.getEmail())) {
            map.put("emailMsg", "邮箱不能为空!");
            return map;
        }
        //验证输入User的内容是否已经存在
        User u=userMapper.selectByName(user.getUsername());
        if(u!=null){
            map.put("usernameMsg","账号已经存在");
            return map;
        }

        u=userMapper.selectByEmail(user.getEmail());
        if(u!=null){
            map.put("passwordMsg","邮箱已经存在");
            return map;
        }

        //----上述都是有问题才会返回map有内容，那么没问题就把数据补全
        user.setSalt(CommunityUtil.generateUUID().substring(0,5));
        user.setPassword(CommunityUtil.md5(user.getPassword()+user.getSalt()));
        user.setType(0);
        user.setStatus(0);
        user.setActivationCode(CommunityUtil.generateUUID());//激活码为随机字符串中拿
        user.setHeaderUrl(String.format("http://images.nowcoder.com/head/%dt.png",new Random().nextInt(1000)));
        user.setCreateTime(new Date());
        userMapper.insertUser(user);

        //成功注册，没有return。--发送激活邮件
        //这里是给templateleft设置属性
        Context context=new Context();
        context.setVariable("eamil",user.getEmail());
        // http://localhost:8080/community/activation/101/code
        //把整个动态页面发给用户
        String url=domin+contextPath+"activation"+user.getId()+user.getActivationCode();
        context.setVariable("url",url);
        String content=templateEngine.process("/mail/activation",context);
        mailClient.sendMail(user.getEmail(),"激活账号",content);

        return map;
    }

}

```

2.Controller层调用Service层的功能。并且与前端页面进行交互

```java
@Controller
public class LoginController   {

    @Autowired
    private UserService userService;

    @RequestMapping(path = "/register", method = RequestMethod.GET)
    public String getRegisterPage() {
        return "/site/register";
    }

    @RequestMapping(path = "/login", method = RequestMethod.GET)
    public String getLoginPage() {
        return "/site/login";
    }

    @RequestMapping(path = "/register", method = RequestMethod.POST)
    public String register(Model model, User user) {
        Map<String, Object> map = userService.register(user);
        if (map == null || map.isEmpty()) {
            model.addAttribute("msg", "注册成功,我们已经向您的邮箱发送了一封激活邮件,请尽快激活!");
            model.addAttribute("target", "/index");//返回首页
            return "/site/operate-result";
        } else {
            model.addAttribute("usernameMsg", map.get("usernameMsg"));
            model.addAttribute("passwordMsg", map.get("passwordMsg"));
            model.addAttribute("emailMsg", map.get("emailMsg"));
            return "/site/register";
        }
    }
}
```

### 2.3激活账号

创建Service层逻辑，处理激活函数。(参数需要Controller层传下来)

```java
 public int activation(int userId,String code){
        User user=userMapper.selectById(userId);
        int status=user.getStatus();
        if(status==1){
            return ACTIVATION_REPEAT;//重复激活
        }else if(status==0){
            userMapper.updateStatus(userId,1);
            return ACTIVATION_SUCCESS;
        }else{
            return ACTIVATION_FAILURE;
        }
    }
```

创建Controller层

```java
// http://localhost:8080/community/activation/101/code
    //用户点击激活链接就会跳转到这个页面，Controller处理
    //在URL中使用 /XX/XX 这样的形式作为路径变量，是一种RESTful风格的设计。101 是userId的值，而code是 code 的值。
    @RequestMapping(path = "/activation/{userId}/{code}", method = RequestMethod.GET)
    public String activation(Model model, @PathVariable("userId") int userId, @PathVariable("code") String code) {
        int result = userService.activation(userId, code);
        if (result == ACTIVATION_SUCCESS) {
            model.addAttribute("msg", "激活成功,您的账号已经可以正常使用了!");
            model.addAttribute("target", "/login");
        } else if (result == ACTIVATION_REPEAT) {
            model.addAttribute("msg", "无效操作,该账号已经激活过了!");
            model.addAttribute("target", "/index");
        } else {
            model.addAttribute("msg", "激活失败,您提供的激活码不正确!");
            model.addAttribute("target", "/index");
        }
        return "/site/operate-result";
    }
```

### 2.4会话管理

1.Cookie

![ ](E:\Typroa_Picture\image-20231121154841202.png)

```java
   // cookie示例
    //cookie是发送给浏览器的，通过response方法发送。
    @RequestMapping(path = "/cookie/set", method = RequestMethod.GET)
    @ResponseBody
    public String setCookie(HttpServletResponse response) {
        // 创建cookie
        Cookie cookie = new Cookie("code", CommunityUtil.generateUUID());
        // 设置cookie生效的范围
        cookie.setPath("/community/alpha");
        // 设置cookie的生存时间
        cookie.setMaxAge(60 * 10);
        // 发送cookie
        response.addCookie(cookie);

        return "set cookie";
    }

    @RequestMapping(path = "/cookie/get", method = RequestMethod.GET)
    @ResponseBody
    public String getCookie(@CookieValue("code") String code) {
        System.out.println(code);
        return "get cookie";
    }
```

2.Session

服务器有许多浏览器的Session，所以需要用Cookie识别哪一个Session是该浏览器的。 Cookie保存了SessionId

![image-20231121160459497](E:\Typroa_Picture\image-20231121160459497.png)

```java
 // session示例
//session是SpringMVC自动帮我们注入，可以直接使用
    @RequestMapping(path = "/session/set", method = RequestMethod.GET)
    @ResponseBody
    public String setSession(HttpSession session) {
        session.setAttribute("id", 1);
        session.setAttribute("name", "Test");
        return "set session";
    }

    @RequestMapping(path = "/session/get", method = RequestMethod.GET)
    @ResponseBody
    public String getSession(HttpSession session) {
        System.out.println(session.getAttribute("id"));
        System.out.println(session.getAttribute("name"));
        return "get session";
    }
```

### 2.5生成验证码

![image-20231121164428445](E:\Typroa_Picture\image-20231121164428445.png)

1.生成验证码需要第三方包，导入依赖

```xml
		<dependency>
			<groupId>com.github.penggle</groupId>
			<artifactId>kaptcha</artifactId>
			<version>2.3.2</version>
		</dependency>
```

2.Spring boot没有整合这个包，所以需要手动 配置配置文件。但是XML方式比较难看，可以选择用类的方式配置

```java
@Configuration
public class KaptchaConfig {
    @Bean
    public Producer kaptchaProducer() {
        Properties properties = new Properties();
        properties.setProperty("kaptcha.image.width", "100");
        properties.setProperty("kaptcha.image.height", "40");
        properties.setProperty("kaptcha.textproducer.font.size", "32");
        properties.setProperty("kaptcha.textproducer.font.color", "0,0,0");
        properties.setProperty("kaptcha.textproducer.char.string", "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYAZ");
        properties.setProperty("kaptcha.textproducer.char.length", "4");
        properties.setProperty("kaptcha.noise.impl", "com.google.code.kaptcha.impl.NoNoise");

        DefaultKaptcha kaptcha = new DefaultKaptcha();
        Config config = new Config(properties);
        kaptcha.setConfig(config);
        return kaptcha;
    }
}

```

3.解释说明

这段代码是一个使用 Spring 的 Java Config 类，用于配置 Kaptcha（验证码生成库）的一个 Bean。让我一步步解释这段代码：

1. **`@Configuration` 注解：**
   - 表示这是一个配置类，用于定义 Spring 应用上下文中的 Bean 配置。
2. **`@Bean` 注解：**
   - 标识了一个方法 `kaptchaProducer()`，该方法返回一个对象，这个对象将被注册为一个 Spring Bean。
   - 方法中配置了 Kaptcha 的属性，因此这个 Bean 是一个 Kaptcha 的实例。
3. **Kaptcha 属性配置：**
   - 通过创建一个 `Properties` 对象，设置了一系列 Kaptcha 的配置属性。这些属性包括验证码图片的宽度、高度、字体大小、字体颜色、验证码字符的范围、验证码字符的长度等。
   - 这些属性会影响最终生成的验证码的外观和行为。
4. **`DefaultKaptcha` 和 `Config` 对象：**
   - 创建了一个 `DefaultKaptcha` 对象，它是 Kaptcha 提供的默认实现。
   - 创建了一个 `Config` 对象，用于将配置应用到 `DefaultKaptcha` 中。
5. **将配置应用到 Kaptcha：**
   - 通过 `kaptcha.setConfig(config)` 将上述配置应用到 `DefaultKaptcha` 对象中。
6. **返回 Kaptcha Bean：**
   - `kaptchaProducer()` 方法返回了一个 `DefaultKaptcha` 对象，该对象被注册为一个 Spring Bean，并可以在应用的其他部分通过注入的方式使用。

```java
1.public class DefaultKaptcha extends Configurable implements Producer
2.Properties是JAVASE的自带的对象import java.util.Properties;
```

**4.拥有了生成校验码的方式，就可以去实现一个“效验码使用方法”**

```java
@RequestMapping(path = "/kaptcha", method = RequestMethod.GET)
    public void getKaptcha(HttpServletResponse response, HttpSession session){
        //生成验证码
        String text=kaptchaProducer.createText();
        BufferedImage image=kaptchaProducer.createImage(text);
        //将验证码存入session
        session.setAttribute("kaptcha",text);
        //讲图片输出给浏览器
        response.setContentType("image/png");
        try {
            OutputStream os=response.getOutputStream();
            ImageIO.write(image,"png",os);
        } catch (IOException e) {
            logger.error("相应校验码失败"+e.getMessage());
        }
    }
```

解释内容：


1. **生成验证码：**
   - 使用之前配置的 `kaptchaProducer`（类型为 `Producer` 接口）来生成验证码文本和图片。
   - `String text = kaptchaProducer.createText()` 生成验证码文本。
   - `BufferedImage image = kaptchaProducer.createImage(text)` 生成验证码图片。
2. **将验证码存入 Session：**
   - 使用 HttpSession 将生成的验证码文本存入会话中，以便后续的验证操作可以访问这个验证码。
   - 将验证码存入session->并将用户输入的验证码与 Session 中存储的验证码进行比较，

**将图片输出给浏览器：**

- 设置响应的内容类型为 "image/png"，告诉浏览器响应的内容是图片格式。
- 获取响应输出流，并将生成的验证码图片通过 `ImageIO.write` 方法写入输出流，从而将验证码图片输出给浏览器。

### 2.6开发登录、退出功能

![image-20231121220519473](E:\Typroa_Picture\image-20231121220519473.png)

**代码是从Service开始写的，但是逻辑是从Controller->Service层**

1.因为我们需要记录Cookie信息，以便后续登录的时候就不需要输入密码。当登录成功之后才能记录cookie。

**创建保存cookie的类----LoginTicket，笔记中少了，代码需要getter和setter方法。** 

```java
public class LoginTicket {

    private int id;
    private int userId;
    private String ticket; //cookie值得
    private int status;//状态，0是保存cookie，1是不保存cookie了。因为业务中不会有删除操作
    private Date expired;//cookie保存时间
 }
```

**对LoginTicket进行SQL语句,创建Mapper接口**

```java
//通过注解查询SQL语句
@Mapper
public interface LoginTicketMapper {
    @Insert({
            "insert into login_ticket(user_id,ticket,status,expired) ",
            "values(#{userId},#{ticket},#{status},#{expired})"
    })
    //插入LoginTicket对象
    public int insertLoginTicket(LoginTicket loginTicket);
    @Select({
            "select id,user_id,ticket,status,expired" ,
            "from login_ticket where ticket=#{ticket}"
    })
    //通过Ticket查找对象
    LoginTicket selectByTicket(String ticket);

    @Update({
            "update login_ticket set status=#{status} where ticket=#{ticket}"
    })
    //更新Ticket状态
    int updateStatus(String ticket, int status);
}
```

2.编写Controller类

这里验证马先保存在Session上，后期再改的

```java
    @RequestMapping(path = "/login",method = RequestMethod.POST)
    public String login(String username,String password,String code,boolean rememberme,
                        Model model,HttpSession session,HttpServletResponse response){
        //1.检查验证码，（在显示页面的时候就已经存储了）验证码存放在session中，code是用户填写的验证码,cookie藏在httprespond中
        String kaptcha=(String) session.getAttribute("kaptcha");
        if (StringUtils.isBlank(kaptcha)||StringUtils.isBlank(code)||!kaptcha.equalsIgnoreCase(code)){
            model.addAttribute("codeMsg","验证码不正确");
            return "/site/login";
        }
        //检查密码账号
        //是否勾选了”记住我“
        int expiredSeconds=rememberme?REMEMBER_EXPIRED_SECONDS:DEFAULT_EXPIRED_SECONDS;
        //从service层拿信息返回给页面
        Map<String,Object> map=userService.login(username,password,expiredSeconds);
        //成功案例
        if (map.containsKey("ticket")){
            Cookie cookie=new Cookie("ticket",map.get("ticket").toString());//ticket是一串数字
            cookie.setPath(contextPath);
            cookie.setMaxAge(expiredSeconds);
            response.addCookie(cookie);
            //避免重复提交表单
            return "redirect:/index";
        }else {//失败案例
            model.addAttribute("usernameMsg",map.get("usernameMsg"));
            model.addAttribute("passwordMsg",map.get("passwordMsg"));
            return "/site/login";
        }
    }
    //退出功能
    @RequestMapping(path = "/logout",method = RequestMethod.GET)
    public String logout(@CookieValue("ticket" )String ticket){
        userService.logout(ticket);
        return "redirect:/login";
    }
```

3.编写Service类,只需要账号密码在数据库中查询

```java
    //登录功能,只需要账号密码在数据库中查询
    public Map<String,Object> login(String username,String password,int expiredSeconds){
        Map<String,Object> map=new HashMap<>();
        //空值处理
        if(StringUtils.isBlank(username)){
            map.put("usernameMsg","账号不能为空");
            return map;
        }
        if(StringUtils.isBlank(password)){
            map.put("passwordMsg","密码不能为空");
            return map;
        }

        //验证账号
        User user=userMapper.selectByName(username);
        if (user==null){
            map.put("usernameMsg","该账号不存在！");
            return map;
        }
        //验证状态
        if(user.getStatus()==0){
            map.put("usernameMsg","该账号尚未激活");
            return map;
        }

        //验证密码
        password=CommunityUtil.md5(password+user.getSalt());
        if(!user.getPassword().equals(password)){
            map.put("passwordMsg","密码不正确");
            return map;
        }
        //发现有这个人了，那么需要记录他的登录其他信息LoginTicket ,主要是下次登录就不需要验证密码了
        //生成登录凭证
        LoginTicket loginTicket=new LoginTicket();
        loginTicket.setUserId(user.getId());
        loginTicket.setTicket(CommunityUtil.generateUUID());
        loginTicket.setStatus(0);
        loginTicket.setExpired(new Date(System.currentTimeMillis()+expiredSeconds * 1000));
        loginTicketMapper.insertLoginTicket(loginTicket);

        //这个ticket就让浏览器知道他的cookie是多少
        map.put("ticket",loginTicket.getTicket());
        return map;
    }
```

![image-20231122163142628](E:\Typroa_Picture\image-20231122163142628.png)

cookie中的ticket用来验证是否需要再次登录。

Cookie中JSESSIONID是保存这服务器中的id。通过可以访问session的属性。

**退出功能**

1.Controller，获取页面上的cookie信息。ticket

```java
    //推出功能
    @RequestMapping(path = "/logout",method = RequestMethod.GET)
    public String logout(@CookieValue("ticket" )String ticket){
        userService.logout(ticket);
        return "redirect:/login";
    }

```

2.Service，只需要改变cookie的状态

```java
//推出登录功能
    public void logout(String ticket){
        loginTicketMapper.updateStatus(ticket,1);
    }
```

### 2.7显示登录信息



显示用户信息流程:因为每一个页面头部都需要显示用户的信息。以前是在每一个页面都需要查询用户，显示信息。比较繁琐，每一个页面都需要实现同一个方法。
那么请求拦截就可以帮我们干这些是。统一拦截所有请求，并且添加所需要的信息。

![image-20231123205643052](E:\Typroa_Picture\image-20231123205643052.png)

**实现拦截器**

**准备工作**

1.编写从浏览器中提取cookie方法工具类。

```java
public class CookieUtil {
    //传入想要得cookie名字
    public static String getValue(HttpServletRequest request,String name){
        if(request == null || name ==null){
            throw new IllegalArgumentException("参数为空");
        }
        //cookie中有很多值的
        Cookie[] cookies=request.getCookies();
        if(cookies!=null){
            for(Cookie cookie :cookies){
                if (cookie.getName().equals(name)){
                    return cookie.getValue();
                }
            }
        }
        return null;
    }
}
```

2.编写保存提取出来的User对象工具类，因为多线程缘故。我们需要把User对象放进线程中的Map中才不会和别的线程冲突，并且长期能够保存。

```java
@Component
public class HostHolder {
    //这个可以函数可以获取当前线程的，进行存放东西在Map中
    private ThreadLocal<User> users= new ThreadLocal<>();

    public void setUser(User user){
        users.set(user);
    }
    public User getUser(){
       return users.get();
    }
    public void clear(){
        users.remove();
    }
}
```

#### 能长期登录的原因

3..编写拦截器所要实现的功能。这个也是能够长期登录的一个原因把。

```java
//需要实现HandlerInterceptor
@Component
public class LoginTicketInterceptor implements HandlerInterceptor {

    @Autowired
    UserService userService;

    @Autowired
    HostHolder hostHolder;
    @Override
    //请求前截取
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //从cookie中获取,cookie在request中获取
        String ticket= CookieUtil.getValue(request,"ticket");
        if(ticket!=null){
            //查询出凭证
            LoginTicket loginTicket= userService.findLoginTicket(ticket);
            //凭证是否有效
            if(loginTicket!=null&&loginTicket.getStatus()==0&&loginTicket.getExpired().after(new Date())){
                //根据凭证查询用户
                User user=userService.findUserById(loginTicket.getUserId());
                hostHolder.setUser(user);
            }
        }
        return true;
    }

    @Override
    //模板前执行
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        User user=hostHolder.getUser();
        if(user!=null&&modelAndView!=null){
            modelAndView.addObject("loginUser",user);
        }
    }
    
    @Override
    //执行Teamplat后执行
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        hostHolder.clear();
    }
}
```

4.配置拦截器运行的范围（拦截哪些请求）

```java
@Configuration
//这个工具类比较特殊需要实现一个接口
public class WebMvcConfig implements WebMvcConfigurer {

    @Autowired
    LoginTicketInterceptor loginTicketInterceptor;

    //实现这个函数就行
    @Override
    //这个函数默认拦截所有请求，如果没有特殊请求的话
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(loginTicketInterceptor)
                .excludePathPatterns("/**/*.css", "/**/*.js", "/**/*.png", "/**/*.jpg", "/**/*.jpeg");
    }
}
```

### 2.8账号设置

![image-20231123224312960](E:\Typroa_Picture\image-20231123224312960.png)

点击Index.htlm 中账号设置->index.html中是配置Controller方法的路径user/settuser -> 在返回设置账户信息页面。

**上传头像**

上传头像一般是把自己的头像文件上传到服务器的某一个文件夹下

流程如下:

1. 创建上传文件类(把headUrl=http://localhost:8080/community/user/header/xxx.png)
2. 在index页面中显示头像中是访问User.hearUrl的->就会访问http://localhost:8080/community/user/header/xxx.png
3. 那么就会执行/user/header/XXX.png方法的Controller
4. Controller方法拿到了XXX.png文件名字，并且去本地磁盘读取头像，以HttpServeletRespond回应给index页面

具体代码如下：

1.创建Controller方法，获取设置信息页面

```java
    @RequestMapping(path = "/setting", method = RequestMethod.GET)
    public String getSettingPage() {
        return "/site/setting";
    }
```

2.更改头像Controller方法,并设置Userheader

```java
@RequestMapping(path = "/upload", method = RequestMethod.POST)
    public String uploadHeader(MultipartFile headerImage, Model model) {
        if (headerImage == null) {
            model.addAttribute("error", "您还没有选择图片!");
            return "/site/setting";
        }

        String fileName = headerImage.getOriginalFilename();
        String suffix = fileName.substring(fileName.lastIndexOf("."));
        if (StringUtils.isBlank(suffix)) {
            model.addAttribute("error", "文件的格式不正确!");
            return "/site/setting";
        }

        // 生成随机文件名
        fileName = CommunityUtil.generateUUID() + suffix;
        // 确定文件存放的路径
        File dest = new File(uploadPath + "/" + fileName);
        try {
            // 存储文件
            headerImage.transferTo(dest);
        } catch (IOException e) {
            logger.error("上传文件失败: " + e.getMessage());
            throw new RuntimeException("上传文件失败,服务器发生异常!", e);
        }

        // 更新当前用户的头像的路径(web访问路径)
        // http://localhost:8080/community/user/header/xxx.png
        User user = hostHolder.getUser();
        String headerUrl = domain + contextPath + "/user/header/" + fileName;
        userService.updateHeader(user.getId(), headerUrl);

        return "redirect:/index";
    }
```

3.以HttpServeletRespond返回图片

```java
    @RequestMapping(path = "/header/{fileName}", method = RequestMethod.GET)
    public void getHeader(@PathVariable("fileName") String fileName, HttpServletResponse response) {
        // 服务器存放路径
        fileName = uploadPath + "/" + fileName;
        // 文件后缀
        String suffix = fileName.substring(fileName.lastIndexOf("."));
        // 响应图片
        response.setContentType("image/" + suffix);
        try (
                FileInputStream fis = new FileInputStream(fileName);
                OutputStream os = response.getOutputStream();
        ) {
            byte[] buffer = new byte[1024];
            int b = 0;
            while ((b = fis.read(buffer)) != -1) {
                os.write(buffer, 0, b);
            }
        } catch (IOException e) {
            logger.error("读取头像失败: " + e.getMessage());
        }
    }
```

### 2.9检查登录状态

因为有些功能只有登录后才能进行使用的。例如是设置头像，但是如果我们强制在Url上访问也是可以的。所以我们需要拦截这些请求。

![image-20231124144126781](E:\Typroa_Picture\image-20231124144126781.png)

1. 自定义注解

   ```java
   @Target(ElementType.METHOD)
   @Retention(RetentionPolicy.RUNTIME)
   public @interface LoginRequired {
   }
   
   @Target(ElementType.METHOD):
   指定了这个注解可以被应用于方法上。ElementType.METHOD 表示该注解只能用于方法级别。
   @Retention(RetentionPolicy.RUNTIME):
   指定了这个注解在运行时保留。RetentionPolicy.RUNTIME 表示这个注解在运行时可通过反射机制获取。
   ```

   

2.定义拦截器

```java
@Component
public class LoginRequiredInterceptor implements HandlerInterceptor {

    @Autowired
    private HostHolder hostHolder;

    @Override
    //Object handler:表示被拦截的处理器，通常是一个控制器方法（HandlerMethod）。
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        if(handler instanceof HandlerMethod ){
            HandlerMethod handlerMethod=(HandlerMethod) handler;
            //通过 handlerMethod.getMethod() 获取处理器方法
            Method method=handlerMethod.getMethod();
            //使用 method.getAnnotation(LoginRequired.class) 判断该方法上是否有 @LoginRequired 注解。
            LoginRequired loginRequired=method.getAnnotation(LoginRequired.class);
            //有注解并且没有登录
            if(loginRequired!=null&&hostHolder.getUser()==null){
                //因为不是Cnntroller,所以不能直接return“页面信息”
                response.sendRedirect(request.getContextPath()+"/login");
            }
        }
        return true;
    }
}
```

3.配置拦截器，在上面逻辑中发现是没有登录，就请setting方法就拦截

```java
   //启动拦截
        registry.addInterceptor(loginRequiredInterceptor)
                .excludePathPatterns("/**/*.css", "/**/*.js", "/**/*.png", "/**/*.jpg", "/**/*.jpeg");

```

# 第三章：Springboot进阶，开发核心功能

### 3.1过滤敏感词

![image-20231124153820176](E:\Typroa_Picture\image-20231124153820176.png) 

![image-20231124160823829](E:\Typroa_Picture\image-20231124160823829.png)

使用前缀树：

1. 构建前缀树
2. 指针1，开头通常指向root节点
3. 指针2，为begin指针，循序渐进地推进
4. 指针3，为position指针为了寻找区间而存在。

描述流程：

1. 指针1指向root

2. 指针2，3指向 string[0]
   1）x字符不在root节点的子节点，指针2，3同时向后推进

   2)  w也不是，指针2，3同时向后推进

   3）a是root的子节点，那么指针1移动到a节点。指针2不动，指针3向后推进

   4)  检查指针3，是b节点，是a的子节点。那么指针b向后移动一位到 f。指针1移动到b

   5)	检查指针3，是f节点,不是b的子节点。那么 指针1回到root节点。指针2，指针3同时向后推进

举例：指针3已经遍历过a，b并且在c的位置

​		指针3指向c，发现c是b的子节点。并且是带状态的。那么替换字串，指针2，3指向下一个节点。

```java
//这是一个工具类，用于实现对敏感词过滤的作用
@Component
public class SensitiveFilter {
    private static final Logger logger = LoggerFactory.getLogger(SensitiveFilter.class);

    //替换符号
    private static final  String REPLACEMENT="***";

    //根节点
    private TrieNode rootNode=new TrieNode();
    //这个注解就是在构造器运行后执行
    @PostConstruct
    public void init(){
        try (            //这里拿铭感词文件
              InputStream is = this.getClass().getClassLoader().getResourceAsStream("sensitive-words.txt");
              BufferedReader reader = new BufferedReader(new InputStreamReader(is));
        ) {
            String keyword;
            while ((keyword = reader.readLine()) != null) {
                    //添加一行单词到前缀树
                this.addKeyword(keyword);
            }

        } catch (Exception e) {
            logger.error("加载敏感词文件失败:"+e.getMessage());
        }
    }
    //将敏感词加入到前缀树中
    private void addKeyword(String keywords){
        TrieNode tempNode = rootNode;
        for(int i=0;i<keywords.length();i++){
            char c=keywords.charAt(i);//获取某一个字符
            //获取下一个子节点
            TrieNode subNode=tempNode.getSubNode(c);
            if (subNode==null){
                //初始化子节点
                subNode=new TrieNode();
                tempNode.addSubNode(c,subNode);
            }
            //指向子节点，进入下一个字母
            tempNode=subNode;
            //如果是最后一个字母，则标记为结束标记，是敏感词结束尾巴
            if(i==keywords.length()-1){
                tempNode.setKeywordEnd(true);
            }
        }
    }

    /**
     * 过滤敏感词
     */
    public String filter(String text){
        if(StringUtils.isBlank(text)){
            return null;
        }
        //指针1
        TrieNode tempNode=rootNode;
        //指针2
        int begin=0;
        //指针3
        int position=0;
        StringBuilder stringBuilder=new StringBuilder();
        while(position<text.length()){
            char c=text.charAt(position);
            //跳过符号
            if(isSymbol(c)){
                //如果当前指针在头节点
                if(tempNode==rootNode){
                    stringBuilder.append(c);
                    begin++;
                    position++;
                }else{
                    position++;
                }
                continue;//跳过这个符号不处理
            }

            //不是符号操作
            tempNode=tempNode.getSubNode(c);
            //不存在此敏感字
            if (tempNode == null) {
                // 以begin开头的字符串不是敏感词
                stringBuilder.append(text.charAt(begin));
                // 进入下一个位置
                position = ++begin;
                // 重新指向根节点
                tempNode = rootNode;
            } else if (tempNode.isKeywordEnd()) {
                // 发现敏感词,将begin~position字符串替换掉
                stringBuilder.append(REPLACEMENT);
                // 进入下一个位置
                begin = ++position;
                // 重新指向根节点
                tempNode = rootNode;
            } else {
                // 检查下一个字符
                position++;
            }
        }
        stringBuilder.append(text.substring(begin));
        return  stringBuilder.toString();
    }
    //判断是否为符号

    // 判断是否为符号
    private boolean isSymbol(Character c) {
        // 0x2E80~0x9FFF 是东亚文字范围
        return !CharUtils.isAsciiAlphanumeric(c) && (c < 0x2E80 || c > 0x9FFF);
    }

    //构建前缀树,只有在类中才能使用，所以可以定义为私有
    private class TrieNode{
        //关键字结束标志
        private boolean isKeywordEnd=false;
        //子节点<子节点的字符，子节点>
        private Map<Character,TrieNode> subNodes=new HashMap<>();

        public boolean isKeywordEnd( ){
            return isKeywordEnd;
        }
        public void setKeywordEnd(boolean keywordEnd){
            isKeywordEnd =keywordEnd;
        }
        //添加子节点
        public void addSubNode(Character c,TrieNode trieNode){
            subNodes.put(c,trieNode);
        }
        //获取子节点
        public TrieNode getSubNode(Character c){
            return subNodes.get(c);
        }
    }
}

```

### 3.2发布帖子

![image-20231125095132231](E:\Typroa_Picture\image-20231125095132231.png)

#### Ajax

主要使用AjAX的异步请求功能，既不需要刷新页面就可以动态跟新页面数据。

1.利用fastJson包里面的功能，可以创建Json格式的数据。然后再把Jason对象转为Json格式字符串 ，以下为小示例写在CommunityUtil中
导入依赖

```properties
		<!--编写Jason依赖-->
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>fastjson</artifactId>
			<version>1.2.58</version>
		</dependency>

```



```java
 public static String getJSONString(int code, String msg, Map<String, Object> map) {
     	//fastJson包里面的功能
        JSONObject json = new JSONObject();
        json.put("code", code);
        json.put("msg", msg);
        if (map != null) {
            for (String key : map.keySet()) {
                json.put(key, map.get(key));
            }
        }
     	//返回jason字符串
        return json.toJSONString();
    }
```

2.编写Dao层，并且配置mybatis的XML文件

```java
    int insertDiscussPost(DiscussPost discussPost);

    <select id="insertDiscussPost" resultType="int" parameterType="DiscussPost">
        inserte into discuss_post (<include refid="insertFields"></include>)
        values (#{userId},#{title},#{content},#{type},#{status},#{createTime},#{commentCount},#{score})
    </select>
```

3.编写service层

```java
public int addDiscussPost(DiscussPost post){
        if (post==null){
            throw new IllegalArgumentException("参数不能为空");
        }
        //转移HTML标记，就是用户在内容中输入<scri>xxxx </scri>,以免浏览器解析HTML格式
        post.setTitle(HtmlUtils.htmlEscape(post.getTitle()));
        post.setContent(HtmlUtils.htmlEscape(post.getContent()));
        //过滤敏感词语
        post.setTitle(sensitiveFilter.filter(post.getTitle()));
        post.setContent(sensitiveFilter.filter(post.getContent()));
        return discussPostMapper.insertDiscussPost(post);
}
```

4.编写Controller层，这个层只需要返回Json格式字符串，并不需要返回页面

```java
 @RequestMapping(path = "/add",method = RequestMethod.POST)
    @ResponseBody
    public String addDiscussPost(String title,String content){
        User user=hostHolder.getUser();
        if(user==null){
            return CommunityUtil.getJSONString(403,"你还没有登陆");
        }
        DiscussPost discussPost=new DiscussPost();
        discussPost.setTitle(title);
        discussPost.setContent(content);
        discussPost.setUserId(user.getId());
        discussPost.setCreateTime(new Date());
        discussPostService.addDiscussPost(discussPost);

        //报错的情况,将来统一处理.
        return CommunityUtil.getJSONString(0,"发布成功");
    }
}
```

5.在前端页面接受JsonString数据，并且显示

```js
$(function(){
	$("#publishBtn").click(publish);
});

function publish() {
	$("#publishModal").modal("hide");

	// 获取标题和内容
	var title = $("#recipient-name").val();
	var content = $("#message-text").val();
	// 发送异步请求(POST)
	$.post(
		"/community" + "/discuss/add",
		{"title":title,"content":content},
		<!--返回的信息是data-->
        function(data) {
			data = $.parseJSON(data);
			// 在提示框中显示返回消息
			$("#hintBody").text(data.msg);
			// 显示提示框
			$("#hintModal").modal("show");
			// 2秒后,自动隐藏提示框
			setTimeout(function(){
				$("#hintModal").modal("hide");
				// 刷新页面
				if(data.code == 0) {
					window.location.reload();
				}
			}, 2000);
		}
	);

}
```

### 3.3帖子详情

![image-20231125143647997](E:\Typroa_Picture\image-20231125143647997.png)

 1.编写Dao层以及配置mybatis文件

```java
    DiscussPost selectDiscussPostById(int id);
    
    <select id="selectDiscussPostById" resultType="DiscussPost">
        select <include refid="selectFields"></include>
        from discuss_post
        where id=#{id}
    </select>

```

2.编写service层

```java
    public DiscussPost findDiscussPostById(int id){
        return discussPostMapper.selectDiscussPostById(id);
    }
```

3.Controller层

```java
    @RequestMapping(path = "/detail/{discussPostId}",method = RequestMethod.GET)
    public String getDiscussPost(@PathVariable("discussPostId") int discussPostId, Model model){
        DiscussPost discussPost=discussPostService.findDiscussPostById(discussPostId);
        //详细页面不仅仅需要内容，而且需要展示作者的信息。
        User user=userService.findUserById(discussPost.getUserId());
        model.addAttribute("post",discussPost);
        model.addAttribute("user",user);

        return "/site/discuss-detail";
    }
```

### 3.4事务管理

![image-20231125154122608](E:\Typroa_Picture\image-20231125154122608.png)

![image-20231125154502143](E:\Typroa_Picture\image-20231125154502143.png)

![image-20231125154646157](E:\Typroa_Picture\image-20231125154646157.png)

![image-20231125154912265](E:\Typroa_Picture\image-20231125154912265.png)

![image-20231125154952968](E:\Typroa_Picture\image-20231125154952968.png)

![image-20231125155023537](E:\Typroa_Picture\image-20231125155023537.png)

![image-20231125155130303](E:\Typroa_Picture\image-20231125155130303.png)

![image-20231125155229055](E:\Typroa_Picture\image-20231125155229055.png)

![image-20231125155514982](E:\Typroa_Picture\image-20231125155514982.png)

![image-20231125160032056](E:\Typroa_Picture\image-20231125160032056.png)

**Spring以及包含了对事务管理的注解，所以我们可以很轻松的就可以使用事务机制**

添加了Transactional注解之后Spring就会帮我们管理事务。

isolation = Isolation.READ_COMMITTED(设置事务等级)

propagation = Propagation.REQUIRED(设置传播方式)

```java
    // REQUIRED: 支持当前事务(外部事务),如果不存在则创建新事务.
    // REQUIRES_NEW: 创建一个新事务,并且暂停当前事务(外部事务).
    // NESTED: 如果当前存在事务(外部事务),则嵌套在该事务中执行(独立的提交和回滚),否则就会REQUIRED一样.
    @Transactional(isolation = Isolation.READ_COMMITTED, propagation = Propagation.REQUIRED)
    public Object save1() {
        // 新增用户
        User user = new User();
        user.setUsername("alpha");
        user.setSalt(CommunityUtil.generateUUID().substring(0, 5));
        user.setPassword(CommunityUtil.md5("123" + user.getSalt()));
        user.setEmail("alpha@qq.com");
        user.setHeaderUrl("http://image.nowcoder.com/head/99t.png");
        user.setCreateTime(new Date());
        userMapper.insertUser(user);
        // 新增帖子
        DiscussPost post = new DiscussPost();
        post.setUserId(user.getId());
        post.setTitle("Hello");
        post.setContent("新人报道!");
        post.setCreateTime(new Date());
        discussPostMapper.insertDiscussPost(post);

        Integer.valueOf("abc");

        return "ok";
    }
```

2.编程方式管理事务。

```java
 public Object save2() {
        transactionTemplate.setIsolationLevel(TransactionDefinition.ISOLATION_READ_COMMITTED);
        transactionTemplate.setPropagationBehavior(TransactionDefinition.PROPAGATION_REQUIRED);

        return transactionTemplate.execute(new TransactionCallback<Object>() {
            @Override
            public Object doInTransaction(TransactionStatus status) {
                // 新增用户
                User user = new User();
                user.setUsername("beta");
                user.setSalt(CommunityUtil.generateUUID().substring(0, 5));
                user.setPassword(CommunityUtil.md5("123" + user.getSalt()));
                user.setEmail("beta@qq.com");
                user.setHeaderUrl("http://image.nowcoder.com/head/999t.png");
                user.setCreateTime(new Date());
                userMapper.insertUser(user);

                // 新增帖子
                DiscussPost post = new DiscussPost();
                post.setUserId(user.getId());
                post.setTitle("你好");
                post.setContent("我是新人!");
                post.setCreateTime(new Date());
                discussPostMapper.insertDiscussPost(post);

                Integer.valueOf("abc");

                return "ok";
            }
        });
    }
```

### 3.5显示评论

![image-20231125164026374](E:\Typroa_Picture\image-20231125164026374.png)

1.编写Dao层并且配置XML

```java
@Mapper
public interface CommentMapper {
    List<Comment> selectCommentsByEntity(int entityType,int entityId,int offset,int limit);

    int selectCountByEntity(int entityType,int entityId);
}

    <sql id="selectFields">
        id, user_id, entity_type, entity_id, target_id, content, status, create_time
    </sql>
    <select id="selectCommentsByEntity" resultType="Comment">
        select <include refid="selectFields"></include>
        from comment
        where status=0 and entity_type=#{entityType} and entity_id =#{entityId}
        order by create_time asc
        limit #{offset },#{limit}
    </select>

    <select id="selectCountByEntity" resultType="int" >
        select count(id)
        from comment
        where status=0 and entity_type=#{entityType} and entity_id =#{entityId}
    </select>
```

编写Service层

```java
    public List<Comment> findCommentByEntity(int entityType,int entityId,int offset,int limit){
        return commentMapper.selectCommentsByEntity(entityType,entityId,offset,limit);
    }
    public int findCommentCount(int entityType,int entityId){
        return commentMapper.selectCountByEntity(entityType,entityId);
    }
```

编写Controller层，比较绕。就是显示评论之后，还有评论的评论，但是并不难。

```java
    @RequestMapping(path = "/detail/{discussPostId}",method = RequestMethod.GET)
    public String getDiscussPost(@PathVariable("discussPostId") int discussPostId, Model model,Page page){
        DiscussPost discussPost=discussPostService.findDiscussPostById(discussPostId);
        //详细页面不仅仅需要内容，而且需要展示作者的信息。
        User user=userService.findUserById(discussPost.getUserId());
        model.addAttribute("post",discussPost);
        model.addAttribute("user",user);

        // 评论分页信息，一页显示多少数目
        //用于在index分页中使用
        page.setLimit(5);
        //用于下一页点击链接还能回来这个页面的
        page.setPath("/discuss/detail/" + discussPostId);
        page.setRows(discussPost.getCommentCount());

        // 评论: 给帖子的评论
        // 回复: 给评论的评论
        // 评论列表
        List<Comment> commentList = commentService.findCommentByEntity(
                ENTITY_TYPE_COMMENT, discussPost.getId(), page.getOffset(), page.getLimit());
        // 评论Vo，就是不仅仅需要看评论内容，而且还需要看是谁评论，这个评论有多少给人回复
        List<Map<String, Object>> commentVoList = new ArrayList<>();
        if (commentList != null) {
            for (Comment comment : commentList) {
                // 评论VO
                Map<String, Object> commentVo = new HashMap<>();
                // 评论
                commentVo.put("comment", comment);
                // 作者
                commentVo.put("user", userService.findUserById(comment.getUserId()));

                // 回复列表
                List<Comment> replyList = commentService.findCommentByEntity(
                        ENTITY_TYPE_COMMENT, comment.getId(), 0, Integer.MAX_VALUE);
                // 回复VO列表
                List<Map<String, Object>> replyVoList = new ArrayList<>();
                if (replyList != null) {
                    for (Comment reply : replyList) {
                        Map<String, Object> replyVo = new HashMap<>();
                        // 回复
                        replyVo.put("reply", reply);
                        // 作者
                        replyVo.put("user", userService.findUserById(reply.getUserId()));
                        // 回复的目标
                        User target = reply.getTargetId() == 0 ? null : userService.findUserById(reply.getTargetId());
                        replyVo.put("target", target);

                        replyVoList.add(replyVo);
                    }
                }
                commentVo.put("replys", replyVoList);

                // 回复数量
                int replyCount = commentService.findCommentCount(ENTITY_TYPE_REPLY, comment.getId());
                commentVo.put("replyCount", replyCount);

                commentVoList.add(commentVo);
            }
        }

        model.addAttribute("comments", commentVoList);

        return "/site/discuss-detail";
    }
```

### 3.6添加评论

添加评论需要一个联动。
1.对帖子添加评论后，既插入Comment到数据库，那么DiscussPost帖子就需要“回复数量+1”



对Comment插入编写Dao层，以及配置XML文件

```java
    int insertComment(Comment comment);
    
    <insert id="insertComment" parameterType="Comment">
        insert into comment(<include refid="insertFields"></include>)
        values (#{userId},#{entityType},#{entityId},#{targetId},#{content},#{status},#{createTime})
    </insert>
```

对DisCusspostMapper编写增加评论数量，以及配置XML文件

```java
    int updateCommentCount(int id, int commentCount);
    
    <update id="updateCommentCount" >
        update discuss_post set comment_count =#{commentCount} where id=#{id};
    </update>

```



对Comment插入编写Service层

```java
//加一个事务管理，要么都成功，要么都失败
    @Transactional(isolation = Isolation.READ_COMMITTED,propagation = Propagation.REQUIRED)
    public int addComment(Comment comment){
        if (comment == null) {
            throw new IllegalArgumentException("参数不能为空!");
        }
        //先过滤标签符号，再过滤非法字符后才添加
        comment.setContent(HtmlUtils.htmlEscape(comment.getContent()));
        comment.setContent(sensitiveFilter.filter(comment.getContent()));
        int row=commentMapper.insertComment(comment);

        //添加成功后就让评论数目更新。
        if(comment.getEntityType()==ENTITY_TYPE_COMMENT){
            //插入后评论数目就会增加一条，再查出来总数。放进discusspost中
            int count =commentMapper.selectCountByEntity(comment.getEntityType(),comment.getEntityId());
            discussPostService.updateCommentCount(comment.getEntityId(),count);
        }
        return row;
    }
```

对DiscussPost编写Service层

```java
    public int updateCommentCount(int id,int commentCount){
        return discussPostMapper.updateCommentCount(id,commentCount);
    }
```

对Comment编写Controller层

```java
    @RequestMapping(path = "/add/{discussPostId}", method = RequestMethod.POST)
    public String addComment(@PathVariable("discussPostId") int discussPostId, Comment comment) {
        comment.setUserId(hostHolder.getUser().getId());
        comment.setStatus(0);
        comment.setCreateTime(new Date());
        commentService.addComment(comment);

        return "redirect:/discuss/detail/" + discussPostId;
    }
```

### 3.7私信列表

![image-20231126154506794](E:\Typroa_Picture\image-20231126154506794.png)

理解概念：

1.会话：就是两个User进行通信的那一列，就是一个窗口一个会话

2.消息，一条数据就是一个消息



查询：每个会话显示最新一条消息。

消息状态：0未读，1以读，2是删除数据

UserId：1表示系统给你发的消息。

编写Dao层

```java
@Mapper
public interface MessageMapper {
    //查询当前用户的会话列表，针对每一个会话返回最新的私信。
    List<Message> selectConversations(int userId,int offset,int limit);

    //查询当前用户的会话的消息数量
    int selectConversationCount(int userId);

    //查询某一个会话的所有消息
    List<Message> selectLetters(String conversationId,int offset,int limit);

    //查询某一个会话的消息总数
    int selectLetterCount(String conversationId);

    //查询未读私信
    int selectLetterUnreadCount(int userId,String conversationId);
}
```



messageMapper

```xml
    <sql id="selectFields">
        id, from_id, to_id, conversation_id, content, status, create_time
    </sql>

<!--通过会话分组，找到每一个会话的最新消息(ID最大)显示，不仅仅可以用最新的会话消息，代替会话-->
<!--就是找到所有消息ID(经过Conversation归类，user可以是接受消息也可以是发送消息的人，但是不能是1)-->
    <select id="selectConversations" resultType="Message">
        select <include refid="selectFields"></include>
        from message
        where id in (
            select max(id) from message
            where status != 2
            and from_id != 1
            and (from_id = #{userId} or to_id = #{userId})
            group by conversation_id
        )
        order by id desc
        limit #{offset}, #{limit}
    </select>

<!--计算会话的数量，会话分组后，又fromid和toid，只取一个就好了-->

    <select id="selectConversationCount" resultType="int">
        select count(m.maxid) from (
            select max(id) as maxid from message
            where status != 2
            and from_id != 1
            and (from_id = #{userId} or to_id = #{userId})
            group by conversation_id
        ) as m
    </select>

<!--查询该会话的消息-->
    <select id="selectLetters" resultType="Message">
        select <include refid="selectFields"></include>
        from message
        where status != 2
        and from_id != 1
        and conversation_id = #{conversationId}
        order by id desc
        limit #{offset}, #{limit}
    </select>

<!--查询该会话总共有多少条信息-->
    <select id="selectLetterCount" resultType="int">
        select count(id)
        from message
        where status != 2
        and from_id != 1
        and conversation_id = #{conversationId}
    </select>

<!--查询该有多少条未读信息，如果添加会话的话，那么就是会话的为读消息有多少条-->
    <select id="selectLetterUnreadCount" resultType="int">
        select count(id)
        from message
        where status = 0
        and from_id != 1
        and to_id = #{userId}
        <if test="conversationId!=null">
            and conversation_id = #{conversationId}
        </if>
    </select>
```

编写Service层

```java
@Service
public class MessageService {
    @Autowired
    MessageMapper messageMapper;

    public List<Message> findConversations(int userId, int offset, int limit) {
        return messageMapper.selectConversations(userId, offset, limit);
    }

    public int findConversationCount(int userId) {
        return messageMapper.selectConversationCount(userId);
    }

    public List<Message> findLetters(String conversationId, int offset, int limit) {
        return messageMapper.selectLetters(conversationId, offset, limit);
    }

    public int findLetterCount(String conversationId) {
        return messageMapper.selectLetterCount(conversationId);
    }

    public int findLetterUnreadCount(int userId, String conversationId) {
        return messageMapper.selectLetterUnreadCount(userId, conversationId);
    }
}

```

编写Controller层

```java
//用户的消息列表
    @RequestMapping(path = "/letter/list",method = RequestMethod.GET)
    public String getLetterList(Model model, Page page){
        //获取当前用户消息
        User user=hostHolder.getUser();
        //分页信息
        page.setLimit(5);
        page.setPath("/letter/list");
        page.setRows(messageService.findConversationCount(user.getId()));

        //会话列表
        List<Message> conversationList=messageService.findConversations(user.getId(),page.getOffset(),page.getLimit());
        List<Map<String,Object>> conversations=new ArrayList<>();
        if(conversationList!=null){
            for(Message message: conversationList){
                Map<String,Object> map=new HashMap<>();
                map.put("conversation",message);
                //找到该会话总共有多少条信息
                map.put("letterCount",messageService.findLetterCount(message.getConversationId()));
                //该会话有多少条未读信息
                map.put("unreadCount",messageService.findLetterUnreadCount(user.getId(),message.getConversationId()));
                //该会话的对象是谁？如果是自己发送的作为消息发送方，那么就存放另外一段的UserID
                int targetId=user.getId()==message.getFromId()? message.getToId():message.getFromId();
                map.put("target",userService.findUserById(targetId));
                //把会话信息保存
                conversations.add(map);
            }
        }
        model.addAttribute("conversations",conversations);

        //查询总的未读消息数量
        int letterUnreadCount =messageService.findLetterUnreadCount(user.getId(),null);
        model.addAttribute("letterUnreadCount",letterUnreadCount);
        return "/site/letter";
    }

    //用户一个会话的里面详细列表

    @RequestMapping(path = "/letter/detail/{conversationId}",method = RequestMethod.GET)
    public String getLetterDetail(@PathVariable("conversationId")String conversationId,Model model,Page page){
     //分页信息
     page.setLimit(5);
     page.setPath("/letter/detail/"+conversationId);
     page.setRows(messageService.findLetterCount(conversationId));

     //私信列表
     List<Message> letterList =messageService.findLetters(conversationId,page.getOffset(), page.getLimit());
     List<Map<String,Object>> letters=new ArrayList<>();
     if(letterList!=null){
         for(Message message:letterList){
             Map<String,Object> map=new HashMap<>();
             map.put("letter",message);
             //会话消息有两种，一种是自己是接收方，一种是自己是发送放。 这里找到会话的发送方，那就是自己了
             map.put("fromUser",userService.findUserById(message.getFromId()));
             letters.add(map);
         }
     }
     model.addAttribute("letters",letters);

     
     //私信目标，111_222 分割字符串，看一下自己是哪一个，就排除
     model.addAttribute("target",getLetterTarget(conversationId));
     return "/site/letter-detail";
    }
    
    private User getLetterTarget(String conversationId) {
        String[] ids = conversationId.split("_");
        int id0 = Integer.parseInt(ids[0]);
        int id1 = Integer.parseInt(ids[1]);
        
        if (hostHolder.getUser().getId() == id0) {
            return userService.findUserById(id1);
        } else {
            return userService.findUserById(id0);
        }
    }
```

### 3.8发送私信

![image-20231127134651576](E:\Typroa_Picture\image-20231127134651576.png)



编写Dao层

```java
    //新增消息
    int insertMessage(Message message);
    
    //修改消息状态
    int updataStutus(List<Integer> ids,int status);
```

配置XML文件

```xml
    <insert id="insertMessage" parameterType="Message" keyProperty="id">
        insert into message(<include refid="insertFields"></include>)
        values(#{fromId},#{toId},#{conversationId},#{content},#{status},#{createTime})
    </insert>

    <update id="updataStutus" >
        update message set status = #{status}
        where id in   <!--这是mybatis遍历列表方法，可以拼接。“，”是分割符号 item是想要拼的元素-->
        <foreach collection="ids" item="id" open="(" separator="," close=")">
            #{id}
        </foreach>
    </update>
```

编写Service层

```java
    //增加消息功能
    public int addMessage(Message message){
        message.setContent(HtmlUtils.htmlEscape(message.getContent()));
        message.setContent(sensitiveFilter.filter(message.getContent()));
        return messageMapper.insertMessage(message);
    }
    //修改评论状态,(把为读消息改成已读消息)
    public int readMessage(List<Integer> ids){
        return messageMapper.updataStutus(ids,1);
    }
```

编写Controller层

```java
//发送消息，是否成功要返回jason格式字符串。就是插入一个消息实体进入数据库中
    @RequestMapping(path = "/letter/send", method = RequestMethod.POST)
    @ResponseBody
    public String sendLetter(String toName, String content) {
        User target = userService.findUserByName(toName);
        if (target == null) {
            return CommunityUtil.getJSONString(1, "目标用户不存在!");
        }
        
        Message message = new Message();
        message.setFromId(hostHolder.getUser().getId());
        message.setToId(target.getId());
        if (message.getFromId() < message.getToId()) {
            message.setConversationId(message.getFromId() + "_" + message.getToId());
        } else {
            message.setConversationId(message.getToId() + "_" + message.getFromId());
        }
        message.setContent(content);
        message.setCreateTime(new Date());
        messageService.addMessage(message);
        
        return CommunityUtil.getJSONString(0);
    }
```

其实当用户点击某一个会话的时候，那么就把该会话的所有消息“置成已读状态”

```java
     //当用户打开当前会话，那么该会话的消息就会自动变成已读消息
	//打开会话了，那么就把Conversion中的ToId消息设置为“已读状态”
     List<Integer> ids = getLetterIds(letterList);//letterList信息包含FromId和ToID的
        if (!ids.isEmpty()) {
            messageService.readMessage(ids);
        }
        
     return "/site/letter-detail";
    }
    
    //获取当前会话的对象的哪些未读消息。
    private List<Integer> getLetterIds(List<Message> letterList) {
        List<Integer> ids = new ArrayList<>();
    
        if (letterList != null) {
            for (Message message : letterList) {
                //
                if (hostHolder.getUser().getId() == message.getToId() && message.getStatus() == 0) {
                    ids.add(message.getId());
                }
            }
        }
    
        return ids;
    }
```

### 3.9异常处理

**Springboot处理方式就是当出现500、404方法时候自动跳转页面。**

但是呢，需要把error文件放在templat文件下，error文件里面需要有404.html，500html



Spring处理方式：

![](E:\Typroa_Picture\image-20231127211109647.png)



```java
//这个类是管理所有Controller类的异常
@ControllerAdvice(annotations = Controller.class)
public class ExceptionAdvice {
    
    private static final Logger logger= LoggerFactory.getLogger(ExceptionAdvice.class);
    //处理哪一种异常的类型，这里填写Exception，所有异常的父类
    @ExceptionHandler({Exception.class})//传入的异常，
    public void handleException(Exception e, HttpServletResponse response, HttpServletRequest request) throws IOException {
        logger.error("服务器发生异常"+e.getMessage());
        for(StackTraceElement element : e.getStackTrace()){
            logger.error(element.toString());
        }
        //返回有两种。1.一种是返回页面模式，一种是返回Jason模式
        String xRequestedWith=request.getHeader("x-requested-with");
        //返回Jason模式
        if ("XMLHttpRequest".equals(xRequestedWith)){
            response.setContentType("application/plain;charset=utf-8");
            PrintWriter writer=response.getWriter();
            writer.write(CommunityUtil.getJSONString(1,"服务器异常"));
        }else{
            response.sendRedirect(request.getContextPath()+"/error");
        }
    }
}
```

1. `@ControllerAdvice(annotations = Controller.class)`：
   - `@ControllerAdvice` 注解用于定义一个全局的异常处理器，它可以监控所有的 `@Controller` 注解标记的类。
   - `annotations = Controller.class` 表示只处理标记了 `@Controller` 注解的类中抛出的异常。
2. `@ExceptionHandler({Exception.class})`：
   - `@ExceptionHandler` 注解用于定义异常处理方法，这里处理的异常类型是 `Exception` 类型，即所有异常的父类。
   - 当被监控的 `@Controller` 类中的方法抛出 `Exception` 或其子类的异常时，会调用该方法进行异常处理。
3. `public void handleException(Exception e, HttpServletResponse response, HttpServletRequest request) throws IOException`：
   - `handleException` 方法是用来处理异常的具体实现。
   - 参数 `Exception e` 表示捕获到的异常对象。
   - 参数 `HttpServletResponse response` 用于向客户端发送响应。
   - 参数 `HttpServletRequest request` 用于获取请求信息。
4. `logger.error("服务器发生异常" + e.getMessage())`：
   - 记录异常信息到日志中。
5. `for (StackTraceElement element : e.getStackTrace())`：
   - 遍历异常堆栈信息，并记录每一条信息到日志中。
6. `String xRequestedWith = request.getHeader("x-requested-with")`：
   - 获取请求头中的 "x-requested-with" 字段，该字段用于判断请求是否为异步请求（Ajax 请求）。
7. 异常处理逻辑：
   - 如果请求是异步请求（Ajax 请求）：
     - 设置响应内容类型为 "application/plain;charset=utf-8"。
     - 获取 `PrintWriter` 对象，将错误信息以 JSON 格式写入响应。
   - 如果请求不是异步请求：
     - 通过 `response.sendRedirect` 将请求重定向到 "/error" 页面。

总体来说，这段代码是一个全局异常处理器，用于捕获所有 `@Controller` 类中抛出的异常。根据请求是同步还是异步，采取不同的异常处理策略，返回相应的错误信息或重定向到错误页面。这样可以统一处理异常，提高代码的可维护性。



在 Spring MVC 中，通常可以通过检查请求头中的 "X-Requested-With" 字段来判断请求是否是异步请求（Ajax 请求）。当浏览器发起 Ajax 请求时，通常会在请求头中加入 "X-Requested-With: XMLHttpRequest"。

### 3.10统一记录日志

![](E:\Typroa_Picture\image-20231128135333306.png)

![image-20231128135528150](E:\Typroa_Picture\image-20231128135528150.png)

![image-20231128135736670](E:\Typroa_Picture\image-20231128135736670.png)

#### AOP解释

：当多个Service需要增加日志功能，但是当这个日志功能的位置需要变化的时候，那么就修改全部Logger代码的位置。这是非常不好的。那么通过AOP模式，就是通过WeaVing(织入)功能，统一给多个Service声明位置，以及代码。那么就好像图中，切了一刀一样。
Pointcut：切入Service逻辑的哪个位置。但是service也会多个方法，那么Advice就具体到哪个位置了。(不清楚)



![image-20231128140643221](E:\Typroa_Picture\image-20231128140643221.png)

![image-20231128140917197](E:\Typroa_Picture\image-20231128140917197.png)



#### 1.例子

```java
@Component
@Aspect
public class AlphaAspect {
     //"execution(* com.nowcoder.community.service.*.*(..))"
     //第一个*任意的权限修饰符和返回类型，第二个*是service下的所有class，第三个是类型中的所有方法，(..)是所有的方法参数
    @Pointcut("execution(* com.nowcoder.community.service.*.*(..))")
    public void pointcut() {

    }
    //在这个切点pointcut前执行
    @Before("pointcut()")
    public void before() {
        System.out.println("before");
    }
    //在这个切点pointcut后执行
    @After("pointcut()")
    public void after() {
        System.out.println("after");
    }
    //返回结果后执行
    @AfterReturning("pointcut()")
    public void afterRetuning() {
        System.out.println("afterRetuning");
    }
    //有异常执行
    @AfterThrowing("pointcut()")
    public void afterThrowing() {
        System.out.println("afterThrowing");
    }
    //环绕切点执行，joinPoint是执行 哪个方法
    @Around("pointcut()")
    public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("around before");
        Object obj = joinPoint.proceed();
        System.out.println("around after");
        return obj;
    }
}
```

这段代码是使用 Spring AOP（面向切面编程）实现的切面，用于在指定的业务方法执行前后以及发生异常时执行一些操作。以下是对代码的解释：

1. `@Component`：将该类标记为 Spring 的组件，表示由 Spring 扫描并纳入管理。
2. `@Aspect`：将该类标记为一个切面，定义了一组切点和通知。
3. `@Pointcut("execution(* com.nowcoder.community.service.*.*(..))")`：定义切点，该切点匹配 `com.nowcoder.community.service` 包下所有类的所有方法。
4. `public void pointcut() {}`：切点的实际方法体，由于该方法的内容为空，只是为了定义一个切点，方法名 `pointcut` 就是切点的名称。
5. `@Before("pointcut()")`：在切点执行前执行的通知。在匹配到 `pointcut()` 定义的切点时，会执行 `before()` 方法。
6. `@After("pointcut()")`：在切点执行后执行的通知。
7. `@AfterReturning("pointcut()")`：在切点执行后，如果方法成功返回结果，则执行的通知。
8. `@AfterThrowing("pointcut()")`：在切点执行后，如果方法抛出异常，则执行的通知。
9. `@Around("pointcut()")`：在切点执行前后，允许在方法执行的前后添加额外的逻辑，甚至替换原有的逻辑。
   - `joinPoint.proceed()`：调用目标方法，返回目标方法的执行结果。
   - 在 `around()` 方法中，输出了 "around before" 和 "around after"。

**调用时机解释：**

- `@Before`：在切点方法执行前调用。
- `@After`：在切点方法执行后调用，无论方法是否发生异常。
- `@AfterReturning`：在切点方法成功返回结果后调用。
- `@AfterThrowing`：在切点方法抛出异常后调用。
- `@Around`：在切点方法执行前后调用，可以控制是否继续执行切点方法。

总体而言，这段代码是一个简单的切面，用于在 `com.nowcoder.community.service` 包下所有类的所有方法执行前后和发生异常时输出相应的日志信息。

#### 2.具体实现添加Logger日志

```java
@Component
@Aspect
public class ServiceLogAspect {
    
    private static final Logger logger = LoggerFactory.getLogger(ServiceLogAspect.class);
    
    @Pointcut("execution(* com.nowcoder.community.service.*.*(..))")
    public void pointcut() {
    }
    
    @Before("pointcut()")
    public void before(JoinPoint joinPoint){
        // 用户[1.2.3.4],在[xxx],访问了[com.nowcoder.community.service.xxx()].
        //通过Request取到用户的IP地址，时间
        ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        HttpServletRequest request=attributes.getRequest();
        String ip =request.getRemoteHost();
        String now=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date());
        String target=joinPoint.getSignature().getDeclaringTypeName()+"."+joinPoint.getSignature().getName();
        logger.info(String.format("用户[%s],在[%s],访问了[%s].",ip,now,target));
    }
}
```

1. `ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();`：使用 `RequestContextHolder.getRequestAttributes()` 获取当前请求的上下文信息。通过 `ServletRequestAttributes` 可以获取到请求的相关信息，例如 `HttpServletRequest`。
2. `HttpServletRequest request = attributes.getRequest();`：通过 `ServletRequestAttributes` 获取到当前请求的 `HttpServletRequest` 对象，从中可以获取请求的信息，如 IP 地址、请求路径等。
3. `String ip = request.getRemoteHost();`：通过 `HttpServletRequest` 获取请求的远程主机地址，即用户的 IP 地址。
4. `String now = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date());`：获取当前时间，并格式化为字符串。
5. `String target = joinPoint.getSignature().getDeclaringTypeName() + "." + joinPoint.getSignature().getName();`：通过 `JoinPoint` 获取目标方法的签名，包括方法所在的类名和方法名。
6. `logger.info(String.format("用户[%s],在[%s],访问了[%s].", ip, now, target));`：将获取到的信息以日志的形式输出，记录用户的 IP 地址、访问时间、访问的方法等信息。

总体而言，这段代码的目的是在 `com.nowcoder.community.service` 包下的所有方法执行前记录用户的访问日志。

# 第四章：Redis

![image-20231128153258573](E:\Typroa_Picture\image-20231128153258573.png)

### 4.1Redis常用命令

```
redis-cli 使用redis
select Index(1- 15).选择使用的库
flushdb 刷新当前库，使得库清空
```

对String类型进行命令操作 <key,value>值保存的

```
1.设置值：set key value     (set test:count 1)  testcount是一个名字
2.取值	get key   (get test:count)
3.增加 value值+1， incr key (incr test:count)
4.减少 value值-1；decr key (decr test:count)
```

对哈希hashes操作 哈希本来就是一个<key,value> .所以整体格式是：key <key,value>

```
1.设置值 hset test:user id 1   hset test:user username zhangsang
2.取值 hget test:user id      hset test:user username 
```

对列表List操作.List 支持左近左出，也支持左近右出。

```
#这是一个左进的存储数据方法。 lpush  l(表示left)
1.左进数据 lpush test:ids Index1,Index2,Index3 (lpush test:ids 101,102,103)
2.查询数据长度 llen test:ids
3.查询某一个下表位置的数据 lindex test:ids Index (lindex test:ids 101)
4.查询List下的多条数据 lrange test:ids 0 2; (0-2下标的数据)
5.出数据 rpop(r:right)
rpop test:ids
```

对set进行操作。set随机存储，没有顺序的，并且是无重复的

```
1.添加元素 sadd test:teacher 值，...值 (sadd test:teacher aaa,bbb,ccc) 
2.查询数据有多少条在set中 scard test:teacher 
3.删除数据，这里是随机删除的。可以实现抽奖功能 spop test:teacher
4.查询set还剩下多少条数据并且显示 smembers test:teacher
```

对sorted sets进行操作,有序，并且利用分数进行排序

```
1.增加元素 zadd test:students core value,core value2 (zadd test:students 10 aaa,20 bbb)
2.查询有多少个元素 zcard test:students
3.查询某一个值的分数是多少 zscore test:students value (zscore test:students aaa)
4.查询某一个值的排名是多少 zrank test:students value (zrank test:students aaa)
5.查询排名在0-2的值是哪些 zrank test:studengs 0 2
```

全局命令

```
1.查看所有key有多少 keys *
2.查询命名的key有多少： keys test* 以test开头的key有多少
3.查询某一个key的类型 type test:user
4.查询某一个key是否存在 exists test:user
5.删除某一个key ：del test:user
6.设置某一个key多长时间过期 expire test:stuendts 10 (10秒后过期)
```

### 4.2 Spring整合Redis

![image-20231128164651207](E:\Typroa_Picture\image-20231128164651207.png)

1.导入依赖

```properties

                <dependency>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-data-redis</artifactId>
                </dependency>
```

2.去properties配置

```xml
#配置Redis
# RedisProperties
spring.redis.database=11
spring.redis.host=localhost
spring.redis.port=6379端口
```

3.配置RedisConfig ，配置序列化方式。就是Redis以什么为输入输出。

```java
@Configuration
public class RedisConfig {
    
    @Bean
    //RedisConnectionFactory作为形参就会Spring 自动给我们实例化
    public RedisTemplate<String,Object> redisTemplate(RedisConnectionFactory factory){
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(factory);
        
        //设置key的序列化方式
        template.setKeySerializer(RedisSerializer.string());
        //设置value的序列化方式，一般用Json方式
        template.setValueSerializer(RedisSerializer.json());
        
        //设置哈希的序列化方式
        template.setHashKeySerializer(RedisSerializer.string());
        //设置哈希的value序列化方式
        template.setHashValueSerializer(RedisSerializer.json());
        //这是用来成功设置的方法
        template.afterPropertiesSet();
        return template;
    }
}
```

4.测试RdisConfig用法

```java
public class RedisTests {
    
    @Autowired
    private RedisTemplate redisTemplate;
    
    @Test
    public void testStrings() {
        String redisKey = "test:count";
        
        redisTemplate.opsForValue().set(redisKey, 1);
        
        System.out.println(redisTemplate.opsForValue().get(redisKey));
        System.out.println(redisTemplate.opsForValue().increment(redisKey));
        System.out.println(redisTemplate.opsForValue().decrement(redisKey));
    }
    
    @Test
    //哈希方法
    public void testHashes() {
        String redisKey = "test:user";
        
        redisTemplate.opsForHash().put(redisKey, "id", 1);
        redisTemplate.opsForHash().put(redisKey, "username", "zhangsan");
        
        System.out.println(redisTemplate.opsForHash().get(redisKey, "id"));
        System.out.println(redisTemplate.opsForHash().get(redisKey, "username"));
    }
    
    @Test
    public void testLists() {
        String redisKey = "test:ids";
        
        redisTemplate.opsForList().leftPush(redisKey, 101);
        redisTemplate.opsForList().leftPush(redisKey, 102);
        redisTemplate.opsForList().leftPush(redisKey, 103);
        
        System.out.println(redisTemplate.opsForList().size(redisKey));
        System.out.println(redisTemplate.opsForList().index(redisKey, 0));
        System.out.println(redisTemplate.opsForList().range(redisKey, 0, 2));
        
        System.out.println(redisTemplate.opsForList().leftPop(redisKey));
        System.out.println(redisTemplate.opsForList().leftPop(redisKey));
        System.out.println(redisTemplate.opsForList().leftPop(redisKey));
    }
    
    @Test
    public void testSets() {
        String redisKey = "test:teachers";
        
        redisTemplate.opsForSet().add(redisKey, "刘备", "关羽", "张飞", "赵云", "诸葛亮");
        
        System.out.println(redisTemplate.opsForSet().size(redisKey));
        System.out.println(redisTemplate.opsForSet().pop(redisKey));
        System.out.println(redisTemplate.opsForSet().members(redisKey));
    }
    
    @Test
    public void testSortedSets() {
        String redisKey = "test:students";
        
        redisTemplate.opsForZSet().add(redisKey, "唐僧", 80);
        redisTemplate.opsForZSet().add(redisKey, "悟空", 90);
        redisTemplate.opsForZSet().add(redisKey, "八戒", 50);
        redisTemplate.opsForZSet().add(redisKey, "沙僧", 70);
        redisTemplate.opsForZSet().add(redisKey, "白龙马", 60);
        
        System.out.println(redisTemplate.opsForZSet().zCard(redisKey));
        System.out.println(redisTemplate.opsForZSet().score(redisKey, "八戒"));
        System.out.println(redisTemplate.opsForZSet().reverseRank(redisKey, "八戒"));
        System.out.println(redisTemplate.opsForZSet().reverseRange(redisKey, 0, 2));
    }
    
    @Test
    //对key操作
    public void testKeys() {
        redisTemplate.delete("test:user");
        
        System.out.println(redisTemplate.hasKey("test:user"));
        
        redisTemplate.expire("test:students", 10, TimeUnit.SECONDS);
    }
    
    // 批量发送命令,节约网络开销.
    @Test
    //用绑定方法
    public void testBoundOperations() {
        String redisKey = "test:count";
        BoundValueOperations operations = redisTemplate.boundValueOps(redisKey);
        operations.increment();
        operations.increment();
        operations.increment();
        operations.increment();
        operations.increment();
        System.out.println(operations.get());
    }
    
    // 编程式事务
    @Test
    public void testTransaction() {
        Object result = redisTemplate.execute(new SessionCallback() {
            @Override
            public Object execute(RedisOperations redisOperations) throws DataAccessException {
                String redisKey = "text:tx";
                
                // 启用事务
                redisOperations.multi();
                redisOperations.opsForSet().add(redisKey, "zhangsan");
                redisOperations.opsForSet().add(redisKey, "lisi");
                redisOperations.opsForSet().add(redisKey, "wangwu");
                
                System.out.println(redisOperations.opsForSet().members(redisKey));
                
                // 提交事务
                return redisOperations.exec();
            }
        });
        System.out.println(result);
    }
    
}

```

编程事务的解释：


这段代码是一个 Redis 编程式事务的例子。在 Redis 中，事务是通过 MULTI、EXEC、DISCARD 和 WATCH 等命令来实现的。Spring Data Redis 提供了 `SessionCallback` 接口，通过它你可以在一个 Redis 事务中执行多个操作。

下面是代码的主要步骤解释：

1. **启用事务：** `redisOperations.multi();` 开启一个 Redis 事务，此后所有的命令都将被缓存起来，而不是立即执行。
2. **执行操作：** 在事务中，你通过 `opsForSet()` 执行了三个不同的添加元素到集合的操作。
3. **输出集合成员：** `System.out.println(redisOperations.opsForSet().members(redisKey));` 输出了集合的所有成员。需要注意的是，虽然你在事务中执行了添加元素的操作，但这些操作在事务提交前并不会被立即执行，因此这里输出的结果可能为空。
4. **提交事务：** `redisOperations.exec();` 提交事务，将所有缓存的命令一次性执行。`exec()` 方法会返回一个包含每个操作结果的 List。

整个事务中的操作要么全部成功提交，要么全部回滚。如果事务中的任何一个命令执行失败，整个事务都会被回滚，不会产生任何影响。

最后，代码通过 `System.out.println(result);` 输出事务执行的结果，结果是一个 List，包含了每个操作的返回值。在这个例子中，因为只是添加元素到集合，返回的结果可能并不是很有意义。

需要注意的是，Redis 事务不支持回滚操作，因为 Redis 采用的是一种乐观锁的实现，如果在执行事务期间有其他客户端对被监视的键进行了修改，整个事务将会失败。

### 4.3点赞功能

因为点赞实时性比较强，那么就不存入到mysql数据库中。存放在Redis数据库中，所以就不写Dao层

**Redis创建Key工具类**

因为Redis是Key value保存，所以需要拼接进入Redis数据库中。

```java
public class RedisKeyUtil {
    private static final String SPLIT=":";
    private static final String PREFIX_ENTITY_LIKE="like:entity";
    
    //给具体某一个实体创建一个Key名字,用set保存，因为这样不仅仅可以统计该点赞数量，更可以统计是哪个人点赞的
    // like:entity:entityType:entityId -> set(userId)
    public static String getEntityLikeKey(int entityType,int entityId){
        return PREFIX_ENTITY_LIKE+SPLIT+entityType+SPLIT+entityId;
    }
}
```

**Service逻辑代码**

```java
@Service
public class LikeService {
    @Autowired
    private RedisTemplate redisTemplate;
    
    //点赞功能，如果该用户已经点赞了那么就取消点赞，没有点赞就继续点赞
    public void like(int userId,int entityType,int entityId){
        //点赞，需要给谁点赞，所以需要把RedisKey命名好，存放在Redis中
        String entityLikeKey= RedisKeyUtil.getEntityLikeKey(entityType,entityId);
        //判断是否已经点赞了
        boolean isMember=redisTemplate.opsForSet().isMember(entityLikeKey,userId);
        if(isMember){
            redisTemplate.opsForSet().remove(entityLikeKey,userId);
        }else{
            redisTemplate.opsForSet().add(entityLikeKey,userId);
        }
    }
    //查询某一个实体的点赞数量
    public long findEntityLikeCount(int entityType,int entityId){
        String entityLikeKey= RedisKeyUtil.getEntityLikeKey(entityType,entityId);
        return redisTemplate.opsForSet().size(entityLikeKey);
    }
    
    //查询某一个实体对该赞的状态。已经赞1，没攒0
    public int findEntityLikeStatus(int userId,int entityType,int entityId){
        String entityLikeKey= RedisKeyUtil.getEntityLikeKey(entityType,entityId);
        return redisTemplate.opsForSet().isMember(entityLikeKey,userId) ? 1 : 0;
    }
    
}
```



对点赞Controller编写，这是一个异步请求

```java
@Controller
public class LikeController {
    
    @Autowired
    private LikeService likeService;
    
    @Autowired
    private HostHolder hostHolder;
    
    @RequestMapping(path = "/like", method = RequestMethod.POST)
    @ResponseBody
    public String like(int entityType, int entityId) {
        User user = hostHolder.getUser();
        
        // 点赞
        likeService.like(user.getId(), entityType, entityId);
        
        // 数量
        long likeCount = likeService.findEntityLikeCount(entityType, entityId);
        // 状态
        int likeStatus = likeService.findEntityLikeStatus(user.getId(), entityType, entityId);
        // 返回的结果
        Map<String, Object> map = new HashMap<>();
        map.put("likeCount", likeCount);
        map.put("likeStatus", likeStatus);
        
        return CommunityUtil.getJSONString(0, null, map);
    }
    
}
```



对HomeController进行补充，当用户点击帖子的时候，显示帖子的赞数目。

![image-20231128225302812](E:\Typroa_Picture\image-20231128225302812.png)

对帖子详情的Controller进行补充，并且显示当前的用户是否点赞了进行判断。

![image-20231128225436332](E:\Typroa_Picture\image-20231128225436332.png)

### 4.4我收到的赞

![image-20231129135732907](E:\Typroa_Picture\image-20231129135732907.png)

因为个人信息，需要有获得点赞总个数。 那么就需要用户对帖子，对评论点赞的时候一块把对用户点赞的总数统计。

所以需要重构点赞方法，并且给帖子点赞的同时，把UserId的点赞数目也增加。

1.增加Redis对于用户的Key

```java
    // 某个用户的赞
    // like:user:userId -> int
    public static String getUserLikeKey(int userId) {
        return PREFIX_USER_LIKE + SPLIT + userId;
    }

```

重写Like方法

```java
 //点赞功能，如果该用户已经点赞了那么就取消点赞，没有点赞就继续点赞
    public void like(int userId, int entityType, int entityId, int entityUserId){
        //该点赞涉及到两个操作，所以需要添加一个事务，使得 同时成功，同时失败
        redisTemplate.execute(new SessionCallback() {
            @Override
            public Object execute(RedisOperations operations) throws DataAccessException {
                //！！！因为事务开启后，操作都会放在队列中，所以查询操作需要提前查询。
                String entityLikeKey=RedisKeyUtil.getEntityLikeKey(entityType,entityId);
                String userLikeKey=RedisKeyUtil.getUserLikeKey(entityUserId);
                
                boolean isMember=operations.opsForSet().isMember(entityLikeKey,userId);
                //开启事务
                operations.multi();
                    if(isMember){//取消点赞
                        operations.opsForSet().remove(entityLikeKey,userId);
                        operations.opsForValue().decrement(userLikeKey);
                    }else{
                        operations.opsForSet().add(entityLikeKey,userId);
                        operations.opsForValue().increment(userLikeKey);
                    }
                 //提交事务
                return operations.exec();
            }
            
        });
     
    }
```

添加一个可以查询用户获得点赞的总数量

```java
    //查询某一个人获得的总数量
    public int findUserLikeCount(int userId){
        String userLikeKey=RedisKeyUtil.getUserLikeKey(userId);
        Integer count =(Integer)redisTemplate.opsForValue().get(userLikeKey);
        return count==null? 0:count.intValue();
    }
```



Controller，在UserController中。点解个人主页。显示个人信息，并且显示获得赞的总数

```java
//显示个人主页
    @RequestMapping(path = "/profile/{userId}",method = RequestMethod.GET)
    public String getProfilePage(@PathVariable("userId") int userId,Model model){
        User user=userService.findUserById(userId);
        if(user==null){
            throw new RuntimeException("该用户不存在");
        }
        //返回用户
        model.addAttribute("user",user);
        //点赞数量
        int likeCount =likeService.findUserLikeCount(userId);
        model.addAttribute("likeCount",likeCount);
        
        return "/site/profile";
    }
```

### 4.5关注与取消关注

 

![image-20231129150001115](E:\Typroa_Picture\image-20231129150001115.png)

1.编写Util或者该关注的RedisKey

```java
followee:userId:entityType ->zset (entityId,时间)
followee:目标
userId:关注目标的那个用户
entityTpye:类型是什么
//连在一起就是：该用户关注了什么类型(帖子，题目，用户) 该类型的Id为entityId

```

```java
// 某个实体拥有的粉丝
    // follower:entityType:entityId -> zset(userId,now)
    public static String getFollowerKey(int entityType, int entityId) {
        return PREFIX_FOLLOWER + SPLIT + entityType + SPLIT + entityId;
    }
follower:entityType:entityId
follower:粉丝
entityType:该实体
entityId:实体的Id。
//连在一起就是：该实体拥有多少个粉丝。因为entityType+entityId才能唯一标识一个实体
```

2.编写FollowService实例

```java
package com.nowcoder.community.service;

import com.nowcoder.community.util.RedisKeyUtil;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.dao.DataAccessException;
import org.springframework.data.redis.core.RedisOperations;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.core.SessionCallback;
import org.springframework.stereotype.Service;

@Service
public class FollowService {
    @Autowired
    RedisTemplate redisTemplate;
    
    //实现关注功能
    public void follow(int userId,int entityId,int entityType){
        redisTemplate.execute(new SessionCallback() {
            @Override
            public Object execute(RedisOperations operations) throws DataAccessException {
                //获取key
                String followeeKey = RedisKeyUtil.getFolloweeKey(userId, entityType);
                String followerKey = RedisKeyUtil.getFollowerKey(entityType, entityId);
                
                //事务开始
                operations.multi();
                //该用户关注了那个实体
                operations.opsForZSet().add(followeeKey,entityId,System.currentTimeMillis());
                //该实体被用户关注了
                operations.opsForZSet().add(followerKey,userId,System.currentTimeMillis());
                return operations.exec();
            }
        });
    }
    
    //实现去关功能
    public void unfollow(int userId, int entityType, int entityId) {
        redisTemplate.execute(new SessionCallback() {
            @Override
            public Object execute(RedisOperations operations) throws DataAccessException {
                String followeeKey = RedisKeyUtil.getFolloweeKey(userId, entityType);
                String followerKey = RedisKeyUtil.getFollowerKey(entityType, entityId);
            
                operations.multi();
            
                operations.opsForZSet().remove(followeeKey, entityId);
                operations.opsForZSet().remove(followerKey, userId);
            
                return operations.exec();
            }
        });
    }
    
    //查询关注了哪些的实体数量
    public long findFolloweeCount(int userId,int entityType){
        String followKey=RedisKeyUtil.getFolloweeKey(userId,entityType);
        return redisTemplate.opsForZSet().zCard(followKey);
    
    }
    
    //查询该实体被关注的数量
    public long findFollwerCount(int entityType, int entityId){
        String followerKey=RedisKeyUtil.getFollowerKey(entityType,entityId);
        return redisTemplate.opsForZSet().zCard(followerKey);
    }
    
    //查询当前用户是否关注了该实体
    public boolean hasFollowed(int userId, int entityType, int entityId) {
        String followeeKey = RedisKeyUtil.getFolloweeKey(userId, entityType);
        return redisTemplate.opsForZSet().score(followeeKey, entityId) != null;
    }
    
}

```

3.编写FollowController

```java
@Controller
public class FollowController {
    
    @Autowired
    private FollowService followService;
    
    @Autowired
    private HostHolder hostHolder;
    //点击关注按钮会触发的事件
    @RequestMapping(path = "/follow", method = RequestMethod.POST)
    @ResponseBody
    public String follow(int entityType, int entityId) {
        User user = hostHolder.getUser();
        
        followService.follow(user.getId(), entityType, entityId);
        
        return CommunityUtil.getJSONString(0, "已关注!");
    }
    
    @RequestMapping(path = "/unfollow", method = RequestMethod.POST)
    @ResponseBody
    public String unfollow(int entityType, int entityId) {
        User user = hostHolder.getUser();
        
        followService.unfollow(user.getId(), entityType, entityId);
        
        return CommunityUtil.getJSONString(0, "已取消关注!");
    }
    
}
```

4.更改UserController的显示用户页面

```java
    //显示个人主页
    @RequestMapping(path = "/profile/{userId}",method = RequestMethod.GET)
    public String getProfilePage(@PathVariable("userId") int userId,Model model){
        User user=userService.findUserById(userId);
        if(user==null){
            throw new RuntimeException("该用户不存在");
        }
        //返回用户
        model.addAttribute("user",user);
        //点赞数量
        int likeCount =likeService.findUserLikeCount(userId);
        model.addAttribute("likeCount",likeCount);
       
        // 关注数量
        long followeeCount = followService.findFolloweeCount(userId, ENTITY_TYPE_USER);
        model.addAttribute("followeeCount", followeeCount);
        // 粉丝数量
        long followerCount = followService.findFollowerCount(ENTITY_TYPE_USER, userId);
        model.addAttribute("followerCount", followerCount);
        // 是否已关注
        boolean hasFollowed = false;
        if (hostHolder.getUser() != null) {
            hasFollowed = followService.hasFollowed(hostHolder.getUser().getId(), ENTITY_TYPE_USER, userId);
        }
        model.addAttribute("hasFollowed", hasFollowed);
        
        return "/site/profile";
    }
    
```

4.编写profile.js文件

```js
$(function(){
	$(".follow-btn").click(follow);
});

function follow() {
	var btn = this;
	if($(btn).hasClass("btn-info")) {
		// 关注TA
		$.post(
			CONTEXT_PATH + "/follow",
			{"entityType":3,"entityId":$(btn).prev().val()},
			function(data) {
				data = $.parseJSON(data);
				if(data.code == 0) {
					window.location.reload();
				} else {
					alert(data.msg);
				}
			}
		);
		// $(btn).text("已关注").removeClass("btn-info").addClass("btn-secondary");
	} else {
		// 取消关注
		$.post(
			CONTEXT_PATH + "/unfollow",
			{"entityType":3,"entityId":$(btn).prev().val()},
			function(data) {
				data = $.parseJSON(data);
				if(data.code == 0) {
					window.location.reload();
				} else {
					alert(data.msg);
				}
			}
		);
		//$(btn).text("关注TA").removeClass("btn-secondary").addClass("btn-info");
	}
}
```

### 4.6关注列表、粉丝列表

![image-20231130113411944](E:\Typroa_Picture\image-20231130113411944.png)

1.编写FollowService

```java
//查询某个用户关注的人
    public List<Map<String,Object>> findFollowees(int userId, int offset, int limit){
        String followeeKey=RedisKeyUtil.getFolloweeKey(userId,ENTITY_TYPE_USER);
        Set<Integer> targetIds=redisTemplate.opsForZSet().reverseRange(followeeKey,offset,offset+limit-1);
        if(targetIds==null){
            return null;
        }
        //其中list保存各种map信息，map又保存用户信息
        List<Map<String,Object>> list=new ArrayList<>();
        for(Integer targetId:targetIds){
            Map<String,Object> map = new HashMap<>();
            //保存的是该用户关注哪些人的对象
            User user =service.findUserById(targetId);
            map.put("user",user);
            //保存的是该用户的分数
            Double score=redisTemplate.opsForZSet().score(followeeKey,targetId);
            map.put("followTime",new Date(score.longValue()));
            list.add(map);
        }
        return list;
    }
```

```java
 //查询某个用户的粉丝
    public List<Map<String,Object>> findFollowers(int userId, int offset, int limit){
        String followerKey=RedisKeyUtil.getFollowerKey(ENTITY_TYPE_USER,userId);
        Set<Integer> targetIds=redisTemplate.opsForZSet().reverseRange(followerKey,offset,offset+limit-1);
        if(targetIds==null){
            return null;
        }
        //其中list保存各种map信息，map又保存用户信息
        List<Map<String,Object>> list=new ArrayList<>();
        for(Integer targetId:targetIds){
            Map<String,Object> map = new HashMap<>();
            //保存的改实体被那些人关注
            User user =service.findUserById(targetId);
            map.put("user",user);
            //保存的是该用户的分数
            Double score=redisTemplate.opsForZSet().score(followerKey,targetId);
            map.put("followTime",new Date(score.longValue()));
            list.add(map);
        }
        return list;
    }
```

2.编写FollowerController

```java
//显示该用户的关注的人
    @RequestMapping(path = "/followees/{userid}",method = RequestMethod.GET)
    public String getFollowees(@PathVariable("userid") int userId, Model model, Page page){
        //出传入某一个人的用户Id
        User user=userService.findUserById(userId);
        if(user==null){
            throw new RuntimeException("该用户不存在");
        }
        model.addAttribute("user", user);
        page.setLimit(5);
        page.setPath("/followees/" + userId);
        //该项目只能关注用户。followee:userId:entityType;
        page.setRows((int) followService.findFolloweeCount(userId,ENTITY_TYPE_USER));
        
        //获取该userId用户的关注的人
        List<Map<String,Object>> userList =followService.findFollowees(userId,page.getOffset(),page.getLimit());
        if(userList!=null){
            //获取粉丝数
            for (Map<String,Object> map :userList){
                User u= (User) map.get("user");
                //并且增加是否关注了信息
                map.put("hasFollowed",hasFollowed(u.getId()));
            }
        }
        model.addAttribute("users",userList);
        return "/site/follower";
    }
```

```java
    //查询该头像的用户有多少个粉丝。
    @RequestMapping(path = "/followers/{userId}", method = RequestMethod.GET)
    public String getFollowers(@PathVariable("userId") int userId, Page page, Model model) {
        //该User是查看那个信息的Id
        User user = userService.findUserById(userId);
        if (user == null) {
            throw new RuntimeException("该用户不存在!");
        }
        model.addAttribute("user", user);
        
        page.setLimit(5);
        page.setPath("/followers/" + userId);
        page.setRows((int) followService.findFollowerCount(ENTITY_TYPE_USER, userId));
        
        List<Map<String, Object>> userList = followService.findFollowers(userId, page.getOffset(), page.getLimit());
        if (userList != null) {
            for (Map<String, Object> map : userList) {
                User u = (User) map.get("user");
                map.put("hasFollowed", hasFollowed(u.getId()));
            }
        }
        model.addAttribute("users", userList);
        
        return "/site/follower";
    }
    
```

查看当前登录用户是否对某些用户进行了关注

```java
    //查询登录的用户，与传进来的User是否有关注信息
    private boolean hasFollowed(int userId){
        if (hostHolder.getUser()==null){
            return false;
        }
        return followService.hasFollowed(hostHolder.getUser().getId(),ENTITY_TYPE_USER,userId);
    }
```

### 4.7优化登录模块

![image-20231130163709299](E:\Typroa_Picture\image-20231130163709299.png)



**重构存储验证码**

1.RedisKeyUtil 设置验证码的Key。

```java
    //获取验证码的Key。owner是传入的随机字符串
    public static String getKaptchaKey(String owner){
        return PREFIX_KAPTCHA+SPLIT+owner;
    }
```

2.重构Login的Controller功能。生成验证码，以及登录验证。

生成随机字符串存放在Cookie中。 ，随机字符串的值存放在redis中。

用户输入的验证码与cookie中的值看是否一样。





![image-20231130184916893](E:\Typroa_Picture\image-20231130184916893.png)

![image-20231130185006103](E:\Typroa_Picture\image-20231130185006103.png)

![image-20231130185322785](E:\Typroa_Picture\image-20231130185322785.png)

每次刷新kaptchOwner都会刷新。



总结->Cookie(kaptchOwner)是一个组随机字符串，随机字符串的值会保存在redis中。

用户输入code 与这个redis值是否相同。

**用redis存储登录凭证**

我们存放用户凭证ticket，不需要用Mysql存放了，需要改成Redis存放。所以LoginMapper不需要用了。但是其他都需要，LoginTicket需要，service需要。

1.增加RedisKey

```java
    //生成登录凭证的key
    public static String getTicketKey(int ticket){
        return PREFIX_TICKET+SPLIT+ticket;
    }

```

2.修改uservice

![image-20231130200527942](E:\Typroa_Picture\image-20231130200527942.png)

![image-20231130200542207](E:\Typroa_Picture\image-20231130200542207.png)

![image-20231130200554011](E:\Typroa_Picture\image-20231130200554011.png)



**使用redis换成换成登录凭证**

1.增加RedisKey

```java
    private static final String PREFIX_USER = "user";//用户   

//生成用户的Key
    public static String getUserKey(int userId) {
        return PREFIX_USER + SPLIT + userId;
    }
```

2.增加service方法

```java
    //1.优先从缓存中取值
    private User getCache(int userId){
        String userKey=RedisKeyUtil.getUserKey(userId);
        return (User) redisTemplate.opsForValue().get(userKey);
    }
    //2.当缓冲中没有用户数据，那么就增加user进缓存
    private User initCache(int userId){
        String userKey=RedisKeyUtil.getUserKey(userId);
        User user=findUserById(userId);
        redisTemplate.opsForValue().set(userKey,user,3600, TimeUnit.SECONDS);
        return user;
    }
    //3.数据变更时候，要把缓存的User删除
    private void clearCache(int userId){
        String userKey=RedisKeyUtil.getUserKey(userId);
        redisTemplate.delete(userKey);
    }
}
```

查找用户非常常用的方法，所以可以缓存一下user数据。因为redis保存这user的jason数据

![image-20231130204012737](E:\Typroa_Picture\image-20231130204012737.png)

![image-20231130204124359](E:\Typroa_Picture\image-20231130204124359.png)

![image-20231130204139927](E:\Typroa_Picture\image-20231130204139927.png)

# 第五章：kafaka构建TB级异步消息系统

### 5.1消息队列

![image-20231130205444859](E:\Typroa_Picture\image-20231130205444859.png)

1.当阻塞队列满了的时候就会停止生产。

### 5.2 kafka介绍

![image-20231130211231060](E:\Typroa_Picture\image-20231130211231060.png)

1.broker：是服务器

2.Topic是：一个主题，主题可以有很多partion .主题含义：就是你可以是点赞主题，评论主题等。

7.partion是消息队列。

3.Offset：索引

4.leader Replica：主副本，用于备份

5.follower Replica ：从副本，只保存数据。不负责访问。

6.Zookeeper：管理集群



### 5.* 启动kafka命令

先启动[zookeeper](https://so.csdn.net/so/search?q=zookeeper&spm=1001.2101.3001.7020),默认自带的

```
bin/zookeeper-server-start.sh config/zookeeper.properties
```

然后启动kafka服务

```
bin/kafka-server-start.sh config/server.properties
```



**Windows启动kafka**

启动 Zookeeper 服务, 默认端口 2181

```java
bin\windows\zookeeper-server-start.bat config\zookeeper.properties
```

启动 Kafka 服务,默认端口 9092

```java
bin\windows\kafka-server-start.bat config\server.properties
```



### 5.3 Spring整合kafka

![image-20231130215814657](E:\Typroa_Picture\image-20231130215814657.png)

kafkaTemplate以及给Spring 整合了，直接注入就可以。并不需要自己声明一个Bean

生产者：通过send方法就可以把数据发送到topic中

消费者：1.在方法上添加注解, 并且监听topics="test"的消息队列。**一旦**有信息可以获取，就会把信息存放在ConsumerRecord record中。

**生产者的动作是主动的，消费者的动作是被动的。**

```java
public class KafkaTests {

    @Autowired
    private KafkaProducer kafkaProducer;

    @Test
    //需要手动执行
    public void testKafka() {
        kafkaProducer.sendMessage("test", "你ssadddddddddd好");
        kafkaProducer.sendMessage("test", "在asdddddddddddd吗");

        try {
            Thread.sleep(1000 * 10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

}

@Component
class KafkaProducer {

    @Autowired
    private KafkaTemplate kafkaTemplate;

    public void sendMessage(String topic, String content) {
        kafkaTemplate.send(topic, content);
    }

}

@Component
class KafkaConsumer {
    //当有消息传进来的时候会自动执行
    @KafkaListener(topics = {"test"})
    public void handleMessage(ConsumerRecord record) {
        System.out.println(record.value());
    }



```

### 5.4发送系统通知

![image-20231201133545300](E:\Typroa_Picture\image-20231201133545300.png)

1.定义一个对象，封装事件

```java
package com.nowcoder.community.entity;

import java.util.HashMap;
import java.util.Map;

public class Event {
    private String topic;//主题是什么，
    private int userId;//消息的发送者
    private int entityType;//对哪个实体
    private int entityId;//对帖子点赞，对用户关注
    private int entityUserId;//这个帖子或者是给点赞的主人是谁，因为最后需要发通知到另外一个对象的。
    private Map<String,Object> data =new HashMap<>();//这个可以用来接受消息个各种内容，其他信息包括在内
    public String getTopic() {
        return topic;
    }
    
    public Event setTopic(String topic) {
        this.topic = topic;
        return this;
    }
    
    public int getUserId() {
        return userId;
    }
    
    public Event setUserId(int userId) {
        this.userId = userId;
        return this;
    }
    
    public int getEntityType() {
        return entityType;
    }
    
    public Event setEntityType(int entityType) {
        this.entityType = entityType;
        return this;
    }
    
    public int getEntityId() {
        return entityId;
    }
    
    public Event setEntityId(int entityId) {
        this.entityId = entityId;
        return this;
    }
    
    public int getEntityUserId() {
        return entityUserId;
    }
    
    public Event setEntityUserId(int entityUserId) {
        this.entityUserId = entityUserId;
        return this;
    }
    
    public Map<String, Object> getData() {
        return data;
    }
    
    public Event setData(String key, Object value) {
        this.data.put(key, value);
        return this;
    }
}

```

2.构造生产者与消费者 。

生产者：发送消息的内容不是字符串，而是事件的所有消息Event。可以发送json数据。

```java
@Component
public class EventProducer {
    @Autowired
    private KafkaTemplate kafkaTemplate;
    
    //处理事件
    public void fireEvent(Event event){
        // 将事件发布到指定的主题,直接把整个事件发送给消费者，让他自己筛选信息。
        kafkaTemplate.send(event.getTopic(), JSONObject.toJSONString(event));
    }
}
```

消费者：接受的数据要从json转为字符串。 需要把该字符串封装成一条消息Message，存入到数据库中。

```java
@Component
public class EventConsumer implements CommunityConstant {
    private static final Logger logger = LoggerFactory.getLogger(EventConsumer.class);
    
    @Autowired
    private MessageService messageService;//直接在消息队列使用Service层，该类没有经过Controller层。所以会在切面编程使得NULL异常
    //对点赞，关注，评论进行监听
    @KafkaListener(topics = {TOPIC_COMMENT, TOPIC_LIKE, TOPIC_FOLLOW})
    public void handleCommentMessage(ConsumerRecord record){
        if (record == null || record.value() == null) {
            logger.error("消息的内容为空!");
            return;
        }
        //接受生产者传过来的Event
        Event event = JSONObject.parseObject(record.value().toString(), Event.class);
        if (event == null) {
            logger.error("消息格式错误!");
            return;
        }
        
        //获得消息了，然后需要把他存入到数据库中,把消息存入到Message表中
        Message message = new Message();
        message.setFromId(SYSTEM_USER_ID);
        message.setToId(event.getEntityUserId());
        message.setConversationId(event.getTopic());
        message.setCreateTime(new Date());
        
        //上面是消息的基本信息，下面是消息的内容是什么，我们可以用Map 保存来自Envent中基本信息，并且获取的map信息
        Map<String, Object> content = new HashMap<>();
        content.put("userId", event.getUserId());
        content.put("entityType", event.getEntityType());
        content.put("entityId", event.getEntityId());
        
        //Data是一个map.这是一个遍历Map的方法之一
        if (!event.getData().isEmpty()) {
            for (Map.Entry<String, Object> entry : event.getData().entrySet()) {
                content.put(entry.getKey(), entry.getValue());
            }
        }
        //message以及获取了所有内容了，并且插入了数据库
        message.setContent(JSONObject.toJSONString(content));
        messageService.addMessage(message);
  
    }
}
```

![image-20231201140716096](E:\Typroa_Picture\image-20231201140716096.png)

1.conversation_id 如果用1_149并接，因为from_id 永远是1.所以直接用一个comment表示。

2.更改点赞、评论、关注Controller。增加内容

![image-20231201161104022](E:\Typroa_Picture\image-20231201161104022.png)

![image-20231201161617618](E:\Typroa_Picture\image-20231201161617618.png)

![image-20231201163443334](E:\Typroa_Picture\image-20231201163443334.png)

3.补充CommentMapper，service，controller方法。



### 5.5显示系通知信息。



![image-20231201163548741](E:\Typroa_Picture\image-20231201163548741.png)

1.系统通知列表需要构造。

所以需要增加MessgeMapper，MessageService，MessageController。

1.增加MessageMapper方法

```java
    //查询某个主题下的最新通知
    Message selectLatestNotice(int userId, String topic);
    
    //查询某个主题下的包含的通知数量
    int selectNoticeCount(int userId, String topic);
    
    //查询未读消息的通知数量,topic为Null就是查询所有的未读消息总数
    int selectNoticeUnreadCount(int userId, String topic);
```

```xml
    <select id="selectLatestNotice" resultType="Message">
        select <include refid="selectFields"></include>
        from message
        where id in (
        select max(id) from message
        where status != 2
        and from_id = 1
        and to_id = #{userId}
        and conversation_id = #{topic}
        )
    </select>

    <select id="selectNoticeCount" resultType="int">
        select count(id) from message
        where status != 2
        and from_id = 1
        and to_id = #{userId}
          and conversation_id = #{topic}
    </select>

    <!--当传入topic就是寻找某一个会话，当传入就是总的未读数量-->
    <select id="selectNoticeUnreadCount" resultType="int">
        select count(id) from message
        where status = 0
        and from_id = 1
        and to_id = #{userId}
        <if test="topic!=null">
            and conversation_id = #{topic}
        </if>
    </select>
```

2.创建service

```java
    //查询某个主题下的最新通知
    Message selectLatestNotice(int userId, String topic);
    
    //查询某个主题下的包含的通知数量
    int selectNoticeCount(int userId, String topic);
    
    //查询未读消息的通知数量,topic为Null就是查询所有的未读消息总数
    int selectNoticeUnreadCount(int userId, String topic);
```

3.编写Controller类型

```java
 //查看系统通知的功能,
    @RequestMapping(path = "/notice/list",method = RequestMethod.GET)
    public String getNoticeList(Model model){
        User user =hostHolder.getUser();
        
        //查询评论类的通知,只有一条最新的消息
        Message message = messageService.findLatestNotice(user.getId(), TOPIC_COMMENT);
        Map<String, Object> messageVO = new HashMap<>();
        if (message != null) {
            messageVO.put("message", message);

            
            //内容 content：{"entityType":1,"entityId":280,"postId":280,"userId":149}
            //这是防止内容变成HTML解析
            String content = HtmlUtils.htmlUnescape(message.getContent());
            //这行代码使用了阿里巴巴的 FastJSON 库，将一个 JSON 字符串 content 转换为 Java 中的 Map<String, Object> 对象。
            Map<String, Object> data = JSONObject.parseObject(content, HashMap.class);
            //将内容全部解析出来
            messageVO.put("user", userService.findUserById((Integer) data.get("userId")));
            messageVO.put("entityType", data.get("entityType"));
            messageVO.put("entityId", data.get("entityId"));
            messageVO.put("postId", data.get("postId"));
    
            int count = messageService.findNoticeCount(user.getId(), TOPIC_COMMENT);
            messageVO.put("count", count);
    
            int unread = messageService.findNoticeUnreadCount(user.getId(), TOPIC_COMMENT);
            messageVO.put("unread", unread);
  
        }
        model.addAttribute("commentNotice", messageVO);
    
        // 查询点赞类通知
        message = messageService.findLatestNotice(user.getId(), TOPIC_LIKE);
        messageVO = new HashMap<>();
        if (message != null) {
            messageVO.put("message", message);
        
            String content = HtmlUtils.htmlUnescape(message.getContent());
            Map<String, Object> data = JSONObject.parseObject(content, HashMap.class);
        
            messageVO.put("user", userService.findUserById((Integer) data.get("userId")));
            messageVO.put("entityType", data.get("entityType"));
            messageVO.put("entityId", data.get("entityId"));
            messageVO.put("postId", data.get("postId"));
        
            int count = messageService.findNoticeCount(user.getId(), TOPIC_LIKE);
            messageVO.put("count", count);
        
            int unread = messageService.findNoticeUnreadCount(user.getId(), TOPIC_LIKE);
            messageVO.put("unread", unread);
        }
        model.addAttribute("likeNotice", messageVO);
    
        // 查询关注类通知
        message = messageService.findLatestNotice(user.getId(), TOPIC_FOLLOW);
        messageVO = new HashMap<>();
        if (message != null) {
            messageVO.put("message", message);
        
            String content = HtmlUtils.htmlUnescape(message.getContent());
            Map<String, Object> data = JSONObject.parseObject(content, HashMap.class);
        
            messageVO.put("user", userService.findUserById((Integer) data.get("userId")));
            messageVO.put("entityType", data.get("entityType"));
            messageVO.put("entityId", data.get("entityId"));
        
            int count = messageService.findNoticeCount(user.getId(), TOPIC_FOLLOW);
            messageVO.put("count", count);
        
            int unread = messageService.findNoticeUnreadCount(user.getId(), TOPIC_FOLLOW);
            messageVO.put("unread", unread);
        }
        model.addAttribute("followNotice", messageVO);
    
        // 查询未读消息数量
        int letterUnreadCount = messageService.findLetterUnreadCount(user.getId(), null);
        model.addAttribute("letterUnreadCount", letterUnreadCount);
        int noticeUnreadCount = messageService.findNoticeUnreadCount(user.getId(), null);
        model.addAttribute("noticeUnreadCount", noticeUnreadCount);
    
        return "/site/notice";
    }
```

**查看系统信息详情**

1.增加MessageMper和XML文件

```java
 //查询某个主题所包含的所有通知系列
List<Message> selectNotices(int userId, String topic, int offset, int limit);

<select id="selectNotices" resultType="Message">
        select <include refid="selectFields"></include>
        from message
        where status != 2
        and from_id = 1
        and to_id = #{userId}
        and conversation_id = #{topic}
        order by create_time desc
        limit #{offset}, #{limit}
 </select>
```

2.service层

```java
    //查询某一个会话的所有信息。
    public List<Message> findNotices(int userId, String topic, int offset, int limit) {
        return messageMapper.selectNotices(userId, topic, offset, limit);
    }
```

3.Controller

```java
    @RequestMapping(path = "/notice/detail/{topic}",method = RequestMethod.GET)
    public String getNoticeDetail(@PathVariable("topic") String topic , Page page, Model model){
        User user=hostHolder.getUser();
        
        page.setLimit(5);
        page.setPath("/notice/detail/"+topic);
        page.setRows(messageService.findNoticeCount(user.getId(), topic));
        
        List<Message> noticeList =messageService.findNotices(user.getId(), topic,page.getOffset(),page.getLimit());
        List<Map<String,Object>> noticeVoList=new ArrayList<>();
        if(noticeList!=null){
            for(Message notice :noticeList){
                Map<String,Object> map=new HashMap<>();
                // 通知
                map.put("notice",notice);
                
                //内容
                String content=HtmlUtils.htmlUnescape(notice.getContent());
                Map<String, Object> data = JSONObject.parseObject(content, HashMap.class);
                map.put("user", userService.findUserById((Integer) data.get("userId")));
                map.put("entityType", data.get("entityType"));
                map.put("entityId", data.get("entityId"));
                map.put("postId", data.get("postId"));
                
                //是谁通知的作者，一般都是系统
                map.put("fromUser",userService.findUserById(notice.getFromId()));
    
                noticeVoList.add(map);
            }
        }
        model.addAttribute("notices", noticeVoList);
        
        //当用户点击详情信息的时候，那么就会把通知改成已读
        List<Integer> ids=getLetterIds(noticeList);
        if (!ids.isEmpty()) {
            messageService.readMessage(ids);
        }
    
        return "/site/notice-detail";
    }
```

**增加一个拦截器，每当有一个请求就要查拦截一下，页面头的消息跟新一下**

```java
@Component
public class MessageInterceptor implements HandlerInterceptor {
    @Autowired
    private MessageService messageService;
    @Autowired
    private HostHolder hostHolder;
    
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView){
        User user =hostHolder.getUser();
        if(user !=null&&modelAndView!=null){
            int letterUnreadCount = messageService.findLetterUnreadCount(user.getId(), null);
            int noticeUnreadCount = messageService.findNoticeUnreadCount(user.getId(), null);
            modelAndView.addObject("allUnreadCount", letterUnreadCount + noticeUnreadCount);
        }
    }
}
```

配置拦截器

```java
        registry.addInterceptor(messageInterceptor)
                .excludePathPatterns("/**/*.css", "/**/*.js", "/**/*.png", "/**/*.jpg", "/**/*.jpeg");
```



# 第六章：Elasticsearch进行搜索

### 6.1Elasticsearch入门



![image-20231201214714723](E:\Typroa_Picture\image-20231201214714723.png)



有点像Mysql
1.索引 类似与Mysql的数据库

2.类型 类似于Mysql的表

3.文档 类似于mysql的一行数据。文档保存的数据一般以json格式保存

4.文档里面的数据叫字段。相当于Mysql中的一列。

最新的ES6.0以后。索引表示一张表，类型就不怎么用了。

5.多台服务器组合在一起叫：集群

6.集群中的一个服务器叫节点。

7.可以将 索引分成多个分片。

8.一个副本是一个分片的备份。



**配置elasticserach.yml**

cluster.name: nowcoder

path.data: E:\Java_explore_Envirnment\Elasticserach\data

path.logs: E:\Java_explore_Envirnment\Elasticserach\log

****

配置**elasticserach环境变量**

```
E:\Java_explore_Envirnment\Elasticserach\elasticsearch-6.4.3\bin
```

安装elasticserach中文分词搜索插件。

分词插件一定要安装在

E:\Java_explore_Envirnment\Elasticserach\elasticsearch-6.4.3\plugins\ik  下



**用Postman进行访问ES服务器**

1.查询ES服务器有多少个索引：localhost:9200/_cat/indices?v (GET)

![image-20231201223853530](E:\Typroa_Picture\image-20231201223853530.png)

2.新建索引 localhost:9200/索引名字(PUT)

![image-20231201223838527](E:\Typroa_Picture\image-20231201223838527.png)

3.删除索引：localhost:9200/(索引名字)（DELECT）

![image-20231201224010921](E:\Typroa_Picture\image-20231201224010921.png)

4.增加数据

localhost:9200/(索引名字)/_doc/id. (PUT)

选择响应体Boyd  

raw Json格式

![image-20231201224714019](E:\Typroa_Picture\image-20231201224714019.png)

5.删除数据：local host:9200/(索引)/_doc/id(DELECAT)



![image-20231201224908359](E:\Typroa_Picture\image-20231201224908359.png)



使用：类似与查询数据库一样。

1.先构建索引（表）、再往索引插入数据。

2.这个服务器会给我们自动分词

3.查询：当在表里面查询数据，就会查询到有分词的内容

简单查询title

![image-20231201225916846](E:\Typroa_Picture\image-20231201225916846.png)

简单查询content

![image-20231201225844523](E:\Typroa_Picture\image-20231201225844523.png)

复杂查询：又要查询title，又要查询content

![image-20231201225820349](E:\Typroa_Picture\image-20231201225820349.png)



### 6.2Spring 整合ElasticSearch

![image-20231202145836798](E:\Typroa_Picture\image-20231202145836798.png)

Template和Repository都可可以使用。

把数据中的帖子放到ES服务器中

1.导入依赖

```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-elasticsearch</artifactId>
		</dependency>
```

2.在properties配置

```properties
# ElasticsearchProperties
spring.data.elasticsearch.cluster-name=nowcoder
spring.data.elasticsearch.cluster-nodes=127.0.0.1:9300
```

3.解决Netty与Redis启动冲突问题

```java
这段代码使用了 @PostConstruct 注解，表明这是一个在 Spring Bean 初始化阶段执行的方法。
在这个方法中，通过 System.setProperty 设置了一个系统属性："es.set.netty.runtime.available.processors" 的值为 "false"。这是为了解决 Netty 启动时的冲突问题。

@PostConstruct
    public void init() {
        // 解决netty启动冲突问题
        // see Netty4Utils.setAvailableProcessors()
        System.setProperty("es.set.netty.runtime.available.processors", "false");
    }
```

4.配置数据库哪些内容与ES对应，通过注解配置。 配置哪个实体的数据需要存放到ES中

5.加了注解后，Spring会自动把实体与ES服务器映射。但是需要我们自己声明规则：就是需要映射哪个索引，有没有副本等。

```java
//用该实体创建在ES类似的索引， 一下四个参数分别是：索引，_doc(舍弃) 分片，副本
@Document(indexName = "discusspost", type = "_doc", shards = 6, replicas = 3)
public class DiscussPost {
    //给属性分配"列"的类型
    @Id
    private int id;
    @Field(type = FieldType.Integer)
    private int userId;
    //title和Content是需要查询的内容。analyzer:分词器 searchAnalyzer:搜索器
    @Field(type = FieldType.Text, analyzer = "ik_max_word", searchAnalyzer = "ik_smart")
    private String title;
    @Field(type = FieldType.Text, analyzer = "ik_max_word", searchAnalyzer = "ik_smart")
    private String content;
    @Field(type = FieldType.Integer)
    private int type;
    @Field(type = FieldType.Integer)
    private int status;
    @Field(type = FieldType.Date)
    private Date createTime;
    @Field(type = FieldType.Integer)
    private int commentCount;
    @Field(type = FieldType.Double)
    private double score;

```

6.实现一个ElasticsearchRepository接口

```java
@Repository
public interface DiscussPostRepository extends ElasticsearchRepository<DiscussPost, Integer> {

}
```


这段代码定义了一个 Spring Data Elasticsearch 的 Repository 接口，用于与 Elasticsearch 交互。具体来说，它是一个用于操作 `DiscussPost` 实体的接口，该实体映射到 Elasticsearch 中的索引。

让我们逐步解释这段代码：

- `@Repository`: 这个注解表明该接口是一个 Spring Data Repository，用于访问数据存储。在这种情况下，它是用于访问 Elasticsearch 存储的数据。
- `ElasticsearchRepository<DiscussPost, Integer>`: 这个接口继承自 Spring Data Elasticsearch 提供的 `ElasticsearchRepository` 接口。泛型参数 `<DiscussPost, Integer>` **表示这个 Repository 处理的实体类型是 `DiscussPost`，而主键的类型是 `Integer`。**

通过继承 `ElasticsearchRepository` 接口，**`DiscussPostRepository` 就继承了一系列用于操作 Elasticsearch 的方法，如保存、查询、删除等。Spring Data Elasticsearch 将根据方法的命名规则自动生成相应的 Elasticsearch 查询。**例如，根据方法名的约定，如果你定义了一个名为 `findByTitle` 的方法，Spring Data Elasticsearch 将自动生成一个查询，用于按照标题查找文档。

7.实现把数据增、删、改放进ES服务器中

```java
public class ElasticsearchTests {
    
    @Autowired
    private DiscussPostMapper discussMapper;
    
    @Autowired
    DiscussPostRepository discussRepository;
    
    @Autowired
    ElasticsearchTemplate elasticTemplate;
    
    //从数据库中查询出来数据，再放进ES服务器中
    //把一个DiscussPost放入ES中
    @Test
    public void testInsert() {
        discussRepository.save(discussMapper.selectDiscussPostById(241));
        discussRepository.save(discussMapper.selectDiscussPostById(242));
        discussRepository.save(discussMapper.selectDiscussPostById(243));
    }
    
    //从数据库中把一个List<DiscussPost>放入ES中。
    @Test
    public void testInsertList() {
        discussRepository.saveAll(discussMapper.selectDiscussPosts(101, 0, 100));
        discussRepository.saveAll(discussMapper.selectDiscussPosts(102, 0, 100));
        discussRepository.saveAll(discussMapper.selectDiscussPosts(103, 0, 100));
        discussRepository.saveAll(discussMapper.selectDiscussPosts(111, 0, 100));
        discussRepository.saveAll(discussMapper.selectDiscussPosts(112, 0, 100));
        discussRepository.saveAll(discussMapper.selectDiscussPosts(131, 0, 100));
        discussRepository.saveAll(discussMapper.selectDiscussPosts(132, 0, 100));
        discussRepository.saveAll(discussMapper.selectDiscussPosts(133, 0, 100));
        discussRepository.saveAll(discussMapper.selectDiscussPosts(134, 0, 100));
    }
    //更新。其实就是把数据库的数据从新拿出来把ES中的数据覆盖
    @Test
    public void testUpdate() {
        DiscussPost post = discussMapper.selectDiscussPostById(231);
        post.setContent("我是新人,使劲灌水.");
        discussRepository.save(post);
    }
    //删除
    @Test
    public void testDelete() {
        // discussRepository.deleteById(231);
        discussRepository.deleteAll();
    }
```

8.查找数据是最重要的，构造搜索条件。最复杂的，因为需要把查找出来的数据进行标红等操作。

这里使用ElasticsearchTemplate，而不是Repository。因为Template可以拿标红数据。



```java
    @Test
    public void testSearchByTemplate() {
        SearchQuery searchQuery = new NativeSearchQueryBuilder()
                .withQuery(QueryBuilders.multiMatchQuery("互联网寒冬", "title", "content"))
                .withSort(SortBuilders.fieldSort("type").order(SortOrder.DESC))
                .withSort(SortBuilders.fieldSort("score").order(SortOrder.DESC))
                .withSort(SortBuilders.fieldSort("createTime").order(SortOrder.DESC))
                .withPageable(PageRequest.of(0, 10))
                .withHighlightFields(
                        new HighlightBuilder.Field("title").preTags("<em>").postTags("</em>"),
                        new HighlightBuilder.Field("content").preTags("<em>").postTags("</em>")
                ).build();
        
        Page<DiscussPost> page = elasticTemplate.queryForPage(searchQuery, DiscussPost.class, new SearchResultMapper() {
            @Override
            public <T> AggregatedPage<T> mapResults(SearchResponse response, Class<T> aClass, Pageable pageable) {
                //response就是查出的结果，hits感觉就像一个内容的集合一样
                SearchHits hits = response.getHits();
                if (hits.getTotalHits() <= 0) {
                    return null;
                }
                
                List<DiscussPost> list = new ArrayList<>();
                for (SearchHit hit : hits) {
                    DiscussPost post = new DiscussPost();
                    
                    String id = hit.getSourceAsMap().get("id").toString();
                    post.setId(Integer.valueOf(id));
                    
                    String userId = hit.getSourceAsMap().get("userId").toString();
                    post.setUserId(Integer.valueOf(userId));
                    
                    String title = hit.getSourceAsMap().get("title").toString();
                    post.setTitle(title);
                    
                    String content = hit.getSourceAsMap().get("content").toString();
                    post.setContent(content);
                    
                    String status = hit.getSourceAsMap().get("status").toString();
                    post.setStatus(Integer.valueOf(status));
                    
                    String createTime = hit.getSourceAsMap().get("createTime").toString();
                    post.setCreateTime(new Date(Long.valueOf(createTime)));
                    
                    String commentCount = hit.getSourceAsMap().get("commentCount").toString();
                    post.setCommentCount(Integer.valueOf(commentCount));
                    
                    // 处理高亮显示的结果,如果title是高亮的覆盖原有的
                    HighlightField titleField = hit.getHighlightFields().get("title");
                    if (titleField != null) {
                        post.setTitle(titleField.getFragments()[0].toString());
                    }
                    
                    HighlightField contentField = hit.getHighlightFields().get("content");
                    if (contentField != null) {
                        post.setContent(contentField.getFragments()[0].toString());
                    }
                    
                    list.add(post);
                }
                
                return new AggregatedPageImpl(list, pageable,
                        hits.getTotalHits(), response.getAggregations(), response.getScrollId(), hits.getMaxScore());
            }
        });
        
        System.out.println(page.getTotalElements());
        System.out.println(page.getTotalPages());
        System.out.println(page.getNumber());
        System.out.println(page.getSize());
        for (DiscussPost post : page) {
            System.out.println(post);
        }
    }
```

### 6.3开发社区的搜索功能

![image-20231202202636705](E:\Typroa_Picture\image-20231202202636705.png)

1.构建ElasticsearchService

```java
@Service
public class ElasticsearchService {
    //用来进行对ES服务器增删
    @Autowired
    DiscussPostRepository discussRepository;
    @Autowired
    ElasticsearchTemplate elasticTemplate;
    
    //将一个DiscussPost装入ES中
    public void saveDiscussPost(DiscussPost post){
        discussRepository.save(post);
    }
    //删除,通过传入ESid删除，其实也是DiscussPost的Id
    public void delectDiscussPost(int id){
        discussRepository.deleteById(id);
    }
    
    //显示ES查出来的DiscussPost，他是一个Page。这个Page是一个列表一样的，而不是我们自己定义的
    public Page<DiscussPost> searchDiscussPost(String keyword, int current, int limit) {
        SearchQuery searchQuery = new NativeSearchQueryBuilder()
                .withQuery(QueryBuilders.multiMatchQuery(keyword, "title", "content"))
                .withSort(SortBuilders.fieldSort("type").order(SortOrder.DESC))
                .withSort(SortBuilders.fieldSort("score").order(SortOrder.DESC))
                .withSort(SortBuilders.fieldSort("createTime").order(SortOrder.DESC))
                .withPageable(PageRequest.of(current, limit))
                .withHighlightFields(
                        new HighlightBuilder.Field("title").preTags("<em>").postTags("</em>"),
                        new HighlightBuilder.Field("content").preTags("<em>").postTags("</em>")
                ).build();
    
        return elasticTemplate.queryForPage(searchQuery, DiscussPost.class, new SearchResultMapper() {
            @Override
            public <T> AggregatedPage<T> mapResults(SearchResponse response, Class<T> aClass, Pageable pageable) {
                SearchHits hits = response.getHits();
                if (hits.getTotalHits() <= 0) {
                    return null;
                }
            
                List<DiscussPost> list = new ArrayList<>();
                for (SearchHit hit : hits) {
                    DiscussPost post = new DiscussPost();
                
                    String id = hit.getSourceAsMap().get("id").toString();
                    post.setId(Integer.valueOf(id));
                
                    String userId = hit.getSourceAsMap().get("userId").toString();
                    post.setUserId(Integer.valueOf(userId));
                
                    String title = hit.getSourceAsMap().get("title").toString();
                    post.setTitle(title);
                
                    String content = hit.getSourceAsMap().get("content").toString();
                    post.setContent(content);
                
                    String status = hit.getSourceAsMap().get("status").toString();
                    post.setStatus(Integer.valueOf(status));
                
                    String createTime = hit.getSourceAsMap().get("createTime").toString();
                    post.setCreateTime(new Date(Long.valueOf(createTime)));
                
                    String commentCount = hit.getSourceAsMap().get("commentCount").toString();
                    post.setCommentCount(Integer.valueOf(commentCount));
                
                    // 处理高亮显示的结果
                    HighlightField titleField = hit.getHighlightFields().get("title");
                    if (titleField != null) {
                        post.setTitle(titleField.getFragments()[0].toString());
                    }
                
                    HighlightField contentField = hit.getHighlightFields().get("content");
                    if (contentField != null) {
                        post.setContent(contentField.getFragments()[0].toString());
                    }
                
                    list.add(post);
                }
            
                return new AggregatedPageImpl(list, pageable,
                        hits.getTotalHits(), response.getAggregations(), response.getScrollId(), hits.getMaxScore());
            }
        });
    }
}
```

2.在发布帖子时候增加功能、增加评论时候。DiscussPostController、CommentController

当帖子增加评论的时候，生产者需要把“事件”通知给消费者。消费者把评论内容放进ES服务器中。

```java
        if(comment.getEntityType()==ENTITY_TYPE_POST){//如果是对帖子的评论
            event=new Event()
                    .setTopic(TOPIC_PUBLISH)
                    .setEntityType(ENTITY_TYPE_POST)
                    .setEntityId(discussPostId)
                    .setUserId(comment.getUserId());
            eventProducer.fireEvent(event);
        }
```

![image-20231202214959526](E:\Typroa_Picture\image-20231202214959526.png)



3.生产者不用增加东西、消费者需要增加监听哪个类型。

```java
@KafkaListener(topics = {TOPIC_PUBLISH})
    public void handlePublishMessage(ConsumerRecord record){
        if(record==null||record.value()==null){
            logger.error("消息内容为空");
            return;
        }
        Event event=JSONObject.parseObject(record.value().toString(),Event.class);
        if (event == null) {
            logger.error("消息格式错误!");
            return;
        }
        
        //从even中获取基本信息，在数据库中找到该帖子，再放进ES
        DiscussPost post = discussPostService.findDiscussPostById(event.getEntityId());
        elasticsearchService.saveDiscussPost(post);
    }
```

4.展现ES服务器数据的功能，构建SearchController。

查询功能的数据不是从Mysql中提取，而是从ES服务器中查询。

```java
//查询不需要从数据
@Controller
public class SearchController implements CommunityConstant {
    

    @Autowired
    private ElasticsearchService elasticsearchService;
    @Autowired
    private UserService userService;
    @Autowired
    private LikeService likeService;
    
    // search?keyword=xxx
    @RequestMapping(path = "/search", method = RequestMethod.GET)
    public String search(String keyword, Page page, Model model){
        //搜索帖子 这里传入的是是第几页，差多少个
        org.springframework.data.domain.Page<DiscussPost> searchResult=
                elasticsearchService.searchDiscussPost(keyword,page.getCurrent()-1, page.getLimit());
        
        //searchResult保存了结果
        //接下来聚合数据
        List<Map<String,Object>> discussPosts=new ArrayList<>();
        if(searchResult!=null){
            for(DiscussPost post:searchResult){
                Map<String,Object> map=new HashMap<>();
                //帖子
                map.put("post",post);
                //作者
                map.put("user",userService.findUserById(post.getUserId()));
                //点赞数量
                map.put("likeCount",likeService.findEntityLikeCount(ENTITY_TYPE_POST,post.getId()));
                
                discussPosts.add(map);
            }
        }
        model.addAttribute("discussPosts", discussPosts);
        model.addAttribute("keyword", keyword);
        
        //自己的分页信息
        page.setRows(searchResult == null ? 0 : (int) searchResult.getTotalElements());
        page.setPath("/search?keyword=" + keyword);
        
        return "/site/search";
    }
    
}

```

# 第七章：为社区提供安全功能

### 7.1Spring Security

![image-20231203130835622](E:\Typroa_Picture\image-20231203130835622.png)

Spring Security是控制整个模块。

![ ](E:\Typroa_Picture\image-20231203132243407.png)

SecurityDemo

**1.导入依赖**

```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-security</artifactId>
		</dependency>
```

**2.对用户角色进行管理，需要在实体类实现一些方法。实体类要实现UserDetails接口**

```java
public class User implements UserDetails {

    private int id;
    private String username;
    private String password;
    private String salt;
    private String email;
    private int type;
    private int status;
    private String activationCode;
    private String headerUrl;
    private Date createTime;

    // true: 账号未过期.
    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    // true: 账号未锁定.
    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    // true: 凭证未过期.
    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    // true: 账号可用.
    @Override
    public boolean isEnabled() {
        return true;
    }

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        List<GrantedAuthority> list = new ArrayList<>();
        list.add(new GrantedAuthority() {
            @Override
            public String getAuthority() {
                switch (type) {
                    case 1:
                        return "ADMIN";
                    default:
                        return "USER";
                }
            }
        });
        return list;
    }

}

```

1. **实现 `UserDetails` 接口：**
   - `UserDetails` 是 Spring Security 中用于表示用户详细信息的接口。
   - `isAccountNonExpired()`: 判断账号是否未过期，这里始终返回 `true` 表示账号不会过期。
   - `isAccountNonLocked()`: 判断账号是否未锁定，这里也返回 `true` 表示账号不会被锁定。
   - `isCredentialsNonExpired()`: 判断凭证是否未过期，同样返回 `true` 表示凭证不会过期。
   - `isEnabled()`: 判断账号是否可用，返回 `true` 表示账号可用。
2. **实现 `getAuthorities()` 方法：**
   - `getAuthorities()`: 返回用户的授权信息，即用户拥有的权限集合。
   - 在这里，通过 `type` 字段的值判断用户的类型，如果 `type` 为 1，则赋予用户 "ADMIN" 权限，否则赋予 "USER" 权限。
   - 这里采用了匿名内部类实现 `GrantedAuthority` 接口，其 `getAuthority()` 方法返回用户的权限。

整体而言，这段代码是为了将用户的认证和授权信息整合到 Spring Security 中。在实际应用中，用户的类型（`type`）和相应的权限会根据具体需求进行设置。

**3.对UserService实现接口UserDetailsService**

```java
@Service
public class UserService implements UserDetailsService {

    @Autowired
    private UserMapper userMapper;

    public User findUserByName(String username) {
        return userMapper.selectByName(username);
    }

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        return this.findUserByName(username);
    }
}
```

在这段代码中，`UserService` 实现了 Spring Security 的 `UserDetailsService` 接口。这是 Spring Security 用于加载用户信息的核心接口之一。通过实现 `UserDetailsService` 接口，你可以自定义从不同的数据源（例如数据库、LDAP、内存等）加载用户信息。

具体来说：

1. **`UserDetailsService` 接口：**
   - Spring Security 中的 `UserDetailsService` 接口定义了一个方法 `loadUserByUsername(String username)`，该方法用于根据用户名加载用户信息。
   - 在你的实现中，`loadUserByUsername` 方法通过调用 `findUserByName` 方法从数据库中获取用户信息。
2. **`UserDetails` 接口：**
   - `UserService` 中的 `findUserByName` 方法返回了一个 `User` 对象，而 `User` 类实现了 Spring Security 的 `UserDetails` 接口。
   - `UserDetails` 接口包含了描述用户详细信息的方法，例如判断账号是否过期、账号是否被锁定、密码是否过期等。
   - 通过实现 `UserDetails` 接口，你提供了一个将用户信息整合到 Spring Security 框架中的方式。
3. **联系：**
   - 当用户尝试进行身份验证时，Spring Security 将使用 `loadUserByUsername` 方法加载用户信息。
   - 加载的用户信息会被封装成一个 `Authentication` 对象，该对象用于表示用户的身份和凭证信息。
   - Spring Security 将通过这个 `Authentication` 对象来完成身份验证和授权的工作。

总体而言，通过将 `UserService` 实现 `UserDetailsService` 接口，你可以将自定义的用户信息加载逻辑集成到 Spring Security 中，从而实现对用户身份验证和授权的控制。

**4.配置类，实现需求。认证、授权 **

**配置静态资源**

```
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    private UserService userService;

    @Override
    public void configure(WebSecurity web) throws Exception {
        // 忽略静态资源的访问
        web.ignoring().antMatchers("/resources/**");
    }
 }
```

这段代码是 Spring Security 的配置类，它继承了 `WebSecurityConfigurerAdapter`，主要用于配置 Spring Security 的行为。具体来说：

1. **`configure(WebSecurity web)` 方法：**
   - 这个方法用于配置 `WebSecurity`，其中 `WebSecurity` 是一个链式的安全过滤器，用于配置静态资源的访问。
   - `web.ignoring().antMatchers("/resources/**")` 表示忽略对 "/resources/" 下所有静态资源的访问限制，这样用户可以自由访问这些静态资源而无需进行身份验证。
2. **作用：**
   - 配置静态资源的访问，以确保这些资源可以直接被用户访问而无需经过 Spring Security 的身份验证和授权过程。
   - `/resources/**` 是一个通配符，表示匹配所有 "/resources/" 下的路径，通常用于存放静态资源，如样式表、JavaScript 文件、图片等。

总体来说，这段代码的作用是确保静态资源能够被用户自由访问，而不受 Spring Security 的保护。

**认证，支持用户使用账户密码登录方式**

```java
// AuthenticationManager: 认证的核心接口.
// AuthenticationManagerBuilder: 用于构建AuthenticationManager对象的工具.
// ProviderManager: AuthenticationManager接口的默认实现类.
@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
    // 内置的认证规则
    // auth.userDetailsService(userService).passwordEncoder(new Pbkdf2PasswordEncoder("12345"));

    // 自定义认证规则
    // AuthenticationProvider: ProviderManager持有一组AuthenticationProvider,每个AuthenticationProvider负责一种认证.
    // 委托模式: ProviderManager将认证委托给AuthenticationProvider.
    auth.authenticationProvider(new AuthenticationProvider() {
        // Authentication: 用于封装认证信息的接口,不同的实现类代表不同类型的认证信息.
        @Override
        public Authentication authenticate(Authentication authentication) throws AuthenticationException {
            String username = authentication.getName();
            String password = (String) authentication.getCredentials();

            User user = userService.findUserByName(username);
            if (user == null) {
                throw new UsernameNotFoundException("账号不存在!");
            }

            password = CommunityUtil.md5(password + user.getSalt());
            if (!user.getPassword().equals(password)) {
                throw new BadCredentialsException("密码不正确!");
            }

            // principal: 主要信息; credentials: 证书; authorities: 权限;
            return new UsernamePasswordAuthenticationToken(user, user.getPassword(), user.getAuthorities());
        }

        // 当前的AuthenticationProvider支持哪种类型的认证.
        @Override
        public boolean supports(Class<?> aClass) {
            // UsernamePasswordAuthenticationToken: Authentication接口的常用的实现类.
            return UsernamePasswordAuthenticationToken.class.equals(aClass);
        }
    });
}
```


这段代码是 Spring Security 中配置认证规则的一部分，主要是通过重写 `configure(AuthenticationManagerBuilder auth)` 方法来定义认证规则。

1. `AuthenticationManagerBuilder` 是用于构建 `AuthenticationManager` 对象的工具。
2. `AuthenticationManager` 是 Spring Security 中负责认证的核心接口。
3. `AuthenticationProvider` 是 `AuthenticationManager` 接口的实现类，用于实际执行认证的逻辑。`ProviderManager` 持有一组 `AuthenticationProvider`，每个 `AuthenticationProvider` 负责一种认证方式。在这段代码中，通过实现 `AuthenticationProvider` 接口，自定义了一个认证规则。

具体解释代码的逻辑：

- `authenticate` 方法：该方法负责执行认证的逻辑。首先，获取用户名和密码，然后根据用户名从数据库中获取用户信息。接着，将输入的密码加盐并进行 MD5 加密，与数据库中存储的密码比对。如果密码匹配，则认证成功，创建一个 `UsernamePasswordAuthenticationToken` 对象，其中包含用户信息、密码和权限信息。
- `supports` 方法：判断当前的 `AuthenticationProvider` 是否支持某种认证类型。在这里，支持 `UsernamePasswordAuthenticationToken` 类型的认证。

总体而言，这段代码通过自定义的 `AuthenticationProvider` 实现了一种基于用户名密码的认证方式，并将其配置到 `AuthenticationManagerBuilder` 中。在实际的认证过程中，Spring Security 会根据配置的 `AuthenticationProvider` 来选择适当的认证方式。



**登录相关的配置，用于配置应用程序的安全性**

```java
@Override
    protected void configure(HttpSecurity http) throws Exception {
        // 登录相关配置
        http.formLogin()
                .loginPage("/loginpage")
                .loginProcessingUrl("/login")
                .successHandler(new AuthenticationSuccessHandler() {
                    @Override
                    public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws IOException, ServletException {
                        response.sendRedirect(request.getContextPath() + "/index");
                    }
                })
                .failureHandler(new AuthenticationFailureHandler() {
                    @Override
                    public void onAuthenticationFailure(HttpServletRequest request, HttpServletResponse response, AuthenticationException e) throws IOException, ServletException {
                        request.setAttribute("error", e.getMessage());
                        request.getRequestDispatcher("/loginpage").forward(request, response);
                    }
                });

        // 退出相关配置
        http.logout()
                .logoutUrl("/logout")
                .logoutSuccessHandler(new LogoutSuccessHandler() {
                    @Override
                    public void onLogoutSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws IOException, ServletException {
                        response.sendRedirect(request.getContextPath() + "/index");
                    }
                });

        // 授权配置
        http.authorizeRequests()
                .antMatchers("/letter").hasAnyAuthority("USER", "ADMIN")
                .antMatchers("/admin").hasAnyAuthority("ADMIN")
                .and().exceptionHandling().accessDeniedPage("/denied");

        // 增加Filter,处理验证码
        http.addFilterBefore(new Filter() {
            @Override
            public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
                HttpServletRequest request = (HttpServletRequest) servletRequest;
                HttpServletResponse response = (HttpServletResponse) servletResponse;
                if (request.getServletPath().equals("/login")) {
                    String verifyCode = request.getParameter("verifyCode");
                    if (verifyCode == null || !verifyCode.equalsIgnoreCase("1234")) {
                        request.setAttribute("error", "验证码错误!");
                        request.getRequestDispatcher("/loginpage").forward(request, response);
                        return;
                    }
                }
                // 让请求继续向下执行.
                filterChain.doFilter(request, response);
            }
        }, UsernamePasswordAuthenticationFilter.class);

        // 记住我
        http.rememberMe()
                .tokenRepository(new InMemoryTokenRepositoryImpl())
                .tokenValiditySeconds(3600 * 24)
                .userDetailsService(userService);

    }
```


这段代码是 Spring Security 中通过 Java 配置实现的一个 `HttpSecurity` 的配置类，用于配置应用程序的安全性。以下是主要配置项的解释：

1. **登录相关配置 (`http.formLogin()`)：**
   - `.loginPage("/loginpage")`: 指定登录页面的路径。
   - `.loginProcessingUrl("/login")`: 指定处理登录请求的路径。
   - `.successHandler()`: 登录成功的处理器，指定登录成功后的操作。
   - `.failureHandler()`: 登录失败的处理器，指定登录失败后的操作。
2. **退出相关配置 (`http.logout()`)：**
   - `.logoutUrl("/logout")`: 指定退出登录的路径。
   - `.logoutSuccessHandler()`: 退出成功的处理器，指定退出成功后的操作。
3. **授权配置 (`http.authorizeRequests()`)：**
   - `.antMatchers("/letter").hasAnyAuthority("USER", "ADMIN")`: 指定路径 "/letter" 需要具备 "USER" 或 "ADMIN" 权限才能访问。
   - `.antMatchers("/admin").hasAnyAuthority("ADMIN")`: 指定路径 "/admin" 需要具备 "ADMIN" 权限才能访问。
   - `.and().exceptionHandling().accessDeniedPage("/denied")`: 配置异常处理，当访问被拒绝时，跳转到 "/denied" 页面。
4. **增加过滤器 (`http.addFilterBefore()`)：**
   - 通过 `UsernamePasswordAuthenticationFilter.class` 之前添加一个自定义的过滤器。该过滤器用于处理验证码的逻辑。当请求路径是 "/login" 时，检查验证码是否正确，不正确则跳转到 "/loginpage"。
5. **记住我功能 (`http.rememberMe()`)：**
   - `.tokenRepository(new InMemoryTokenRepositoryImpl())`: 使用内存存储令牌的方式。在实际应用中，可以使用数据库或其他持久存储方式。
   - `.tokenValiditySeconds(3600 * 24)`: 设置令牌的有效时长，这里设置为 24 小时。
   - `.userDetailsService(userService)`: 指定自定义的 `userDetailsService` 来加载用户信息。

总体而言，这段配置代码定义了登录、退出、授权、验证码处理、记住我等功能的行为。通过配置这些行为，可以实现对应用程序的安全性进行管理和控制。



### 7.10优化网站功能-caffeine

![image-20231203165434135](E:\Typroa_Picture\image-20231203165434135.png)



1.本地缓存的速度比Redis更快。因为Redis是分布式的，有网络上的时间开销。

2.查找到Mysql数据后，会把数据进行缓存。

3.缓存是有空间的，当达到某一个量的时候就会淘汰数据，或者达到某一个时间也会淘汰一些数据。

4.缓存不适用与常跟新的东西。



**使用caffeine**

缓存热门帖子，缓存热门帖子的数量。因为需要经常查询

`1.导入依赖`

```xml
		<dependency>
			<groupId>com.github.ben-manes.caffeine</groupId>
			<artifactId>caffeine</artifactId>
			<version>2.7.0</version>
		</dependency>
```

`2.配置文件`

```properties
# caffeine，最大缓存数量以及存活时间
caffeine.posts.max-size=15
caffeine.posts.expire-seconds=180

```

`3.优化DiscusService`

```java
    //热门帖子列表缓存 key:Value
    private LoadingCache<String,List<DiscussPost>> postListCache;
    //热门帖子总数缓存 key:Value
    private LoadingCache<Integer,Integer> postRowsCache;
    //使用caffine
    @PostConstruct
    public void init() {
        // 初始化帖子列表缓存,PostList保存了Key和Value
        postListCache = Caffeine.newBuilder()
                .maximumSize(maxSize)
                .expireAfterWrite(expireSeconds, TimeUnit.SECONDS)
                //String key ,Vaule :List
                .build(new CacheLoader<String, List<DiscussPost>>() {//如果缓存中没有该数据，会从数据库中加载。并且把数据存放在postListCache中
                    @Nullable
                    @Override
                    public List<DiscussPost> load(@NonNull String key) throws Exception {
                        if (key == null || key.length() == 0) {
                            throw new IllegalArgumentException("参数错误!");
                        }
                        
                        String[] params = key.split(":");
                        if (params == null || params.length != 2) {
                            throw new IllegalArgumentException("参数错误!");
                        }
                        
                        int offset = Integer.valueOf(params[0]);
                        int limit = Integer.valueOf(params[1]);
                        
                        // 二级缓存: Redis -> mysql
                        //查询出来的数据会保存在postListCache中
                        logger.debug("load post list from DB.");
                        return discussPostMapper.selectDiscussPosts(0, offset, limit, 1);
                    }
                });
        // 初始化帖子总数缓存
        postRowsCache = Caffeine.newBuilder()
                .maximumSize(maxSize)
                .expireAfterWrite(expireSeconds, TimeUnit.SECONDS)
                .build(new CacheLoader<Integer, Integer>() {
                    @Nullable
                    @Override
                    public Integer load(@NonNull Integer key) throws Exception {
                        logger.debug("load post rows from DB.");
                        return discussPostMapper.selectDiscussPostRows(key);
                    }
                });
    }
```

![image-20231203223649477](E:\Typroa_Picture\image-20231203223649477.png)

### 7.2权限控制

![image-20231204093710750](E:\Typroa_Picture\image-20231204093710750.png)

1.导入依赖

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

2.废弃拦截器,因为我们使用Spring Security

![image-20231204111807017](E:\Typroa_Picture\image-20231204111807017.png)

3.增加角色在CommunityConstant

```java
    /**
     * 权限: 普通用户
     */
    String AUTHORITY_USER = "user";

    /**
     * 权限: 管理员
     */
    String AUTHORITY_ADMIN = "admin";

    /**
     * 权限: 版主
     */
    String AUTHORITY_MODERATOR = "moderator";

```

4.配置SecurityConfig

```java
public class SecurityConfig extends WebSecurityConfigurerAdapter implements CommunityConstant {
    
    
    //对静态资源不限制访问
    @Override
    public void configure(WebSecurity web) throws Exception {
        web.ignoring().antMatchers("/resources/**");
    }
    
    //对http哪些资源，的哪些用户可以访问。
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //授权
        http.authorizeRequests()//启用请求授权配置。
                .antMatchers(//配置需要特定权限的路径。
                        "/user/setting",
                        "/user/upload",
                        "/discuss/add",
                        "/comment/add/**",
                        "/letter/**",
                        "/notice/**",
                        "/like",
                        "/follow",
                        "/unfollow"
                ).hasAnyAuthority(//hasAnyAuthority(...): 指定需要的权限，即上述路径需要具备的权限。
                        AUTHORITY_USER,
                        AUTHORITY_MODERATOR,
                        AUTHORITY_ADMIN
                ).anyRequest().permitAll()//.anyRequest().permitAll(): 允许其他请求（不在 antMatchers 中的请求）访问，不需要特定权限。
                .and().csrf().disable();//.and().csrf().disable(): 禁用 CSRF（Cross-Site Request Forgery）防护，一般在前后端分离的应用中会禁用 CSRF 防护，以简化开发。
    
        //当权限不够的时候怎么处理:启用异常处理配置。
        http.exceptionHandling()
                .authenticationEntryPoint(new AuthenticationEntryPoint() {//配置处理身份验证异常的入口点，即在用户没有登录时执行的操作。
                    //在用户没有登录时执行的操作。
                    @Override
                    public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException e) throws IOException, ServletException {
                        //我们需要返回两种，一种如果是Ajax 请求，一种是返回http请求
                        //如果是 Ajax 请求，返回 JSON 格式的错误信息；如果是普通请求，重定向到登录页面。
                        String xRequsetedWith=request.getHeader("x-requested-with");
                        if ("XMLHttpRequest".equals(xRequsetedWith)) {
                            response.setContentType("application/plain;charset=utf-8");
                            PrintWriter writer = response.getWriter();
                            writer.write(CommunityUtil.getJSONString(403, "你还没有登录哦!"));
                        } else {
                            response.sendRedirect(request.getContextPath() + "/login");
                        }
                    }
                })
                .accessDeniedHandler(new AccessDeniedHandler() {
                    //权限不足的时候
                    @Override
                    public void handle(HttpServletRequest request, HttpServletResponse response, AccessDeniedException e) throws IOException, ServletException {
                        String xRequsetedWith=request.getHeader("x-requested-with");
                        if ("XMLHttpRequest".equals(xRequsetedWith)) {
                            response.setContentType("application/plain;charset=utf-8");
                            PrintWriter writer = response.getWriter();
                            writer.write(CommunityUtil.getJSONString(403, "你还没有权限哦!"));
                        } else {
                            response.sendRedirect(request.getContextPath() + "/denied");
                        }
                    }
                });
        // Security底层默认会拦截/logout请求,进行退出处理.
        // 覆盖它默认的逻辑,才能执行我们自己的退出代码.让Security拦截一个没有的页面，所以退出登录会执行自己写的逻辑代码
        http.logout().logoutUrl("/securitylogout");
    
    }    
}
```

5.在Userservice中构造一个获得权限的方法。

```java
    public Collection<? extends GrantedAuthority> getAuthorities(int userId) {
        User user = this.findUserById(userId);

        List<GrantedAuthority> list = new ArrayList<>();
        list.add(new GrantedAuthority() {

            @Override
            public String getAuthority() {
                switch (user.getType()) {
                    case 1:
                        return AUTHORITY_ADMIN;
                    case 2:
                        return AUTHORITY_MODERATOR;
                    default:
                        return AUTHORITY_USER;
                }
            }
        });
        return list;
    }
```

这方法是一个自定义的获取用户权限的方法。在Spring Security中，权限被表示为`GrantedAuthority`对象的集合。这个方法返回一个`Collection<? extends GrantedAuthority>`，即表示该用户拥有的权限集合。

具体而言，这个方法根据`userId`查找对应的用户，然后根据用户的类型（在这里通过`user.getType()`获取）来决定用户拥有的权限。在这里，权限被简化为字符串，分为三种类型：`AUTHORITY_ADMIN`、`AUTHORITY_MODERATOR`、`AUTHORITY_USER`。这是一种简单的方式，通常实际项目中权限可能更加复杂，可能会对资源和操作进行更详细的划分。





6.在LoginTicketinteceptor里面构建用户权限结果,当用户登录获得ticket时候，那么就要把权限赋值给User。

![image-20231204121309280](E:\Typroa_Picture\image-20231204121309280.png)

```
UsernamePasswordAuthenticationToken是Authentication接口的一个实现类，表示基于用户名和密码的身份验证令牌。在这里，使用用户对象、密码和用户的权限信息创建了一个UsernamePasswordAuthenticationToken实例。

SecurityContextHolder是Spring Security中用于持有当前认证信息的上下文对象。通过SecurityContextHolder.getContext()获取当前的上下文。

SecurityContextImpl是SecurityContext接口的一个具体实现类，表示了当前安全上下文。这里使用SecurityContextImpl的构造函数传入一个Authentication对象创建了一个新的安全上下文。

最后，通过SecurityContextHolder.setContext(...)将新创建的安全上下文设置为当前的安全上下文。这样，Spring Security就知道当前用户已经通过认证，并且可以在后续的请求中使用该信息进行授权判断。
```



7.在拦截器里，推出时候清理SecurityContextholder; LonginController中退出登录也需要清理用户权限。

![image-20231204121432642](E:\Typroa_Picture\image-20231204121432642.png)



 8.MessageContoller更改一下逻辑，让Map非空的时候才装进去

![image-20231204103356456](E:\Typroa_Picture\image-20231204103356456.png)

 

```java
package com.nowcoder.community.config;

import com.nowcoder.community.util.CommunityConstant;
import com.nowcoder.community.util.CommunityUtil;
import org.springframework.security.access.AccessDeniedException;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.builders.WebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.AuthenticationEntryPoint;
import org.springframework.security.web.access.AccessDeniedHandler;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter implements CommunityConstant {
    
    
    //对静态资源不限制访问
    @Override
    public void configure(WebSecurity web) throws Exception {
        web.ignoring().antMatchers("/resources/**");
    }
    
    //对http哪些资源，的哪些用户可以访问。
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //授权
        /*http.authorizeRequests()//启用请求授权配置。
                .antMatchers(//配置需要特定权限的路径。
                        "/user/setting",
                        "/user/upload",
                        "/discuss/add",
                        "/comment/add/**",
                        "/letter/**",
                        "/notice/**",
                        "/like",
                        "/follow",
                        "/unfollow"
                ).hasAnyAuthority(//hasAnyAuthority(...): 指定需要的权限，即上述路径需要具备的权限。
                        AUTHORITY_USER,
                        AUTHORITY_MODERATOR,
                        AUTHORITY_ADMIN
                ).anyRequest().permitAll()//.anyRequest().permitAll(): 允许其他请求（不在 antMatchers 中的请求）访问，不需要特定权限。
                .and().csrf().disable();//.and().csrf().disable(): 禁用 CSRF（Cross-Site Request Forgery）防护，一般在前后端分离的应用中会禁用 CSRF 防护，以简化开发。
        */
        // 授权
        http.authorizeRequests()
                .antMatchers(
                        "/user/setting",
                        "/user/upload",
                        "/discuss/add",
                        "/comment/add/**",
                        "/letter/**",
                        "/notice/**",
                        "/like",
                        "/follow",
                        "/unfollow"
                )
                .hasAnyAuthority(
                        AUTHORITY_USER,
                        AUTHORITY_ADMIN,
                        AUTHORITY_MODERATOR
                )
                .anyRequest().permitAll()
                .and().csrf().disable();
        //当权限不够的时候怎么处理:启用异常处理配置。
        http.exceptionHandling()
                .authenticationEntryPoint(new AuthenticationEntryPoint() {//配置处理身份验证异常的入口点，即在用户没有登录时执行的操作。
                    //在用户没有登录时执行的操作。
                    @Override
                    public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException e) throws IOException, ServletException {
                        //我们需要返回两种，一种如果是Ajax 请求，一种是返回http请求
                        //如果是 Ajax 请求，返回 JSON 格式的错误信息；如果是普通请求，重定向到登录页面。
                        String xRequsetedWith=request.getHeader("x-requested-with");
                        if ("XMLHttpRequest".equals(xRequsetedWith)) {
                            response.setContentType("application/plain;charset=utf-8");
                            PrintWriter writer = response.getWriter();
                            writer.write(CommunityUtil.getJSONString(403, "你还没有登录哦!"));
                        } else {
                            response.sendRedirect(request.getContextPath() + "/login");
                        }
                    }
                })
                .accessDeniedHandler(new AccessDeniedHandler() {
                    //权限不足的时候
                    @Override
                    public void handle(HttpServletRequest request, HttpServletResponse response, AccessDeniedException e) throws IOException, ServletException {
                        String xRequsetedWith=request.getHeader("x-requested-with");
                        if ("XMLHttpRequest".equals(xRequsetedWith)) {
                            response.setContentType("application/plain;charset=utf-8");
                            PrintWriter writer = response.getWriter();
                            writer.write(CommunityUtil.getJSONString(403, "你还没有权限哦!"));
                        } else {
                            response.sendRedirect(request.getContextPath() + "/denied");
                        }
                    }
                });
        // Security底层默认会拦截/logout请求,进行退出处理.
        // 覆盖它默认的逻辑,才能执行我们自己的退出代码.让Security拦截一个没有的页面，所以退出登录会执行自己写的逻辑代码
        http.logout().logoutUrl("/securitylogout");
    
    }
    
    
    
}

```

### 7.3置顶，加精、删除

![image-20231204164011726](E:\Typroa_Picture\image-20231204164011726.png)



1.因为Thymeleaf需要拿到用户角色的权限，所以需要导入一个依赖。

```xml
		<dependency>
			<groupId>org.thymeleaf.extras</groupId>
			<artifactId>thymeleaf-extras-springsecurity5</artifactId>
		</dependency>
```

2.增加DiscussPostMapper方法

```
    int updateType(int id, int type);

    int updateStatus(int id, int status);

   
   <update id="updateType">
        update discuss_post set type = #{type} where id = #{id}
    </update>

    <update id="updateStatus">
        update discuss_post set status = #{status} where id = #{id}
    </update>
```

3.增加DiscussPostService方法

```java
    public int updateType(int id, int type) {
        return discussPostMapper.updateType(id, type);
    }

    public int updateStatus(int id, int status) {
        return discussPostMapper.updateStatus(id, status);
    }
```

4.增加DiscussContoller方法

```java
 //置顶方法：
    @RequestMapping(path = "/top",method = RequestMethod.POST)
    @ResponseBody
    public String setTop(int id){
        discussPostService.updateType(id,1);
        //更改Elasticsearch的缓存
    
        // 触发发帖事件
        Event event = new Event()
                .setTopic(TOPIC_PUBLISH)
                .setUserId(hostHolder.getUser().getId())
                .setEntityType(ENTITY_TYPE_POST)
                .setEntityId(id);
        eventProducer.fireEvent(event);
        
        return CommunityUtil.getJSONString(0);
    }
    
    //加精方法
    @RequestMapping(path = "/wonderful", method = RequestMethod.POST)
    @ResponseBody
    public String setWonderful(int id) {
        discussPostService.updateStatus(id, 1);
    
        // 触发发帖事件，发帖会覆盖原有的缓存
        Event event = new Event()
                .setTopic(TOPIC_PUBLISH)
                .setUserId(hostHolder.getUser().getId())
                .setEntityType(ENTITY_TYPE_POST)
                .setEntityId(id);
        eventProducer.fireEvent(event);
    
        return CommunityUtil.getJSONString(0);
    }
    
    //删除
    @RequestMapping(path = "/dalete",method = RequestMethod.POST)
    @ResponseBody
    public String setDelete(int id){
        discussPostService.updateStatus(id, 2);
    
        // 触发删帖事件
        Event event = new Event()
                .setTopic(TOPIC_DELETE)
                .setUserId(hostHolder.getUser().getId())
                .setEntityType(ENTITY_TYPE_POST)
                .setEntityId(id);
        eventProducer.fireEvent(event);
        
        return CommunityUtil.getJSONString(0);
    }
```

5.删除贴的消费者还需要增加一些功能。

```java
    //对删除帖子进行监听
    @KafkaListener(topics = {TOPIC_DELETE})
    public void handleDeleteMessage(ConsumerRecord record){
        if(record==null||record.value()==null){
            logger.error("消息内容为空");
            return;
        }
        Event event=JSONObject.parseObject(record.value().toString(),Event.class);
        if (event == null) {
            logger.error("消息格式错误!");
            return;
        }
        elasticsearchService.deleteDiscussPost(event.getEntityId());
    }
```

6.在Spring Security 配置增加方法

7.discuss-detali.html与js功能都要修改。 

8.配置权限

9.Thymeleaf控制角色不同，显示不同的按钮。



### 7.4Redis高级数据类型

![image-20231204191927020](E:\Typroa_Picture\image-20231204191927020.png) 

**HyperLogLog使用方法**

1.添加数据

```java
    // 统计20万个重复数据的独立总数.包含去重操作的
    @Test
    public void testHyperLogLog() {
        String redisKey = "test:hll:01";
        
        for (int i = 1; i <= 100000; i++) {
            redisTemplate.opsForHyperLogLog().add(redisKey, i);
        }
        
        for (int i = 1; i <= 100000; i++) {
            int r = (int) (Math.random() * 100000 + 1);
            redisTemplate.opsForHyperLogLog().add(redisKey, r);
        }
        
        long size = redisTemplate.opsForHyperLogLog().size(redisKey);
        System.out.println(size);
    }
```

2.数据合并

```java
// 将3组数据合并, 再统计合并后的重复数据的独立总数.
    @Test
    public void testHyperLogLogUnion() {
        String redisKey2 = "test:hll:02";
        for (int i = 1; i <= 10000; i++) {
            redisTemplate.opsForHyperLogLog().add(redisKey2, i);
        }
        
        String redisKey3 = "test:hll:03";
        for (int i = 5001; i <= 15000; i++) {
            redisTemplate.opsForHyperLogLog().add(redisKey3, i);
        }
        
        String redisKey4 = "test:hll:04";
        for (int i = 10001; i <= 20000; i++) {
            redisTemplate.opsForHyperLogLog().add(redisKey4, i);
        }
        
        String unionKey = "test:hll:union";
        redisTemplate.opsForHyperLogLog().union(unionKey, redisKey2, redisKey3, redisKey4);
        
        long size = redisTemplate.opsForHyperLogLog().size(unionKey);
        System.out.println(size);
    }
```

**BitMap的使用，BitMap其实也是String类型的，所以opsForValue**

BitMap的统计，

```java
 @Test
    public void testBitMap() {
        String redisKey = "test:bm:01";

        // 记录(Key,索引，boolean)
        redisTemplate.opsForValue().setBit(redisKey, 1, true);
        redisTemplate.opsForValue().setBit(redisKey, 4, true);
        redisTemplate.opsForValue().setBit(redisKey, 7, true);

        // 查询
        System.out.println(redisTemplate.opsForValue().getBit(redisKey, 0));
        System.out.println(redisTemplate.opsForValue().getBit(redisKey, 1));
        System.out.println(redisTemplate.opsForValue().getBit(redisKey, 2));

        // 统计
        //这里需要用到底层的连接
        Object obj = redisTemplate.execute(new RedisCallback() {
            @Override
            public Object doInRedis(RedisConnection connection) throws DataAccessException {
                return connection.bitCount(redisKey.getBytes());
            }
        });

        System.out.println(obj);
    }
```

把不同BitMap进行“与” “或”运算

```java
// 统计3组数据的布尔值, 并对这3组数据做OR运算.
    @Test
    public void testBitMapOperation() {
        String redisKey2 = "test:bm:02";
        redisTemplate.opsForValue().setBit(redisKey2, 0, true);
        redisTemplate.opsForValue().setBit(redisKey2, 1, true);
        redisTemplate.opsForValue().setBit(redisKey2, 2, true);

        String redisKey3 = "test:bm:03";
        redisTemplate.opsForValue().setBit(redisKey3, 2, true);
        redisTemplate.opsForValue().setBit(redisKey3, 3, true);
        redisTemplate.opsForValue().setBit(redisKey3, 4, true);

        String redisKey4 = "test:bm:04";
        redisTemplate.opsForValue().setBit(redisKey4, 4, true);
        redisTemplate.opsForValue().setBit(redisKey4, 5, true);
        redisTemplate.opsForValue().setBit(redisKey4, 6, true);

        String redisKey = "test:bm:or";
        Object obj = redisTemplate.execute(new RedisCallback() {
            @Override
            public Object doInRedis(RedisConnection connection) throws DataAccessException {
                connection.bitOp(RedisStringCommands.BitOperation.OR,
                        redisKey.getBytes(), redisKey2.getBytes(), redisKey3.getBytes(), redisKey4.getBytes());
                return connection.bitCount(redisKey.getBytes());
            }
        });

        System.out.println(obj);
```

### 7.5网站数据统计 

![image-20231204204202764](E:\Typroa_Picture\image-20231204204202764.png)

1.增加Redis的Key

```java
    // 单日UV
    public static String getUVKey(String date) {
        return PREFIX_UV + SPLIT + date;
    }
    
    // 区间UV
    public static String getUVKey(String startDate, String endDate) {
        return PREFIX_UV + SPLIT + startDate + SPLIT + endDate;
    }
    
    // 单日活跃用户
    public static String getDAUKey(String date) {
        return PREFIX_DAU + SPLIT + date;
    }
    
    // 区间活跃用户
    public static String getDAUKey(String startDate, String endDate) {
        return PREFIX_DAU + SPLIT + startDate + SPLIT + endDate;
    }
```

2.创建Service

```java
@Service
public class DataService {
    
    @Autowired
    private  RedisTemplate redisTemplate;
    //定义数据格式
    @Autowired
    private SimpleDateFormat df=new SimpleDateFormat("yyyyMMdd");
    
    // 将指定的IP计入UV
    public void recordUV(String ip){
        //将当前日期转成字符串，形成一个Key
        String redisKey = RedisKeyUtil.getUVKey(df.format(new Date()));
        redisTemplate.opsForHyperLogLog().add(redisKey, ip);
    }
    //统计置顶日期范围内的UV
    public long calculateUV(Date start,Date end){
        
        if (start == null || end == null) {
            throw new IllegalArgumentException("参数不能为空!");
        }
        /*这段代码创建了一个Calendar对象，
        并通过Calendar.getInstance()方法获取了当前日期和时间的Calendar实例。
        Calendar是Java中用于处理日期和时间的类，它允许你执行各种日期和时间操作，
        如获取年、月、日、时、分、秒等信息，还可以进行日期和时间的计算和比较。
        int year = calendar.get(Calendar.YEAR);
        int month = calendar.get(Calendar.MONTH); // 月份从0开始，所以需要+1
        int dayOfMonth = calendar.get(Calendar.DAY_OF_MONTH);
        int hour = calendar.get(Calendar.HOUR_OF_DAY);
        int minute = calendar.get(Calendar.MINUTE);
        int second = calendar.get(Calendar.SECOND);
        * */
        List<String> keyList = new ArrayList<>();
        Calendar calendar = Calendar.getInstance();
        calendar.setTime(start);
        //遍历区间内的每一天，获取Key
        while (!calendar.getTime().after(end)) {
            String key = RedisKeyUtil.getUVKey(df.format(calendar.getTime()));
            keyList.add(key);
            calendar.add(Calendar.DATE, 1);
        }
        //合并区间内的数据
        String redisKey = RedisKeyUtil.getUVKey(df.format(start), df.format(end));
        //多个数据最后统计在redisKey中
        redisTemplate.opsForHyperLogLog().union(redisKey, keyList.toArray());
        return redisTemplate.opsForHyperLogLog().size(redisKey);
    }
    
    //将指定用户计入DAU
    public void recordDAU(int userId) {
        String redisKey = RedisKeyUtil.getDAUKey(df.format(new Date()));
        redisTemplate.opsForValue().setBit(redisKey, userId, true);
    }
    
    //统计指定日期范围内的DAU
    public long calculateDAU(Date start,Date end){
        
        if (start == null || end == null) {
            throw new IllegalArgumentException("参数不能为空!");
        }
        //DAU的key是用byte保存的
        List<byte[]> keyList = new ArrayList<>();
        Calendar calendar = Calendar.getInstance();
        calendar.setTime(start);
        
        //获取区间内的Key
        while (!calendar.getTime().after(end)) {
            String key = RedisKeyUtil.getDAUKey(df.format(calendar.getTime()));
            keyList.add(key.getBytes());
            calendar.add(Calendar.DATE, 1);
        }
        
        //合并，用or计算出
        return (long) redisTemplate.execute(new RedisCallback() {
            @Override
            public Object doInRedis(RedisConnection connection) throws DataAccessException {
                String redisKey = RedisKeyUtil.getDAUKey(df.format(start), df.format(end));
                connection.bitOp(RedisStringCommands.BitOperation.OR,
                        redisKey.getBytes(), keyList.toArray(new byte[0][0]));
                return connection.bitCount(redisKey.getBytes());
            }
        });
        
    }
```

3.写拦截器，每次访问都截取。

```java
@Component
public class DataInterceptor implements HandlerInterceptor {
    @Autowired
    HostHolder hostHolder;
    @Autowired
    DataService dataService;
    
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //获取当前ip
        String ip =request.getRemoteHost();
        //统计UV
        dataService.recordUV(ip);
        //统计DAU
        User user= new User();
        dataService.recordDAU(user.getId());
        return true;
    }
}

```

4.写一个Controller，展现数据

```java
import java.util.Date;

@Controller
public class DataController {
    @Autowired
    DataService dataService;
    
    //跳转统计页面
    @RequestMapping(path = "/data", method = {RequestMethod.GET, RequestMethod.POST})
    public String getDataPage() {
        return "/site/admin/data";
    }
    
    //统计UV
    @RequestMapping(path = "/data/uv",method = RequestMethod.POST)
    //获取页面传进来的日期数据
    public String getUV(@DateTimeFormat(pattern = "yyyy-MM-dd") Date start,
                        @DateTimeFormat(pattern = "yyyy-MM-dd") Date end, Model model) {
        long uv = dataService.calculateUV(start, end);
        model.addAttribute("uvResult", uv);
        model.addAttribute("uvStartDate", start);
        model.addAttribute("uvEndDate", end);
        return "forward:/data";
    }
    
    //统计DAU
    @RequestMapping(path = "/data/dau", method = RequestMethod.POST)
    public String getDAU(@DateTimeFormat(pattern = "yyyy-MM-dd") Date start,
                         @DateTimeFormat(pattern = "yyyy-MM-dd") Date end, Model model) {
        long dau = dataService.calculateDAU(start, end);
        model.addAttribute("dauResult", dau);
        model.addAttribute("dauStartDate", start);
        model.addAttribute("dauEndDate", end);
        return "forward:/data";
    }
}

```

5.权限控制

Spring Security 配置网页权限

 

### 7.6任务执行和调度--线程池

![image-20231205133631261](E:\Typroa_Picture\image-20231205133631261.png)

主要使用Spring Quartz，但是也需要了解一下JDK线程池，和Spring 线程池

1.JDK线程池

```java
//JDK的普通线程池
    private ExecutorService executorService= Executors.newFixedThreadPool(5);
    //JDK可执行定时任务的线程池
    private ScheduledExecutorService scheduledExecutorService=Executors.newScheduledThreadPool(5);

    @Test
    public void testExecutor(){
        //实现一个任务
        Runnable task=new Runnable() {
            @Override
            public void run() {
                logger.debug("Hello ExecutorService");
            }
        };
        for (int i = 0; i < 10; i++) {
            executorService.submit(task);
        }
    }
    
    // 2.JDK定时任务线程池
    @Test
    public void testScheduledExecutorService() {
        Runnable task = new Runnable() {
            @Override
            public void run() {
                logger.debug("Hello ScheduledExecutorService");
            }
        };
        //推迟多久执行，每隔多久执行一次
        scheduledExecutorService.scheduleAtFixedRate(task, 10000, 1000, TimeUnit.MILLISECONDS);
    
        sleep(30000);
    }
```

2.Spring 线程池需要配置、properties ，并且要构造一个配置类。   

```properties
# TaskExecutionProperties
#核心数量，最大数量，等待队列
spring.task.execution.pool.core-size=5 
spring.task.execution.pool.max-size=15
spring.task.execution.pool.queue-capacity=100
```

![image-20231205150414563](E:\Typroa_Picture\image-20231205150414563.png)

```java
// 3.Spring普通线程池
    @Test
    public void testThreadPoolTaskExecutor() {
        Runnable task = new Runnable() {
            @Override
            public void run() {
                logger.debug("Hello ThreadPoolTaskExecutor");
            }
        };

        for (int i = 0; i < 10; i++) {
            taskExecutor.submit(task);
        }

        sleep(10000);
    }

    // 4.Spring定时任务线程池
    @Test
    public void testThreadPoolTaskScheduler() {
        Runnable task = new Runnable() {
            @Override
            public void run() {
                logger.debug("Hello ThreadPoolTaskScheduler");
            }
        };

        Date startTime = new Date(System.currentTimeMillis() + 10000);
        taskScheduler.scheduleAtFixedRate(task, startTime, 1000);

        sleep(30000);
    }

```

3.Spring线程池可以通过注解简化。增加一个Async就可以养execute1(),多线程下。@Scheduled是用来定时任务的。

![image-20231205152829672](E:\Typroa_Picture\image-20231205152829672.png)

```java
    // 5.Spring普通线程池(简化)
    @Test
    public void testThreadPoolTaskExecutorSimple() {
        for (int i = 0; i < 10; i++) {
            alphaService.execute1();
        }

        sleep(10000);
    }

    // 6.Spring定时任务线程池(简化)
    @Test
    public void testThreadPoolTaskSchedulerSimple() {
        sleep(30000);
    }

}
```

4.使用Quartz

1.导入依赖

```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-quartz</artifactId>
		</dependency>

```

2.Job是我们需要做的具体逻辑。job_details是配置job的范围，Trigger触发器配置我们什么时候启用，什么频率启动。 

​		1)实现Job接口，在Job接口里面实现我们的逻辑

```java
public class AlphaJob implements Job {
    @Override
    public void execute(JobExecutionContext context) throws JobExecutionException {
        System.out.println(Thread.currentThread().getName() + ": execute a quartz job.");
    }
}

```

​		2)配置Job_details和Trigger

```java
@Configuration
public class QuartzConfig {
    // FactoryBean可简化Bean的实例化过程:
    // 1.通过FactoryBean封装Bean的实例化过程.
    // 2.将FactoryBean装配到Spring容器里.
    // 3.将FactoryBean注入给其他的Bean.
    // 4.该Bean得到的是FactoryBean所管理的对象实例.
    
    
    //配置JobDetail
    @Bean
    public JobDetailFactoryBean alphaJobDetail(){
        JobDetailFactoryBean factoryBean=new JobDetailFactoryBean();
        //配置刚刚那个Job
        factoryBean.setJobClass(AlphaJob.class);
        factoryBean.setName("alphaJob");
        factoryBean.setGroup("alphaJobGroup");
        factoryBean.setDurability(true);
        factoryBean.setRequestsRecovery(true);
        return factoryBean;
    }
    
     @Bean
    public SimpleTriggerFactoryBean alphaTrigger(JobDetail alphaJobDetail) {
        SimpleTriggerFactoryBean factoryBean = new SimpleTriggerFactoryBean();
        factoryBean.setJobDetail(alphaJobDetail);
        factoryBean.setName("alphaTrigger");
        factoryBean.setGroup("alphaTriggerGroup");
        factoryBean.setRepeatInterval(3000);
        factoryBean.setJobDataMap(new JobDataMap());
        return factoryBean;
    }
    
}
```

这是使用Quartz Scheduler库配置定时任务的一部分。在Spring中，你可以使用`JobDetailFactoryBean`和`SimpleTriggerFactoryBean`来定义和配置Quartz中的任务（Job）和触发器（Trigger）。

1. **JobDetailFactoryBean:**
   - `setJobClass(AlphaJob.class)`: 设置要执行的具体任务（`AlphaJob`类）。
   - `setName("alphaJob")`: 为该Job定义一个名称。
   - `setGroup("alphaJobGroup")`: 为该Job定义一个分组。
   - `setDurability(true)`: 设置任务是否持久化。如果设置为true，即使没有触发器与之关联，任务也会一直存在直到被显式删除。
   - `setRequestsRecovery(true)`: 如果设置为true，在任务执行时，如果Scheduler出现故障（比如崩溃），则重新执行任务。
2. **SimpleTriggerFactoryBean:**
   - `setJobDetail(alphaJobDetail)`: 设置与该触发器关联的JobDetail。
   - `setName("alphaTrigger")`: 为该触发器定义一个名称。
   - `setGroup("alphaTriggerGroup")`: 为该触发器定义一个分组。
   - `setRepeatInterval(3000)`: 设置触发器的重复间隔，即多久触发一次任务，这里设置为3000毫秒（3秒）。
   - `setJobDataMap(new JobDataMap())`: 设置传递给Job的参数。在这里，没有设置具体的参数，你可以在Job中通过`JobExecutionContext`访问这些参数。



3.线程池默认线程等是固定的，我们可以配置properties修改配置。 Quartz的数据是默认存在内存的，需要配置才能存到数据库。

```properties
# QuartzProperties
spring.quartz.job-store-type=jdbc
spring.quartz.scheduler-name=communityScheduler
spring.quartz.properties.org.quartz.scheduler.instanceId=AUTO
spring.quartz.properties.org.quartz.jobStore.class=org.quartz.impl.jdbcjobstore.JobStoreTX
spring.quartz.properties.org.quartz.jobStore.driverDelegateClass=org.quartz.impl.jdbcjobstore.StdJDBCDelegate
spring.quartz.properties.org.quartz.jobStore.isClustered=true
spring.quartz.properties.org.quartz.threadPool.class=org.quartz.simpl.SimpleThreadPool
spring.quartz.properties.org.quartz.threadPool.threadCount=5
```

4.删除任务函数，需要调度器。在正常运行时候，调度器自动调度。

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@ContextConfiguration(classes = CommunityApplication.class)
public class QuartzTests {

    @Autowired
    private Scheduler scheduler;

    @Test
    public void testDeleteJob() {
        try {
            boolean result = scheduler.deleteJob(new JobKey("alphaJob", "alphaJobGroup"));
            System.out.println(result);
        } catch (SchedulerException e) {
            e.printStackTrace();
        }
    }

}
```

7.7帖子热度排行。

![image-20231205160140184](E:\Typroa_Picture\image-20231205160140184.png)

1.先把点赞，评论的帖子用Redis缓存起来。

2.时间一到，就更新帖子分数。



1.Redis增加帖子的Key

```java
// 帖子分数
public static String getPostScoreKey() {
    return PREFIX_POST + SPLIT + "score";
}
```

2.增加帖子的时候把Key仍进Redis中

3.加精时候把Key仍进Redis中

![](E:\Typroa_Picture\image-20231205171301562.png)

4.评论帖子时候Key仍进Redis中

5.点赞的时候评论帖子时候Key仍进Redis中

![image-20231205171551217](E:\Typroa_Picture\image-20231205171551217.png)

6.写一个Job，算是一个定时执行的线程任务。

```java
public class PostScoreRefreshJob implements Job, CommunityConstant {
    private static final Logger logger = LoggerFactory.getLogger(PostScoreRefreshJob.class);
    
    @Autowired
    private RedisTemplate redisTemplate;
    
    @Autowired
    private DiscussPostService discussPostService;
    
    @Autowired
    private LikeService likeService;
    
    @Autowired
    private ElasticsearchService elasticsearchService;
    
    // 牛客纪元
    private static final Date epoch;
    //这里自己定义一个时间，把时间转换为Date
    static {
        try {
            epoch = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").parse("2014-08-01 00:00:00");
        } catch (ParseException e) {
            throw new RuntimeException("初始化牛客纪元失败!", e);
        }
    }
    
    @Override
    public void execute(JobExecutionContext jobExecutionContext) throws JobExecutionException {
        String reidsKey= RedisKeyUtil.getPostScoreKey();
        BoundSetOperations operations = redisTemplate.boundSetOps(reidsKey);
    
    
        if (operations.size() == 0) {
            logger.info("[任务取消] 没有需要刷新的帖子!");
            return;
        }
        
        logger.info("[任务开始] 正在刷新帖子分数");
        //每计算一个帖子，就把数据给删除
        while (operations.size() > 0) {
            this.refresh((Integer) operations.pop());
        }
        logger.info("[任务结束] 帖子分数刷新完毕!");
    }
    
    //这里就是统一计算分数
    private void refresh(int postId) {
        DiscussPost post = discussPostService.findDiscussPostById(postId);
        
        if (post == null) {
            logger.error("该帖子不存在: id = " + postId);
            return;
        }
        
        // 是否精华
        boolean wonderful = post.getStatus() == 1;
        // 评论数量
        int commentCount = post.getCommentCount();
        // 点赞数量
        long likeCount = likeService.findEntityLikeCount(ENTITY_TYPE_POST, postId);
        
        // 计算权重
        double w = (wonderful ? 75 : 0) + commentCount * 10 + likeCount * 2;
        // 分数 = 帖子权重 + 距离天数
        double score = Math.log10(Math.max(w, 1))
                + (post.getCreateTime().getTime() - epoch.getTime()) / (1000 * 3600 * 24);
        // 更新帖子分数
        discussPostService.updateScore(postId, score);
        // 同步搜索数据
        post.setScore(score);
        elasticsearchService.saveDiscussPost(post);
    }
}

```

6.1配置Job_detail ,Trigger

```java
// 刷新帖子分数任务
    @Bean
    public JobDetailFactoryBean postScoreRefreshJobDetail() {
        JobDetailFactoryBean factoryBean = new JobDetailFactoryBean();
        factoryBean.setJobClass(PostScoreRefreshJob.class);
        factoryBean.setName("postScoreRefreshJob");
        factoryBean.setGroup("communityJobGroup");
        factoryBean.setDurability(true);
        factoryBean.setRequestsRecovery(true);
        return factoryBean;
    }

    @Bean
    public SimpleTriggerFactoryBean postScoreRefreshTrigger(JobDetail postScoreRefreshJobDetail) {
        SimpleTriggerFactoryBean factoryBean = new SimpleTriggerFactoryBean();
        factoryBean.setJobDetail(postScoreRefreshJobDetail);
        factoryBean.setName("postScoreRefreshTrigger");
        factoryBean.setGroup("communityTriggerGroup");
        factoryBean.setRepeatInterval(1000 * 60 * 5);
        factoryBean.setJobDataMap(new JobDataMap());
        return factoryBean;
    }
```

7.补充DiscussPostMapper，中添加一个跟新分数的方法。

8.配置JobDetail和Trigger



9.重构查询帖子算法，需要用Score排序

10.从新更改调用的函数





### 7.8生成长图

![image-20231205203002162](E:\Typroa_Picture\image-20231205203002162.png)



### 7.9将文件上传到云服务器

![image-20231205203626793](E:\Typroa_Picture\image-20231205203626793.png)

# 第八章：总结

### 8.1单元测试

![image-20231205213322021](E:\Typroa_Picture\image-20231205213322021.png)

1. **@Before:**

   - **用途：** 用于标注在测试方法之前执行的方法。通常用于设置测试的前置条件、初始化资源等。

   - 示例：

     ```java
     @Before
     public void setUp() {
         // 初始化测试环境
     }
     ```

2. **@After:**

   - **用途：** 用于标注在测试方法之后执行的方法。通常用于清理测试环境、释放资源等。

   - 示例：

     ```java
     @After
     public void tearDown() {
         // 清理测试环境
     }
     ```

这两个注解可以帮助你在执行测试方法前后进行一些准备工作和清理工作，确保测试的独立性和可重复性。



`@BeforeClass` 和 `@AfterClass` 注解也是测试框架中的注解，但与 `@Before` 和 `@After` 不同，它们分别用于在测试类的所有测试方法执行前和执行后执行一次。

1. **@BeforeClass:**

   - **用途：** 用于标注在所有测试方法执行前执行的方法。通常用于进行一些整个测试类级别的准备工作。

   - 示例：

     ```java
     @BeforeClass
     public static void setUpClass() {
         // 执行一次的准备工作
     }
     ```

2. **@AfterClass:**

   - **用途：** 用于标注在所有测试方法执行后执行的方法。通常用于进行一些整个测试类级别的清理工作。

   - 示例：

     ```java
     @AfterClass
     public static void tearDownClass() {
         // 执行一次的清理工作
     }
     ```

这两个注解的方法必须是静态的，因为它们在测试类的实例创建之前就会执行。通常，`@BeforeClass` 用于创建共享资源，而 `@AfterClass` 用于释放这些资源。



是的，`@AfterClass` 注解标注的方法在整个测试类执行完成后只会被调用一次。这使得它适合于执行一些清理工作，例如释放资源或关闭连接，这些工作只需要在所有测试方法执行完毕后执行一次。



![image-20231205215134274](E:\Typroa_Picture\image-20231205215134274.png)

**断言测试，查出来的数据和自己想要的数据是否一样。** 



### 8.2项目监控



![image-20231205215735936](E:\Typroa_Picture\image-20231205215735936.png)

 

![image-20231205221217796](E:\Typroa_Picture\image-20231205221217796.png)

@Endpoint(id="端点")

这里是获取数据库是否可以连接。

通过一个方法返回Json字符串是否可行。

自己配置一个端点，@ReadOperation，是通过get请求获得。





# 第九章：面试模块回答

#### 1.如何记录⽤户的**登陆**状态？

- 当加载页面的时候，程序会随机生成数字生成验证码加载到Redis中，并且设置到cookie上。

- 用户登录，查看是否勾选了记住我。如果是则有一个大的数字3600，如果不是则100.

- 验证用户的账号密码。

- 如果成功，就要生成登录凭证。

- 程序定义一个LoginTicket登录凭证表。里面记录了用户id，登录凭证ticket(随机生成字符串)，状态status(0有效，1无效)，还有过期时间。

- 并且把登录凭证ticket放到Redis中。同时也把登录凭证返回到cookie中，设置cookie过期时间。

- 构造拦截器，当用户执行每一个操作的时候，都会触发拦截器。

- 拦截器里的内容就是检查浏览器的ticket是否和Redis中的ticket中相等，并且用户是否相同。

- 登陆凭证没有过期并且在Redis中就允许访问。

  

#### 2.使用Redis存放验证码。

![image-20231210222722665](E:\Typroa_Picture\image-20231210222722665.png)

当客户访问登录页面的时候，程序会生成随机字符串kaptchaOwner返回给cookie。 

把kaptchaOwner用来Redis的key，Value则是验证码的那个数字

当用户登录的时候，就会用应cookie中的kaptchaOwner，并且用户输入的验证码，与Redis中kaptchaOwner中的Value做比较。

#### 3.使用kafka作为消息队列，生产者消费者

定义一个事件实体。里面包含了、点赞者ID、点赞的对象等消息

- 当用户点击点赞的时候，触发点赞事件，生成Event类。
- 生产者会把事件Event类存放在点赞消息队列中。
- 消费者监听到点赞消息队列有信息，就会取出Event。
- 并且通过Event里面的关键信息，填充内容封装成一个Message对象，并存放在数据库中。

#### 4.把常用用户信息存入Redis中。

- 因为当前登录用户，使用页面比较频繁，每个页面都需要查询该用户的信息。

- 所以把用户信息放入Redis中更为合适。

- 把当前用户信息存放在Redis中。key是用户的id，value是User的Jason数据。


#### 5.实现热门帖子排行。

- ​	每一个帖子都有固定的分数，贴子的分数越高那么就越靠前。

- ​	帖子的分数与（点赞、评论、加精、）有关。

- ​	拿点赞来说，当帖子收到点赞，那么就会把帖子的Id存放在Redis中。

  那么就用到Spring Quartz 创造一个定时任务。

  创建一个PostScoreFreshJob类实现Job接口，并且编写跟新分数的逻辑。

  配置Job_detail和Trigger设置触发时间。

  时间一到就可以就从Redis取出帖子Id计算帖子的分数，并且更新数据库中帖子的分数。

#### 6.把热门帖子存放在本地缓存Caffeine

热门帖子其实就是分数高的帖子，按分数排序。

用户查询热门帖子的时候，会先去caffine中找

如果caffeine中没有缓存数据，那么就在mysql中把数据存放在caffeine。

下次查找就会命中caffeine。

```
private LoadingCache<String,List<DiscussPost>> postListCache;
```

postListCache保存了热门帖子。