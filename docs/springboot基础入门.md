### 一. 什么是springboot与微服务？

Spring 是一个开源框架，2003 年兴起的一个轻量级的 Java 开发框架，作者：Rod Johnson 。

什么是 SpringBoot 呢，就是一个 javaweb 的开发框架，和 SpringMVC 类似，对比其他 javaweb 框架的好处，官方说是简化开发，约定大于配置， you can "just run"，能迅速的开发web应用，几行代码开发一个 http 接口。

- 构建一个个功能独立的微服务应用单元，可以使用 springboot,可以帮我们快速构建一个应用
- 大型分布式网络服务的调用，这部分由 spring cloud 来完成，实现分布式
- 在分布式中间，进行流式数据计算、批处理，我们有 spring cloud data flow
- spring为 我们想清楚了整个从开始构建应用到大型分布式应用全流程方案



#### 1.1. springboot 初体验

File ——>new project ——> 选择spring Initailizr ——> 默认Default:https://start.spring.io，点击next

——>选择配置依赖 ——> 输入项目名称 ——> 点击完成

配置 IDEA 的 Maven，指定 Setting 的 Maven目录和 MAVEN的setting.xml 文件

如果下载依赖太慢，设置 maven 的仓库：

```xml
<mirrors>
	<mirror>
		<id>alimaven</id>
		<name>aliyun maven</name>
		<url>http://maven.aliyun.com/nexus/content/groups/public/</url>
		<mirrorOf>central</mirrorOf>       
	</mirror>
</mirrors>
```

删掉多余的东西：.mvn   .gitignore  HELP.md  mvnw  mvnw.cmd

在启动主程序同目录创建 controller 层：

```java
package com.example.demo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/hello")
public class HelloController {
    @GetMapping("/hello")
    @ResponseBody
    public String sayHello(){
        return "hello";
    }
}
```

运行主程序，访问：http://localhost:8080/hello/hello，页面即显示 hello 字符串。

也可以打包运行：在 Terminal 中输入 mvn package 

运行结果：

> Building jar: D:\Repository\IdeaCode\springboot\demo\target\demo-0.0.1-SNAPSHOT.jar

cmd 控制台进入 D:\Repository\IdeaCode\springboot\demo\target\目录，执行命令：

```cmake
java -jar .\demo-0.0.1-SNAPSHOT.jar
```

浏览器访问可以得到相同的结果。

扩展 IDE 快捷键：

```xaml
Ctrl+D 复制一行
Ctrl+Y 删除一行
Ctrl+P 参数提示
Ctrl+Alt+V 自动补齐方法
Ctrl+N 查找类方法
Alt+Ins 构造器、getter/setter toString
Ctrl+O 重载方法提示
Alt+Enter 提示导入类etc
Shift+F6 :文件重命名
```

修改启动图标，在resources 下创建banner.txt，输入  https://www.bootschool.net/ascii 网站得到的图标字符串即可。

#### 1.2. HelloWorld深度理解

##### 1.2.1. 父项目

```xaml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.1.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

这个父项目**spring-boot-starter-parent**又依赖一个父项目

```xaml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.0.1.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
</parent>
```

下面有个属性，定义了对应的版本号

```xaml
<properties>
    <activemq.version>5.15.3</activemq.version>
    <antlr2.version>2.7.7</antlr2.version>
    <appengine-sdk.version>1.9.63</appengine-sdk.version>
    <artemis.version>2.4.0</artemis.version>
    <aspectj.version>1.8.13</aspectj.version>
    <assertj.version>3.9.1</assertj.version>
    <atomikos.version>4.0.6</atomikos.version>
    <bitronix.version>2.1.4</bitronix.version>
    <build-helper-maven-plugin.version>3.0.0</build-helper-maven-plugin.version>
    。。。。。。。
```

##### 1.2.2. 启动器

```xaml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**spring-boot-starter-web**：帮我们导入 web 模块正常运行所依赖的组件

**spring boot** 将所有的功能场景都抽取出来，做成一个个的 starter (启动器)，只需要在项目里引入这些 starter 相关场景的所有依赖都会被导入进来，要用什么功能就导入什么场景的启动器。

##### 1.2.3. 主程序入口

```java
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

**@SpringBootApplication**：说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动应用。

进入 `SpringBootApplication` 注解

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)

public @interface SpringBootApplication {...}
```

**@SpringBootConfiguration**：SpringBoot的配置类： 标准在某个类上，表示这是一个SpringBoot的配置类

**@EnableAutoConfiguration**：借助@Import的帮助，将所有符合条件的`@Configuration`配置都加载到当前SpringBoot创建并使用的IoC容器

```
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {...}
```

借助于Spring框架原有的一个工具类：SpringFactoriesLoader的支持，`@EnableAutoConfiguration`可以智能的自动配置功效才得以大功告成！

在 `AutoConfigurationImportSelector` 类中可以看到通过 `SpringFactoriesLoader.loadFactoryNames()`

把 `spring-boot-autoconfigure.jar`/`META-INF`/`spring.factories` 中每一个 xxxAutoConfiguration 文件都加载到容器中，spring.factories 文件里每一个 xxxAutoConfiguration 文件一般都会有下面的条件注解:

- @ConditionalOnClass ： classpath 中存在该类时起效
- @ConditionalOnMissingClass ： classpath 中不存在该类时起效
- @ConditionalOnBean ： DI 容器中存在该类型 Bean 时起效
- @ConditionalOnMissingBean ： DI 容器中不存在该类型 Bean 时起效
- @ConditionalOnSingleCandidate ： DI 容器中该类型 Bean 只有一个或 @Primary 的只有一个时起效
- @ConditionalOnExpression ： SpEL 表达式结果为 true 时
- @ConditionalOnProperty ： 参数设置或者值一致时起效
- @ConditionalOnResource ： 指定的文件存在时起效
- @ConditionalOnJndi ： 指定的 JNDI 存在时起效
- @ConditionalOnJava ： 指定的 Java 版本存在时起效
- @ConditionalOnWebApplication ： Web 应用环境下起效
- @ConditionalOnNotWebApplication ： 非 Web 应用环境下起效

