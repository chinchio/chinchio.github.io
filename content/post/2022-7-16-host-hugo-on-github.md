---
title:       "將Hugo網站放置在github page上"
subtitle:    "想要擁有屬於自己的website嗎？"
description: "教你如何建立自己的website"
date:        2022-07-16T21:22:17+08:00
author:      "chinchio"
image:       "https://images.unsplash.com/photo-1578728281593-a06405bf2c8e?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1173&q=80"
tags:        ["Hugo", "Github", "Github Actions", "Git", "Github Pages"]
categories:  ["Tech"]
---

# 緣由
用hugo建一個blog，把自己的學到的東西都寫上去，把他當作是學習日記好了

---

# Install

## Git

這個不用多說吧，直接在Git官網下載然後安裝即可

## Hugo for Windows

進入這個[hugo release](https://github.com/gohugoio/hugo/releases)網站
我個人是偏好是下載binary檔案然後放在env path裏面，如果不喜歡也可以到[hugo installing](https://gohugo.io/getting-started/installing/)
跟隨官方步驟使用`Chocolatey`來以command line的方式安裝`hugo`

### 下載hugo binary
![Hugo Github release page](https://i.imgur.com/Z7WYjP9.png)
因為我是要在`Windows x64`上面使用
所以在這裏我會下載`hugo_0.101.0_Windows-64bit.zip`
其他人按照自己的系統版本選擇吧！

### 解壓縮並設定path
![open hugo_0.101.0_Windows-64bit.zip with 7zip](https://i.imgur.com/NS4MV9A.png)
打開`hugo_0.101.0_Windows-64bit.zip`後
可以看到裏面有三個檔案
我們需要的就只有`hugo.exe`
我個人習慣是把這些binary file（如ffmpeg)等都放在`C:\Program Files\<program_name>`裏面
打開Environment Variable的設定工具
![system path](https://i.imgur.com/WFyRm5v.png)
把hugo的path加到path裏面去
![add hugo path as value to system variable > path](https://i.imgur.com/XWUsXoP.png)

### 測試是否正常
然後打開`terminal`，直接輸入`hugo version`看看有正確輸出hugo version相關msg
如果沒有那可能你設定path時出錯了
![excute `hugo version` on powershell](https://i.imgur.com/a8B7VpD.png)

---

# Github page

## Github page引言

[Host on GitHub | Hugo](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
要把網頁架設在Github上就會用到`GitHub Pages`(廢話)

之前我也曾經在其他Github account用hugo作為blog
那時只需要把生成的html && image && css %% js push到`main` branch即可

但如果跟隨hugo的tutorial是利用`Github Actions`讓Github自動把你寫好的`*.md`生成web page並把該生成的檔案放在`gh-pages`這條branch上


所以才會用到`gh-pages`的branch (我猜，如果有寫錯的話可以在留言區提醒 如果我有弄留言區的話 XD)

因為時間關係（誤），我已經把以username建立的repo建立好，那現在先把該repo clone 到 local再說吧
![clone github repo](https://i.imgur.com/kg6ZBbA.png)

>如果不知道為甚麼要把repo命名為`xxx.github.io`，可以參考[Github|Github Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site)裏面有說明

## hugo newsite

當然你要先用hugo建立一個網站才可以讓github幫你托管網頁，所以跟隨官方tutorial [Windows run command hugo new site](https://gohugo.io/getting-started/installing/#2-run-the-command)
```bash
hugo new site <newsite>
```
然後再去[hugo themes](https://themes.gohugo.io/)下載你喜歡的主題，安裝主題及裏面的`exampleSite`不懂請參考其他的教學

>但要注意如果你是直接把hugo themes的theme git clone下來，
有機會會在github actions的步驟裏有
`fatal: No url found for submodule path 'themes/hugo-theme-cleanwhite' in .gitmodules`
的error，所以要把theme裏面的.git刪除掉


如果你安裝主題沒有出任何錯的話應該可以在 `<newsite>` folder 下指令
```bash
hugo server
```
應該就可以看到像這樣類似的畫面
![hugo run server demo](https://i.imgur.com/u7Rg1Yn.png)

最後把`<newsite>`的檔案放在剛才clone下來的repo的folder裏面(像下圖)
![put hugo file to repo](https://i.imgur.com/knDCCbC.png)

## 使用Github Actions
做完以上的步驟，接下來學官方的教學新增一個 `gh-pages.yml` 來讓github生成webpage到`gh-pages` branch裏面
[Build Hugo With GitHub Action](https://gohugo.io/hosting-and-deployment/hosting-on-github/#build-hugo-with-github-action)
![create gh-pages.yml to .github/workflows/ folder](https://i.imgur.com/n3erE6e.png)
好了 完成上述步驟 就來上傳github的三步曲`add`, `commit`還有 `push`
你`push`完後應該可以在github actions有以下的畫面
![github action](https://i.imgur.com/Odqp1LI.png) (All green is good!)

## 來一個結尾
好了 經過這麼多工序後，終於來到最後一步

就是要把source的選項改成`gh-pages`的branch,要不然你打開`<username>.github.io`只有你`main` branch那個空空如也的`readme.md`
![github pages setting](https://i.imgur.com/wT8XOPW.png)

好了 大家快跟我一起來探討人生的意義吧
![My blog](https://i.imgur.com/gqdPPwe.png)

# reference
https://gohugo.io/hosting-and-deployment/hosting-on-github/

# thanks
Cover photo from [Thomas Jensen](https://unsplash.com/photos/05wlAhC3DM4)