# Spring
构造器注入-字面量注入
```Java
package chapter2.xmlbean;

public class HelloWorld {
	private String message;
	
	public HelloWorld(String message) {
		this.message = message;
	}
	public void getMessage() {
		System.out.println("YourMessage ： "+ message);
	}
	
}
```
配置方式一：使用<constructor-arg>元素
```java
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <bean id="helloWorld" class="chapter2.xmlbean.HelloWorld">
       <constructor-arg type="java.lang.String" value="Hello Good Life!"/>
   </bean>

</beans>
```
配置方式二
```java
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <bean id="helloWorld" class="chapter2.xmlbean.HelloWorld">
       <constructor-arg type="java.lang.String" value="Hello Good Life!"/>
   </bean>

</beans>
```
Setter方法注入-注入字面量

异常信息1
```Java
Exception encountered during context initialization - cancelling refresh attempt: org.springframework.beans.factory.CannotLoadBeanClassException: Cannot find class [chapter2.xmlbean] for bean with name 'helloWorld' defined in class path resource [bean.xml]; nested exception is java.lang.ClassNotFoundException: chapter2.xmlbean
Exception in thread "main" org.springframework.beans.factory.CannotLoadBeanClassException: Cannot find class [chapter2.xmlbean] for bean with name 'helloWorld' defined in class path resource [bean.xml]; nested exception is java.lang.ClassNotFoundException: chapter2.xmlbean
	at org.springframework.beans.factory.support.AbstractBeanFactory.resolveBeanClass(AbstractBeanFactory.java:1380)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.determineTargetType(AbstractAutowireCapableBeanFactory.java:666)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.predictBeanType(AbstractAutowireCapableBeanFactory.java:633)
	at org.springframework.beans.factory.support.AbstractBeanFactory.isFactoryBean(AbstractBeanFactory.java:1489)
	at org.springframework.beans.factory.support.AbstractBeanFactory.isFactoryBean(AbstractBeanFactory.java:1012)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:740)
	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:869)
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:550)
	at org.springframework.context.support.ClassPathXmlApplicationContext.<init>(ClassPathXmlApplicationContext.java:144)
	at org.springframework.context.support.ClassPathXmlApplicationContext.<init>(ClassPathXmlApplicationContext.java:85)
	at chapter2.xmlbean.HelloWorldTest.main(HelloWorldTest.java:9)
Caused by: java.lang.ClassNotFoundException: chapter2.xmlbean
	at java.net.URLClassLoader.findClass(Unknown Source)
	at java.lang.ClassLoader.loadClass(Unknown Source)
	at sun.misc.Launcher$AppClassLoader.loadClass(Unknown Source)
	at java.lang.ClassLoader.loadClass(Unknown Source)
	at org.springframework.util.ClassUtils.forName(ClassUtils.java:274)
	at org.springframework.beans.factory.support.AbstractBeanDefinition.resolveBeanClass(AbstractBeanDefinition.java:437)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doResolveBeanClass(AbstractBeanFactory.java:1428)
	at org.springframework.beans.factory.support.AbstractBeanFactory.resolveBeanClass(AbstractBeanFactory.java:1372)
	... 10 more

```
异常信息2
```
Exception in thread "main" org.springframework.beans.factory.NoSuchBeanDefinitionException: No bean named 'HelloWorld' available
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.getBeanDefinition(DefaultListableBeanFactory.java:686)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getMergedLocalBeanDefinition(AbstractBeanFactory.java:1210)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:291)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:199)
	at org.springframework.context.support.AbstractApplicationContext.getBean(AbstractApplicationContext.java:1089)
	at chapter2.xmlbean.HelloWorldTest.main(HelloWorldTest.java:12)
```
bean.xml配置文件如下
```Java
<bean id="helloWorld" class="chapter2.xmlbean.HelloWorld">
       <constructor-arg type="java.lang.String" value="Hello Good Life!"/>
   </bean>
```
而get语句如下
```Java
HelloWorld hwl = (HelloWorld)context.getBean("HelloWorld");
```
异常分析：

Setter方法注入-注入字面量
```Java
package chapter2.xmlbean;

public class BlankDisk {
	private String title;
	private String artist;
	
	public void setTitle(String title) {
		this.title = title;
	}
	
	public void setArtist(String artist) {
		this.artist=artist;
	}
	
	public void message() {
		System.out.println("title:"+title+"  &  "+"artist:"+artist);
	}
}
```
测试代码
```Java
package chapter2.xmlbean;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class DiskTest {
	private static ApplicationContext context;

	public static void main(String[] args) {
		context = new ClassPathXmlApplicationContext("bean.xml");
		BlankDisk bd = (BlankDisk)context.getBean("blankDisk");
		bd.message();
	}
}
```
配置文件-使用<property>元素
```Java
package chapter2.xmlbean;

public class BlankDisk {
	private String title;
	private String artist;
	
	public void setTitle(String title) {
		this.title = title;
	}
	
	public void setArtist(String artist) {
		this.artist=artist;
	}
	
	public void message() {
		System.out.println("title:"+title+"  &  "+"artist:"+artist);
	}
}
```
输出结果
```Java
title:Paris  &  artist:Jar Chou
```
配置文件-使用p命名空间
需要注意的是两个：第一个是要引入约束，第二个是使用p命名空间的语法
* 在beans中引入约束如下
```Java
xmlns:p="http://www.springframework.org/schema/p"
```
* p命名空间的语法
如果注入的是字面量，那么直接是

