springcloud config 分布式统一配置中心
所有微服务都有自己唯一的一个的yml配置文件(同一个微服务对应的唯一的yml文件在dev/test环境用---拼接),
且都在该项目的根目录下(做到了统一管理),并push到github, eureka-provider微服务的配置统一由github获得,
,完成多环境的变更,即eureka-provider从3344cofig-server从github获取远程配置

应用的目的:
由于多个微服务模块的yml修改需要手动修改,运维麻烦(dev, prod, test环境下),
首先将 springcloud-config项目下的eureka-provider-config-client.yml
(包含dev/test两个环境下的配置信息)上传到github,
我们只需将每个微服务client集成 config-client依赖,
在其bootstrap.yml中添加,config-server的地址,并设置 本次访问的
配置项(dev/test), 就会加载github远程库springcloud-config的
eureka-provider-config-client.yml文件的dev的配置信息作为eureka-provider
项目的配置信息(只需要验证端口即可)


最终实现 不同环境下 配置信息的 动态切换
以后只需要修改bootstrap.yml的profile: dev或test,
        运维工程师修改congfig-server下某个微服务的配置文件即可.
        而我这里的dev不变,最终连接的数据库就变了
        
而微服务自己原有的的application.yml只需要写
spring:
  application:
    name: eureka-provider
打jar包就可以了,也是从github动态读取多环境下的配置文件



