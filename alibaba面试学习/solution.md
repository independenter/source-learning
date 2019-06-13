# 说出你常见的搜索算法
顺序搜索、二分搜索、斐波那契数列搜索、插值搜索、二叉树、2~3树、红黑树、b树、b+树
# 说出常见的推荐算法
基于流行度的算法、协同过滤算法、基于内容的算法、基于模型的算法、混合算法
# 说说对于BS和CS的认知
- CS即Client/Server(客户机/服务器)结构。C/S结构在技术上很成熟，它的主要特点是交互性强、具有安全的存取模式、网络通信量低、响应速度快、利于处理大量数据。
但是该结构的程序是针对性开发，变更不够灵活，维护和管理的难度较大。通常只局限于小型局域网，不利于扩展。并且，由于该结构的每台客户机都需要安装相应的客户端
程序，分布功能弱且兼容性差，不能实现快速部署安装和配置，因此缺少通用性，具有较大的局限性。要求具有一定专业水准的技术人员去完成。
- BS即Browser/Server(浏览器/服务器)结构，就是只安装维护一个服务器(Server)，而客户端采用浏览器(Browse)运行软件。B/S结构应用程序相对于传统的C/S结构应用
程序是一个非常大的进步。B/S结构的主要特点是分布性强、维护方便、开发简单且共享性强、总体拥有成本低。但数据安全性问题、对服务器要求过高、数据传输速度慢、软
件的个性化特点明显降低，这些缺点是有目共睹的，难以实现传统模式下的特殊功能要求。例如：通过浏览器进行大量的数据输入或进行报表的应答、专用性打印输出都比较困
难和不便。此外，实现复杂的应用构造有较大的困难。
# 排查线上问题
1. 锁定问题种类
主观原因：系统问题（是否可以重现场景）
   - 系统数据沉淀异常
   - 系统流程异常
   - 系统网络中断无响应（服务挂了、主机挂了、死锁）
客观原因：操作问题
工具：linux命令、java命令、python工具、jmeter压测工具、soupui接口调试工具、postman调测、Arthas监测系统
2. 产品demo
分析需求： 功能要求、后期扩展可能性、是否要给外部调用、是否涉及并发
最小功能产品MVP(minimum viable product,最小化可行产品)：性能测试
扩展产品

# 关于java基础知识
java对象的内存结构、java加载方式、JVM参数、类编译流程、三大特性、23设计模式、5大基本原则、java值传递和引用传递、
自动拆装箱、Integer缓存机制、java中1.6和1.7中substring的区别、switch对于string的支持、ArrayList和LinkedList
Vector的区别、SynchronizedList和Vector的区别、（HashMap、HashTable、ConcurrentHashMap区别）、Set和List区别？
、Set如何保证元素不重复、Arrays.asList获得的List使用时需要注意什么、fail-fast 和 fail-safe、CopyOnWriteArrayList
、ConcurrentSkipListMap、枚举的用法、多线程等、GC、synchronized是如何实现的？、synchronized和lock之间关系、
不使用synchronized如何实现一个线程安全的单例、synchronized和原子性、可见性和有序性之间的关系


