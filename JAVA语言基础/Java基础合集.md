### Java基础合集

##### 一、命名规范

- 包：全小写。域名倒置。
- 类和接口、文件名：大驼峰
- 变量：小驼峰
- 常量：全大写

##### 二、字符串（String、StringBuilder、StringBuffer）

1. 不可变的String

   - 构造方法：

     String（String/StringBuilder/StringBuffer/char []/byte []）、String.format()

   - 查找：

     indexOf（），lastIndexOf（）

   - 比较：

     equals(), equalsIgnoreCase()

     compareTo(), compareToIgnoreCase()

     endsWith(), startsWith()

     trim() :去掉空格

   - 截取

     substring()

     split()

2. StringBuilder、StringBuffer（两个的接口基本一样）
   - 构造方法StringBuilder（String）等
   - 追加、插入、删除字符串

##### 三、接口与抽象类的区别

抽象类基本与普通类无异，而接口类着许多的限制：

- 支持多继承
- 不能有实例成员变量，默认都是静态常量
- 没有构造方法
- 没有具体方法（Java8后可以通过声明默认方法写具体方法）

##### 四、枚举类型

枚举类 == 工厂 ？

- 构造方法有的话，则是私有的
- 常量列表好比工厂的实例化列表