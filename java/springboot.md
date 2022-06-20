## 一、基础入门

### 1.hello，Springboot

**主程序**

```java
@SpringBootApplication
public class SpringbootDemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootDemoApplication.class, args);
    }

}
```

@SpringBootApplication//用来标注类是一个springboot的应用

**编写业务**

```java
@RestController
public class HelloControl {
    @RequestMapping("/hello")
    public String hello(){
        return "HelloSpring";
    }
}
```

### 2.配置

#### 2.1依赖管理

```xml
父项目，依赖管理
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.5.7</version>
</parent>
父项目的父项目，声明并导入了大部分依赖
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.5.7</version>
  </parent>
```



若依赖版本不符合想使用的默认依赖版本，则可以在**项目**中加入以下格式的代码修改版本。

以下是修改mysql依赖版本

```xml
<properties>
    <mysql.version>1.5.43</mysql.version>
</properties>
```

这段修改的是mysql驱动的版本，同理可修改其它的版本。

#### 属性配置

属性在application.properties文件中配置

- 每个属性都有默认配置
  - 每个默认配置最终都映射到MultipartProperties
  - 配置文件的值最终会绑定到每个类上，这个类会在容器中创建对象
- 按需加载所有自动配置项
  - 有非常多的starter
  - 引入了哪些场景，这个场景的自动配置才会开启
  - SpringBoot所有的自动配置功能都在spring-boot-autoconfigure包里面

### 3.容器功能

#### 3.1组件添加

1、@Configuration

- 取代Spring中的xml配置文件，可以把组件（bean）配置在其中，@Configuration本身也可以看做是一个bean
- Full模式与Lite模式
  - 参数proxyBeanMethods，等于true时，创建的所有bean对象都是同一个对象（单实例对象），指针是相同的。即为Full模式，速度比Lite模式慢。

2、@Bean //方法注解

- 在容器中中注册bean。

3、@Component、@Controller、@Service、@Repository

- 都是将类作为组件（bean）加入容器，4个注解作用相同。

4、@ComponentScan、@Import

- 类注解，向容器中导入组件

5、@conditional

- 条件装配：满足条件后，则进行组件注入
- 包含一类注解@conditionalOnBean、@conditionalOnMissingBean ......

6、@ImportResource("classpath:**.xml")

- 导入将xml文件中的bean注入到容器中

#### 3.2属性配置绑定

从properties文件中读取属性，绑定到javabean上。

@ConfigurationProperties(perfix="   "),perfix为前缀。

- 该注解标注在需要绑定属性的类上。
- EnableConfigurationProperties+@ConfigurationProperties
  - @EnableConfigurationProperties标注在Configuration类上，属性为class类对象，会自动将类注入容器
- @Component+@ConfigurationProperties

### 4.自动配置原理

#### @SpringBootApplication简析

##### 1、@SpringBootApplication

```java
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
```

##### 2、@ComponentScan

##### 3、@EnableAutoConfiguration

```java
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {}
```

- 1、@AutoConfigurationPackage

  ```java
  @Import({Registrar.class})//给容器导入组件
  public @interface AutoConfigurationPackage {
      
       //利用Register给容器导入一系列组件
       //将Main程序所在的包的所有组件导入到容器
  }
  ```

- 2.@import，AutoConfigurationImportSelector类

  ![image-20211119151750009](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/image-20211119151750009.png)

  import的主要作用就是导入需要的环境配置类

  自动加载的配置类是在写死的，具体位置为：

  ![image-20211119152819883](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/image-20211119152819883.png)

  配置类不一定全部加载，**按需自动加载**

##### 4、按需装配

自动装配的配置类，都是条件装配，当没有对应的依赖(类),就无法自动装配。实现源码在spring-boot-autoconfigure-2.5.7.jar中



##### 5.修改默认配置

虽然springboot能自动装配大部分组件，但我们仍然可以自己添加组件，这样springboot就不会重复装配该组件。

![image-20211119161433709](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/image-20211119161433709.png)

##### 6.总结

- SpringBoot先加载所有的自动配置类，xxxxAutoConfiguration.Class
- 每个自动配置类按照条件进行生效，默认都会绑定配置文件指定的值。xxxxxProperties.Class
- 可以通过properties文件，开启debug模式来看配置类是否生效。Negative（不生效）\Positive（生效）
- 生效的配置类就会给容器中装配很多组件
- 只要容器中有这些组件，相当于这些功能就有了
- 只要用户有自己配置的，就以用户的优先
- 定制化配置
  - 用户直接自己用@Bean替换自动装配的组件
  - 直接在properties文件中修改

xxxxAutoConfiguration.Class---->组件--->xxxxxProperties.Class----->xxxx.properties

### 5.开发工具

devtools，代码修改后，使用其可以重新加载代码，而不是直接重启，速度更快

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```

## 二、核心功能

### 1.yaml使用

yaml可以在Spring中代替properties

#### 1.1语法



#### 1.2自定义类，配置提示

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

引入依赖，可以为自己定义的类，在yaml文件中的代码提示

### 2.Web开发

#### 2.1简单功能

##### 2.1.1静态资源访问

- **静态资源目录**

  类路径下：called /static or /public or /resources or /META-INF/resources

  原理：静态映射为/**

  请求进来，先去找Controller，若无法处理该请求，则去找静态资源，若也没有，则404

- **静态资源访问前缀**

  默认无前缀

  前缀可通过修改属性增加

  ```yaml
  spring:
    mvc:
      static-path-pattern: /res/**
  ```

  以上代码将前缀修改为/res

- **webjars**

  将一些常用静态资源通过jar包导入项目

##### 2.1.2欢迎页支持

- 静态资源路径下index.html

  当静态资源路径设置后，则无法使用欢迎页

- controller 能处理index

#### 2.2 基本参数注解

- @PathVariable(路径变量)
  - 用以restful风格传参
- @RequestHeader（获取请求头）
- @RequestParam（获取请求参数）
  - 参数类型可以为String或map，或List
- @CookieValue（获取cookie值）
  - 可以为String或Cookie类型
- @RequestBody（获取请求体）
- @RequestAttribute（获取request域属性）
  - 作用同于request.getAttribute(String name);
- @MatrixVariable（矩阵变量）
