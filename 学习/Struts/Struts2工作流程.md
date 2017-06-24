### 请求在Struts2框架中的处理大概分为以下几个步骤：
1. 客户端初始化一个指向Servlet容器的请求；
2. 这个请求经过一系列的过滤器（Filter）（这些过滤器中有一个叫做ActionContextCleanUp的可选过滤器，这个过滤器对于Struts2和其他框架的集成很有帮助，例如：SiteMesh Plugin）
3. 接着FilterDispatcher被调用，FilterDispatcher询问ActionMapper来决定这个请是否需要调用某个Action
4. 如果ActionMapper决定需要调用某个Action，FilterDispatcher把请求的处理交给ActionProxy
5. ActionProxy通过Configuration Manager询问框架的配置文件，找到需要调用的Action类
6. ActionProxy创建一个ActionInvocation的实例。
7. ActionInvocation实例使用命名模式来调用，在调用Action的过程前后，涉及到相关拦截器（Intercepter）的调用。
8. 一旦Action执行完毕，ActionInvocation负责根据struts.xml中的配置找到对应的返回结果。返回结果通常是（但不总是，也可 能是另外的一个Action链）一个需要被表示的JSP或者FreeMarker的模版。在表示的过程中可以使用Struts2 框架中继承的标签。在这个过程中需要涉及到ActionMapper###拦截器、过滤器

### 过滤器与拦截器的区别
1. 拦截器是基于java的反射机制的，而过滤器是基于函数回调。
2. 拦截器不依赖与servlet容器，过滤器依赖与servlet容器。
3. 拦截器只能对action请求起作用，而过滤器则可以对几乎所有的请求起作用。
4. 拦截器可以访问action上下文、值栈里的对象，而过滤器不能访问。
5. 在action的生命周期中，拦截器可以多次被调用，而过滤器只能在容器初始化时被调用一次

### 过滤器定义:
```xml
<filter>
  <filter-name>TestFilter</filter-name>
  <filter-class>com.filter.TestFilter</filter-class>
</filter>
<filter-mapping>
  <filter-name>TestFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```

### 拦截器定义:
```xml
<interceptors>
  <interceptor name="testInterceptor" class="com.xxxx.TestInterceptor" />
  <interceptor-stack name="filterIPStack">
    <interceptor-ref name="testInterceptor" />
  </interceptor-stack>
</interceptors>
```

<http://zhidao.baidu.com/question/583953960492530045.html>

### 监听器的定义：
1. 监听器实际上是一个类，这个类实现了特定的接口，然后将这个类在 web.xml 文件中进行描述，这样服务器在启动的时候就可以实例化这个类，启动监听器。当范围对象的状态发生变化的时候，服务器自动调用监听器对象中的方法。例如统计用户在线人数。
2. web监听器是Servlet规范中定义的一种特殊类，用于监听ServletContext，HttpSession,ServletRequest等域对象的创建、销毁、以及属性的变化等，可以在事件发生前、发生后进行一些处理。
3. 监听器的用途:
	* 统计在线人数和在线用户
	* 系统加载时进行信息的初始化工作
	* 统计网站的访问量
	* 跟Spring结合
4. 按监听的对象划分，可以分为监听ServletContext对象HttpSession对象ServletRequest对象

#### 在web.xml中配置session超时
```xml
<session-config>
    <session-timeout> 30 </session-timeout>
</session-config>
```

### Hibernate与MyBatis的比较
Hibernate的调优方案:
制定合理的缓存策略；尽量使用延迟加载特性；采用合理的Session管理机制；使用批量抓取，设定合理的批处理参数（batch\_size）;进行合理的O/R映射设计
Mybatis调优方案:
MyBatis在Session方面和Hibernate的Session生命周期是一致的，同样需要合理的Session管理机制。MyBatis同样具有二级缓存机制。 MyBatis可以进行详细的SQL优化设计。
<http://blog.csdn.net/firejuly/article/details/8190229>

SQL查询
<http://www.cnblogs.com/yubinfeng/archive/2010/11/02/1867386.html>

truncate和delete之间有什么区别TRUNCATE TABLE 在功能上与不带 WHERE 子句的 DELETE 语句相同：二者均删除表中的全部行。但 TRUNCATE TABLE 比 DELETE 速度快，且使用的系统和事务日志资源少。 DELETE 语句每次删除一行，并在事务日志中为所删除的每行记录一项。
TRUNCATE TABLE 通过释放存储表数据所用的数据页来删除数据，并且只在事务日志中记录页的释放。
TRUNCATE,DELETE,DROP放在一起比较：
TRUNCATE TABLE：删除内容、释放空间但不删除定义。
DELETE TABLE:删除内容不删除定义，不释放空间。
DROP TABLE：删除内容和定义，释放空间。