```Java
<bean id="?" class="?" p:属性名="?">
</bean>
```
如果注入的是引用，那么语法如下
```Java
<bean id="?" class="?" p:属性名-ref="所注入bean的ID">
</bean>

```
最后的配置文件如下
```Java
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
	<!--  
   <bean id="helloWorld" class="chapter2.xmlbean.HelloWorld">
		<property name="message" value="Hello Good Future!"/>
   </bean>
   -->
   <bean id="blankDisk" class="chapter2.xmlbean.BlankDisk"
   			p:title="Paris"
   			p:artist="Jar Chou">
   </bean>
</beans>
```
##Setter方法注入-注入list/set
```Java
package chapter2.xmlbean;

import java.util.List;

public class BlankDisk {
	private String title;
	private String artist;
	
	private List<String> tracks;
	public void setTitle(String title) {
		this.title = title;
	}
	
	public void setArtist(String artist) {
		this.artist=artist;
	}
	
	public void setTracks(List<String> tracks) {
		this.tracks = tracks;
	}
	public void message() {
		System.out.println("title:"+title+"  &  "+"artist:"+artist);
		for(String s:tracks) {
			System.out.println(s);
		}
	}
}
```
配置文件
如果注入的是字面量语法：
<property name="属性名">
	<list>
		<value>字面量1</value>
		<value>字面量2</value>
	<list>
</property>
```Java
如果注入的是引用语法：
```Java
<property name="属性名">
	<list>
		<ref bean="id1"/>
		<ref bean="id2">
	<list>
</property>
```
最终的配置文件为
```java
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
   <bean id="blankDisk" class="chapter2.xmlbean.BlankDisk"
   			p:title="Paris"
   			p:artist="Jar Chou">
   		<property name="tracks">
   			<list>
   				<value>Lonely</value>
   				<value>Courage</value>
   				<value>Confident</value>
   			</list>
   		</property>
   </bean>
</beans>
```

## 构造器注入和Setter方法注入

### 1、当一个JavaBean中没有带参的构造方法，同时也没有写无参的构造方法时，系统会自动调用无参的构造方法
JavaBean类
```Java
package chapter3.scopingbeans;

public class LifeBeans {
	private String name;
	
	public LifeBeans() {
		System.out.println("Constructor of LifeBean()");
	}
	
	public String getName() {
		return name;
	}
	
	public void setName(String name) {
		this.name = name;
	}
	
	public void init() {
		System.out.println("this is init of LifeBean");
	}
	
	public void destory() {
		System.out.println("this is destory of LifeBean"+this);
	}
}
```
* 配置文件
```Java	
   <bean id="lifeBeans" class="chapter3.scopingbeans.LifeBeans" scope="singleton">
    <property name="name" value="Jay Chou"/> 
   	<!-- <constructor-arg value="Jack Chen"/> -->
   </bean>
```
* 输出结果
```Java
Constructor of LifeBean()
Jay Chou
```

### 2、当一个JavaBean中同时写了带参构造和无参构造方法，并且我们在配置文件中配置了带参构造方法的注入属性，此时Spring不会调用无参构造
```Java
package chapter3.scopingbeans;

public class LifeBeans {
	private String name;
	
	public LifeBeans() {
		System.out.println("Constructor of LifeBean()");
	}
	public LifeBeans(String name) {
		this.name = name;
	}
	public String getName() {
		return name;
	}
	
	public void setName(String name) {
		this.name = name;
	}
	
	public void init() {
		System.out.println("this is init of LifeBean");
	}
	
	public void destory() {
		System.out.println("this is destory of LifeBean"+this);
	}
}
```
配置文件LifeBeans.xml
```Java
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
	
   <bean id="lifeBeans" class="chapter3.scopingbeans.LifeBeans" scope="singleton">
   	<!-- <property name="name" value="Jay Chou"/> -->
   	<constructor-arg value="Jack Chen"/>
   </bean>
</beans>
```

### 3、当一个JavaBean同时采用构造器注入和Setter注入时，系统会自动忽略构造器注入的方式
* JavaBean类
```Java
package chapter3.scopingbeans;

public class LifeBeans {
	private String name;
	
	public LifeBeans() {
		System.out.println("Constructor of LifeBean()");
	}
	public LifeBeans(String name) {
		this.name = name;
	}
	public String getName() {
		return name;
	}
	
	public void setName(String name) {
		this.name = name;
	}
	
	public void init() {
		System.out.println("this is init of LifeBean");
	}
	
	public void destory() {
		System.out.println("this is destory of LifeBean"+this);
	}
}

```
* 配置文件
```Java
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
	
   <bean id="lifeBeans" class="chapter3.scopingbeans.LifeBeans" scope="singleton">
    <property name="name" value="Jay Chou"/> 
   	<constructor-arg value="Jack Chen"/>
   </bean>
</beans>
```
* 输出结果
```Java
Constructor of LifeBean()
Jay Chou
```
