### Spring使用

### IOC方面：

##### 一、核心配置文件

applicationContext.xml（src目录下）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" ... >
 	//以下配置一个bean，索引名为c
    <bean name="c" class="com.how2java.pojo.Category">
    	//注入成员变量属性
        <property name="name" value="category 1" />
    </bean>
	<bean name="p" class="com.how2java.pojo.Product">
		<property name="name" value="product1" />
        //注入的成员变量为对象时
		<property name="category" ref="c" />
	</bean>
</beans>

```

说明：在为bean注入成员属性的时候，在对应的类里需要实现set/get方法，或者构造函数？

##### 二、注解方式的配置

需要在applicationContext.xml文件里声明：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" ... >
 	//声明bean的目录，用于扫描
 	<context:component-scan base-package="com.how2java.pojo"/>
 	
</beans>

```

同时为bean的类上需要加上@Component注解

常用注解：

- @Autowired、@Resource： 注入对象	（可以加在对象上，和set方法上）

### AOP方面：

配置文件方式就不说了，直接注解方式配置：

1. 配置文件：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans" ... >
    	//定位bean目录
    	<context:component-scan base-package="com.how2java.aspect"/>
    	<context:component-scan base-package="com.how2java.service"/>
       //aop声明
    	<aop:aspectj-autoproxy/>  
     
   </beans>
   
   ```

2. 注解示例

   ```java
   @Aspect
   @Component
   public class LoggerAspect { 
   	
   	@Around(value = "execution(* com.how2java.service.ProductService.*(..))")
   	public Object log(ProceedingJoinPoint joinPoint) throws Throwable {
   		System.out.println("start log:" + joinPoint.getSignature().getName());
   		Object object = joinPoint.proceed(); //执行原函数代码
   		System.out.println("end log:" + joinPoint.getSignature().getName());
   		return object;
   	}
   }
   ```

   @Aspect 注解表示这是一个切面
   @Component 表示这是一个bean,由Spring进行管理
   @Around(value = "execution(* com.how2java.service.ProductService.*(..))") 

   表示对com.how2java.service.ProductService 这个类中的所有方法进行切面操作

