# ᴺᴱᵂ JS 错误诊断 {#concept_hzy_spn_jhb .concept}

ARMS 前端监控的 JS 错误诊断功能可展示 JS 错误的基本信息和分布情况，帮助您快速定位错误位置。

## 快速入门 {#section_qjy_fkj_yfb .section}

## 功能入口 {#section_em1_54j_fhb .section}

1.  登录 [ARMS 控制台](https://arms.console.aliyun.com/#/home)，并在页面左上角选择所需地域。
2.  在左侧导航栏中单击**前端监控**，在**前端监控**页面上单击应用名称。
3.  在左侧导航栏中选择**应用** \> **JS 错误诊断**。

## 查看错误总览 {#section_fxz_5pj_fhb .section}

**错误总览**区域可展示选定时间段内的 JS 错误基本统计信息和趋势，包括以下指标：

-   错误数：选定时间段内的 JS 错误总数。
-   JS 错误率：选定时间段内发生过错误的 PV 占总 PV 的比例。
-   影响用户数：JS 错误影响到的用户数量和比例。

![ARMS 前端监控 - JS 错误诊断 - 应用层面错误总览](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/arms/pg_bm_js_error_diag_overview.png "应用层面的错误总览")

在**错误总览**区域可执行以下操作：

-   将鼠标悬浮于曲线上，曲线拐点所对应时间点的错误数、错误率、影响用户数将显示在浮层中。

    ![ARMS 前端监控 - JS 错误诊断 - 错误曲线拐点](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/arms/sc_bm_js_error_turning_point.png)

-   将鼠标悬浮于曲线拐点上，当鼠标显示为手形指针时单击拐点，可打开该时间点的**异常洞察**对话框。

    ![ARMS 前端监控 - JS 错误诊断 - 异常洞察](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/arms/db_exception_insight.png)

-   用鼠标在曲线图上框选其中一段，即可放大查看该段曲线。在曲线图上双击即可还原视图。


**说明：** 在 JS 错误诊断页面上，默认情况下**错误总览**区域显示的是应用层面的总览信息。在**页面错误率排行**或**页面错误率 Top 5** 页签单击**分析**后，展示的是对应页面的总览信息。

![ARMS 前端监控 - JS 错误诊断 - 页面层面错误总览](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/arms/pg_bm_js_error_diag_overview_for_page.png "页面层面的错误总览")

## 查看高频错误 {#section_cxl_jl4_fhb .section}

**高频错误**页签可按出现次数从多到少的顺序展示选定时间段内的 JS 错误，包括以下指标：

-   错误信息：ARMS 捕捉到的 JS 错误内容。
-   页面：JS 错误出现的页面。
-   错误数：JS 错误出现的次数。
-   影响用户数：JS 错误影响到的用户数量和比例。

在**高频错误**页签可执行以下操作：

-   诊断：单击**操作**列中的**诊断**，进入 JS 错误诊断页面。

**说明：** 在 JS 错误诊断页面上，默认情况下**高频错误**页签显示的是应用层面的 JS 错误。在**页面错误率排行**或**页面错误率 Top 5** 页签单击**分析**后，**高频错误**页签展示的是对应页面上的 JS 错误。

## 查看页面错误率排行 {#section_avg_km4_fhb .section}

**页面错误率排行**页签可按 JS 错误率从高到低的顺序展示选定时间段内出现 JS 错误的页面，包括以下指标：

-   页面：出现过 JS 错误的页面。
-   错误率：选定时间段内在该页面发生过错误的 PV 占总 PV 的比例。
-   访问量：页面的访问量。

![ARMS 前端监控 - JS 错误诊断 - 页面错误率排行](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/arms/pg_bm_js_error_diag_overview_page_ranking.png "页面错误率排行")

在**页面错误率排行**页签可执行以下操作：

-   分析：单击**操作**列中的**分析**，进入该页面的错误总览视图。

## 查看异常洞察 {#section_cf4_wcp_fhb .section}

**异常洞察**对话框可显示具体时间点的 JS 错误情况，包括以下指标：

-   错误数：对应时间点的 JS 错误总数。
-   JS 错误率：对应时间点发生过错误的 PV 占总 PV 的比例。
-   影响用户数：JS 错误影响到的用户数量和比例。
-   高频错误 Top 5：对应时间点出现次数最多的前 5 种 JS 错误，包括 ARMS 捕捉到的 JS 错误内容、错误出现次数和影响用户数。
-   页面错误率 Top 5：对应时间点 JS 错误率最高的前 5 个页面，包括出现过 JS 错误的页面名称、页面的 JS 错误率和页面访问量。

在**异常洞察**对话框可执行以下操作：

-   单击**高频错误 Top 5** 页签，然后单击**操作**列中的**诊断**，进入 JS 错误诊断页面。
-   单击**页面错误率 Top 5** 页签，然后单击**操作**列中的**分析**，进入该页面的错误总览视图。

## 查看错误详情 {#section_k43_m2p_fhb .section}

JS 错误诊断页面的**错误详情**页签可展示具体 JS 错误的概要信息和堆栈信息。概要信息是 JS 错误的名称、发现时间等基本信息，堆栈信息是与 JS 错误出现位置有关的信息。

概要信息包括以下指标：

-   名称
-   类型
-   时间（JS 错误的发现时间）
-   设备
-   操作系统
-   浏览器
-   IP
-   地区
-   行
-   列
-   URL
-   文件：出现 JS 错误的文件路径

![ARMS 前端监控 - JS 错误诊断 - 错误详情](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/arms/pg_bm_js_error_diag_details_basic.png "JS 错误详情页面")

在**错误详情**页签上可执行以下操作：

-   如需确定 JS 错误的准确出错位置，请在**堆栈信息**区域单击一条堆栈信息左侧的三角形图标展开该行，单击**选择 Source Map**，然后在 Source Map 文件对话框中选择现有的 Source Map 文件或上传新的 Source Map 文件，最后单击确定。

    ![ARMS 前端监控 - JS 错误诊断 - Source Map 对话框](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/arms/db_source_map_select.png)

    ARMS 将利用 Source Map 文件还原准确的 JS 错误位置。

    ![ARMS 前端监控 - JS 错误诊断 - ARMS 还原准确出错位置](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/arms/sc_bm_js_error_diag_details_stack_mapped.png)

-   如需查看该错误的分布情况，请单击**错误分布**页签。


## 查看错误分布 {#section_ajg_bgp_fhb .section}

JS 错误诊断页面的**错误分布**页签可展示具体 JS 错误的分布情况，统计维度包括：

-   时间：最近 24 小时和最近 30 天。
-   浏览器
-   操作系统
-   设备
-   地理：在中国维度下按省、直辖市、自治区统计，在世界维度下按国家/地区统计。

![ARMS 前端监控 - JS 错误诊断 - 错误分布](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/arms/pg_bm_js_error_diag_distribution_basic.png "JS 错误分布页面")

在**错误分布**页签上可执行以下操作：

-   在**时间分布**区域，将鼠标悬浮于曲线上，曲线拐点所对应时间点的错误数将显示在浮层中。

-   在**地理分布**区域的**中国**或**世界**页签上，单击右侧表格中的**错误数**列标题，可切换表格的排序顺序（从正序切换为倒序，或从倒序切换为正序）。

    ![ARMS 前端监控 - JS 错误诊断 - 地理分布](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/arms/sc_bm_js_error_distribution_geo_china.png)


