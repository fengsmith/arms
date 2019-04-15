# 为开源 Kubernetes 环境中的应用安装探针 {#concept_103108_zh .concept}

借助 ARMS 应用监控，您可以对开源 Kubernetes 环境的应用进行应用拓扑、接口调用、异常事务和慢事务监控、SQL 分析等监控。本文将帮助您将开源 Kubernetes 环境中的应用接入 ARMS 应用监控。

## 前提条件 {#section_0b4_69s_m9c .section}

-   确保您的 Kubernetes api-server 组件接口版本在 1.10 及以上。
-   确保您的集群联通公网。
-   您已成功开通 ARMS 服务，参见[开通 ARMS 服务](../../../../../intl.zh-CN/快速入门/开通 ARMS 服务.md#)。

## 操作步骤 { .section}

ARMS 应用监控目前仅支持无状态（Deployment）和有状态（StatefulSet）两种类型的应用接入。下文将以无状态（Deployment）类型的应用为例介绍将开源 Kubernetes 环境中应用接入 ARMS 应用监控的操作步骤。

1.  安装 arms-pilot。

    1.  采用以下方法之一下载 arms-pilot。

        -   方法一：手动下载[最新安装包](http://arms-apm-hangzhou.oss-cn-hangzhou.aliyuncs.com/arms-pilot/arms-pilot-0.1.1-community.tgz)。

        -   方法2： 执行以下 wget 命令下载 arms-pilot 安装包。

            ```
            wget 'http://arms-apm-hangzhou.oss-cn-hangzhou.aliyuncs.com/arms-pilot/arms-pilot-0.1.1-community.tgz' -O arms-pilot-0.1.1.tgz
            									
            ```

    2.  解压 arms-pilot 安装包。

        ```
        tar zxvf arms-pilot-0.1.1.tgz 
        							
        ```

    3.  执行以下命令安装 arms-pilot。

        ```
        helm install ./arms-pilot --namespace arms-pilot-system
        							
        ```

2.  获取 ARMS 的 LicenseKey。参见[获取 LicenseKey](intl.zh-CN/应用监控/开始监控 Java 应用/以普通方式安装探针/为 Java 应用安装探针.md#step1)。

3.  修改目标 Deployment 应用的 yaml 文件。

    1.  执行以下命令查看目标 Deployment 应用的配置。

        ```
        ### 查看指定 Deployment 类型应用的配置
        kubectl get deployment {deployment名} -o yaml
        							
        ```

        **说明：** 若您不清楚 `{deployment 名}`，先执行以下命令查看所有 Deployment 应用，在执行结果中找到目标 deployment 应用，再查看目标 deployment 应用的配置。

        ```
        ### 查看所有 deployment 类型应用的配置
        kubectl get deployments --all-namespace
        								
        ```

    2.  启动编辑目标 deployment 应用的 yaml 文件。

        ```
        kubectl edit deployment {deployment名} -o yaml
        							
        ```

    3.  在 yaml 文件中的 **spec -\> template -\> metadata -\> labels** 下加入以下内容。

        **说明：** 

        -   请将 `xxx` 分别替换成您的 `licenseKey` 和应用名称，应用名暂不支持中文。

        -   将 `licenseKey` 中的符号 `@` 替换为符号 `_` 。

        ```
        ARMSApmAppName: xxx
        ARMSApmLicenseKey: xxx
        										
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152231/155529388143108_zh-CN.png)

        **示例：**

        在开源环境中创建一个 Deployment 应用并接入 ARMS 应用监控的完整 yaml 文件如下。

        ```language-yaml
        apiVersion: apps/v1beta1 # for versions before 1.8.0 use apps/v1beta1
        kind: Deployment
        metadata:
          name: arms-springboot-demo
          labels:
            app: arms-springboot-demo
        spec:
          replicas: 2
          selector:
            matchLabels:
              app: arms-springboot-demo
          template:
            metadata:
              labels:
                app: arms-springboot-demo
                ARMSApmLicenseKey: "xxx_xxx"
                ARMSApmAppName: "arms-k8s-demo"
            spec:
              containers:
                - resources:
                    limits:
                      cpu: 0.5
                  image: registry.cn-hangzhou.aliyuncs.com/arms-docker-repo/arms-springboot-demo:v0.1
                  imagePullPolicy: Always
                  name: arms-springboot-demo
                  env:
                    - name: MYSQL_SERVICE_HOST
                      value: "arms-demo-mysql"
                    - name: MYSQL_SERVICE_PORT
                      value: "3306"
        ---
        apiVersion: apps/v1beta1 # for versions before 1.8.0 use apps/v1beta1
        kind: Deployment
        metadata:
          name: arms-demo-mysql
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

    4.  保存配置。

        应用将自动重启，以上配置生效。


2 ~ 5 分钟后，若您的应用出现在 ARMS 控制台的**应用监控** \> **应用列表**中且有数据上报，则说明接入成功。

## 卸载 arms-pilot { .section}

若您不再需要监控开源 Kubernetes 环境的应用，可执行以下命令卸载 arms-pilot。

```
helm del --purge arms-pilot
```

## 相关文档 { .section}

 [为开源 Kubernetes 环境中的应用安装探针常见问题](../../../../../intl.zh-CN/常见问题/应用监控常见问题/为开源 Kubernetes 环境中的应用安装探针常见问题.md#)

