# 一、SpringBoot 是什么

## 1、为什么要用 SpringBoot

- Spring Boot makes it easy to create stand-alone, production-grade Spring based Applications that you can "just run".
  - 能快速创建出生产级别的 Spring 应用。

## 2、SpringBoot 优点

- Create stand-alone Spring applications
  - 创建独立 Spring 应用
- Embed Tomcat, Jetty or Undertow directly (no need to deploy WAR files)
  - 内嵌 Web 服务器（默认使用 Tomcat）
- Provide opinionated 'starter' dependencies to simplify your build configuration
  - 自动starter依赖，简化构建配置
- Automatically configure Spring and 3rd party libraries whenever possible
  - 自动配置 Spring 以及第三方功能
- Provide production-ready features such as metrics, health checks, and externalized configuration
  - 提供生产级别的监控、健康检查及外部化配置
- Absolutely no code generation and no requirement for XML configuration
  - 无代码生成、无需编写 XML

> SpringBoot 是整合 Spring 技术栈的一站式框架。
>
> SpringBoot 是简化 Spring 技术栈的快速开发脚手架。

## 3、SpringBoot 缺点

- 迭代快，需要时刻关注版本变化。
- 封装太深，内部原理复杂，不容易精通。

## 4、微服务

[James Lewis and Martin Fowler (2014)](https://martinfowler.com/articles/microservices.html) 提出[微服务](https://martinfowler.com/microservices/)完整概念。

> In short, the **microservice architectural style** is an approach to developing a single application as a **suite of small services**, each **running in its own process** and communicating with **lightweight** mechanisms, often an **HTTP** resource API. These services are **built around business capabilities** and **independently deployable** by fully **automated deployment** machinery. There is a **bare minimum of centralized management** of these services, which may be **written in different programming languages** and use different data storage technologies.-- [James Lewis and Martin Fowler (2014)](https://martinfowler.com/articles/microservices.html)

- 微服务是一种**架构风格**
- 一个应用拆分为**一组小型服务**
- 每个服务运行在自己的进程内，也就是可**独立部署和升级**
- 服务之间使用轻量级 **HTTP 交互**
- 服务围绕业务功能**拆分**
- 可以由**全自动部署**机制独立部署
- **去中心化**，**服务自治**。服务可以使用**不同的语言**、**不同的存储技术**

## 5、分布式

### 分布式的困难

- 配置管理
- 服务发现
- 远程调用
- 负载均衡
- 服务容错
- 服务监控
- 链路追踪
- 日志管理
- 任务调度
- ......

### 分布式的解决

- SpringBoot + SpringCloud

## 6、云原生（Cloud Native）

原生应用如何上云。

### 上云的困难

- 服务自愈
- 弹性伸缩
- 服务隔离
- 自动化部署
- 灰度发布
- 流量治理
- ......

### 上云的解决

- **Docker** 容器化技术
- **Kubernetes** 容器编排，简称 k8s
- **DevOps**，企业 **CI/CD**，构建企业云平台
- 拥抱新一代架构 **Service Mesh** 与 **Serverless**

# 二、基础入门

## 1、环境准备

参照[官方帮助文档](https://docs.spring.io/spring-boot/docs/current/reference/html/getting-started.html#getting-started-installing-the-cli)配置：

- [Java 8](https://www.oracle.com/cn/java/technologies/javase/javase-jdk8-downloads.html) 及以上。
- [Maven 3.3+](https://maven.apache.org/download.cgi)：项目管理工具，可以对 Java 项目进行构建、依赖管理。
  - **pom.xml** 按照官方帮助文档设定。
- [Gradle 6.x](https://gradle.org/releases/)：基于 Apache Ant 和 Apache Maven 概念的项目自动化构建开源工具。它使用一种基于 [Groovy](http://groovy.codehaus.org/) 的特定领域语言(DSL)来声明项目设置，目前也增加了基于 [Kotlin](https://www.runoob.com/kotlin/kotlin-tutorial.html) 语言的 kotlin-based DSL，抛弃了基于 XML 的各种繁琐配置。
- [SpringBoot CLI](https://docs.spring.io/spring-boot/docs/current/reference/html/getting-started.html#getting-started-installing-the-cli)：命令行工具，用于使用 Spring 进行快速原型搭建。它允许你运行 [Groovy](http://groovy.codehaus.org/) 脚本，这意味着你可以使用类 Java 的语法，并且没有那么多的模板代码。
- [IntellIJ IDEA](https://share.weiyun.com/5yRVoYL)： 微云上下载。

### 1.1 Maven 设置

给 Maven 的 **settings.xml** 中添加默认 **JDK** 版本以及默认 **UTF-**8 编码。

```xml
<profiles>
    <profile>
      <id>JDK1.8</id>
      <activation>
        <activeByDefault>true</activeByDefault>
        <jdk>1.8</jdk>
      </activation>
      <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
        <!--编译编码-->
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
      </properties>
    </profile>
</profiles>
```

### 1.2 IDEA 设置

将 Maven 整合进来，IDEA 中 **设置** 和 **新项目设置** 中的 Maven 都要设定。

![Maven 设置](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1596209109700-68b195fc-408c-4236-96a3-dad6503ca597.png)

## 2、HelloWorld

功能：浏览器发送`/hello`请求，服务器接受请求并处理，响应 **Hello World** 字符串。

### 2.1 创建一个 Maven 工程

新建一个名为 **spring-boot-01-helloworld** 的 Maven 工程。

### 2.2 导入 SpringBoot 相关的依赖

**pom.xml** 中加入 SpringBoot 依赖。

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.6.RELEASE</version>
    <!-- 一般依赖最新版本 -->
</parent>
 
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <!-- Web 应用开发场景 -->
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

### 2.3 编写主程序 MainApplication

**src\main\java** 下新建一个 com.atguigu.`HelloWorldMainApplication`类。

```Java
/**
 *  SpringBootApplication 标注一个主程序类，说明这是一个SpringBoot应用
 */
@SpringBootApplication
public class HelloWorldMainApplication {
    public static void main(String[] args) {
        // 启动Spring应用
        SpringApplication.run(HelloWorldMainApplication.class,args);
    }
}
```

### 2.4 编写 Controller、Service

**src\main\java** 下新建一个 `controller.HelloController` 类。

```Java
@Controller // 作为控制器组件注册到容器中
public class HelloController {
    @ResponseBody // 返回结果（json或xml）直接写入 HTTP response body 中，一般在Ajax异步获取数据时使用
    @RequestMapping("/hello") // 映射/hello请求，即该方法处理/hello请求
    public String hello(){
        return "Hello World!";
    }
}
```

### 2.5 运行主程序测试

`Ctrl+F5` 运行 `HelloWorldMainApplication`。

### 2.6 简化部署

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <!-- Maven 插件，可以将应用打包成一个可执行的 jar 包 -->
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <!-- 打包时跳过测试 -->
                <skipTests>true</skipTests>
            </configuration>
        </plugin>
    </plugins>
</build>
```

将这个应用打成 jar 包，直接使用`java -jar`的命令进行执行。

## 3、Hello World探究

### 3.1 POM文件

#### 3.1.1 父项目

项目下的 **pom.xml**：

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <!-- SpringBoot 的版本仲裁中心 -->
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.6.RELEASE</version>
</parent>
```

spring-boot-starter-parent 包下的 spring-boot-starter-parent-**a.b.c**.pom（**a.b.c** 就是上面 pom.xml 中的版本号，本文是 **2.2.6**）：

```xml
<!-- 管理SpringBoot应用里面的所有默认依赖版本 -->
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-dependencies</artifactId>
  <version>2.2.6.RELEASE</version>
  <relativePath>../../spring-boot-dependencies</relativePath>
</parent>
```

以后我们导入默认依赖是不需要写版本号的（没有在 **dependencies** 里面管理的依赖需要声明版本号）。

#### 3.1.2 启动器

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <!-- Web 应用开发场景 -->
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**spring-boot-starter-\***：SpringBoot 官方场景启动器，会导入相关开发场景所需的所有依赖。

***-spring-boot-starter**：自定义的场景启动器。

SpringBoot 将所有的功能场景都**抽取**出来，做成一个个的 **starter**（启动器），只需要在项目里面**引入**这些 **starter**，相关场景的所有依赖都会**导入**进来。要用什么功能就导入什么场景的启动器，引入之后记得点击编辑区右上角的 **m 图标**，来下载这些依赖。

> SpringBoot 支持的[所有场景](https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter)。

### 3.2 主程序类

```java
/**
 *  SpringBootApplication 标注一个主程序类，说明这是一个SpringBoot应用
 */
@SpringBootApplication
public class HelloWorldMainApplication {
    public static void main(String[] args) {
        // 启动Spring应用
        SpringApplication.run(HelloWorldMainApplication.class, args);
    }
}
```

`@SpringBootApplication`：标注在某个类上说明这个类是 SpringBoot 的主配置类，SpringBoot 就应该运行这个类的 `main()` 方法来启动 SpringBoot 应用。

## 4、快速创建 SpringBoot 项目

### 4.1 IDEA：使用 Spring Initializer 快速创建项目

项目类型选择 **Spring Initializr**，修改完**组**（公司名）、**工件**（项目名）和**Java版本**后，选择我们需要的模块（如 Spring Web），向导会联网创建 SpringBoot 项目并下载到本地。

- 主程序已经生成好了，只需编写自己的业务代码。

- resources

  文件夹中目录结构

  - **static**：静态资源， js、css、images。
  - **templates**：模板页面（SpringBoot 默认默认不支持 jsp 页面），可以使用模板引擎（**FreeMarker**、**Thymeleaf**）。
  - **application.properties**：配置文件，可以修改一些默认设置。

### 4.2 Eclipse：使用 STS 快速创建项目

# 三、容器功能

## 1、配置文件

SpringBoot 使用一个全局的配置文件，配置文件名是固定的。

- **application.properties**
- **application.yml** 或 **application.yam**l（建议使用）

配置文件的作用：修改 SpringBoot 自动配置的默认值。

YAML（YAML Ain't Markup Language）

 YAML A Markup Language：是一种**标记语言**

 YAML isn't Markup Language：不是一种标记语言

 Yet another Markup Language：仍是一种标记语言

优点：**没有**额外的定界符，更**轻量**，更**易读**。

## 2、YAML语法

### 2.1 基本语法

- `key: value` -> **value** 前面一定要有**空格**
- 大小写**敏感**
- 使用**缩进**表示层级关系
- 缩进不允许使用tab，只允许**空格**
- 缩进的空格数不重要，只要相同层级的元素**左对齐**即可
- `#` 表示注释

```yaml
server:
    port: 8081
    path: /hello
```

### 2.2 值的写法

#### 2.2.1 字面量

普通的值（**数字**，**字符串**，**布尔值**）

 `k: v`

 字符串默认不用加上单引号或者双引号。

 ""：双引号。**不会转义**字符串里面的特殊字符，特殊字符会作为本身想表示的意思。

 `name: "zhangsan \n lisi"` 输出：**zhangsan** **换行** **lisi**

 ''：单引号。**会转义**特殊字符，特殊字符最终只是一个普通的字符串数据。

 `name: 'zhangsan \n lisi'` 输出：**zhangsan** **\n** **lisi**

#### 2.2.2 对象、Map

（**属性和值**、**键值对**）

 `k: v` 在下一行来写对象的属性和值的关系，注意缩进。

 对象还是 `k: v` 的方式：

```yaml
friends:
    lastName: zhangsan
    age: 20
```

 行内写法：

```yaml
friends: {lastName: zhangsan,age: 18}
```

#### 2.2.3 数组（List、Set）

用 `- 值` 表示数组中的一个元素：

```yaml
pets: - cat - dog - pig
```

行内写法：

```bash
pets: [cat, dog, pig]yaml
```

## 3、配置文件值注入

配置文件 **application.yml**：

```yaml
person:  userName: zhangsan  boss: false  birth: 2019/12/12 20:12:33  age: 18  pet:     name: tomcat    weight: 23.4  interests: [篮球,游泳]  animal:     - jerry    - mario  score:    english:       first: 30      second: 40      third: 50    math: [131,140,148]    chinese: {first: 128,second: 136}  salarys: [3999,4999.98,5999.99]  allPets:    sick:      - {name: tom}      - {name: jerry,weight: 47}    health: [{name: mario,weight: 47}]
```

JavaBean：

```java
/**
 * @ConfigurationProperties：告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定。
 */
@Component // 只有将组件注册到容器中，才能使用容器提供的ConfigurationProperties功能。
@ConfigurationProperties(prefix = "person") // 映射到配置文件中的person属性。
@Data
public class Person {
  private String userName;
  private Boolean boss;
  private Date birth;
  private Integer age;
  private Pet pet;
  private String[] interests;
  private List<String> animal;
  private Map<String, Object> score;
  private Set<Double> salarys;
  private Map<String, List<Pet>> allPets;
}
 
@Data
public class Pet {
  private String name;
  private Double weight;
}
```

自定义的类和配置文件绑定一般没有提示，我们可以导入**配置文件处理器**。

```xml
<!-- 导入配置文件处理器，配置文件进行绑定就会有提示 --><dependency>    <groupId>org.springframework.boot</groupId>    <artifactId>spring-boot-configuration-processor</artifactId>    <optional>true</optional></dependency>
```

### 3.1 配置文件的坑

#### 3.1.1 中文乱码

**application.properties** 配置中文值的时候，读取出来的属性值会出现乱码问题。但是 **application.yml** 不会出现乱码问题。原因是，Spring Boot 是以 **ISO-8859-1** 的编码方式读取 **application.properties** 配置文件。

#### 3.1.2 加载顺序

Java 的 `Properties` 加载属性文件后是无法保证输出的顺序与文件中一致的，因为 `Properties` 是继承自 `Hashtable` 的， **key/value** 都是直接存在 `Hashtable` 中的，而 `Hashtable` 是不保证进出顺序的。

所以如果需要属性保持有序，请使用 **application.yml**。

#### 3.1.3 用户名

如果定义一个键值对 `user.name=xxx` , 这里会读取不到对应写的属性值。为什么呢？SpringBoot 的默认 `StandardEnvironment` 首先将会加载 **systemEnvironment** 作为首个 **PropertySource**。而 **source** 即为 `System.getProperties()`，按照读取顺序，返回 **systemEnvironment** 的值即 `System.getProperty("user.name")`，Mac 电脑会读取自己的登录账号。

### 3.3 `@Value`和`@ConfigurationProperties`比较

| 项目                 | **@ConfigurationProperties** | **@Value** |
| -------------------- | ---------------------------- | ---------- |
| 功能                 | 批量注入配置文件中的属性     | 分别指定   |
| 松散绑定（松散语法） | 支持                         | 不支持     |
| SpEL                 | 不支持                       | 支持       |
| JSR-303数据校验      | 支持                         | 不支持     |
| 复杂类型封装         | 支持                         | 不支持     |

无论配置文件是 **yml** 还是 **properties** 他们都能获取到值。

如果说，我们只是在某个业务逻辑中需要获取一下配置文件中的某项值，使用 `@Value`。

如果说，我们专门编写了一个 JavaBean 来和配置文件进行映射，我们就直接使用`@ConfigurationProperties`。

## 4、输入数据校验

```Java
@Component
@ConfigurationProperties(prefix = "person")
@Validated // 启用数据校验
public class Person {
    /**
     * <bean class="Person">
     *      <property name="lastName" value="字面量/${key}从环境变量、配置文件中获取值/#{SpEL}"></property>
     * <bean/>
     */
 
    // lastName必须是邮箱格式
    @Email
    // @Value("${person.last-name}")
    private String lastName;
    // @Value("#{11*2}")
    private Integer age;
    // @Value("true")
    private Boolean boss;
 
    private Date birth;
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
```

## 5、加载配置文件

### 5.1 `@PropertySource`

加载指定的 **properties** 配置文件。

```Java
/**
 * @ConfigurationProperties：告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定。
 */
@Component // 只有将组件注册到容器中，才能使用容器提供的ConfigurationProperties功能。
@PropertySource(value = {"classpath:person.properties"}) // 指定加载配置文件person.properties。
@ConfigurationProperties(prefix = "person") // 映射到配置文件中的person属性。
//@Validated
public class Person {
    /**
     * <bean class="Person">
     *      <property name="lastName" value="字面量/${key}从环境变量、配置文件中获取值/#{SpEL}"></property>
     * <bean/>
     */
 
   //lastName必须是邮箱格式
   // @Email
    //@Value("${person.last-name}")
    private String lastName;
    //@Value("#{11*2}")
    private Integer age;
    //@Value("true")
    private Boolean boss;
```

### 5.2 `@ImportResource`

为了兼容 Spring 应用，导入 Spring 的 **xml** 配置文件，让配置文件里面的内容生效。

```Java
@ImportResource(locations = {"classpath:beans.xml"}) // 导入Spring的配置文件@SpringBootApplicationpublic class SpringBoot02ConfigApplication {
```

beans.xml：

```xml
<?xml version="1.0" encoding="UTF-8"?><beans xmlns="http://www.springframework.org/schema/beans"       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">    <bean id="helloService" class="com.atguigu.springboot.service.HelloService"></bean></beans>
```

SpringBoot 推荐给容器中添加组件的方式。推荐使用全注解的方式：

1、配置类 `@Configuration` Spring 配置文件

2、使用 `@Bean` 给容器中添加组件

3、组件注解：`@Component`、`@Controller`、`@Service`、`@Repository`

```Java
/**
 * @Configuration指明当前类是一个配置类，替代Spring配置文件。
 * 在配置文件中是使用<bean></bean>标签添加组件。
 */
@Configuration
public class MyAppConfig {
    // 将方法的返回值添加到容器中，容器中这个组件默认的ID就就是方法名
    @Bean
    public HelloService helloService() {
        System.out.println("配置类使用@Bean给容器中添加组件");
        return new HelloService();
    }
}
```

## 6、配置文件占位符

### 6.1 随机数

```Java
${random.uuid}、${random.value}、${random.int}、${random.long}${random.int(10)}、${random.int[1024,65536]}
```

### 6.2 使用占位符

占位符获取之前配置的值，可以使用 `:` 指定默认值。

```properties
person.last-name=王五${random.uuid}person.age=${random.int}person.birth=2020/04/18person.boss=falseperson.maps.k11=v11person.maps.k22=v22person.lists=a,b,cperson.dog.name=${person.hello:hello}_小黑person.dog.age=2
```

## 7、Profile

### 7.1 多 Profile 文件

我们在主配置文件编写的时候，文件名可以是 **application-{profile}.properties/yml/yaml**。

默认使用 **application.properties** 的配置。

### 7.2 yml 支持多文档块方式

```yaml
server:  port: 8081spring:  profiles:    active: dev---# 文档块2server:  port: 8083spring:  profiles: dev---# 文档块3server:  port: 8084spring:  profiles: prod
```

### 7.3 激活指定 Profile

 1、在 **application.properties** 配置文件中指定：

```properties
spring.profiles.active=dev
```

 2、命令行：

```properties
java -jar spring-boot-02-config-0.0.1-SNAPSHOT.jar --spring.profiles.active=dev
```

可以直接在测试的时候，配置传入命令行参数。

 3、虚拟机参数：

```properties
-Dspring.profiles.active=dev
```

## 8、配置文件加载位置

SpringBoot 启动会扫描以下位置的 **application.properties** 或者 **application.yml** 文件作为 SpringBoot 的默认配置文件。

当前工程根目录和 **resources** 文件夹下：

/config/

/

优先级由高到底，高优先级的配置会覆盖低优先级的配置。

SpringBoot 会从这四个位置全部加载主配置文件，共同起作用形成**互补配置**。

我们还可以通过 `spring.config.location` 来改变默认的配置文件位置。

项目打包好以后，我们可以使用**命令行参数**的形式，启动项目的时候来指定配置文件的新位置。

指定配置文件和默认加载的这些配置文件共同起作用形成**互补配置**。

```lua
java -jar spring-boot-02-config-02-0.0.1-SNAPSHOT.jar --spring.config.location=D:/application.properties
```

多个配置用空格分开，*--配置项**=**值*。

SpringBoot 也可以[从外部加载配置](https://docs.spring.io/spring-boot/docs/2.5.3/reference/htmlsingle/#features.external-config)。

## 9、自动配置原理

### 9.1 **引导加载自动配置类**

SpringBoot 启动的时候加载主配置类，主配置类上的 `@SpringBootApplication` 注解开启了自动配置功能 `@EnableAutoConfiguration`。

```less
@SpringBootConfiguration
@EnableAutoConfiguration // 开启自动配置
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
    @Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
```

#### 9.1.1 `@SpringBootConfiguration`

标注在某个类上，表示这是一个 SpringBoot 的配置类。

- `@Configuration`：配置类上使用这个注解。

配置类 → 配置文件，配置类也是容器中的一个组件（`@Component`）。

#### 9.1.2 `@ComponentScan`

指定扫描哪些包，默认扫描主程序 `MainApplication` 所在的包及其子包。

#### 9.1.3`@EnableAutoConfiguration`

开启自动配置功能。

以前我们需要配置的东西，SpringBoot 帮我们自动配置。

```less
    @AutoConfigurationPackage
    @Import({AutoConfigurationImportSelector.class})
    public @interface EnableAutoConfiguration {
```

- `@AutoConfigurationPackage`：自动配置包。

```less
    @Import({Registrar.class})
    public @interface AutoConfigurationPackage {
```

 Spring 的底层注解 `@Import`，给容器中导入一个 `Registrar.class` 类型的组件。

 然后利用 `Registrar` 将主程序 `MainApplication` 所在的包及其子包里面的所有组件扫描并配置到 Spring 容器。

- `@Import({AutoConfigurationImportSelector.class})`：导入组件。

  - 利用 `AutoConfigurationImportSelector.getAutoConfigurationEntry(AnnotationMetadata annotationMetadata)` 给容器中导入一些组件；

  - 可以查看 `AutoConfigurationImportSelector.selectImports(AnnotationMetadata annotationMetadata)` 方法的内容；

  - 获取候选的配置项。

    ```Java
    List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);
    ```

  - 加载配置

    ```Java
    List<String> configurations = SpringFactoriesLoader.loadFactoryNames(this.getSpringFactoriesLoaderFactoryClass(), this.getBeanClassLoader());
    ```

扫描所有 jar 包类路径下 **META-INF/spring.factories**，把扫描到的这些文件的内容包装成 **properties** 对象。

从 **properties** 中获取到 `EnableAutoConfiguration` 属性下的类全限定名列表，然后把他们添加到容器中。

```properties
# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
...
```

每一个这样的 `xxxAutoConfiguration` 类都是容器中的一个组件，都加入到容器中，用他们来做自动配置。

### 9.2 按需开启自动配置项

#### 9.2.1 `@Conditional`

作用：必须是 `@Conditional` 指定的条件成立，才给容器中添加组件，配置里面的所有内容才生效。

| @Conditional扩展注解                                         | 作用（判断是否满足当前指定条件）                 |
| ------------------------------------------------------------ | ------------------------------------------------ |
| [@ConditionalOnJava](https://www.yuque.com/ConditionalOnJava) | 系统的java版本是否符合要求                       |
| [@ConditionalOnBean](https://www.yuque.com/ConditionalOnBean) | 容器中存在指定Bean；                             |
| @ConditionalOnMissingBean                                    | 容器中不存在指定Bean；                           |
| @ConditionalOnExpression                                     | 满足SpEL表达式指定                               |
| [@ConditionalOnClass](https://www.yuque.com/ConditionalOnClass) | 系统中有指定的类                                 |
| @ConditionalOnMissingClass                                   | 系统中没有指定的类                               |
| @ConditionalOnSingleCandidate                                | 容器中只有一个指定的Bean，或者这个Bean是首选Bean |
| @ConditionalOnProperty                                       | 系统中指定的属性是否有指定的值                   |
| @ConditionalOnResource                                       | 类路径下是否存在指定资源文件                     |
| @ConditionalOnWebApplication                                 | 当前是web环境                                    |
| @ConditionalOnNotWebApplication                              | 当前不是web环境                                  |
| [@ConditionalOnJndi](https://www.yuque.com/ConditionalOnJndi) | JNDI存在指定项                                   |

**自动配置类必须在一定的条件下才能生效。**

我们怎么知道哪些自动配置类生效？

我们可以通过启用 `debug=true` 属性，来让控制台打印自动配置报告，这样我们就可以很方便地知道哪些自动配置类生效。

```Java
=========================
AUTO-CONFIGURATION REPORT
=========================
 
 
Positive matches: // 自动配置类启用的
-----------------
 
   DispatcherServletAutoConfiguration matched:
      - @ConditionalOnClass found required class 'org.springframework.web.servlet.DispatcherServlet'; @ConditionalOnMissingClass did not find unwanted class (OnClassCondition)
      - @ConditionalOnWebApplication (required) found StandardServletEnvironment (OnWebApplicationCondition)
        
    
Negative matches: // 没有启动，没有匹配成功的自动配置类
-----------------
 
   ActiveMQAutoConfiguration:
      Did not match:
         - @ConditionalOnClass did not find required classes 'javax.jms.ConnectionFactory', 'org.apache.activemq.ActiveMQConnectionFactory' (OnClassCondition)
 
   AopAutoConfiguration:
      Did not match:
         - @ConditionalOnClass did not find required classes 'org.aspectj.lang.annotation.Aspect', 'org.aspectj.lang.reflect.Advice' (OnClassCondition)
```

### 9.3 Spring MVC 自动配置

以下是 SpringBoot 对 SpringMVC 的默认配置（`WebMvcAutoConfiguration`）：

- Inclusion of `ContentNegotiatingViewResolver` and `BeanNameViewResolver` beans.

  - 自动配置了 `ViewResolver`（视图解析器）：根据方法的返回值得到视图对象（`View`），视图对象决定如何渲染（转发？重定向？）.
  - `ContentNegotiatingViewResolver`：组合所有的视图解析器的。
  - 可以自己给容器中添加一个视图解析器（`@Bean`，`@Component`），SpringBoot 会自动地将其组合进来。

- Support for serving static resources, including support for WebJars (see below).

  - 静态资源文件夹路径，包括 `webjars`。

- Static `index.html` support.

  - 静态首页访问（欢迎页）。

- Custom `Favicon` support (see below).

  - 网站图标支持，即 **favicon.ico**。

- 自动注册了 of `Converter`, `GenericConverter`, `Formatter` beans.

  - `Converter<S, T>`：转换器接口，将源类型 `S` 转换为目标类型 `T`。

  - `GenericConverter`：通用转换器接口，用于两个以上类型之间的转换。

  - `Formatter<T>` 格式化器接口，格式化成目标类型 `T`。

    ```Java
    @Bean
    @ConditionalOnProperty(prefix = "spring.mvc", name = "date-format") // 在文件中配置日期格式化的规则
    public Formatter<Date> dateFormatter() {
        return new DateFormatter(this.mvcProperties.getDateFormat()); // 日期格式化组件
    }
    ```

    可以自己给容器中添加一个格式化器转换器。

- Support for `HttpMessageConverters` (see below).

  - `HttpMessageConverter`：SpringMVC 用来转换 `HTTP` 请求和响应的。
  - `HttpMessageConverters` 是从容器中确定的，获取所有的 `HttpMessageConverter`，然后看哪一个转换器能处理。
  - 可以自己给容器中添加一个 `HttpMessageConverter`。

- Automatic registration of `MessageCodesResolver` (see below).

  - 定义错误代码生成规则。

- Automatic use of a `ConfigurableWebBindingInitializer` bean (see below).

  - 可以自己给容器中添加一个 `ConfigurableWebBindingInitializer` 来替换默认的。

### 9.4 扩展SpringMVC

编写一个配置类（`@Configuration`），是 `WebMvcConfigurer` 类型。不能标注`@EnableWebMvc` 。

既保留了所有的自动配置，也能用我们扩展的配置。

```Java
// 使用 WebMvcConfigurer 可以扩展 SpringMVC 的功能
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        // 浏览器发送 /atguigu 请求来到 success 页面
        registry.addViewController("/atguigu").setViewName("success");
    }
}
```

原理：

1. `WebMvcConfigurer` 是 SpringMVC 的自动配置类;
2. 在做其他自动配置时会导入；

```Java
@Import(EnableWebMvcConfiguration.class)
@Configuration
public static class EnableWebMvcConfiguration extends DelegatingWebMvcConfiguration {
```

父类 `DelegatingWebMvcConfiguration`：

```Java
@Configuration(proxyBeanMethods = false)
public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
    private final WebMvcConfigurerComposite configurers = new WebMvcConfigurerComposite();
    // 从容器中获取所有的 WebMvcConfigurer。
    @Autowired(required = false)
    public void setConfigurers(List<WebMvcConfigurer> configurers) {
        if (!CollectionUtils.isEmpty(configurers)) {
            this.configurers.addWebMvcConfigurers(configurers);
            // 一个参考实现：将所有 WebMvcConfigurer 相关配置一起调用。
            // @Override
            // protected void addViewControllers(ViewControllerRegistry registry) {
            //     this.configurers.addViewControllers(registry);
            // }
        }
    }
```

1. 容器中所有的 `WebMvcConfigurer` 都会同时起作用；
2. 我们的配置类也会被调用。

效果：SpringMVC 的自动配置和我们的扩展配置都会起作用。

### 9.5 全面接管 SpringMVC

不使用 SpringBoot 对 SpringMVC 的自动配置，而是使用自定义配置。

只要配置类中添加 `@EnableWebMvc` 即可：

```Java
// 使用 WebMvcConfigurerAdapter 可以来扩展 SpringMVC 的功能
@EnableWebMvc
@Configuration
public class MyMvcConfig extends WebMvcConfigurerAdapter {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        // super.addViewControllers(registry);
        //浏览器发送 /atguigu 请求来到 success 页面
        registry.addViewController("/atguigu").setViewName("success");
    }
}
```

**原理：**

为什么使用 `@EnableWebMvc` 自动配置就失效了。

1. `@EnableWebMvc` 导入了 `DelegatingWebMvcConfiguration` 组件；

```java
@Import(DelegatingWebMvcConfiguration.class)
public @interface EnableWebMvc {
```

1. ```
   DelegatingWebMvcConfiguration
   ```

    

   继承了

    

   ```
   WebMvcConfigurationSupport
   ```

    

   类；

   - 把所有系统中的 `WebMvcConfigurer` 拿过来，所有功能的定制都是这些 `WebMvcConfigurer` 合起来一起生效的；
   - 自动配置了一些非常底层的组件，`RequestMappingHandlerMapping`、这些组件依赖的组件都是从容器中获取。

```java
@Configuration
public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
```

1. `WebMvcAutoConfiguration` 只有当容器中没有 `WebMvcConfigurationSupport` 组件的时候才生效；

```java
@Configuration(proxyBeanMethods = false)
@ConditionalOnWebApplication(type = Type.SERVLET)
@ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
// 容器中没有这个组件的时候，这个自动配置类才生效。
@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
@AutoConfigureAfter({ DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class,
        ValidationAutoConfiguration.class })
public class WebMvcAutoConfiguration {
```

1. 而第 2 步中，`@EnableWebMvc` 已经将 `WebMvcConfigurationSupport` 组件导入进来了。
2. 导入的 `WebMvcConfigurationSupport` 只是 SpringMVC 最基本的功能。

### 9.6 修改默认配置

可配置的[属性列表](https://docs.spring.io/spring-boot/docs/2.5.3/reference/htmlsingle/#application-properties)。

1. SpringBoot 在自动配置很多组件的时候，先看容器中有没有用户自己配置的（`@Bean`、`@Component`）如果有就用用户配置的，如果没有，才自动配置。如果有些组件可以有多个（`ViewResolver`）将用户配置的和自己默认的组合起来。
2. 在 SpringBoot 中会有非常多的 `xxxConfigurer` 帮助我们进行**扩展**配置；
3. 在 SpringBoot 中会有非常多的 `xxxCustomizer` 帮助我们进行**定制**配置。

### 9.7 总结

- SpringBoot 先加载所有的自动配置类 `xxxAutoConfiguration`。
- 每个自动配置类按照条件生效，默认都会绑定 `xxxProperties` 类，`xxxProperties` 类和对应配置文件进行了绑定。
- 定制化配置：
  - 用户直接使用 `@Bean`、`@Component` 替换底层的组件；
  - 用户去看这个组件是获取的配置文件什么值就去修改。

场景 **starter** -> `xxxAutoConfiguration` -> 导入 `xxx` 组件 -> 绑定 `xxxProperties` -> 绑定配置文件项（application.properties）

### 9.8 最佳实践

- 引入场景依赖；

  - [Starters](https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter)。

- 查看自动配置了哪些（选做）；

  - 自己分析，引入场景对应的自动配置一般都生效了；
  - 配置文件中 `debug=true` 开启自动配置报告：**Negative**（不生效） \ **Positive**（生效）。

- 是否需要修改配置项；

  - 参照文档修改配置项。
    - [Common Application Properties](https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html#common-application-properties)；
    - 自己分析，`xxxProperties` 配置类绑定了哪个配置文件；

- 自定义加入或者替换组件；

  - 编写自定义的配置类 `xxxConfiguration` + `@Bean`、`@Component` 替换、增加容器中默认组件。

    - Web应用：编写一个配置类实现 `WebMvcConfigurer` 即可定制化 **Web** 功能 + `@Bean` 给容器中再扩展一些组件。

    ```Java
    @Configuration
    public class AdminWebConfig implements WebMvcConfigurer {
        @Bean
        public MybatisPlusInterceptor mybatisPlusInterceptor() {
            MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
            // 分页拦截器
            PaginationInnerInterceptor paginationInnerInterceptor = new PaginationInnerInterceptor(DbType.MYSQL);
            // 跳回首页
            paginationInnerInterceptor.setOverflow(true);
            // 每页不受限制
            paginationInnerInterceptor.setMaxLimit(-1L);
            interceptor.addInnerInterceptor(paginationInnerInterceptor);
            return interceptor;
        }
    }
    ```

- 自定义器 `xxxCustomizer`；

- ......

### 9.9 示例

以 `HttpEncodingAutoConfiguration`（Http 编码自动配置）为例解释自动配置原理。

```java
@Configuration(proxyBeanMethods = false) // 表示这是一个配置类，关闭 bean 代理（每次获取一个新的 bean，相当于 prototype）
@EnableConfigurationProperties(HttpProperties.class) // 将配置文件中对应的值和HttpProperties绑定起来，并把HttpProperties加入到容器中。
@ConditionalOnWebApplication(type = ConditionalOnWebApplication.Type.SERVLET) // 仅对 Web 应用生效。
@ConditionalOnClass(CharacterEncodingFilter.class) // CharacterEncodingFilter 类存在时生效。SpringMVC中进行乱码解决的过滤器。
@ConditionalOnProperty(prefix = "spring.http.encoding", value = "enabled", matchIfMissing = true)
// 判断配置文件中是否存在某个配置 spring.http.encoding=enabled。
// 即使我们不配置，pring.http.encoding.enabled=true，也是默认生效的。
public class HttpEncodingAutoConfiguration {
    // 它已经和SpringBoot的配置文件映射了。
    private final HttpProperties.Encoding properties;
 
    // 只有一个有参构造器的情况下，参数的值就会从容器中拿。
    public HttpEncodingAutoConfiguration(HttpProperties properties) {
        this.properties = properties.getEncoding();
    }
 
    @Bean // 给容器中添加一个组件，这个组件的某些值需要从 properties 中获取。
    @ConditionalOnMissingBean // 容器中没有这个组件时才生效。
    public CharacterEncodingFilter characterEncodingFilter() {
        CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
        filter.setEncoding(this.properties.getCharset().name());
        filter.setForceRequestEncoding(this.properties.shouldForce(Type.REQUEST));
        filter.setForceResponseEncoding(this.properties.shouldForce(Type.RESPONSE));
        return filter;
    }
```

# 四、日志

## 1、日志框架

小张要开发一个大型系统。

1. System.out.println("")。将关键数据打印在控制台。去掉？写在一个文件？
2. 框架来记录系统的一些运行时信息。日志框架 。**zhanglogging.jar**。
3. 高大上的几个功能？异步模式？自动归档？xxx？**zhanglogging-good.jar。**
4. 将以前框架卸下来？换上新的框架，重新修改之前相关的API。**zhanglogging-prefect.jar**。
5. JDBC---数据库驱动。

写了一个统一的接口层，日志门面（日志的一个抽象层）：**logging-abstract.jar**。

给项目中导入具体的日志实现就行了。我们之前的日志框架都是实现的抽象层。

**市面上的日志框架。**

JUL、JCL、Jboss-logging、logback、log4j、log4j2、SLF4j...

| 日志门面 （日志的抽象层）                                    | 日志实现                                             |
| ------------------------------------------------------------ | ---------------------------------------------------- |
| JCL（Jakarta Commons Logging） 、**SLF4j**（Simple Logging Facade for Java）、Jboss-logging | Log4j、JUL（java.util.logging）、Log4j2、**Logback** |

左边选一个门面（抽象层）、右边来选一个实现。

Spring 框架默认是用 **JCL**。

SpringBoot 选用 **SLF4j** 和 **logback**。

## 2、SLF4j 使用

### 2.1 如何在系统中使用 SLF4j

[https://www.slf4j.org](https://www.slf4j.org/)

以后开发的时候，日志记录方法的调用，不应该来直接调用日志的实现类，而是调用日志抽象层里面的方法。

给系统里面导入 **slf4j** 的抽象层 jar 和 **logback** 的实现层 jar。

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

图示：

![使用 SLF4J](https://cdn.nlark.com/yuque/0/2020/png/1304385/1596209109663-f4b8c543-d2f8-4274-97d5-32a1fb8d11d9.png)

每一个日志的实现框架都有自己的配置文件。

### 2.2 遗留问题

A系统（slf4j + logback）: Spring（commons-logging）、Hibernate（jboss-logging）、MyBatis...

统一日志记录，即使是别的框架和我一起统一使用 **slf4j** 进行输出？

![遗留系统使用 SLF4J](https://cdn.nlark.com/yuque/0/2020/png/1304385/1596209109705-151151c4-614d-461d-afa6-fa09014cc32e.png?x-oss-process=image%2Fresize%2Cw_1500)

**如何让系统中所有的日志都统一到 slf4j？**

1. 将系统中其他日志框架先排除出去。
2. 用中间包来替换原有的日志框架。
3. 我们导入 **slf4j** 其他的实现。

## 3、SpringBoot 日志关系

SpringBoot 使用它来做日志功能。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-logging</artifactId>
</dependency>
```

底层依赖关系：

![日志关系](https://cdn.nlark.com/yuque/0/2020/png/1304385/1596209109699-16aee8c6-ba3f-498b-8577-34a6f1bb0d0d.png)

总结：

 1. SpringBoot 底层也是使用 **slf4j** + **logback** 的方式进行日志记录。

 2. SpringBoot 也把其他的日志都替换成了**slf4j**。

 3. 中间转换包？

```java
@SuppressWarnings("rawtypes")
public abstract class LogFactory {
    static String UNSUPPORTED_OPERATION_IN_JCL_OVER_SLF4J = "http://www.slf4j.org/codes.html#unsupported_operation_in_jcl_over_slf4j";
 
    static LogFactory logFactory = new SLF4JLogFactory();
```

![日志中间转换包](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1596209109682-d1c29993-26da-4800-a535-4ac8fbdf3171.png)

 4. 如果我们要引入其他框架，一定要把这个框架的默认日志依赖移除掉。

 Spring 框架用的是 **commons-logging（JCL）**。

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <exclusions>
        <exclusion>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

SpringBoot 能自动适配所有的日志，而且底层使用 **slf4j** + **logback** 的方式记录日志，引入其他框架的时候，只需要把这个框架依赖的日志框架排除掉即可。

## 4、日志使用

### 4.1 默认配置

SpringBoot 默认帮我们配置好了日志。

```java
// 记录器
Logger logger = LoggerFactory.getLogger(getClass());
@Test
public void contextLoads() {
    // System.out.println();
 
    // 日志的级别。
    // 由低到高：trace < debug < info < warn < error
    // 可以调整输出的日志级别，日志就只会在这个级别以以后的高级别生效
    logger.trace("这是trace日志...");
    logger.debug("这是debug日志...");
    // SpringBoot 默认给我们使用的是 info 级别的
    logger.info("这是info日志...");
    logger.warn("这是warn日志...");
    logger.error("这是error日志...");
}
日志输出格式：
    %d表示日期时间，
    %thread表示线程名，
    %-5level：级别从左显示5个字符宽度
    %logger{50} 表示logger名字最长50个字符，否则按照句点分割。 
    %msg：日志消息，
    %n是换行符
-->
%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n
```

SpringBoot 修改日志的默认配置：

```properties
logging.level.com.atguigu=trace
 
# 不指定路径，在当前项目下生成 springboot.log 日志。
#logging.file.path=
 
# 可以指定完整的路径。
#logging.file.name=D:/springboot.log
 
# 在当前磁盘的根路径下创建 /spring/log 文件夹，使用 spring.log 作为日志文件。
logging.file.path=/spring/log
 
# 在控制台输出的日志格式
logging.pattern.console=%d{yyyy-MM-dd} [%thread] %-5level %logger{50} - %msg%n
 
# 指定文件中输出的日志格式
logging.pattern.file=%d{yyyy-MM-dd} === [%thread] === %-5level === %logger{50} === %msg%n
```

| logging.file | logging.path | Example  | Description                            |
| ------------ | ------------ | -------- | -------------------------------------- |
| (none)       | (none)       |          | 只在控制台输出                         |
| 指定文件名   | (none)       | my.log   | 输出日志到 **my.log** 文件             |
| (none)       | 指定目录     | /var/log | 输出到指定目录的 **spring.log** 文件中 |

### 4.2 指定配置

给类路径下放上每个日志框架自己的配置文件即可，SpringBoot 就不使用它默认配置的了。

| Logging System          | Customization                                                |
| ----------------------- | ------------------------------------------------------------ |
| Logback                 | `logback-spring.xml`, `logback-spring.groovy`, `logback.xml` or `logback.groovy` |
| Log4j2                  | `log4j2-spring.xml` or `log4j2.xml`                          |
| JDK (Java Util Logging) | `logging.properties`                                         |

**logback.xml**：直接就被日志框架识别了。

**logback-spring.xml**：日志框架就不直接加载日志的配置项，由 SpringBoot 解析日志配置，可以使用 SpringBoot 的高级 **profile** 功能。

```xml
<springProfile name="dev">
    <!-- 可以指定某段配置只在某个环境下生效 -->
</springProfile>
```

如：

```xml
<appender name="stdout" class="ch.qos.logback.core.ConsoleAppender">
    <!--
    日志输出格式：
        %d表示日期时间，
        %thread表示线程名，
        %-5level：级别从左显示5个字符宽度
        %logger{50} 表示logger名字最长50个字符，否则按照句点分割。 
        %msg：日志消息，
        %n是换行符
    -->
    <layout class="ch.qos.logback.classic.PatternLayout">
        <springProfile name="dev">
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} ----> [%thread] ---> %-5level %logger{50} - %msg%n</pattern>
        </springProfile>
        <springProfile name="!dev">
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} ==== [%thread] ==== %-5level %logger{50} - %msg%n</pattern>
        </springProfile>
    </layout>
</appender>
```

如果使用 **logback.xml** 作为日志配置文件，还要使用 Profile 功能，会有以下错误

```bash
no applicable action for [springProfile]
```

## 5、切换日志框架

可以按照 **slf4j** 的日志适配图，进行相关的切换。

**slf4j** + **log4j** 的方式：

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <exclusions>
    <exclusion>
      <artifactId>logback-classic</artifactId>
      <groupId>ch.qos.logback</groupId>
    </exclusion>
    <exclusion>
      <artifactId>log4j-over-slf4j</artifactId>
      <groupId>org.slf4j</groupId>
    </exclusion>
  </exclusions>
</dependency>
 
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
</dependency>
```

切换为 **log4j2**：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <artifactId>spring-boot-starter-logging</artifactId>
            <groupId>org.springframework.boot</groupId>
        </exclusion>
    </exclusions>
</dependency>
 
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
```

# 五、Web 开发

## 1、简介

1. 创建 SpringBoot 应用，选中我们需要的 **starter** 模块。
2. SpringBoot 已经默认将这些场景配置好了，只需要在配置文件中指定少量配置就可以运行起来
3. 自己编写业务代码。

## 2、简单功能分析

### 2.1 静态资源访问

> 静态资源：`css`、`jss`、`html`、`jpg` 等资源文件，用户多次访问都是获取同样的资源，可以被浏览器缓存。

#### 2.1.1 静态资源访问目录

只要静态资源放在类路径下： called `/static` (or `/public` or `/resources` or `/META-INF/resources`

```yaml
"classpath:/META-INF/resources/", 
"classpath:/resources/",
"classpath:/static/", 
"classpath:/public/" 
"/"：当前项目的根路径
```

访问当前项目根路径 `/` + `静态资源名` ，就可以获取到对应的静态资源。

访问地址：http://localhost:8080/**hello.jpg**，去静态资源目录下找 **hello.jpg**。

原理： **静态映射/\****。

请求进来，先去找 Controller 看能不能处理。不能处理的所有请求又都交给静态资源处理器。静态资源也找不到则响应 404 页面。

#### 2.1.2 静态资源访问前缀

默认无前缀。

```yaml
spring:
  mvc:
    static-path-pattern: /res/**
```

访问当前项目根路径 `/` + `static-path-pattern` + `静态资源名` ，就可以获取到对应的静态资源。

访问地址：http://localhost:8080/**res**/**hello.jpg**，去静态资源文件夹里面找 **hello.jpg**。

2.1.3 webjar 映射

所有 `/webjars/**` 请求，都会去 `classpath:/META-INF/resources/webjars/` 下找资源。

**webjars**：以 jar 包的方式引入静态资源。

[http://www.webjars.org](http://www.webjars.org/)

```xml
<!-- 引入jquery-webjar -->
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>3.5.0</version>
</dependency>
```

![webjars](https://cdn.nlark.com/yuque/0/2020/png/1304385/1596209109671-00442641-a167-4d1d-affc-47826e35fa07.png)

访问地址：http://localhost:8080/webjars/**jquery/3.5.0/jquery.js**，后面地址要按照依赖里面的包路径，就可以获取到对应的静态资源。

#### 2.1.3 修改静态资源路径

改变默认的静态资源前缀和目录：

```yaml
spring:
  mvc:
    static-path-pattern: /res/** # 修改前缀
  resources:
    static-locations: [classpath:/haha/] # 修改目录
```

访问地址：http://localhost:8080/**res**/**haha**/**hello.jpg**，去静态资源目录下找 **hello.jpg**。

### 2.2 欢迎页

静态资源路径下的所有 **index.html** 页面，被 `/**` 映射。

- 可以配置静态资源访问目录；
- 但是不能配置静态资源访问前缀，否则导致 **index.html** 不能被默认访问；

```yaml
spring:
#  mvc:
#    static-path-pattern: /res/** # 这个会导致 Welcome Page 功能失效
```

访问地址：http://localhost:8080/，就会去静态资源目录下找 **index.html** 页面。

Controller 能处理 `/index` 请求。

### 2.3 自定义 Favicon

将 `favicon.ico` 放在静态资源目录下即可。

跟「欢迎页」一样，不能配置静态资源访问前缀，否则不能访问自定义图标。

### 2.4 静态资源配置原理

- SpringBoot 启动默认加载 `xxxAutoConfiguration` 类（自动配置类）。
- SpringMVC 功能的自动配置类 `WebMvcAutoConfiguration` 生效。

```Java
@Configuration(proxyBeanMethods = false)
@ConditionalOnWebApplication(type = Type.SERVLET)
@ConditionalOnClass({Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class})
@ConditionalOnMissingBean({WebMvcConfigurationSupport.class})
@AutoConfigureOrder(-2147483638)
@AutoConfigureAfter({DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class, ValidationAutoConfiguration.class})
public class WebMvcAutoConfiguration {
```

- 查看 `WebMvcConfigurer` 接口的实现类 `WebMvcAutoConfigurationAdapter`，看看给容器中配置了什么

  ```Java
  @Configuration(proxyBeanMethods = false)
  @Import({WebMvcAutoConfiguration.EnableWebMvcConfiguration.class})
  @EnableConfigurationProperties({WebMvcProperties.class, ResourceProperties.class, WebProperties.class})
  @Order(0)
  public static class WebMvcAutoConfigurationAdapter implements WebMvcConfigurer, ServletContextAware {
  ```

- 配置类和配置文件的相关属性 `spring.xxx` 进行了绑定。

  - `WebMvcProperties` 绑定 `spring.mvc`

  - `ResourceProperties`（已过时） 绑定 `spring.resources`

    ```java
    @Deprecated
    @ConfigurationProperties(prefix = "spring.resources", ignoreUnknownFields = false)
    public class ResourceProperties extends Resources {
    ```

  - `WebProperties` 绑定 `spring.web`

    ```java
    @ConfigurationProperties("spring.web")
    public class WebProperties {
    	public static class Resources {
     
    		private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { "classpath:/META-INF/resources/",
    				"classpath:/resources/", "classpath:/static/", "classpath:/public/" };
     
    		/**
    		 * Locations of static resources. Defaults to classpath:[/META-INF/resources/,
    		 * /resources/, /static/, /public/].
    		 */
    		private String[] staticLocations = CLASSPATH_RESOURCE_LOCATIONS;
    ```

#### 2.4.1 配置唯一的有参类构造函数

org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration.`WebMvcAutoConfigurationAdapter#WebMvcAutoConfigurationAdapter()`

```Java
// 有参构造器所有参数的值都会从容器中获取
// ResourceProperties：获取和 spring.resources 绑定的所有的值的对象
// WebMvcProperties：获取和 spring.mvc 绑定的所有的值的对象
// ListableBeanFactory：Spring 的 beanFactory
// HttpMessageConverters：找到所有的 HttpMessageConverters
// ResourceHandlerRegistrationCustomizer：找到资源处理器的自定义器。
// DispatcherServletPath：配置访问路径
// ServletRegistrationBean：给应用注册 Listener -> Fliter -> Servlet
public WebMvcAutoConfigurationAdapter (ResourceProperties resourceProperties, WebMvcProperties mvcProperties,
                                       ListableBeanFactory beanFactory, ObjectProvider<HttpMessageConverters> messageConvertersProvider,
                                       ObjectProvider<ResourceHandlerRegistrationCustomizer> resourceHandlerRegistrationCustomizerProvider,
                                       ObjectProvider<DispatcherServletPath> dispatcherServletPath,
                                       ObjectProvider<ServletRegistrationBean<?>> servletRegistrations) {
    this.resourceProperties = resourceProperties;
    this.mvcProperties = mvcProperties;
    this.beanFactory = beanFactory;
    this.messageConvertersProvider = messageConvertersProvider;
    this.resourceHandlerRegistrationCustomizer = resourceHandlerRegistrationCustomizerProvider.getIfAvailable ();
    this.dispatcherServletPath = dispatcherServletPath;
    this.servletRegistrations = servletRegistrations;
}
```

#### 2.4.2 静态资源和 webjars 的处理规则

org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration.`WebMvcAutoConfigurationAdapter#addResourceHandlers()`

```Java
@Override
public void addResourceHandlers (ResourceHandlerRegistry registry) {
    if (!this.resourceProperties.isAddMappings ()) {
        logger.debug ("Default resource handling disabled");
        return;
    }
    Duration cachePeriod = this.resourceProperties.getCache ().getPeriod ();
    CacheControl cacheControl = this.resourceProperties.getCache ().getCachecontrol ().toHttpCacheControl ();
    // webjars 的处理规则
    if (!registry.hasMappingForPattern ("/webjars/**")) {
        customizeResourceHandlerRegistration (registry.addResourceHandler ("/webjars/**")
                                              .addResourceLocations ("classpath:/META-INF/resources/webjars/")
                                              .setCachePeriod (getSeconds (cachePeriod)).setCacheControl (cacheControl));
    }
    // 静态资源的处理规则
    String staticPathPattern = this.mvcProperties.getStaticPathPattern ();
    if (!registry.hasMappingForPattern (staticPathPattern)) {
        customizeResourceHandlerRegistration (registry.addResourceHandler (staticPathPattern)
                                              .addResourceLocations (getResourceLocations (this.resourceProperties.getStaticLocations ()))
                                              .setCachePeriod (getSeconds (cachePeriod)).setCacheControl (cacheControl));
    }
}
```

#### 2.4.3 欢迎页的处理规则

org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration.`EnableWebMvcConfiguration#welcomePageHandlerMapping()`

```java
// 配置欢迎页映射
@Bean
public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext applicationContext,
                                                           FormattingConversionService mvcConversionService, ResourceUrlProvider mvcResourceUrlProvider) {
    WelcomePageHandlerMapping welcomePageHandlerMapping = new WelcomePageHandlerMapping(
        new TemplateAvailabilityProviders(applicationContext), applicationContext, getWelcomePage(),
        this.mvcProperties.getStaticPathPattern());
    welcomePageHandlerMapping.setInterceptors(getInterceptors(mvcConversionService, mvcResourceUrlProvider));
    welcomePageHandlerMapping.setCorsConfigurations(getCorsConfigurations());
    return welcomePageHandlerMapping;
}
 
// WelcomePageHandlerMapping 类的唯一有参构造函数
WelcomePageHandlerMapping(TemplateAvailabilityProviders templateAvailabilityProviders,
                          ApplicationContext applicationContext, Resource welcomePage, String staticPathPattern) {
    if (welcomePage != null && "/**".equals(staticPathPattern)) {
        // 要用欢迎页，必须使用默认的静态资源目录 /**
        logger.info("Adding welcome page: " + welcomePage);
        setRootViewName("forward:index.html");
    }
    else if (welcomeTemplateExists(templateAvailabilityProviders, applicationContext)) {
        // 调用 Controller 处理 /index 请求
        logger.info("Adding welcome page template: index");
        setRootViewName("index");
    }
}
```

#### 2.4.4 Favicon 的处理规则

```Java
// 配置喜欢的图标
@Configuration
@ConditionalOnProperty(value = "spring.mvc.favicon.enabled", matchIfMissing = true)
public static class FaviconConfiguration {
 
    private final ResourceProperties resourceProperties;
 
    public FaviconConfiguration(ResourceProperties resourceProperties) {
        this.resourceProperties = resourceProperties;
    }
 
    @Bean
    public SimpleUrlHandlerMapping faviconHandlerMapping() {
        SimpleUrlHandlerMapping mapping = new SimpleUrlHandlerMapping();
        mapping.setOrder(Ordered.HIGHEST_PRECEDENCE + 1);
        // 映射所有 **/favicon.ico 
        mapping.setUrlMap(Collections.singletonMap("**/favicon.ico",
                faviconRequestHandler()));
        return mapping;
    }
 
    @Bean
    public ResourceHttpRequestHandler faviconRequestHandler() {
        ResourceHttpRequestHandler requestHandler = new ResourceHttpRequestHandler();
        requestHandler
                .setLocations(this.resourceProperties.getFaviconLocations());
        return requestHandler;
    }
}
```

新版本没有这个类了，找到了下面这个类：

```Java
public enum StaticResourceLocation {
	CSS("/css/**"),
	JAVA_SCRIPT("/js/**"),
	IMAGES("/images/**"),
	WEB_JARS("/webjars/**"),
	FAVICON("/favicon.*", "/*/icon-*");
 
	private final String[] patterns;
 
	StaticResourceLocation(String... patterns) {
		this.patterns = patterns;
	}
 
	public Stream<String> getPatterns() {
		return Arrays.stream(this.patterns);
	}
}
```

## 3、请求响应参数处理

### 3.1 请求映射

#### 3.1.1 REST 使用

- `@xxxMapping`（`@GetMapping`、`@PostMapping`、`@PutMapping`、`@DeleteMapping`，`@PatchMapping`是对 Put 的补充，区别是 Patch 是部分更新，Put 是全部更新，这些注解都是 Spring4.3 引入的）；
- **REST** 风格支持（使用 **HTTP** 请求方式动词来表示对资源的操作）

| 操作     | 以前                     | 现在（REST 风格）       |
| -------- | ------------------------ | ----------------------- |
| 获取用户 | /getUser 的 GET 请求     | /user/id 的 GET 请求    |
| 保存用户 | /saveUser 的 POST 请求   | /user/id 的 POST 请求   |
| 修改用户 | /editUser 的 POST 请求   | /user/id 的 PUT 请求    |
| 删除用户 | /deleteUser 的 POST 请求 | /user/id 的 DELETE 请求 |

- 核心过滤器：浏览器 **form** 表单只支持 GET 与 POST 请求，而 DELETE、PUT 等 method 并不支持，Spring 3.0 添加了一个过滤器 `HiddenHttpMethodFilter`，可以将这些请求转换为标准的 **HTTP** 方法，使得支持 GET、POST、PUT 与 DELETE 请求。

用法： SpringBoot 中手动开启 **REST** 支持，表单 `method=post`，隐藏域 `_method=put`。

扩展：如何把 `_method` 这个名字换成我们自己喜欢的。

```java
@RequestMapping(value = "/user",method = RequestMethod.GET)
public String getUser(){
    return "GET-张三";
}
 
@RequestMapping(value = "/user",method = RequestMethod.POST)
public String saveUser(){
    return "POST-张三";
}
 
@RequestMapping(value = "/user",method = RequestMethod.PUT)
public String putUser(){
    return "PUT-张三";
}
 
@RequestMapping(value = "/user",method = RequestMethod.DELETE)
public String deleteUser(){
    return "DELETE-张三";
}
 
@Bean
@ConditionalOnMissingBean(HiddenHttpMethodFilter.class)
@ConditionalOnProperty(prefix = "spring.mvc.hiddenmethod.filter", name = "enabled", matchIfMissing = false)
public OrderedHiddenHttpMethodFilter hiddenHttpMethodFilter() {
    return new OrderedHiddenHttpMethodFilter();
}
 
// 自定义 HiddenHttpMethodFilter，将隐藏域方法名换成自己喜欢的 _m
@Bean
public HiddenHttpMethodFilter hiddenHttpMethodFilter(){
    HiddenHttpMethodFilter methodFilter = new HiddenHttpMethodFilter();
    methodFilter.setMethodParam("_m");
    return methodFilter;
}
```

#### 3.1.2 REST 原理

原理（表单提交要使用 **REST** 的时候）

- 表单提交会带上 `_method=使用的方法`。
- 请求过来被 `HiddenHttpMethodFilter` 拦截。
  - 请求是否正常，并且是 **POST**。
    - 获取到 `_method` 的值，统一转换成大写。
    - 兼容以下请求：**PUT**、**DELETE**、**PATCH**。
    - 原生 `request(post)`，包装模式 `requesWrapper` 重写了 `getMethod` 方法，返回的是传入的值。
    - 过滤器链放行的时候用 `wrapper`，以后的方法调用 `getMethod` 是调用 `requesWrapper` 的。

**使用客户端工具发送 REST 请求**

- 如 PostMan 直接发送 Put、Delete 等方式请求，无需 Filter。

```yaml
spring:
  mvc:
    hiddenmethod:
      filter:
        enabled: true   #开启页面表单的 REST 功能
```

#### 3.1.3 请求映射原理

![请求映射原理](https://cdn.nlark.com/yuque/0/2020/png/1354552/1603181171918-b8acfb93-4914-4208-9943-b37610e93864.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_27%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

SpringMVC 功能分析都从 org.springframework.web.servlet.`DispatcherServlet.doDispatch()` 方法开始。

```Java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
    HttpServletRequest processedRequest = request;
    HandlerExecutionChain mappedHandler = null;
    boolean multipartRequestParsed = false;
    WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);
 
    try {
        try {
            ModelAndView mv = null;
            Object dispatchException = null;
 
            try {
                processedRequest = this.checkMultipart(request);
                multipartRequestParsed = processedRequest != request;
                // 找到当前请求使用哪个 Handler（Controller 的方法）处理
                mappedHandler = this.getHandler(processedRequest);
            
@Nullable
protected HandlerExecutionChain getHandler(HttpServletRequest request) throws Exception {
    if (this.handlerMappings != null) {
        Iterator var2 = this.handlerMappings.iterator();
        while(var2.hasNext()) {
            HandlerMapping mapping = (HandlerMapping)var2.next();
            HandlerExecutionChain handler = mapping.getHandler(request);
            if (handler != null) {
                return handler;
            }
        }
    }
    return null;
}
```

![handlerMappings](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1603181460034-ba25f3c0-9cfd-4432-8949-3d1dd88d8b12.png)

`RequestMappingHandlerMapping`：保存了所有 `@RequestMapping`（请求路径） 和 `Handlerg`（`Controller` 的方法） 的映射规则。

![RequestMappingHandlerMapping](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1603181662070-9e526de8-fd78-4a02-9410-728f059d6aef.png)

所有的请求映射都在 `HandlerMapping` 中。

- SpringBoot 自动配置欢迎页的 `WelcomePageHandlerMapping`，访问 `/` 能访问到**index.html**；
- SpringBoot 自动配置了默认的 `RequestMappingHandlerMapping`：
- 请求进来，挨个尝试所有的 `HandlerMapping` 看是否有请求信息；
- 如果有，就找到这个请求对应的 `Handler`；
- 如果没有，就遍历下一个 `HandlerMapping`；

如果我们需要一些自定义的映射处理，可以通过给容器中放 **HandlerMapping** 来自定义 **HandlerMapping**

![Spring MVC 的运行原理与机制](https://img-blog.csdnimg.cn/20200813163620336.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDM4MjIz,size_16,color_FFFFFF,t_70#pic_center)

有的时候，比如说同一组 API 有不同的版本如 v1，v2，我们可以在 `Controller` 中写两组mapping（比如 `v1/user`，`v2/user`）。但同时我们也可以放在两个包下，都是 `/user`，这个时候我们就可以自定义 `HandlerMapping`，把 `v1/user` 映射到一个包下的 `/user`，把 `v2/user`映射到另外一个包下的 `/user`。

### 3.2 普通参数与基本注解

#### 3.2.1 注解

- `@PathVariable`：请求路径中的值
- `@RequestParam`：请求参数中的值
- `@RequestHeader`：请求 Header 中的值
- `@RequestBody`：请求 Body 中的值
- `@ModelAttribute`：绑定请求参数到实体对象
- `@CookieValue`：获取浏览器中的 Cookie 值
- `@MatrixVariable`：以分号分隔的矩阵变量键值对，形如 **;name1=value1;name2=value2,value3**

```Java
@RestControllerpublic class ParameterTestController {    // 请求路径：car/2/owner/zhangsan    @GetMapping("/car/{id}/owner/{username}")    public Map<String,Object> getCar(@PathVariable("id") Integer id,                                     @PathVariable("username") String name,                                     @PathVariable Map<String,String> pv,                                     @RequestHeader("User-Agent") String userAgent,                                     @RequestHeader Map<String,String> header,                                     @RequestParam("age") Integer age,                                     @RequestParam("inters") List<String> inters,                                     @RequestParam Map<String,String> params,                                     @CookieValue("_ga") String _ga,                                     @CookieValue("_ga") Cookie cookie){        Map<String,Object> map = new HashMap<>();//        map.put("id",id);//        map.put("name",name);//        map.put("pv",pv);//        map.put("userAgent",userAgent);//        map.put("headers",header);        map.put("age",age);        map.put("inters",inters);        map.put("params",params);        map.put("_ga",_ga);        System.out.println(cookie.getName()+"===>"+cookie.getValue());        return map;    }    @PostMapping("/save")    public Map postMethod(@RequestBody String content){        Map<String,Object> map = new HashMap<>();        map.put("content",content);        return map;    }    //1、请求路径：/cars/sell;low=34;brand=byd,audi,yd    //2、SpringBoot 默认禁用了矩阵变量的功能    //      手动开启的原理：URL 路径使用 UrlPathHelper 进行解析。    //              实现 WebMvcConfigurer 接口，重写 configurePathMatch()    //              将 removeSemicolonContent 设置为 false 即可开启。    //3、矩阵变量必须有 URL 路径变量才能被解析    @GetMapping("/cars/{path}")    public Map carsSell(@MatrixVariable("low") Integer low,                        @MatrixVariable("brand") List<String> brand,                        @PathVariable("path") String path){        Map<String,Object> map = new HashMap<>();        map.put("low",low);        map.put("brand",brand);        map.put("path",path);        return map;    }    // 请求路径：/boss/1;age=20/2;age=10    @GetMapping("/boss/{bossId}/{empId}")    public Map boss(@MatrixVariable(value = "age",pathVar = "bossId") Integer bossAge,                    @MatrixVariable(value = "age",pathVar = "empId") Integer empAge){        Map<String,Object> map = new HashMap<>();        map.put("bossAge",bossAge);        map.put("empAge",empAge);        return map;    }}
```

#### 3.2.2 Servlet API

```
WebRequest`、`ServletRequest`、`MultipartRequest`、 `HttpSession`、`PushBuilder`、`Principal`、`InputStream`、`Reader`、`HttpMethod`、`Locale`、`TimeZone`、`ZoneId
```

以上参数都是在 `ServletRequestMethodArgumentResolver.supportsParameter()` 里判断的：

![supportsParameter](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1626275268232-a7f2f6e6-ff73-46e8-aeea-7175ebde6882.png)

然后获取请求：

![获取请求](https://cdn.nlark.com/yuque/0/2021/png/1304385/1626275296541-ecc62d23-43a5-44c8-a9e2-a09e241eae71.png)

#### 3.2.3 复杂参数

```
Map`、`Model`、`Errors`/`BindingResult`、`RedirectAttributes`**（ 重定向携带数据）**、`ServletResponse`**（response）**、`SessionStatus`、`UriComponentsBuilder`、`ServletUriComponentsBuilder
```

Map<String, Object> `map`、Model `model`、HttpServletRequest `request` 都可以给 request 域中放数据 `request.setAttribute()`。

```
Map`、`Model`类型的参数，会返回 `mavContainer.getModel()
// ModelAndViewContainer：模型和视图的容器private final ModelMap defaultModel = new BindingAwareModelMap();public ModelMap getModel() {    if (this.useDefaultModel()) {        return this.defaultModel; // 默认返回 BindingAwareModelMap，既是 Model 又是 Map    } else {        if (this.redirectModel == null) {            this.redirectModel = new ModelMap(); // 重定向为空，返回一个空的 LinkedHashMap        }        return this.redirectModel; // 返回 RedirectAttributesModelMap    }}
```

![Model 和 Map 是同一个对象](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1603271678813-d8e1a1e5-94fa-412c-a7f1-6f27174fd127.png)

![defaultModel](https://cdn.nlark.com/yuque/0/2020/png/1354552/1603271813894-037be041-92a5-49af-a49c-c350b3dd587a.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_27%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

#### 3.2.4 自定义对象参数

可以自动类型转换与格式化，还可以级联封装。

```Java
/**
 * html 中引用对象参数
 *     姓名： <input name="userName"/> <br/>
 *     年龄： <input name="age"/> <br/>
 *     生日： <input name="birth"/> <br/>
 *     宠物姓名：<input name="pet.name"/><br/>
 *     宠物年龄：<input name="pet.age"/>
 */
@Data
public class Person {
    private String userName;
    private Integer age;
    private Date birth;
    private Pet pet;
}
 
@Data
public class Pet {
    private String name;
    private String age;
}
```

### 3.3 自定义类型参数：POJO 封装

`RequestParamMethodArgumentResolver` 支持简单类型的请求方法参数，`ServletModelAttributeMethodProcessor` 支持非简单类型的请求方法参数。

所以对于请求 http://localhost:8080/**testCustomObj?name=张三&age=18**，由于请求方法参数`Employee` 不是简单类型，所以会调用 `ServletModelAttributeMethodProcessor` 将请求参数封装为 `Employee` 对象并返回。

```java
@RequestMapping("/testCustomObj")
@ResponseBody
public Employee testCustomObj(Employee e) {
    return e;
}
```

### 3.4 参数处理原理

1. `HandlerMapping` 中找到能处理请求的 `Handler`（`Controller` 中请求对应的方法）；
2. 为当前 `Handler` 找一个适配器 `HandlerAdapter`，对于请求来说就是 `RequestMappingHandlerAdapter`；
3. 适配器执行目标方法并确定方法参数的每一个值。

**拓展：**

SpringMVC 处理请求大致是这样的：

1. 首先被 `DispatcherServlet` 截获，`DispatcherServlet` 通过 `handlerMapping` 获得 `HandlerExecutionChain`，然后获得 `HandlerAdapter`。

2. ```
   HandlerAdapter
   ```

    

   在内部对于每个请求，都会实例化一个

    

   ```
   ServletInvocableHandlerMethod
   ```

    

   进行处理，

   ```
   ServletInvocableHandlerMethod
   ```

    

   在进行处理的时候，会分两部分别对

   请求

   跟

   响应

   进行处理。

   1. 处理请求的时候，会根据 `ServletInvocableHandlerMethod` 的属性 `argumentResolvers`（这个属性是它的父类 `InvocableHandlerMethod` 中定义的）进行处理，其中 `argumentResolvers` 属性是一个 `HandlerMethodArgumentResolverComposite` 类 （这里使用了**组合模式**的一种变形），这个类是实现了 `HandlerMethodArgumentResolver` 接口的类，里面有各种实现了 `HandlerMethodArgumentResolver` 的 **List** 集合。
   2. 处理响应的时候，会根据 `ServletInvocableHandlerMethod` 的属性 `returnValueHandlers` (自身属性) 进行处理，`returnValueHandlers` 属性是一个 `HandlerMethodReturnValueHandlerComposite` 类（这里使用了**组合模式**的一种变形），这个类是实现了 `HandlerMethodReturnValueHandler` 接口的类，里面有各种实现了 `HandlerMethodReturnValueHandler` 的 **List** 集合。

3. 之后 `HandlerAdapter` 得到 `ModelAndView`，然后做相应的处理。

以 **Resolver** 结尾的类是实现了 `HandlerMethodArgumentResolver` 接口的类，以 **Processor** 结尾的类是实现了 `HandlerMethodArgumentResolver` 和 `HandlerMethodReturnValueHandler` 的类。

- `HandlerMethodArgumentResolver` ：处理请求方法的参数
- `HandlerMethodReturnValueHandler`：处理请求方法的返回值

![HandlerMethodReturnValueHandler](https://cdn.nlark.com/yuque/0/2021/png/1304385/1628903972290-958e800d-9677-4cc0-8ba1-bca274023c8f.png)

> [详解 SpringMVC 中 Controller 的方法中参数的工作原理](https://www.cnblogs.com/fangjian0423/p/springMVC-request-param-analysis.html)

#### 3.4.1 处理器的适配器HandlerAdapter

```Java
// DispatcherServlet#getHandlerAdapterprotected HandlerAdapter getHandlerAdapter(Object handler) throws ServletException {    if (this.handlerAdapters != null) {        for (HandlerAdapter adapter : this.handlerAdapters) {            if (adapter.supports(handler)) {                return adapter;            }        }    }    throw new ServletException("No adapter for handler [" + handler +            "]: The DispatcherServlet configuration needs to include a HandlerAdapter that supports this handler");}
```

![HandlerAdapter](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1603262942726-107353bd-f8b7-44f6-93cf-2a3cad4093cf.png)

- `RequestMappingHandlerAdapter` ：支持方法上标注 `@RequestMapping` 的适配器。
- `HandlerFunctionAdapter`：支持函数式编程的适配器。
- `HttpRequestHandlerAdapter`：无返回值，用于处理静态资源。
- `SimpleControllerHandlerAdapter`：是 `Controller` 实现类的适配器类，其本质是执行 `Controller` 中的 `handleRequest` 方法。

#### 3.4.2 执行目标方法

```Java
// DispatcherServlet#doDispatch// Actually invoke the handler.mv = ha.handle(processedRequest, response, mappedHandler.getHandler());// RequestMappingHandlerAdapter#handleInternalmav = invokeHandlerMethod(request, response, handlerMethod); //执行目标方法// RequestMappingHandlerAdapter#invokeHandlerMethodinvocableMethod.invokeAndHandle(webRequest, mavContainer);// ServletInvocableHandlerMethod#invokeAndHandleObject returnValue = invokeForRequest(webRequest, mavContainer, providedArgs);// InvocableHandlerMethod#invokeForRequestObject[] args = getMethodArgumentValues(request, mavContainer, providedArgs);return this.doInvoke(args); // 通过反射调用目标方法
```

#### 3.4.3 参数解析器 HandlerMethodArgumentResolver

确定将要执行的目标方法的每一个参数的值是什么。

SpringMVC 目标方法能写多少种参数类型，取决于参数解析器。

```Java
// InvocableHandlerMethod#invokeForRequestObject[] args = getMethodArgumentValues(request, mavContainer, providedArgs);// InvocableHandlerMethod#getMethodArgumentValuesif (!this.resolvers.supportsParameter(parameter)) { // 判断是否支持当前参数类型// 解析支持的参数，并放入参数列表args[i] = this.resolvers.resolveArgument(parameter, mavContainer, request, this.dataBinderFactory);// HandlerMethodArgumentResolverComposite#resolveArgumentHandlerMethodArgumentResolver resolver = this.getArgumentResolver(parameter);// HandlerMethodArgumentResolverComposite#getArgumentResolver// 先从参数解析器缓存中获取，若获取不到再从参数解析器中获取Iterator var3 = this.argumentResolvers.iterator();// 判断解析器是否支持当前参数类型，若支持放入参数解析器缓存中，然后返回参数解析器
```

![argumentResolvers](https://cdn.nlark.com/yuque/0/2020/png/1354552/1603263283504-85bbd4d5-a9af-4dbf-b6a2-30b409868774.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_20%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

#### 3.4.4 返回值处理

![image.png](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1626512571526-f97b165f-652c-4b2b-942a-a3cb3cfdffe6.png)

#### 3.4.5 自定义类型参数：POJO 封装

`ServletModelAttributeMethodProcessor` 负责处理自定义的参数类型（非简单类型）。

简单类型如下：

```Java
// BeanUtils#isSimpleValueTypepublic static boolean isSimpleValueType(Class<?> type) {    return (Void.class != type && void.class != type &&            (ClassUtils.isPrimitiveOrWrapper(type) ||            Enum.class.isAssignableFrom(type) ||            CharSequence.class.isAssignableFrom(type) ||            Number.class.isAssignableFrom(type) ||            Date.class.isAssignableFrom(type) ||            Temporal.class.isAssignableFrom(type) ||            URI.class == type ||            URL.class == type ||            Locale.class == type ||            Class.class == type));}
```

![isSimpleValueType](https://cdn.nlark.com/yuque/0/2021/png/1304385/1626445334943-b3f18848-d490-44c3-9e72-319ef8ac9888.png)

解析参数：

```Java
// ModelAttributeMethodProcessor#resolveArgumentWebDataBinder binder = binderFactory.createBinder(webRequest, attribute, name);
```

`WebDataBinder`：Web 数据绑定器，利用它里面的 `Converters` 将请求数据转成指定的数据类型，然后封装到指定的 POJO 对象里面。

`GenericConversionService`：在设置每一个值的时候，找它里面所有的 `Converters`，哪个可以将这个数据类型（request 带来参数的字符串）转换到指定的类型。

![GenericConversionService](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1626448410878-008f3f9c-5013-4b4b-aced-1bc74895799a.png)

```Java
@FunctionalInterfacepublic interface Converter<S, T>
```

![ExtendServletRequestDataBinder](https://cdn.nlark.com/yuque/0/2020/png/1354552/1603337871521-25fc1aa1-133a-4ce0-a146-d565633d7658.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_39%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

![converters](https://cdn.nlark.com/yuque/0/2020/png/1354552/1603338486441-9bbd22a9-813f-49bd-b51b-e66c7f4b8598.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_44%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10%2Cresize%2Cw_752)

可以给WebDataBinder里面放自己的Converter；

**private static final class** StringToNumber<T **extends** Number> **implements** Converter<String, T>

自定义 Converter：

```Java
// 1、WebMvcConfigurer 定制化 SpringMVC 的功能@Beanpublic WebMvcConfigurer webMvcConfigurer (){    return new WebMvcConfigurer () {        @Override        public void configurePathMatch (PathMatchConfigurer configurer) {            UrlPathHelper urlPathHelper = new UrlPathHelper ();            // 不移除分号后面的内容，矩阵变量功能就可以生效            urlPathHelper.setRemoveSemicolonContent (false);            configurer.setUrlPathHelper (urlPathHelper);        }        @Override        public void addFormatters (FormatterRegistry registry) {            registry.addConverter (new Converter<String, Pet>() {                @Override                public Pet convert (String source) {                    // 阿猫，3                    if (!StringUtils.isEmpty (source)){                        Pet pet = new Pet ();                        String [] split = source.split (",");                        pet.setName (split [0]);                        pet.setAge (Integer.parseInt (split [1]));                        return pet;                    }                    return null;                }            });        }    };}
```

#### 3.4.6 目标方法执行完成

将所有返回的数据都放在 `ModelAndViewContainer`，包含要去的页面地址 `View`，还包含 `Model` 数据。

![mavContainer](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1603272018605-1bce3142-bdd9-4834-a028-c753e91c52ac.png)

![updateBindResult](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1626361978596-259159d7-7aa2-459c-b43a-7e31c8f63277.png)

#### 3.4.7 处理派发结果

![exposeModelAsRequestAttribute](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1626362958127-42143dee-56bd-4143-bd60-ed423ee81b98.png)

## 4、数据响应与内容协商

### 4.1 响应 JSON

#### 4.1.1 jackson.jar + @ResponseBody

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<!-- Web 场景自动引入了 json 场景,下面的依赖不需要显示声明 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-json</artifactId>
    <version>2.3.4.RELEASE</version>
    <scope>compile</scope>
</dependency>
```

自动给前端返回 **json** 数据。

**1、返回值解析器**

```Java
// ServletInvocableHandlerMethod#invokeAndHandle
this.returnValueHandlers.handleReturnValue(
        returnValue, getReturnValueType(returnValue), mavContainer, webRequest);
 
// HandlerMethodReturnValueHandlerComposite#handleReturnValue
handler.handleReturnValue(returnValue, returnType, mavContainer, webRequest);
 
// RequestResponseBodyMethodProcessor#handleReturnValue
// 使用对应的消息转换器进行写出操作
writeWithMessageConverters(returnValue, returnType, inputMessage, outputMessage);
```

![returnValueHandlers](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605151359370-01cd1fbe-628a-4eea-9430-d79a78f59125.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_25%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

**2、返回值解析器原理**

![HandlerMethodReturnValueHandler](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605151728659-68c8ce8a-1b2b-4ab0-b86d-c3a875184672.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_23%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

1. 返回值处理器判断是否支持这种类型的返回值 `supportsReturnType`；

   ![supportsReturnType](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1626514119167-937aa79a-8223-4ef3-82af-cdf4a62f987e.png)

2. 返回值处理器调用 `handleReturnValue` 进行处理；

   ![handleReturnValue](https://cdn.nlark.com/yuque/0/2021/png/1304385/1626514211219-23d6c27a-ea41-4e92-ade2-d66d4cc91362.png)

3. `RequestResponseBodyMethodProcessor` 可以处理有 @ModelAttribute 且为对象类型的 @ResponseBody 注解。

   1. 利用 `MessageConverters` 进行处理将数据写为 **json**。

      1. 内容协商：浏览器默认会以请求头（**Accept** 字段）的方式告诉服务器他能接受什么样的内容类型；

      2. 服务器根据自己的能力，决定能生产什么内容类型的数据；

         ![内容类型](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1626515255797-09a21cb2-a28d-4d3a-9167-a671177a293d.png)

      3. SpringMVC 会遍历所有容器底层的 `HttpMessageConverter`，看谁能处理。

         1. 得到 `MappingJackson2HttpMessageConverter`，它可以将对象写为 **json**；
         2. 利用 `MappingJackson2HttpMessageConverter` 将对象转为 **json** 再写出去。

#### 4.1.2 支持的返回类型

- `ModelAndView`：包含 `Model` 和 `View` 对象，可以通过它访问 `@ModelAttribute` 注解的对象。
- `Model`：仅包含数据访问，通过 `RequestToViewNameTranslator` 来隐蔽地决定此请求返回的 `View` 视图对象。
- `Map`：和 `Model` 相似。
- `View`：仅包含视图数据，而 `Model` 数据隐含在 `@ModelAttribute` 注解标注的对象中、或者
- `String`：表示 `View` 视图的名称。数据信息的保存同上。
- `void`：当开发者直接操作 `ServletResponse` / `HttpServletResponse` 进行请求跳转，或者 `View` 由 `RequestToViewNameTranslator` 隐蔽地决定时，可使用此返回值。
- 任意对象：如果方法被 `@ResponseBody` 注解，可采用此值。Spring 会使用 `HttpMessageConverters` 将对象转化成文本输出。
- `HttpEntity<?>` 或 `ResponseEntity<?>`：使用此值，Spring 也会使用 `HttpMessageConverters` 将对象转化成文本输出。
- `Callable`：异步请求时使用。
- `DeferredResult`：当 Spring 决定使用选择的某个线程产生值时可以使用此对象。
- `WebAsyncTask`：带有超时时间的 Callable 异步任务。

![支持的返回类型](https://cdn.nlark.com/yuque/0/2021/png/1304385/1628917925463-98d86339-8ba2-4c57-95f2-95781682a3a2.png?x-oss-process=image%2Fresize%2Cw_750)

#### 4.1.3 HTTPMessageConverter 原理

**1、MessageConverter 规范**

![HTTPMessageConverter](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1605163447900-e2748217-0f31-4abb-9cce-546b4d790d0b.png)

`HttpMessageConverter`: 调用 `canRead()` 或 `canWrite()` 方法判断是否支持将 此 Class 类型的对象，转为 `MediaType` 类型的数据。

例子： json 转为 Person 对象，或者Person 对象转为 json。

**2、默认的MessageConverter**

```Java
// AbstractMessageConverterMethodArgumentResolver#readWithMessageConverters
for (HttpMessageConverter<?> converter : this.messageConverters) {
```

![messageConverters](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605163584708-e19770d6-6b35-4caa-bf21-266b73cb1ef1.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_17%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

- `ByteArrayHttpMessageConverter`：只支持字节数组类型的。
- `StringHttpMessageConverter`：**UTF-8** 类型的字符串。
- `StringHttpMessageConverter`：**ISO-8859-1** 类型的字符串。
- `ResourceHttpMessageConverter`：`Resource` 类型。
- `ResourceRegionHttpMessageConverter`：`ResourceHttpMessageConverter` 的缺省设置，用于支持 HTTP Range 头部使用时，将静态资源的部分写入到响应对象。
- `SourceHttpMessageConverter`：`DOMSource.class`、`SAXSource.class`、`StAXSource.class`、`StreamSource.class`、`Source.class`。
- `AllEncompassingFormHttpMessageConverter`：对 `FormHttpMessageConverter`（表单与 `MultiValueMap` 的相互转换）的扩展，提供了对 **xml** 和 **json** 的支持。
- `MappingJackson2HttpMessageConverter`：使用 Jackson 的 ObjectMapper 转换 Json 数据
- `MappingJackson2HttpMessageConverter`：
- `Jaxb2RootElementHttpMessageConverter`：支持注解方式将对象转换为 xml。

最终 `MappingJackson2HttpMessageConverter` 把对象转为 **json**（利用底层 **jackson** 的`objectMapper` 转换的）。

![outputMessage](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605164243168-1a31e9af-54a4-463e-b65a-c28ca7a8a2fa.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_34%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

> [【小家 Spring】Spring MVC 容器的 web 九大组件之 ---HandlerAdapter 源码详解 ---HttpMessageConverter 消息转换器详解](https://cloud.tencent.com/developer/article/1497632)

### 4.2 内容协商

#### 4.2.1 引入 xml 依赖

根据客户端接收能力不同，返回不同媒体类型的数据。

若客户端无法解析服务端返回的内容，即媒体类型未匹配，那么响应 **406**。

```xml
<dependency>
    <groupId>com.fasterxml.jackson.dataformat</groupId>
    <artifactId>jackson-dataformat-xml</artifactId>
</dependency>
```

#### 4.2.2 返回 json 和 xml

Postman 分别测试，只需要改变请求头中 **Accept** 字段。**HTTP** 协议中规定的，告诉服务器本客户端可以接收的数据类型。

![Postman](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605173127653-8a06cd0f-b8e1-4e22-9728-069b942eba3f.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_33%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

#### 4.2.3 开启浏览器参数方式内容协商功能

为了方便内容协商，开启基于请求参数的内容协商功能。

```yaml
spring:
    contentnegotiation:
      favor-parameter: true  #开启请求参数内容协商模式
```

**优缺点：**

- 优点：不受浏览器约束。
- 缺点：需要额外的传递 **format** 参数，**URL** 变得冗余繁琐，缺少了 **REST** 的简洁风范。还有个缺点便是：需要手动显示开启。

发请求：

http://localhost:8080/test/person?format=json

http://localhost:8080/test/person?format=xml

![getAcceptableMediaTypes](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1626528669116-d399ae57-2709-4b94-9426-32e4a2ec4390.png)

![contentNegotiationManager](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605230907471-b0ed34bc-6782-40e7-84b7-615726312f01.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_22%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

确定客户端接收什么样的内容类型：

1. Parameter 策略优先确定是要返回 **json** 数据（获取请求头中的 **format** 的值）
2. 最终进行内容协商返回给客户端 **json** 即可。

#### 4.2.4 内容协商原理

1. 判断当前响应头中是否已经有确定的媒体类型 `MediaType`；

2. 获取客户端（PostMan、浏览器）支持接收的内容类型，即获取 **Accept** 请求头字段，如**application/xml**；

   - `contentNegotiationManager` 内容协商管理器，默认使用基于请求头的策略；

     ![strategies](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1626527877945-76d64ff0-a578-4ab7-b061-36bd451e6709.png)

   - `HeaderContentNegotiationStrategy` 确定客户端可以接收的内容类型 。

     ![getHeaderValues](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605230546376-65dcf657-7653-4a58-837a-f5657778201a.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_28%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

3. 遍历循环所有当前系统的 `MessageConverter`，看谁支持操作这个 `Person` 对象；

   ![messageConverters](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1605173657818-73331882-6086-490c-973b-af46ccf07b32.png)

   上面这四个并不完全一样：

   ![messageConverter 差异](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1626519411779-4c8b7c30-53f2-46cb-beed-304eddb9f46c.png)

4. 找到支持操作 `Person` 对象的 `MessageConverter`，把 `MessageConverter` 支持的媒体类型统计出来；

5. 客户端需要 **application/xml**，在服务端能力列表中；

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605173876646-f63575e2-50c8-44d5-9603-c2d11a78adae.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_20%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

6、进行内容协商，选出最佳匹配媒体类型；

7、用 `MessageConverter` 将对象转为最佳匹配媒体类型 。

导入了 **jackson** 处理 **xml** 的包，**xml** 的 `MessageConverter` 就会自动配置进来。

```Java
// WebMvcConfigurationSupport
jackson2XmlPresent = ClassUtils.isPresent("com.fasterxml.jackson.dataformat.xml.XmlMapper", classLoader);
 
// WebMvcConfigurationSupport#addDefaultHttpMessageConverters
if (jackson2XmlPresent) {
    Jackson2ObjectMapperBuilder builder = Jackson2ObjectMapperBuilder.xml();
    if (this.applicationContext != null) {
        builder.applicationContext(this.applicationContext);
    }
    messageConverters.add(new MappingJackson2XmlHttpMessageConverter(builder.build()));
}
```

#### 4.2.5 自定义 MessageConverter

实现多协议数据兼容：**json**、**xml**、x-guigu。

1. `@ResponseBody` 响应数据出去 调用 `RequestResponseBodyMethodProcessor` 处理；
2. Processor 处理方法返回值，通过 `MessageConverter` 处理；
3. 所有 `MessageConverter` 合起来可以支持各种媒体类型数据的操作（读、写）；
4. 内容协商找到最终的 `MessageConverter` 。

```yaml
spring:
  mvc:
    contentnegotiation:
      media-types:
        gg: application/x-guigu
      favor-parameter: true
```

配置 SpringMVC 的什么功能，只需要给容器中添加一个 `WebMvcConfigurer` 并配置即可。

```Java
@Bean
public WebMvcConfigurer webMvcConfigurer(){
    return new WebMvcConfigurer() {
        @Override
        public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
 
        }
    }
}
```

![strategy](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1605260623995-8b1f7cec-9713-4f94-9cf1-8dbc496bd245.png)

![contentNegotiationManager](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605261062877-0a27cc41-51cb-4018-a9af-4e0338a247cd.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_27%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

有可能我们添加的自定义的功能会覆盖默认很多功能，导致一些默认的功能失效。

## 5、视图解析与模板引擎

**视图解析**：SpringBoot默认不支持 [JSP](https://www.w3cschool.cn/jsp/)，需要引入第三方模板引擎技术实现页面渲染。

**模板引擎**：是为了使用户界面与业务数据（内容）分离而产生的，它可以生成特定格式的文档，用于网站的模板引擎就会生成一个标准的文档。

![模板引擎](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1596209109710-99fba456-92b1-43f3-8fa4-1db486247ecf.png)

- [Velocity](https://www.jianshu.com/p/5913903324ff)：一个基于 Java 的模板引擎，其提供了一个 `Context` 容器，在 Java 代码里面我们可以往容器中存值，然后在 **vm** 文件中使用特定的语法获取。
- [Freemarker]([FreeMarker 教程网](http://www.freemarker.net/))：一个基于模板生成文本输出的通用工具，使用纯 Java 编写，模板中没有业务逻辑，外部 Java 程序通过数据库操作等生成数据传入模板（template）中，然后输出页面。它能够生成各种文本：**HTML**、**XML**、**RTF**、**Java** 源代码等等，而且不需要 Servlet 环境，并且可以从任何源载入模板，如本地文件、数据库等等。
- [Thymeleaf](https://www.docs4dev.com/docs/zh/thymeleaf/3.0/reference/using_thymeleaf.html)：是适用于 Web 和独立环境的现代服务器端 Java 模板引擎，能够处理 HTML，XML，JavaScript，CSS 甚至纯文本。有两种*标记*模板模式 `HTML`（常用）和 `XML`，三种*文本*模板模式 `TEXT`，`JAVASCRIPT` 和 `CSS` 和*无操作*模板模式 `RAW`。

SpringBoot 推荐使用 **Thymeleaf**，因为与 **Velocity**、**FreeMarker** 等传统的 Java 模板引擎不同，**Thymeleaf** 支持 `HTML` 模板模式，模板后缀为 `.html`，可以直接被浏览器打开，因此，预览时非常方便。

### 5.1 视图解析原理

1. 目标方法处理的过程中，所有数据都会被放在 `ModelAndViewContainer` 里面，包括**数据**和**视图地址**；

   ![mavContainer](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1626602773740-bb9b98c0-1108-48c9-9a52-364d61a4ea56.png)

2. 方法的参数是一个**自定义类型**对象（从请求参数中确定的），把它重新放在 `ModelAndViewContainer`；

3. 任何目标方法执行完成以后都会返回 `ModelAndView`（数据和视图地址）；

4. `processDispatchResult` 处理派发结果（页面该如何响应）。

   - `render(mv, request, response);` 进行页面渲染逻辑。

     - 根据方法的 `String` 返回值得到 `View` 对象（定义了页面的渲染逻辑）

       1. 所有的视图解析器尝试是否能根据当前返回值得到 View 对象；

          ![viewResolver](https://cdn.nlark.com/yuque/0/2021/png/1304385/1626603433682-b9cb246c-dddf-47f2-8b3c-30046077a13f.png)

       2. 得到了 **redirect:/main.html** --> **Thymeleaf** 使用 `new RedirectView()` 创建视图；

          ![RedirectView](https://cdn.nlark.com/yuque/0/2021/png/1304385/1626603907392-3cd1dfce-afa8-43fd-a03d-a52a52204368.png)

       3. `ContentNegotiationViewResolver` 里面包含了下面所有的视图解析器，内部还是利用上面所有视图解析器得到视图对象；

       4. `view.render(mv.getModelInternal(), request, response);` 视图对象调用自定义的 `render` 进行页面渲染工作。

          ![render](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1626604323783-4e6ca09d-35f0-4f84-a3ba-32889e29e49f.png)

          - `RedirectView` 如何渲染（重定向到一个页面)。

            1. 获取目标 `URL` 地址；

            2. 调用原生的 `response.sendRedirect(encodedURL);` 发送重定向。

               ![sendRedirect](https://cdn.nlark.com/yuque/0/2021/png/1304385/1626604421372-b748b478-6665-4361-b41d-abbc80cf70b2.png)

**Controller 方法的返回值如下**

#### 5.1.1 返回 ModelAndView

- `ModelAndView` 存放数据，`addObject()`，往 `model`（`request` 域）添加数据；
- `ModelAndView` 添加逻辑视图名，`setViewName()`，经过视图解析器，得到物理视图，转发到物理视图。

```Java
@RequestMapping("/getUser.action")
public ModelAndView getUser(@RequestParam(name="userId",required = true) Integer id) throws Exception{
    System.out.println("id=" + id);
    ModelAndView modelAndView = new ModelAndView();
    User user = userService.queryOne(id);
    modelAndView.addObject("user", user);
    modelAndView.setViewName("userinfo");
    return modelAndView;
}
```

#### 5.1.2 返回 String

- 逻辑视图名： 经过视图解析器，得到物理视图，转发到物理视图；

  ```Java
  @RequestMapping("/index.action")
  public String  toIndex() {
      return "index";
  }
  ```

- `redirect:资源路径`：不经过视图解析器，要求这个资源路径以完整的路径 `/` 开头，**重定向**到资源；

  `new RedirectView()` -> `response.sendRedirect(encodedURL);`

  ```Java
  @RequestMapping("/index.action")
  public String  toIndex() {
      return "redirect:/jsp/index.jsp";
  }
  ```

- `forward:资源路径`： 不经过视图解析器，要求这个资源路径以完整的路径 `/` 开头，**转发**到资源；

  `new InternalResourceView(forwardUrl);` -> `request.getRequestDispatcher(path).forward(request, response);`

  ```java
  @RequestMapping("/index.action")
  public String  toIndex() {
      return "forward:/jsp/index.jsp";
  }
  ```

- 普通字符串、对象：

  `new ThymeleafView()` -> 调用模板引擎的 `process` 方法进行页面渲染（用 `writer` 输出）

  ```java
  // 将 user 对象以 json 的格式响应给前端页面
  @RequestMapping("/queryUserByCondition.action")
  @ResponseBody // 需要结合该注解，表示响应的不是视图
  public User queryUserByCondition(User user) throws Exception{
      return user;
  }
  ```

### 5.2 Thymeleaf 语法

#### 5.2.1 表达式

| 表达式名字 | 语法   | 用途                                   |
| ---------- | ------ | -------------------------------------- |
| 变量取值   | ${...} | 获取请求域、`session` 域、对象等值     |
| 选择变量   | *{...} | 获取上下文对象值                       |
| 消息       | #{...} | 获取国际化等值                         |
| 链接       | @{...} | 生成链接                               |
| 片段表达式 | ~{...} | 类似于 `jsp:include`，引入公共页面片段 |

#### 5.2.2 字面量

文本值：`'one text'`、`'Another one!'`、…

数字：`0`、`34`、`3.0`、`12.3`、…

布尔值：`true`、`false`

空值：`null`

变量：`one`、`two`、.... 变量不能有空格

#### 5.2.3 文本操作

字符串拼接：`+`

变量替换：`|The name is ${name}|`

#### 5.2.4 数学运算

运算符：`+`、`-` 、`*`、`/`、`%`

#### 5.2.5布尔运算

运算符：`and`、`or`

一元运算：`!`、`not`

#### 5.2.6比较运算

比较：`>`、`<`、`>=`、`<=` (`gt`、`lt`、`ge`、`le`)

等式：`==`、`!=` (`eq`、`ne` )

#### 5.2.7 条件运算

If-then：`(if) ? (then)`

If-then-else：`(if) ? (then) : (else)`

Default：`(value) ?: (defaultvalue)`

#### 5.2.8 特殊操作

无操作：`_`

#### 5.2.9 设置属性值 `th:attr`

设置单个值：

```html
<form action="subscribe.html" th:attr="action=@{/subscribe}">
  <fieldset>
    <input type="text" name="email" />
    <input type="submit" value="Subscribe!" th:attr="value=#{subscribe.submit}"/>
  </fieldset>
</form>
```

设置多个值：

```html
<img src="../../images/gtvglogo.png"  th:attr="src=@{/images/gtvglogo.png},title=#{logo},alt=#{logo}" />
```

以上两个的代替写法 `th:xxx`

```html
<input type="submit" value="Subscribe!" th:value="#{subscribe.submit}"/>
<form action="subscribe.html" th:action="@{/subscribe}">
```

所有 `HTML5` 兼容的标签写法：

https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#setting-value-to-specific-attributes

#### 5.2.10 迭代

```html
<tr th:each="prod : ${prods}">
        <td th:text="${prod.name}">Onions</td>
        <td th:text="${prod.price}">2.41</td>
        <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
</tr>
<tr th:each="prod,iterStat : ${prods}" th:class="${iterStat.odd}? 'odd'">
  <td th:text="${prod.name}">Onions</td>
  <td th:text="${prod.price}">2.41</td>
  <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
</tr>
```

#### 5.2.11 条件运算

```html
<a href="comments.html"
th:href="@{/product/comments(prodId=${prod.id})}"
th:if="${not #lists.isEmpty(prod.comments)}">view</a>
<div th:switch="${user.role}">
  <p th:case="'admin'">User is an administrator</p>
  <p th:case="#{roles.manager}">User is a manager</p>
  <p th:case="*">User is some other thing</p>
</div>
```

#### 5.2.12属性优先级

![属性优先级](https://cdn.nlark.com/yuque/0/2020/png/1304385/1596209109707-e02b3397-5bac-4c3f-a23b-1779207bc929.png)

### 5.3 Thymeleaf 使用

#### 5.3.1 引入 starter

```xml
<properties>
    <java.version>1.8</java.version>
    <thymeleaf.version>3.0.11.RELEASE</thymeleaf.version>
    <!-- 布局功能的支持程序 thymeleaf3 主程序 layout2 以上版本 -->
    <thymeleaf-layout-dialect.version>2.4.1</thymeleaf-layout-dialect.version>
</properties>
 
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

#### 5.3.2 Thymeleaf 自动配置

```Java
@Configuration(proxyBeanMethods = false)
@EnableConfigurationProperties(ThymeleafProperties.class)
@ConditionalOnClass({ TemplateMode.class, SpringTemplateEngine.class })
@AutoConfigureAfter({ WebMvcAutoConfiguration.class, WebFluxAutoConfiguration.class })
public class ThymeleafAutoConfiguration { }
```

自动配好的策略

- 所有 **Thymeleaf** 的配置值都在 `ThymeleafProperties`。
- 配置好了 `SpringTemplateEngine` 。
- 配置好了 `ThymeleafViewResolver`。
- 我们只需要直接开发页面。

```Java
@ConfigurationProperties(prefix = "spring.thymeleaf")
public class ThymeleafProperties {
    private static final Charset DEFAULT_ENCODING = StandardCharsets.UTF_8;
 
    public static final String DEFAULT_PREFIX = "classpath:/templates/";
 
    public static final String DEFAULT_SUFFIX = ".html";
```

只要我们把 **HTML** 页面放在 **classpath:/templates/**，**Thymeleaf** 就能自动渲染。

#### Thymeleaf 页面开发

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1 th:text="${msg}"> 哈哈 </h1>
<h2>
    <a href="www.atguigu.com" th:href="${link}"> 去百度 </a><br/>
    <a href="www.atguigu.com" th:href="@{link}"> 去百度 2</a>
</h2>
</body>
</html>
```

## 6、构建后台管理系统

### 6.1 创建项目

**pom.xml** 需要引入以下依赖：thymeleaf、web-starter、devtools、lombok。

### 6.2静态资源处理

已经自动配置好，只需把所有静态资源放到 **static** 文件夹下，模板页面放到 **templates** 文件夹下。

### 6.3 路径构建

```
th:action="@{/login}"
```

### 6.4 页面跳转

`Controller` ：

```Java
@PostMapping("/login")
public String main(User user, HttpSession session, Model model){
    if (StringUtils.hasLength (user.getUserName()) && "123456".equals (user.getPassword())){
        // 把登陆成功的用户保存起来
        session.setAttribute("loginUser",user);
        // 登录成功重定向到 main.html;  重定向防止表单重复提交
        return "redirect:/main.html";
    } else {
        model.addAttribute("msg","账号密码错误");
        // 回到登录页面
        return "login";
    }
}
```

登陆错误消息的显示：

```html
<p style="color: red" th:text="${msg}" th:if="${not #strings.isEmpty(msg)}"></p>
```

### 6.5 数据渲染

`Controller` ：

```Java
@GetMapping("/dynamic_table")
public String dynamic_table(Model model){
    // 表格内容的遍历
    List<User> users = Arrays.asList(new User("zhangsan", "123456"),
                                     new User("lisi", "654321"),
                                     new User("haha", "aaa"));
    model.addAttribute("users", users);
    return "table/dynamic_table";
}
```

### 6.6 模板页面

table/**dynamic_table.html**：

```html
<table class="display table table-bordered" id="hidden-table-info">
    <thead>
        <tr>
            <th>#</th>
            <th>用户名</th>
            <th>密码</th>
        </tr>
    </thead>
    <tbody>
        <tr class="userClass" th:each="user,stats:${users}">
            <td th:text="${stats.count}"></td>
            <td th:text="${user.userName}"></td>
            <td>[[${user.password}]]</td> <!-- 行内写法 -->
        </tr>
    </tbody>
</table>
```

开发期间模板引擎页面修改以后，要实时生效，需要禁用模板引擎的缓存，页面修改完成以后**Ctrl + F9**：重新编译（无需重新启动）。

```properties
# 禁用缓存
spring.thymeleaf.cache=false
```

### 6.7 国际化

#### 6.7.1 简介

1. 编写国际化配置文件；
2. 使用 `ResourceBundleMessageSource` 管理国际化资源文件；
3. 在页面使用 `fmt:message` 取出国际化内容。

#### 6.7.2 步骤

1. 编写国际化配置文件，抽取页面需要显示的国际化消息；

![国际化配置文件](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1596209109688-89c69511-6d11-499d-a1d7-bf7cf48e7a98.png)

1. SpringBoot 自动配置好了管理国际化资源文件的组件；

```Java
public class MessageSourceAutoConfiguration {
    private static final Resource[] NO_RESOURCES = {};
 
    @Bean
    // 我们的配置文件可以直接放在类路径下叫messages.properties。
    @ConfigurationProperties(prefix = "spring.messages")
    public MessageSourceProperties messageSourceProperties() {
        return new MessageSourceProperties();
    }
 
    @Bean
    public MessageSource messageSource(MessageSourceProperties properties) {
        ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
        if (StringUtils.hasText(properties.getBasename())) {
            // 设置国际化资源文件的基础名（去掉语言国家代码的）。
            messageSource.setBasenames(StringUtils
                    .commaDelimitedListToStringArray(StringUtils.trimAllWhitespace(properties.getBasename())));
        }
        if (properties.getEncoding() != null) {
            messageSource.setDefaultEncoding(properties.getEncoding().name());
        }
        messageSource.setFallbackToSystemLocale(properties.isFallbackToSystemLocale());
        Duration cacheDuration = properties.getCacheDuration();
        if (cacheDuration != null) {
            messageSource.setCacheMillis(cacheDuration.toMillis());
        }
        messageSource.setAlwaysUseMessageFormat(properties.isAlwaysUseMessageFormat());
        messageSource.setUseCodeAsDefaultMessage(properties.isUseCodeAsDefaultMessage());
        return messageSource;
    }
```

1. 去页面获取国际化的内容。

![image](https://cdn.nlark.com/yuque/0/2020/png/1304385/1596209109713-04088a42-a925-4a03-857a-ba3b119d4adf.png)

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
        <meta name="description" content="">
        <meta name="author" content="">
        <title>Signin Template for Bootstrap</title>
        <!-- Bootstrap core CSS -->
        <link href="asserts/css/bootstrap.min.css" th:href="@{/webjars/bootstrap/4.4.1/css/bootstrap.css}" rel="stylesheet">
        <!-- Custom styles for this template -->
        <link href="asserts/css/signin.css" th:href="@{/asserts/css/signin.css}" rel="stylesheet">
    </head>
 
    <body class="text-center">
        <form class="form-signin" action="dashboard.html">
            <img class="mb-4" th:src="@{/asserts/img/bootstrap-solid.svg}" src="asserts/img/bootstrap-solid.svg" alt="" width="72" height="72">
            <h1 class="h3 mb-3 font-weight-normal" th:text="#{login.tip}">Please sign in</h1>
            <label class="sr-only" th:text="#{login.username}">Username</label>
            <input type="text" class="form-control" placeholder="Username" th:placeholder="#{login.username}" required="" autofocus="">
            <label class="sr-only" th:text="#{login.password}">Password</label>
            <input type="password" class="form-control" placeholder="Password" th:placeholder="#{login.password}" required="">
            <div class="checkbox mb-3">
                <label>
                    <input type="checkbox" value="remember-me"/> [[#{login.remember}]]
                </label>
            </div>
            <button class="btn btn-lg btn-primary btn-block" type="submit" th:text="#{login.btn}">Sign in</button>
            <p class="mt-5 mb-3 text-muted">© 2020-2021</p>
            <a class="btn btn-sm">中文</a>
            <a class="btn btn-sm">English</a>
        </form>
    </body>
</html>
```

效果：根据浏览器设置的语言信息切换了国际化。

#### 6.7.3 原理

- `Locale`：区域信息对象；
- `LocaleResolver`：获取区域信息对象。

```Java
@Bean
@ConditionalOnMissingBean
@ConditionalOnProperty(prefix = "spring.mvc", name = "locale")
public LocaleResolver localeResolver() {
    if (this.mvcProperties.getLocaleResolver() == WebMvcProperties.LocaleResolver.FIXED) {
        return new FixedLocaleResolver(this.mvcProperties.getLocale());
    }
    AcceptHeaderLocaleResolver localeResolver = new AcceptHeaderLocaleResolver();
    localeResolver.setDefaultLocale(this.mvcProperties.getLocale());
    return localeResolver;
}
```

默认的就是根据请求头带来的区域信息获取 `Locale` 进行国际化。

1. 点击链接切换国际化

```Java
// 可以在链接上携带区域信息
public class MyLocaleResolver implements LocaleResolver {
    @Override
    public Locale resolveLocale(HttpServletRequest request) {
        String l = request.getParameter("l");
        Locale locale = Locale.getDefault();
        if(!StringUtils.isEmpty(l)){
            String[] split = l.split("_");
            locale = new Locale(split[0],split[1]);
        }
        return locale;
    }
 
    @Override
    public void setLocale(HttpServletRequest request, HttpServletResponse response, Locale locale) {
 
    }
 
@Bean
    public LocaleResolver localeResolver(){
        return new MyLocaleResolver();
    }
}
```

## 7、RESTful 的 CRUD

### 7.1 实验要求

1. **CRUD** 满足 **REST** 风格。

**URI**：**/资源名称/资源标识**，以 **HTTP** 请求方式区分对资源的 **CRUD** 操作。

|      | 普通的 CRUD（URI 来区分操作） | RESTful 的 CRUD   |
| ---- | ----------------------------- | ----------------- |
| 查询 | getEmp                        | emp---GET         |
| 添加 | addEmp?xxx                    | emp---POST        |
| 修改 | updateEmp?id=xxx&xxx=xx       | emp/{id}---PUT    |
| 删除 | deleteEmp?id=1                | emp/{id}---DELETE |

1. 请求架构

| 实验功能                               | 请求 URI | 请求方式 |
| -------------------------------------- | -------- | -------- |
| 查询所有员工                           | emps     | GET      |
| 查询某个员工（来到修改页面）           | emp/1    | GET      |
| 来到添加页面                           | emp      | GET      |
| 添加员工                               | emp      | POST     |
| 来到修改页面（查出员工信息并进行回显） | emp/1    | GET      |
| 修改员工                               | emp      | PUT      |
| 删除员工                               | emp/1    | DELETE   |

### 7.2 CRUD-员工列表

**Thymeleaf** 公共页面元素抽取：

1. 抽取公共片段

   ```html
   <div th:fragment="copy">&copy; 2021 The Good Thymes Virtual Grocery</div>
   ```

2. 引入公共片段

   ```html
   <div th:insert="~{footer :: copy}"></div>
   ```

   - `~{templatename::selector}` -> 模板名 :: 选择器
   - `~{templatename::fragmentname}` -> 模板名 :: 片段名

3. 默认效果

   - `insert` 的公共片段在 `div` 标签中；
   - 如果使用 `th:insert` 等属性进行引入，可以不用写 `~{}`；
   - 行内写法：`[[~{}]]` 或 `[(~{})]`。

三种引入公共片段的 `th` 属性：

- `th:insert`：将公共片段整个**插入**到声明引入的元素中；
- `th:replace`：将声明引入的元素**替换**为公共片段；
- `th:include`：将被引入的片段的内容**包含**进这个标签中。

参考：[Tutorial: Using Thymeleaf](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#template-layout)

```html
<footer th:fragment="copy">&copy; 2011 The Good Thymes Virtual Grocery</footer>
 
<!-- 三种引入方式 -->
<div th:insert="footer :: copy"></div>
<div th:replace="footer :: copy"></div>
<div th:include="footer :: copy"></div>
 
<!-- 对应的效果 -->
<div><footer>&copy; 2011 The Good Thymes Virtual Grocery</footer></div>
<footer>&copy; 2011 The Good Thymes Virtual Grocery</footer>
<div>&copy; 2011 The Good Thymes Virtual Grocery</div>
```

引入片段的时候传入参数：

```html
<nav class="col-md-2 d-none d-md-block bg-light sidebar" id="sidebar">
    <div class="sidebar-sticky">
        <ul class="nav flex-column">
            <li class="nav-item">
                <a class="nav-link active"
                   th:class="${activeUri=='main.html' ? 'nav-link active' : 'nav-link'}"
                   href="#" th:href="@{/main.html}">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-home">
                        <path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path>
                        <polyline points="9 22 9 12 15 12 15 22"></polyline>
                    </svg>
                    Dashboard <span class="sr-only">(current)</span>
                </a>
            </li>
 
<!-- 引入侧边栏并传入参数 -->
<div th:replace="commons/bar :: #sidebar(activeUri='emps')"></div>
```

### 7.3 CRUD-员工添加

添加页面：

```html
<form th:action="@{/emp}" method="post">
    <div class="form-group">
        <label>LastName</label>
        <input name="LastName" type="text" class="form-control" placeholder="zhangsan">
    </div>
    <div class="form-group">
        <label>Email</label>
        <input name="email" type="email" class="form-control" placeholder="zhangsan@atguigu.com">
    </div>
    <div class="form-group">
        <label>Gender</label><br/>
        <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" name="gender"  value="1">
            <label class="form-check-label">男</label>
        </div>
        <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" name="gender"  value="0">
            <label class="form-check-label">女</label>
        </div>
    </div>
    <div class="form-group">
        <label>department</label>
        <select class="form-control" name="department.id">
            <!-- 提交的是部门的 id -->
            <option th:value="${dept.id}" th:each="dept:${depts}" th:text="${dept.departmentName}">1</option>
        </select>
    </div>
    <div class="form-group">
        <label>Birth</label>
        <input name="birth" type="text" class="form-control" placeholder="zhangsan">
    </div>
    <button type="submit" class="btn btn-primary">添加</button>
</form>
```

### 7.4 CRUD-员工修改

修改添加二合一表单：

```html
<!-- 需要区分是员工修改还是添加 -->
<form th:action="@{/emp}" method="post">
    <!-- 发送 PUT 请求修改员工数据 -->
    <!-- REST 使用方式：
         1、SpringMVC 中配置 HiddenHttpMethodFilter（Springboot 自动配置）；
         2、页面创建一个 POST 表单；
         3、创建一个 input 项，name="_method"，值就是我们指定的请求方式。
    -->
    <input type="hidden" name="_method" value="put" th:if="${emp!=null}"/>
    <input type="hidden" name="id" th:if="${emp!=null}" th:value="${emp.id}"/>
    <div class="form-group">
        <label>LastName</label>
        <input name="lastName" type="text" class="form-control" placeholder="zhangsan" th:value="${emp!=null}?${emp.lastName}">
    </div>
    <div class="form-group">
        <label>Email</label>
        <input name="email" type="email" class="form-control" placeholder="zhangsan@atguigu.com" th:value="${emp!=null}?${emp.email}">
    </div>
    <div class="form-group">
        <label>Gender</label><br/>
        <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" name="gender"  value="1" th:checked="${emp!=null}?${emp.gender==1}">
            <label class="form-check-label">男</label>
        </div>
        <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" name="gender"  value="0" th:checked="${emp!=null}?${emp.gender==0}">
            <label class="form-check-label">女</label>
        </div>
    </div>
    <div class="form-group">
        <label>department</label>
        <!-- 提交的是部门的 id -->
        <select class="form-control" name="department.id">      
            <option th:selected="${emp!=null}?${dept.id}==${emp.department.id}" th:value="${dept.id}" th:each="dept:${depts}" th:text="${dept.departmentName}">1</option>
        </select>
    </div>
    <div class="form-group">
        <label>Birth</label>
        <input name="birth" type="text" class="form-control" placeholder="zhangsan" th:value="${emp!=null}?${#dates.format(emp.birth, 'yyyy-MM-dd HH:mm')}">
    </div>
    <button type="submit" class="btn btn-primary" th:text="${emp!=null}?'修改':'添加'">添加</button>
</form>
```

### 7.5 CRUD-员工删除

如果报错：

```java
org.springframework.web.HttpRequestMethodNotSupportedException: Request method 'POST' not supported
```

那么需要在 **application.properties** 中配置：

```properties
# 启用 hiddenMethod 过滤器
spring.mvc.hiddenmethod.filter.enabled=true
<tr th:each="emp:${emps}">
    <td th:text="${emp.id}"></td>
    <td>[[${emp.lastName}]]</td>
    <td th:text="${emp.email}"></td>
    <td th:text="${emp.gender}==0?'女':'男'"></td>
    <td th:text="${emp.department.departmentName}"></td>
    <td th:text="${#dates.format(emp.birth, 'yyyy-MM-dd HH:mm')}"></td>
    <td>
        <a class="btn btn-sm btn-primary" th:href="@{/emp/}+${emp.id}">编辑</a>
        <button th:attr="del_uri=@{/emp/}+${emp.id}" class="btn btn-sm btn-danger deleteBtn">删除</button>
    </td>
</tr>
 
<script>
    $(".deleteBtn").click(function(){
        // 删除当前员工
        $("#deleteEmpForm").attr("action", $(this).attr("del_uri")).submit();
        return false;
    });
</script>
```

## 8、拦截器

### 8.1 `HandlerInterceptor` 接口

```Java
/**
 * 登录检查：
 * 1、配置好拦截器要拦截哪些请求；
 * 2、把这些配置放在容器中。
 */
@Slf4j
public class LoginInterceptor implements HandlerInterceptor {
    // 目标方法执行之前
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        String requestURI = request.getRequestURI();
        log.info("preHandle 拦截的请求路径是 {}",requestURI);
 
        // 登录检查逻辑
        HttpSession session = request.getSession();
        Object loginUser = session.getAttribute ("loginUser");
        if (loginUser != null){
            // 放行
            return true;
        }
 
        // 未登录：拦截住请求，然后跳转到登录页
        request.setAttribute("msg","请先登录");
        // request.sendRedirect("/");
        request.getRequestDispatcher("/").forward(request,response);
        return false;
    }
 
    // 目标方法执行完成以后
    @Override
    public void postHandle (HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        log.info ("postHandle 执行 {}", modelAndView);
    }
 
    // 页面渲染完成以后
    @Override
    public void afterCompletion (HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        log.info ("afterCompletion 执行异常 {}",ex);
    }
}
```

### 8.2 配置拦截器

```Java
/**
 * 1、编写一个拦截器实现 HandlerInterceptor 接口
 * 2、拦截器注册到容器中（实现 WebMvcConfigurer 的 addInterceptors 方法）
 * 3、指定拦截规则（如果是拦截所有，静态资源也会被拦截）
 */
@Configuration
public class AdminWebConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors (InterceptorRegistry registry) {
        registry.addInterceptor (new LoginInterceptor ())
                .addPathPatterns ("/**")  // 所有请求都被拦截包括静态资源
                .excludePathPatterns ("/","/login","/css/**","/fonts/**","/images/**","/js/**"); // 放行的请求
    }
}
```

### 8.3 拦截器原理

1. 根据当前请求，找到 `HandlerExecutionChain`（可以处理请求的 handler 以及 handler 的所有拦截器）；

   ![mappedHandler](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1605764129365-5b31a748-1541-4bee-9692-1917b3364bc6.png)

2. 先来**顺序执行**所有拦截器的 `preHandle` 方法；

   ![preHandle](https://cdn.nlark.com/yuque/0/2021/png/1304385/1626617930537-72c79112-cb73-47e3-a35f-66f93a8b09a1.png)

- 如果当前拦截器 `prehandler` 返回 `true`，则**执行**下一个拦截器的 `preHandle`；
- 如果当前拦截器 `prehandler` 返回 `false`，直接**倒序触发**所有**已经执行**了的拦截器的 `afterCompletion`；

1. 如果任何一个拦截器返回 `false`，直接**跳出**，**不执行**目标方法；
2. 如果所有拦截器都返回 `true`，**执行**目标方法；
3. **倒序执行**所有拦截器的 `postHandle` 方法；
4. 前面的步骤有任何异常，都会直接**倒序触发** `afterCompletion`；
5. 页面成功渲染完成以后，也会**倒序触发** `afterCompletion`。

![拦截器执行顺序](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1605765121071-64cfc649-4892-49a3-ac08-88b52fb4286f.png)

## 9、文件上传

一个文件上传的过程如下图所示：

![文件上传过程](https://img2018.cnblogs.com/blog/242916/201811/242916-20181110161028169-1735049652.png)

1. 浏览器发起 **HTTP POST** 请求，指定请求头：
   ***Content-Type: multipart/form-data***
2. 服务端解析请求内容，执行文件保存处理，返回成功消息。

参考：[文件上传原理](https://www.cnblogs.com/littleatp/p/9939705.html)

### 9.1 页面表单

```html
<form method="post" action="/upload" enctype="multipart/form-data">
    <input type="file" name="file"><br>
    <input type="submit" value="提交">
</form>
```

### 9.2 限制上传文件大小

```yaml
spring:
  servlet:
    multipart:
      max-file-size: 10MB # 单个上传文件大小上限
      max-request-size: 100MB # 一次请求上传所有文件大小上限
```

### 9.3 文件上传代码

```Java
// MultipartFile 自动封装上传过来的文件
@PostMapping("/upload")
public String upload(@RequestParam("email") String email,
                     @RequestParam("username") String username,
                     @RequestPart("headerImg") MultipartFile headerImg,
                     @RequestPart("photos") MultipartFile[] photos) throws IOException {
    log.info("上传的信息：email={}，username={}，headerImg={}，photos={}",
            email, username, headerImg.getSize(), photos.length);
 
    if(!headerImg.isEmpty()){
        // 保存到文件服务器，OSS服务器
        String originalFilename = headerImg.getOriginalFilename();
        headerImg.transferTo(new File("D:\\cache\\"+originalFilename));
    }
 
    if(photos.length > 0){
        for (MultipartFile photo : photos) {
            if(!photo.isEmpty()){
                String originalFilename = photo.getOriginalFilename();
                photo.transferTo(new File("D:\\cache\\" + originalFilename));
            }
        }
    }
 
    return "main";
}
```

### 9.4 自动配置原理

文件上传自动配置类 `MultipartAutoConfiguration` 自动配置好了文件上传解析器 `StandardServletMultipartResolver`（使用 Servlet 所提供的功能支持，不需要依赖任何其他的项目） 。

原理步骤：

1. `request` 请求进来使用文件上传解析器判断（`isMultipart(request`）并封装（`resolveMultipart(request)`），返回（`MultipartHttpServletRequest`）文件上传请求；
2. 参数解析器 `RequestPartMethodArgumentResolver` 来解析请求中的文件内容并封装成 `MultipartFile`（上传文件的详细信息，如原始文件名、内容类型、大小等等）；
3. 将 `request` 中的文件信息封装为一个 `MultiValueMap<String, MultipartFile>`；
4. 遍历 `MultiValueMap` 对于每一个 `MultipartFile` 调用 `FileCopyUtils.copy()` 实现文件流的拷贝。

## 10、异常处理

### 10.1 默认规则

默认情况下，SpringBoot 提供 `/error` 处理所有错误的映射。

- 对于浏览器客户端，响应一个 **Whitelabel** 错误页面（因为浏览器请求头的 **Accept** 字段默认以 **text/html** 开头），中包含错误，**HTTP** 状态和异常消息的详细信息；

  ![html 响应](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606024616835-bc491bf0-c3b1-4ac3-b886-d4ff3c9874ce.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_28%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

- 对于其他客户端（如 **Postman**），它将生成 **json** 响应。

  ![json 响应](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1606024421363-77083c34-0b0e-4698-bb72-42da351d3944.png)

要对其进行自定义，添加 `View` 解析为 `error`

要完全替换默认行为，可以实现 `ErrorController` 并注册该类型的 `Bean` 定义，或添加 `ErrorAttributes` 类型的组件以使用现有机制但替换其内容。

### 10.2 定制异常处理逻辑

**error/404.html**、**error/5xx.html**：有精确的错误状态码页面就匹配精确，没有就找 **4xx.html**，如果都没有就触发白页。

自定义错误页：

1. `@ControllerAdvice` + `@ExceptionHandler` 处理全局异常，底层是 `ExceptionHandlerExceptionResolver` 支持的。

2. 自定义实现 `HandlerExceptionResolver` 处理异常，可以作为默认的全局异常处理规则。

   ![HandlerExceptionResolver](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606114688649-e6502134-88b3-48db-a463-04c23eddedc7.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_16%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

3. `ErrorViewResolver` 实现自定义异常处理。

- 自己调用 `response.sendError(statusCode, resolvedReason)`，**error** 请求就会转给 `BasicErrorController`。
- 如果自己没有调用，并且异常没有处理器能处理，**Tomcat** 底层调用 `response.sendError(statusCode, resolvedReason)`，**error** 请求就会转给 `BasicErrorController`。
- `BasicErrorController` 要去的页面地址是 `ErrorViewResolver` 。

1. `@ResponseStatus` + 自定义异常

   底层是 `ResponseStatusExceptionResolver`，被 `ResponseStatus` 注解的元素会使**Tomcat** 底层调用 `response.sendError(statusCode, resolvedReason)`。

   ```java
   response.sendError(HttpServletResponse.SC_BAD_REQUEST, ex.getMessage());
   ```

   ![404 页面](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606114118010-f4aaf5ee-2747-4402-bc82-08321b2490ed.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_19%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

### 10.3 异常处理自动配置原理

- `ErrorMvcAutoConfiguration` 自动配置异常处理规则。

  1. 定义错误页面属性：组件类型为 `DefaultErrorAttributes`，组件 ID 为 `errorAttributes`。

     - `public class DefaultErrorAttributes implements ErrorAttributes, HandlerExceptionResolver`。

  2. 定义页面跳转逻辑：组件类型为 `BasicErrorController`，组件 ID 为`basicErrorController`（**json** + 白页 适配响应）。

     - 处理默认 **/error** 路径的请求，页面响应 `new ModelAndView("error", model)`。

       ```Java
       @Controller
       @RequestMapping("${server.error.path:${error.path:/error}}")
       public class BasicErrorController extends AbstractErrorController {
           // 产生 html 类型的数据，浏览器发送的请求来到这个方法处理
           @RequestMapping(produces = "text/html")
           public ModelAndView errorHtml(HttpServletRequest request,
                   HttpServletResponse response) {
               HttpStatus status = getStatus(request);
               Map<String, Object> model = Collections.unmodifiableMap(getErrorAttributes(
                       request, isIncludeStackTrace(request, MediaType.TEXT_HTML)));
               response.setStatus(status.value());
               
               // 去哪个页面作为错误页面，包含页面地址和页面内容
               ModelAndView modelAndView = resolveErrorView(request, response, status, model);
               return (modelAndView == null ? new ModelAndView("error", model) : modelAndView);
           }
        
           // 产生 json 数据，其他客户端来到这个方法处理
           @RequestMapping
           @ResponseBody
           public ResponseEntity<Map<String, Object>> error(HttpServletRequest request) {
               Map<String, Object> body = getErrorAttributes(request,
                       isIncludeStackTrace(request, MediaType.ALL));
               HttpStatus status = getStatus(request);
               return new ResponseEntity<Map<String, Object>>(body, status);
           }
       ```

     - 容器中有组件 `View`，组件 ID 为 `error`（响应默认错误页）

     - 容器中放组件 `BeanNameViewResolver`（视图解析器），按照返回的视图名作为组件 ID 去容器中找 `View` 对象。

  3. 定义错误页面 **html** 路径：类型为 `DefaultErrorViewResolver`，组件 ID 为 `conventionErrorViewResolver`。

     - 如果发生错误，会以 **HTTP** 状态码作为视图页地址（`viewName`），找到真正的页面。
     - **error/404.html**、**error/5xx.html**。

### 10.4 异常处理流程

1. 执行目标方法，目标方法运行期间有任何异常都会被 `catch` 捕捉，而且标志当前请求结束，并且将异常赋值给 `dispatchException`。

2. 进入视图解析流程：

   `processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);`

3. `mv = processHandlerException` 处理 handler 发生的异常，处理完成返回 `ModelAndView`。

   1. 遍历所有的 `handlerExceptionResolvers` 处理器异常解析器**，**看谁能处理当前异常。

   2. 系统默认的异常解析器：

      ![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606047109161-c68a46c1-202a-4db1-bbeb-23fcae49bbe9.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_17%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

      1. `DefaultErrorAttributes`先来处理异常，把异常信息保存到 **request** 域，并且返回 `null`；

      2. 然后交由

          

         ```
         HandlerExceptionResolverComposite
         ```

         ，遍历其中的异常处理器，默认没有处理器能处理异常，所以异常会被抛出。

         1. 如果异常没有处理器能处理，最终 **Tomcat** 底层 就会发送 `/error` 请求，会被底层的 `BasicErrorController` 处理。
         2. 解析错误视图：遍历所有的 `ErrorViewResolver` 看谁能解析。
         3. 默认的 `DefaultErrorViewResolver`，作用是把响应状态码作为错误页的地址，**error/500.html** 。
         4. 模板引擎最终响应这个页面 **error/500.html**。

## 11、Web 原生组件注入（`Servlet`、`Filter`、`Listener`）

### 11.1 使用 Servlet API

- `@ServletComponentScan(basePackages = "com.atguigu.admin")`：指定原生 `Servlet` 组件都放在哪里。
- `@WebServlet(urlPatterns = "/my")`：效果，直接响应，没有经过 Spring 的拦截器？
- `@WebFilter(urlPatterns={"/css/*", "/images/*"})`：过滤 css、images 静态资源。
- `@WebListener`：监听器。

扩展：`DispatchServlet` 如何注册进来？

- 容器中自动配置了 `DispatcherServlet` 属性绑定到 `WebMvcProperties`，对应的配置文件配置项是 `spring.mvc`。
- 通过 `ServletRegistrationBean<DispatcherServlet>` 把 `DispatcherServlet` 配置进来。
- 默认映射的是 `/` 路径。

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606284869220-8b63d54b-39c4-40f6-b226-f5f095ef9304.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_32%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

多个 `Servlet` 都能处理到同一层路径，精确优选原则：

A： /my/

B： /my/1 优先

### 11.2 使用 RegistrationBean

```
ServletRegistrationBean`, `FilterRegistrationBean`, and `ServletListenerRegistrationBean
@Configuration
public class MyRegistConfig {
    @Bean
    public ServletRegistrationBean myServlet(){
        MyServlet myServlet = new MyServlet();
        return new ServletRegistrationBean(myServlet,"/my","/my02");
    }
 
    @Bean
    public FilterRegistrationBean myFilter(){
        MyFilter myFilter = new MyFilter();
//        return new FilterRegistrationBean(myFilter,myServlet());
        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean(myFilter);
        filterRegistrationBean.setUrlPatterns(Arrays.asList("/my","/css/*"));
        return filterRegistrationBean;
    }
 
    @Bean
    public ServletListenerRegistrationBean myListener(){
        MySwervletContextListener mySwervletContextListener = new MySwervletContextListener();
        return new ServletListenerRegistrationBean(mySwervletContextListener);
    }
}
```

## 12、配置 `Servlet` 容器

### 12.1 切换嵌入式Servlet容器

SpringBoot 内置的 WebServer：`Tomcat`（默认）, `Jetty`, or `Undertow`。

![SpringBoot 内置的 WebServer](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606280937533-504d0889-b893-4a01-af68-2fc31ffce9fc.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_26%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

`ServletWebServerApplicationContext` 容器启动寻找 `ServletWebServerFactory` 并引导创建服务器。

切换服务器：在 **pom.xml** 中排除 **Tomcat** 依赖，再将目标服务器的 starter 导入即可。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

原理：

SpringBoot 应用启动发现当前是 **Web** 应用，`web-starter` 自动导入**Tomcat**：

1. **Web** 应用会创建一个 **Web** 版的 `IoC` 容器 `ServletWebServerApplicationContext`

2. `ServletWebServerApplicationContext` 启动的时候寻找 `ServletWebServerFactory`（`Servlet` 的 **Web** 服务器工厂 ---> `Servlet` 的 **Web** 服务器）；

   ![ServletWebServerFactory](https://cdn.nlark.com/yuque/0/2021/png/1304385/1627120946492-5ee582f2-7f37-4cc5-9071-2cafac6171b4.png)

3. SpringBoot 底层默认有很多的 **WebServer** 工厂：`TomcatServletWebServerFactory`, `JettyServletWebServerFactory`, or `UndertowServletWebServerFactory`；

4. 底层直接会有一个 **WebServer** 工厂自动配置类：`ServletWebServerFactoryAutoConfiguration`；

5. **WebServer** 工厂自动配置类 `ServletWebServerFactoryAutoConfiguration` 导入了 `ServletWebServerFactoryConfiguration` 配置类；

6. `ServletWebServerFactoryConfiguration` 配置类动态判断系统中到底导入了哪个 **Web** 服务器的包， `web-starter` 默认是导入 **Tomcat** 包，容器中就有 `TomcatServletWebServerFactory`；

7. `TomcatServletWebServerFactory` 创建 **Tomcat** 对象，然后将 **Tomcat** 对象传递给 `TomcatWebServer` ，

   ![getWebServer](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1627121101649-b2304f72-a57b-428b-a1ad-ea16a98d9292.png)

   `TomcatWebServer` 的构造器拥有初始化方法 `initialize()`，使用 `this.tomcat.start();` 启动 **Tomcat**（只要 **Tomcat** 核心 **jar** 包存在，内嵌服务器就是手动把启动服务器的代码调用）。

### 12.2 定制 `Servlet` 容器

1. 在 **application.properties** 中修改和 **server** 有关的配置（`ServerProperties` 提供）；

```properties
server.port=8081
server.context-path=/crud
server.tomcat.uri-encoding=UTF‐8
# 通用的 Servlet 容器设置
server.xxx
# Tomcat 的设置
server.tomcat.xxx
```

1. 实现 `WebServerFactoryCustomizer<ConfigurableServletWebServerFactory>`

   - 把配置文件的值和 `ServletWebServerFactory` 进行绑定；

     ```Java
     @Component
     public class CustomizationBean implements WebServerFactoryCustomizer<ConfigurableServletWebServerFactory> {
         @Override
         public void customize(ConfigurableServletWebServerFactory server) {
             server.setPort(9000);
         }
     }
     ```

2. 直接自定义 `ConfigurableServletWebServerFactory`

   ```Java
   @Bean
   public WebServerFactoryCustomizer configWebServer(){
       WebServerFactoryCustomizer webServerFactoryCustomizer = new WebServerFactoryCustomizer<ConfigurableServletWebServerFactory>() {
           @Override
           public void customize(ConfigurableServletWebServerFactory factory) {
               factory.setPort(9000);
           }
       };
       return webServerFactoryCustomizer;
   }
   ```

# 六、Docker

## 1、简介

**Docker** 是一个开源的应用容器引擎，是一个轻量级容器技术。

**Docker** 支持将软件编译成一个镜像，然后在镜像中各种软件做好配置，将镜像发布出去，其他使用者可以直接使用这个镜像。

运行中的这个镜像称为容器，容器启动是非常快速的。

![Docker 鲸鱼图标](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1596209109779-61872f1d-b724-4151-b1f8-912538ae4570.png)

**Windows** 系统镜像与 **Docker** 镜像：

![Windows 系统镜像与 Docker 镜像](https://cdn.nlark.com/yuque/0/2020/png/1304385/1596209109769-4a6d33ca-4577-4515-8a8d-fc506e7d0465.png)

## 2、核心概念

- 主机（Host）：安装了 **Docker** 程序的机器（**Docker** 直接安装在 **Linux** 或 **Windows** 操作系统之上）。
- 客户端（Client）：连接 **Docker** 主机进行操作。
- 仓库（Registry）：用来保存各种打包好的软件镜像。
- 镜像（Images）：软件打包好的镜像，放在 **Docker** 仓库中。
- 容器（Container）：镜像启动后的实例称为一个容器，容器是独立运行的一个或一组应用。

![Docker 核心概念](https://cdn.nlark.com/yuque/0/2020/png/1304385/1596209109798-7ec9e089-41ca-47f8-9d7e-4abe15124a1d.png)

使用 **Docker** 的步骤：

1. 安装 **Docker**；
2. 去 **Docker** 仓库获取软件对应的镜像；
3. 使用 **Docker** 运行这个镜像，这个镜像就会生成一个 **Docker** 容器；
4. 停止 **Docker** 容器就是停止软件。

## 3、安装Docker

### 3.1 安装 WSL2 版 Linux

[【Windows】WSL 体验](https://www.cnblogs.com/ageovb/p/15072680.html)

### 3.2 Linux 上安装 Docker

[【Linux】安装 Docker](https://www.cnblogs.com/ageovb/p/15086255.html)

## 4、Docker 常用命令

### 4.1 镜像操作

| 操作 | 命令                     | 说明                                                         |
| ---- | ------------------------ | ------------------------------------------------------------ |
| 检索 | `docker search 镜像名`   | 我们经常去 Docker Hub 上检索镜像的详细信息，如镜像的 **tag** 标签。 |
| 拉取 | `docker pull 镜像名:tag` | **:tag** 是可选的，tag 多为软件的版本，默认是 **latest** 最新版。 |
| 列表 | `docker images`          | 查看所有本地镜像。                                           |
| 删除 | `docker rmi 容器ID`      | 删除指定的本地镜像。                                         |

https://hub.docker.com/

### 4.2 容器操作

软件镜像（QQ 安装程序）-> 运行镜像（双击 QQ 图标） -> 产生一个容器（正在运行的软件，如运行的 QQ）。

步骤：

1. 搜索镜像

   ```bash
   [root@localhost ~]# docker search tomcat
   ```

2. 拉取镜像

   ```bash
   [root@localhost ~]# docker pull tomcat
   ```

3. 根据镜像启动容器

   ```bash
   [root@localhost ~]# docker run --name myTomcat -d tomcat:latest
   ```

4. 查看运行中的容器

   ```bash
   [root@localhost ~]# docker ps
   ```

5. 停止运行中的容器

```bash
[root@localhost ~]# docker stop 容器ID
```

1. 查看所有的容器

   ```bash
   [root@localhost ~]# docker ps -a
   ```

2. 启动容器

   ```bash
   [root@localhost ~]# docker start 容器ID
   ```

3. 删除一个容器

   ```bash
   [root@localhost ~]# docker rm 容器ID
   ```

4. 启动一个做了端口映射的 **Tomcat**

   ```bash
   [root@localhost ~]# docker run -d -p 8888:8080 tomcat
   ```

   - `-d`：后台运行
   - `-p`：将主机的端口映射到容器的端口，主机的端口:容器的端口

5. 为了演示简单关闭了 `Linux` 的防火墙

   - 查看防火墙状态：

   ```bash
   [root@localhost ~]# service firewalld status
   ```

   - 关闭防火墙

   ```bash
   [root@localhost ~]# service firewalld stop
   ```

6. 查看容器的日志

   ```bash
   [root@localhost ~]# docker logs container-name/container-id
   ```

更多命令参考：https://docs.docker.com/engine/reference/commandline/docker/

还可以参考每一个镜像的文档。

### 4.3 安装 MySQL

拉取镜像

```bash
[root@localhost ~]# docker pull mysql
```

错误的启动：

```bash
[root@localhost ~]# docker run --name mysql01 -d mysql
42f09819908bb72dd99ae19e792e0a5d03c48638421fa64cce5f8ba0f40f5846
 
# MySQL 退出了
[root@localhost ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                           PORTS               NAMES
42f09819908b        mysql               "docker-entrypoint.sh"   34 seconds ago      Exited (1) 33 seconds ago                            mysql01
538bde63e500        tomcat              "catalina.sh run"        About an hour ago   Exited (143) About an hour ago                       compassionate_
goldstine
c4f1ac60b3fc        tomcat              "catalina.sh run"        About an hour ago   Exited (143) About an hour ago                       lonely_fermi
81ec743a5271        tomcat              "catalina.sh run"        About an hour ago   Exited (143) About an hour ago                       sick_ramanujan
 
# 根据容器 ID 查看错误日志，下面三个参数必须指定一个
[root@localhost ~]# docker logs 42f09819908b
error: database is uninitialized and password option is not specified 
  You need to specify one of MYSQL_ROOT_PASSWORD, MYSQL_ALLOW_EMPTY_PASSWORD and MYSQL_RANDOM_ROOT_PASSWORD。
```

正确的启动：

```bash
[root@localhost ~]# docker run --name mysql01 -e MYSQL_ROOT_PASSWORD=123456 -d mysql
b874c56bec49fb43024b3805ab51e9097da779f2f572c22c695305dedd684c5f
[root@localhost ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
b874c56bec49        mysql               "docker-entrypoint.sh"   4 seconds ago       Up 3 seconds        3306/tcp            mysql01
```

端口映射，将主机的端口映射到容器的端口：

```bash
[root@localhost ~]# docker run -p 3306:3306 --name mysql02 -e MYSQL_ROOT_PASSWORD=123456 -d mysql
ad10e4bc5c6a0f61cbad43898de71d366117d120e39db651844c0e73863b9434
[root@localhost ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
ad10e4bc5c6a        mysql               "docker-entrypoint.sh"   4 seconds ago       Up 2 seconds        0.0.0.0:3306->3306/tcp   mysql02
```

远程 **MySQL** 连接不上的解决方法：

```bash
[root@localhost ~]# docker run -p 3306:3306 --name mysql01 -e MYSQL_ROOT_PASSWORD=123456 -d mysql
18e7e4fc89685c1fb8fac0c999d41f67bcd1c993b6597b1ecd514a121e132c5b
[root@localhost ~]# docker exec -it mysql01 bash
root@18e7e4fc8968:/# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.19 MySQL Community Server - GPL
 
Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.
 
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
 
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
 
mysql> alter user 'root'@'%' identified with mysql_native_password by 'test1234';
Query OK, 0 rows affected (0.02 sec)
 
mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
 
# Ctrl+D退出
```

挂载文件夹：

```bash
[root@localhost ~]# docker run --name mysql03 -v /conf/mysql:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
```

把主机的 **/conf/mysql** 文件夹挂载到容器的 **/etc/mysql/conf.d** 文件夹里面。

修改 **MySQL** 的配置文件就只需要把 **MySQL** 配置文件放在主机的 **/conf/mysql** 文件夹下）。

指定 **MySQL** 的一些配置参数：

```bash
[root@localhost ~]# docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

### 4.4 安装 Redis

[【Linux】Docker 中安装并配置 Redis](https://www.cnblogs.com/ageovb/p/15086265.html)

# 七、数据访问

## 1、SQL

### 1.1 数据源的自动配置

SpringBoot 默认是用 `HikariDataSource` 数据源（数据库连接池）。

#### 1.1.1 导入 JDBC 场景

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jdbc</artifactId>
</dependency>
```

![JDBC 场景的 jar 包](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1606366100317-5e0199fa-6709-4d32-bce3-bb262e2e5e6a.png)

为什么需要显式导入 **JDBC** 场景，官方不导入数据库驱动？因为官方不知道我们接下要操作什么数据库。

数据库版本和驱动版本对应：

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.49</version>
</dependency>
 
<!-- 想要修改版本
1、直接依赖引入具体版本（Maven 的就近依赖原则）
2、重新声明版本（Maven 的属性的就近优先原则） -->
<properties>
    <java.version>1.8</java.version>
    <mysql.version>5.1.49</mysql.version>
</properties>
```

#### 1.1.2 分析自动配置

自动配置的类：

- ```
  DataSourceAutoConfiguration
  ```

  ： 数据源的自动配置；

  - 修改数据源相关的配置：**spring.datasource**；
  - 数据库连接池的配置，是容器中没有 `DataSource` 才自动配置的；
  - 底层配置好的数据库连接池是：**HikariDataSource**。

```Java
@Configuration(proxyBeanMethods = false)
@Conditional(PooledDataSourceCondition.class)
@ConditionalOnMissingBean({ DataSource.class, XADataSource.class })
@Import({ DataSourceConfiguration.Hikari.class, DataSourceConfiguration.Tomcat.class,
        DataSourceConfiguration.Dbcp2.class, DataSourceConfiguration.OracleUcp.class,
        DataSourceConfiguration.Generic.class, DataSourceJmxConfiguration.class })
protected static class PooledDataSourceConfiguration
```

- `JdbcTemplateAutoConfiguration`： `JdbcTemplate` 的自动配置，可以来对数据库进行 **CRUD**；
  - 可以修改这个配置项`@ConfigurationProperties(prefix = "spring.jdbc")` 来修改 `JdbcTemplate`；
  - `@Bean`、`@Primary`修饰 `JdbcTemplate`，从容器中获取这个组件。
- `JndiDataSourceAutoConfiguration`： **JNDI** 的自动配置；
- `DataSourceTransactionManagerAutoConfiguration`： 事务管理器的自动配置；
- `XADataSourceAutoConfiguration`： 分布式事务相关的自动配置。

#### 1.1.3 修改配置项

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/db_account?serverTimezone=GMT%2B8
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
```

#### 1.1.4 测试

```Java
@Slf4j
@SpringBootTest
class Boot05WebAdminApplicationTests {
    @Autowired
    JdbcTemplate jdbcTemplate;
 
    @Test
    void contextLoads() {
        Long aLong = jdbcTemplate.queryForObject("select count(*) from account_tbl", Long.class);
        log.info("记录总数：{}",aLong);
    }
}
```

### 1.2 使用 Druid 数据源

#### 1.2.1 Druid 官方 Github 地址

https://github.com/alibaba/druid

整合第三方技术的两种方式

- 自定义；
- 使用官方 **starter**。

#### 1.2.2 自定义方式

**1、创建数据源**

这种方式可能无法使用。

```xml
<!-- pom.xml -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.17</version>
</dependency>
 
<!-- bean.xml -->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
      destroy-method="close">
    <property name="url" value="${jdbc.url}" />
    <property name="username" value="${jdbc.username}" />
    <property name="password" value="${jdbc.password}" />
    <property name="maxActive" value="20" />
    <property name="initialSize" value="1" />
    <property name="maxWait" value="60000" />
    <property name="minIdle" value="1" />
    <property name="timeBetweenEvictionRunsMillis" value="60000" />
    <property name="minEvictableIdleTimeMillis" value="300000" />
    <property name="testWhileIdle" value="true" />
    <property name="testOnBorrow" value="false" />
    <property name="testOnReturn" value="false" />
    <property name="poolPreparedStatements" value="true" />
    <property name="maxOpenPreparedStatements" value="20" />
```

> Spring Boot 2.5.2 版本导入 **druid 1.1.17** 依赖之后并不能使用监控功能，使用相同版本的 **druid-spring-boot-starter** 之后可以使用监控功能。

2、`StatViewServlet`

用途：

- 提供监控信息展示的 **HTML** 页面；
- 提供监控信息的 **JSON API**。

```xml
<servlet>
    <servlet-name>DruidStatView</servlet-name>
    <servlet-class>com.alibaba.druid.support.http.StatViewServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>DruidStatView</servlet-name>
    <url-pattern>/druid/*</url-pattern>
</servlet-mapping>
```

3、`StatFilter`

用于统计监控信息：如 **SQL** 监控、**URI** 监控。

需要给数据源中配置如下属性，可以允许多个 **Filter**，多个用 `,` 分割；如：

```xml
<property name="filters" value="stat, slf4j" />
```

系统中的 **Filter**：

| 别名          | Filter 类名                                               |
| ------------- | --------------------------------------------------------- |
| default       | com.alibaba.druid.filter.stat.`StatFilter`                |
| stat          | com.alibaba.druid.filter.stat.`StatFilter`                |
| mergeStat     | com.alibaba.druid.filter.stat.`MergeStatFilter`           |
| encoding      | com.alibaba.druid.filter.encoding.`EncodingConvertFilter` |
| log4j         | com.alibaba.druid.filter.logging.`Log4jFilter`            |
| log4j2        | com.alibaba.druid.filter.logging.`Log4j2Filter`           |
| slf4j         | com.alibaba.druid.filter.logging.`Slf4jLogFilter`         |
| commonlogging | com.alibaba.druid.filter.logging.`CommonsLogFilter`       |

使用 `slowSqlMillis` 定义慢 **SQL** 的时长：

```xml
<bean id="stat-filter" class="com.alibaba.druid.filter.stat.StatFilter">
    <property name="slowSqlMillis" value="10000" />
    <property name="logSlowSql" value="true" />
</bean>
```

#### 1.2.3 使用官方 starter 方式

1、引入 druid-starter

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.17</version>
</dependency>
```

2、分析自动配置

- 扩展配置项 **spring.datasource.druid**
- `DruidSpringAopConfiguration`.class, 监控 **Bean** 的配置项：**spring.datasource.druid.aop-patterns**
- `DruidStatViewServletConfiguration`.class，默认开启监控页的配置：**spring.datasource.druid.stat-view-servlet**
- `DruidWebStatFilterConfiguration`.class，默认开启 **Web** 监控配置；**spring.datasource.druid.web-stat-filter**
- `DruidFilterConfiguration`.class，所有 **Druid** 自己 **Filter** 的配置。

```Java
private static final String FILTER_STAT_PREFIX = "spring.datasource.druid.filter.stat";
private static final String FILTER_CONFIG_PREFIX = "spring.datasource.druid.filter.config";
private static final String FILTER_ENCODING_PREFIX = "spring.datasource.druid.filter.encoding";
private static final String FILTER_SLF4J_PREFIX = "spring.datasource.druid.filter.slf4j";
private static final String FILTER_LOG4J_PREFIX = "spring.datasource.druid.filter.log4j";
private static final String FILTER_LOG4J2_PREFIX = "spring.datasource.druid.filter.log4j2";
private static final String FILTER_COMMONS_LOG_PREFIX = "spring.datasource.druid.filter.commons-log";
private static final String FILTER_WALL_PREFIX = "spring.datasource.druid.filter.wall";
```

3、配置示例

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/db_account?serverTimezone=GMT%2B8
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
 
    druid:
      aop-patterns: com.atguigu.admin.*  # 监控 Bean
      filters: stat,wall     # 底层开启功能：stat（sql 监控），wall（防火墙）
 
      stat-view-servlet:   # 配置监控页功能
        enabled: true
        login-username: admin
        login-password: admin
        resetEnable: false
 
      web-stat-filter:  # Web 监控
        enabled: true
        urlPattern: /*
        exclusions: '*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*'
 
      filter:
        stat:    # 对上面 filters 里面的 stat 的详细配置
          slow-sql-millis: 1000
          logSlowSql: true
          enabled: true
        wall:
          enabled: true
          config:
            drop-table-allow: false
```

SpringBoot 配置示例：

https://github.com/alibaba/druid/tree/master/druid-spring-boot-starter

配置项列表：[https://github.com/alibaba/druid/wiki/DruidDataSource%E9%85%8D%E7%BD%AE%E5%B1%9E%E6%80%A7%E5%88%97%E8%A1%A8](https://github.com/alibaba/druid/wiki/DruidDataSource配置属性列表)

### 1.3 整合 MyBatis 操作

https://github.com/mybatis

引入 **MyBatis** 的官方 **starter**：

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.4</version>
</dependency>
```

![MyBatis 的 jar 包](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1606704096118-53001250-a04a-4210-80ee-6de6a370be2e.png)

#### 1.3.1 配置模式

- 配置 **MyBatis** 的配置文件，**SqlMapConfig.xml**（名称不固定）；
- 通过配置文件，加载 **MyBatis** 运行环境，创建 `SqlSessionFactory` 会话工厂，`SqlSessionFactory` 使用单例方式；
- 通过 `SqlSessionFactory` 创建 `SqlSession`，`SqlSession` 是一个面向用户的接口（提供操作数据库方法），实现对象是线程不安全的，建议 `sqlSession` 应用在方法体内；
- 调用 `sqlSession` 的方法去操作数据，如果需要提交事务，需要执行 `sqlSession` 的 `commit()` 方法；
- 释放资源，关闭 `sqlSession`；

自动配置类：

```Java
// 表示这是一个 Spring 配置类
@Configuration 
// SqlSessionFactory 和 SqlSessionFactoryBean 存在时才生效
@ConditionalOnClass({SqlSessionFactory.class, SqlSessionFactoryBean.class})
// DataSource 的 Canidate 注册到 Spring 容器中时才生效
@ConditionalOnSingleCandidate(DataSource.class)
// 使 MybatisProperties 注解类生效
@EnableConfigurationProperties({MybatisProperties.class})
// 在 DataSourceAutoConfiguration 和 MybatisLanguageDriverAutoConfiguration 自动配置之后执行
@AutoConfigureAfter({DataSourceAutoConfiguration.class, MybatisLanguageDriverAutoConfiguration.class})
public class MybatisAutoConfiguration implements InitializingBean {
 
@ConfigurationProperties(prefix = "mybatis")
public class MybatisProperties
```

- `@Import(AutoConfiguredMapperScannerRegistrar.class)`；
- `Mapper`： 我们只要写操作 `MyBatis` 的接口，`@Mapper` 就会被自动扫描进来。

修改配置文件：

```yaml
mybatis:
  config-location: classpath:mybatis/mybatis-config.xml # 全局配置文件位置
  mapper-locations: classpath:mybatis/mapper/*.xml # sql 映射文件位置
```

`Mapper` 接口绑定 **xml**：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.atguigu.admin.mapper.AccountMapper">
    <!-- public Account getAcct(Long id); -->
    <select id="getAcct" resultType="com.atguigu.admin.bean.Account">
        select * from account_tbl where id = #{id}
    </select>
</mapper>
```

配置 **MyBatis** 规则：

```yaml
mybatis:
# config-location: classpath:mybatis/mybatis-config.xml
  mapper-locations: classpath:mybatis/mapper/*.xml
  configuration:
    map-underscore-to-camel-case: true
```

步骤：

1. 导入 **MyBatis** 官方 **starter**；
2. 编写 `Mapper` 接口，标准 `@Mapper` 注解；
3. 编写 **sql** 映射文件并绑定 `Mapper` 接口；
4. 在 **application.yaml **中指定 `Mapper` 配置文件的位置，以及指定全局配置文件的信息 （建议配置在 **mybatis.configuration**）。

#### 1.3.2 注解模式

```java
@Mapper
public interface CityMapper {
    @Select("select * from city where id=#{id}")
    public City getById(Long id);
 
    public void insert(City city);
}
```

#### 1.3.3 混合模式

```java
@Mapper
public interface CityMapper {
    @Select("select * from city where id=#{id}")
    public City getById(Long id);
 
    public void insert(City city);
}
```

#### 1.3.4 最佳实践

1. 引入 **mybatis-starter**；
2. 配置 **application.yaml** 中，指定 **mapper-location** 位置即可；
3. 编写 `Mapper` 接口并标注 `@Mapper` 注解；
4. 简单方法直接注解方式；
5. 复杂方法编写 **mapper.xml** 进行绑定映射；
6. `@MapperScan("com.atguigu.admin.mapper")` 标注在主应用类上，其他的接口就可以不用标注 `@Mapper` 注解。

### 1.4 整合 MyBatis-Plus 完成 CRUD

#### 1.4.1 什么是 MyBatis-Plus

[MyBatis-Plus](https://baomidou.com/)（简称 **MP**）是一个 [MyBatis](http://www.mybatis.org/mybatis-3/) 的增强工具，在 **MyBatis** 的基础上只做增强不做改变，为简化开发、提高效率而生。

建议安装 **MybatisX** 插件。

#### 1.4.2 整合 MyBatis-Plus

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.1</version>
</dependency>
```

自动配置：

- `MybatisPlusAutoConfiguration` 配置类，`MybatisPlusProperties` 配置项绑定；
- `SqlSessionFactory` 是自动配置好的，底层是容器中默认的数据源；
- `mapperLocations` 是自动配置好的，有默认值 `classpath*:/mapper/**/*.xml`，任意包的类路径下的所有 **mapper** 径下的所有 **xml** 都是 **sql** 映射文件。
- 容器中也自动配置好了 `SqlSessionTemplate`；
- `@Mapper` 标注的接口也会被自动扫描，建议直接 `@MapperScan("com.atguigu.admin.mapper")` 批量扫描就行。

**优点：**只需要我们的 `Mapper` 继承 `BaseMapper` 就可以拥有 **CRUD** 能力。

#### 1.4.3 CRUD 功能

```Java
@GetMapping("/user/delete/{id}")
public String deleteUser(@PathVariable("id") Long id,
                         @RequestParam(value = "pn",defaultValue = "1") Integer pn,
                         RedirectAttributes ra){
    userService.removeById(id);
    ra.addAttribute("pn", pn);
    return "redirect:/dynamic_table";
}
 
@GetMapping("/dynamic_table")
public String dynamic_table(@RequestParam(value="pn",defaultValue = "1") Integer pn,Model model){
    // 表格内容的遍历
    //        response.sendError
    //        List<User> users = Arrays.asList(new User("zhangsan", "123456"),
    //                new User("lisi", "123444"),
    //                new User("haha", "aaaaa"),
    //                new User("hehe ", "aaddd"));
    //        model.addAttribute("users",users);
    //
    //        if(users.size()>3){
    //            throw new UserTooManyException();
    //        }
    // 从数据库中查出user表中的用户进行展示
 
    // 构造分页参数
    Page<User> page = new Page<>(pn, 2);
    // 调用page进行分页
    Page<User> userPage = userService.page(page, null);
    //        userPage.getRecords()
    //        userPage.getCurrent()
    //        userPage.getPages()
    model.addAttribute("users",userPage);
    return "table/dynamic_table";
}
@Service
public class UserServiceImpl extends ServiceImpl<UserMapper,User> implements UserService {}
 
public interface UserService extends IService<User> {}
```

## 2、NoSQL

**Redis** 是一个开源（**BSD** 许可）的，内存中的数据结构存储系统，它可以用作数据库、**缓存**和消息中间件。 它支持多种类型的数据结构，如 [字符串（strings）](http://www.redis.cn/topics/data-types-intro.html#strings)， [散列（hashes）](http://www.redis.cn/topics/data-types-intro.html#hashes)， [列表（lists）](http://www.redis.cn/topics/data-types-intro.html#lists)， [集合（sets）](http://www.redis.cn/topics/data-types-intro.html#sets)， [有序集合（sorted sets）](http://www.redis.cn/topics/data-types-intro.html#sorted-sets) 与范围查询， [bitmaps](http://www.redis.cn/topics/data-types-intro.html#bitmaps)，[基数统计（hyperloglogs）](http://www.redis.cn/topics/data-types-intro.html#hyperloglogs) 和 [地理空间（geospatial）](http://www.redis.cn/commands/geoadd.html) 索引半径查询。 Redis 内置了 [复制（replication）](http://www.redis.cn/topics/replication.html)，[LUA脚本（Lua scripting）](http://www.redis.cn/commands/eval.html)， [LRU驱动事件（LRU eviction）](http://www.redis.cn/topics/lru-cache.html)，[事务（transactions）](http://www.redis.cn/topics/transactions.html) 和不同级别的 [磁盘持久化（persistence）](http://www.redis.cn/topics/persistence.html)， 并通过 [Redis哨兵（Sentinel）](http://www.redis.cn/topics/sentinel.html)和自动 [分区（Cluster）](http://www.redis.cn/topics/cluster-tutorial.html)提供高可用性（**High Availability**）。

### 2.1 Redis 自动配置

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

![Redis 的 jar 包](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606745732785-17d1227a-75b9-4f00-a3f1-7fc4137b5113.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_17%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

自动配置：

- `RedisAutoConfiguration` 自动配置类，`RedisProperties` 属性类；
- 连接工厂是准备好的，`LettuceConnectionConfiguration`、`JedisConnectionConfiguration`；
- 自动注入了 `RedisTemplate<Object, Object>`；
- 自动注入了 `StringRedisTemplate`，键值都是 `String`；
- 我们只要使用 `StringRedisTemplate`、`RedisTemplate` 就可以操作 **Redis**。

### 2.2 RedisTemplate 使用

```Java
@Test
void testRedis(){
    ValueOperations<String, String> operations = redisTemplate.opsForValue();
    operations.set("hello","world");
    String hello = operations.get("hello");
    System.out.println(hello);
}
```

### 2.3 切换至 Jedis

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
 
<!-- 导入 Jedis-->
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
</dependency>
 
redis:
  host: 172.20.134.231
  port: 6379
  client-type: jedis
  jedis:
    pool:
      max-active: 10
```

# 八、单元测试

## 1、JUnit5 的变化

SpringBoot 2.2.0 版本开始引入 **JUnit5** 作为单元测试默认库。

作为最新版本的 **JUnit** 框架，**JUnit5** 与之前版本的 **Junit** 框架有很大的不同。由三个不同子项目的几个不同模块组成。

**JUnit5 = JUnit Platform + JUnit Jupiter + JUnit Vintage**

- **JUnit Platform**：在 **JVM** 上启动测试框架的基础，不仅支持 **Junit** 自制的测试引擎，其他测试引擎也都可以接入。
- **JUnit Jupiter**：提供了 **JUnit5** 的新的编程模型，是 **JUnit5** 新特性的核心。内部包含了一个**测试引擎**，用于在 **Junit Platform** 上运行。
- **JUnit Vintage**：由于 **JUint** 已经发展多年，为了照顾老的项目，**JUnit Vintage** 提供了兼容 **JUnit4.x**、**Junit3.x** 的测试引擎。

![JUnit5](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1606796395719-eb57ab48-ae44-45e5-8d2e-c4d507aff49a.png)

JUnit5：

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-test</artifactId>
  <scope>test</scope>
</dependency>
```

![JUnit5 的 jar 包](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1606797616337-e73010e9-9cac-496d-a177-64b677af5a3d.png)

```java
@SpringBootTest
class Boot05WebAdminApplicationTests {
    @Test // 注意这里可以没有 public
    void contextLoads() {
 
    }
}
```

JUnit4：

SpringBoot 2.4 以上版本移除了默认对 **Vintage** 的依赖，如果需要兼容 **JUnit4** 需要自行引入。

```xml
<dependency>
    <groupId>org.junit.vintage</groupId>
    <artifactId>junit-vintage-engine</artifactId>
    <scope>test</scope>
    <exclusions>
        <exclusion>
            <groupId>org.hamcrest</groupId>
            <artifactId>hamcrest-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>
@RunWith(SpringRunner.class)
@SpringBootTest // 如果启动报错，则需要指定启动类的 class
class Boot05WebAdminApplicationTests {
    @Test
    public void contextLoads() {
 
    }
}
```

SpringBoot 整合 **JUnit** 以后。

- 编写测试方法：`@Test` 标注（注意需要使用 **JUnit5** 版本的注解）；
- **JUnit** 类具有 Spring 的功能，`@Autowired`、比如 `@Transactional` 标注测试方法，测试完成后自动回滚。

## 2、JUnit5 常用注解

JUnit5的注解与JUnit4的注解有所变化：

https://junit.org/junit5/docs/current/user-guide/#writing-tests-annotations

- `@Test`：表示方法是测试方法。但是与 **JUnit4** 的 `@Test` 不同，他的职责非常单一不能声明任何属性，拓展的测试将会由 **Jupiter** 提供额外测试；
- `@ParameterizedTest`：表示方法是参数化测试，下方会有详细介绍；
- `@RepeatedTest`：表示方法可重复执行，下方会有详细介绍；
- `@DisplayName`：为测试类或者测试方法设置展示名称；
- `@BeforeEach`：表示在每个单元测试之前执行；
- `@AfterEach`：表示在每个单元测试之后执行；
- `@BeforeAll`：表示在所有单元测试之前执行；
- `@AfterAll`：表示在所有单元测试之后执行；
- `@Tag`：表示单元测试类别，类似于 **JUnit4** 中的 `@Categories`；
- `@Disabled`：表示测试类或测试方法不执行，类似于 **JUnit4** 中的 `@Ignore`；
- `@Timeout`：表示测试方法运行如果超过了指定时间将会返回错误；
- `@ExtendWith`：为测试类或测试方法提供扩展类引用。

```java
import org.junit.jupiter.api.Test; // 注意这里使用的是 jupiter 的 Test 注解
 
@SpringBootTest
public class TestDemo {
  @Test
  @DisplayName("第一次测试")
  public void firstTest() {
      System.out.println("hello world");
  }
```

## 3、断言（Assertions）

**断言**（Assertions）是测试方法中的核心部分，用来对测试需要满足的条件进行验证。这些断言方法都是 org.junit.jupiter.api.`Assertions` 的静态方法。**JUnit5** 内置的断言可以分成如下几个类别：

### 3.1 简单断言

用来对单个值进行简单的验证，如：

| 方法              | 说明                                 |
| ----------------- | ------------------------------------ |
| `assertEquals`    | 判断两个对象或两个原始类型是否相等   |
| `assertNotEquals` | 判断两个对象或两个原始类型是否不相等 |
| `assertSame`      | 判断两个对象引用是否指向同一个对象   |
| `assertNotSame`   | 判断两个对象引用是否指向不同的对象   |
| `assertTrue`      | 判断给定的布尔值是否为 `true`        |
| `assertFalse`     | 判断给定的布尔值是否为 `false`       |
| `assertNull`      | 判断给定的对象引用是否为 `null`      |
| `assertNotNull`   | 判断给定的对象引用是否不为 `null`    |

```javascript
@Test
@DisplayName("simple assertion")
public void simple() {
     assertEquals(3, 1 + 2, "simple math");
     assertNotEquals(3, 1 + 1);
 
     assertNotSame(new Object(), new Object());
     Object obj = new Object();
     assertSame(obj, obj);
 
     assertFalse(1 > 2);
     assertTrue(1 < 2);
 
     assertNull(null);
     assertNotNull(new Object());
}
```

### 3.2 数组断言

通过 `assertArrayEquals` 方法来判断两个对象或原始类型的数组内容是否相等：

```Java
@Test
@DisplayName("array assertion")
public void array() {
 assertArrayEquals(new int[]{1, 2}, new int[] {1, 2});
}
```

### 3.3 组合断言

`assertAll` 方法接受多个 org.junit.jupiter.api.`Executable` 函数式接口的实例作为要验证的断言，可以通过 `lambda` 表达式很容易的提供这些断言：

```Java
@Test
@DisplayName("assert all")
public void all() {
 assertAll("Math",
    () -> assertEquals(2, 1 + 1),
    () -> assertTrue(1 > 0)
 );
}
```

### 3.4 异常断言

在 **JUnit4** 时期，想要测试方法的异常情况时，需要用 `@Rule` 注解的 `ExpectedException` 变量还是比较麻烦的。而 **JUnit5** 提供了一种新的断言方式 `Assertions.assertThrows()`，配合函数式编程就可以进行使用：

```Java
@Test
@DisplayName("异常测试")
public void exceptionTest() { // 扔出断言异常
    ArithmeticException exception = Assertions.assertThrows(
        ArithmeticException.class, () -> System.out.println(1 % 0));
}
```

### 3.5 超时断言

**Junit5** 还提供了 `Assertions.assertTimeout()` 为测试方法设置了超时时间：

```Java
@Test
@DisplayName("超时测试")
public void timeoutTest() {
    // 如果测试方法时间超过 1s 将会异常
    Assertions.assertTimeout(Duration.ofMillis(1000), () -> Thread.sleep(500));
}
```

### 3.6 快速失败

通过 `fail` 方法直接使得测试失败：

```Java
@Test
@DisplayName("fail")
public void shouldFail() {
     fail("This should fail");
}
```

## 4、前置条件（Assumptions）

JUnit 5 中的前置条件（**Assumptions【假设】**）类似于断言，不同之处在于**不满足的断言会使得测试方法失败**，而不满足的**前置条件只会使得测试方法的执行终止**。前置条件可以看成是测试方法执行的前提，当该前提不满足时，就没有继续执行的必要。

```Java
@DisplayName("前置条件")
public class AssumptionsTest {
 private final String environment = "DEV";
 
 @Test
 @DisplayName("simple")
 public void simpleAssume() {
    assumeTrue(Objects.equals(this.environment, "DEV"));
    assumeFalse(() -> Objects.equals(this.environment, "PROD"));
 }
 
 @Test
 @DisplayName("assume then do")
 public void assumeThenDo() {
    assumingThat(
       Objects.equals(this.environment, "DEV"),
       () -> System.out.println("In DEV")
    );
 }
}
```

`assumeTrue` 和 `assumFalse` 确保给定的条件为 `true` 或 `false`，不满足条件会使得测试执行终止。`assumingThat` 的参数是表示条件的布尔值和对应的 `Executable` 接口的实现对象。只有条件满足时，`Executable` 对象才会被执行；当条件不满足时，测试执行并不会终止。

## 5、嵌套测试

**JUnit5** 可以通过 Java 中的内部类和 `@Nested` 注解实现嵌套测试，从而可以更好的把相关的测试方法组织在一起。在内部类中可以使用 `@BeforeEach` 和 `@AfterEach` 注解，而且嵌套的层次没有限制。

内层的 test 可以驱动外层的 Before(After)Each/All 之类的方法提前/延后运行，外层的不能驱动内层的。

```Java
@DisplayName("A stack")
class TestingAStackDemo {
    Stack<Object> stack;
 
    @Test
    @DisplayName("is instantiated with new Stack()")
    void isInstantiatedWithNew() {
        new Stack<>();
    }
 
    @Nested
    @DisplayName("when new")
    class WhenNew {
        @BeforeEach
        void createNewStack() {
            stack = new Stack<>();
        }
 
        @Test
        @DisplayName("is empty")
        void isEmpty() {
            assertTrue(stack.isEmpty());
        }
 
        @Test
        @DisplayName("throws EmptyStackException when popped")
        void throwsExceptionWhenPopped() {
            assertThrows(EmptyStackException.class, stack::pop);
        }
 
        @Test
        @DisplayName("throws EmptyStackException when peeked")
        void throwsExceptionWhenPeeked() {
            assertThrows(EmptyStackException.class, stack::peek);
        }
 
        @Nested
        @DisplayName("after pushing an element")
        class AfterPushing {
            String anElement = "an element";
 
            @BeforeEach
            void pushAnElement() {
                stack.push(anElement);
            }
 
            @Test
            @DisplayName("it is no longer empty")
            void isNotEmpty() {
                assertFalse(stack.isEmpty());
            }
 
            @Test
            @DisplayName("returns the element when popped and is empty")
            void returnElementWhenPopped() {
                assertEquals(anElement, stack.pop());
                assertTrue(stack.isEmpty());
            }
 
            @Test
            @DisplayName("returns the element when peeked but remains not empty")
            void returnElementWhenPeeked() {
                assertEquals(anElement, stack.peek());
                assertFalse(stack.isEmpty());
            }
        }
    }
}
```

## 6、参数化测试

参数化测试是 **JUnit5** 很重要的一个新特性，它使得用不同的参数多次运行测试成为了可能，也为我们的单元测试带来许多便利。

利用 `@ValueSource` 等注解，指定入参，我们将可以使用不同的参数进行多次单元测试，而不需要每新增一个参数就新增一个单元测试，省去了很多冗余代码。

- `@ValueSource`：为参数化测试指定入参来源，支持八大基础类以及 `String` 类型，`Class`类型；
- `@NullSource`：表示为参数化测试提供一个 `null` 的入参；
- `@EnumSource`：表示为参数化测试提供一个枚举入参；
- `@CsvFileSource`：表示读取指定 **CSV** 文件内容作为参数化测试入参；
- `@MethodSource`：表示读取指定静态方法的返回值作为参数化测试入参，注意方法返回需要是一个 `Stream`。

当然如果参数化测试仅仅只能做到指定普通的入参，还达不到让我觉得惊艳的地步。让我真正感到他的强大之处的地方在于他可以支持外部的各类入参。如 **CSV**、**YAML**、**JSON** 文件甚至方法的返回值也可以作为入参。只需要去实现 `ArgumentsProvider` 接口，任何外部文件都可以作为它的入参。

```Java
@ParameterizedTest
@ValueSource(strings = {"one", "two", "three"})
@DisplayName("参数化测试1")
public void parameterizedTest1(String string) {
    System.out.println(string);
    Assertions.assertTrue(StringUtils.isNotBlank(string));
}
 
@ParameterizedTest
@MethodSource("method") // 指定方法名
@DisplayName("方法来源参数")
public void testWithExplicitLocalMethodSource(String name) {
    System.out.println(name);
    Assertions.assertNotNull(name);
}
 
static Stream<String> method() {
    return Stream.of("apple", "banana");
}
```

## 7、迁移指南

在进行迁移的时候需要注意如下的变化：

- 注解在 **org.junit.jupiter.api** 包中，断言在 org.junit.jupiter.api.`Assertions` 类中，前置条件在 org.junit.jupiter.api.``Assumptions` 类中；
- 把 `@Before` 和 `@After` 替换成 `@BeforeEach` 和 `@AfterEach`；
- 把 `@BeforeClass` 和 `@AfterClass` 替换成 `@BeforeAll` 和 `@AfterAll`；
- 把 `@Ignore` 替换成 `@Disabled`；
- 把 `@Category` 替换成 `@Tag`；
- 把 `@RunWith`、`@Rule` 和 `@ClassRule` 替换成 `@ExtendWith`。

# 九、指标监控

## 1、SpringBoot Actuator

### 1.1 简介

未来每一个微服务在云上部署以后，我们都需要对其进行**监控**、**追踪**、**审计**、**控制**等。SpringBoot 就抽取了 **Actuator** 场景，使得我们每个微服务快速引用即可获得生产级别的应用监控、审计等功能。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

![Actuator 的 jar 包](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1606886483335-697ee1c1-2f69-43ab-bddc-3a038382319c.png)

### 1.2 1.x 与 2.x 的不同

![1.x 与 2.x 的不同](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1606884394162-ac7f2d8e-7abb-44df-9998-fb0f2705f238.png)

### 1.3 如何使用

- 引入场景 **starter**；
- 访问 http://localhost:8080/actuator/**；
- 暴露所有监控信息为 **HTTP**；

```yaml
management:
  endpoints:
    enabled-by-default: true # 暴露所有端点信息
    web:
      exposure:
        include: '*' # 以 Web 方式暴露
```

- 测试。

http://localhost:8080/actuator/beans

http://localhost:8080/actuator/configprops

http://localhost:8080/actuator/metrics

http://localhost:8080/actuator/metrics/jvm.gc.pause

[http://localhost:8080/actuator/](http://localhost:8080/actuator/metrics)endpointName/detailPath

### 1.4 可视化

https://github.com/codecentric/spring-boot-admin

## 2、Actuator Endpoint

### 2.1 最常使用的 Endpoint

| ID                 | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| `auditevents`      | 暴露当前应用程序的审核事件信息。需要一个 `AuditEventRepository` 组件。 |
| `beans`            | 显示应用程序中所有 `Bean` 的完整列表。                       |
| `caches`           | 暴露可用的**缓**存。                                         |
| `conditions`       | 显示**自动配置**的所有条件信息，包括匹配或不匹配的原因。     |
| `configprops`      | 显示所有 `@ConfigurationProperties`。                        |
| `env`              | 暴露 Spring 的属性 `ConfigurableEnvironment`。               |
| `flyway`           | 显示已应用的所有 **Flyway** 数据库迁移， 需要一个或多个 `Flyway` 组件。 |
| `health`           | 显示应用程序**运行状况**信息。                               |
| `httptrace`        | 显示 **HTTP** 跟踪信息（默认情况下，最近 100 个 **HTTP** 请求-响应）。需要一个 `HttpTraceRepository` 组件。 |
| `info`             | 显示应用程序信息。                                           |
| `integrationgraph` | 显示Spring `integrationgraph` 。需要依赖 `spring-integration-core`。 |
| `loggers`          | 显示和修改应用程序中**日志**的配置。                         |
| `liquibase`        | 显示已应用的所有 **Liquibase** 数据库迁移。需要一个或多个`Liquibase`组件。 |
| `metrics`          | 显示当前应用程序的**指标**信息。                             |
| `mappings`         | 显示所有 `@RequestMapping` 路径列表。                        |
| `scheduledtasks`   | 显示应用程序中的**计划任务**。                               |
| `sessions`         | 允许从 Spring Session 支持的会话存储中检索和删除用户会话。需要使用 Spring Session 的基于 `Servlet` 的 Web 应用程序。 |
| `shutdown`         | 使应用程序正常关闭，默认禁用。                               |
| `startup`          | 显示由 `ApplicationStartup` 收集的启动步骤数据。需要使用 `SpringApplication` 进行配置 `BufferingApplicationStartup`。 |
| `threaddump`       | 执行**线程转储**。                                           |

如果您的应用程序是 **Web** 应用程序（Spring MVC，Spring WebFlux 或 Jersey），则可以使用以下附加端点：

| ID           | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| `heapdump`   | 返回 `hprof` 堆转储文件。                                    |
| `jolokia`    | 通过 **HTTP** 暴露 JMX Bean（需要引入 Jolokia，不适用于 WebFlux）。需要引入依赖 `jolokia-core`。 |
| `logfile`    | 返回日志文件的内容（如果已设置 `logging.file.name` 或 `logging.file.path` 属性）。支持使用 **HTTP** `Range` 标头来检索部分日志文件的内容。 |
| `prometheus` | 以 Prometheus 服务器可以抓取的格式公开指标。需要依赖 `micrometer-registry-prometheus`。 |

最常用的 **Endpoint**：

- `Health`：监控状况
- `Metrics`：运行时指标
- `Loggers`：日志记录

### 2.2 Health Endpoint

健康检查端点，我们一般用于在云平台，平台会定时的检查应用的健康状况，我们就需要 **Health Endpoint** 可以为平台返回当前应用的一系列组件健康状况的集合。

重要的几点：

- **Health Endpoint** 返回的结果，应该是一系列健康检查后的一个汇总报告；
- 很多的健康检查默认已经自动配置好了，比如数据库、**Redis** 等；
- 可以很容易的添加自定义的健康检查机制。

![健康端点](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1606908975702-4f9a3208-15ca-4a78-9f76-939ef986db7e.png)

### 2.3 Metrics Endpoint

提供详细的、层级的、空间指标信息，这些信息可以被 `pull`（主动推送）或者 `push`（被动获取）方式得到；

- 通过 **Metrics** 对接多种监控系统；
- 简化核心 **Metrics** 开发；
- 添加自定义 **Metrics** 或者扩展已有 **Metrics**。

![指标端点](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606909073222-c6e77ca3-4b1c-4f38-a1c6-8614dec4f7bc.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_16%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

### 2.4 管理 Endpoints

#### 2.4.1 开启与禁用 Endpoints

- 默认所有的 **Endpoint** 除了 `shutdown` 都是开启的；
- 需要开启或者禁用某个 **Endpoint**。配置模式为 `management.endpoint.<endpointName>.enabled = true`；

```yaml
management:
  endpoint:
    beans:
      enabled: true
```

- 或者禁用所有的 **Endpoint** 然后手动开启指定的 **Endpoint**。

```yaml
management:
  endpoints:
    enabled-by-default: false
  endpoint:
    beans:
      enabled: true
    health:
      enabled: true
```

#### 2.4.2 暴露 Endpoints

支持的暴露方式

- **HTTP**：默认只暴露 **Health** 和**Info Endpoint**；
- **JMX**：默认暴露所有 **Endpoint**，**JConsole** 就是通过 **JMX** 实现的；
- 除了 **Health** 和 **Info**，剩下的 **Endpoint** 都应该进行保护访问。如果引入 `SpringSecurity`，则会默认配置安全访问规则。

| ID                 | JMX  | Web  |
| ------------------ | ---- | ---- |
| `auditevents`      | Yes  | No   |
| `beans`            | Yes  | No   |
| `caches`           | Yes  | No   |
| `conditions`       | Yes  | No   |
| `configprops`      | Yes  | No   |
| `env`              | Yes  | No   |
| `flyway`           | Yes  | No   |
| `health`           | Yes  | Yes  |
| `heapdump`         | N/A  | No   |
| `httptrace`        | Yes  | No   |
| `info`             | Yes  | Yes  |
| `integrationgraph` | Yes  | No   |
| `jolokia`          | N/A  | No   |
| `logfile`          | N/A  | No   |
| `loggers`          | Yes  | No   |
| `liquibase`        | Yes  | No   |
| `metrics`          | Yes  | No   |
| `mappings`         | Yes  | No   |
| `prometheus`       | N/A  | No   |
| `scheduledtasks`   | Yes  | No   |
| `sessions`         | Yes  | No   |
| `shutdown`         | Yes  | No   |
| `startup`          | Yes  | No   |
| `threaddump`       | Yes  | No   |

## 3、定制 Endpoint

### 3.1 定制 Health 信息

```Java
@Component
public class MyHealthIndicator implements HealthIndicator {
    @Override
    public Health health() {
        int errorCode = check(); // perform some specific health check
        if (errorCode != 0) {
            return Health.down().withDetail("Error Code", errorCode).build();
        }
        return Health.up().build();
    }
 
}
 
// 构建 Health
Health build = Health.down()
                .withDetail("msg", "error service")
                .withDetail("code", "500")
                .withException(new RuntimeException())
                .build();
management:
    health:
      enabled: true
      show-details: always # 总是显示详细信息，可显示每个模块的状态信息
@Component
public class MyComHealthIndicator extends AbstractHealthIndicator {
    // 真实的检查方法
    @Override
    protected void doHealthCheck(Health.Builder builder) throws Exception {
        // MongoDB，获取连接进行测试
        Map<String,Object> map = new HashMap<>();
        // 检查完成
        if(1 == 2){
//            builder.up(); // 健康
            builder.status(Status.UP);
            map.put("count",1);
            map.put("ms",100);
        }else {
//            builder.down();
            builder.status(Status.OUT_OF_SERVICE);
            map.put("err","连接超时");
            map.put("ms",3000);
        }
 
        builder.withDetail("code",100).withDetails(map);
    }
}
```

### 3.2 定制info信息

常用两种方式：

#### 3.2.1 编写配置文件

```yaml
info:
  appName: boot-admin
  version: 2.0.1
  mavenProjectName: @project.artifactId@  #使用@@可以获取maven的pom文件值
  mavenProjectVersion: @project.version@
```

#### 3.2.2 编写 InfoContributor

```Java
@Component
public class ExampleInfoContributor implements InfoContributor {
    @Override
    public void contribute(Info.Builder builder) {
        builder.withDetail("example",
                Collections.singletonMap("key", "value"));
    }
}
```

http://localhost:8080/actuator/info：会输出以上方式返回的所有 **Info** 信息。

### 3.3 定制 Metrics 信息

#### 3.3.1 SpringBoot 支持自动适配的 Metrics

- JVM metrics, report utilization of:
- Various memory and buffer pools
- Statistics related to garbage collection
- Threads utilization
- Number of classes loaded/unloaded
- CPU metrics
- File descriptor metrics
- Kafka consumer and producer metrics
- Log4j2 metrics: record the number of events logged to Log4j2 at each level
- Logback metrics: record the number of events logged to Logback at each level
- Uptime metrics: report a gauge for uptime and a fixed gauge representing the application’s absolute start time
- Tomcat metrics (`server.tomcat.mbeanregistry.enabled` must be set to `true` for all Tomcat metrics to be registered)
- [Spring Integration](https://docs.spring.io/spring-integration/docs/5.4.1/reference/html/system-management.html#micrometer-integration) metrics

#### 3.2.2 增加定制 Metrics

```Java
class MyService{
    Counter counter;
    public MyService(MeterRegistry meterRegistry){
         counter = meterRegistry.counter("myservice.method.running.counter");
    }
 
    public void hello() {
        counter.increment();
    }
}
 
// 也可以使用下面的方式
@Bean
MeterBinder queueSize(Queue queue) {
    return (registry) -> Gauge.builder("queueSize", queue::size).register(registry);
}
```

### 3.4定制 Endpoint

```Java
@Component
@Endpoint(id = "container")
public class DockerEndpoint {
    @ReadOperation
    public Map getDockerInfo(){
        return Collections.singletonMap("info","docker started...");
    }
 
    @WriteOperation
    private void restartDocker(){
        System.out.println("docker restarted....");
    }
}
```

场景：开发 **ReadinessEndpoint** 来管理程序是否就绪，或者 **Liveness Endpoint **来管理程序是否存活。

当然，这个也可以直接使用 https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html#production-ready-kubernetes-probes

# 十、原理解析

## 10.1 Profile 功能

为了方便多环境适配，SpringBoot 简化了 **Profile** 功能。

### 10.1.1 application-profile 功能

- 默认配置文件 **application.yaml** 任何时候都会加载；
- 环境配置文件 **application-{env}.yaml**；
- 激活指定环境；
  - 配置文件激活；
  - 命令行激活：``java -jar xxx.jar --spring.profiles.active=prod --person.name=haha`。
    - 优先级：命令行 > 环境配置文件 > 默认配置文件
- 默认配置与环境配置同时生效；
- 同名配置项，**Profile** 配置优先；

### 10.1.2 `@Profile` 条件装配功能

```Java
@Configuration(proxyBeanMethods = false)
@Profile("production")
public class ProductionConfiguration {
    // ...
}
```

### 10.1.3 Profile 分组

```properties
spring.profiles.group.production[0]=proddb
spring.profiles.group.production[1]=prodmq
 
# 使用：--spring.profiles.active=production 激活
```

## 10.2 外部化配置

https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config

Spring Boot uses a very particular `PropertySource` order that is designed to allow sensible overriding of values. Properties are considered in the following order (with values from lower items overriding earlier ones):

1. Default properties (specified by setting `SpringApplication.setDefaultProperties`).
2. `@PropertySource` annotations on your `@Configuration` classes. Please note that such property sources are not added to the `Environment` until the application context is being refreshed. This is too late to configure certain properties such as `logging.*` and `spring.main.*` which are read before refresh begins.
3. **Config data (such as** `application.properties` **files)**
4. A `RandomValuePropertySource` that has properties only in `random.*`.
5. OS environment variables.
6. Java System properties (`System.getProperties()`).
7. JNDI attributes from `java:comp/env`.
8. `ServletContext` init parameters.
9. `ServletConfig` init parameters.
10. Properties from `SPRING_APPLICATION_JSON` (inline JSON embedded in an environment variable or system property).
11. Command line arguments.
12. `properties` attribute on your tests. Available on `@SpringBootTest` and the [test annotations for testing a particular slice of your application](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-testing-spring-boot-applications-testing-autoconfigured-tests).
13. `@TestPropertySource` annotations on your tests.
14. [Devtools global settings properties](https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-devtools-globalsettings) in the `$HOME/.config/spring-boot` directory when devtools is active.

Config data files are considered in the following order:

1. [Application properties](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.external-config.files) packaged inside your jar (`application.properties` and YAML variants).
2. [Profile-specific application properties](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.external-config.files.profile-specific) packaged inside your jar (`application-{profile}.properties` and YAML variants).
3. [Application properties](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.external-config.files) outside of your packaged jar (`application.properties` and YAML variants).
4. [Profile-specific application properties](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.external-config.files.profile-specific) outside of your packaged jar (`application-{profile}.properties` and YAML variants).

### 10.2.1 外部配置源

常用：**Java** 属性文件、环境变量、命令行参数、**YAML **文件。

### 10.2.2 配置文件查找位置

越往下优先级越高，会覆盖上面的配置：

1. **classpath** 根路径；
2. **classpath** 根路径下 **config** 目录；
3. **jar** 包当前目录；
4. **jar** 包当前目录的 **config** 目录；
5. **/config** 子目录的直接子目录。

### 10.2.3 配置文件加载顺序：

1. 当前jar包内部的application.properties和application.yml
2. 当前jar包内部的application-{profile}.properties 和 application-{profile}.yml
3. 引用的外部jar包的application.properties和application.yml
4. 引用的外部jar包的application-{profile}.properties 和 application-{profile}.yml

### 10.2.4 指定环境优先，外部优先，后面的可以覆盖前面的同名配置项

## 10.3 自定义 starter

### 10.3.1 starter 启动原理

1. **starter-pom** 引入 **autoconfigurer** 包；

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606995919308-b2c7ccaa-e720-4cc5-9801-2e170b3102e1.png)

1. **autoconfigure** 包中配置使用 **META-INF/spring.factories** 中 `EnableAutoConfiguration` 的值，使得项目启动时加载指定的自动配置类；
2. 编写自动配置类 `xxxAutoConfiguration` -> `xxxxProperties`；
3. 编写业务：``@Configuration`、`@Conditional`、`@EnableConfigurationProperties`、`@Bean`......

引入 **starter** --- `xxxAutoConfiguration` --- `容器中放入组件` ---- 绑定 `xxxProperties` ---- 配置项

### 10.3.2 自定义 starter

atguigu-hello-spring-boot-starter（启动器）

atguigu-hello-spring-boot-starter-autoconfigure（自动配置包）

## 10.4 SpringBoot 原理

Spring原理【[Spring注解](https://www.bilibili.com/video/BV1gW411W7wy?p=1)】、**SpringMVC** 原理、自动配置原理

### 10.4.1 SpringBoot 启动过程

1. 创建

    

   ```
   SpringApplication
   ```

   ；

   1. 保存一些信息；

   2. 用工具类 `ClassUtils` 获取到应用类型为`Servlet`、还可能为 Reactive 或 None；

   3. 去

       

      spring.factories

       

      找

       

      ```
      Bootstrapper
      ```

      ；

      - `List<Bootstrapper> bootstrappers`

   4. 去

       

      spring.factories

       

      找

       

      ```
      ApplicationContextInitializer
      ```

      ；

      - `List<ApplicationContextInitializer<?>> initializers`

   5. 去

       

      spring.factories

       

      找

       

      ```
      ApplicationListener
      ```

      - `List<ApplicationListener<?>> listeners`

2. 运行

    

   ```
   SpringApplication
   ```

   。

   1. `StopWatch` 用于监听整个应用的启动停止；

   2. 记录应用的启动时间；

   3. 调用

       

      ```
      createBootstrapContext()
      ```

       

      创建引导上下文（

      ```
      Context
      ```

       

      环境）；

      - 获取到所有之前的 `bootstrappers` 挨个执行 `intitialize()` 来设置引导启动器上下文环境

   4. 让当前应用进入 java.awt.`headless` 模式（自力更生模式）；

   5. 获取所有

       

      ```
      RunListener
      ```

      （运行监听器），为了方便所有

       

      ```
      Listener
      ```

       

      进行事件感知；

      - `getSpringFactoriesInstances()` 去 **spring.factories** 找`SpringApplicationRunListener`

   6. 遍历

       

      ```
      SpringApplicationRunListener
      ```

       

      调用

       

      ```
      starting()
      ```

       

      方法；

      - 相当于通知所有感兴趣系统正在启动过程的人，项目正在 starting

   7. 保存命令行参数 `ApplicationArguments`；

   8. 准备环境

       

      ```
      prepareEnvironment()
      ```

      ；

      1. 返回或者创建基础环境信息对象 `StandardServletEnvironment`；
      2. 配置环境信息对象；
         - 读取所有的配置源的配置属性值
      3. 绑定环境信息；
      4. 监听器调用 `listener.environmentPrepared() 通知所有的监听器当前环境准备完成。

   9. 创建

       

      IOC

       

      容器

       

      ```
      createApplicationContext
      ```

      ；

      1. 根据项目类型（`Servlet`）创建容器；
      2. 当前会创建 `AnnotationConfigServletWebServerApplicationContext`。

   10. 准备

       ```
       ApplicationContext
       ```

       容器的基本信息

        

       ```
       prepareContext()
       ```

       1. 保存环境信息；

       2. IOC容器的后置处理流程；

       3. 应用初始化器

           

          ```
          applyInitializers()
          ```

          ；

          1. 遍历所有的 `ApplicationContextInitializer`，调用 `initialize()`来对容器进行初始化扩展功能;
          2. 遍历所有的 `listener` 调用 `contextPrepared()`。通知所有的监听器 `EventPublishRunListenr` 上下文准备完成。

       4. 所有的监听器调用 `contextLoaded()`，通知所有的监听器上下文加载完成；

   11. 刷新容器

        

       ```
       refreshContext()
       ```

       ；

       1. 创建容器中的所有组件（Spring 注解）

   12. 容器刷新完成后工作 `afterRefresh()`;

   13. 所有监听器调用 `listeners.started(context)` 通知所有的监听器启动完成；

   14. 调用所有

        

       ```
       callRunners()
       ```

       ；

       1. 获取容器中的 `ApplicationRunner`；
       2. 获取容器中的 `CommandLineRunner` （执行数据初始化等操作）；
       3. 合并所有 **runner** 并且按照 `@Order` 进行排序；
       4. 遍历所有的 **runner** 调用 `run()` 方法。

   15. 如果以上有异常；

       - 调用 `Listener` 的 `failed()` 方法通知所有的监听器 **failed**

   16. 调用所有监听器的 `listeners.running(context)`通知所有的监听器正在运行；

   17. **running** 如果有问题，继续调调用 `Listener` 的 `failed()` 方法通知所有的监听器 **failed**。

> Spring 的核心方法实际调用的是抽象类 `ApplicationContext.refresh()` 方法。该方法是一种模版方法的设计模式，定义了方法的执行顺序和骨架，其中 `onRefresh()` 是一个抽象方法，由具体子类实现。
>
> 因为启动的是 **Web** 版的容器因此执行的是 `ServletWebServerApplicationContext.refresh()` 方法。该方法从容器中获得一个 `ServletWebServerFactory` 工厂（如果有多个工厂，则抛异常），根据工厂对象获得 `WebServer` 对象。
>
> 因为内嵌 **Tomcat**，所以是 `TomcatWevServer` 后续通过 `new Tomcat()` 并执行 `start()` 启动 **Tomcat** 服务器。

```Java
public interface Bootstrapper {
	/**
	 * Initialize the given {@link BootstrapRegistry} with any required registrations.
	 * @param registry the registry to initialize
	 */
	void intitialize(BootstrapRegistry registry);
}
```

![容器初始化器](https://cdn.nlark.com/yuque/0/2020/png/1354552/1607005958877-bf152e3e-4d2d-42b6-a08c-ceef9870f3b6.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_18%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

![img](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1607004823889-8373cea4-6305-40c1-af3b-921b071a28a8.png)

![监听器](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/1607006112013-6ed5c0a0-3e02-4bf1-bdb7-423e0a0b3f3c.png)

```Java
@FunctionalInterface
public interface ApplicationRunner {
	/**
	 * Callback used to run the bean.
	 * @param args incoming application arguments
	 * @throws Exception on error
	 */
	void run(ApplicationArguments args) throws Exception;
}
 
@FunctionalInterface
public interface CommandLineRunner {
	/**
	 * Callback used to run the bean.
	 * @param args incoming main method arguments
	 * @throws Exception on error
	 */
	void run(String... args) throws Exception;
}
```

### 10.4.2 Application Events and Listeners

https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-application-events-and-listeners

```
ApplicationContextInitializer
ApplicationListener
SpringApplicationRunListener
```

### 10.4.3 `ApplicationRunner` 与 `CommandLineRunner`