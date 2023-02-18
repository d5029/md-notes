# 前端新知
## Vite 
前端開發工具，支援多種前端框架如：react vue等，尤雨溪開發。
## framework agnostic
中文翻為"框架無關"。從[你或許看過 Vite，但你可能念錯發音了？](https://ithelp.ithome.com.tw/m/articles/10292623)知道這名詞，是跨框架開發的意思嗎?
## Hook
Hook的意思是勾住，也就是在訊息過去之前，先把訊息勾住，不讓其傳遞，使用戶可以優先處理。想要從這裡過，留下買路財。
- hook function (攔截程式) [圖](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7fc85972f28a4d009ba137103265f4c2~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)
- [可以寫自己的hook](https://medium.com/hannah-lin/react-hook-%E7%AD%86%E8%A8%98-custom-hooks-%E4%B9%8B%E6%89%93%E9%80%A0%E8%87%AA%E5%B7%B1%E7%9A%84-hook-b046f6778f33)
## Static Site Generator (SSG)
靜態網站 (Static Site)，由於沒有伺服器在運作, Static Site 比所有網站都快, 存放在多地區的寄存服務, 可以根據用戶最近位置顯示網站, 一般在1秒內可以在瀏覽器出現。

寄存服務有：
- [Netlify ](https://www.netlify.com/)
- [Vercel (Zeit) ](https://vercel.com/)
- [AWS S3 ](https://aws.amazon.com/s3/)
- [Github Pages ](https://pages.github.com/)
- [Cloudflare Workers](https://workers.cloudflare.com/)
    
Static Site 很適合內容較少更新的網站, 例子有Blog, 中小企公司網站, 文檔網站, 個人網站, Portfolio, 新聞網站等。如果網站頁面較多, 你需要另外搭建一個靜態內容管理系統 (CMS)。

製作 Static Site 需要Static Site Generator (SSG), SSG 可以把內容由markdown 格式轉換到瀏覽器支援的HTML 格式。現在較多人使用的有 Gatsby, Jekyll, Hugo, 11ty 等, 如果網站頁面多, 使用Hugo 可以快速生成大量頁面, 速度最快。

- [Gatsby](https://www.gatsbyjs.org/)
- [Jekyll](https://jekyllrb.com/)
- [Hugo](https://gohugo.io/)
- [11ty](https://www.11ty.dev/)