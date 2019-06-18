# 安装 Agent {#concept_42451_zh .concept}

ARMS 通过日志服务的 Logtail Agent 收集 ECS 日志。本文介绍如何在 ECS 实例上安装 Logtail Agent。

## 安装 Logtail {#section_xzh_yxr_gfb .section}

1.  登录 [ARMS 控制台](https://arms.console.aliyun.com/#/home)，并在页面左上角选择所需地域。
2.  在控制台左侧导航栏中选择**自定义监控-数据源管理** \> **云服务器 ECS**。
3.  在**实例列表**页面，单击需要安装的 ECS 实例右侧的**安装 Agent** 选项。
4.  根据机器所处的网络环境和日志服务所在地域下载安装器，选择不同参数完成安装。

**说明：** Agent 安装为覆盖安装模式。如果之前已安装过 Logtail，那么安装器会先执行卸载、删除`/usr/local/ilogtail`目录后再重新安装。安装后默认启动 Logtail。

## ECS 经典网络 { .section}

数据通过阿里云内网写入日志服务，不消耗公网带宽。适用于阿里云 ECS。

-   华北 2（北京）

```
 wget http://logtail-release-cn-beijing.oss-cn-beijing-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh
  chmod 755 logtail.sh
  sudo ./logtail.sh install cn-beijing
  sudo touch /etc/ilogtail/users/1098370038733503
```

-   华北 1（青岛）

```
 wget http://logtail-release-cn-qingdao.oss-cn-qingdao-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh
  chmod 755 logtail.sh
  sudo ./logtail.sh install cn-qingdao
  sudo touch /etc/ilogtail/users/1098370038733503
```

-   华东 1（杭州）

```
 wget http://logtail-release-cn-hangzhou.oss-cn-hangzhou-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh
  chmod 755 logtail.sh
  sudo ./logtail.sh install cn-hangzhou
  sudo touch /etc/ilogtail/users/1098370038733503
```

-   华东 2（上海）

```
 wget http://logtail-release-cn-shanghai.oss-cn-shanghai-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh
  chmod 755 logtail.sh
  sudo ./logtail.sh install cn-shanghai
  sudo touch /etc/ilogtail/users/1098370038733503
```

-   华南 1（深圳）

```
 wget http://logtail-release-cn-shenzhen.oss-cn-shenzhen-internal.aliyuncs.com/linux64/logtail.sh -O logtail.sh
  chmod 755 logtail.sh
  sudo ./logtail.sh install cn-shenzhen
  sudo touch /etc/ilogtail/users/1098370038733503
```


## ECS 专有网络 VPC { .section}

数据通过阿里云专有网络写入日志服务。适用于阿里云 ECS VPC。

-   华北 2（北京）

```
 wget http://logtail-release-bj.vpc100-oss-cn-beijing.aliyuncs.com/linux64/logtail.sh
  chmod 755 logtail.sh
  sudo sh logtail.sh install cn_beijing_vpc
  sudo touch /etc/ilogtail/users/1098370038733503
```

-   华东 1（杭州）

```
 wget http://logtail-release.vpc100-oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh
  chmod 755 logtail.sh
  sudo sh logtail.sh install cn_hangzhou_vpc
  sudo touch /etc/ilogtail/users/1098370038733503
```

-   华东 2（上海）

```
 wget http://logtail-release-sh.vpc100-oss-cn-shanghai.aliyuncs.com/linux64/logtail.sh
  chmod 755 logtail.sh
  sudo sh logtail.sh install cn_shanghai_vpc
  sudo touch /etc/ilogtail/users/1098370038733503
```

-   华南 1（深圳）

```
 wget http://logtail-release-sz.vpc100-oss-cn-shenzhen.aliyuncs.com/linux64/logtail.sh
  chmod 755 logtail.sh
  sudo sh logtail.sh install cn_shenzhen_vpc
  sudo touch /etc/ilogtail/users/1098370038733503
```


## 公网（自建 IDC 或其他云主机） { .section}

数据通过公网写入日志服务，占用公网带宽。适用于非阿里云虚拟机或其它 IDC。

日志服务无法获取非阿里云机器的属主信息，请在安装 Logtail 后手动配置用户标识，否则会导致 Logtail 心跳异常，无法收集日志。

-   华北 2（北京）

```
wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh
 chmod 755 logtail.sh
 sudo sh logtail.sh install cn_beijing_internet
 sudo touch /etc/ilogtail/users/1098370038733503
```

-   华北 1（青岛）

```
wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh
 chmod 755 logtail.sh
 sudo sh logtail.sh install cn_qingdao_internet
 sudo touch /etc/ilogtail/users/1098370038733503
```

-   华东 1（杭州）

```
 wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh
 chmod 755 logtail.sh
 sudo sh logtail.sh install cn_hangzhou_internet
 sudo touch /etc/ilogtail/users/1098370038733503
```

-   华东 2（上海）

```
wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh
 chmod 755 logtail.sh
 sudo sh logtail.sh install cn_shanghai_internet
 sudo touch /etc/ilogtail/users/1098370038733503
```

-   华南 1（深圳）

```
wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh
 chmod 755 logtail.sh
 sudo sh logtail.sh install cn_shenzhen_internet
 sudo touch /etc/ilogtail/users/1098370038733503
```


## 验证 Logtail { .section}

1.  在 ARMS 控制台左侧导航栏中选择**自定义监控-数据源管理** \> **云服务器 ECS**。
2.  在实例列表页面，在需要检查的 ECS 实例右侧单击**检查 Agent**。稍等片刻，当 Agent 状态变为**已安装**时则表示生效。

## 卸载 Logtail { .section}

参考[安装 Logtail](#section_xzh_yxr_gfb)下载安装器 logtail.sh，并在 shell 环境下以管理员权限执行以下命令：

```
sudo sh logtail.sh uninstall
sudo rm -rf /etc/ilogtail/users/1098370038733503 
```

