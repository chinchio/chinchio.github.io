---
title:       "使用Un Poller來可視化UniFi數據（？）"
subtitle:    "初次使用influxdb及Grafana"
description: "學習使用一下Grafana(皮毛)"
date:        2022-10-29T00:03:40+08:00
author:      "chinchio"
image:       "https://images.unsplash.com/photo-1521542464131-cb30f7398bc6?ixlib=rb-4.0.3&dl=thomas-jensen-UrtxBX5i5SE-unsplash.jpg"
tags:        ["UnPoller", "Unifi", "influxdb", "Grafana"]
categories:  ["Tech"]
---

# 引言

因為之後可能會遇到需要Monitor系統的需求，所以一直在找server monitor工具，目標是免費開源、而且較多人使用，即使遇到問題也可以比較輕鬆找到相應的解決方案。

所以看到很多人的推薦都是使用`Prometheus`及`Grafana`來搭建監控平台。

但問題又來了，如果我沒有服務可以監控，那我如何去學用`Prometheus`及`Grafana`來監控測試呢？

因為我現在家中是用`UniFi全家桶`，雖然內建已經有不錯的可視化工具來監控網路情況，但還是不時在界面上有點BUG。

![UniFi Network Status](https://i.imgur.com/z8oC8EW.png)

而且今天我要做一些`port forward`設定去access我網路中的機器，才發現原來我家中使用的ISP `MTel`網路不穩，所以想著有沒有辦法去把我一些我想監控的數據整合在一起。

後來用`UniFi`及`Grafana`作為關鍵詞，找到了[`Un Poller`](https://unpoller.com/)這個軟件。

# 前置步驟

因為`Un Poller`需要登入並讀取我`UniFi`的數據，所以需要我建立一個有Read Only權限的user

> 可參考[Getting Started | Un Poller](https://unpoller.com/docs/install/gettingstarted)的說明

![Unifi add new local user](https://i.imgur.com/jAwfh2O.png)


# 安裝

因為使用`Un Poller`要搭配`InfluxDB`或`Prometheus`作為數據的儲存，而`Grafana`則作為數據的可視化。

所以本次我選擇使用`Docker`方式來安裝`influxdb`及`Grafana`來使用。

> 用`Docker Compose`來安裝可以簡化安裝，而且方便本人之後隨時刪除這些`Container`，避免弄髒Envirnoment

> 請參考[Docker Compose | Un Poller](https://unpoller.com/docs/install/dockercompose)

---

## 安裝 - 下載`.env`

首先先把envirnoment file下載下來

```bash=
wget https://raw.githubusercontent.com/unpoller/unpoller/master/init/docker/docker-compose.env.example #下載env file
mv docker-compose.env.example .env #將下載的檔案rename為要求的`.env`
```

> 注意：因為在Linux的檔案系統裏面file or folder前面有`.`就是一個隱藏的檔案，所以要查看這個`.env`檔案，必須透過`ls -a`的指令
> ![Use `ls -al` command list `.env` file](https://i.imgur.com/jODEnKS.png)

---

## 安裝 - 下載`docker-compose.yml`

再下載`docker-compose.yml`檔案

```bash=
wget https://raw.githubusercontent.com/unpoller/unpoller/master/init/docker/docker-compose.yml #下載docker-compose.yml
```

---

## 安裝 - 設定`.env`

> 注意：`.env` 及 `docker-compose.yml`需要放在同一個folder裏面

然後要修改`.env`檔案，把他改成你想要的樣子
```text=
# 這就是`.env`檔案裏面，是長這個樣子
#influxdb
INFLUXDB_HTTP_AUTH_ENABLED=true
INFLUXDB_ADMIN_USER=unifi-poller
INFLUXDB_ADMIN_PASSWORD=CHANGEME
INFLUXDB_DB=unifi

#grafana
GRAFANA_USERNAME=admin
GRAFANA_PASSWORD=grafanaadmin

#unifi-poller
POLLER_TAG=latest
POLLER_DEBUG=false
POLLER_SAVE_DPI=false
UNIFI_USER=unifipoller
UNIFI_PASS=set_this_on_your_controller
UNIFI_URL=https://127.0.0.1:8443
```
而根據官方所述，要修改的是`UNIFI_USER` 及 `UNIFI_PASS`

但很明顯的，不設定`UNIFI_URL`, `Un Poller` 是不會知道您要在那裏進入`UniFi`的管理頁面

> 本人認為官方是假設您在安裝的`UniFi Controller`的機器上面安裝`Un Poller`，所以才會把`UNIFI_URL`設定成`https://127.0.0.1:8443`

---

## 安裝 - docker-compose up

完成上一個設定的步驟，那按下來就是要啓動了

```bash=
docker-compose up
```

> 沒有安裝 `docker-compose` 的要自己去根據您的OS去安裝喔！

![Running command `docker-compose up`](https://i.imgur.com/S9HUDjE.png)

![](https://i.imgur.com/czdV1Vy.png)
可以看到service已經跑起來了

那您可能就要問，進入`Grafana`的`URL`是甚麼？

問答的答案就藏在`docker-compose.yml`裏面
![Run command `cat docker-compose.yml`](https://i.imgur.com/rizikKJ.png)

# Post Setup

## Post Setup - `InfluxDB`

[InfluxDB post-setup | Un Poller](https://unpoller.com/docs/dependencies/influxdb/#post-setup)

可以按照網站的方式來為`InfluxDB`新增`retention(保留) policy`

首先我要知道`InfluxDB`的`container ID`才可以對`InfluxDB container`下指令

![Run command `sudo docker ps`](https://i.imgur.com/I0Rntiw.png)

可以看到這次`InfluxDB` 的 container ID 為 `208adc4f885d`

那接下來就是用interactive Shell的方式在container下指令
```bash=
sudo docker exec -it {CONTAINER_ID} bash
```
![Run command `sudo docker exec -it {CONTAINER_ID} bash`](https://i.imgur.com/9zefxxb.png)

> 以interactive Shell方式對container執行指令參考自[How To Use docker exec to Run Commands in a Docker Container | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-use-docker-exec-to-run-commands-in-a-docker-container)

![influx | Run command `show database`](https://i.imgur.com/i0anLjo.png)

可見這樣是沒有權限對`influx`進行操作的，更不用說官方一樣對`influx`進行建立database的操作

```bash=
influx -username {USERNAME} -password {PASSWORD}
```
![login influx with command line](https://i.imgur.com/7Ipv4bv.png)
> 參考[Authentication and authorization in InfluxDB | InfluxDB OSS 1.8 Documentation](https://docs.influxdata.com/influxdb/v1.8/administration/authentication_and_authorization/)

![setting poilcy](https://i.imgur.com/q0jr3ep.png)
> 其實真的要設定只有最後一條command 讓數據保存期限長一點（據官方document所述）

## Setup Grafana
設定好了`influx`後，之後就要設定`Grafana`

1. 按`Add your first data source`
![`Grafana`| `Add your first data source`](https://i.imgur.com/bxyEPoz.png)

2. 選擇`influxDB`
![choose `influxDB`](https://i.imgur.com/DLH2PyA.png)

3. 填好`influxDB`的URL, 要access 的 database, username 及 password
![fill up access `influxDB` information](https://i.imgur.com/sGYsqz7.png)

---

# 設定`Grafana` Dashboard
既然都設定好了
![Dashboard| new > import](https://i.imgur.com/crPWNY2.png)

這裏我用官方的"10419"來做測試
![Setup Grafana | Un Poller `Grafana` dashboard](https://i.imgur.com/dfHB3Aq.png)

![import dashboard "10419"](https://i.imgur.com/S577k3H.png)
[https://unpoller.com/docs/install/grafana/]

![UniFi-Poller: Client DPI - InfluxDB](https://i.imgur.com/fd1VPvW.png)

![DPI result](https://i.imgur.com/LqNVXQu.png)

> 笑左 暴雪居然加埋有90GiB

既然dashboard都可以正常顯示，那就打完收工了

![周星馳 - 打完收工](https://i.imgur.com/OWazxz8.png)