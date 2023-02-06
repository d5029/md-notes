# React Router

## Goals

- 解釋什麼是 client-side routing 以及它為何有用?
- 比較 client-side routing 與 server-side routing
- 使用 React Router 實作基本的 client-side routing

## Server-Side Routing

- 傳統的 routing 就是"Server-Side Routing"
  - <a>連結觸發連覽器請求新的頁面或重置整個 DOM
- 伺服器基於 URL 的請求而決定回傳什麼 HTML，並整頁重新載入

## 假的 Client Side Routing 的缺點

- 四處亂逛其他頁面無法得到不同的 URL
- 無法使用瀏覽器回上一頁、到下一頁的按鈕
- 無法將頁面加入書籤
- 這就是為何要使用 React Router

## Client Side Routing

- 僅使用 client side routing 的網站就是 SPA(single-page-applications)
- 使用 javascript 透過名為 history 的 web api 來操作 URL bar

## React Router

- https://reactrouter.com/en/main
