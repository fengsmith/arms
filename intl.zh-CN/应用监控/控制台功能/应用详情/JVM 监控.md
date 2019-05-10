# JVM 监控 {#concept_70073_zh .concept}

ARMS 应用监控提供 JVM 监控功能，用于监控堆内存指标、非堆内存指标、直接缓冲区指标、内存映射缓冲区指标、GC（垃圾收集）累计详情和 JVM 线程数等 JVM 指标。本文将介绍 JVM 监控功能和查看 JVM 监控指标的操作步骤。

## 功能介绍 {#section_wjv_5tg_74v .section}

ARMS 的 JVM 监控功能可以帮助您监控以下指标。

-   堆内存
    -   heap\_init：堆内存初始字节数
    -   heap\_max：堆内存最大字节数
    -   heap\_commited：堆内存提交字节数
    -   heap\_used：堆内存使用字节数
-   非堆内存
    -   non\_heap\_init：非堆内存初始字节数
    -   non\_heap\_max：非堆内存最大字节数
    -   non\_heap\_commited：非堆内存提交字节数
    -   non\_heap\_used：非堆内存使用字节数
-   直接缓冲区
    -   direct\_capacity：直接缓冲区总大小（字节）
    -   direct\_used：直接缓冲区已使用大小（字节）
-   内存映射缓冲区
    -   mapped\_capacity：内存映射缓冲区总大小（字节）
    -   mapped\_used：内存映射缓冲区已使用大小（字节）
-   GC（垃圾收集）累计详情
    -   GcPsMarkSweepCount：垃圾收集 PS MarkSweep 数量
    -   GcPsScavengeCount：垃圾收集 PS Scavenge 数量
    -   GcPsMarkSweepTime：垃圾收集 PS MarkSweep 时间
    -   GcPsScavengeTime：垃圾收集 PS Scavenge 时间
-   JVM 线程数
    -   ThreadCount：线程总数量
    -   ThreadDeadLockCount：死锁线程数量
    -   ThreadNewCount：新建线程数量
    -   ThreadBlockedCount：阻塞线程数量
    -   ThreadRunnableCount：可运行线程数量
    -   ThreadTerminatedCount：终结线程数量
    -   ThreadTimedWaitCount：限时等待线程数量
    -   ThreadWaitCount：等待中线程数量

## 操作步骤 { .section}

1.  登录 [ARMS 控制台](https://arms-ap-southeast-1.console.aliyun.com/#/home)，在左侧导航栏中选择**应用监控** \> **应用列表** 。
2.  在应用列表页面选择您想查看的应用。

3.  在应用详情页面选择您想查看的节点，并在页面右侧单击 **JVM 监控**页签。

    JVM 监控页签内展示了 GC 瞬时次数、GC 瞬时耗时、堆内存详情、非堆内存详情和 JVM 线程数的时序曲线。

    -   单击 **GC 瞬时次数/每分钟**和 **GC 瞬时耗时的瞬时值/每分钟**面板右上角的**瞬时值**和**累计值**按钮，可以切换查看 GC 瞬时次数和 GC 瞬时耗时的瞬时值或累计值的时序曲线，默认为瞬时值。
    -   单击各监控面板上的指标名称（例如 GC 瞬时次数），可打开或关闭该指标在图表中的可见性。

        **说明：** 每个图表必须至少有一个指标设为可见。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152242/155745764943130_zh-CN.png)


