# 为开源 Kubernetes 环境中的应用安装探针常见问题 {#concept_106851_zh .concept}

本文解答了关于将开源 Kubernetes 环境的应用接入 ARMS 应用监控的常见问题。

## 应用无法正常启动 {#section_sfb_dsy_pgb .section}

执行以下命令查看 arms-pilot-system 的日志信息来排查问题。

```
kubectl logs -f {arms-pilot-arms-pilot-XXX} -n arms-pilot-system
```

## 如何查看 Agent 日志？ {#section_lnd_hsy_pgb .section}

在 Kubernetes 集群 worker 机上查看 /home/admin/.opt/ArmsAgent/logs/xxxx.log。

