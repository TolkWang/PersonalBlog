# Spring源码分析日记

主要就是简述一下spring中的这个refresh()方法拉，防止自己忘记了，个人理解，可能有些出入，学艺不精，并没有debug完所有源码，嘿嘿

1.prepareRefresh

设置标志位，创建了2个集合，并且做了一些前期的准备工作

2.obtainFreshBeanFactory

创建bean工厂，获取bean的定义信息

3.preparBeanFactory

设置属性值

4.postprocessBeanFactory

空方法，用于扩展

5.invokeBeanFactoryPostProcessors(beanFactory)

非常重要的方法，实例化并执行所有的后置处理类

6.registerBeanPostProcessors

实例化并注册所有的BeanPostProcessor beans

7.initMessageSource

国际化相关

8.initApplicationEventMulticaster

初始化事件监听器

9.onFresh

空方法，据我了解，spring boot的tomcat就是在这里实现的

10.registerListeners

注册监听器

11.finishBeanFactoryInitialization(beanfactory)

实例化所有的非懒加载的bean对象

##### FactoryBean的三个方法

isSingleton

getObject

getObjectType

##### Spring Bean的生命周期

1 spring容器跟据配置中的bean定义实例化bean

2 spring使用依赖注入填充所有属性，如bean中所定义的配置

3 如果bean实现了BeanNameAware接口，则工厂通过传递ID来调用setBeanName()

4 如果bean实现了BeanFactoryAware接口，则工厂通过传递自身的实例来调用setBeanFactory()

5 如果存在于Bean关联的任何BeanPostProcessors,则调用preProcessBeforeInitization方法

6 如果Bean指定了init方法的(init-Method)属性，将调用它

7 如果存在于bean关联的任何BeanPostProcessors.则调用postProcessAfterInitization()方法

8 如果bean实现了DisposeableBean接口，当spring容器关闭时，调用destory()

9 如果bean指定了destory方法(destory-method),将调用它