`SpringFactoriesLoader` 是一个抽象类，类中定义的静态属性定义了其加载资源的路径 `public static final String FACTORIES_RESOURCE_LOCATION = "META-INF/spring.factories"`，此外还有三个静态方法：

- loadFactories：加载指定的 factoryClass 并进行实例化。
- loadFactoryNames：加载指定的 factoryClass 的名称集合。
- instantiateFactory：对指定的 factoryClass 进行实例化。

 在 loadFactories 方法中调用了 loadFactoryNames 以及 instantiateFactory 方法。

```java
public static <T> List<T> loadFactories(Class<T> factoryType, 
                                        @Nullable ClassLoader classLoader) {
        Assert.notNull(factoryType, "'factoryType' must not be null");
        ClassLoader classLoaderToUse = classLoader;
        if (classLoader == null) {
            classLoaderToUse = SpringFactoriesLoader.class.getClassLoader();
        }

        List<String> factoryImplementationNames = 
            loadFactoryNames(factoryType, classLoaderToUse);
       
    	if (logger.isTraceEnabled()) {
            logger.trace("Loaded [" + factoryType.getName() + "] names: " + 		
                         factoryImplementationNames);
        }

        List<T> result = new ArrayList(factoryImplementationNames.size());
        Iterator var5 = factoryImplementationNames.iterator();

        while(var5.hasNext()) {
            String factoryImplementationName = (String)var5.next();
            result.add(instantiateFactory(factoryImplementationName, 
                                          factoryType, classLoaderToUse));
        }

        AnnotationAwareOrderComparator.sort(result);
        return result;
    }
```

`loadFactories` 方法首先获取类加载器，然后调用 `loadFactoryNames`方法获取所有的指定资源的名称集合、接着调用 `instantiateFactory` 方法实例化这些资源类并将其添加到 result 集合中。最后调用AnnotationAwareOrderComparator.sort 方法进行集合的排序。