# Spring/SpringMVC/SpringBoot原理
- 理解AOP、IOC等基本原理
```
1. IOC依赖翻转，通过配置文件告诉工厂如何实例化对象
注入方式：属性注入需要有set方法(集合注入)，构造器注入，静态方法，实例方法
interface进行接口多继承
/WEB-INF/applicationContext.xml
org.springframework.web.context.support.XmlWebApplicationContext
org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(java.lang.String, org.springframework.beans.factory.support.RootBeanDefinition, java.lang.Object[])
org.springframework.beans.factory.support.SimpleInstantiationStrategy.instantiate(org.springframework.beans.factory.support.RootBeanDefinition, java.lang.String, org.springframework.beans.factory.BeanFactory)
2.启动方式
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/classes/applicationContext-*.xml</param-value>
</context-param>
<listener> 
    <listener-class>org.springframework.web.context.ContextLoaderListener
 </listener-class>
 </listener>
 
<servlet> 
    <servlet-name>context</servlet-name> 
    <servlet-class>org.springframework.web.context.ContextLoaderServlet</servlet-class>
    <load-on-startup>1</load-on-startup>
</servlet> 
3. AOP面向切面，通过动态代理实现，可以通过3种动态代理
META-INF/spring.handlers 获得命名空间和句柄类 
org.springframework.beans.factory.xml.DefaultNamespaceHandlerResolver
加载资源 org.springframework.core.io.support.PropertiesLoaderUtils.loadAllProperties(java.lang.String, java.lang.ClassLoader)
<-- org.springframework.beans.factory.xml.PluggableSchemaResolver.getSchemaMappings
<-- org.springframework.beans.factory.xml.PluggableSchemaResolver.resolveEntity
<-- org.springframework.beans.factory.xml.DelegatingEntityResolver.resolveEntity
<-- org.springframework.beans.factory.xml.ResourceEntityResolver.resolveEntity
<-- org.springframework.beans.factory.xml.DefaultDocumentLoader.loadDocument
<-- org.springframework.beans.factory.xml.XmlBeanDefinitionReader.doLoadDocument
<-- org.springframework.beans.factory.xml.XmlBeanDefinitionReader.doLoadBeanDefinitions
<-- org.springframework.beans.factory.xml.XmlBeanDefinitionReader.loadBeanDefinitions(org.springframework.core.io.support.EncodedResource)
<-- org.springframework.beans.factory.xml.XmlBeanDefinitionReader.loadBeanDefinitions(org.springframework.core.io.Resource)
<-- org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(org.springframework.core.io.Resource...)
<-- org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(java.lang.String, java.util.Set<org.springframework.core.io.Resource>)
<-- org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(java.lang.String)
<-- org.springframework.context.support.AbstractXmlApplicationContext.loadBeanDefinitions(org.springframework.beans.factory.xml.XmlBeanDefinitionReader)
<-- org.springframework.context.support.AbstractRefreshableApplicationContext.refreshBeanFactory
<-- org.springframework.context.support.AbstractApplicationContext.obtainFreshBeanFactory
<-- org.springframework.context.support.AbstractApplicationContext.refresh

registerBeanDefinitionParser("config", new ConfigBeanDefinitionParser());
registerBeanDefinitionParser("aspectj-autoproxy", new AspectJAutoProxyBeanDefinitionParser());
registerBeanDefinitionDecorator("scoped-proxy", new ScopedProxyBeanDefinitionDecorator());
registerBeanDefinitionParser("spring-configured", new SpringConfiguredBeanDefinitionParser());

org.springframework.beans.factory.parsing.ParseState存放实例如AOPEntry,
class org.springframework.aop.aspectj.autoproxy.AspectJAwareAdvisorAutoProxyCreator
org.springframework.aop.config.ConfigBeanDefinitionParser.parseAspect
<aop:before>、<aop:after>、<aop:after-returning>、<aop:after-throwing method="">、<aop:around method="">

before对应AspectJMethodBeforeAdvice
After对应AspectJAfterAdvice
after-returning对应AspectJAfterReturningAdvice
after-throwing对应AspectJAfterThrowingAdvice
around对应AspectJAroundAdvice

new RootBeanDefinition(AspectJPointcutAdvisor.class);
parserContext.getReaderContext().registerWithGeneratedName(advisorDefinition);

org.springframework.aop.interceptor.ExposeInvocationInterceptor.currentInvocation
org.springframework.aop.interceptor.ExposeInvocationInterceptor.invocation
<-- org.springframework.aop.aspectj.AbstractAspectJAdvice.getJoinPointMatch()
<-- org.springframework.aop.interceptor.ExposeInvocationInterceptor.currentInvocation
<-- org.aopalliance.intercept.MethodInterceptor
<-- org.aopalliance.intercept.Interceptor
- org.aopalliance.intercept.MethodInterceptor
- org.aopalliance.intercept.ConstructorInterceptor

org.springframework.aop.interceptor.ExposeInvocationInterceptor.invoke
<-- org.springframework.aop.framework.ReflectiveMethodInvocation.proceed
<-- org.springframework.aop.framework.CglibAopProxy.DynamicAdvisedInterceptor.intercept
<-- org.springframework.cglib.proxy.MethodInterceptor

org/springframework/aop/SpringProxy.java
org/springframework/aop/aspectj/annotation/AspectJProxyFactory.java
org.springframework.aop.support.AopUtils

org.springframework.aop.framework.CglibAopProxy
org.springframework.aop.framework.JdkDynamicAopProxy
核心切入方式 BeanPostProcessor
org.springframework.aop.config.internalAutoProxyCreator
org.springframework.aop.aspectj.annotation.AnnotationAwareAspectJAutoProxyCreator
AspectJExpressionPointcut: () execution(* com.aop.*.*(..))
spring中BeanNameAutoProxyCreator和AnnotationAwareAspectJAutoProxyCreator两种AOP代理方式使用总结

aop三种实现
1、匹配Bean的名称自动创建匹配到的Bean的代理，实现类BeanNameAutoProxyCreator
2、根据Bean中的AspectJ注解自动创建代理，实现类AnnotationAwareAspectJAutoProxyCreator
3、根据Advisor的匹配机制自动创建代理，会对容器中所有的Advisor进行扫描，自动将这些切面应用到匹配的Bean中
，实现类DefaultAdvisorAutoProxyCreator

4.
0 = {ConcurrentHashMap$MapEntry@1950} "http://www.springframework.org/schema/aop" -> "org.springframework.aop.config.AopNamespaceHandler"
1 = {ConcurrentHashMap$MapEntry@1951} "http://www.springframework.org/schema/oxm" -> "org.springframework.oxm.config.OxmNamespaceHandler"
2 = {ConcurrentHashMap$MapEntry@1952} "http://www.springframework.org/schema/websocket" -> "org.springframework.web.socket.config.WebSocketNamespaceHandler"
3 = {ConcurrentHashMap$MapEntry@1953} "http://www.springframework.org/schema/task" -> "org.springframework.scheduling.config.TaskNamespaceHandler"
4 = {ConcurrentHashMap$MapEntry@1954} "http://www.springframework.org/schema/lang" -> "org.springframework.scripting.config.LangNamespaceHandler"
5 = {ConcurrentHashMap$MapEntry@1955} "http://www.springframework.org/schema/c" -> "org.springframework.beans.factory.xml.SimpleConstructorNamespaceHandler"
6 = {ConcurrentHashMap$MapEntry@1956} "http://www.springframework.org/schema/jee" -> "org.springframework.ejb.config.JeeNamespaceHandler"
7 = {ConcurrentHashMap$MapEntry@1957} "http://www.springframework.org/schema/jms" -> "org.springframework.jms.config.JmsNamespaceHandler"
8 = {ConcurrentHashMap$MapEntry@1958} "http://www.springframework.org/schema/cache" -> "org.springframework.cache.config.CacheNamespaceHandler"
9 = {ConcurrentHashMap$MapEntry@1959} "http://www.springframework.org/schema/jdbc" -> "org.springframework.jdbc.config.JdbcNamespaceHandler"
10 = {ConcurrentHashMap$MapEntry@1960} "http://www.springframework.org/schema/p" -> "org.springframework.beans.factory.xml.SimplePropertyNamespaceHandler"
11 = {ConcurrentHashMap$MapEntry@1961} "http://www.springframework.org/schema/util" -> "org.springframework.beans.factory.xml.UtilNamespaceHandler"
12 = {ConcurrentHashMap$MapEntry@1962} "http://www.springframework.org/schema/tx" -> "org.springframework.transaction.config.TxNamespaceHandler"
13 = {ConcurrentHashMap$MapEntry@1963} "http://www.springframework.org/schema/context" -> "org.springframework.context.config.ContextNamespaceHandler"
14 = {ConcurrentHashMap$MapEntry@1964} "http://www.springframework.org/schema/mvc" -> "org.springframework.web.servlet.config.MvcNamespaceHandler"
```
- sprintMVC
```
```
- springboot原理
1. META-INF/spring.factories
org.springframework.core.io.support.SpringFactoriesLoader
2.
META-INF/spring-autoconfigure-metadata.properties
org.springframework.boot.autoconfigure.AutoConfigurationMetadataLoader
3.META-INF/additional-spring-configuration-metadata.json
  META-INF/spring-configuration-metadata.json
