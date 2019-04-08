# 为 PHP 应用安装探针 {#concept_102783_zh .concept}

本文介绍了 PHP 应用如何接入 ARMS 应用监控。

## 前提条件 {#section_rgl_qs1_mgb .section}

-   确保您机器的安全组已开放 8442、8443、8883 三个端口的 TCP 公网入方向权限。若您使用 ECS 请参考文档[添加安全组规则](../../../../../intl.zh-CN/安全/安全组/添加安全组规则.md#)。

    **说明：** ARMS 不仅可接入阿里云 ECS 上的应用，还能接入其他能访问公网的服务器的应用。

-   确保您使用的第三方组件或框架在[应用监控兼容性列表](intl.zh-CN/.md#)范围内。


## 接入 PHP 探针 { .section}

1.  登录[ARMS 控制台](https://arms-intl.console.aliyun.com/#/home)，在左侧导航栏中选择**应用监控** \> **应用列表** 。
2.  在应用列表页面右上角单击**新接入应用**。

3.  在新应用接入页面**选择安装的语言包**区域选择 PHP 语言。

4.  采用以下方法之一下载 PHP 探针，完成后单击**下一步**。

    -   方法一：手动下载。单击**点击下载**按钮，下载最新 ZIP 包。

    -   方法2：wget 命令下载。使用 wget 命令下载 Agent 压缩包。（请根据地域下载对应的压缩包。）

    ```
    # 杭州地域
    wget "http://arms-apm-hangzhou.oss-cn-hangzhou.aliyuncs.com/arms-php-agent.zip" -O arms-php-agent.zip
    
    # 上海地域
    wget "http://arms-apm-shanghai.oss-cn-shanghai.aliyuncs.com/arms-php-agent.zip" -O arms-php-agent.zip
    
    # 青岛地域
    wget "http://arms-apm-qingdao.oss-cn-qingdao.aliyuncs.com/arms-php-agent.zip" -O arms-php-agent.zip
    
    # 北京地域
    wget "http://arms-apm-beijing.oss-cn-beijing.aliyuncs.com/arms-php-agent.zip" -O arms-php-agent.zip
    
    # 深圳地域
    wget "http://arms-apm-shenzhen.oss-cn-shenzhen.aliyuncs.com/arms-php-agent.zip" -O arms-php-agent.zip
    
    # 新加坡地域
    wget "http://arms-apm-ap-southeast.oss-ap-southeast-1.aliyuncs.com/cloud_ap-southeast-1/arms-php-agent.zip" -O arms-php-agent.zip
    
    # 金融云环境
    wget "http://arms-apm-hangzhou.oss-cn-hangzhou.aliyuncs.com/finance/arms-php-agent.zip" -O arms-php-agent.zip
    
    ```

5.  在安装探针页面查看并保存 LicenseKey。

    ![get LicenseKey](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152237/155469274143126_zh-CN.png)

6.  解压探针包。切换到安装包所在目录，解压安装包到任意工作目录下。

    ```
    unzip arms-php-agent.zip /{user.workspace}/
    
    ```

    **说明：** “\{user.workspace\}” 是示例路径，请用户根据自身不同的环境修改正确的目录。

7.  采用以下方法之一安装配置探针。

    -   自动安装。执行以下命令自动安装探针。

        **说明：** 将 `<licenseKey>` 替换为您的 LicenseKey；将 PHP-Demo 替换成您的应用名，应用名暂不支持中文。

        ```
        cd arms-php-agent 
        ./install.sh <licenseKey> PHP-Demo
        
        ```

    -   手动安装。

        1.  切换到解压后 Agent 目录。

            ```
            cd arms-php-agent
            
            ```

        2.  修改 arms-agent.conf 文件中的配置项。

            **说明：** 

            -   将 `<licenseKey>` 替换为您的 LicenseKey。
            -   将 PHP-Demo 替换成您的应用名，应用名暂不支持中文。

            -   将 `<arms-php-agent>` 替换为解压后 arms-php-agent 里面 plugins 目录的绝对路径。

            ```
            LicenseKey=<licenseKey>
            AppName=PHP-Demo
            PluginRootDir=<arms-php-agent>
            
            
            ```

        3.  执行以下命令获取 PHP 拓展安装目录。

            ```
            php -i | grep ^extension_dir
            
            ```

            例如返回结果如下，则安装目录为 `/usr/lib/php/extensions/no-debug-non-zts-20160303`。

            ```
            extension_dir => /usr/lib/php/extensions/no-debug-non-zts-20160303 => /usr/lib/php/extensions/no-debug-non-zts-20160303
            
            ```

        4.  将 arms.so 拷贝到上述目录。

            ```
            sudo cp arms.so <php_extension_dir>
            
            ```

        5.  新增动态连接库路径。

            ```
            sudo vi /etc/ld.so.conf
            
            ```

        6.  在文件尾新增一行。

            ```
            <php-agent-dir>/lib
            
            ```

            **说明：** `<php-agent-dir>` 为探针解压后绝对路径。

        7.  加载动态链接库。

            ```
            sudo ldconfig 
            
            ```

8.  设置 php.ini。在文件末尾添加以下内容。

    **说明：** 请替换变量（若使用自动安装脚本，请直接使用脚本提示的替换内容）： 将 `<php_extension_dir>` 替换为前述 PHP 拓展安装目录，默认目录为 `/usr/lib64/xxx`；`<php-agent-dir>`替换为解压后探针目录的绝对路径。

    ```
    [arms]
    extension=<php_extension_dir>/arms.so
    arms.trace_exception=true
    arms.config_full_name=/<php-agent-dir>/arms-agent.conf
    
    ```

9.  重启您的 PHP 应用。

    约一分钟后，若您的应用出现在应用列表中且有数据上报，则说明接入成功。


## 卸载 PHP 探针 { .section}

当您不需要 ARMS 探针采集数据时，可按照以下步骤卸载探针。

1.  修改 php.ini，删除以下四行：

    ```
    [arms] 
    extension=<php_extension_dir>/arms.so
    arms.trace_exception=true
    arms.config_full_name=/<php-agent-dir>/arms-agent.conf
    
    ```

2.  重启您的 PHP 应用。


