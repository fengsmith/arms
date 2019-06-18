# 为容器服务 Kubernetes 版应用安装探针 {#concept_103106_zh .concept}

借助 ARMS 应用监控，您可以对容器服务 Kubernetes 版的应用进行应用拓扑、接口调用、异常事务和慢事务监控、SQL 分析等监控。本文介绍了如何在容器服务管理控制台上将容器服务 Kubernetes 版应用接入 ARMS 应用监控。

## 前提条件 {#section_54q_uxi_j2i .section}

-   您已成功创建 Kubernetes 集群，请参见[创建Kubernetes集群](../../../../intl.zh-CN/用户指南/Kubernetes 集群/集群管理/创建Kubernetes集群.md#)。
-   您已成功开通 ARMS 服务，请参见[开通 ARMS](../../../../intl.zh-CN/快速入门/开通 ARMS.md#)。

## 安装组件 { .section}

1.  登录[容器服务管理控制台](https://cs.console.aliyun.com/?spm=a2c4g.11186623.2.17.66d75bc79AYTH7)。

2.  在左侧导航栏中选择**市场** \> **应用目录**，在应用目录页面选择 **ack-arms-pilot**。

3.  在应用目录-ack-arms-pilot 页面右侧的**创建**区域，编辑创建信息后单击**创建**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152230/156021837943097_zh-CN.png)


## 授权 {#accredition .section}

1.  在容器服务管理控制台左侧导航栏中选择**集群**。

    **说明：** 请以主账号登录容器服务管理控制台进行授权操作。

2.  在集群列表页面单击目标集群的名称，查看集群的详细信息。

3.  在**集群资源**区域单击 **Worker RAM 角色**栏的链接，进入 RAM 控制台的 **RAM 角色管理**页面。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152230/156021837943098_zh-CN.png)

    若您使用的是旧版的 RAM 控制台，请切换至新版 RAM 控制台，然后重新执行[第 3 步](#step3)。切换步骤如下：

    1.  在 RAM 控制台左侧导航栏中选择**概览**页。

    2.  在**概览**页右下角单击**体验新版**。

4.  在 RAM 角色管理页面，单击**权限管理**页签下的权限策略名称，查看具体的权限策略。

5.  在策略内容页签中单击**修改策略内容**。
6.  在**修改策略内容**弹框的**策略内容**区域增加以下字段并单击**确定**。

    ```
    ,
    {
        "Action": "arms:*",
        "Resource": "*",
        "Effect": "Allow"
    }				
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152230/156021838043099_zh-CN.png)


## 部署 ARMS 应用监控 { .section}

**新建应用并部署** 

您可以根据实际选择新建应用的类型，目前仅支持无状态（Deployment）和有状态（StatefulSet）两种类型的应用接入 ARMS 应用监控。下文将以新建无状态（Deployment）类型的应用为例介绍新建应用并接入 ARMS 应用监控的操作步骤。

1.  在容器服务管理控制台左侧导航栏中选择**应用** \> **无状态** 。

2.  在无状态（Deployment）页面右上角单击**使用模板创建**。

3.  根据您的需求选择**目标集群**、**命名空间**和**示例模板**，在**模板**右侧的 yaml 文件中，添加以下字段来部署 ARMS 应用监控。

    -   将 `annotations` 添加到 yaml 文件的 spec -\> template -\> metadata 层级下。

    -   将 `<your-deployment-name>` 的内容替换为您的应用名称。

        ```
        annotations:
          armsPilotAutoEnable: "on"
          armsPilotCreateAppName: "<your-deployment-name>"								
        ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152230/156021838043103_zh-CN.png)

    示例：

    创建一个 Deployment 并接入 ARMS 应用监控的 yaml 完整示例如下：（若使用该示例请先创建 arms-demo 命名空间）

    ```language-yaml
    apiVersion: v1
    kind: Namespace
    metadata:
      name: arms-demo
    ---
    apiVersion: apps/v1beta1 # for versions before 1.8.0 use apps/v1beta1
    kind: Deployment
    metadata:
      name: arms-springboot-demo
      namespace: arms-demo
      labels:
        app: arms-springboot-demo
    spec:
      replicas: 2
      selector:
        matchLabels:
          app: arms-springboot-demo
      template:
        metadata:
          annotations:
            armsPilotAutoEnable: "on"
            armsPilotCreateAppName: "arms-k8s-demo"
          labels:
            app: arms-springboot-demo
        spec:
          containers:
            - resources:
                limits:
                  cpu: 0.5
              image: registry.cn-hangzhou.aliyuncs.com/arms-docker-repo/arms-springboot-demo:v0.1
              imagePullPolicy: Always
              name: arms-springboot-demo
              env:
                - name: SELF_INVOKE_SWITCH
                  value: "true"
                - name: COMPONENT_HOST
                  value: "arms-demo-component"
                - name: COMPONENT_PORT
                  value: "6666"
                - name: MYSQL_SERVICE_HOST
                  value: "arms-demo-mysql"
                - name: MYSQL_SERVICE_PORT
                  value: "3306"
    ---
    apiVersion: apps/v1beta1 # for versions before 1.8.0 use apps/v1beta1
    kind: Deployment
    metadata:
      name: arms-springboot-demo-subcomponent
      namespace: arms-demo
      labels:
        app: arms-springboot-demo-subcomponent
    spec:
      replicas: 2
      selector:
        matchLabels:
          app: arms-springboot-demo-subcomponent
      template:
        metadata:
          annotations:
            armsPilotAutoEnable: "on"
            armsPilotCreateAppName: "arms-k8s-demo-subcomponent"
          labels:
            app: arms-springboot-demo-subcomponent
        spec:
          containers:
            - resources:
                limits:
                  cpu: 0.5
              image: registry.cn-hangzhou.aliyuncs.com/arms-docker-repo/arms-springboot-demo:v0.1
              imagePullPolicy: Always
              name: arms-springboot-demo-subcomponent
              env:
                - name: SELF_INVOKE_SWITCH
                  value: "false"
                - name: MYSQL_SERVICE_HOST
                  value: "arms-demo-mysql"
                - name: MYSQL_SERVICE_PORT
                  value: "3306"
    ---
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        name: arms-demo-component
      name: arms-demo-component
      namespace: arms-demo
    spec:
      ports:
        # the port that this service should serve on
        - name: arms-demo-component-svc
          port: 6666
          targetPort: 8888
      # label keys and values that must match in order to receive traffic for this service
      selector:
        app: arms-springboot-demo-subcomponent
    ---
    apiVersion: apps/v1beta1 # for versions before 1.8.0 use apps/v1beta1
    kind: Deployment
    metadata:
      name: arms-demo-mysql
      namespace: arms-demo
      labels:
        app: mysql
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: mysql
      template:
        metadata:
          labels:
            app: mysql
        spec:
          containers:
            - resources:
                limits:
                  cpu: 0.5
              image: registry.cn-hangzhou.aliyuncs.com/arms-docker-repo/arms-demo-mysql:v0.1
              name: mysql
              ports:
                - containerPort: 3306
                  name: mysql
    ---
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        name: mysql
      name: arms-demo-mysql
      namespace: arms-demo
    spec:
      ports:
        # the port that this service should serve on
        - name: arms-mysql-svc
          port: 3306
          targetPort: 3306
      # label keys and values that must match in order to receive traffic for this service
      selector:
        app: mysql
    ---
    						
    ```


**部署已有应用** 

1.  在容器服务管理控制台左侧导航栏中选择**应用**，在下拉菜单中选择您的应用类型。

    **说明：** 目前仅支持无状态（Deployment）和有状态（StatefulSet）两种应用接入 ARMS 应用监控。

2.  在应用列表页选择**集群**和**命名空间**，并在目标应用右侧选择**更多** \> **查看 Yaml**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152230/156021838043106_zh-CN.png)

3.  在 yaml 文件中，添加以下字段来部署 ARMS 应用监控，并单击**更新**。

    -   将 `annotations` 添加到 yaml 文件的 spec -\> template -\> metadata 层级下。

    -   将 `<your-deployment-name>` 的内容替换为您的应用名称。

        ```
         annotations:
           armsPilotAutoEnable: "on"
           armsPilotCreateAppName: "<your-deployment-name>"									
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152230/156021838145372_zh-CN.png)


## 结果验证 { .section}

返回应用列表页，目标应用**操作**列将出现 **ARMS 控制台**项。单击 **ARMS 控制台**可跳转至 ARMS 控制台应用总览页。约一分钟后，若您的应用出现在 ARMS 控制台应用列表中且有数据上报，则说明接入成功。

**说明：** 若**操作**列没有出现 **ARMS 控制台**，请检查您是否授权容器服务调用 ARMS，请参考[授权](#accredition)。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152230/156021838143107_zh-CN.png)

