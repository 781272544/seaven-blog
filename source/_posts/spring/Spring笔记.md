---
title: Spring笔记
date: 2022-05-09 09:05:12
tags: ["spring"]
categories: ["spring"]
---

* 耦合：程序间的依赖关系。包括：类和方法之间的依赖

* 解耦：降低程序间的依赖关系。在实际开发中应该做到编译时不依赖，运行时才依赖。

* 解耦思路：1.使用反射来创建对象，而避免使用new关键字。2.通过读取配置文件来获取要创建的对象的全限定类名。

* 依赖注入和控制反转是同一概念：

  ​        依赖注入和控制反转是对同一件事情的不同描述，从某个方面讲，就是它们描述的角度不同。依赖注入是从应用程序的角度在描述：应用程序依赖容器创建并注入它所需要的外部资源；而控制反转是从容器的角度在描述：容器控制应用程序，由容器反向的向应用程序注入应用程序所需要的外部资源。

* JavaBean：用Java语言编写的可重用组件

  1.需要配置文件来配置我们的service和dao。配置的内容：全限定类名

  2.通过读取配置文件中的配置内容，反射创建对象

#### 核心容器的两个接口

* applicationContext：是BeanFactory的子接口。它在构建核心容器时，创建对象采用的策略是立即加载的方式。只要一读取完配置文件马上就创建配置文件中配置的对象。[单例对象适用]

* BeanFactory：它在构建核心容器时，创建对象采用的策略是延迟加载的方式。什么时候根据id获取对象了，什么时候才真正创建对象。[多例对象适用]

#### Spring对bean的管理细节


1.xml中创建bean的三种方式

第一种方式：使用默认构造函数创建。在Spring的配置文件中使用bean标签，配以id和class属性之后，且没有其他属性和标签时。采用的就是默认构造函数创建bean对象，此时如果类中没有默认构造函数，则对象无法创建。

```
<bean id="userService" class="com.fc.service.imp.UserServiceImpl"></bean>
```

第二种方式：使用普通工厂中的方法创建对象（把工厂的创建交给Spring来管理，然后使用工厂的bean来调用里面的方法来创建对象，并存入Spring容器）

```
<bean id="instanceFactory" class="com.fc.factory.InstanceFactory"></bean>
<bean id="userService" factory-bean="instanceFactory" factory-method="getUserService"></bean>
```

第三种方式：使用工厂中的静态方法创建对象（使用某个类中的静态方法创建对象，并存入Spring容器）

```
<bean id="userService" class="com.fc.factory.StaticFactory" factory-method="getUserService"></bean>
```

2.bean对象的作用范围

bean标签的scope属性：

作用：用于指定bean对象的作用范围

取值：singleton:单例的（默认值）

  ​			    prototype：多例的

request：作用于web应用的请求范围，第一次请求有效，其他请求无效(转发有效，重定向无效)

session：作用于web应用的会话范围，同一次会话有效(无论怎么跳转都有效，退出系统后无效)

  ​			    global-session：作用于集群环境的会话范围，当不是集群环境时，它就是session

  3.bean对象的生命周期

  ​	单例对象：

  ​		出生：当容器创建时对象出生

  ​		活着：只要容器还在，对象一直活着

  ​		死亡：容器销毁，对象消亡

总结：单例对象的生命周期和容器相同

多例对象：

出生：当我们使用对象时Spring框架为我们创建

活着：对象只要是在使用过程中就一直活着

死亡：当对象长时间不用，且没有别的对象引用，由Java的垃圾回收器回收

#### Spring中的依赖注入

依赖注入：dependency injection

IOC的作用：降低程序间的耦合

依赖关系的管理：以后都交给Spring来维护。在当前类需要其他类的对象时，由Spring为我们提供，我们只需要在配置文件中说明

依赖关系的维护：称之为依赖注入

​	依赖注入能注入的数据有三类：

​		基本类型和String

​		其他bean类型（在配置文件中或者注解中配置过的bean）

​		复杂类型/集合类型

##### 	依赖注入的方式有三种：

> 使用构造函数

> 使用set方法，注解属于set注入

> 使用工厂注入

