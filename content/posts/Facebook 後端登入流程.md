Title: Facebook 後端登入流程
Slug: Facebook 後端登入流程
Date: 2019-05-20 12:00
Category: Note
Tags: Facebook Login

參考官方文件 [Manually Build a Login Flow](https://developers.facebook.com/docs/facebook-login/manually-build-a-login-flow/)，用 Python 實作使用者從前端按下登入按鈕，跳出 Facebook Login Dialog 確認授權後，前端再打後端 api，讓後端取得 access token，完成登入流程。

完整程式碼在 [這邊](https://github.com/badboy99tw/demo-facebook-login-in-backend)。

幾個重點：

* 前端要有地方可以跳出 Facebook Login Dialog
* 前端實作 redirect api，讓 Facebook 授權成功後打回來
* 後端實作 login api，讓前端拿到授權成功的 code 後可以打
* 前端要處理 CSRF Token

目前還沒處理 CSRF Token，但只是 demo 的話好像也不需要，有空再說吧。
