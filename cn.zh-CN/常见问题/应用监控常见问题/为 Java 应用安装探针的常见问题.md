# 为 Java 应用安装探针的常见问题 {#concept_66214_zh .concept}

本文解答了关于为 Java 应用安装探针的常见问题。

## ARMS 探针和其他 APM 产品探针（比如 Pinpoint）是否兼容？ {#section_yxp_yso_ct3 .section}

ARMS 探针和其他 APM 产品探针都不兼容。 由于 APM 技术大部分都是基于 ASM 框架进行字节码插装，同时安装两个探针相当于对您的代码插装两次，由于不同商家的插装代码实现不一样，代码冲突可能造成严重的性能问题，因此强烈建议您不要同时安装多个 APM 探针。

## 挂载 Agent 后应用启动时报 OutOfMemoryError 错误 {#section_add_w4y_pgb .section}

如出现此情况，请在 Java 启动命令后面加上堆内存大小配置项，以适量调大 JVM 参数。

以堆内存初始值（Xms）512 M、堆内存最大值（Xmx）2 G 为例，配置项如下所示。请根据实际情况适量调节。对于 Tomcat 等其他环境，请在配置文件的 JAVA\_OPTS 中加入此参数。

```
   -Xms512M
   -Xmx2048M
```

如果出现`OutOfMemoryError: PermGen space`错误，加入参数：

```
   -XX:PermSize=256M 
   -XX:MaxPermSize=512M
```

## 如何检查网络连通性？ {#section_o5v_zr4_d2b .section}

部署 ARMS-APM Agent 时，可使用 Telnet 命令测试目标主机与 ARMS 服务器网络是否连通。

|地域|地址|
|--|--|
|杭州|arms-dc-hz.aliyuncs.com|
|北京|arms-dc-bj.aliyuncs.com|
|上海|arms-dc-sh.aliyuncs.com|
|青岛|arms-dc-qd.aliyuncs.com|
|深圳|arms-dc-sz.aliyuncs.com|

假设用户在 ARMS 的深圳地域创建应用，则需测试深圳地域环境与 ARMS 服务器网络是否连通。以下结果表明网络已连通：

```screen
telnet arms-dc-sz.aliyuncs.com 8443Trying 119.23.169.12...Connected to arms-dc-sz.aliyuncs.com.Escape character is '^]'.
```

**说明：** 必须根据所在地域替换服务器地址，端口不变。

## 如何检查 ArmsAgent 是否加载成功？ {#section_yq1_1s4_d2b .section}

1.  使用 ps 命令查看命令行启动参数中是否成功加载 ArmsAgent。

    ```
    ps -ef | grep 'arms-bootstrap'
    ```

    成功加载时，如下图所示：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152359/155902212843211_zh-CN.png)

2.  命令行中的 arms.licenseKey 及 arms.appId 属性必须与 ARMS 应用设置界面中显示的内容保持一致。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152359/155902212843212_zh-CN.png)


## ArmsAgent 加载成功，为什么界面仍无监控数据？ {#section_ow1_1s4_d2b .section}

1.  如果 Agent 日志中出现“send agent metrics. no metrics.”，请确认您的应用是否有持续的外部请求访问，包括 HTTP 请求、HSF 请求和 Dubbo 请求，并确认开发框架是否在 ArmsAgent 的支持范围内。关于 ARMS 应用监控对第三方组件和框架的支持情况，请参考[应用监控概述](../../../../intl.zh-CN/应用监控/应用监控概述.md#)。

2.  确认选择的查询时间范围是否正确。请您将查询时间条件设为最近 15 分钟，然后再次确认是否有监控数据。

3.  如果是通过 `-jar` 命令行启动的，请检查命令行设置，确保 -javaagent 参数在 `-jar` 之前。

    ```
    java -javaagent:/{user.workspace}/ArmsAgent/arms-bootstrap-1.7.0-SNAPSHOT.jar -Darms.licenseKey=xxx -Darms.appId=xxx -jar demoApp.jar
    ```

4.  如果 ArmsAgent/log/ 下的日志中出现 LicenseKey is invalid. 异常，请您检查应用所属地域与 Agent 所属地域是否一致。

5.  如果应用启动之后 ArmsAgent 目录下无 log 子目录，是由于 arms-bootstrap-1.7.0-SNAPSHOT.jar 未被成功加载导致的，请您检查 ArmsAgent 安装目录的权限是否正确。

6.  若应用启动时出现以下报错，请您检查 arms-bootstrap-1.7.0-SNAPSHOT.jar 软件包及相应权限是否正确。

     ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152359/155902212843213_zh-CN.png)

7.  如果仍无监控数据，请打包 Java 探针日志（路径：ArmsAgent/log），并联系钉钉服务账号 `@ARMS-服务`解决问题。

8.  检查 JDK 版本。如果 JDK 版本为 1.8.0\_25 或者 1.8.0\_31，可能会出现无法安装探针的情况，建议您升级对应的 JDK 版本，或咨询钉钉服务账号 `@ARMS-服务`。


## ArmsAgent 加载成功，但 IP 显示不正确或无显示，怎么办？ {#section_sbp_q1m_sfb .section}

1.  首先通过 `ifconfig -a` 命令确认当前机器的网络配置情况，若为多网卡环境，则 Agent 采集到的 IP 地址可能会与预期不符合（与网络配置环境有关）。
2.  任选以下方式之一来解决问题。
    -   通过显式配置 JVM 的 `-DEAGLEEYE.LOCAL.IP=10.XX.XX.XX` 参数来解决。

        **说明：** 将 `10.XX.XX.XX` 替换为实际 IP 地址。

    -   配置 Agent 获取 JVM 的 `-DNETWORK.INTERFACE=eth0` 参数，其中 `eth0`为网卡名称。

## ArmsAgent日志（路径：ArmsAgent/log）错误常用排查方法 {#section_mj5_dbm_sfb .section}

-    LicenseKey is invalid 错误

    1.  首先确保该应用在 ARMS 中创建成功，并且确认安装 Agent 时填写的 AppId 以及 LicenseKey 均正确。

    2.  由于 ARMS 产品是多地域（Region）环境，所以还需要确认 Agent 的下载地址所属的地域与应用所属地域是否一致。


