### 一、Prometheus

​		Prometheus是一个开源监控系统，它前身是SoundCloud的警告工具包。从2012年开始，许多公司和组织开始使用Prometheus。该项目的开发人员和用户社区非常活跃，越来越多的开发人员和用户参与到该项目中。目前它是一个独立的开源项目，且不依赖与任何公司。 为了强调这点和明确该项目治理结构，Prometheus在2016年继Kurberntes之后，加入了Cloud Native Computing Foundation。

​		Prometheus的基本原理是通过HTTP周期性抓取被监控组件的状态，任意组件只要提供对应的HTTP接口并且符合Prometheus定义的数据格式，就可以接入Prometheus监控。

​		Prometheus Server负责定时在目标上抓取metrics（指标）数据并保存到本地存储里面。Prometheus采用了一种`Pull（拉）`的方式获取数据，不仅降低客户端的复杂度，客户端只需要采集数据，无需了解服务端情况，而且服务端可以更加方便的水平扩展。

​		Prometheus自研一套高性能的tsdb时序数据库，在V3版本可以达到每秒千万级别的数据存储，通过对接第三方时序数据库扩展历史数据的存储。



### 二、基本架构

![img](https://i.loli.net/2021/04/24/iNWDC8kBz9LYl7R.jpg)

### 三、主要特征

1. 多维度数据模型
2. 灵活的查询语言
3. 不依赖分布式存储，单个服务器节点是自主的
4. 以HTTP方式，通过pull模型拉去时间序列数据
5. 也通过中间网关支持push模型
6. 通过服务发现或者静态配置，来发现目标服务对象
7. 支持多种多样的图表和界面展示，grafana也支持它