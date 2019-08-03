### Mybatis

> 涉及jar包：mybatis-3.4.2.jar, mysql-connector-java-5.0.8-bin.jar

##### 一、主配置文件

mybatis-config.xml（src目录下）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--设置别名？ -->
	<typeAliases>
	  <package name="com.how2java.pojo"/>
	</typeAliases>
    <!--配置环境 -->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/how2java?characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="admin"/>
            </dataSource>
        </environment>
    </environments>
    <!-- 声明映射文件-->
    <mappers>
        <mapper resource="com/how2java/pojo/Category.xml"/>
    </mappers>
</configuration>

```

主要就是**配置环境**和**声明映射文件**，映射也能用注解来做，后期会说到

![](E:\读书笔记\readingNotes\框架\img\a.png)

##### 二、使用

```java
        //预处理
		String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        SqlSession session = sqlSessionFactory.openSession();
 		//获取数据
        Category c= session.selectOne("getCategory",3);
         
        System.out.println(c.getName());
         
        session.commit();
        session.close();
```

返回对象若是集合，则使用selectList

映射文件**Category.xml**（一般一个类对应一个xml？）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
	PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

	<mapper namespace="com.how2java.pojo">
	    <insert id="addCategory" parameterType="Category" >
	        insert into category_ ( name ) values (#{name})    
	    </insert>
	    
	    <delete id="deleteCategory" parameterType="Category" >
	        delete from category_ where id= #{id}   
	    </delete>
	    
	    <select id="getCategory" parameterType="_int" resultType="Category">
	        select * from   category_  where id= #{id}    
	    </select>

	    <update id="updateCategory" parameterType="Category" >
	        update category_ set name=#{name} where id=#{id}    
	    </update>
	    <select id="listCategory" resultType="Category">
	        select * from   category_      
	    </select>	    
	</mapper>
```

##### 三、高级点的使用

- 一对多

  指多表查询里的一对多关系，比如一个班有很多个学生，体现在对象上就是一个对象里包含一个集合。

  我们先来看看类：

  ```java
  package com.how2java.pojo;
  
  import java.util.List;
  
  public class Category {
  	private int id;
  	private String name;
  	List<Product> products;	//多个商品
  	
      //省略get/set方法
  }
  
  ```

  Category.xml文件：

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE mapper
  	PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  	"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  	<mapper namespace="com.how2java.pojo">
          <!-- 定义一个resultMap当作返回对象，其实就是手动匹配 -->
  		<resultMap type="Category" id="categoryBean">
  			<id column="cid" property="id" />
  			<result column="cname" property="name" />
  	
  			<!-- 一对多的关系 -->
  			<!-- property: 指的是集合属性的值, ofType：指的是集合中元素的类型 -->
  			<collection property="products" ofType="Product">
  				<id column="pid" property="id" />
  				<result column="pname" property="name" />
  				<result column="price" property="price" />
  			</collection>
  		</resultMap>
  	
  		<!-- 关联查询分类和产品表 -->
  		<select id="listCategory" resultMap="categoryBean">
  			select c.*, p.*, c.id 'cid', p.id 'pid', c.name 'cname', p.name 'pname' from category_ c left join product_ p on c.id = p.cid
  		</select>    
  	</mapper>
  
  ```

  说明：

  `<id column="cid" property="id" />`直接指定数据库中的**列**对应对象中的**属性**

