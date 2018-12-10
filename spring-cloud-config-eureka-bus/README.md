### 启动过程：
- 配置rabbitmq： hd02：
```$xslt
    docker pull radditmq
    
    启动rabbit image；
    docker run --rm -d --hostname my-rabbit --name some-rabbit -p 5671-5672  -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=111111 rabbitmq:3-management
    
    注意：启动地址是随机的，需要ps后看实际地址并修改配置
    FIXME: 后续应该修改为固定地址
    
    spring.cloud.bus.trace.enabled： 开启消息跟踪，通过mq进行消息侦听
    

```