> 参考文章：《[SpringBoot之@EnableAutoConfiguration注解](https://blog.csdn.net/zxc123e/article/details/80222967?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1)》 

上面解释了`AutoConfigurationImportSelector` 将类路径下 MATE-INF/spring.factories里面配置的所有的EnableAutoConfiguration的值加入到了容器中。

下面以`HttpEncodingAutoConfiguration` 为例

```java
@Configuration 
//表示是一个配置类，以前编写的配置文件一样，也可以给容器中添加组件

@EnableConfigurationProperties({HttpEncodingProperties.class})
//启动指定类的 Configurationproperties 功能；将配置文件中的值和 HttpEncodingProperties 绑定起来了；并把 HttpEncodingProperties 加入 ioc 容器中

@ConditionalOnWebApplication
//根据不同的条件，进行判断，如果满足条件，整个配置类里面的配置就会失效，判断是否为 web 应用；
(
    type = Type.SERVLET
)
@ConditionalOnClass({CharacterEncodingFilter.class})
//判断当前项目有没有这个类，解决乱码的过滤器

@ConditionalOnProperty(
    prefix = "spring.http.encoding",
    value = {"enabled"},
    matchIfMissing = true
)
//判断配置文件是否存在某个配置 spring.http.encoding，matchIfMissing = true 如果不存在也是成立，即使不配置也生效

public class HttpEncodingAutoConfiguration {
   //给容器添加组件，这个组件的值需要从properties属性中获取
    private final HttpEncodingProperties properties;
	
    //只有一个有参数构造器情况下，参数的值就会从容器中拿
    public HttpEncodingAutoConfiguration(HttpEncodingProperties properties) {
        this.properties = properties;
    }

    @Bean
    @ConditionalOnMissingBean
    public CharacterEncodingFilter characterEncodingFilter() {
        CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
        filter.setEncoding(this.properties.getCharset().name());
        filter.setForceRequestEncoding(
            this.properties.shouldForce(
          org.springframework.boot.autoconfigure.http.HttpEncodingProperties.Type.REQUEST
            ));
        filter.setForceResponseEncoding(
            this.properties.shouldForce(
         org.springframework.boot.autoconfigure.http.HttpEncodingProperties.Type.RESPONSE
            ));
        return filter;
    }
```

所有在配置文件中能配置的属性都是在xxxProperties类中封装着；配置文件能配置什么就可以参照某个功能对应的这个属性类。

```java
@ConfigurationProperties(prefix = "spring.http.encoding")
//从配置文件中的值进行绑定和bean属性进行绑定
public class HttpEncodingProperties {}
```

根据当前不同条件判断，决定这个配置类是否生效？

一旦这个配置类生效；这个配置类会给容器添加各种组件；这些组件的属性是从对应的properties中获取的，这些类里面的每个属性又是和配置文件绑定的。

总结：

1. SpringBoot启动会加载大量的自动配置类。
2. 我们看我们需要的功能有没有SpringBoot默认写好的默认配置类。
3. 如果有在看这个自动配置类中配置了哪些组件；（只要我们要用的组件有，我们需要再来配置）。
4. 给容器中自动配置添加组件的时候，会从properties类中获取属性。我们就可以在配置文件中指定这些属性的值。

自动配置类必须在一定条件下生效，我们可以通过在配置文件中启用 debug=true 属性，配置文件，打印自动配置报告，这样就可以知道自动配置类生效。

### 二、配置文件

#### 2.1. 配置文件

Spring Boot使用全局配置文件，配置文件名是固定的；

- application.properties
- application.yml

配置文件作用：修改Spring Boot在底层封装好的默认值；

YAML（YAML AIN'T Markup Language） 

它的基本语法规则如下：

- 大小写敏感
- 使用缩进表示层级关系
- 缩进时不允许使用Tab键，只允许使用空格。
- 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可

`#` 表示注释，从这个字符一直到行尾，都会被解析器忽略。

#### 2.2. 配置文件值注入

编写一个实体类 Dog

```java
@Component //注册bean到容器中
public class Dog {
    @Value("阿黄")
    private String name;
    @Value("3")
    private Integer age;
    
    //set get toString 方法   
}
```

在SpringBoot的测试类下注入狗狗输出一下

```java
@SpringBootTest
class DemoApplicationTests {
    @Autowired
    Dog dog;

    @Test
    void contextLoads() {
        System.out.println(dog);
    }

}
```

结果：

> Dog{name='阿黄', age=3}

这是以前的方法，

@Value 支持三种取值方式，分别是 字面量、${key} 从环境变量、配置文件中获取值以及 #{SpEL}

试试yaml配置：

```java
@Component 
@ConfigurationProperties(prefix = "person")
public class Person {
    private String name;
    private Integer age;
    private Boolean happy;
    private Date birth;
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
    
    //有参无参构造、get、set方法、toString()方法  
}
```

在 springboot 项目中的 resources 目录下新建一个文件 application.yml，内容添加：

```xml
person:
  name: qinjiang
  age: 3
  happy: false
  birth: 2000/01/01
  maps: {k1: v1,k2: v2}
  lists:
   - code
   - girl
   - music
  dog:
    name: 旺财
    age: 1
```

在SpringBoot的测试类下输出：

```java
@SpringBootTest
class DemoApplicationTests {
    @Autowired
    Person person;

    @Test
    void contextLoads() {
        System.out.println(person);
    }
}
```

结果：

> Person{name='qinjiang', age=3, happy=false, birth=Sat Jan 01 00:00:00 CST 2000, maps={k1=v1, k2=v2}, lists=[code, girl, music], dog=Dog{name='旺财', age=1}}

【注意】properties配置文件在写中文的时候，会有乱码 ， 我们需要去IDEA中设置编码格式为UTF-8；

Fiel --> settings --> file encoding  --> property-->utf-8 ,并勾上Transparent native-to-ascii conversion

IDEA 提示，springboot配置注解处理器没有找到，则添加依赖：

```xml
        <!-- 导入配置文件处理器，配置文件进行绑定就会有提示，需要重启 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
```

| 二者区别             | @Value                   | @ConfigurationProperties |
| -------------------- | ------------------------ | ------------------------ |
| 功能                 | 批量注入配置文件中的属性 | 一个个指定               |
| 松散绑定（松散语法） | 支持                     | 不支持                   |
| SPEL                 | 不支持                   | 支持                     |
| JSR303数据校验       | 支持                     | 不支持                   |
| 复杂类型封装         | 支持                     | 不支持                   |

配置yml和配置properties都可以获取到值 ， 强烈推荐 yml；

如果我们在某个业务中，只需要获取配置文件中的某个值，可以使用一下 @value；

如果说，我们专门编写了一个JavaBean来和配置文件进行一一映射，就直接@configurationProperties，不要犹豫！



#### 2.3. JSR303校验

请求参数的校验是很多新手开发非常容易犯错，或存在较多改进点的常见场景。比较常见的问题主要表现在以下几个方面：

- 仅依靠前端框架解决参数校验，缺失服务端的校验。这种情况常见于需要同时开发前后端的时候，虽然程序的正常使用不会有问题，但是开发者忽略了非正常操作。比如绕过前端程序，直接模拟客户端请求，这时候就会突然在前端预设的各种限制，直击各种数据访问接口，使得我们的系统存在安全隐患。
- 大量地使用`if/else`语句嵌套实现，校验逻辑晦涩难通，不利于长期维护。

所以，针对上面的问题，建议服务端开发在实现接口的时候，对于请求参数必须要有服务端校验以保障数据安全与稳定的系统运行。同时，对于参数的校验实现需要足够优雅，要满足逻辑易读、易维护的基本特点。

**什么是JSR？**

JSR是Java Specification Requests的缩写，意思是Java 规范提案

使用数据校验，可以保证数据的正确性；例如上面的person类

```
@Component 
@ConfigurationProperties(prefix = "person")
@Validated  
public class Person {
    @Email(message="邮箱格式错误") 
    private String name;
    
    // getter 和setter方法
}
```

常见参数

```xml
@NotNull(message="名字不能为空")
private String userName;

@Max(value=120,message="年龄最大不能查过120")
private int age;

@Email(message="邮箱格式错误")
private String email;

空检查
@Null       验证对象是否为null
@NotNull    验证对象是否不为null, 无法查检长度为0的字符串
@NotBlank   检查约束字符串是不是Null还有被Trim的长度是否大于0,只对字符串,且会去掉前后空格.
@NotEmpty   检查约束元素是否为NULL或者是EMPTY.
    
Booelan检查
@AssertTrue     验证 Boolean 对象是否为 true  
@AssertFalse    验证 Boolean 对象是否为 false  
    
长度检查
@Size(min=, max=) 验证对象（Array,Collection,Map,String）长度是否在给定的范围之内  
@Length(min=, max=) string is between min and max included.

日期检查
@Past       验证 Date 和 Calendar 对象是否在当前时间之前  
@Future     验证 Date 和 Calendar 对象是否在当前时间之后  
@Pattern    验证 String 对象是否符合正则表达式的规则

.......等等
除此以外，我们还可以自定义一些数据校验规则
```

又例：

```
@NotNull(message = "adultTax不能为空")
private Integer adultTax;

@NotNull(message = "adultTaxType不能为空")
@Min(value = 0, message = "adultTaxType 的最小值为0")
@Max(value = 1, message = "adultTaxType 的最大值为1")
private Integer adultTaxType;

@NotNull(message = "reason信息不可以为空")
@Pattern(regexp = "[1-7]{1}", message = "reason的类型值为1-7中的一个类型")
private String reason;//订单取消原因
```

下边是一个完整的例子：

```java
public class ValidateTestClass {
    @NotNull(message = "reason信息不可以为空")
    @Pattern(regexp = "[1-7]{1}", message = "reason的类型值为1-7中的一个类型")
    private String reason;//订单取消原因
    
    //get、set方法、有参构造方法、无参构造方法、toString方法省略
    
    /**
     * 验证参数：就是验证上述注解的完整方法
     */
    public void validateParams() {
        //调用JSR303验证工具，校验参数
        Validator validator = Validation.buildDefaultValidatorFactory().getValidator();
        Set<ConstraintViolation<ValidateTestClass>> violations = validator.validate(this);
        Iterator<ConstraintViolation<ValidateTestClass>> iter = violations.iterator();
        if (iter.hasNext()) {
            String errMessage = iter.next().getMessage();
            throw new ValidationException(errMessage);
        }
    }
    
}
```

然后写一个测试类进行验证：

```java
public class ValidateTestClassValidateTest {

    @Test
    public void validateParam(){
        ValidateTestClass validateTestClass = new ValidateTestClass();
        validateTestClass .setReason("12");

        validateTestClass .validateParams(); //调用验证的方法
    }
}
```

运行结果：

> javax.validation.ValidationException: reason的类型值为1-7中的一个类型

正则则表达式的验证对象可以为String类型的，但是不可以为Integer类型的数据，否则使用正则表达式进行验证的时候就会出现错误：

> javax.validation.UnexpectedTypeException: HV000030: No validator could be found for type: java.lang.Integer.



#### 2.4. 多环境配置及配置文件位置

profile 是 Spring 对不同环境提供不同配置功能的支持，可以通过激活不同的环境版本，实现快速切换环境；

我们在主配置文件编写的时候，文件名可以是 application-{profile}.properties/yml , 用来指定多个环境版本；

例如：

application-test.properties 代表测试环境配置

application-dev.properties 代表开发环境配置

但是 Springboot 并不会直接启动这些配置文件，它默认使用 application.properties 主配置文件；

我们需要通过一个配置来选择需要激活的环境：

```xml
spring.profiles.active=dev
```

**yaml 的多文档块**

和 properties 配置文件中一样，但是使用 yml 去实现不需要创建多个配置文件，更加方便了 

```xml
server:
  port: 8081
  
#选择要激活那个环境块
spring:
  profiles:
    active: prod

server:
  port: 8083
spring:
  profiles: dev #配置环境的名称

server:
  port: 8084
spring:
  profiles: prod  #配置环境的名称
```

注意：如果 yml 和 properties 同时都配置了端口，并且没有激活其他环境 ， 默认会使用properties配置文件的！

- project:/config/（项目根目录下面config文件夹里的配置文件）
- project:/（项目根目录下面的配置文件）
- classpath:/config/（Resources文件夹下面config文件夹里的配置文件）
- classpath:/（Resources文件夹下面的配置文件）

官方参考文档：[Externalized Configuration](https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#boot-features-external-config)

[Common application properties](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#common-application-properties)

####  2.5. 自动配置的原理

### 三、日志文件

slf4j 为各种日志框架提供了一个统一的界面，使用户可以用统一的接口记录日志，动态地决定要使用的实现框架，比如 Logback，Log4j，common-logging 等框架都实现了这些接口。

| 日志抽象层                                                   | 日志实现             |
| ------------------------------------------------------------ | -------------------- |
| ~~JCL(Jakarta Commons Logging)~~ SLF4j (Simple Logging Facade for Java) | Log4j Log4j2 Logback |

SpringBoot 默认使用 logback，logback 相当于 log4j 的升级版，做了很多改进，比如更快的运行速度，更充分的测试等。

logback 由三个模块组成

- logback-core
- logback-classic
- logback-access

`logback-core`是其它模块的基础设施，其它模块基于它构建，显然，`logback-core`提供了一些关键的通用机制。`logback-classic`的地位和作用等同于 `Log4J`，它也被认为是 `Log4J`的一个改进版，并且它实现了简单日志门面 `SLF4J`；而 `logback-access`主要作为一个与 `Servlet`容器交互的模块，比如说`tomcat`或者 `jetty`，提供一些与 `HTTP`访问相关的功能。

以后开发的时候，日志记录方法的调用，不应该来直接调用日志的实现类，而是调用日志抽象层里面的方法。

应该给系统里面导入 slf4j 的 jar 包和 logback 的实现 jar。

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);
    logger.info("Hello World");
  }
}
```

在项目中，我们使用Logback，其实只需增加一个配置文件（自定义你的配置）即可。

#### 3.1.  配置文件详解

配置文件精简结构如下所示

```xml
<configuration scan="true" scanPeriod="60 seconds" debug="false">  
         <!-- 属性文件:在properties/yml文件中找到对应的配置项 -->
    <springProperty scope="context" name="logging.path" source="logging.path"/>
    <contextName>程序员小明</contextName> 
    
    <appender>
        //xxxx
    </appender>   
    
    <logger>
        //xxxx
    </logger>
    
    <root>             
       //xxxx
    </root>  
