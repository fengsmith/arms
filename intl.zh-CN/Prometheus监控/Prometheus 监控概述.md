# Prometheus 监控概述 {#concept_662038 .concept}

ARMS Prometheus 监控全面对接开源 Prometheus 生态，支持类型丰富的组件监控，提供多种开箱即用的预置监控大盘，且提供全面托管的 Prometheus 服务。

## 什么是 Prometheus？ {#section_jfk_tib_7tc .section}

Prometheus 是一套开源的系统监控和报警框架，灵感源自 Google 的 Borgmon 监控系统。2012 年，SoundCloud 的 Google 前员工创造了 Prometheus，并作为社区开源项目进行开发。2015 年，该项目正式发布。2016 年，Prometheus 加入云原生计算基金会（[Cloud Native Computing Foundation](https://cncf.io/)），成为受欢迎度仅次于 Kubernetes 的项目。

Prometheus 具有以下特性：

-   多维的数据模型（基于时间序列的 Key/Value 键值对）
-   灵活的查询和聚合语言 PromQL
-   提供本地存储和分布式存储
-   通过基于 HTTP 的 Pull 模型采集时间序列数据
-   可利用 Pushgateway（Prometheus 的可选中间件）实现 Push 模式
-   可通过动态服务发现或静态配置发现目标机器
-   支持多种图表和数据大盘

## 为什么要使用 ARMS Prometheus 监控？ {#section_ge4_umu_35c .section}

借助 ARMS Prometheus 监控，您无需自行搭建 Prometheus 监控系统，因而无需关心底层数据存储、数据展示、系统运维等问题。

ARMS Prometheus 监控具有以下特性：

-   类型丰富的组件监控
-   开箱即用的监控大盘
-   全面托管的 Prometheus 服务

## 类型丰富的组件监控 {#section_aul_2u2_1vj .section}

ARMS Prometheus 监控全面对接 Prometheus 生态，支持数据库、消息、HTTP 等多种类型组件的监控。

![ARMS Prometheus Components](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/531966/156082312549410_zh-CN.png)

## 开箱即用的监控大盘 {#section_yr7_wor_wh2 .section}

ARMS Prometheus 监控提供若干预先配置的监控大盘，可展示丰富的关键性能指标，包括但不限于：

-   Kubernetes 集群监控

    -   Kubelet
    -   kube-state-metric
    -   api-server
-   主机监控

    -   CPU 使用率
    -   内存使用率
    -   系统负载
    -   磁盘使用量
    -   网络流量

![](images/49437_zh-CN.png "预置监控大盘：主机详情")

![](images/49438_zh-CN.png "预置监控大盘：Kubernetes 概览")

## 全面托管的 Prometheus 服务 {#section_pbm_9an_9ss .section}

ARMS Prometheus 监控为您提供全面托管的 Prometheus 服务，成本比自建 Prometheus 监控系统更低。该服务的特性包括：

-   提供高可用、可扩展的 Prometheus Server
-   与阿里云容器服务深度集成
-   提供监控数据无限存储能力
-   提供 Prometheus on Prometheus 能力

