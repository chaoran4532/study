## SpringMVC

## 一、SpringMVC配置

### 1.配置(不使用注解)

在web.xml下配置DispatcherServlet，配置如下

需要在

```xml
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://java.sun.com/xml/ns/javaee" 
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0">
    <display-name>springMVC</display-name>
    <!-- 部署 DispatcherServlet -->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
            <!--WARNING,若有多个spring文件，应写ApplicationContext.xml-->
        </init-param>
        <!-- 表示容器再启动时立即加载servlet -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <!-- 处理所有URL -->
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    <!--字符过滤器-->
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

需要为DispatcherServet配置Spring配置文件，路径为classpath:springmvc-servlet.xml



配置springmvc-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--以下两个bean默认启动
<!--    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>-->


	
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>


    <bean id="/hello" class="com.chaoran.control.HelloControl"/>
</beans>
```



Controller类

```java
public class HelloControl implements Controller {
    @Override
    public ModelAndView handleRequest(javax.servlet.http.HttpServletRequest request, javax.servlet.http.HttpServletResponse response) throws Exception {
        ModelAndView mv=new ModelAndView();
        
        //调用业务层
        
        //传入参数，给jsp等页面使用
        mv.addObject("msg","Hello,SpringMVC");
        mv.setViewName("hello");	//通过InternalResourceViewResolver将hello指向/WEB-INF/jsp/hello.jsp
        return mv;
    }
}
```

### 2.配置（使用注解）

springmvc-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.1.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context-4.1.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!--自动扫描该包，使SpringMVC认为包下用了@controller注解的类是控制器-->
    <context:component-scan base-package="com.chaoran.control"/>
    <!--    注解驱动-->
    <mvc:annotation-driven/>
    <!--    让SpringMVC不处理静态资源-->
    <mvc:default-servlet-handler/>
    <!--    视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```



control类

@Controller将类变成控制器，并自动注入Spring

@RequestMapping 将请求的地址映射到**类**或者**方法**

```java
@Controller
public class HelloControl {

    @RequestMapping("/hello")//等价于 <bean id="/hello" class="com.chaoran.control.HelloControl"/>
    public String Hello(Model model){

//        封装数据给视图
        model.addAttribute("msg","HelloSpringMVC");

        return "hello";//会被视图解析器处理，将request发送到指定位置
    }
}
```



## 二、执行流程

![SpringMVC执行流程](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/image-20211029160711505.png)

## 三、接受前端数据

传入键值对数据

```java
@RequestMapping("/hello2")
public String Hello2(@RequestParam("username")String name,@RequestParam("password") String pwd){
    System.out.println(name+" "+pwd);
    return "hello";
}
```

传入参数的key值要与@RequestParam中的字符串一致，才能传入参数。



传入对象数据

```java
@RequestMapping("/hello3")
public String Hello3(User user){
    System.out.println(user);
    return "hello";
}
```

http://localhost:8080/springmvc_annotation_war/hello3?id=1&name=ascuas&password=123123

请求带的参数，key值要与对象中的变量名一致，会自动将数据封装进类对象。



## 四、注解

* @Controller 将类作为controller接口实现类并注入spring容器，其中方法返回值会走**视图解析器InternalResourceViewResolver**。
* @RestController 同Controller，但其中方法返回值不会走视图解析器。
* @RequestMaping(String path) 将url映射到标注的方法或类，该类要返回String类型。若类的注解为@Controller则返回值会走视图解析器，否则返回到前端。
* @ResponseBody返回值不走视图解析器，直接返回到前端。返回值不会经过filter,可能产生乱码。需要在@RequestMaping()中为produces变量赋值，例如@RequestMapping(value = "/hello5",produces = "text/html; charset=UTF-8")

## 五、重定向和转发

## 六、拦截器

拦截器(interceptor)，运用了AOP思想实现，类实现HandlerInterceptor接口并重写方法就可以实现拦截器。

配置spring

```xml
<mvc:interceptors>
    <mvc:interceptor>
        <!--拦截所有路径-->
        <mvc:mapping path="/**"/>
        <!--不拦截的路径-->
        <mvc:exclude-mapping path="/test"/>
        <bean class="com.chaoran.config.interceptor"/>
    </mvc:interceptor>
</mvc:interceptors>
```



