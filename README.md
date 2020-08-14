# nacos-elasticsearch

这是一个elasticsearch的插件，实现将当前节点的http访问点注册到nacos，以便其他基于nacos注册中心的微服务得以访问elasticsearch。

欢迎提交PR和Issue

插件主要依赖nacos-client实现：

- 注册服务
- 保持服务心跳
- 周期性刷新nacos服务器列表

## 编译

```sh
mvn package
```

## 安装

将构建的target/nacos-elasticsearch-xx.zip包解压到每个es节点的`plugins`目录，将外层文件夹重命名为`nacos-elasticsearch`（注意权限），最终目录结构如下：

```
plugins
|--nacos-elasticsearch
    |--nacos-elasticsearch-1.0.0.jar
    |--plugin-descriptor.properties
    |--plugin-security.policy
    |--....
```

配置每个es节点，增加nacos相关配置项，并重启每个es节点，注意节点是否能正常启动。如果一切顺利，在nacos管理界面可以查看到elasticsearch的服务和实例。

> 当前构建的插件包，默认支持7.0.0的elasticsearch，如果需要适配其他版本，可以自行修改`plugin-descriptor.properties`
> 但也有可能因为jar包冲突等问题，无法适配elasticsearch

## 配置

为每个elasticsearch增加了如下配置：

| key  | required | default value | description |
| :--- | :----: | :------: | :---------- |
| nacos.register.enabled | false | false  | 是否启动nacos自动注册功能，默认不开启 |
| nacos.server.addr | true |   | nacos服务ip |
| nacos.server.port | false | 8848  | nacos服务port |
| nacos.server.user | false |   | nacos登录用户名 |
| nacos.server.password | false |   | nacos登录密码 |
| nacos.service.name | false | elasticsearch  | 配置当前es集群注册到nacos的服务名，默认服务名就是elasticsearch |
| nacos.beat.interval | false | 10s  | 保持心跳的上报周期 |
| nacos.refresh.interval | false | 30s  | 刷新nacos服务列表的周期 |

要开启插件功能，至少配置`nacos.register.enabled: true`和`nacos.server.addr`