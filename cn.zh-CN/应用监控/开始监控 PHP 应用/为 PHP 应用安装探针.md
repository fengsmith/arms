# 为 PHP 应用安装探针 {#concept_102783_zh .concept}

为 PHP 应用安装 ArmsAgent 探针并重启应用后，ARMS 即可对 PHP 应用进行应用拓扑、接口调用、异常事务和慢事务监控、SQL 分析等监控。本文将介绍为 PHP 应用安装和卸载探针的方法。

## 前提条件 {#section_rgl_qs1_mgb .section}

-   确保您使用的机器安全组已开放 8442、8443、8883 三个端口的 TCP 公网出方向权限。为 ECS 开放出方向权限，请参见[添加安全组规则](../../../../intl.zh-CN/安全/安全组/添加安全组规则.md#)。

    **说明：** ARMS 不仅可接入阿里云 ECS 上的应用，还能接入其他能访问公网的服务器上的应用。

-   确保您使用的第三方组件或框架在应用监控兼容性列表范围内，请参见[应用监控兼容性列表](intl.zh-CN/应用监控/应用监控兼容性列表.md#)。


## 接入 PHP 探针 { .section}

1.  登录 [ARMS 控制台](https://arms-ap-southeast-1.console.aliyun.com/#/home)，在左侧导航栏中选择**应用监控** \> **应用列表** 。
2.  在应用列表页面右上角单击**新接入应用**。

3.  在新应用接入页面选择使用语言为 **PHP**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152237/155739540044408_zh-CN.png)

4.  采用以下方法之一下载 PHP 探针，完成后单击**下一步**。

    -   方法一：手动下载。单击**点击下载**按钮，下载最新 ZIP 包。

    -   方法二：Wget 命令下载。根据所在 Region 使用 Wget 命令下载 Agent 压缩包。

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

5.  解压探针包。切换到安装包所在目录，解压安装包到任意工作目录下。

    **说明：** `{user.workspace}` 是示例路径，请按需修改。

    ``` {#codeblock_7cw_45q_dig}
    unzip arms-php-agent.zip /{user.workspace}/
    ```

6.  在安装探针页签查看并保存 LicenseKey。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152237/155739540043126_zh-CN.png)

7.  采用以下方法之一安装和配置探针。

    -   自动安装。执行以下命令自动安装探针。

        **说明：** 将 `<licenseKey>` 替换为您的 LicenseKey；将 PHP-Demo 替换成您的应用名，应用名暂不支持中文。

        ```
        cd arms-php-agent 
        ./install.sh <licenseKey> PHP-Demo                     
        ```

    -   手动安装。

        1.  切换到探针解压目录。

            ```
            cd arms-php-agent                                 
            ```

        2.  修改 arms-agent.conf 文件中的配置项。

            **说明：** 

            -   将 `<licenseKey>` 替换为您的 LicenseKey。将 PHP-Demo 替换成您的应用名，应用名暂不支持中文。
            -   将 `<arms-php-agent>` 替换为解压后 arms-php-agent 目录下 plugins 的绝对路径。

            ```
            LicenseKey=<licenseKey>
            AppName=PHP-Demo
            PluginRootDir=<arms-php-agent>                            
            ```

        3.  执行以下命令获取 PHP 拓展安装目录。

            ```
            php -i | grep ^extension_dir                               
            ```

            若返回结果如下所示，则安装目录为 `/usr/lib/php/extensions/no-debug-non-zts-20160303`。

            ```
            extension_dir => /usr/lib/php/extensions/no-debug-non-zts-20160303 => /usr/lib/php/extensions/no-debug-non-zts-20160303                  
            ```

        4.  将 arms.so 拷贝到上一步获取的 PHP 拓展安装目录。

            **说明：** `<php_extension_dir>`为上一步获取的 PHP 拓展安装目录，请根据实际替换。

            ```
            sudo cp arms.so <php_extension_dir>                  
            ```

        5.  新增动态连接库路径。

            ```
            sudo vi /etc/ld.so.conf                      
            ```

        6.  在文件尾新增一行。

            **说明：** `<php-agent-dir>` 为探针解压后绝对路径。

            ```
            <php-agent-dir>/lib                           
            ```

        7.  加载动态链接库。

            ```
            sudo ldconfig                                   
            ```

8.  在文件末尾替换以下内容来设置 php.ini。

    **说明：** 

    -   若您使用自动安装脚本，请使用脚本提示的替换内容；
    -   若您使用手动安装，则需将 `<php_extension_dir>` 替换为 PHP 拓展安装目录，默认安装目录为 `/usr/lib64/xxx`，`<php-agent-dir>`替换为解压后探针目录的绝对路径。
    ```
    [arms]
    extension=<php_extension_dir>/arms.so
    arms.trace_exception=true
    arms.config_full_name=/<php-agent-dir>/arms-agent.conf        
    ```

9.  重启您的 PHP 应用。


约一分钟后，若您的 PHP 应用出现在应用列表中且有数据上报，则说明接入成功。

## 卸载 PHP 探针 { .section}

当您不需要 ARMS 监控 PHP 应用时，可按照以下步骤卸载探针。

1.  修改 php.ini，删除以下四行：

    ```
    [arms] 
    extension=<php_extension_dir>/arms.so
    arms.trace_exception=true
    arms.config_full_name=/<php-agent-dir>/arms-agent.conf                  
    ```

2.  重启您的 PHP 应用。


