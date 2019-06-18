# API 请求监控 {#concept_91587_zh .concept}

本文介绍了前端监控中的 API 请求监控功能。

阿里云 ARMS 前端监控的 API 请求监控模块，可清晰展示以下信息：

-   每个 API 的成功率
-   API 返回信息
-   API 接口的调用成功平均耗时
-   API 接口的调用失败平均耗时

此外，该模块还会展示上述统计数据在以下维度上的分布情况。

-   地理位置
-   浏览器
-   操作系统
-   设备
-   分辨率

## API 成功率 { .section}

左侧的成功率标签页展示的是 API 成功率排行。右侧图表展示的是左侧列表选中 API 在指定时间范围内的 API 调用量和成功率曲线。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152276/155496246243657_zh-CN.png)

## API 返回信息 { .section}

左侧的 Msg聚类标签页展示的是 API 返回信息排行。右侧的 Msg 调用详情区域展示的是左侧列表选中返回信息的 API 调用列表，按调用量降序排列。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152276/155496246243661_zh-CN.png)

## API 成功耗时 { .section}

左侧的成功耗时标签页展示的是 API 调用成功时的平均耗时。右侧的 API 成功耗时区域展示的是左侧列表选中 API 的调用成功平均耗时曲线和调用成功次数柱形图。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152276/155496246243666_zh-CN.png)

## API 失败耗时 { .section}

左侧的失败耗时标签页展示的是 API 调用失败时的平均耗时。右侧的 API 失败耗时区域展示的是左侧列表选中 API 的调用失败平均耗时曲线和调用失败次数柱形图。

![](http://aliware-images.oss-cn-hangzhou.aliyuncs.com/arms/tab_api_response_time_failure.png)

## 地理分布 { .section}

在**地理分布**模块，您可以查看上述统计信息的地理分布情况。地理分布又分为中国和世界两个维度，中国维度的单位为省，世界维度的单位为国家/地区。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152274/155496246343635_zh-CN.png)

## 终端分布 { .section}

在**终端分布**模块中，您可以查看上述统计信息的终端分布情况。终端分布又细分为浏览器、操作系统、设备、分辨率等维度。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152274/155496246343638_zh-CN.png)

## 通用操作 { .section}

在 JS 错误统计模块中，您可以执行以下通用操作。

-   在左侧标签页上，单击标签上的上箭头或下箭头来改变列表的排列顺序。上箭头表示升序，下箭头表示降序。
-   在右侧的详情显示区域中，单击右上角的列表图标，可在图表和表格视图间切换。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152274/155496246343639_zh-CN.png)

-   在右侧的详情显示区域中，单击右上角的时钟图标，可指定时间范围。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152274/155496246343644_zh-CN.png)


