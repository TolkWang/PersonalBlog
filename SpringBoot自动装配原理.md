# SpringBoot自动装配原理

个人总结的springboot自动装配源码解析

从主启动类开始说起

```
@SpringBootApplication
@EnableScheduling
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

------

## 流程回忆

@SpringBootApplication 启动类注解

@EnableAutoConfiguration 自动配置注解

@Import({AutoConfigurationImportSelector.class}) 自动导入配置文件的选择器

getCondidateConfigurations() 获取所有候选配置

通过spring.factories获取配置类的位置

获取配置类，在上层方法中循环封装为properties供我们使用

当然，并不是所有的配置类都会启动，@conditionalOnXXX 如果其中的条件都满足，该类才会生效

## 从源码流程分析

SpringApplication.run(Application.class, args);

Application.class这个参数流程,从当前堆栈中推测出main方法

this.primarySources = new LinkedHashset<>(ArrayList())

deduceMainApplicatinClass()

new RunTimeException().getStackTrace() 

------

run 方法分析

1.设置相关属性

开始时间，上下文对象，异常报告器，环境对象

2.prepareContext

给应用程序上下文设置具体属性值，初始化参数，创建对象工厂

getAllSources() ,Load beans into the application context

isComponent(sources) 判断主类的继承关系中，是否有component注解

3.refreshContext(context) 这里就耦合上了spring的refresh方法，对于spring的源码，可以查阅之前的博客内容

这里重点描述一下invokeBeanFactoryPostProcessors()这个方法

beanFactory.getBeanNameForTypes()

interralConfigurationAnnotationPeocessor 一个标记

BeanFactoryPostProcessor执行

ConfigurationClassPostProcessor 核心处理类

ProcessConfigBeanDefinitions (依赖于@Configuration注解)

parse() 解析

ProcessImports ----- getImports() 取出imports注解中的2个类进行解析

this.deferredImportSelectorHander.Process()

AutoConfigurationEntry

getCandidateConfigurations() 获取并加载
