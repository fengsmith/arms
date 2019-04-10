# 安装 ARMS SDK {#concept_51904_zh .concept}

ARMS 提供 Java、Python 和 PHP 的 SDK。

要安装 ARMS Java SDK，只需在 pom.xml 文件中添加 Maven 依赖即可。

```
<dependencies>
    <dependency>
       <groupId>com.aliyun</groupId>
       <artifactId>aliyun-java-sdk-arms</artifactId>
       <version>2.2.0</version>
    </dependency>
    <dependency>
        <groupId>com.aliyun</groupId>
        <artifactId>aliyun-java-sdk-core</artifactId>
        <version>2.2.0</version>
    </dependency>
</dependencies>

```

## 安装 Python SDK { .section}

要安装 ARMS Python SDK，只需执行以下命令即可：

```
pip install aliyun-python-sdk-arms

```

## 安装 PHP SDK { .section}

请按照以下步骤安装 PHP SDK。

1.  执行以下命令，将 PHP SDK 克隆到本地的 `aliyun-openapi-php-sdk` 文件夹。

    ```
    git clone https://github.com/aliyun/aliyun-openapi-php-sdk
    
    ```

2.  将 `aliyun-openapi-php-sdk` 下的 `aliyun-php-sdk-arms` 和 `aliyun-php-sdk-core` 文件夹复制到您的 PHP 项目目录，目录结构如下图所示。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152342/155488982943309_zh-CN.png)

## 更多信息 { .section}

-    [下钻数据集查询接口]() 
-    [通用数据集查询接口]() 
-    [Metric查询接口]() 

