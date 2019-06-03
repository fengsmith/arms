# 前端监控 SDK 配置项 {#concept_58655_zh .concept}

ARMS 前端监控提供一系列 SDK 配置项，让您能够通过设置不同的参数来满足不同应用场景的需求。

可通过以下两种方式使用前端监控 SDK 配置项：

-   在向页面埋入 BI 探针时，在 **config** 中添加额外参数。

    例如，以下示例代码的 **config** 中，除了默认的 **pid** 参数外，还添加了 **enableSPA** 参数，用于单页面应用（Single Page Application）的场景。

    ``` {#codeblock_zyo_bg7_cdt}
    <script>
    !(function(c,b,d,a){c[a]||(c[a]={});c[a].config={pid:"xxxxxx",enableSPA:true};
    with(b)with(body)with(insertBefore(createElement("script"),firstChild))setAttribute("crossorigin","",src=d)
    })(window,document,"https://retcode.alicdn.com/retcode/bl.js","__bl");
    </script>
    				
    ```

-   在页面初始化完成之后，在前端 JS 代码中调用**setConfig** 方法来修改。关于具体方法，请参考[API 使用指南](intl.zh-CN/前端监控/高级选项/API 使用指南.md#)。


## SDK 配置项参数 {#section_39l_g0m_a2p .section}

|参数名|类型|描述|是否必须|默认值|
|---|--|--|----|---|
|pid|String|项目唯一 ID，由 ARMS 在创建站点时自动生成|是|无|
|tag|String|传入的标记，每条日志都会携带该标记|否|无|
|page|String|页面名称，默认取当前页面 URL 的关键部分|否|`host + pathname`|
|enableSPA|Boolean|是否监听页面的 hashchange 事件并重新上报 PV，适用于单页面应用场景|否|`false`|
|parseHash|Function|配合 enableSPA 使用，详情见“部分设置项详细说明”。|否|见“部分设置项详细说明”。|
|disableHook|Boolean|是否禁用 AJAX 请求监听，默认会监听并用于 API 调用成功率上报|否|`false`|
|ignoreUrlCase|Boolean|是否忽略 Page URL 大小写，默认为忽略|否|`true`|
|urlHelper|\*|URL 过滤规则，详情见“部分设置项详细说明”。|否|见“部分设置项详细说明”。|
|apiHelper|\*|API 过滤规则，详情见“部分设置项详细说明”。|否|见“部分设置项详细说明”。|
|ignore|Object|忽略指定 URL/API/JS Error。符合规则的日志将被忽略，不会上报，包含子配置项 `ignoreUrls`、`ignoreApis` 和 `ignoreErrors`。|否|见“部分设置项详细说明”。|
|disabled|Boolean|禁用日志上报功能|否|`false`|
|sample|Integer|日志采样配置，值为 `1`、`10` 或 `100`。性能和成功 API 日志按照 `1/sample`。详情见“部分设置项详细说明”。|否|见“部分设置项详细说明”。|
|sendResource|Boolean|是否上报页面静态资源，详情见“部分设置项详细说明”。|否|`false`|
|useFmp|Boolean|是否采集首屏 FMP（First Meaningful Paint，首次有效渲染）数据。|否|`false`|
|enableLinkTrace|Boolean|是否允许进行前后端链路追踪。详情见[前后端链路追踪](intl.zh-CN/前端监控/控制台功能/前后端链路追踪.md#)。|否|`false`|
|release|String|应用版本号。|否|`undefined`|
|enviroment|String|设置 tarcing 的环境，默认为 `production`。|否|`production`|

## 部分设置项详细说明 {#section_fbe_il5_mi0 .section}

1.  **`parseHash` 将 URL hash 解析为 page 的方法**

    单页面应用场景中（参考[进阶场景](intl.zh-CN/前端监控/高级选项/进阶场景.md#)），在 **enableSPA** 设为 true 的前提下，页面触发 hashchange 事件时，此参数用于将 URL hash 解析为 page 字段的方法。默认值是一个简单的字符串处理方法：

    ``` {#codeblock_swn_50y_bt5}
    function (hash) {
        var page = hash ? hash.replace(/^#/, '').replace(/\?.*$/, '') : '';
        return page || '[index]';
    }
    					
    ```

    此项一般情况下不需要修改。如果需要在上报时使用自定义的页面名，或者 URL 的 hash 比较复杂，则需要修改此配置项。例如：

    ``` {#codeblock_w9z_de8_bm5}
    // 定义页面 hash 和 page 的映射关系
    var PAGE_MAP = {
        '/': '首页',
        '/contact': '联系我们',
        '/list': '数据列表',
        // ...
    };
    // 页面 onload 后调用 SDK 方法
    window.addEventListener('load', function (e) {
        // 调用 setConfig 方法修改 SDK 配置项
        __bl.setConfig({
            parseHash: function (hash) {
                key = hash.replace(/\?.*$/, '');
                return PAGE_MAP[key] || '未知页面';
            }
        });
    });
    					
    ```

2.  **`urlHelper` URL 过滤规则**

    在页面 URL 类似于 `http://xxx.com/projects/123456` 的场景中（projects 后面紧跟的是项目 ID），如果将`xxx.com/projects/123456` 作为 page 上报，会导致在数据查看时页面无法聚成一类，所以需要过滤掉这些非关键字符。

    **说明：** 表示 URL 过滤规则的旧字段为 `ignoreUrlPath`，现已废弃，请使用新字段 `urlHelper`。若继续使用旧字段未使用新字段，配置仍然生效。若同时使用新旧字段，则新字段的配置生效。

    **生效场景**

    此设置项只在自动获取页面 URL 作为 page 时才会生效。如果手动调用 `setPage` 或 `setConfig` 方法（参考[API 使用指南](intl.zh-CN/前端监控/高级选项/API 使用指南.md#)）修改过 page，或者 `enableSPA` 已设为 `true`，则此设置项无效。

    默认值是一个数组，**一般情况下不需要修改**。

    ``` {#codeblock_0vk_fet_8rp}
    [
        // 将所有 path 中的数字变成 *
        {rule: /\/([a-z\-_]+)?\d{2,20}/g, target: '/$1**'},
        // 去掉 url 末尾的'/'
        /\/$/
    ]
    					
    ```

    此设置项的默认值会过滤掉类似于 `xxxx/123456` 后面的数字，例如 `xxxx/00001` 和`xxxx/00002` 都会变成 `xxxx/**`。

    `ignoreUrlPath` 的值可以是以下多种类型，用法分别为：

    -   `String` 或 `RegExp`：将匹配到的字符串去掉。关于 `RegExp` 请参考正则表达式的语法；
    -   `Object<rule, target>`：对象包含两个 key，分别是 rule 和 target，作为 JS 字符串的 `replace` 方法的入参，请参考 JS 相关教程中的 `String::replace` 方法；
    -   `Function`：将原字符串作为入参执行方法，将执行结果作为 page；
    -   `Array`：用于设置多条规则，每条子规则都可以是上述类型之一。
3.  **`apiHelper` API 过滤规则**

    用于在自动上报 API 时过滤掉接口 URL 中的非关键字符，用法及含义同 `ignoreUrlPath`。

    默认值是一个对象，一般情况下不需要修改：

    ``` {#codeblock_8xh_vu6_ns2}
    {rule: /(\w+)\/\d{2,}/g, target: '$1'}
    					
    ```

    此设置项的默认值会过滤掉接口 URL 中类似 `xxxx/123456` 后面的数字。

    **说明：** 表示 API 过滤规则的旧字段为 `ignoreApiPath`，现已废弃，请使用新字段 `apiHelper`。若继续使用旧字段未使用新字段，配置仍然生效；若同时使用新旧字段，则新字段的配置生效。

4.  **`config.parseResponse` Response 解析方法**

    该方法用于解析自动上报 API 时返回的数据。默认值为：

    ``` {#codeblock_tef_3r3_582}
    function (res) {
        if (!res || typeof res !== 'object') return {};
        var code = res.code;
        var msg = res.msg || res.message || res.subMsg || res.errorMsg || res.ret || res.errorResponse || '';
        if (typeof msg === 'object') {
            code = code || msg.code;
            msg = msg.msg || msg.message || msg.info || msg.ret || JSON.stringify(msg);
        }
        return {msg: msg, code: code, success: true};
    }
    					
    ```

    以上代码会解析返回的数据，尝试提取出 msg 和 code。对于一般应用来说，此设置项不需要修改。如果默认值无法满足业务需求，则可以重新设置。

5.  **`config.sample` 抽样上报配置**

    该配置通过随机抽样上报来减小用户上报量并降低负载。

    该配置会对**性能**和**成功 API** 日志随机抽样上报，后台日志处理会根据对应的抽样配置进行还原，不会影响 JS Error 和失败 API 等其他类型日志的上报。

    抽样值 `sample` 默认为 `1`，可选 `1`、`10` 或 `100`，对应的抽样率为 `1/sample`，即 `1` 表示 `100% 采样`，`10` 表示 `10% 采样`，`100` 表示 `1% 采样`。

    由于随机抽样会与实际监控结果有一定误差，建议日均 PV 在 100 万以上的站点采用抽样。

    **Note:** 如果用户原上报量较小，该配置可能会造成较大的统计结果误差。

6.  **`sendResource` 上报页面加载的静态资源**

    在页面 load 时会根据该项配置判断是否要上报页面加载的静态资源，所以请直接使用在 config 配置 sendResource 的方式。而 setConfig 的方式可能在页面 load 后才会触发，从而配置的 sendResource 会无效。使用方式如下：

    ``` {#codeblock_1z3_ndw_r2u}
    <script>
    !(function(c,b,d,a){c[a]||(c[a]={});c[a].config={pid:"xxxxxx",sendResource:true};
    with(b)with(body)with(insertBefore(createElement("script"),firstChild))setAttribute("crossorigin","",src=d)
    })(window,document,"https://retcode.alicdn.com/retcode/bl.js","__bl");
    </script>
    					
    ```

    sendResource 配置为 true 后，在页面 onload 时会上报当前页面加载的静态资源信息。如果发现页面加载较慢，可以在会话追踪页面，点击查看本次页面加载的静态资源瀑布图，判断页面加载较慢的具体原因。

7.  **`ignore` 忽略指定 URL、API、JS Error 的上报**

    `ignore`配置项的值是一个对象，对象中包含 3 个属性：`ignoreUrls`、`ignoreApis` 和 `ignoreErrors`。可单独设置其中的 1 个或多个属性。默认值如下:

    ``` {#codeblock_1ji_xyc_voi}
    ignore: {
            ignoreUrls: [],
            ignoreApis: [],
            ignoreErrors: []
        },
    					
    ```

    `ignoreUrls`表示忽略某些URL, 命中之后该URL下日志都不会上报, 值可以是`String`、`RegExp`、`Function`或者以上前面三种类型组成的数组, 参考示例如下:

    ``` {#codeblock_n3t_nrl_hsj}
    __bl.setConfig({
                    ignore: {
                        ignoreUrls: [
                        'http://host1/',  // 字符串
                        /.+?host2.+/,     // 正则表达式
                        function(str) {   // 方法
                            if (str && str.indexOf('host3') >= 0) {
                                return true;
                            }
                            return false;
                        }]
                    }
                });
    					
    ```

    `ignoreApis` 表示忽略某些API, 命中之后该API将不会被监控, 值可以是`String`、`RegExp`、`Function`或者以上前面三种类型组成的数组, 使用方式与`ignoreUrls`类似, 参考示例如下:

    ``` {#codeblock_4gc_dns_rk3}
    __bl.setConfig({
                    ignore: {
                        ignoreApis: ['api1','api2','api3', /^random/, function(str) {
                            if (str && str.indexOf('api3') >= 0) return true;
                            return false;
                        }]
                    }
                });
    					
    ```

    `ignoreErrors` 表示忽略某些JS错误, 值可以是`String`、`RegExp`、`Function`或者以上前面三种类型组成的数组, 使用方式与`ignoreUrls`、`ignoreApis`类似, 参考示例如下:

    ``` {#codeblock_nia_t8m_ul9}
    __bl.setConfig({
                    ignore: {
                        ignoreErrors: ['test error', /^Script error\.?$/, function(str) {
                            if (str && str.indexOf('Unkown error') >= 0) return true;
                            return false;
                        }]
                    }
                });
    					
    ```


