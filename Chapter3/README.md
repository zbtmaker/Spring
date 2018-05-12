# Bean的作用域

## 1、Singleton在同一个Context下
* JavaBean实例
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

* 测试代码
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
* 输出结果
```Java
Constructor of LifeBean()
l1 and l2 reference the same address of LifeBean
```
我们可以从输出结果中看到构造器只调用了一次，但是我们却调用了两次getBean()方法。也就证明了在Spring的一个ApplicationContext下采用单例模式创建我们的Bean实例。  
同时我们需要注意的是，如果我们的<bean>元素中并没有添加scope属性时，那么此时Spring默认采用单例模式创建bean实例

## 2、Sington在不同的ApplicationContext下
前面我们提到了在同一个ApplicationContext下，采用默认的作用域或者通过设置scope属性使得Spring采用单例模式的方式去创建Bean。使用这种方式返回的都是同一个实例，那么我们如果在两个或者多个ApplicationContext下，也采用单例模式创建Bean实例，那么是两个变量引用同一个变量还是引用不同的变量？我们来看下面的测试代码(JavaBean类、配置文档和前面的一致)
* 测试代码
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
* 输出结果
```Java
五月 12, 2018 8:09:31 下午 org.springframework.context.support.AbstractApplicationContext prepareRefresh
信息: Refreshing org.springframework.context.support.ClassPathXmlApplicationContext@6956de9: startup date [Sat May 12 20:09:31 CST 2018]; root of context hierarchy
五月 12, 2018 8:09:31 下午 org.springframework.beans.factory.xml.XmlBeanDefinitionReader loadBeanDefinitions
信息: Loading XML bean definitions from class path resource [LifeBeans.xml]
Constructor of LifeBean()
五月 12, 2018 8:09:32 下午 org.springframework.context.support.AbstractApplicationContext prepareRefresh
信息: Refreshing org.springframework.context.support.ClassPathXmlApplicationContext@670b40af: startup date [Sat May 12 20:09:32 CST 2018]; root of context hierarchy
五月 12, 2018 8:09:32 下午 org.springframework.beans.factory.xml.XmlBeanDefinitionReader loadBeanDefinitions
信息: Loading XML bean definitions from class path resource [LifeBeans.xml]
Constructor of LifeBean()
l1 and l2 reference the different address of LifeBean
五月 12, 2018 8:09:32 下午 org.springframework.context.support.AbstractApplicationContext doClose
信息: Closing org.springframework.context.support.ClassPathXmlApplicationContext@670b40af: startup date [Sat May 12 20:09:32 CST 2018]; root of context hierarchy
五月 12, 2018 8:09:32 下午 org.springframework.context.support.AbstractApplicationContext doClose
信息: Closing org.springframework.context.support.ClassPathXmlApplicationContext@6956de9: startup date [Sat May 12 20:09:31 CST 2018]; root of context hierarchy
```





