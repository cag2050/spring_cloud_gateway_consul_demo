### 网关 Gateway 注册到服务中心 Consul
Spring Cloud Gateway 提供了一种默认转发的能力，只要将 Spring Cloud Gateway 注册到服务中心，Spring Cloud Gateway 默认就会代理服务中心的所有服务。

### 步骤一：创建项目
1. 选择 Gateway 和 Consul

### 步骤二：配置 src/main/resources/application.yml 文件
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

### 步骤五：验证服务网关转发成功
1. 项目 spring_cloud_consul_producer1_demo 的 /hello 服务是：http://localhost:8501/hello ，访问：http://localhost:8506/service
-producer/hello ，正常返回类似文字：hello，说明服务网关转发成功。

### 步骤六：Filter 的使用：使用 AddRequestParameter GatewayFilter 在请求中添加指定参数
1. 修改 src/main/resources/application.yml，添加 routes 部分
2. 访问地址 http://localhost:8501/foo 页面返回类似：`参数 foo 的值是：null`，说明并没有接受到参数 foo；通过网关来调用此服务，浏览器访问地址 http://localhost:8506/foo
页面返回类似：`参数 foo 的值是：bar`，说明成功接收到参数 foo 的值 bar ,证明网关在转发的过程中已经通过 filter 添加了设置的参数和值。

### 步骤七：服务化路由转发
1. 再启动服务端：https://github.com/cag2050/spring_cloud_consul_producer2_demo
2. 修改 src/main/resources/application.yml，将`uri: http://localhost:8501` 修改成 `uri: lb://service-producer`，浏览器访问地址 http
://localhost:8506/foo，页面交替出现：`参数 foo 的值是：bar，from port：8501` 和 `参数 foo 的值是：bar，from port：8502`，证明请求依据均匀转发到后端服务，并且后端服务均接收到了 filter 增加的参数 foo 值。

### 参考
参考资料 | 备注 | 网址
--- | --- | ---
springcloud(十六)：服务网关 Spring Cloud GateWay 服务化和过滤器 | 此参考资料，项目里使用的注册中心是 Eureka。 | http://www.ityouknow.com/springcloud/2019/01/19/spring-cloud-gateway-service.html

> 代码仓库：https://github.com/cag2050/spring_cloud_gateway_consul_demo