---
title:       "Pikpak API"
subtitle:    "Pikpak API 記錄"
description: "記錄一下Pikpak登入會使用到的API以及使用方法"
date:        2022-07-27T15:11:22+08:00
author:      "chinchio"
image:       "https://images.unsplash.com/photo-1623282033815-40b05d96c903?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1470&q=80"
tags:        ["pikpak", "API"]
categories:  ["Tech"]
---

# Pikpak

## Login

### Login with email address and password
利用email address 以及password進行login

- API URL: [https://user.mypikpak.com/v1/auth/signin](https://user.mypikpak.com/v1/auth/signin)

- HTTP METHOD: `POST`

---

Payload example:
```json
{
    "captcha_token": "",
    "client_id": "YNxT9w7GMdWvEOKa",
    "client_secret": "dbw2OtmVEeuUvIptb1Coyg",
    "username": "test@test.com",
    "password": "test_password"
}
```
> Comment:
> `username` 就是你註冊時使用的email address
>
> `password` 顧名思義就是你的password

> `client_id` and `client_secret` 是被寫死在js裏面，反正也不會有隱私的問題，所以我就寫上去了

---

Response example:
```json
{
    "token_type": "Bearer",
    "access_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6IjI5fdsDJCSiIsImF1ZCI6IllOeFQ5dzdHTWRXdkVPS2EiLCJleHAiOjE2NTg5MzExMfsdfds-QBR7O8UM6xcXoGF5tT48z4VfdsfdHMeWmN6igKFFWwcOfOgvAaYA",
    "refresh_token": "os.RaysBsfdsdfIowCi9PX4fdsfsdfsdfdsfdsf",
    "expires_in": 7200,
    "sub": "YR5fsdfdsBJ"
}
```

# Tasks

## Get tasks

- API URL: [https://api-drive.mypikpak.com/drive/v1/tasks](https://api-drive.mypikpak.com/drive/v1/tasks)

- HTTP METHOD: `GET`

> 必須在`header`加入`Authorization`
> 
> 而`Authorization`的**value**則為`Bearer Tokens`(JWT)
> 
> 而該`Bearer Tokens`則來自`login`時取得的`access_token`

> mumuchenchen/pikpak:
>
> [https://api-drive.mypikpak.com/drive/v1/tasks?type=offline&filters={"phase":{"eq":"PHASE_TYPE_RUNNING"}}&thumbnail_size=SIZE_LARGE&with=reference_resource](https://api-drive.mypikpak.com/drive/v1/tasks?type=offline&filters={"phase":{"eq":"PHASE_TYPE_RUNNING"}}&thumbnail_size=SIZE_LARGE&with=reference_resource)

> official pikpak:
>
> [https://api-drive.mypikpak.com/drive/v1/tasks?type=offline&thumbnail_size=SIZE_SMALL&limit=10000&filters={"phase":{"in":"PHASE_TYPE_RUNNING,PHASE_TYPE_ERROR"}}}](https://api-drive.mypikpak.com/drive/v1/tasks?type=offline&thumbnail_size=SIZE_SMALL&limit=10000&filters={"phase":{"in":"PHASE_TYPE_RUNNING,PHASE_TYPE_ERROR"}}})

---

Argu:
- type(Str):
> `offline`
- filters(Str):
> ```json
> {
>   "phase": {
>    "in": "PHASE_TYPE_RUNNING,PHASE_TYPE_ERROR"
>  }
>}
>```
or
> ```json
> {
>   "phase": {
>     "eq": "PHASE_TYPE_RUNNING"
>   }
> }
>```
- limit(int):
> any interger
- thumbnail_size(Str):
> `SIZE_SMALL` or `SIZE_LARGE`
- with(Str):
> `reference_resource`

---



# 更新Log
- 於**2022-07-27T15:11:00+08:00**寫了有關於Pikpak的`login`及`tasks`相關API