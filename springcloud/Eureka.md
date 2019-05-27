# Eureka Learn
Eureka Learn 记录了对于Eureka的相关学习总结
# [源码地址](https://github.com/Netflix/eureka.git)
# [学习地址](https://github.com/independenter/eureka.git)

:100:鸣谢
- 感谢学习本笔记的每一个观众
- 感谢修改本笔记的每一个贡献者

## 目录
- [官方定义](#Eureka)
- [学习版本](#学习版本)
- [依赖配置](#依赖配置)
- [概念阐述](#概念阐述) 
- [设计模式](#设计模式) 
- [自我理解](#自我理解)

## 学习版本
| 模块 | 版本 |
| --- | --- |
| spring-boot | 2.1.5.RELEASE |
| java.version | 1.8 |
| spring-cloud.version | Greenwich.SR1 |

## 依赖配置
### server
```
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```
### client
```
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```
## 概念阐述
- Register：服务注册
当Eureka客户端向Eureka Server注册时，它提供自身的元数据，比如IP地址、端口，运行状况指示符URL，主页等。

- Renew：服务续约
Eureka客户会每隔30秒发送一次心跳来续约。 通过续约来告知Eureka Server该Eureka客户仍然存在，没有出现问题。 正常情况下，如果Eureka Server在90秒没有收到Eureka客户的续约，它会将实例从其注册表中删除。 建议不要更改续约间隔。

- Fetch Registries：获取注册列表信息
Eureka客户端从服务器获取注册表信息，并将其缓存在本地。客户端会使用该信息查找其他服务，从而进行远程调用。该注册列表信息定期（每30秒钟）更新一次。每次返回注册列表信息可能与Eureka客户端的缓存信息不同， Eureka客户端自动处理。如果由于某种原因导致注册列表信息不能及时匹配，Eureka客户端则会重新获取整个注册表信息。 Eureka服务器缓存注册列表信息，整个注册表以及每个应用程序的信息进行了压缩，压缩内容和没有压缩的内容完全相同。Eureka客户端和Eureka 服务器可以使用JSON / XML格式进行通讯。在默认的情况下Eureka客户端使用压缩JSON格式来获取注册列表的信息。

- Cancel：服务下线
Eureka客户端在程序关闭时向Eureka服务器发送取消请求。 发送请求后，该客户端实例信息将从服务器的实例注册表中删除。该下线请求不会自动完成，它需要调用以下内容：
DiscoveryManager.getInstance().shutdownComponent()；

- Eviction 服务剔除
在默认的情况下，当Eureka客户端连续90秒没有向Eureka服务器发送服务续约，即心跳，Eureka服务器会将该服务实例从服务注册列表删除，即服务剔除。

## 行为分析
### 日志
- [启动日志](https://github.com/independenter/source-learning/blob/master/springcloud/Eukeka.log)
### 涉及类名及行为
- org.springframework.cloud.context.scope.GenericScope
```
Generating bean factory id from names: [default.org.springframework.cloud.netflix.ribbon.eureka.RibbonEurekaAutoConfiguration.RibbonClientSpecification,eureka.dashboard-org.springframework.cloud.netflix.eureka.server.EurekaDashboardProperties, eureka.instance.registry-org.springframework.cloud.netflix.eureka.server.InstanceRegistryProperties, eurekaApplication, eurekaApplicationInfoManager, eurekaAutoServiceRegistration, eurekaClient, eurekaClientConfigBean, eurekaController, eurekaDiscoverClientMarker, eurekaFeature, eurekaHealthIndicator, eurekaInstanceConfigBean, eurekaRegistration, eurekaServerBootstrap, eurekaServerConfig, eurekaServerContext, eurekaServerFeature, eurekaServerMarkerBean, eurekaServiceRegistry,org.springframework.cloud.netflix.eureka.EurekaClientAutoConfiguration, org.springframework.cloud.netflix.eureka.EurekaClientAutoConfiguration$EurekaHealthIndicatorConfiguration, org.springframework.cloud.netflix.eureka.EurekaClientAutoConfiguration$RefreshableEurekaClientConfiguration, org.springframework.cloud.netflix.eureka.EurekaDiscoveryClientConfiguration, org.springframework.cloud.netflix.eureka.EurekaDiscoveryClientConfiguration$EurekaClientConfigurationRefresher, org.springframework.cloud.netflix.eureka.config.DiscoveryClientOptionalArgsConfiguration, org.springframework.cloud.netflix.eureka.server.EurekaServerAutoConfiguration, org.springframework.cloud.netflix.eureka.server.EurekaServerAutoConfiguration$EurekaServerConfigBeanConfiguration, org.springframework.cloud.netflix.eureka.server.EurekaServerInitializerConfiguration, org.springframework.cloud.netflix.eureka.server.EurekaServerMarkerConfiguration,org.springframework.cloud.netflix.ribbon.eureka.RibbonEurekaAutoConfiguration,peerEurekaNodes,scopedTarget.eurekaApplicationInfoManager, scopedTarget.eurekaClient, scopedTarget.eurekaRegistration,]
```
- org.springframework.beans.factory.support.DefaultListableBeanFactory
```
2019-05-27 23:22:30.518 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'eurekaHealthIndicator'
2019-05-27 23:22:30.519 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'org.springframework.cloud.netflix.eureka.EurekaClientAutoConfiguration$EurekaHealthIndicatorConfiguration'
2019-05-27 23:22:30.521 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'eurekaInstanceConfigBean'
2019-05-27 23:22:30.533 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'eurekaInstanceConfigBean' via factory method to bean named 'inetUtils'
2019-05-27 23:22:30.533 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'eurekaInstanceConfigBean' via factory method to bean named 'serviceManagementMetadataProvider'
2019-05-27 23:22:31.015 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'eurekaHealthIndicator' via factory method to bean named 'eurekaClient'
2019-05-27 23:22:31.015 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'eurekaHealthIndicator' via factory method to bean named 'eurekaInstanceConfigBean'
2019-05-27 23:22:31.015 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'eurekaHealthIndicator' via factory method to bean named 'eurekaClientConfigBean'
2019-05-27 23:22:31.026 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'discoveryCompositeHealthIndicator' via factory method to bean named 'eurekaHealthIndicator'
2019-05-27 23:22:31.162 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'eurekaFeature'
2019-05-27 23:22:31.170 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'default.org.springframework.cloud.netflix.ribbon.eureka.RibbonEurekaAutoConfiguration.RibbonClientSpecification'
2019-05-27 23:22:31.176 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'eurekaServerFeature'
2019-05-27 23:22:31.184 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'eurekaRegistration'
2019-05-27 23:22:31.195 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'eurekaServiceRegistry'
2019-05-27 23:22:31.196 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'serviceRegistryEndpoint' via factory method to bean named 'eurekaServiceRegistry'
2019-05-27 23:22:31.986 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'eurekaApplication'
2019-05-27 23:22:31.987 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'org.springframework.cloud.netflix.eureka.server.EurekaServerMarkerConfiguration'
2019-05-27 23:22:31.987 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'eurekaServerMarkerBean'
2019-05-27 23:22:33.782 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'org.springframework.cloud.netflix.eureka.EurekaDiscoveryClientConfiguration$EurekaClientConfigurationRefresher'
2019-05-27 23:22:33.786 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'eurekaAutoServiceRegistration'
2019-05-27 23:22:33.789 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'eurekaAutoServiceRegistration' via factory method to bean named 'org.springframework.boot.web.servlet.context.AnnotationConfigServletWebServerApplicationContext@12796d3'
2019-05-27 23:22:33.790 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'eurekaAutoServiceRegistration' via factory method to bean named 'eurekaServiceRegistry'
2019-05-27 23:22:33.790 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'eurekaAutoServiceRegistration' via factory method to bean named 'eurekaRegistration'
2019-05-27 23:22:33.791 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'org.springframework.cloud.netflix.eureka.EurekaDiscoveryClientConfiguration'
2019-05-27 23:22:33.792 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'eurekaDiscoverClientMarker'
2019-05-27 23:22:33.800 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'org.springframework.cloud.netflix.eureka.EurekaClientAutoConfiguration$RefreshableEurekaClientConfiguration'
2019-05-27 23:22:33.803 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'discoveryClientOptionalArgs'
2019-05-27 23:22:33.803 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'org.springframework.cloud.netflix.eureka.config.DiscoveryClientOptionalArgsConfiguration'
2019-05-27 23:22:33.810 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'discoveryClient'
2019-05-27 23:22:33.812 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'discoveryClient' via factory method to bean named 'eurekaClient'
2019-05-27 23:22:33.812 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'discoveryClient' via factory method to bean named 'eurekaClientConfigBean'
2019-05-27 23:22:36.553 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'org.springframework.cloud.netflix.eureka.server.EurekaServerInitializerConfiguration'
2019-05-27 23:22:36.556 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'eurekaServerBootstrap'
2019-05-27 23:22:36.584 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'scopedTarget.eurekaClient' via factory method to bean named 'eurekaApplicationInfoManager'
2019-05-27 23:22:36.584 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'scopedTarget.eurekaClient' via factory method to bean named 'eurekaClientConfigBean'
2019-05-27 23:22:36.585 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'scopedTarget.eurekaClient' via factory method to bean named 'eurekaInstanceConfigBean'
2019-05-27 23:22:36.588 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'scopedTarget.eurekaApplicationInfoManager' via factory method to bean named 'eurekaInstanceConfigBean'
2019-05-27 23:22:36.757 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'eurekaServerContext'
2019-05-27 23:22:36.759 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'peerEurekaNodes'
2019-05-27 23:22:36.759 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'peerEurekaNodes' via factory method to bean named 'peerAwareInstanceRegistry'
2019-05-27 23:22:36.760 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'peerEurekaNodes' via factory method to bean named 'serverCodecs'
2019-05-27 23:22:36.766 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'eurekaServerContext' via factory method to bean named 'serverCodecs'
2019-05-27 23:22:36.767 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'eurekaServerContext' via factory method to bean named 'peerAwareInstanceRegistry'
2019-05-27 23:22:36.767 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'eurekaServerContext' via factory method to bean named 'peerEurekaNodes'
2019-05-27 23:22:38.064 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'eurekaServerBootstrap' via factory method to bean named 'peerAwareInstanceRegistry'
2019-05-27 23:22:38.064 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'eurekaServerBootstrap' via factory method to bean named 'eurekaServerContext'
2019-05-27 23:22:38.066 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'eurekaController'
2019-05-27 23:22:38.069 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'eureka.dashboard-org.springframework.cloud.netflix.eureka.server.EurekaDashboardProperties'
2019-05-27 23:22:38.070 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Creating shared instance of singleton bean 'org.springframework.cloud.netflix.ribbon.eureka.RibbonEurekaAutoConfiguration'
2019-05-27 23:22:38.293 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'scopedTarget.eurekaRegistration' via factory method to bean named 'eurekaClient'
2019-05-27 23:22:38.293 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'scopedTarget.eurekaRegistration' via factory method to bean named 'eurekaInstanceConfigBean'
2019-05-27 23:22:38.293 [main] DEBUG org.springframework.beans.factory.support.DefaultListableBeanFactory - Autowiring by type from bean name 'scopedTarget.eurekaRegistration' via factory method to bean named 'eurekaApplicationInfoManager'
```
- org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider
```
2019-05-27 23:22:30.228 [main] DEBUG org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider - Identified candidate component class: URL [jar:file:/C:/Users/Administrator/.m2/repository/com/netflix/eureka/eureka-client/1.9.8/eureka-client-1.9.8.jar!/com/netflix/discovery/provider/DiscoveryJerseyProvider.class]
2019-05-27 23:22:30.339 [main] DEBUG org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider - Identified candidate component class: URL [jar:file:/C:/Users/Administrator/.m2/repository/com/netflix/eureka/eureka-core/1.9.8/eureka-core-1.9.8.jar!/com/netflix/eureka/resources/ASGResource.class]
2019-05-27 23:22:30.343 [main] DEBUG org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider - Identified candidate component class: URL [jar:file:/C:/Users/Administrator/.m2/repository/com/netflix/eureka/eureka-core/1.9.8/eureka-core-1.9.8.jar!/com/netflix/eureka/resources/ApplicationsResource.class]
2019-05-27 23:22:30.345 [main] DEBUG org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider - Identified candidate component class: URL [jar:file:/C:/Users/Administrator/.m2/repository/com/netflix/eureka/eureka-core/1.9.8/eureka-core-1.9.8.jar!/com/netflix/eureka/resources/InstancesResource.class]
2019-05-27 23:22:30.346 [main] DEBUG org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider - Identified candidate component class: URL [jar:file:/C:/Users/Administrator/.m2/repository/com/netflix/eureka/eureka-core/1.9.8/eureka-core-1.9.8.jar!/com/netflix/eureka/resources/PeerReplicationResource.class]
2019-05-27 23:22:30.347 [main] DEBUG org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider - Identified candidate component class: URL [jar:file:/C:/Users/Administrator/.m2/repository/com/netflix/eureka/eureka-core/1.9.8/eureka-core-1.9.8.jar!/com/netflix/eureka/resources/SecureVIPResource.class]
2019-05-27 23:22:30.347 [main] DEBUG org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider - Identified candidate component class: URL [jar:file:/C:/Users/Administrator/.m2/repository/com/netflix/eureka/eureka-core/1.9.8/eureka-core-1.9.8.jar!/com/netflix/eureka/resources/ServerInfoResource.class]
2019-05-27 23:22:30.348 [main] DEBUG org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider - Identified candidate component class: URL [jar:file:/C:/Users/Administrator/.m2/repository/com/netflix/eureka/eureka-core/1.9.8/eureka-core-1.9.8.jar!/com/netflix/eureka/resources/StatusResource.class]
2019-05-27 23:22:30.348 [main] DEBUG org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider - Identified candidate component class: URL [jar:file:/C:/Users/Administrator/.m2/repository/com/netflix/eureka/eureka-core/1.9.8/eureka-core-1.9.8.jar!/com/netflix/eureka/resources/VIPResource.class]
```
- org.springframework.core.env.PropertySourcesPropertyResolver
```
2019-05-27 23:22:30.533 [main] DEBUG org.springframework.core.env.PropertySourcesPropertyResolver - Found key 'eureka.instance.hostname' in PropertySource 'configurationProperties' with value of type String
2019-05-27 23:22:38.398 [Thread-11] DEBUG org.springframework.core.env.PropertySourcesPropertyResolver - Found key 'eureka.environment' in PropertySource 'configurationProperties' with value of type String
```
- org.springframework.cloud.netflix.eureka.metadata.DefaultManagementMetadataProvider
```
2019-05-27 23:22:31.000 [main] DEBUG org.springframework.cloud.netflix.eureka.metadata.DefaultManagementMetadataProvider - Constructed eureka meta-data healthcheckUrl: http://peer1:8000/actuator/health
2019-05-27 23:22:31.001 [main] DEBUG org.springframework.cloud.netflix.eureka.metadata.DefaultManagementMetadataProvider - Constructed eureka meta-data statusPageUrl: http://peer1:8000/actuator/info
```
- org.springframework.boot.web.servlet.ServletContextInitializerBeans
```
Mapping filters: filterRegistrationBean urls=[/*], filterRegistrationBean urls=[/*], filterRegistrationBean urls=[/eureka/*], characterEncodingFilter urls=[/*], hiddenHttpMethodFilter urls=[/*], formContentFilter urls=[/*], requestContextFilter urls=[/*]
```
- org.springframework.cloud.netflix.eureka.InstanceInfoFactory
```
2019-05-27 23:22:36.594 [main] INFO  org.springframework.cloud.netflix.eureka.InstanceInfoFactory - Setting initial instance status as: STARTING
```
- com.netflix.discovery.DiscoveryClient 
```
2019-05-27 23:22:36.666 [main] INFO  com.netflix.discovery.DiscoveryClient - Initializing Eureka in region us-east-1
```
- com.netflix.eureka.DefaultEurekaServerContext
```
2019-05-27 23:22:36.769 [main] INFO  com.netflix.eureka.DefaultEurekaServerContext - Initializing ...
```
- com.netflix.eureka.cluster.PeerEurekaNodes
```
2019-05-27 23:22:36.773 [main] INFO  com.netflix.eureka.cluster.PeerEurekaNodes - Adding new peer nodes [http://peer2:7000/eureka/]
2019-05-27 23:22:38.046 [main] INFO  com.netflix.eureka.cluster.PeerEurekaNodes - Replica node URL:  http://peer2:7000/eureka/
```
- com.netflix.discovery.shared.transport.jersey.AbstractJerseyEurekaHttpClient
```
2019-05-27 23:22:37.948 [main] DEBUG com.netflix.discovery.shared.transport.jersey.AbstractJerseyEurekaHttpClient - Created client for url: http://peer2:7000/eureka/
```
- com.netflix.eureka.registry.AbstractInstanceRegistry
```
2019-05-27 23:22:38.062 [main] INFO  com.netflix.eureka.registry.AbstractInstanceRegistry - Finished initializing remote region registries. All known remote regions: []
2019-05-27 23:22:48.656 [Eureka-EvictionTimer] INFO  com.netflix.eureka.registry.AbstractInstanceRegistry - Running the evict task with compensationTime 0ms
2019-05-27 23:22:48.656 [Eureka-EvictionTimer] DEBUG com.netflix.eureka.registry.AbstractInstanceRegistry - Running the evict task
2019-05-27 23:22:58.656 [Eureka-EvictionTimer] INFO  com.netflix.eureka.registry.AbstractInstanceRegistry - Running the evict task with compensationTime 0ms
2019-05-27 23:22:58.656 [Eureka-EvictionTimer] DEBUG com.netflix.eureka.registry.AbstractInstanceRegistry - Running the evict task
```
- com.netflix.eureka.DefaultEurekaServerContext
```
2019-05-27 23:22:38.063 [main] INFO  com.netflix.eureka.DefaultEurekaServerContext - Initialized
```
- org.springframework.cloud.netflix.eureka.serviceregistry.EurekaServiceRegistry
```
2019-05-27 23:22:38.296 [main] INFO  org.springframework.cloud.netflix.eureka.serviceregistry.EurekaServiceRegistry - Registering application EUREKA-SERVER with eureka with status UP
```
- org.springframework.context.support.DefaultLifecycleProcessor
```
2019-05-27 23:22:38.300 [main] DEBUG org.springframework.context.support.DefaultLifecycleProcessor - Successfully started bean 'eurekaAutoServiceRegistration'
2019-05-27 23:22:38.307 [main] DEBUG org.springframework.context.support.DefaultLifecycleProcessor - Successfully started bean 'org.springframework.cloud.netflix.eureka.server.EurekaServerInitializerConfiguration'
```
- org.springframework.cloud.netflix.eureka.server.EurekaServerBootstrap
```
2019-05-27 23:22:38.386 [Thread-11] INFO  org.springframework.cloud.netflix.eureka.server.EurekaServerBootstrap - Setting the eureka configuration..
2019-05-27 23:22:38.397 [Thread-11] INFO  org.springframework.cloud.netflix.eureka.server.EurekaServerBootstrap - Eureka data center value eureka.datacenter is not set, defaulting to default
2019-05-27 23:22:38.653 [Thread-11] INFO  org.springframework.cloud.netflix.eureka.server.EurekaServerBootstrap - isAws returned false
2019-05-27 23:22:38.654 [Thread-11] INFO  org.springframework.cloud.netflix.eureka.server.EurekaServerBootstrap - Initialized server context
```
- org.springframework.cloud.netflix.eureka.serviceregistry.EurekaAutoServiceRegistration
```
2019-05-27 23:22:38.588 [main] INFO  org.springframework.cloud.netflix.eureka.serviceregistry.EurekaAutoServiceRegistration - Updating port to 8000
```
- com.netflix.eureka.registry.PeerAwareInstanceRegistryImpl
```
2019-05-27 23:22:38.654 [Thread-11] INFO  com.netflix.eureka.registry.PeerAwareInstanceRegistryImpl - Got 1 instances from neighboring DS node
2019-05-27 23:22:38.654 [Thread-11] INFO  com.netflix.eureka.registry.PeerAwareInstanceRegistryImpl - Renew threshold is: 1
2019-05-27 23:22:38.655 [Thread-11] INFO  com.netflix.eureka.registry.PeerAwareInstanceRegistryImpl - Changing status to UP
```
- org.springframework.cloud.netflix.eureka.server.EurekaServerInitializerConfiguration
```
2019-05-27 23:22:38.670 [Thread-11] INFO  org.springframework.cloud.netflix.eureka.server.EurekaServerInitializerConfiguration - Started Eureka Server
```
- com.netflix.discovery.shared.MonitoredConnectionManager
```
2019-05-27 23:23:07.946 [Eureka-JerseyClient-Conn-Cleaner2] DEBUG com.netflix.discovery.shared.MonitoredConnectionManager - Closing connections idle longer than 30 SECONDS
```
- com.netflix.discovery.shared.NamedConnectionPool
```
2019-05-27 23:23:07.948 [Eureka-JerseyClient-Conn-Cleaner2] DEBUG com.netflix.discovery.shared.NamedConnectionPool - Closing connections idle longer than 30 SECONDS
```
### spring cloud report
```
   CommonsClientAutoConfiguration.DiscoveryLoadBalancerConfiguration#discoveryCompositeHealthIndicator matched:
      - @ConditionalOnProperty (spring.cloud.discovery.client.composite-indicator.enabled) matched (OnPropertyCondition)
      - @ConditionalOnBean (types: org.springframework.cloud.client.discovery.health.DiscoveryHealthIndicator,org.springframework.boot.actuate.health.HealthAggregator; SearchStrategy: all) found beans 'discoveryClientHealthIndicator', 'eurekaHealthIndicator', 'healthAggregator' (OnBeanCondition)
   EurekaClientAutoConfiguration matched:
      - @ConditionalOnClass found required class 'com.netflix.discovery.EurekaClientConfig' (OnClassCondition)
      - @ConditionalOnProperty (eureka.client.enabled) matched; @ConditionalOnProperty (spring.cloud.discovery.enabled) matched (OnPropertyCondition)
      - @ConditionalOnBean (types: org.springframework.cloud.netflix.eureka.EurekaDiscoveryClientConfiguration$Marker; SearchStrategy: all) found bean 'eurekaDiscoverClientMarker' (OnBeanCondition)

   EurekaClientAutoConfiguration#eurekaAutoServiceRegistration matched:
      - @ConditionalOnProperty (spring.cloud.service-registry.auto-registration.enabled) matched (OnPropertyCondition)
      - @ConditionalOnBean (types: org.springframework.cloud.client.serviceregistry.AutoServiceRegistrationProperties; SearchStrategy: all) found bean 'spring.cloud.service-registry.auto-registration-org.springframework.cloud.client.serviceregistry.AutoServiceRegistrationProperties' (OnBeanCondition)

   EurekaClientAutoConfiguration#eurekaClientConfigBean matched:
      - @ConditionalOnMissingBean (types: com.netflix.discovery.EurekaClientConfig; SearchStrategy: current) did not find any beans (OnBeanCondition)

   EurekaClientAutoConfiguration#eurekaInstanceConfigBean matched:
      - @ConditionalOnMissingBean (types: com.netflix.appinfo.EurekaInstanceConfig; SearchStrategy: current) did not find any beans (OnBeanCondition)

   EurekaClientAutoConfiguration#serviceManagementMetadataProvider matched:
      - @ConditionalOnMissingBean (types: org.springframework.cloud.netflix.eureka.metadata.ManagementMetadataProvider; SearchStrategy: all) did not find any beans (OnBeanCondition)

   EurekaClientAutoConfiguration.EurekaHealthIndicatorConfiguration matched:
      - @ConditionalOnClass found required class 'org.springframework.boot.actuate.health.Health' (OnClassCondition)

   EurekaClientAutoConfiguration.EurekaHealthIndicatorConfiguration#eurekaHealthIndicator matched:
      - @ConditionalOnMissingBean (types: org.springframework.cloud.netflix.eureka.EurekaHealthIndicator; SearchStrategy: all) did not find any beans (OnBeanCondition)
      - @ConditionalOnEnabledHealthIndicator management.health.defaults.enabled is considered true (OnEnabledHealthIndicatorCondition)

   EurekaClientAutoConfiguration.RefreshableEurekaClientConfiguration matched:
      - @ConditionalOnClass found required class 'org.springframework.cloud.context.scope.refresh.RefreshScope' (OnClassCondition)
      - @ConditionalOnBean (types: org.springframework.cloud.autoconfigure.RefreshAutoConfiguration; SearchStrategy: all) found bean 'org.springframework.cloud.autoconfigure.RefreshAutoConfiguration' (OnBeanCondition)

   EurekaClientAutoConfiguration.RefreshableEurekaClientConfiguration#eurekaApplicationInfoManager matched:
      - @ConditionalOnMissingBean (types: com.netflix.appinfo.ApplicationInfoManager; SearchStrategy: current) did not find any beans (OnBeanCondition)

   EurekaClientAutoConfiguration.RefreshableEurekaClientConfiguration#eurekaClient matched:
      - @ConditionalOnMissingBean (types: com.netflix.discovery.EurekaClient; SearchStrategy: current) did not find any beans (OnBeanCondition)

   EurekaClientAutoConfiguration.RefreshableEurekaClientConfiguration#eurekaRegistration matched:
      - @ConditionalOnProperty (spring.cloud.service-registry.auto-registration.enabled) matched (OnPropertyCondition)
      - @ConditionalOnBean (types: org.springframework.cloud.client.serviceregistry.AutoServiceRegistrationProperties; SearchStrategy: all) found bean 'spring.cloud.service-registry.auto-registration-org.springframework.cloud.client.serviceregistry.AutoServiceRegistrationProperties' (OnBeanCondition)

   EurekaDiscoveryClientConfiguration matched:
      - @ConditionalOnClass found required class 'com.netflix.discovery.EurekaClientConfig' (OnClassCondition)
      - @ConditionalOnProperty (eureka.client.enabled) matched; @ConditionalOnProperty (spring.cloud.discovery.enabled) matched (OnPropertyCondition)

   EurekaDiscoveryClientConfiguration.EurekaClientConfigurationRefresher matched:
      - @ConditionalOnClass found required class 'org.springframework.cloud.context.scope.refresh.RefreshScopeRefreshedEvent' (OnClassCondition)

   EurekaServerAutoConfiguration matched:
      - @ConditionalOnBean (types: org.springframework.cloud.netflix.eureka.server.EurekaServerMarkerConfiguration$Marker; SearchStrategy: all) found bean 'eurekaServerMarkerBean' (OnBeanCondition)

   EurekaServerAutoConfiguration#eurekaController matched:
      - @ConditionalOnProperty (eureka.dashboard.enabled) matched (OnPropertyCondition)

   EurekaServerAutoConfiguration#peerEurekaNodes matched:
      - @ConditionalOnMissingBean (types: com.netflix.eureka.cluster.PeerEurekaNodes; SearchStrategy: all) did not find any beans (OnBeanCondition)

   EurekaServerAutoConfiguration.EurekaServerConfigBeanConfiguration#eurekaServerConfig matched:
      - @ConditionalOnMissingBean (types: com.netflix.eureka.EurekaServerConfig; SearchStrategy: all) did not find any beans (OnBeanCondition)
   RibbonEurekaAutoConfiguration matched:
      - AllNestedConditions 3 matched 0 did not; NestedCondition on ConditionalOnRibbonAndEurekaEnabled.OnRibbonAndEurekaEnabledCondition.OnEurekaClientEnabled found matching nested conditions @ConditionalOnProperty (eureka.client.enabled) matched; @ConditionalOnProperty (spring.cloud.discovery.enabled) matched, @ConditionalOnProperty (eureka.client.enabled) matched; @ConditionalOnProperty (spring.cloud.discovery.enabled) matched; NestedCondition on ConditionalOnRibbonAndEurekaEnabled.OnRibbonAndEurekaEnabledCondition.EurekaBeans @ConditionalOnBean (types: com.netflix.discovery.EurekaClient; SearchStrategy: all) found bean 'scopedTarget.eurekaClient'; NestedCondition on ConditionalOnRibbonAndEurekaEnabled.OnRibbonAndEurekaEnabledCondition.Defaults found matching nested conditions @ConditionalOnClass found required class 'com.netflix.niws.loadbalancer.DiscoveryEnabledNIWSServerList', @ConditionalOnBean (types: org.springframework.cloud.netflix.ribbon.SpringClientFactory; SearchStrategy: all) found bean 'springClientFactory', @ConditionalOnProperty (ribbon.eureka.enabled) matched (ConditionalOnRibbonAndEurekaEnabled.OnRibbonAndEurekaEnabledCondition)
   ServiceRegistryAutoConfiguration.ServiceRegistryEndpointConfiguration matched:
      - @ConditionalOnClass found required class 'org.springframework.boot.actuate.endpoint.annotation.Endpoint' (OnClassCondition)
      - @ConditionalOnBean (types: org.springframework.cloud.client.serviceregistry.ServiceRegistry; SearchStrategy: all) found bean 'eurekaServiceRegistry' (OnBeanCondition)
   EurekaClientAutoConfiguration.EurekaClientConfiguration:
      Did not match:
         - AnyNestedCondition 0 matched 2 did not; NestedCondition on EurekaClientAutoConfiguration.OnMissingRefreshScopeCondition.MissingScope @ConditionalOnMissingBean (types: org.springframework.cloud.autoconfigure.RefreshAutoConfiguration; SearchStrategy: all) found beans of type 'org.springframework.cloud.autoconfigure.RefreshAutoConfiguration' org.springframework.cloud.autoconfigure.RefreshAutoConfiguration; NestedCondition on EurekaClientAutoConfiguration.OnMissingRefreshScopeCondition.MissingClass @ConditionalOnMissingClass found unwanted class 'org.springframework.cloud.context.scope.refresh.RefreshScope' (EurekaClientAutoConfiguration.OnMissingRefreshScopeCondition)
   EurekaClientConfigServerAutoConfiguration:
      Did not match:
         - @ConditionalOnClass did not find required class 'org.springframework.cloud.config.server.config.ConfigServerProperties' (OnClassCondition)

   EurekaDiscoveryClientConfigServiceAutoConfiguration:
      Did not match:
         - @ConditionalOnProperty (spring.cloud.config.discovery.enabled) did not find property 'spring.cloud.config.discovery.enabled' (OnPropertyCondition)

   EurekaDiscoveryClientConfiguration.EurekaHealthCheckHandlerConfiguration:
      Did not match:
         - @ConditionalOnProperty (eureka.client.healthcheck.enabled) did not find property 'eureka.client.healthcheck.enabled' (OnPropertyCondition)
```
## 核心类行为分析
- org.springframework.cloud.netflix.eureka.metadata.DefaultManagementMetadataProvider
:warning:该类org.springframework.cloud.netflix.eureka.metadata.DefaultManagementMetadataProvider#get用于提供元数据

## Eureka
[![Build Status](https://travis-ci.org/Netflix/eureka.svg?branch=master)](https://travis-ci.org/Netflix/eureka)

Eureka is a REST (Representational State Transfer) based service that is primarily used in the AWS cloud for locating services for the purpose of load balancing and failover of middle-tier servers.

At Netflix, Eureka is used for the following purposes apart from playing a critical part in mid-tier load balancing.

* For aiding Netflix Asgard - an open source service which makes cloud deployments easier, in  
    + Fast rollback of versions in case of problems avoiding the re-launch of 100's of instances which 
      could take a long time.
    + In rolling pushes, for avoiding propagation of a new version to all instances in case of problems.

* For our cassandra deployments to take instances out of traffic for maintenance.

* For our memcached caching services to identify the list of nodes in the ring.

* For carrying other additional application specific metadata about services for various other reasons.


Building
----------
The build requires java8 because of some required libraries that are java8 (servo), but the source and target compatibility are still set to 1.7.


Support
----------
[Eureka Google Group](https://groups.google.com/forum/?fromgroups#!forum/eureka_netflix)


Documentation
--------------
Please see [wiki](https://github.com/Netflix/eureka/wiki) for detailed documentation.