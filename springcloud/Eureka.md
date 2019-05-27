# Eureka Learn
Eureka Learn 记录了对于Eureka的相关学习
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
## 设计模式

## 自我理解
### 日志
- [启动日志](https://github.com/independenter/source-learning/blob/master/springcloud/Eukeka.log)
### 核心类名
- org.springframework.cloud.context.scope.GenericScope
Generating bean factory id from names: [default.org.springframework.cloud.netflix.ribbon.eureka.RibbonEurekaAutoConfiguration.RibbonClientSpecification,eureka.dashboard-org.springframework.cloud.netflix.eureka.server.EurekaDashboardProperties, eureka.instance.registry-org.springframework.cloud.netflix.eureka.server.InstanceRegistryProperties, eurekaApplication, eurekaApplicationInfoManager, eurekaAutoServiceRegistration, eurekaClient, eurekaClientConfigBean, eurekaController, eurekaDiscoverClientMarker, eurekaFeature, eurekaHealthIndicator, eurekaInstanceConfigBean, eurekaRegistration, eurekaServerBootstrap, eurekaServerConfig, eurekaServerContext, eurekaServerFeature, eurekaServerMarkerBean, eurekaServiceRegistry,org.springframework.cloud.netflix.eureka.EurekaClientAutoConfiguration, org.springframework.cloud.netflix.eureka.EurekaClientAutoConfiguration$EurekaHealthIndicatorConfiguration, org.springframework.cloud.netflix.eureka.EurekaClientAutoConfiguration$RefreshableEurekaClientConfiguration, org.springframework.cloud.netflix.eureka.EurekaDiscoveryClientConfiguration, org.springframework.cloud.netflix.eureka.EurekaDiscoveryClientConfiguration$EurekaClientConfigurationRefresher, org.springframework.cloud.netflix.eureka.config.DiscoveryClientOptionalArgsConfiguration, org.springframework.cloud.netflix.eureka.server.EurekaServerAutoConfiguration, org.springframework.cloud.netflix.eureka.server.EurekaServerAutoConfiguration$EurekaServerConfigBeanConfiguration, org.springframework.cloud.netflix.eureka.server.EurekaServerInitializerConfiguration, org.springframework.cloud.netflix.eureka.server.EurekaServerMarkerConfiguration,org.springframework.cloud.netflix.ribbon.eureka.RibbonEurekaAutoConfiguration,peerEurekaNodes,scopedTarget.eurekaApplicationInfoManager, scopedTarget.eurekaClient, scopedTarget.eurekaRegistration,]
- org.springframework.beans.factory.support.DefaultListableBeanFactory
Creating shared instance of singleton bean on the factory id above.
- org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider
2019-05-27 23:22:30.228 [main] DEBUG org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider - Identified candidate component class: URL [jar:file:/C:/Users/Administrator/.m2/repository/com/netflix/eureka/eureka-client/1.9.8/eureka-client-1.9.8.jar!/com/netflix/discovery/provider/DiscoveryJerseyProvider.class]
2019-05-27 23:22:30.339 [main] DEBUG org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider - Identified candidate component class: URL [jar:file:/C:/Users/Administrator/.m2/repository/com/netflix/eureka/eureka-core/1.9.8/eureka-core-1.9.8.jar!/com/netflix/eureka/resources/ASGResource.class]
2019-05-27 23:22:30.343 [main] DEBUG org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider - Identified candidate component class: URL [jar:file:/C:/Users/Administrator/.m2/repository/com/netflix/eureka/eureka-core/1.9.8/eureka-core-1.9.8.jar!/com/netflix/eureka/resources/ApplicationsResource.class]
2019-05-27 23:22:30.345 [main] DEBUG org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider - Identified candidate component class: URL [jar:file:/C:/Users/Administrator/.m2/repository/com/netflix/eureka/eureka-core/1.9.8/eureka-core-1.9.8.jar!/com/netflix/eureka/resources/InstancesResource.class]
2019-05-27 23:22:30.346 [main] DEBUG org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider - Identified candidate component class: URL [jar:file:/C:/Users/Administrator/.m2/repository/com/netflix/eureka/eureka-core/1.9.8/eureka-core-1.9.8.jar!/com/netflix/eureka/resources/PeerReplicationResource.class]
2019-05-27 23:22:30.347 [main] DEBUG org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider - Identified candidate component class: URL [jar:file:/C:/Users/Administrator/.m2/repository/com/netflix/eureka/eureka-core/1.9.8/eureka-core-1.9.8.jar!/com/netflix/eureka/resources/SecureVIPResource.class]
2019-05-27 23:22:30.347 [main] DEBUG org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider - Identified candidate component class: URL [jar:file:/C:/Users/Administrator/.m2/repository/com/netflix/eureka/eureka-core/1.9.8/eureka-core-1.9.8.jar!/com/netflix/eureka/resources/ServerInfoResource.class]
2019-05-27 23:22:30.348 [main] DEBUG org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider - Identified candidate component class: URL [jar:file:/C:/Users/Administrator/.m2/repository/com/netflix/eureka/eureka-core/1.9.8/eureka-core-1.9.8.jar!/com/netflix/eureka/resources/StatusResource.class]
2019-05-27 23:22:30.348 [main] DEBUG org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider - Identified candidate component class: URL [jar:file:/C:/Users/Administrator/.m2/repository/com/netflix/eureka/eureka-core/1.9.8/eureka-core-1.9.8.jar!/com/netflix/eureka/resources/VIPResource.class]
- 
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