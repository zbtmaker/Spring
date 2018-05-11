# Spring
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