org.springframework.boot.configurationprocessor.MetadataStore

#熟练掌握数据库原理、索引优化、分库分表，熟悉Mybatis或Hibernate的使用 
```$xslt
- 原理
1. FROM子句组装来自不同数据源的数据
2. WHERE子句基于指定的条件对记录进行筛选
3. GROUP BY子句将数据划分为多个分组
4. 使用聚集函数进行计算
5. 使用HAVING子句筛选分组
6. 计算全部的表达式
7. 使用ORDER BY对结果集进行排序
- 索引 
B B+树
- 分库分表

- 

```

#有业界主流分布式中间件使用经历（如：dubbo、redies、rabbitmq等），并了解其原理和实现细节
```
- dubbo
# dubbo运作方式
如何解析dubbo标签
org.springframework.beans.factory.xml.DefaultBeanDefinitionDocumentReader.parseBeanDefinitions
0 = {ConcurrentHashMap$MapEntry@1672} "http://dubbo.apache.org/schema/dubbo" -> "com.alibaba.dubbo.config.spring.schema.DubboNamespaceHandler"
1 = {ConcurrentHashMap$MapEntry@1673} "http://code.alibabatech.com/schema/dubbo" -> "com.alibaba.dubbo.config.spring.schema.DubboNamespaceHandler"
2 = {ConcurrentHashMap$MapEntry@1674} "http://www.springframework.org/schema/aop" -> "org.springframework.aop.config.AopNamespaceHandler"
3 = {ConcurrentHashMap$MapEntry@1675} "http://www.springframework.org/schema/task" -> "org.springframework.scheduling.config.TaskNamespaceHandler"
4 = {ConcurrentHashMap$MapEntry@1676} "http://www.springframework.org/schema/lang" -> "org.springframework.scripting.config.LangNamespaceHandler"
5 = {ConcurrentHashMap$MapEntry@1677} "http://www.springframework.org/schema/c" -> "org.springframework.beans.factory.xml.SimpleConstructorNamespaceHandler"
6 = {ConcurrentHashMap$MapEntry@1678} "http://www.springframework.org/schema/jee" -> "org.springframework.ejb.config.JeeNamespaceHandler"
7 = {ConcurrentHashMap$MapEntry@1679} "http://www.springframework.org/schema/cache" -> "org.springframework.cache.config.CacheNamespaceHandler"
8 = {ConcurrentHashMap$MapEntry@1680} "http://www.springframework.org/schema/p" -> "org.springframework.beans.factory.xml.SimplePropertyNamespaceHandler"
9 = {ConcurrentHashMap$MapEntry@1681} "http://www.springframework.org/schema/util" -> "org.springframework.beans.factory.xml.UtilNamespaceHandler"
10 = {ConcurrentHashMap$MapEntry@1682} "http://www.springframework.org/schema/context" -> "org.springframework.context.config.ContextNamespaceHandler"

org.springframework.beans.BeanInfoFactory=org.springframework.beans.ExtendedBeanInfoFactory
com.alibaba.dubbo.common.extension.ExtensionLoader

com.alibaba.dubbo.config.spring.ServiceBean.afterPropertiesSet
<-- com.alibaba.dubbo.config.ApplicationConfig

dubbo的spi
C:\Users\Administrator\.m2\repository\com\alibaba\dubbo\2.6.6\dubbo-2.6.6.jar!\META-INF\dubbo\internal

org.springframework.beans.BeanWrapper包装类，通过内部object属性，包装配置类
com.alibaba.dubbo.config.spring.extension.SpringExtensionFactory
代理类
com.alibaba.dubbo.rpc.ProxyFactory
<-- org.apache.dubbo.config.ReferenceConfig.createProxy

org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.applyBeanPostProcessorsAfterInitialization

resolveFile = System.getProperty("dubbo.resolve.file");
if (resolveFile == null || resolveFile.length() == 0) {
    File userResolveFile = new File(new File(System.getProperty("user.home")), "dubbo-resolve.properties");
    if (userResolveFile.exists()) {
        resolveFile = userResolveFile.getAbsolutePath();
    }
}
127.0.0.1:20880/com.dobb.demo.DemoService?application=demo-consumer&check=false
&dubbo=2.0.2&interface=com.dobb.demo.DemoService&methods=sayHello&pid=5232
&register.ip=192.168.232.1&side=consumer&timestamp=1560228921288

com.alibaba.dubbo.rpc.RpcInvocation.RpcInvocation(java.lang.reflect.Method, java.lang.Object[])

com.alibaba.dubbo.rpc.support.RpcUtils.attachInvocationIdIfAsync
# redies
- 内存淘汰策略
 # maxmemory-policy volatile-lru
 noeviction：当内存使用达到阈值的时候，所有引起申请内存的命令会报错。
 allkeys-lru：在主键空间中，优先移除最近未使用的key。(推荐)
 volatile-lru：在设置了过期时间的键空间中，优先移除最近未使用的key。
 allkeys-random：在主键空间中，随机移除某个key。
 volatile-random：在设置了过期时间的键空间中，随机移除某个key。
 volatile-ttl：在设置了过期时间的键空间中，具有更早过期时间的key优先移除。
- 1 缓存穿透 2 缓存雪崩 3 缓存击穿
# rabbitmq

```

