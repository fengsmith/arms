# 自定义切分中 if/assign 的语法介绍 {#concept_52282_zh .concept}

ARMS 默认支持 Aviator 表达式引擎，您可以完成对数据处理时的逻辑控制、赋值操作等。

## 条件判断 if-else {#section_vzd_5xx_gfb .section}

在自定义切分标签页的左侧操作面板中选择**逻辑**，系统提供两种类型的逻辑判断积木块“if/else”和“if”，如下图所示:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152321/155496243644128_zh-CN.png) 

下图演示一些简单的逻辑表达式使用例子，用户日志如下所示：

```
2017-01-09 16:02:49|ERROR|it is error...
2017-01-09 16:03:49|INFO|it is ok...
2017-01-09 16:03:49|INFO_1|it is ok...
```

日志按照“|”切分之后的字段分别为`date`、`type`、`message`。

示例一：当`type`为 INFO 时，该行日志直接丢弃；

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152321/155496243644129_zh-CN.png) 

示例二：当`type`包含 INFO 的时候，该行日志丢弃；

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152321/155496243644130_zh-CN.png) 

## 赋值 { .section}

接下来演示一个简单的赋值使用例子，用户日志如下所示：

```
2017-01-09 16:02:49|松江区
2017-01-09 16:03:49|浦东区
2017-01-09 16:03:49|上城区
2017-01-09 16:03:49|下城区
```

日志按照“|”切分之后的的字段分别为`date`、`region`。如果`region`为“松江区”或者“浦东区”，则添加一个新的字段`province`（String 类型），其值为“上海市”；如果`region`为“上城区”或者“下城区”，则添加一个新的字段`province`（String 类型），其值为“杭州市”。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152321/155496243644131_zh-CN.png) 

## 内置函数（转自 Aviator 官方文档） { .section}

Aviator 表达式是一个开源产品（[点此查看官方文档](https://code.google.com/archive/p/aviator/wikis/User_Guide_zh.wiki)）。常见内置函数的说明如下：

|函数|描述|
|--|--|
|sysdate\(\)|返回当前日期对象 java.util.Date|
|rand\(\)|返回一个介于0-1的随机数，Double 类型|
|now\(\)|返回 System.currentTimeMillis|
|long\(v\)|将值的类型转为 Long|
|double\(v\)|将值的类型转为 Double|
|date\_to\_string\(date,format\)|将 Date 对象转化化特定格式的字符串|
|string\_to\_date\(source,format\)|将特定格式的字符串转化为 Date 对象|
|string.contains\(s1,s2\)|判断 s1 是否包含 s2，返回 Boolean|
|string.length\(s\)|求字符串长度,返回 Long|
|string.startsWith\(s1,s2\)|s1 是否以 s2 开始，返回 Boolean|
|string.endsWith\(s1,s2\)|s1 是否以 s2 结尾,返回 Boolean|
|string.substring\(s,begin\[,end\]\)|截取字符串 s，从 begin 到 end，end 如果忽略的话，将从 begin 到结尾，与 java.util.String.substring 一样|
|string.indexOf\(s1,s2\)|java 中的 s1.indexOf\(s2\)，求 s2 在 s1 中的起始索引位置，如果不存在为-1|
|string.split\(target,regex,\[limit\]\)|Java 里的 String.split 方法一致|
|string.replace\_first\(s,regex,replacement\)|Java 里的 String.replaceFirst 方法|
|string.replace\_all\(s,regex,replacement\)|Java 里的 String.replaceAll 方法|
|math.abs\(d\)|求 d 的绝对值|
|math.sqrt\(d\)|求 d 的平方根|
|math.pow\(d1,d2\)|求 d1 的 d2 次方|
|math.log\(d\)|求 d 的自然对数|
|math.log10\(d\)|求 d 以 10 为底的对数|
|math.sin\(d\)|正弦函数|
|math.cos\(d\)|余弦函数|
|math.tan\(d\)|正切函数|

