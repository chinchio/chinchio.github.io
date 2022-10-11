---
title:       "截取RTMP URL"
subtitle:    "利用pyshark取得Application的RTMP URL"
description: "學習使用pyshark來獲取及取得封包資訊"
date:        2022-10-11T21:12:55+08:00
author:      "chinchio"
image:       "https://images.unsplash.com/photo-1522327646852-4e28586a40dd?ixlib=rb-1.2.1&dl=nicolas-lb-uVSyr0EUaLY-unsplash.jpg&q=80&fm=jpg&crop=entropy&cs=tinysrgb"
tags:        ["RTMP", "FFMPEG"]
categories:  ["Tech"]
---

# 前言
相信各位都有首先直播的經驗，不管是VTuber、或者看玩家在Twitch或Youtube上面分享遊戲實況。

而據我所知Youtube, Facebook 及 Twitch都是透過`RTMP` protocol來把畫面串流到它們的主機上。

當然有`RTMP push`也會有`RTMP pull`，而本人這次的重點就是抓取用作觀看直播APP上的`RTMP` URL。

# 觀察
據我本人的觀察，網頁上的直播通常都會透過`HLS(HTTP Live Streaming)`來進行，那麼截取直播的鏈結其實會相當簡單，只要找到`*.m3u8`就可以，而直播的特徵也相當明顯，只要找到一直有connection的那個就是了。

![Open Develop Tools on Twitch](https://i.imgur.com/iscsr0k.png)

但如果是像在電腦上運行的程式呢？沒有像Browser的Develop Tools，如果幸運的只是套個皮裏面還在用`HLS`直播的，找個`Fiddler`、`Charles`之類針對的`HTTP`封包進行截取的程式就可以了。

但如果是透過其他的協定，那就要打開`Wireshark`來看看有甚麼直播用的封包。

# 截取封包

但幸好，本人此次要截取的直播App是用`RTMP`進行串流，只需簡單在`Wireshark` filter加上 `RTMP`字樣即可。

![Using `RTMP` filter on `Wireshark`](https://i.imgur.com/Sa4rdSJ.png)

但這樣一個一個抓取其實相當麻煩，所以我就自己寫了一個Script，可以自動在偵測到`RTMP` `connect` 及 `play` 的時候會把hostname及path輸出。

鏈結在此：[chinchio/rtmp-url-capture: Capture Application RTMP URL](https://github.com/chinchio/rtmp-url-capture)

![chinchio/rtmp-url-capture main page capture | Github](https://i.imgur.com/MDXcLIt.png)

> 項目寫得比較傷眼，請見諒！

# Code重要部份
```python=
display_filter = 'rtmpt contains "rtmp" or rtmpt contains "play"'
```
首先您看到這個`rtmpt`你可能會覺得奇怪，明明我標題及內容都是在說抓取`RTMP`協定，但到了`Wireshark`部份就變成了`RTMPT`

這是因為在`Wireshark`裏面的`RTMP`是指[`Routing Table Maintenance Protocol (RTMP)`](https://wiki.wireshark.org/RTMP.md)，`RTMPT`才是指[`Real Time Messaging Protocol (Tunnel) (RTMPT)`](https://wiki.wireshark.org/RTMPT.md)

而`rtmpt contains "<String>"`是指把`RTMPT`封包裏面包括有**rtmp**的字串都要抓出來，但由於我要抓取的APP會把`rtmp://<hostname>:[port]/live/`*[前半部]*跟`<Key>`*[後半部]*會分開兩個封包傳送，要取得整個串流URL必須取得`rtmp://<hostname>:[<port>]/live/<key>`。

所以第一部份的`rtmpt contains "rtmp"`就是為了取得 *[前半部]* ，而第二部份`rtmpt contains "play"`就是在取得 *[後半部]* ，因為兩個條件其中一個合符即可，所以就用了`or`。

---

```python=
capture = pyshark.LiveCapture(interface='<NetworkInterfaceName>', display_filter= display_filter, use_json=True)
```
`pyshark.LiveCapture`顧名思義就是抓取實時的封包，而`display_filter`就是上面解釋過的code，而`use_json=True`是為了把取得的封包`RTMP`的`URL`等資訊

---

```python=
for packet in capture.sniff_continuously():
    rtmpt = packet.rtmpt
    rtmp_body = []
    if "RTMP Body" in rtmpt._all_fields:
        rtmp_body = rtmpt._all_fields["RTMP Body"]
```
我私心認為最重要的程式碼莫過於line 4的`rtmpt._all_fields`，但如果沒有配合capture封包用`json`方式顯示出來，一切都前功盡廢。

上述程式碼簡單來說就是把封包裏面的`Application layer`的`RTMP` data抓取出來，看看有沒有符合特徵的，有就取出進行邏輯判斷，像line 4 及 line 5就是判斷`"RTMP Body"`在不在這次抓取的`RTMP`封包裏面。

