# 同步 LogHub 数据源 {#concept_44589_zh .concept}

如果您已经开通阿里云日志服务（Log Service），则可以在 ARMS 控制台直接同步日志服务 LogHub 中的数据并作为自定义监控的数据源使用。

## 前提条件 {#section_wpx_rvc_dhb .section}

-   您已开通[阿里云日志服务](https://www.aliyun.com/product/sls)。
-   使用主账号或已被授权访问日志服务的 RAM 用户登录 ARMS 控制台。如果使用未被授权访问日志服务的 RAM 用户登录，则无法授权 ARMS 访问您在日志服务中的资源，继而无法同步 LogHub 数据源。

## 操作步骤 {#section_emj_rvc_dhb .section}

1.  登录 [ARMS 控制台](https://arms.console.aliyun.com/#/home)，并在页面左上角选择所需地域。
2.  在左侧导航栏中选择**自定义监控-数据源管理** \> **LogHub 数据源**。如果此前未授权 ARMS 读取 LogHub 数据，则 ARMS 会给出提示信息。
3.  在提示对话框中单击**进入 RAM 进行授权**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152295/155496239643711_zh-CN.png) 

4.  在云资源访问授权页面勾选系统创建的 RAM 角色，并单击**同意授权**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152295/155496239643712_zh-CN.png) 

5.  在 LogHub 数据源页面右上角单击**同步 LogHub**。
6.  在同步 LogHub对话框中，单击**确定同步**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152295/155496239643713_zh-CN.png) 

7.  同步完毕后，在同步 LogHub 对话框中，单击**关闭**。同步后的数据将显示在 LogHub 数据源页面上。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152295/155496239643714_zh-CN.png) 


## 后续操作 {#section_swr_1gd_dhb .section}

LogHub 数据源同步成功后，您可以前往**自定义监控** \> **监控任务管理**页面创建自定义监控任务。在自定义监控任务的数据源配置页面上，当选择 **LogHub 数据源**作为**日志源类型**时，同步后的 LogHub 数据源将显示在**选择日志源**下拉列表中。

## 更多信息 {#section_fwh_lgd_dhb .section}

-   [配置数据源](cn.zh-CN/自定义监控/创建监控任务/配置数据源.md#)
-   [什么是日志服务](../../../../../../cn.zh-CN/产品简介/什么是日志服务.md#)

