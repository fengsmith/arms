# 为 Java 应用快速安装探针的常见问题 {#concept_102290_zh .concept}

本文解答了关于以快速方式为 Java 应用安装探针的常见问题。

## 运行接入脚本时出现 getcwd 相关错误，该如何解决？ {#section_zcw_2ry_pgb .section}

若您在运行一键接入 Java 应用的接入脚本时，遇到以下错误：

```
shell-init: error retrieving current directory: getcwd: cannot access parent directories: No such file or directory Error occurred during initialization of VM java.lang.Error: Properties init: Could not determine current working directory. at java.lang.System.initProperties(Native Method) at java.lang.System.initializeSystemClass(System.java:1119)
```

可能原因是执行脚本过程中误删了当前目录，解决方法为执行 `cd` 后重新运行脚本。

