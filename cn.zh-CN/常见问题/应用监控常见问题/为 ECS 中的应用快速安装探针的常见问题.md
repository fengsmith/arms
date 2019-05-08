# 为 ECS 中的应用快速安装探针的常见问题 {#concept_109300_zh .concept}

本文解答了为 ECS 中的应用快速安装探针的常见问题。

## 探针安装失败怎么处理？ {#section_wqk_sxc_zgb .section}

1.  确保您的 ECS 可以访问所在地域的探针下载链接。

    ```
    首先确保 ECS 可以访问外网，且能够访问所在地域的探针下载链接。
    
    # 杭州地域
    http://arms-apm-hangzhou.oss-cn-hangzhou.aliyuncs.com/install.sh
    # 上海地域
    http://arms-apm-shanghai.oss-cn-shanghai.aliyuncs.com/install.sh
    # 青岛地域
    http://arms-apm-qingdao.oss-cn-qingdao.aliyuncs.com/install.sh
    # 北京地域
    http://arms-apm-beijing.oss-cn-beijing.aliyuncs.com/install.sh
    # 深圳地域
    http://arms-apm-shenzhen.oss-cn-shenzhen.aliyuncs.com/install.sh
    # 新加坡地域
    http://arms-apm-ap-southeast.oss-ap-southeast-1.aliyuncs.com/cloud_ap-southeast-1/install.sh
    ```

2.  确保您的 ECS 可以访问控制台。

    ```
    #国内
    https://arms.console.aliyun.com/
    #新加坡
    https://arms-ap-southeast-1.console.aliyun.com
    ```

3.  登录 [ECS 控制台](https://ecs.console.aliyun.com/#/home)，并完成以下检查工作。
    1.  在左侧导航栏中选择**云助手**。
    2.  在云助手页面的搜索框中选择命令输入 `InstallJavaAgent` 命令。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152362/155728368443214_zh-CN.png)

        **说明：** 若查找结果不存在，请联系 ARMS 服务账号。

    3.  在云助手页面的**执行记录**区域的搜索栏中输入 `InstallJavaAgent` 命令对应的 id，在查找结果中单击该记录右侧**操作**列的**查看结果**，查看 `InstallJavaAgent` 命令是否执行成功。若未执行成功，根据细执行结果排查问题（如 ECS 磁盘满、未安装 探针等，可以通过清理磁盘或安装探针来解决），若不能解决请将详细执行结果反馈给 ARMS 服务账号。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152362/155728368443215_zh-CN.png)


## 安装探针后 ECS 实例进程信息不准确 { .section}

若您在 ECS 上成功安装探针后，没有出现 ECS 实例进程信息，或者 ECS 实例进程信息不准确，请单击 ECS 实例左侧的 **-** 号之后点击 **+** 号刷新数据。若不能解决请联系 ARMS 服务账号协助排查。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152362/155728368443217_zh-CN.png)

## ECS 中的进程启动应用监控失败怎么处理？ {#section_czz_vr2_zgb .section}

请检查 ECS 上 `/root/.arms/supervisor/logs/arms-supervisor.log` 日志中是否有日志级别为 Error 的信息，根据信息排查。若不能解决请联系 ARMS 服务账号协助排查。

