# 创建 RAM 用户角色并授权 {#concept_74785_zh .concept}

本文介绍了如何借助 RAM 用户角色实现跨账号调用 OpenAPI。

## 背景信息 {#section_bn5_bvx_g2b .section}

RAM 用户角色是一种虚拟用户，它没有实际的身份认证密钥，需要被一个受信的实体用户（例如云账号、RAM-User 账号）扮演才能正常使用。扮演成功后，实体用户将获得 RAM 用户角色的临时安全令牌，凭借这个临时安全令牌就能以 RAM 用户角色身份访问被授权的资源。

如果您需要将 ARMS OpenAPI 的调用权限授予各个受信实体用户，允许该实体用户下的 RAM 角色操作 ARMS OpenAPI，那么您需要执行以下步骤。

## 步骤一：创建用户角色并指定受信云账号 {#section_hrq_234_hhb .section}

1.  在 [RAM 控制台](http://ram.console.aliyun.com)左侧导航栏中选择**角色管理**。
2.  在角色管理页面右上角单击**新建角色**。
3.  在创建角色对话框的**选择角色类型**子页面上，单击**用户角色**。
4.  在**填写类型信息**子页面上，在**受信云账号 ID** 文本框中输入受信云账号的 ID，并单击**下一步**。
5.  在**配置角色基本信息**子页面上，输入角色名称和备注，并单击**创建**。创建的角色显示在角色管理页面上。

## 步骤二：为 RAM 用户角色授权 {#section_irq_234_hhb .section}

新建的用户角色没有任何权限，您需要为其授予 ARMS 的访问权限。上个步骤指定的受信云账号将有权限扮演该 RAM 用户角色在 ARMS 中进行操作。

1.  在 [RAM 控制台](http://ram.console.aliyun.com)左侧导航栏中选择**角色管理**。
2.  在角色管理页面上找到需要授权的角色，单击**操作**栏中的**授权**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152350/155489759443265_zh-CN.png) 

3.  在编辑角色授权策略对话框中，通过关键词搜索授权策略 **AliyunARMSFullAccess**，并单击 **\>** 按钮将该授权策略移至右侧的**已选授权策略名称**列表，然后单击**确定**。

## 步骤三：为受信云账号的 RAM 用户授权 {#section_jkv_g34_hhb .section}

RAM 用户角色需要被一个受信的实体用户扮演才能正常使用，但是受信实体用户不能以自己的身份扮演 RAM 用户角色，而必须以 RAM 用户的身份和形式扮演。换言之，RAM 用户角色只能通过 RAM 用户身份来扮演和使用。另外，受信云账号必须为其名下的 RAM 用户授予 AliyunSTSAssumeRoleAccess 权限，使 RAM 用户能够调用 STS 服务 AssumeRole 接口，RAM 用户才能代表受信云账号扮演[步骤一：创建用户角色并指定受信云账号](#section_hrq_234_hhb)中创建的 RAM 用户角色。

1.  在 RAM 控制台左侧导航栏中选择**用户管理**。
2.  在用户管理页面上找到需要授权的用户，单击**操作**栏中的**授权**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152350/155489759443266_zh-CN.png) 

    **说明：** 如果没有 RAM 用户，请按照[创建 RAM 子账号并授权](cn.zh-CN/访问控制/创建 RAM 子账号并授权.md#)的说明创建一个。

3.  在编辑个人授权策略对话框中，通过关键词搜索授权策略 AliyunSTSAssumeRoleAccess，并单击 **\>** 按钮将该授权策略移至右侧的**已选授权策略名称**列表，然后单击**确定**。

## 步骤四：获取 RAM 用户角色的临时安全令牌 {#section_ybq_j34_hhb .section}

关于获取 STS 安全令牌的详细信息，请参见[使用入门](../../../../../cn.zh-CN/SDK参考/SDK参考（STS）/Java SDK/使用入门.md#)。

1.  在 pom.xml 中添加 Maven 依赖。

    ```
    <dependency>
      <groupId>com.aliyun</groupId>
      <artifactId>aliyun-java-sdk-sts</artifactId>
      <version>3.0.0</version>
    </dependency>
    <dependency>
      <groupId>com.aliyun</groupId>
      <artifactId>aliyun-java-sdk-core</artifactId>
      <version>3.5.0</version>
    </dependency>
    ```

2.  获取 STS Token 的代码（示例）

    ```
    String endpoint = "sts.aliyuncs.com";
    String regionId = "cn-hangzhou";
    String accessKeyId = "RAM账号的AK";
    String accessKeySecret = "RAM账号的SK";
    String roleArn = "acs:ram::创建RAM角色的主账号ID:role/角色名称";
    String roleSessionName = "session-name";
    try {
       DefaultProfile.addEndpoint(regionId,regionId, "Sts", endpoint);
       IClientProfile profile = DefaultProfile.getProfile(regionId, accessKeyId, accessKeySecret);
       DefaultAcsClient client = new DefaultAcsClient(profile);
       final AssumeRoleRequest request = new AssumeRoleRequest();
       request.setMethod(MethodType.POST);
       request.setRoleArn(roleArn);
       request.setRoleSessionName(roleSessionName);
       request.setPolicy(null); //可选，此处可以填写为NULL
       final AssumeRoleResponse response = client.getAcsResponse(request);
       System.out.println("Expiration: " + response.getCredentials().getExpiration());
       System.out.println("Access Key Id: " + response.getCredentials().getAccessKeyId());
       System.out.println("Access Key Secret: " + response.getCredentials().getAccessKeySecret());
       System.out.println("Security Token: " + response.getCredentials().getSecurityToken());
    } catch (Exception e) {
    }
    ```

    **说明：** 

    -   -   关于 STS 各地域的 Endpoint，请参见[服务地址](../../../../../cn.zh-CN/API 参考（STS）/附录/服务地址.md#)。
    -   获取 roleArn 的方法：以创建 RAM 角色的主账号登录 [RAM 控制台](http://ram.console.aliyun.com)，并在左侧导航栏中选择**角色管理**。然后在角色管理页面上单击角色名称或**操作**栏中的**管理**，即可在角色详情页面的**基本信息**区域查看该角色 ARN。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152350/155489759443267_zh-CN.png) 


