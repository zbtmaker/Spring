# Bean的生命周期
## <1>JavaBean类
```Java
package chapter1.beanlifecycle;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.BeanFactory;
import org.springframework.beans.factory.BeanFactoryAware;
import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.InitializingBean;
import org.springframework.beans.factory.config.BeanPostProcessor;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;

public class Person implements BeanFactoryAware,ApplicationContextAware,
								InitializingBean,BeanPostProcessor,DisposableBean{
	private String name;
	private String phone;
	
	private BeanFactory beanFactory;
	private String beanName;
	private ApplicationContext applicationContext;
	/**
	 * default constructor
	 */
	public Person() {
		
	}
	
	public String getName() {
		return name;
	}


	public void setName(String name) {
		this.name = name;
	}


	public String getPhone() {
		return phone;
	}


	public void setPhone(String phone) {
		this.phone = phone;
	}


	@Override
	public void setBeanFactory(BeanFactory arg0) throws BeansException {
		System.out.println("Spring call the setBeanFactory to transfer the Instance of BeanFactory");
		this.beanFactory = arg0;
	}


	@Override
	public void setApplicationContext(ApplicationContext arg0) throws BeansException {
		System.out.println("Spring call the setApplicationContext to transfer the ApplicationContext");
		this.applicationContext = arg0;
	}


	@Override
	public void afterPropertiesSet() throws Exception {
		System.out.println("Spring call the afterPropertiesSet()");
	}


	@Override
	public void destroy() throws Exception {
		System.out.println("Spring call the destory() to call the destory-method of Person bean");
	}
	
	public void selfDefineDestory() {
		System.out.println("Spring call the destory-method by self definition");
	}
	
	public void selfDefineInit() {
		System.out.println("Spring call the initialize-method by self definition");
	}
}
```
### 配置文件
```Java
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    xmlns:p="http://www.springframework.org/schema/p"
    xmlns:aop="http://www.springframework.org/schema/aop" 
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="
            http://www.springframework.org/schema/beans 
            http://www.springframework.org/schema/beans/spring-beans-3.2.xsd">
    
    <bean id="person" class="chapter1.beanlifecycle.Person"  scope="singleton" 
    p:name="Zhang San" p:phone="15900000000" 
    init-method="selfDefineInit" destroy-method="selfDefineDestory"/> 
</beans>
```
### 输出结果
```Java
信息: Loading XML bean definitions from class path resource [beanlifecycle.xml]
Spring call the setBeanFactory to transfer the Instance of BeanFactory
Spring call the setApplicationContext to transfer the ApplicationContext
Spring call the afterPropertiesSet()
Spring call the initialize-method by self definition
容器初始化成功
chapter1.beanlifecycle.Person@3ecd23d9
现在开始关闭容器！
五月 12, 2018 10:37:44 下午 org.springframework.context.support.AbstractApplicationContext doClose
信息: Closing org.springframework.context.support.ClassPathXmlApplicationContext@270421f5: startup date [Sat May 12 22:37:44 CST 2018]; root of context hierarchy
Spring call the destory() to call the destory-method of Person bean
Spring call the destory-method by self definition
```
参考文献
[1]http://www.cnblogs.com/zrtqsk/p/3735273.html
[2]<Spring in Action>>chapter1-bean的生命周期
