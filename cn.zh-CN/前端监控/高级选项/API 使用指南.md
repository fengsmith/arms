# API 使用指南 {#concept_58657_zh .concept}

本文介绍了前端监控 SDK 的一些接口及其使用场景。

SDK 开放了部分数据上报接口，用户可在页面中自行调用来上报更多数据。

## api\(\) 接口调用成功率上报 { .section}

此接口用于上报页面的 API 调用成功率，SDK 默认会监听页面的 AJAX 请求并调用此接口上报。 如果页面的数据请求方式是 JSONP 或者其他自定义方法（例如客户端 SDK 等），可以在数据请求方法中调用`api()`方法手动上报。

**说明：** 如果要调用此接口，建议在 SDK 配置项中将`disabledHook`设置为`true`，具体配置参见 [前端监控 SDK 配置项](intl.zh-CN/前端监控/高级选项/前端监控 SDK 配置项.md#)。

 **调用参数说明：** `__bl.api(api, success, time, code, msg)` 

|参数|类型|描述|是否必须|默认值|
|--|--|--|----|---|
|api|String|接口名|是|-|
|success|Boolean|是否调用成功|是|-|
|time|Number|接口耗时|是|-|
|code|String/Number|返回码|否|''|
|msg|String|返回信息|否|''|

 **示例** 

```language-javascript
var begin = Date.now(),
    url = '/data/getTodoList.json';

$.ajax({
    url: url,
    data: {id: 123456}
}).done(function (result) {
    var time = Date.now() - begin;
    // 上报接口调用成功
    window.__bl && __bl.api(url, true, time, result.code, result.msg);
    // do something ....
}).fail(function (error) {
    var time = Date.now() - begin;
    // 上报接口调用失败
    window.__bl && __bl.api(url, false, time, 'ERROR', error.message);
    // do something ...
});

```

## error\(\) 错误信息上报 { .section}

此接口用于上报页面中的 JS 错误或使用者想关注的异常。

一般情况下，SDK 会监听页面全局的 Error 并调用此接口上报异常信息，但由于浏览器的同源策略往往无法获取错误的具体信息，此时就需要使用者手动上报。

 **调用参数说明：** `__bl.error(error, pos)` 

|参数|类型|描述|是否必须|默认值|
|--|--|--|----|---|
|error|Error|JS 的 Error 对象|是|-|
|pos|Object|错误发生的位置，包含以下3个属性|否|-|
|pos.filename|String|错误发生的文件名|否|-|
|pos.lineno|Number|错误发生的行数|否|-|
|pos.colno|Number|错误发生的列数|否|-|

 **示例1：监听页面的 JS Error 并上报** 

```language-js
window.addEventListener('error', function (ex) {
    // 一般事件的参数中会包含pos信息
    window.__bl && __bl.error(ex.error, ex);
});

```

 **示例2：上报一个自定义的错误信息** 

```language-js
window.__bl && __bl.error(new Error('发生了一个自定义的错误'), {
    filename: 'app.js', 
    lineno: 10, 
    colno: 15
});

```

## sum\(\) 求和统计 { .section}

此接口用于统计业务中某些事件发生的次数。

 **调用参数说明：** `__bl.sum(key, value)` 

|参数|类型|描述|是否必须|默认值|
|--|--|--|----|---|
|key|String|事件名|是|-|
|value|Number|单次累加上报量，默认 1|否|1|

 **示例** 

```language-js
__bl.sum('event-a');

__bl.sum('event-b', 3);

__bl.sum('group-x::event-c', 2);

```

## avg\(\) 求平均统计 { .section}

此接口用于统计业务场景中某些事件发生的平均次数或平均值。

 **调用参数说明：** `__bl.avg(key, value)` 

|参数|类型|描述|是否必须|默认值|
|--|--|--|----|---|
|key|String|事件名|是|-|
|value|Number|统计上报量，默认 0|否|0|

 **示例** 

```language-js
__bl.avg('event-a', 1);
__bl.avg('event-b', 3);

__bl.avg('events::event-c', 10);
__bl.avg('speed::event-d', 142.42);

```

## 其它接口 { .section}

非日志上报接口，一般用于修改 SDK 的部分设置项。

## setConfig\(\) 修改配置项 { .section}

用于在 SDK 初始完成后重新修改部分配置项，具体配置参见 [前端监控 SDK 配置项](intl.zh-CN/前端监控/高级选项/前端监控 SDK 配置项.md#)。

 **调用参数说明：** `__bl.setConfig(next)` 

|参数|类型|描述|是否必须|默认值|
|--|--|--|----|---|
|next|Object|需要修改的配置项以及值|是|-|

 **示例：修改 disableHook 禁用 API 自动上报** 

```language-js
__bl.setConfig({
    disableHook: true
});

```

## setPage\(\) 设置当前页面的 page name { .section}

用于重新设置页面的 page name（默认会触发重新上报 PV）。此接口一般用于单页面应用，更多信息请参考[进阶场景](intl.zh-CN/前端监控/高级选项/进阶场景.md#)。

 **调用参数说明：** `__bl.setPage(next, sendPv)` 

|参数|类型|描述|是否必须|默认值|
|--|--|--|----|---|
|page|String|新的 page name|是|-|
|sendPv|Boolean|是否上报 PV，默认会上报|否| `true` |

示例 1 ：设置当前页面的 page name 为当前的 URL hash，并重新上报 PV

```language-javascript
__bl.setPage(location.hash);

```

示例 2 ：仅设置当前页面的 page 为'homepage'，但不触发 PV 上报

```language-javascript
__bl.setPage('homepage', false);

```