* 使用构造函数：

		bean内使用==constructor-arg==标签

		type属性：用于指定要注入的数据的数据类型，该数据类型也是构造函数中的某个或某些参数的类型
		
		index属性：用于指定要注入的数据给构造函数中指定索引位置的参数赋值。索引的位置从0开始
		
		name属性：用于指定构造函数中指定名称的参数赋值【更常用】
		
		value属性：用于提供基本类型和string类型的数据
		
		ref属性：用于指定其他的bean类型数据。它指定的是在Spring的IOC核心容器中出现过的bean对象
		
		优势：在获取bean对象时，注入数据是必须的操作，否则对象无法创建成功
		
		劣势：改变了bean对象的实例化方式，使得我们在创建对象时，如果用不到这些数据时，也必须提供

```
<bean id="userService" class="com.fc.service.imp.UserServiceImpl">
	<constructor-arg name="name" value="张三"></constructor-arg>
	<constructor-arg name="age" value="18"></constructor-arg>
	<constructor-arg name="birthday" ref=now"></constructor-arg>
</bean>
<bean id="now" class="java.util.Date"></bean>
```

* 使用set方法         【更常用】

		bean内使用==property==标签

		name属性：用于指定构造函数中指定名称的参数赋值
		
		value属性：用于提供基本类型和string类型的数据
		
		ref属性：用于指定其他的bean类型数据。它指定的是在Spring的IOC核心容器中出现过的bean对象
		
		优势：创建对象时没有明确的限制，可以直接使用默认构造函数
		
		劣势：如果某个成员必须有值，则获取对象时有可能set方法没有执行

```
<bean id="userService" class="com.fc.service.imp.UserServiceImpl">
	<property name="name" value="张三"></property>
	<property name="age" value="18"></property>
	<property name="birthday" ref=now"></property>
</bean>
<bean id="now" class="java.util.Date"></bean>
```

​		复杂类型/集合类型注入配置如下：

​			用于给list结构集合注入的标签：list array set

​			用于给map结构集合注入的标签：map props

​			结构相同，标签可以互换

```
<bean id="userService" class="com.fc.service.imp.UserServiceImpl">
	<property name="mylist">
		<set>
			<value>aaa</value>
			<value>bbb</value>
			<value>ccc</value>
		</set>
	</property>
	<property name="mymap">
		<map>
			<value>aaa</value>
			<value>bbb</value>
			<value>ccc</value>
		</map>
	</property>
</bean>
```

* 工厂注入
  * bean标签内使用==property+ref==标签，引入配置文件中通过静态工厂或者实例工厂产生的bean

    ```
     <bean id="instanceFactory" class="com.fc.factory.InstanceFactory"></bean>
     <bean id="userService" factory-bean="instanceFactory" factory-method="getUserService"></bean>
      
    <bean id="userController" class="com.fc.UserController">
        <!-- 将上面通过实例工厂方法创建的bean，注入到userController中 -->
        <property name="service" ref="userService"/>
    </bean>
    ```

    ```
    <bean id="userService" class="com.fc.factory.StaticFactory" factory-method="getUserService"></bean>
    
    <bean id="userController" class="com.fc.UserController">
        <!-- 将上面通过静态工厂方法创建的bean，注入到userController中 -->
        <property name="service" ref="userService"/>
    </bean>
    ```

    

#### spring中的注解

1.用于创建对象的注解：作用就和在XML配置文件中编写一个bean标签实现的功能是一样的

​	@Component  @Controller @Service @Repository 四个作用一样

​		用于把当前类对象存入Spring容器中

​		value属性 用于指定bean的id。默认值为当前类名，且首字母小写

​		使用时需要在XML配置文件中告知Spring在创建容器时要扫描的包，配置如下：

```
<context:component-scan base-package="com.fc"></context:component-scan>
```

2.用于注入数据的注解：作用就和在XML配置文件中的bean标签中写一个property标签的作用是一样的

​	@Autowired

​		用于自动按照类型注入。只要容器中唯一的一个bean对象类型和要注入的变量类型匹配，就可以注入成功。可用于变量上，亦可用于方法上。在使用注解注入时，set方法就不是必须的。

​		当容器中有多个类型相同的bean对象时，bean对象的id需要和变量名保持一致，否则会报错。

​	@Qualifier

​		用于在按照类型注入的基础上再按照名称注入。在给类成员注入时不能单独使用（和@Autowired配合使用），给方法参数注入时可以。

​		value值为：bean对象的id

​	@Resource

​		作用：直接按照bean的id注入。可以单独使用

​		属性name：用于指定bean的id

​	以上三个注入都只能注入其他bean类型的数据，而基本类型和string类型无法使用上述注解实现。另外，集合类型的注入只能通过XML来实现。

