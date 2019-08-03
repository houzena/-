### SpringMvc使用

##### 一、配置文件

web.xml（配置DispatcherServlet，所有请求都要经过这个，然后分发）:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.4" xmlns="http://java.sun.com/xml/ns/j2ee" .. >
	<servlet>
		<servlet-name>springmvc</servlet-name>
		<servlet-class>
			org.springframework.web.servlet.DispatcherServlet
		</servlet-class>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>springmvc</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
</web-app>
```

springmvc-servlet.xml（配置路径映射，声明后用注解配置就行）：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"... >
	//定位目录
	<context:component-scan base-package="controller" />
    //定位视图，其作用是把视图约定在 /WEB-INF/page/*.jsp 这个位置
	<bean id="irViewResolver"
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/page/" />
		<property name="suffix" value=".jsp" />
	</bean>
</beans>

```

##### 二、注解配置

- 在类前面加上**@Controller** 表示该类是一个控制器
- 在方法handleRequest 前面加上 **@RequestMapping("/index")** 表示路径/index会映射到该方法上

- springmvc提供了很多转换器来将页面参数绑定到controller方法的形参上

  详细说明：<https://blog.csdn.net/qq_33530388/article/details/72784199>

  普通变量参数和传进来的参数名一致，可以自动绑定；

  对象作为参数，则他的成员属性名要和传进来的参数名一致，且要添加set方法？

示例：

```java
@Controller
public class ProductController {

	@RequestMapping("/addProduct")
	public ModelAndView add(Product product) throws Exception {
		ModelAndView mav = new ModelAndView("showProduct");
		return mav;
	}
}
```

##### 三、其它功能

1. 重定向跳转

```java
	@RequestMapping("/jump")
	public ModelAndView jump() {
		ModelAndView mav = new ModelAndView("redirect:/index");
		return mav;
	}	
```

2. 使用session

   ```java
   	@RequestMapping("/check")
   	public ModelAndView check(HttpSession session) {
   		Integer i = (Integer) session.getAttribute("count");
   		if (i == null)
   			i = 0;
   		i++;
   		session.setAttribute("count", i);
   		ModelAndView mav = new ModelAndView("check");
   		return mav;
   	}
   ```

3. 应对中文可能出现乱码，添加Filter

   在web.xml里面配置，增加如下字段：

   ```xml
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
           <url-pattern>/*</url-pattern>  <!-- 所有请求 -->
       </filter-mapping>  	
   ```

4. 访问静态资源

   - 在web.xml中新增加一段：

   ```xml
   	<servlet-mapping>
   	    <servlet-name>default</servlet-name>
   	    <url-pattern>*.jpg</url-pattern>
   	</servlet-mapping>
   ```

    因为配置springmvc的servlet的时候，使用的路径是**"/"**，导致静态资源在默认情况下不能访问，所以要加上下面一段，允许访问jpg。 **并且必须加在springmvc的servlet之前**

   - springmvc-servlet.xml中也要新增一段配置：

   `<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"/>`

   用于开放对上传功能的支持

   详细样例见：<http://how2j.cn/k/springmvc/springmvc-upload/621.html#nowhere>
     

5. 拦截器

   preHandle（）：在请求进入controller之前

   postHandle（）：执行完controller后

   afterCompletion（）：渲染完view后执行，用来释放资源

   详细还是见：<http://how2j.cn/k/springmvc/springmvc-interceptor/1141.html#nowhere>