# 创建 RAM 子账号并授权 {#concept_74784_zh .concept}

本文介绍了如何创建 RAM 子账号，并授权该子账号访问 ARMS 和调用 OpenAPI。

出于安全考虑，您可以使用主账号创建 RAM 子账号，让子账号处理日常运维工作，或以其自身的 AK 和 SK 调用 OpenAPI。但在没有为子账号授权的情况下，如果在 [RAM 用户登录入口](https://signin.aliyun.com/login.htm)登录该 RAM 子账号，并访问 ARMS 控制台，或使用该子账号的 AK 和 SK 调用 OpenAPI，您将收到关于没有权限的错误信息。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152349/155489763943259_zh-CN.png) 

因此，在创建 RAM 子账号之后，您还需要为该子账号赋予 `AliyunARMSFullAccess` 权限。

## 步骤一：创建 RAM 子账号 { .section}

1.  在 [RAM 控制台](http://ram.console.aliyun.com)左侧导航栏中选择**用户管理**。
2.  在用户管理页面右上角单击**新建用户**。
3.  在创建用户对话框中，填写用户信息，勾选**为该用户自动生成 AccessKey**，并单击**确定**。
4.  在手机验证对话框中单击**点击获取**，并输入收到的手机验证码，然后单击**确定**。创建的用户显示在用户管理页面上。
5.  单击用户名称或**操作**栏中的**管理**。
6.  在用户详情页面的 **Web 控制台登录管理**区域中，单击**启用控制台登录**，然后在弹出对话框中输入用户登录密码，并单击**确定**。

## 步骤二：授权子账号访问 ARMS 或调用 OpenAPI { .section}

1.  在 [RAM 控制台](http://ram.console.aliyun.com)左侧导航栏中选择**用户管理**。
2.  在用户管理页面上找到需要授权的用户，单击**操作**栏中的**授权**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152349/155489764143260_zh-CN.png) 

3.  在对话框中，通过关键词搜索授权策略 AliyunARMSFullAccess，并单击 **\>** 按钮将该授权策略移至右侧的**已选授权策略名称**列表，然后单击**确定**。