</configuration>  
```

这个文件在 springboot 中默认叫做 `logback-spring.xml`，我们只要新建一个同名文件放在 `resources` 下面， 配置即可生效。

- contextName：每个`logger`都关联到`logger`上下文，默认上下文名称为`“default”`。但可以使用`contextName`标签设置成其他名字，用于区分不同应用程序的记录

- appender：负责写日志的组件

- appender 的种类：

  - ConsoleAppender：把日志添加到控制台
  - FileAppender：把日志添加到文件
  - RollingFileAppender：滚动记录文件，先将日志记录到指定文件，当符合某个条件时，将日志记录到其他文件。它是FileAppender的子类


#### 3.2.  项目实例

有关日志的简单配置，我们可以直接在`application.yml`中进行简单的配置，比如指明日志的打印级别和日志的输出位置。

```xml
logging:
  level:
    root: info
  path: ./logs
```

也可以根据分环境配置指明使用的配置文件，缺省为 logback-spring.xml 。

```xml
logging:
  level:
    root: info
  path: ./logs
  config: classpath:/logback-dev.xml
```

通过控制台输出的 log

在 resources 目录下新建 logback-spring.xml 文件。举例一个简单的需求，通过控制台输出的 log

```xml
<configuration>
    <!-- 默认的控制台日志输出，一般生产环境都是后台启动，这个没太大作用 -->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <Pattern>%d{HH:mm:ss.SSS} %-5level %logger{80} - %msg%n</Pattern>
        </encoder>
    </appender>
    
    <root level="info">
        <appender-ref ref="STDOUT"/>
    </root>
