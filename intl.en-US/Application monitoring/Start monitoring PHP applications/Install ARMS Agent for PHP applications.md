# Install ARMS Agent for PHP applications {#concept_102783_zh .concept}

This topic describes how to connect PHP applications to ARMS.

## Prerequisites {#section_rgl_qs1_mgb .section}

-   Ports 8442, 8443, and 8883 in the security group of your machine have opened the inbound access permissions for TCP-based public networks. For more information about how to open the inbound access permissions of ECS instances, see [../../../../dita-oss-bucket/SP\_2/DNECS19100344/EN-US\_TP\_9718.md\#](../../../../intl.en-US/Security/Security groups/Add security group rules.md#).

    **Note:** In addition to applications on Alibaba Cloud ECS instances, applications on servers on the Internet can also access ARMS.

-   Your third-party component or framework is listed in the [ARMS-compatible components and frameworks](intl.en-US/Application monitoring/ARMS-compatible components and frameworks.md#).


## Connect ARMS PHP Agent { .section}

1.  Log on to the [ARMS console](https://arms-intl.console.aliyun.com/#/home). In the left-side navigation pane, choose **Application Monitoring** \> **Applications**.
2.  On the Applications page that appears, click **Create Application** in the upper-right corner.

3.  On the Create Application page that appears, select PHP in the **Programming language** section.

4.  Download ARMS PHP Agent by using either of the following methods. After ARMS PHP Agent is downloaded, click **Next**.

    -   Method 1: Manually download ARMS PHP Agent. Click **Click to Download** to download the latest .zip file.

    -   Method 2: Run the wget command to download the .zip file of ARMS PHP Agent of the corresponding region.

    ```
    # China (Hangzhou)
    wget "http://arms-apm-hangzhou.oss-cn-hangzhou.aliyuncs.com/arms-php-agent.zip" -O arms-php-agent.zip
    
    # China (Shanghai)
    wget "http://arms-apm-shanghai.oss-cn-shanghai.aliyuncs.com/arms-php-agent.zip" -O arms-php-agent.zip
    
    # China (Qingdao)
    wget "http://arms-apm-qingdao.oss-cn-qingdao.aliyuncs.com/arms-php-agent.zip" -O arms-php-agent.zip
    
    # China (Beijing)
    wget "http://arms-apm-beijing.oss-cn-beijing.aliyuncs.com/arms-php-agent.zip" -O arms-php-agent.zip
    
    # China (Shenzhen)
    wget "http://arms-apm-shenzhen.oss-cn-shenzhen.aliyuncs.com/arms-php-agent.zip" -O arms-php-agent.zip
    
    # Singapore
    wget "http://arms-apm-ap-southeast.oss-ap-southeast-1.aliyuncs.com/cloud_ap-southeast-1/arms-php-agent.zip" -O arms-php-agent.zip
    
    # AntCloud environment
    wget "http://arms-apm-hangzhou.oss-cn-hangzhou.aliyuncs.com/finance/arms-php-agent.zip" -O arms-php-agent.zip
    
    ```

5.  On the Install Probe page, view and save the LicenseKey.

    ![get LicenseKey](images/43126_en-US.png)

6.  Switch to the directory of the installation package, and decompress the ARMS PHP Agent package to any working directory.

    ```
    unzip arms-php-agent.zip /{user.workspace}/
    
    ```

    **Note:** “\{user.workspace\}” is an example path. Replace it as needed.

7.  Install and configure ARMS PHP Agent by using either of the following methods:

    -   Run the following command to install ARMS PHP Agent automatically:

        **Note:** Replace `<licenseKey>` with the actual LicenseKey and replace PHP-Demo with the name of your application. Application names in Chinese are not currently supported.

        ```
        cd arms-php-agent 
        ./install.sh <licenseKey> PHP-Demo
        
        ```

    -   Manually install ARMS PHP Agent.

        1.  Switch to the directory where the ARMS PHP Agent package is decompressed.

            ```
            cd arms-php-agent
            
            ```

        2.  Modify configuration items in the arms-agent.conf file.

            **Note:** 

            -   Replace `<licenseKey>` with the actual LicenseKey.
            -   Replace PHP-Demo with the name of your application. Application names in Chinese are not supported currently.

            -   Replace `<arms-php-agent>` with the absolute path of the plugins directory of arms-php-agent after the package is decompressed.

            ```
            LicenseKey=<licenseKey>
            AppName=PHP-Demo
            PluginRootDir=<arms-php-agent>
            
            
            ```

        3.  Run the following command to obtain the PHP extension installation directory:

            ```
            php -i | grep ^extension_dir
            
            ```

            For example, if the following information is returned, the installation directory is `/usr/lib/php/extensions/no-debug-non-zts-20160303`:

            ```
            extension_dir => /usr/lib/php/extensions/no-debug-non-zts-20160303 => /usr/lib/php/extensions/no-debug-non-zts-20160303
            
            ```

        4.  Copy arms.so and paste it to the preceding directory.

            ```
            sudo cp arms.so <php_extension_dir>
            
            ```

        5.  Run the following command to add a dynamic connection library path:

            ```
            sudo vi /etc/sysctl.conf.
            
            ```

        6.  Add a line to the end of the file.

            ```
            <php-agent-dir>/lib
            
            ```

            **Note:** `<php-agent-dir>` is the absolute path after the ARMS PHP Agent package is decompressed.

        7.  Run the following command to load the dynamic connection library:

            ```
            sudo ldconfig 
            
            ```

8.  Set php.ini. Add the following information to the end of the file.

    **Note:** Replace `<php_extension_dir>` with the preceding PHP extension installation directory, which is `/usr/lib64/xxx` by default. Replace `<php-agent-dir>` with the absolute path of the ARMS PHP Agent directory after the ARMS PHP Agent package is decompressed. If you run a script to automatically install ARMS PHP Agent, replace the preceding variables as prompted.

    ```
    [arms]
    extension=<php_extension_dir>/arms.so
    arms.trace_exception=true
    arms.config_full_name=/<php-agent-dir>/arms-agent.conf
    
    ```

9.  Restart your PHP application.

    After one minute, if your application is displayed in the application list and has data reported, your application has been connected to ARMS.


## Uninstall ARMS PHP Agent { .section}

If you no longer need to use ARMS Agent to collect data, perform the following steps to uninstall it.

1.  Delete the following four lines from php.ini:

    ```
    [arms] 
    extension=<php_extension_dir>/arms.so
    arms.trace_exception=true
    arms.config_full_name=/<php-agent-dir>/arms-agent.conf
    
    ```

2.  Restart your PHP application.


