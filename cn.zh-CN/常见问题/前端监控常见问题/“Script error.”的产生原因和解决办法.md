# “Script error.”的产生原因和解决办法 {#concept_88579_zh .concept}

“Script error.”是一个常见错误，但由于该错误不提供完整的报错信息（错误堆栈），问题排查往往无从下手。本文提出了该错误的产生原因和解决办法，以及在 ARMS 中忽略该错误的方法。

## “Script error.”的产生原因 {#section_bc1_yzy_gfb .section}

“Script error.”有时也被称为跨域错误。当网站请求并执行一个托管在第三方域名下的脚本时，就可能遇到该错误。最常见的情形是使用 CDN 托管 JS 资源。

为了更好地理解，假设以下 HTML 页面部署在 http://test.com 域名下：

```
<!doctype html>
<html>
<head>
  <title>Test page in http://test.com</title>
</head>
<body>
  <script src="http://another-domain.com/app.js"></script>
  <script>
  window.onerror = function (message, url, line, column, error) {
    console.log(message, url, line, column, error);
  }
  foo(); // 调用app.js中定义的foo方法
  </script>
</body>
</html>
```

假设 `foo` 方法调用了一个未定义的 `bar` 方法：

```
// another-domain.com/app.js
function foo() {
  bar(); // ReferenceError: bar is not a function
}
```

页面运行之后，捕获到的异常信息如下：

```
"Script error.", "", 0, 0, undefined 
```

其实这并不是一个 JavaScript Bug。出于安全考虑，浏览器会刻意隐藏其他域的 JS 文件抛出的具体错误信息，这样做可以有效避免敏感信息无意中被不受控制的第三方脚本捕获。因此，浏览器只允许同域下的脚本捕获具体错误信息，而其他脚本只知道发生了一个错误，但无法获知错误的具体内容。

请参考 [Webkit 源码](https://trac.webkit.org/browser/branches/chromium/648/Source/WebCore/dom/ScriptExecutionContext.cpp)：

```
bool ScriptExecutionContext::sanitizeScriptError(String& errorMessage, int& lineNumber, String& sourceURL)
    {
        KURL targetURL = completeURL(sourceURL);
        if (securityOrigin()->canRequest(targetURL))
            return false;
        errorMessage = "Script error.";
        sourceURL = String();
        lineNumber = 0;
        return true;
    }
```

了解了 “Script error.”的产生原因之后，接下来看看如何解决这个问题。

## 解法 1：开启 CORS（Cross Origin Resource Sharing，跨域资源共享） { .section}

为了跨域捕获 JavaScript 异常，可执行以下两个步骤：

1.  添加 `crossorigin="anonymous"` 属性。

    ```
    <script src="http://another-domain.com/app.js" crossorigin="anonymous"></script>
    ```

    此步骤的作用是告知浏览器以匿名方式获取目标脚本。这意味着请求脚本时不会向服务端发送潜在的用户身份信息（例如 Cookies、HTTP 证书等）。

2.  添加跨域 HTTP 响应头。

    ```
    Access-Control-Allow-Origin: * 
    ```

    或者

    ```
    Access-Control-Allow-Origin: http://test.com 
    ```

    **说明：** 大部分主流 CDN 默认添加了 `Access-Control-Allow-Origin` 属性。以下是阿里 CDN 的示例：

    ```
    $ curl --head https://retcode.alicdn.com/retcode/bl.js | grep -i "access-control-allow-origin"
    => access-control-allow-origin: *
    ```


完成上述两步之后，即可通过 `window.onerror` 捕获跨域脚本的报错信息。回到之前的案例，页面重新运行后，捕获到的结果是：

```
=> "ReferenceError: bar is not defined", "http://another-domain.com/app.js", 2, 1, [Object Error]
```

## 解法 2（可选）：`try catch`  { .section}

难以在 HTTP 请求响应头中添加跨域属性时，还可以考虑 `try catch` 这个备选方案。

在之前的示例 HTML 页面中加入 `try catch`：

```
<!doctype html>
<html>
<head>
  <title>Test page in http://test.com</title>
</head>
<body>
  <script src="http://another-domain.com/app.js"></script>
  <script>
  window.onerror = function (message, url, line, column, error) {
    console.log(message, url, line, column, error);
  }
  try {
    foo(); // 调用app.js中定义的foo方法
  } catch (e) {
    console.log(e);
    throw e;
  }
  </script>
</body>
</html>
```

再次运行，输出结果如下：

```
=> ReferenceError: bar is not defined
     at foo (http://another-domain.com/app.js:2:3)
     at http://test.com/:15:3
=> "Script error.", "", 0, 0, undefined
```

可见 `try catch` 中的 Console 语句输出了完整的信息，但 `window.onerror` 中只能捕获“Script error”。根据这个特点，可以在 catch 语句中手动上报捕获的异常。

```
// 参考本文末尾的相关文档“前端监控 API 使用指南”
__bl.error(error, pos);
```

**说明：** 尽管 `try catch` 方法可以捕获部分异常，但推荐采用解法 1。

## 如何在 ARMS 中忽略“Script error.”？ {#section_ujs_hz3_fhb .section}

“Script error.”往往来源于第三域名下的脚本，如果不影响应用的运行，您可以选择通过 ARMS 前端监控的 SDK 参数 ignore 忽略该错误。

`ignore` 可忽略指定 JS 错误的上报，其配置项的值是一个对象，包含 3 个属性：ignoreUrls、ignoreApis 和 ignoreErrors。默认值如下：

```
ignore: {
    ignoreUrls: [],
    ignoreApis: [],
    ignoreErrors: []
},
```

针对“Script error.”，使用 ignoreErrors 即可。ignoreErrors 表示忽略某些 JS 错误，符合该规则的 JS 错误不会上报。值可以是 String、RegExp、Function 或者以上三种类型组成的数组。示例：

```
__bl.setConfig({
    ignore: {
        ignoreErrors: /^Script error\.?$/
    }
});
```

## 更多信息 { .section}

-   [../../../../../dita-oss-bucket/SP\_177/DNARMS19100476/ZH-CN\_TP\_152281.md\#](../../../../../intl.zh-CN/前端监控/高级选项/API 使用指南.md#)
-   [../../../../../dita-oss-bucket/SP\_177/DNARMS19100476/ZH-CN\_TP\_152280.md\#](../../../../../intl.zh-CN/前端监控/高级选项/前端监控 SDK 配置项.md#)