</configuration>
```

打印日志的 controller 

```java
@Controller
@RequestMapping("/hello")
public class HelloController {
    private static final Logger LOGGER = LoggerFactory.getLogger(HelloController.class);

    @Autowired
    private TestLogService testLogService;

    @GetMapping("/hello")
    @ResponseBody
    public String sayHello(){
        LOGGER.info("GLMAPPER-SERVICE:info");
        LOGGER.error("GLMAPPER-SERVICE:error");
        testLogService.printLogToSpecialPackage();

        return "hello";
    }
}
```

验证结果：

> 09:48:17.716 INFO  com.example.demo.controller.HelloController - GLMAPPER-SERVICE:info
> 09:48:17.716 ERROR com.example.demo.controller.HelloController - GLMAPPER-SERVICE:error

上面的就是通过控制台打印出来的，这个时候因为我们没有指定日志文件的输出，因为不会在工程目录下生产`logs`文件夹。

控制台不打印，直接输出到日志文件，先来看下配置文件：

<div align="center"> <img src="pics/输出到日志配置文件.png" width="800"/> </div>
参考文章：《[看完这个不会配置 logback ，请你吃瓜！](https://juejin.im/post/5b51f85c5188251af91a7525)》

### 四、web开发

web 开发要解决的问题：

- 导入静态资源
- 首页
- jsp,模板引擎Thymeleaf
- 装配扩展springmvc
- 增删查改
- 拦截器
- 国际化

#### 4.1. 导入静态资源

查看`WebMVCAutoConfiguration`源码如下：

```java
     @Override
     public void addResourceHandlers(ResourceHandlerRegistry registry) {
            if (!this.resourceProperties.isAddMappings()) {
                logger.debug("Default resource handling disabled");
                return;
            }
            Integer cachePeriod = this.resourceProperties.getCachePeriod();
            if (!registry.hasMappingForPattern("/webjars/**")) {
                customizeResourceHandlerRegistration(
                        registry.addResourceHandler("/webjars/**")
                                .addResourceLocations(
                                        "classpath:/META-INF/resources/webjars/")
                        .setCachePeriod(cachePeriod));
            }
           
            // 对静态资源文件映射支持
            // 第一步拿到 staticPathPattern = "/**"
            String staticPathPattern = this.mvcProperties.getStaticPathPattern();
          
            // 第二步如果资源请求没有对应映射，就添加资源处理器及资源找寻位置并设置缓存
            if (!registry.hasMappingForPattern(staticPathPattern)) {
                customizeResourceHandlerRegistration(
                        registry.addResourceHandler(staticPathPattern)
                                .addResourceLocations(
                                        this.resourceProperties.getStaticLocations())
                        .setCachePeriod(cachePeriod));
            }
        }
```

其中 staticLocations 在 `ResourceProperties`类中，源码如下：

```java
@ConfigurationProperties(
    prefix = "spring.resources",
    ignoreUnknownFields = false
)
public class ResourceProperties {
    private static final String[] CLASSPATH_RESOURCE_LOCATIONS = new String[]{
        				"classpath:/META-INF/resources/", 
                 		"classpath:/resources/", 
                        "classpath:/static/", 
                        "classpath:/public/"};
    private String[] staticLocations;
 
    public ResourceProperties() {
        this.staticLocations = CLASSPATH_RESOURCE_LOCATIONS;
        ......
    }

    public String[] getStaticLocations() {
        return this.staticLocations;
    }
    ......
}
```

对于所有访问 /webjars/** 下面的请求，都会去 classpath:/META-INF/resources/webjars/ 找资源。

在项目中以 jar 包的方式引入静态资源，比如引入 jquery，就可以在 pom.xml 文件中，加入如下依赖：

```xml
    <dependency>
		<groupId>org.webjars</groupId>
		<artifactId>jquery</artifactId>
		<version>3.3.1</version>
	</dependency>
