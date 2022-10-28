---
title:       "SUSE 遊戲解題"
subtitle:    "Wechat SUSE的1024解謎遊戲"
description: "描述1024解謎遊戲的思路"
date:        2022-10-28T23:56:40+08:00
author:      "chinchio"
image:       "https://images.unsplash.com/photo-1531482615713-2afd69097998?ixlib=rb-4.0.3&dl=desola-lanre-ologun-IgUR1iX0mqM-unsplash.jpg"
tags:        ["SUSE", "1024", "game"]
categories:  ["Tech"]
---

# SUSE 遊戲解題
看完RACHER相關的線上課程後，主持人說SUSE有一個1024活動的小遊戲，那就去玩玩吧

![](https://i.imgur.com/50hDtm0.png)

---

那個鏈結的網站是[Decoding Challenge Level 1](https://games.rancher.cn/begin.html)
避免活動結束被關了，所以我在[web.archive 也做了備份](http://web.archive.org/web/20221020132226/https://games.rancher.cn/begin.html)，可以體驗一下當時的樂趣

# 解題思路

## Challenge Level 1
![](https://i.imgur.com/sP1pjyx.png)

其實看到SUSE活動的描述圖，我已經猜到是跟`rot13`有關
因為看到了**凱撒**大帝在說話了
如果不知道甚麼是`rot13`，可以去看看[ROT13 - 維基百科，自由的百科全書](https://zh.wikipedia.org/wiki/ROT13)
![](https://i.imgur.com/1n7RXot.jpg)

那如何解題呢？
看到`key=2`了嗎，那可能的解題方式就只有`rot2`或`rot24`了

配合[rot13.com](https://rot13.com/)，只要把字串貼上去就有答案了

![](https://i.imgur.com/TRtomX4.png)

```text!
i hope you didnt translate it by hand. thats what computers are for. doiog it in by hand is inefficient and that's why this text is so long. please replace "begio" in your browser with "InnovateEverywhere" and move on to the next level.
```

> 大意就是叫你不要這麼笨一個一個用手去做字母位移，電腦就是用來做這些重覆無聊的工作，這段字這麼長就是要避免你用這麼笨的方式來算出答案。將link裏面的`begio`替代成`InnovateEverywhere`就可以到下一題啦！

## Challenge Level 2
有注意到SUSE簡介有叫你用PC更容易找到答案嗎？
因為Challenge Level 1 的 URL 就是 `https://games.rancher.cn/begin.html`

那下一關的鏈結明顯就是`https://games.rancher.cn/InnovateEverywhere.html`了

進入到第二關後[Decoding Challenge Level 2](https://games.rancher.cn/InnovateEverywhere.html)
[Decoding Challenge Level 2 | WebArchive](http://web.archive.org/web/20221020133634/https://games.rancher.cn/InnovateEverywhere.html)

可以看到這張圖片
![](https://i.imgur.com/VytEGxS.png)

明顯就是叫你去看source code

Source code被我備份放在了Github Gist內：
[decoding-challenge-level-2-suse-wechat | Github Gist](https://gist.github.com/chinchio/2c85b12c2e5d96f1e4371458822df90e#file-decoding-challenge-level-2-suse-wechat-html)

> 在下面的乱码中找到稀有字母。找到这些字母后，将他们按照顺序拼接起来就可以获取到答案(这些字母都是大写呦)！
>
> 然后将获取到的单词替换掉浏览器中的 "InnovateEverywhere"，即可获取通关密码！

[Decoding Challenge Level 2 | SUSE 1024 Event (2022-10-20)](https://i.imgur.com/GMGHwVQ.png)

那很明顯就是指這些符號中隱藏了一些大寫英文Char

那思路就很明顯了
1. 只要把不是大寫字符的東西找出來刪掉就行了
2. 只要把大寫的英文字找出來就好了

### 把不是大寫字符的東西找出來刪掉
那這種方式只要透過`regular expression`即可解決
真的是piece of cake
Pattern: `\W|_`
![](https://i.imgur.com/GcELYOP.png)
其實白色沒有被選取的字就已經是答案了
![](https://i.imgur.com/qx6Lg74.png)
但這樣看還是很傷眼

因為我這台電腦沒有安裝notepad++，所以用了線上工具來幫我進行regex expression replace[Regex Replace Online Tool - Coding.Tools](https://coding.tools/regex-replace)
![](https://i.imgur.com/zXu67Vv.png)

好吧，答案呼之欲出 就是~~`SUSERANCHER`~~

那根據剛才Level 1 到 Level 2的原理，不難猜到下一個Level 的URL是...

### 把大寫的英文字找出來就好了
好了，再補充另外一種做法，但這個要自己去慢慢把答案組成字串，有點麻煩

已知`[A-Z]`意思是把大寫的英文字母找出來，那就把答案找了出來啦！
![](https://i.imgur.com/9Tt7hd5.png)
![](https://i.imgur.com/2ZWhuKW.png)
好吧，把上述的字慢慢打出來好了


## 暫時的勝利（？
學無止境可能會比較配合這個語景
![](https://i.imgur.com/8lzcPcV.png)
[Temporary End](https://games.rancher.cn/SUSERANCHER.html)
[Temporary End | WebArchive](http://web.archive.org/web/20221020134857/https://games.rancher.cn/SUSERANCHER.html)

這樣就通關了，因為說了不要把驗證碼分享給任何人，那我只要把他蓋掉好了