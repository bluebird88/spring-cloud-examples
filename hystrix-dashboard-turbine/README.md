### 说明：
原有教程：http://www.ityouknow.com/springcloud/2017/05/18/hystrix-dashboard-turbine.html 说明和程序
存在问题：
- 端口冲突：node1和dashboard端口9001冲突
- 没有service producer，所以无法完成成功调用

解决方案：
- node2端口调整未9003
- copy一个producer到本项目中


项目说明：
- eureka是网关，负责所有请求的接受并指派一个服务者
- node1/node2/spring-cloud-consumer-hystrix-dashboard 是三个消费者，都在remote接口中指定了fallback的接口，也就是熔断后的临时服务对象
    其中spring-cloud-consumer-hystrix-dashboard 在application中，增加了 : @EnableHystrixDashboard ，也就是提供HystrixDashboard服务UI
    node1/node2只是普通消费者，并注册eureka中
- 关于相关的各个节点的调用点：
    - node1/node2/spring-cloud-consumer-hystrix-dashboard的监控入口： 
        - node1： http://hd01:9003/hystrix.stream ,注册到turbine中
        - node2: http://hd01:9002/hystrix.stream ,注册到turbine中
        - spring-cloud-consumer-hystrix-dashboar: http://hd01:9001/hystrix.stream  ,未注册到turbine中
    - http://hd01:8001/hystrix.stream ： turbine 项目的熔断监控界面，需要制定各个监控节点的stream监控入口
        - http://localhost:8001/turbine.stream： turbine聚合后的监控点
- 图形化监控方法：
    - 在turbine的监控界面：http://hd01:9001/hystrix中，输入监控点url，则会看到实时的调用信息、熔断状况
    - 其中http://localhost:8001/turbine.stream，可以看到被监控的node1、node2的状况
    - 本例中，node2调用了一个不存在的服务producer（producer2），所以总是显示是熔断的！ node1状态正常