```

对于在项目中引入该jquery，可以采用如下方式：

> localhost:8080/webjars/jquery/3.3.1/jquery.js

对于访问当前项目的所有请求  /**，如果没有任何控制器处理，都会去（静态资源的文件夹下）找映射，具体目录如下：

`classpath:/META-INF/resources/`,
`classpath:/resources/`,
`classpath:/static/`,
`classpath:/public/`
`/`：当前项目的根路径
例如：访问 http://localhost:8080/abc 请求，没有控制器处理该请求，就会去静态资源文件夹里面找 abc

例如：访问 http://localhost:8080/asserts/img/test.png

目录结构：

```xml
-src
	-main
		-java
		-resources
			-static.asserts.img
				-test.png
```

这里注意URL上面不要添加静态资源文件夹(路径)的名字，如static、public等等。

#### 4.2. 欢迎页

##### 4.2.1. 使用 index.html 作为首页面

默认访问 resources/static/ 目录下的 index.html 作为首页文件，如果没有，则访问 resources/templates  目录下的 index.html 作为首页文件，需要添加依赖：

```xml
    <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
```

可以在配置文件修改静态资源文件夹

```xml
spring.resources.static-locations=classpath:xxxx
```

##### 4.2.2. 使用非index.html作为首页

静态页面的 return 默认是跳转到 /static/ 目录下，当在 pom.xml 中引入了 thymeleaf 组件，动态跳转会覆盖默认的静态跳转，默认就会跳转到/templates/下，注意看两者 return 代码也有区别，动态没有 html 后缀。

**静态首页**

```java
@Controller
public class HelloController {
    @GetMapping("/")
    public String hello(){
        return "forward:login.html";
    }
}
```

 或者通过自定义一个 **MVC** 配置，并重写 **addViewControllers** 方法进行转发：

```java
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("forward:login.html");
    }
}
```

**动态首页**

```java
@Controller
public class HelloController {
     @GetMapping("/")
     public String hello(){
         return "login";
     }
}
```

另一种方式是通过自定义一个 MVC 配置，并重写 addViewControllers 方法进行映射关系配置即可。

```java
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("login");
    }
}
```

官方参考文档：[29. Developing Web Applications](https://docs.spring.io/spring-boot/docs/2.1.7.RELEASE/reference/html/boot-features-developing-web-applications.html#boot-features-spring-mvc-auto-configuration)

#### 4.3. 模板引擎Thymeleaf

第一步：引入jar包（thymeleaf对应的starter）：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

第二步：配置 thymeleaf：

```
spring:
  thymeleaf:
  	prefix: classpath:/templates/
  	check-template-location: true
  	cache: false
  	suffix: .html
  	encoding: UTF-8
  	content-type: text/html
  	mode: HTML5
```

- prefix：指定模板所在的目录
- check-tempate-location: 检查模板路径是否存在
- cache: 是否缓存，开发模式下设置为false，避免改了模板还要重启服务器，线上设置为true，可以提高性能。
- encoding&content-type：这个大家应该比较熟悉了，与Servlet中设置输出对应属性效果一致。　
- mode：这个还是参考官网的说明吧，并且这个是2.X与3.0不同，本文自动引入的包是2.15。

第三步 编写 thymeleaf 模板文件：

只要把 HTML 页面放到 classpath:/templates/，`thymeleaf` 就能自动渲染；

使用：

1. 导入 thymeleaf 的名称空间

   > ```jsp
   > <html xmlns:th="http://www.thymeleaf.org">    
   > ```

2. thymeleaf 语法规则

   1. 变量表达式(获取变量值)

      > ```jsp
      > <div th:text="'你是否读过，'+${session.book}+'!!'">
      > </div>
      > ```

      ```jsp
      代码分析：
      1.可以看出获取变量值用$符号,对于javaBean的话使用变量名.属性名方式获取,这点和EL表达式一样
      2.它通过标签中的th:text属性来填充该标签的一段内容，意思是$表达式只能写在th标签内部,不然不会生效,上面例子就是使用th:text标签的值替换div标签里面的值,至于div里面的原有的值只是为了给前端开发时做展示用的.这样的话很好的做到了前后端分离.意味着div标签中的内容会被表达式${session.book}的值所替代，无论模板中它的内容是什么，之所以在模板中“多此一举“地填充它的内容，完全是为了它能够作为原型在浏览器中直接显示出来。
      3.访问spring-mvc中model的属性，语法格式为“${}”，如${user.id}可以获取model里的user对象的id属性 
      4.牛叉的循环 
         <tbody th:each="article : ${list}">
           <tr>
              <td th:text="${article.id}"></td>
              <td th:text="${article.title}"></td>
          </tr>
          </tbody>
      ```

      2. URL表达式(引入URL)

         - 引用静态资源文件(CSS使用th:href，js使用使用th:src)

           ```html
           <html xmlns:th="http://www.thymeleaf.org">
           <head>
               <!--jquery-3.5.0.js 放在目录resources/static/js下-->
               <script type="text/javascript" th:src="@{js/jquery-3.5.0.js}"></script>
               <script type="text/javascript" >
                   alert($)
               </script>
           </head>
           ```

         - href 链接URL(使用th:href)

           ```html
           <link href="#" th:href="@{/css/signin.css}" rel="stylesheet" />
           ```

      3. 选择变量表达方法：语法：*{…}
         也叫星号变量表达式，使用th:object属性来绑定对象，比如：

         ```html
         <div th:object="${session.user}">
             <p>Name: <span th:text="*{firstName}">Sebastian</span>.</p>
             <p>Surname: <span th:text="*{lastName}">Pepper</span>.</p>
         </div>
         
         <!--等价于-->
         
         <div>
           <p>Name: <span th:text="${session.user.firstName}">Sebastian</span>.</p>
           <p>Surname: <span th:text="${session.user.lastName}">Pepper</span>.</p>
         </div>
         ```

         官方参考文档：[usingthymeleaf](https://www.thymeleaf.org/doc/tutorials/2.1/usingthymeleaf.html)

#### 4.4. SpringMVC自动配置

Spring Boot 自动配置好了SpringMVC ，SpringBoot 对 SpringMVC 的默认配置参考官方参考文档： [Developing web applications](https://docs.spring.io/spring-boot/docs/1.5.12.RELEASE/reference/htmlsingle/#boot-features-developing-web-applications)

##### 4.4.1. 扩展 springmvc

编写一个配置类，直接实现WebMvcConfigurer，也可以直接继承WebMvcConfigurationSupport

不能标注@EnableWebMvc，这样就既保留了配置，也能拓展我们自己的应用

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

    @Override
    protected void addViewControllers(ViewControllerRegistry registry) {
        //super.addViewControllers(registry);
        //浏览器发送 /aa 请求来到 success
        registry.addViewController("/aa").setViewName("success");
    }
}
```

