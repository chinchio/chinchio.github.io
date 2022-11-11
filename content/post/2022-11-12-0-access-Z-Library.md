---
title:       "存取Z-Library"
subtitle:    "透過tor訪問Z-Library"
description: "因為Z-Library被美國政府禁止訪問，所以改用tor來存取此網站"
date:        2022-11-12T00:05:57+08:00
author:      "chinchio"
image:       "https://images.unsplash.com/photo-1511075675422-c8e008f749d7?ixlib=rb-4.0.3&dl=stefan-steinbauer-HK8IoD-5zpg-unsplash.jpg&q=80&fm=jpg&crop=entropy&cs=tinysrgb"
tags:        ["Z-Library", "z-lib", "tor"]
categories:  ["Tech"]
---

> 此文的日期格式皆為 YYYY-MM-DD ，請各位以此為前提檢視時間

> 此篇文章編寫於2022-11-11，您閱讀此文章時文中訊息可能已經過期；如文中消息不準確，請參見Wikipedia得知`Z-Library`最新消息

# 緣由

相信進來讀取此文章的各位都知道`Z-Library` domain 於2022-11-6被美國政府扣押了；如果連上`Z-Library`的domain即為看到如下警告標語

![http://z-lib.org/ screenshot](https://i.imgur.com/Ehb8dNg.jpg)

![http://z-lib.org/ | whois](https://i.imgur.com/MfawAu5.png)

其實`Z-Library`伺服器沒有被關掉，只是domain被美國政府收掉而已。

# 進入

既然前面而已說了，伺服器沒有關掉，只是沒有辦法用domain訪問server而已；所以`Z-Library`就提供了`tor`訪問方式

[free download. Ebooks library. Online books store on Z-Library](http://zlibrary24tuxziyiyfr7zd46ytefdqbqd2axkmxm4o5374ptpc52fad.onion)

---

## 安裝tor

既然要連上`.onion`，自然要使用`tor` browser

[Tor Project | Download](https://www.torproject.org/download/)

![tor browser download page](https://i.imgur.com/BQqRE0R.png)

根據您的OS去選擇要下載的tor版本

> 其實說來也蠻諷刺，當初`onion routing`就是美國政府主導開發；但現在卻變成讓美國政府想封鎖的網站可以被其他人正常訪問的工具

---

## 連接

![About Tor Browser](https://i.imgur.com/7Vyvut5.png)

> 以下的截圖都是緣自Tor Browser 11.5.7

![Tor Browser](https://i.imgur.com/1K28dmX.png)

打開`tor`後，應該可以看到這個畫面，然後就點擊`connect`即可。

> 為何在`connect` button旁邊還會有一個`configure connection`呢？
>
> 因為在**某些不可名狀之國**是不能直接使用`tor`（還有一些公司、學校等機構也是會把`tor`流量擋掉；原因就是這些機構沒有辦法解讀`tor`的封包，以及有一些惡意軟件也是會透過`tor`來隱藏自己`C&C server`）,所以就會以自行設定proxy或使用`Bridges`來讓`tor`能正常運作
>
> ![tor connection settings](https://i.imgur.com/yYwjyjG.png)


> `Bridges` 相關解釋請參見[橋接中繼站 | Tor 專案｜洋蔥路由瀏覽器使用手冊](https://tb-manual.torproject.org/zh-TW/bridges/)

在**某些不可名狀之國**外或某些機構外，邏輯上應該是可以直接連接上tor的，那就大膽按connect就可以了。

![tor browser connecting tor network](https://i.imgur.com/twRQxNq.png)

![loading...](https://i.imgur.com/Tyg6BJB.png)

> 用`tor browser`來連接網站是會比較慢，詳情可以參見`tor`的運作原理

![connection detail](https://i.imgur.com/pusdRxa.png)

> 也可以透過按下URL旁邊的Onion圖示來查看`tor circuit`

![Success open Z-Library with tor browser](https://i.imgur.com/G5ElzIU.jpg)

等待慢長的數分鐘，網站終於載入完成，然後就是正常的搜尋您要的書本了~

# 結尾
雖然`tor browser`號稱可以保護您的連線，但據說**某些不可名狀之國**會設置一些honey pot來偵測誰在使用以及訪問過甚麼網站，所以還是要小心這一點；還有tor雖然有一些保護poilcy，但onion網站還是有機會有惡意的script來攻擊您的電腦，所以不要隨便瀏覽`.onion`的網站

# 參考
- [Z-Library - 維基百科，自由的百科全書](https://zh.wikipedia.org/zh-tw/Z-Library)
- [全球最大盜版電子書Z-Library網址被美國扣押了 | iThome](https://www.ithome.com.tw/news/154057)