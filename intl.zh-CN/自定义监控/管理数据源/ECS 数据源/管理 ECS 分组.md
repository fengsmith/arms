# 管理 ECS 分组 {#concept_42913_zh .concept}

分组管理是 ARMS 简化用户资源管理的一个重要方法。您可以通过分组管理的方式一次性管理所有需要通过 Logtail 客户端收集日志的 ECS 机器。目前 ARMS 仅支持 ECS 日志源分组。

1.  登录 [ARMS 控制台](https://arms.console.aliyun.com/#/home)，并在页面左上角选择所需地域。
2.  在控制台左侧导航栏中选择**自定义监控-数据源管理** \> **云服务器 ECS**。
3.  在实例列表页面，单击**云服务器 ECS**分组标签页。
4.  在云服务器 ECS 分组标签页上单击右上角的**创建 ECS 分组**。
5.  在新增 ECS 分组对话框中，输入组名、选择地域、添加需要分组的 ECS 服务器，单击**确定**。

    **说明：** 只能将同一个地域的机器纳入同一个 ECS 分组。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152294/155496245843700_zh-CN.png) 


## 查看 ECS 分组内的机器详情 { .section}

在云服务器 ECS 分组标签页上，单击对应 ECS 分组的展开按钮，即可查看该分组内的 ECS 机器的详细信息，如图所示。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152294/155496245843701_zh-CN.png) 

您可以在此页面将特定 ECS 移出分组。

## 编辑 ECS 分组 { .section}

单击 ECS 分组上方的铅笔按钮，即可对当前 ECS 分组进行修改。您可以修改 ECS 分组名称，添加 ECS 机器到分组，或者将某个 ECS 机器移出分组。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152294/155496245843704_zh-CN.png) 

## 删除 ECS 分组 { .section}

单击 ECS 分组上方的删除按钮，即可删除当前分组。