#### 4.5. RestfulCRUD

首先下载 [前端素材](https://pan.baidu.com/s/1yyjSeZcXd2RmaSwTtyCLig)，提取码：hb8k

html 页面放入 templates 目录下，css img js 放入 static 目录下，pom.xml 添加依赖：

```xml
    <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
```

此时页面没有样式，所有页面的静态资源都需要使用 thymeleaf 接管： 

- 导入 thymeleaf 的名称空间
- 引用css img js ，修改为 thymeleaf 语法

```xml
#application.properties 关闭模板引擎缓存
spring.thymeleaf.cache=false
```

参考官方文档：[Using Thymeleaf](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html)

##### 4.5.1. 国际化

在 resources 下新建文件夹 i18n ，新建文件login.properties、login_en_US.properties、login_zh_CN.properties，分别添加内容：

```xml
login.btn=登录
login.password=密码
login.remember=记住我
login.tip=请登录
login.username=用户名
```

```xml
login.btn=Sign in
login.password=Password
login.remember=Remember me
login.tip=Please sign in
login.username=Username
```

```xml
login.btn=登录
login.password=密码
login.remember=记住我
login.tip=请登录
login.username=用户名
```

在 application.properties 中指定

```xml
#我们配置文件的真实位置
spring.messages.basename=i18n.login
```

修改 html 页面：

```html
<h1 class="h3 mb-3 font-weight-normal">Please sign in</h1>
<input type="checkbox" value="remember-me"> Remember me
```

使用 thymeleaf 语法 #{} 引入修改为：

```html
<h1 class="h3 mb-3 font-weight-normal" th:text="#{login.tip}">Please sign in</h1>
<input type="checkbox" value="remember-me" > [[#{login.remember}]]
```

其他页面的修改类似。接下来实现`LocaleResolver`接口，获取前端参数 L：

```java
public class MyLocaleResolver implements LocaleResolver {
    @Override
    public Locale resolveLocale(HttpServletRequest request) {
        //获取请求中的参数链接
        String Language = request.getParameter("L");
        Locale local = Locale.getDefault();//如果没有就使用默认的
        //如果请求的链接携带了国际化的参数
        if(!StringUtils.isEmpty(Language)){
            String[] split = Language.split("_");
            //国家，地区
            local = new Locale(split[0],split[1]);
        }
        return local;
    }
}
```

将 `MyLocaleResolver` 类通过`@bean` 注入IOC容器 ：

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    @Bean
    public LocaleResolver localeResolver(){
        return new MyLocaleResolver();
    }
    
    //扩展 mvc 添加新的映射
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("index");
        registry.addViewController("/index.html").setViewName("index");
    }
}
```

这样点击下面的超链接，就能实现中英切换了。

```html
<a class="btn btn-sm" th:href="@{/index.html(L='zh_CN')}">中文</a>
<a class="btn btn-sm" th:href="@{/index.html(L='en_US')}">English</a>
```

##### 4.5.2. 登录功能实现

页面添加 action 请求：

```html
	<form class="form-signin" th:action="@{/user/login}">
        <button type="submit" th:text="#{login.btn}">Sign in</button>
	</form>
```

添加 Controller 类：

```java
@Controller
public class LoginController {
    @RequestMapping("/user/login")
    public String login(@RequestParam("username") String username, 			
                        @RequestParam("password") String password, 
                        Model model, 
                        HttpSession session){
        if(!StringUtils.isEmpty(username) && password.equals("123456")){
            session.setAttribute("loginUser",username);
            return "dashboard";
        }else{
            model.addAttribute("msg","用户名或密码错误!");
            return "index";
        }
    }
}
```

密码正确会跳转到 dashboard.html ，否则会跳转到 index.html 。失败跳转添加提示：

```html
<p style="color: red" th:text="${msg}" th:if="${not #strings.isEmpty(msg)}"></p>
```

但这种跳转用户名和密码会显示在页面，可以设置 controller 跳转 `main.html`，然后扩展 mvc 映射 `main.html` 到 dashboard。

```java
@Controller
public class LoginController {
    @RequestMapping("/user/login")
    public String login(@RequestParam("username") String username, @RequestParam("password") String password, Model model, HttpSession session){
        if(!StringUtils.isEmpty(username) && password.equals("123456")){
            session.setAttribute("loginUser",username);
            return "redirect:/main.html";
        }else{
            model.addAttribute("msg","用户名或密码错误!");
            return "index";
        }
    }
}
```

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("index");
        registry.addViewController("/index.html").setViewName("index");
        registry.addViewController("/main.html").setViewName("dashboard");
    }
}
```

