## 一、内置注解

@override  表示一个方法声明打算重写超类中的另一个方法。

@Deprecated 表示不鼓励程序员使用这样的元素，通常是因为它很危险或者存在更好的选择

@SuppressWarnings 用来抑制编译时的警告信息。

## 二、元注解

元注解是用来对注解进行注解的注解。

@Target()   

​		表示该注解能用在什么地方

@Retention() 

​		表示该注解在什么状态还有效。

​		runtime>class>sources

@Documented 

​		表示是否将注解生成在JAVAdoc中

@Inherited

​		表示子类可以继承父类的注解