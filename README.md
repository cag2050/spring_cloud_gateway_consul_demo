### 网关 Gateway 注册到服务中心 Consul
Spring Cloud Gateway 提供了一种默认转发的能力，只要将 Spring Cloud Gateway 注册到服务中心，Spring Cloud Gateway 默认就会代理服务中心的所有服务。

### 步骤一：创建项目
1. 选择 Gateway 和 Consul

### 步骤二：配置 src/main/resources/application.properties 文件
1. `spring.cloud.gateway.discovery.locator.enabled`
：是否与服务注册和发现组件进行结合，通过 serviceId 转发到具体的服务实例。默认为 false，设为 true 便开启通过服务中心的自动根据 serviceId 创建路由的功能。
2. 指定注册中心的地址，以便使用服务发现功能
```
spring.cloud.consul.host=127.0.0.1
spring.cloud.consul.port=8500
# 设置不需要注册到 consul 中
spring.cloud.consul.discovery.register=false
```

### 步骤三：创建 Consul 容器
1.镜像官方网址：https://hub.docker.com/_/consul

2.pull 镜像：
```
docker pull consul:1.6.0
```
3.创建容器（默认http管理端口：8500）
```
docker run -p 8500:8500 consul:1.6.0
```
4.访问管理网址
```
http://localhost:8500/
```

### 步骤四：启动服务端
1. 运行项目：https://github.com/cag2050/spring_cloud_consul_producer1_demo

### 步骤五：验证
1. 访问：http://localhost:8506/service-producer/hello ，正常返回，说明服务网关转发成功。

### 参考
参考资料 | 备注 | 网址
--- | --- | ---
springcloud(十六)：服务网关 Spring Cloud GateWay 服务化和过滤器（章节：服务网关注册到注册中心）| 此参考资料，项目里使用的注册中心是 Eureka。 | http://www.ityouknow.com/springcloud/2019/01/19/spring-cloud-gateway-service.html