但是这样即使不登录也可以访问主页面，添加登录拦截器。

##### 4.5.3. 登录拦截器

实现 `HandlerInterceptor`接口创建拦截器：

```java
public class LoginHandlerInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, 
                             HttpServletResponse response, 
                             Object handler) throws Exception {
        Object loginUser = request.getSession().getAttribute("loginUser");
        if (loginUser==null){
            request.setAttribute("msg","没有权限,请先登录!");
            request.getRequestDispatcher("/index.html").forward(request,response);
            return false;
        }else{
            return true;
        }
    }
}
```

在配置类中将拦截器注入到容器：

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginHandlerInterceptor())
            .addPathPatterns("/**")
            .excludePathPatterns("/index.html","/","/user/login");
    }
}
```

登录到主页后显示登录用户：

```html
<a> [[${session.loginUser}]] </a>
```

##### 4.5.4. 员工增删查改

引入`lombok`，可以减少一些  `get/set/toString` 方法的编写

```xml
  <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
  </dependency>
```

创建`pojo`类：

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Department {
    private Integer id;
    private String departmentName;
}
```

```java
@Data
@NoArgsConstructor
public class Employee {
    private Integer id;
    private String lastName;
    private String email;
    private Integer gender;// 0 男 1女
    private Department department;
    private Date birth;

    public Employee(Integer id, 
                    String lastName, 
                    String email, 
                    Integer gender, 
                    Department department) {
        this.id = id;
        this.lastName = lastName;
        this.email = email;
        this.gender = gender;
        this.department = department;
        //创建默认的日期
        this.birth = new Date();
    }
}
```

创建 `dao` 类：

```java
@Repository
public class DepartmentDao {
    //模拟数据库中的数据
    private static Map<Integer, Department> departments=null;
    static{
        departments=new HashMap<Integer, Department>();//创建一个部门表
        departments.put(101,new Department(101,"教学部"));
        departments.put(102,new Department(102,"市场部"));
        departments.put(103,new Department(103,"教研部"));
        departments.put(104,new Department(104,"运营部"));
        departments.put(105,new Department(105,"后勤部"));
    }
    //获得所有部门信息
    public Collection<Department> getDepartment(){
        return departments.values();
    }
    //通过id得到部门
    public Department getDepartmentById(Integer id){
        return departments.get(id);
    }
}
```

```java
@Repository
public class EmployeeDao {
    //模拟数据库的信息
    private static Map<Integer, Employee> employees=null;
    //员工所属的部门
    @Autowired
    private DepartmentDao departmentDao;
    static {
        employees=new HashMap<Integer, Employee>();
        employees.put(1001,new Employee(1001,"AA1","1918308849@qq.com",0,new Department(101,"教学部")));
        employees.put(1002,new Employee(1002,"AA2","1918308849@qq.com",1,new Department(102,"市场部")));
        employees.put(1003,new Employee(1003,"AA3","1918308849@qq.com",0,new Department(103,"教研部")));
        employees.put(1004,new Employee(1004,"AA4","1918308849@qq.com",1,new Department(104,"运营部")));
        employees.put(1005,new Employee(1005,"AA5","1918308849@qq.com",0,new Department(105,"后勤部")));
    }
    
    //主键自增
    private static Integer initId=1006;
    
    //增加一个员工
    public void save(Employee employee){
        if(employee.getId()==null){
            employee.setId(initId++);
        }
        employee.setDepartment(departmentDao.
                               getDepartmentById(employee.getDepartment().getId()));
        employees.put(employee.getId(),employee);
    }
    
    //查询全部员工信息
    public Collection<Employee> getAll(){
        return employees.values();
    }
    
    //通过id查询员工
    public Employee getEmployeeById(Integer id){
        return  employees.get(id);
    }
    
    //删除员工通过id
    public void delete(Integer id){
        employees.remove(id);
    }
}
```

创建 `controller` 类：

```java
@Controller
public class EmployeeController {
    @Autowired
    EmployeeDao employeeDao;
    @Autowired
    DepartmentDao departmentDao;
    @RequestMapping("/emps")
    public String list(Model model){
        Collection<Employee> employees=employeeDao.getAll();
        model.addAttribute("emps",employees);
        return "emp/list";
    }
    @GetMapping("/emp")
    public String toAddpage(Model model){
        //查询所有部门的信息
        Collection<Department> departments=departmentDao.getDepartment();
        System.out.println(departments);
        model.addAttribute("departments",departments);
        return "emp/add";
    }
    @PostMapping("/emp")
    public String addEmp(Employee employee){
        //添加操作
        System.out.println("save employee:"+employee);
        employeeDao.save(employee);
        return "redirect:/emps";
    }
    //去员工的修改页面
    @GetMapping("/emp/{id}")
    public String toUpdateEmp(@PathVariable("id")Integer id,Model model){
        //查出原来的数据
        Employee employee=employeeDao.getEmployeeById(id);
        System.out.println(employee);
        model.addAttribute("emp",employee);
        //查询所有部门信息
        Collection<Department> departments=departmentDao.getDepartment();
        System.out.println(departments);
        model.addAttribute("departments",departments);
        return "emp/update";
    }
    @PostMapping("/updateEmp")
    public String updateEmp(Employee employee){
        employeeDao.save(employee);
        return "redirect:/emps";
    }
    @GetMapping("/delemp/{id}")
    public String toDeleteEmp(@PathVariable("id")Integer id){
        employeeDao.delete(id);
        return "redirect:/emps";
    }
    @RequestMapping("/user/logout")
    public String UserLogout(HttpSession session){
        session.invalidate();
        return "redirect:/index.html";
    }
}
```

