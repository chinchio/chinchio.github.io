---
title:       "Google reCAPTCHA v3學習之路 (0)"
subtitle:    "配合cloudflare worker進行人機驗證"
description: "利用Google reCAPTCHA v3及cloudflare worker來進行人機驗證"
date: 2022-09-12T09:36:57+08:00
author:      "chinchio"
image:       "https://images.unsplash.com/photo-1563207153-f403bf289096?ixlib=rb-1.2.1&dl=lenin-estrada-OI1ToozsKBw-unsplash.jpg&q=80&fm=jpg&crop=entropy&cs=tinysrgb"
tags:        ["cloudflare", "recaptcha", "cloudflare worker"]
categories:  ["Tech"]
---

# 引言
由於本人近日有閱讀一本有關於`node.js`的書本，而`node.js`據我所知就是把`javascript`可以執行在server side上面，而本人之前已經有用`goindex`拿來把google drive裏面的檔案公開在網路上方便本人存取。

# 實作
所以既然要碰`node.js`，那就拿`javascript`來做點甚麼熟悉一下。

所以在閱讀[Cloudflare Workers documentation](https://developers.cloudflare.com/workers/)後，打算做一下`Google reCAPTCHA v3`的人機驗證實作。

之前本人碰到一個將`簡體epub轉成繁體epub`的網站，裏面就是使用`Google reCAPTCHA`來進行人機驗證，目的是避免robot盜用該服務導致該server loading 過大，但其實作者沒有在server side作驗證，所以讓我輕鬆地用broser自帶的dev tool來修改程式碼達到繞過驗證的機制。

> 所以在上述事件的教訓，就是千萬不要相信使用者的input

而參考了下方數個reference後，就寫出下面可以執行在`cloudflare worker`的程式碼

# Code

```javascript
addEventListener("fetch", event => {
  event.respondWith(handleRequest(event.request))
})

var index_html = `
<!doctype html>
<head>
  <title>cloudflare worker - google recaptcha v3 demo</title>
</head>
<body>
  <h1>cloudflare worker - google recaptcha v3 demo</h1>
  <form id="verify_form" method="post" action="/verify">
    <input type="hidden" id="catcha" name="catcha" />
    <input type="submit" value="verify">
  </form>
  <script src="https://www.google.com/recaptcha/api.js?render=<YOUR-RECAPTCHA-V3-PUBLIC-KEY>"></script>
  <script>
    grecaptcha.ready(function() {
      grecaptcha.execute('<YOUR-RECAPTCHA-V3-PUBLIC-KEY>', {action: 'submit'}).then(function(token) {
        // Add your logic to submit to your backend server here.
        document.getElementById("catcha").value = token;
        document.getElementById("pathname").value = document.location.pathname;
      });
    });
  </script>
</body>
`

const recaptcha_secret_key = "<YOUR-RECAPTCHA-V3-SECRET-KEY>";

async function gatherResponse(response) {
	/*Copy from [Fetch JSON | Cloudflare Workers docs]*/
    const { headers } = response;
    const contentType = headers.get('content-type') || '';
    if (contentType.includes('application/json')) {
        return JSON.stringify(await response.json()); //這才是最重要的部份
    } else if (contentType.includes('application/text')) {
        return response.text();
    } else if (contentType.includes('text/html')) {
        return response.text();
    } else {
        return response.text();
    }
}

async function handleRequest(request) {
    const url = new URL(request.url);
    const {pathname, search} = url;
    switch (pathname) {
    case "/verify":
        const formData = await request.formData();
        const body = {};
        for (const entry of formData.entries()) {
            body[entry[0]] = entry[1];
        }
        json_formData = JSON.stringify(body);
        catcha = body["catcha"];
        console.log(body["catcha"]);

        const isUrlValid =
        'https://www.google.com/recaptcha/api/siteverify?secret=' +
        recaptcha_secret_key + '&response=' + catcha

        const response = await fetch(isUrlValid);
        const google_recaptcha_v3_verify_results = await gatherResponse(response);
        json_verify_results = JSON.parse(google_recaptcha_v3_verify_results)

        output = `\ncatcha: ${catcha}\ngoogle_recaptcha_v3_verify_results: ${google_recaptcha_v3_verify_results}`

        if(json_verify_results["success"]){
            return new Response("You are human!" + output);
        }else{
            return new Response("You are robot!!!" + output);
        }
    }
    return new Response(index_html, {
        headers: {
        'content-type': 'text/html;charset=UTF-8',
        },
    });
}
```

# 實際demo
如果各位有使用過註冊一下服務應該會看過Google reCaptcha，之前的驗證方法是給你書本中的圖片，要你去輸入該圖片中的文字，據說是請人類來幫忙把圖書電子化~(即是人肉OCR)~，但reCaptcha v3據說是通過使用者的操作去檢驗該操作是否人類所為。

Google reCaptcha v3 會在網頁的右下角就會有這樣的提示，相當容易辨認。
![demo index](https://i.imgur.com/fm04bm9.png)

而如果timeout或者reCaptcha v3認為你不是人為操作就會顯示類似如下(下圖是timeout所致)
![not success with google recaptcha v3 challenge](https://i.imgur.com/k3rEmeU.png)

如果檢測是人為操作便會有像下圖的顯示
![success with google recaptcha v3 challenge](https://i.imgur.com/RC5Ii0X.png)

值得注意的是本人寫的程式去檢驗是否人機是由recaptcha v3的verify的`success`(bool) value來決定，我暫時沒有辦法找到recaptcha v3 document裏面有提及`score`多少才決定`success`才為true，當然您也可以透過`score`來的值來決定是否把該操作視為人為。

# Demo Link
[cloudflare worker - google recaptcha v3 demo](https://google-recaptcha-v3-demo.cf-learn.workers.dev/)

#reference: 
- [How to Add Google ReCaptcha v3 in Node Js App](https://remotestack.io/how-to-add-google-recaptcha-v3-in-node-js-app/)
- [reCAPTCHA v3 | Google Developers](https://developers.google.com/recaptcha/docs/v3)
- [Read POST | Cloudflare Workers docs](https://developers.cloudflare.com/workers/examples/read-post/)
- [Fetch JSON | Cloudflare Workers docs](https://developers.cloudflare.com/workers/examples/fetch-json/)