​	@Value

​		用于注入基本类型和string类型的数据。

​		属性：value用于指定数据的值。它可以使用Spring中SpEl(也就是Spring中的el表达式)

​		

3.用于改变作用范围的注解：他们的作用就和在bean标签中使用scope属性实现的功能是一样的

​	@Scope 用于指定bean的作用范围

​		属性value：指定范围的取值。常用singleton prototype

4.和生命周期相关的注解：他们的作用就和在bean标签中使用init-method和destroy的作用是一样的

​	@preDestory 用于指定销毁方法

​	@postConstruct 用于指定初始化方法


​	5.其它注解

​		@configuration 用于指定当前类是一个配置类

​		@ComponentScan 用于通过注解指定Spring在创建容器时要扫描的包

​			属性value：它和XML文件中的context:component-scan作用一样，用于指定创建容器时要扫描的包

​		@Bean 用于把当前方法的返回值作为bean对象存入Spring的IOC容器中

​			当我们是使用注解配置方法时，当方法有参数时，Spring框架会去容器中查找有没有可用的bean对象

​		@Import 用于导入其他配置类

​		@Value 

​		@PropertySource 用于指定properties文件的位置		属性value：指定文件路径和名称	关键字classpath表示类路径下

#### Spring+Junit

配置示例：【spring_text_transaction】

1.导入Spring-text的jar坐标

2.使用@Runwith注解测试类，用于把原有Junit提供的main方法替换成Spring提供的main方法。

3.使用@ContextConfiguration注解测试类，用于告知Spring的运行器，Spring的IOC创建是基于XML提供，还是注解提供，并说明位置

​	locations：指定XML文件的位置	（locations=“classpath：bean.xml”）

​	classes：指定注解类所在位置	（classes=SpringConfiguration.class）

最后当Spring版本为5.x的时候，要求Junit必须是4.12及以上。



#### AOP

* AOP 就是把我们程序中重复的代码抽取出来，在需要执行的时候，使用动态代理技术，在不修改源码的基础上，对我们的已有方法进行加强

  ​	优势：减少重复代码  提高开发效率  维护方便

  * 银行转账案例：由于事务被自动控制了，每次执行一条语句没有问题。但在执行多条SQL语句时，且中间出现错误时，会导致钱不翼而飞了。

  * 解决方法：让业务层来控制事务的提交和回滚

    开启事务		提交事务		回滚事务		释放资源

  * 动态代理技术：解耦、方法增强

    * 基于接口的动态代理

      ​	由JDK提供的proxy类，要求被代理类最少实现一个接口

    * 基于子类的动态代理

      ​	第三方的CGLib，要求被代理类不能用final修饰类（最终类）

  * Joinpoint（连接点）指那些被拦截到的点。在Spring中，这些点指的是方法，因为Spring只支持方法类型的连接点

  * pointcut（切入点）指的是我们要对哪些joinpoint进行拦截的定义

  * advice（通知/增强）是指拦截到joinpoint之后所要做的事情就是通知。

    ​	通知类型：前置通知、后置通知、异常通知、最终通知、环绕通知。

    ​	    环绕通知是spring框架为我们提供的一种可以在代码中手动控制增强方法何时执行的方式。

![](D:\笔记文件\picture\AOP.PNG)

* 

* 配置AOP

  * 第一步：把通知类用bean标签配置起来

  * 第二步：使用aop：config声明aop配置

  * 第三步：使用aop：aspect配置切面

  * 第四步：使用aop：pointcut配置切入点表达式

  * 第五步：使用aop：xxx配置对应通知类型

    ```
    <bean id="txLogger" class="com.fc.utils.Logger"></bean>
        <!--配置AOP-->
        <aop:config>
            <!--配置切面 -->
            <aop:aspect id="txAdvice" ref="txLogger">
                <!-- 配置通知的类型，并且建立通知方法和切入点方法的关联-->
                <aop:before method="printLog" pointcut="execution(* com.fc.service.impl.*.*(..))"></aop:before>
            </aop:aspect>
        </aop:config>
    </bean>
    ```

    切入点表达式语法： execution([修饰符] 返回值类型 包名.类名.方法名(参数)) 

    ​	修饰符可以省略

    ​	返回值类型、类名、方法名、参数用*表示任意类型、任意类名、任意方法名、任意参数

    ​	用..表示有无参数均可，有参数可以是任意类型

    ```
    <bean id="txLogger" class="com.fc.utils.Logger"></bean>
    <aop:config>
    	<aop:pointcut expression="execution(* com.fc.service.impl.*.*(..))" id="pt1"/>
    		<aop:aspect id="txAdvice" ref="txLogger">
    			<!-- 配置环绕通知 -->
    			<aop:around method="transactionAround" pointcut-ref="pt1"/>
    		</aop:aspect>
    </aop:config>
    ```

  * Spring 框架监控切入点方法的执行。一旦监控到切入点方法被运行，使用代理机制，动态创建目标对

  象的代理对象，根据通知类别，在代理对象的对应位置，将通知对应的功能织入，完成完整的代码逻辑运行 

  * aop的应用：事务管理、日志记录、缓存、加密处理、权限校验

