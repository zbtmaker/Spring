# 1、Bean作用域-Singleton模式
Singleton方式的作用域表明Spring在同一个ApplicationContext下只会调用JavaBean类的一次构造方法，如果第二次调用返回的是同一个JavaBean实例。这种方式就好像我们将这个JavaBean类实现了单例模式的方式来创建我们的对象。
## 1.1、Singleton在同一个ApplicationContext下
### <1>JavaBean实例
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
### <2>配置文件
```Java
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
	
   <bean id="lifeBeans" class="chapter3.scopingbeans.LifeBeans" scope="singleton">
  	<constructor-arg value="Jack Chen"/>
   </bean>
</beans>
```
配置文件中有一些不同的地方，就是<bean>这个元素中添加了scope这个属性同时我们将属性的值设置为了singleton的方式
```Java
<bean id="lifeBeans" class="chapter3.scopingbeans.LifeBeans" scope="singleton">
```

### <3>测试代码
```Java
@Test
	public void testSingletone() {
		AbstractApplicationContext ac = new ClassPathXmlApplicationContext("LifeBeans.xml");
		
		LifeBeans l1 = (LifeBeans)ac.getBean("lifeBeans");
		
		LifeBeans l2 = (LifeBeans)ac.getBean("lifeBeans");
		
		if(l1 == l2) {
			System.out.println("l1 and l2 reference the same address of LifeBean");
		}else {
			System.out.println("l1 and l2 reference the different address of LifeBean");
		}
		ac.close();
	}
```
### <4>输出结果
```Java
Constructor of LifeBean()
l1 and l2 reference the same address of LifeBean
```
我们可以从输出结果中看到构造器只调用了一次，但是我们却调用了两次getBean()方法。也就证明了在Spring的一个ApplicationContext下采用单例模式创建我们的Bean实例。  
同时我们需要注意的是，如果我们的<bean>元素中并没有添加scope属性时，那么此时Spring默认采用单例模式创建bean实例

## 1.2、Sington在不同的ApplicationContext下
前面我们提到了在同一个ApplicationContext下，采用默认的作用域或者通过设置scope属性使得Spring采用单例模式的方式去创建Bean。使用这种方式返回的都是同一个实例，那么我们如果在两个或者多个ApplicationContext下，也采用单例模式创建Bean实例，那么是两个变量引用同一个变量还是引用不同的变量？我们来看下面的测试代码(JavaBean类、配置文档和前面的一致)
### <1>测试代码
```Java
@Test
	public void testSingletonContext() {
		AbstractApplicationContext ac = new ClassPathXmlApplicationContext("LifeBeans.xml");		
		
		LifeBeans l1 = (LifeBeans)ac.getBean("lifeBeans");

		AbstractApplicationContext ac1 = new ClassPathXmlApplicationContext("LifeBeans.xml");
		
		LifeBeans l2 = (LifeBeans)ac1.getBean("lifeBeans");
		
		if(l1 == l2) {
			System.out.println("l1 and l2 reference the same address of LifeBean");
		}else {
			System.out.println("l1 and l2 reference the different address of LifeBean");
		}
		ac1.close();
		ac.close();
	}
```
### <2>输出结果
```Java
Constructor of LifeBean()
Constructor of LifeBean()
l1 and l2 reference the different address of LifeBean
```
### <3> 结论
我们可以从结果中看到调用了两次构造方法。因此在不同的ApplicationContext中使用调用getBean()方法，在每个ApplicationContext下都会调用一次构造方法创建一个Bean实例。

# 2、Bean作用域-prototype模式
### <1>avaBean类
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
### <2>配置文件
```Java
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
	
   <bean id="lifeBeans" class="chapter3.scopingbeans.LifeBeans" scope="prototype">
    <property name="name" value="Jay Chou"/> 
   	<!-- <constructor-arg value="Jack Chen"/>--> 
   </bean>
</beans>
```
### <3>输出结果
```Java
Constructor of LifeBean()
Constructor of LifeBean()
l1 and l2 reference the different address of LifeBean
```





