# ECS 管理和 Agent 部署 {#concept_42906_zh .concept}

云服务器 ECS 是用户日志产生的重要场所。Log 和 ECS 紧密集成，让您便捷管理云服务器上产生的日志数据。

购买 ECS 后，即可将自己名下的 ECS 机器同步到 ARMS。

1.  登录 [ARMS 控制台](https://arms.console.aliyun.com/#/home)，并在页面左上角选择所需地域。
2.  在左侧导航栏中选择**自定义监控-数据源管理** \> **云服务器 ECS** 。
3.  在实例列表页面顶部选择需要操作的地域，单击右上角的**同步 ECS**。

    **说明：** 同步操作只会同步当前地域的 ECS。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152292/155496245343559_zh-CN.png) 

4.  如果用户没有授权，第一次同步时会提示用户需要授权。在提示框中单击**进入 RAM 进行授权**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152292/155496245343560_zh-CN.png) 

5.  在**云资源访问授权**页面勾选所需的权限，并单击**同意授权**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152292/155496245343561_zh-CN.png) 

    **说明：** 如果当前账号是 RAM 子账号，则无法授权。请确保使用主账号授权。


## Agent 状态检测 { .section}

ARMS 系统通过 Logtail（日志收集客户端）来收集 ECS 上的日志，因此您需要在每台 ECS 服务器上安装 Logtail。

您可以对单台 ECS 进行状态检查，也可以批量进行检查。

## Agent 安装 { .section}

检测发现未安装 Agent 的机器，请参考[安装 Agent](cn.zh-CN/自定义监控/管理数据源/ECS 数据源/安装 Agent.md#)完成安装。

**说明：** ECS 安装完 Agent 之后，需要返回实例列表页面进行**检查 Agent** 操作，以更新当前 ECS 的相关状态。