* 注解版
  * 切面类上使用@Aspect 
  * 通知方法上使用@Before 前置通知
  * 通知方法上使用@AfterReturning 最终通知
  * 通知方法上使用@AfterThrowing 异常通知
  * 通知方法上使用@After 后置通知
  * 通知方法上使用@Around 环绕通知
  * 

#### Spring的事务隔离级别

* ISOLATION_DEFAULT    使用数据库的默认隔离级别
* ISOLATION_READ_UNCOMMITTED     允许另一个事务可以看到这个事务未提交的数据。这种隔离级别会产生脏读，不可重复读和幻读
* ISOLATION_READ_COMMITTED   一个事务修改的数据提交后才能被另外一个事务读取。解决了脏读问题，但是可能会出现不可重复读和幻读
* ISOLATION_REPEATABLE_READ   解决了脏读，不可重复读，但是可能会出现幻读
* ISOLATION_SERIALIZABLE   串行化，事务被处理为顺序执行，防止脏读、不可重复读、幻读

#### 事务的传播

当事务方法被另一个事务方法调用时，必须指定事务应该如何传播。例如：方法可能继续在现有的事务中运行，也可能开启一个新事务，然后在这个新事务中运行。

* Spring的事务传播级别

  * PROPAGATION_REQUIRED: 默认的事务传播行为。如果调用者存在一个事务，则支持调用者的事务，与调用者处于同一个事务上下文，回滚统一回滚。如果调用者没有事务，则开启一个新事务。
  * PROPAGATION_SUPPORTS: 如果调用者存在一个事务，则支持调用者的事务，与调用者处于同一个事务上下文，回滚统一回滚。如果调用者没有事务，则以非事务方法执行。
  * PROPAGATION_MANDATORY: 如果调用者存在一个事务，则支持调用者的事务，与调用者处于同一个事务上下文，回滚统一回滚。如果调用者没有事务，则抛出异常。 
  * PROPAGATION_REQUIRES_NEW: 新建一个事务，如果调用者存在一个事务，则把当前事务挂起。这两个事务不处于同一个上下文，如果各自发生异常，各自回滚。
  * PROPAGATION_NOT_SUPPORTED: 以非事务方式运行操作。如果调用者存在一个事务，就把当前事务挂起。
  * PROPAGATION_NEVER: 以非事务方式执行操作，如果调用者存在一个事务，则抛出异常。
  * PROPAGATION_NESTED: 如果调用者不存在事务，则新建一个事务。如果调用者存在一个事务，则以嵌套事务执行该方法。嵌套事务是指：内层事务依赖于外层事务，外层事务失败时，会回滚内层事务的操作；而内层事务操作失败并不会引起外层事务的回滚。

* 获取IOC容器中的bean

  ```
  @Component
  public class SpringBeanUtil implements ApplicationContextAware {
  
      private static ApplicationContext applicationContext;
  
      @Override
      public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
          this.applicationContext = applicationContext;
      }
  
      public ApplicationContext getApplicationContext() {
          return applicationContext;
      }
  }
  
  @Autowired
  SpringBeanUtil springBeanUtil;
  springBeanUtil.getApplicationContext().getBean
  
  ```

  

### spring中bean的装配方式

* 隐式的bean扫描发现机制和自动装配
  * 通过@Component和@ComponentScan，配合@Autowired（自动装配）
* 在java中进行显示配置
  * 通过@Configuration和@bean
* 在XML中进行显示配置
  * 通过`<bean></bean>`

### spring bean 依赖注入的方式

* setter方法
* 构造方法
* 自动装配
  * xml方式
  * 注解方式

### spring bean自动装配的方式

* byName
* byType
* constructor
* no手动指定
