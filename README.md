# 注册中心

Netflix Eureka

Alibaba Nacos

HashiCorp Consul

Apache Zookeeper

CoreOS etcd

CNCF CoreDNS



对比

| 特性           | Eureka                       | Nacos                      | Consul                | Zookeeper              | etcd              |
| -------------- | ---------------------------- | -------------------------- | --------------------- | ---------------------- | ----------------- |
| CAP            | AP                           | CP+AP                      | CP                    | CP                     | CP                |
| 健康检查       | Client Beat                  | TCP/HTTP/MYSQL/Client Beat | TCP/HTTP/gRPC/Cmd     | Keep Alive（弱）长连接 | 连接心跳          |
| 雪崩保护       | 有                           | 有                         | 无                    | 无                     | 无                |
| 一致性协议     | -                            | -                          | Raft                  | Paxos                  | Raft              |
| 自动注销实例   | 支持                         | 支持                       | 不支持                | 支持                   | 不支持            |
| 访问协议       | HTTP                         | HTTP/DNS                   | HTTP/DNS              | TCP                    | HTTP/gRPC         |
| 监听支持       | 支持 long polling/大部分增量 | -                          | 全量/支持long polling | 支持                   | 支持 long polling |
| 多数据中心     | 支持                         | 支持                       | 支持                  | 不支持                 | 不支持            |
| 安全           | -                            | -                          | acl/https             | acl                    | https支持（弱）   |
| 跨注册中心同步 | 不支持                       | 支持                       | 支持                  | 不支持                 | 不支持            |





# 环境

## 安装

git clone https://github.com/hashicorp/consul.git

make dev

consul version

## 启动

consul agent -dev



# 特性

Raft算法

服务发现

健康检查

Key/Value存储

多数据中心

支持Http和DNS协议接口

官方提供Web管理页面

# 角色

**Client** ：客户端，无状态，将HTTP和DNS接口请求转发给局域网内的服务端集群

**Server**：服务端，保存配置信息，高可用集群，每个数据中心的Server数量推荐为3个或5个

`consule agent -dev` 开发模式运行

`consule agent -client `  客户端运行

`consule agent -server` 服务端运行

# 命令

可以使用cmd方式 `consul members -detailed` 查看consul成员详情

Node       Address         Status  Tags
machine-1  127.0.0.1:8301  alive   acls=0,ap=default,build=1.13.0dev:58eed6b0,dc=dc1,ft_fs=1,ft_si=1,grpc_port=8502,id=67b8ce17-7804-8ff8-ac1f-27d4acc70f62,port=8300,raft_vsn=3,role=consul,segment=<all>,vsn=2,vsn_max=3,vsn_min=2,wan_join_port=8302

也可使用http方式`curl localhost:8500/v1/catalog/nodes`

[

{

​    "ID": "67b8ce17-7804-8ff8-ac1f-27d4acc70f62",

​    "Node": "machine-1",

​    "Address": "127.0.0.1",

​    "Datacenter": "dc1",

​    "TaggedAddresses": {

​      "lan": "127.0.0.1",

​      "lan_ipv4": "127.0.0.1",

​      "wan": "127.0.0.1",

​      "wan_ipv4": "127.0.0.1"

​    },

​    "Meta": {

​      "consul-network-segment": ""

​    },

​    "CreateIndex": 13,

​    "ModifyIndex": 14

}

]

# 工作原理

- Consul（注册中心）
    - 健康检查（health）到Producer，默认间隔10s检查

- Producer（服务提供者）
    - 将自身服务IP+Port注册到Consul中

- Consumer（服务消费者）
    - 拉取Consul的注册信息temp table（临时表）
    - 访问注册信息中的Producer

# 配置





# KV操作

consul kv put username zhangsan 设置Key Value

consul kv get username 获取指定Key

consul kv get -detailed username 获取指定Key的详情

consul kv get -recurse 遍历所有

consul kv put -cas -modify-index=88 username lisi 以CAS方式原子更新

consul kv delete username 删除指定Key

# 参考

[Consul官网](https://www.consul.io/) \
[Consul 简介和快速入门](https://www.bookstack.cn/books/consul-guide)
