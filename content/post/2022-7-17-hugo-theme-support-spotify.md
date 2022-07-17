---
title:       "讓Hugo theme支援spotify的shortcode吧！"
subtitle:    "hugo 支持 sportify Shortcode"
description: ""
date:        2022-07-17T17:06:17+08:00
author:      "chinchio"
image:       ""
tags:        ["Spotify", "Hugo", "Hugo Shortcode"]
categories:  ["Tech"]
---

# 引言
據上一次把hugo架在github上，又為google analytics v4修改程式碼後，接下來又要做甚麼呢？

昨天因為時間關係就沒有聽[Youtube| 陳一發兒](https://www.youtube.com/channel/UC7QVieoTCNwwW84G0bddXpA)的[陈一发儿： 夏日周末 家庭怀旧KTV~20220716](https://www.youtube.com/watch?v=U5eLsn59vOQ&list=PLi3zrmUZHiY-eH8eNJiwj-viwP3ngIkcd)直播，所以今天才有時間來聽。

陳一發兒選的歌單還不錯，所以我就在Spotify建立一個[playlist](https://open.spotify.com/playlist/0ZD7FzMijE02thpUI9J9RA?si=c6e3b412975a4757)拿來聽，獨樂樂不如眾樂樂又想把自己建好的playlist放在自己的blog跟其他人分享，然後才發現原來直接用`iframe`來embed在hugo post裏面是不行的

那就在網上搜尋一下就看到有人寫好給Hugo用的spotify用的shortcode只要複製一下到theme的`.\layouts\shortcodes` folder裏面即可

# 動手改
{{< gist j-un e7d0b3118556479392bd2269f7059242 >}}
[Github Gist| spotify.html](https://gist.github.com/j-un/e7d0b3118556479392bd2269f7059242)
跟著做即可

# 試用
下面的code就是是次要embed的`iframe`
```html
<iframe style="border-radius:12px" src="https://open.spotify.com/embed/playlist/0ZD7FzMijE02thpUI9J9RA?utm_source=generator" width="100%" height="380" frameBorder="0" allowfullscreen="" allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture"></iframe>
```
那如果根據那位仁兄提供的code理論下應該改寫成這樣

```html
{{
    < spotify type="playlist" id="0ZD7FzMijE02thpUI9J9RA" width="100%" height="350" >
}}
```
> 因為直接把該code貼出來hugo會「很聰明」的把他解析為我們embed spotify，所以只能出此下策

{{< spotify type="playlist" id="0ZD7FzMijE02thpUI9J9RA" width="100%" height="350" >}}

OK! 完成