#具备较强的问题分析与开发调试能力，精通java技术栈应用异常问题排查
定位进程及线程，arthas监控转发器、过滤器、拦截器等请求，监听方法的入参和出参
解析语法糖

#具备客户服务意识与ownership、团队沟通协作能力以及自我驱动能力，有很好的技术敏感度和风险识别能力

#有全栈经验，了解React、AntD或者Android、Ios等前端技术的优先
```
蚂蚁金服底层框架umi，内部使用React
routes与嵌套routes
.
├── dist/                          // 默认的 build 输出目录
├── mock/                          // mock 文件所在目录，基于 express
├── config/
    ├── config.js                  // umi 配置，同 .umirc.js，二选一
└── src/                           // 源码目录，可选
    ├── layouts/index.js           // 全局布局
    ├── pages/                     // 页面目录，里面的文件即路由
        ├── .umi/                  // dev 临时目录，需添加到 .gitignore
        ├── .umi-production/       // build 临时目录，会自动删除
        ├── document.ejs           // HTML 模板
        ├── 404.js                 // 404 页面
        ├── page1.js               // 页面 1，任意命名，导出 react 组件
        ├── page1.test.js          // 用例文件，umi test 会匹配所有 .test.js 和 .e2e.js 结尾的文件
        └── page2.js               // 页面 2，任意命名
    ├── global.css                 // 约定的全局样式文件，自动引入，也可以用 global.less
    ├── global.js                  // 可以在这里加入 polyfill
    ├── app.js                     // 运行时配置文件
├── .umirc.js                      // umi 配置，同 config/config.js，二选一
├── .env                           // 环境变量
└── package.json
```
