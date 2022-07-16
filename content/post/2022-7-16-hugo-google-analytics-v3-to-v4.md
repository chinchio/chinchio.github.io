---
title:       "為舊有的hugo theme支援Google Analytics V4"
subtitle:    "來吧！為舊有的hugo theme程式碼做小小的修改"
description: "來為hugo theme做小改造吧！"
date:        2022-07-16
author:      "chinchio"
image:       "https://images.unsplash.com/photo-1484417894907-623942c8ee29?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1932&q=80"
tags:        ["Hugo", "Google Analytics"]
categories:  ["Tech"]
---

# 引言
如果有看到本人上一篇有關於將Github Pages作為本人blog的文章，可以看到我是使用由[zhaohuabing](https://zhaohuabing.com/)開發的[CleanWhite Hugo Theme](https://themes.gohugo.io/themes/hugo-theme-cleanwhite/)

我最需要的tag系統也有做出來，而本人想使用Google Analytics來進行分析到底有多少人來看我的網站，所以在`config.toml`裏面填入
```toml
googleAnalytics = "G-JYRLDDQD28"
```
但奇怪的是，Google Analytics一直沒有訪客的數據，好吧，那我開始找出問題所在吧！

# 追蹤
{{< gist chinchio 1f0901f6525a2955ea43ec0263e30c8b >}}

在上述的code中可以看到我的google 追蹤code在於
```html
  <script type=application/javascript>
    var doNotTrack = !1;
    doNotTrack || (window.ga = window.ga || function() {
      (ga.q = ga.q || []).push(arguments)
    }, ga.l = +new Date, ga("create", "G-JYRLDDQD28", "auto"), ga("send", "pageview"))
  </script>
  <script async src=https://www.google-analytics.com/analytics.js></script>
```
而根據我在Google Analytics看到的code怎麼完全不一樣的？
![Google Analytics gtag.js](https://i.imgur.com/HO8sWiu.png)

好吧，那我就在theme的code裏面試圖找答案吧

## 思路
- 首先，因為我是從`config.toml`裏面透過`googleAnalytics`這個var來設定，那不妨先該repo搜尋`googleAnalytics`
![search googleAnalytics in hugo-theme-cleanwhite repo](https://i.imgur.com/p0VwK5d.png)

好吧，幸好這個項目不是很大，我們很快就可以看到`{{ if .Site.GoogleAnalytics }}`

- 從`footer.html`入手，我們就可以看到
```html
{{ if .Site.GoogleAnalytics }}
{{ template "_internal/google_analytics_async.html" . }}
{{ end }}


<!-- Side Catalog -->
{{ if not (eq (.Param "showtoc") false) }}
```
很明顯如果GoogleAnalytics如果存在 那就會載入`template "_internal/google_analytics_async.html"`

但現在問題是到底甚麼是`template _internal`呢？

萬事不決，Goolge解決，那讓我們先搬出Google大神搜尋一下問題好了！

[templates internal #configure-google-analytics](https://gohugo.io/templates/internal/#configure-google-analytics)
![Hugo| template about Google Analytics](https://i.imgur.com/RjwzrfA.png)

# 替換

hugo官方的document已經告訴我們答案了，我現在使用的theme因為還在使用hugo 內建適用於`Google Analytics v3`的template，所以一直都沒有辦法使Google Analytics接收到數據

所以把footer 裏面的`{{ template "_internal/google_analytics.html" . }}`更換成`{{ template "_internal/google_analytics_async.html" . }}`即可

# 反思
出現這次的問題很明顯是本人對於hugo theme不熟悉所導致，如果沒有追蹤看code完全沒有辦法解決問題

如果真的想一直用hugo做blog，還是去學一學如何寫hugo theme好了

那...下次見囉！