# RAM 用户（子账号）访问控制台 {#concept_52552_zh .concept}

ARMS 支持阿里云访问控制（RAM），本文介绍如何授权 RAM 用户（子账号）访问 ARMS。

## 前提条件 {#section_ik2_pjy_gfb .section}

使用 RAM 子账号访问 ARMS 控制台，子账号需要满足以下两个条件：

-   需要为该子账号创建 AccessKeyId/AccessSecretKey。

-   必须启用该子账号的控制台登录功能，为该子账号设置登录用户名、密码。


操作方法请参考[用户](../../../../../cn.zh-CN//身份管理/用户.md#) 。

## 授权子账号访问 ARMS 服务控制台 {#section_iqq_qjy_gfb .section}

满足登录前提条件的子账号，还需要授权才能使用 ARMS 服务控制台。目前 ARMS 仅支持授予子账号 ARMS 服务资源完全访问权限。

**说明：** Open API 暂时不支持子账号访问。

操作步骤如下：

1.  登录 RAM 控制台，在子账号管理界面，选择要授权的子账号。操作方法参见[RAM 授权](../../../../../cn.zh-CN/用户指南/权限管理/授权管理/RAM 授权.md#)。
2.  在授权策略页面选择 AliyunARMSFullAccess。

授权完成后，即可用子账号登录 ARMS 控制台。

