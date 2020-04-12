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
        Set<ConstraintViolation<ValidateTestClass>> violations = 
        														validator.validate(this);
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

SpringBoot 默认使用 logback，logback 相当于 log4j 的升级版，做了很多改进，比如更快的运行速度，更充分的测试。

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

在项目中，我们使用Logback，其实只需增加一个配置文件（自定义你的配置）即可

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

这个文件在springboot中默认叫做 `logback-spring.xml`，我们只要新建一个同名文件放在 `resources` 下面， 配置即可生效。

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

在 resources 目录下新建 logback-spring.xml 文件，举例一个简单的需求，如果在项目中我们如果需要指定日志的输出格式以及根据日志级别输出到不同的文件，可以配置如下：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<configuration>
    <!-- 属性文件:在properties文件中找到对应的配置项 -->
    <springProperty scope="context" name="logging.path" source="logging.path"/>
    <contextName>xiaoming</contextName>
    <appender name="consoleLog" class="ch.qos.logback.core.ConsoleAppender">
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <!--格式化输出（配色）：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽				度%msg：日志消息，%n是换行符-->
            <pattern>%yellow(%d{yyyy-MM-dd HH:mm:ss}) %red([%thread]) %highlight(%-5level) %cyan(%logger{50}) - %magenta(%msg) %n
            </pattern>
            <charset>UTF-8</charset>
        </encoder>
    </appender>

    <!--根据日志级别分离日志，分别输出到不同的文件-->
    <appender name="fileInfoLog" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>ERROR</level>
            <onMatch>DENY</onMatch>
            <onMismatch>ACCEPT</onMismatch>
        </filter>
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <pattern>
                %d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{50} - %msg%n
            </pattern>
            <charset>UTF-8</charset>
        </encoder>
        <!--滚动策略-->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--按时间保存日志 修改格式可以按小时、按天、月来保存-->
            <fileNamePattern>${logging.path}/xiaoming.info.%d{yyyy-MM-dd}.log</fileNamePattern>
            <!--保存时长-->
            <MaxHistory>90</MaxHistory>
            <!--文件大小-->
            <totalSizeCap>1GB</totalSizeCap>
        </rollingPolicy>
    </appender>

    <appender name="fileErrorLog" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>ERROR</level>
        </filter>
        <encoder>
            <pattern>
                %d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{50} - %msg%n
            </pattern>
        </encoder>
        <!--滚动策略-->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--路径-->
            <fileNamePattern>${logging.path}/gugibv.error.%d{yyyy-MM-dd}.log</fileNamePattern>
            <MaxHistory>90</MaxHistory>
        </rollingPolicy>
    </appender>
    <root level="info">
        <appender-ref ref="consoleLog"/>
        <appender-ref ref="fileInfoLog"/>
        <appender-ref ref="fileErrorLog"/>
    </root>
</configuration>
```

更详细的使用参考文章：《[看完这个不会配置 logback ，请你吃瓜](https://juejin.im/post/5b51f85c5188251af91a7525)》