---
lessAds: false
description: 一行命令开启Kubernetes多集群管理之路_Kuboard_V3安装
meta:
  - name: keywords
    content: Kubernetes Dashboard安装,Kuboard安装,K8S Dashboard安装
---

# 安装 Kuboard v3 - 内建用户库

<AdSenseTitle/>

## Alpha 版本说明

Kuboard v3.0 支持 Kubernetes 多集群管理。当前为 alpha 内测版本，已经完成了多集群管理的主要特性，有如下功能尚未验证：
* Kuboard Proxy
* Kuboard PortForward
* Kuboard 套件

如果您从 Kuboard v1.0.x 或者 Kuboard v2.0.x 升级到 Kuboard，请注意：
* 您可以同时使用 Kuboard v3.0.x 和 Kuboard v2.0.x；
* Kuboard v3.0.x 在正式发布时，考虑将 Kuboard 相关的所有组件从 kube-system 名称空间迁移到 kuboard 名称空间；

Alpha 版本为早期内测版本，预计 Kuboard v3.0.0 的正式发布时间在 2021 年元旦前后

## 部署计划

在正式安装 kuboard v3 之前，需做好一个简单的部署计划的设计，在本例中，各组件之间的连接方式，如下图所示：

* 假设用户通过 http://kuboard.mycompany.com 访问 Kuboard v3；

* 安装在 Kubernetes 中的 Kuboard Agent 通过 `kuboard.mycompany.com` 访问 Kuboard 的 Web 服务端口 80 / 443 和 Kuboard Agent Server 端口 10081。


<p>
<img src="./install-built-in.assets/image-20201117194202143.png" style="max-width: 600px; margin-left: 20px;"/>
</p>

安装 Kuboard 之前，假设：

* 您已经准备好了一个 Linux 服务器用于安装 Kuboard-V3，并且该机器上的 docker 版本不低于 19.03
* 用于安装 Kuboard v3.0 的机器已经安装了 docker，并且版本不低于 docker 19.03
* 您已经有自己的 Kubernetes 集群，并且版本不低于 Kubernetes v1.13

## 安装

安装 Kuboard v3.0 alpha 版本的指令如下：

``` sh {10}
sudo docker run -d \
  --restart=unless-stopped \
  --name=kuboard \
  -p 80:80/tcp \
  -p 10081:10081/udp \
  -p 10081:10081/tcp \
  -e KUBOARD_ENDPOINT="http://kuboard.mycompany.com" \
  -e KUBOARD_AGENT_SERVER_UDP_PORT="10081" \
  -e KUBOARD_AGENT_SERVER_TCP_PORT="10081" \
  -v /root/kuboard-data:/data \
  eipwork/kuboard:v3-alpha
```

::: tip 参数解释
* 建议将此命令保存为一个 shell 脚本，例如 `start-kuboard.sh`，后续升级 Kuboard 或恢复 Kuboard 时，需要通过此命令了解到最初安装 Kuboard 时所使用的参数；
* 第 4 行，将 Kuboard Web 端口 80 映射到宿主机的 `80` 端口（您可以根据自己的情况选择宿主机的其他端口）；
* 第 5、6 行，将 Kuboard Agent Server 的端口 `10081/udp`、`10081/tcp` 映射到宿主机的 `10081` 端口（您可以根据自己的情况选择宿主机的其他端口）；
* 第 7 行，指定 KUBOARD_ENDPOINT 为 `http://kuboard.mycompany.com`，如果后续修改此参数，需要将已导入的 Kubernetes 集群从 Kuboard 中删除，再重新导入；
* 第 8、9 行，指定 KUBOARD_AGENT_SERVER 的端口为 `10081`，此参数与第 5、6 行中的宿主机端口应保持一致，修改此参数不会改变容器内监听的端口 `10081`；
* 第 10 行，将持久化数据 `/data` 目录映射到宿主机的 `/root/kuboard-data` 路径，请根据您自己的情况调整宿主机路径；
:::

## 访问 Kuboard v3.0

在浏览器输入 `http://your-host-ip` 即可访问 Kuboard v3.0 的界面，登录方式：
* 用户名： `admin`
* 密 码： `Kuboard123`


## 问题反馈

关于 Kuboard v3.0.0-alpha 版本的问题反馈，请加 QQ 群：656520037

<img style="padding: 10px; width: 200px; border: 1px solid #eee; border-radius: 5px;" src="https://kuboard.cn/images/kuboard_qq.png"/>
