# 集成Alibaba Sentinel

## 1.Sentinel下载

进入Alibaba Sentinel Github https://github.com/alibaba/Sentinel/releases

下载sentinel-dashboard-x.x.x.jar

## 2.启动，端口修改

```sh
java -jar sentinel-dashboard-x.x.x.jar
```

Sentinel-dashboard默认的启动端口号为8080，部分用户的端口可能已经被占用，

可以通过以下命令修改启动端口号。

```sh
java -jar sentinel-dashboard-x.x.x.jar --server.port=8070
```

初始账号密码为：sentinel

## 3.Sentinel初始监控

修改bootstrap.yml,添加

```yml
spring:
  cloud:
    nacos:
      sentinel:
        transport:
          #配置Sentinel dashboard地址
          dashboard: localhost:8070
          #默认8719端口,如果被占用会向上扫描。
          port: 8719
```

添加依赖

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-alibaba-sentinel</artifactId>
</dependency>
```

## 4.Sentinel持久化

修改pom

```xml
<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-datasource-nacos</artifactId>
</dependency>
```

修改bootstarp

```yaml
spring:
  application:
    name: mogu-web
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
      sentinel:
        transport:
          #配置Sentinel dashboard地址
          dashboard: localhost:8070
          #默认8719端口,如果被占用会向上扫描。
          port: 8719
        datasource:
          dsl:
            nacos:
              server-addr: localhost:8848
              dataId: mogu-web
              groupId: dev
              data-type: json
              rule-type: flow
```

在nacos中添加配置

```json
[
    {
        "resource": "/rateLimit/byUrl",//接口或者方法名
        "limitApp": "default",
        "grade": 1,
        "count": 1,
        "stragegy": 0,
        "controlBehavior": 0,
        "clusterMode": false
    }
]
```



