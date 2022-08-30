---
title:       "利用code-server實現在browser裏面寫code"
subtitle:    "可以讓你在有網路的地方去access你的PC來寫code"
description: "在ubuntu裏面安裝code-server，並讓它接受remote connection"
date: 2022-08-30T22:16:18+08:00
author:      "chinchio"
image:       "https://images.unsplash.com/photo-1617854818583-09e7f077a156?ixlib=rb-1.2.1&dl=remotar-jobs-s5kTY-Ve1c0-unsplash.jpg&q=80&fm=jpg&crop=entropy&cs=tinysrgb"
tags:        ["code-server", "ubuntu"]
categories:  ["Tech"]
---

# 引言
本人在數年前(即大概2018、2019左右)在上技術轉移中心的java課程時曾經有使用過`c9`及`codeanywhere`來嘗試寫`java`的程式，因為其實要架設`java`環境在公用電腦其實是很麻煩的事，所以才去嘗鮮。
而現在自己手邊有一台沒有空閑的nuc，所以打算灌上`ubuntu`來玩一下，想來學一下`unix script`、`docker`或`Elastic Stack`，畢竟在windows的環境下來玩這些東西還是比較麻煩。

# 選擇
![Google| search results about `cloud9 alternatives`](https://i.imgur.com/bcOaQlV.png)
其實現在市面上的`online IDE`選擇已經有很多，例如我之前提及的`c9`、`codeanywhere`、`codenvy`等等（泛義來說`colab`應該也算是online IDE才對）

但我最印象深刻的online IDE就是之前`colab`還沒有嚴格限制不能使用ssh等remote方式對colab分配的VM進行連線時本人常用的`code-server`，它的介面跟`vs-code`一模一樣，那時的script是配合`ngrok`來進行port forward來access該online IDE。

# code-server

## 安裝
既然已經做好了選擇，那接下來就是安裝了，而該code-server的 repo是[Github| code-server](https://github.com/coder/code-server)

而它下方已經提供了安裝的command
```bash
curl -fsSL https://code-server.dev/install.sh | sh
```

而如果想知道該script到底做了甚麼事，可以執行
```bash
curl -fsSL https://code-server.dev/install.sh | sh -s -- --dry-run
```
![dry-run code-server install script](https://i.imgur.com/US8eZrR.png)

![installing code-server with code-server official script](https://i.imgur.com/1egK7rl.png)

```bash
xx@xx-NUC8xxxxx:~$ curl -fsSL https://code-server.dev/install.sh | sh
Ubuntu 22.04.1 LTS
Installing v4.6.0 of the amd64 deb package from GitHub.

+ mkdir -p ~/.cache/code-server
+ curl -#fL -o ~/.cache/code-server/code-server_4.6.0_amd64.deb.incomplete -C - https://g                                                                                                                                                    ithub.com/coder/code-server/releases/download/v4.6.0/code-server_4.6.0_amd64.deb
######################################################################## 100.0%
+ mv ~/.cache/code-server/code-server_4.6.0_amd64.deb.incomplete ~/.cache/code-server/cod                                                                                                                                                    e-server_4.6.0_amd64.deb
+ sudo dpkg -i ~/.cache/code-server/code-server_4.6.0_amd64.deb
[sudo] password for xx:
Selecting previously unselected package code-server.
(Reading database ... 206229 files and directories currently installed.)
Preparing to unpack .../code-server_4.6.0_amd64.deb ...
Unpacking code-server (4.6.0) ...
Setting up code-server (4.6.0) ...

deb package has been installed.

To have systemd start code-server now and restart on boot:
  sudo systemctl enable --now code-server@$USER
Or, if you don't want/need a background service you can run:
  code-server

```

## 執行
既然已經安裝完成，那接下來就是要來執行看看

![open code-server with browser and show code-server config](https://i.imgur.com/MTX2tHo.png)

code-server的password可以在`~/.config/code-server/config.yaml`裏面(找到/設定)

![open code-server success with local browser](https://i.imgur.com/MCUAjSa.png)

![excute code-server command in the terminal](https://i.imgur.com/hXgqWuz.png)
想當然因為該程式只有在監聽`http://128.0.0.1:8080`，所以其他電腦是沒有辦法進入該網站，而解決問題的辦法也很簡單

```bash
code-server --host 0.0.0.0
```

![excute command `code-server --host 0.0.0.0` in the terminal](https://i.imgur.com/XCcOb6h.png)

執行上方的command即可在其他的電腦打開code-server

![open code-server with other computer](https://i.imgur.com/n9Mmfbf.png)

![write and excute code with browser](https://i.imgur.com/IoLj57z.png)

看起來用來寫code還有執行都不是甚麼問題

# 結論
現在這個`code-server`已經可以使用，已經達到本人最初的目標，所以接下來的目標就是可以隨時去access這個`code-server`，而最安全的做法就是透過`VPN`來連接到家中的LAN來access該網頁；而懶一點的做法就是直接簡單粗暴進行`port forward`，但各有各的好處跟壞處，而remote部份我打算放在下節購買domain後再說（如果我有動力把remote寫完的話！）

現在看完這篇文章應該很簡單可以把各位的code-server架起來，當然用windows的也可以透過`WSL`來安裝`code-server`，但印象中`WSL`不會隨著windows的啓動而開啓，所以要注意這一點！