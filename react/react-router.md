# React Router

## Goals

- 解釋什麼是 client-side routing 以及它為何有用?
- 比較 client-side routing 與 server-side routing
- 使用 React Router 實作基本的 client-side routing

## Server-Side Routing

- 傳統的 routing 就是"Server-Side Routing"
  - `<a>` 連結觸發連覽器請求新的頁面或重置整個 DOM
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
- 使用 v6.4 版
- `<Link>` 組件要在被 router 的組件內才能使用
- `<NavLink>` 可以標記作用中的頁面

## URL 參數

### Route 的 paht

- component 的屬性值變成路徑的一部分
- 路徑結果為：http://localhost:3000/soda/XXX
- 文件
  - https://reactrouter.com/en/main/route/route#path
  - https://reactrouter.com/en/main/route/route#dynamic-segments
  - https://reactrouter.com/en/main/route/route#optional-segments

```javascript
const router = createBrowserRouter([
  {
    path: '/',
    element: <VendingMachine />,
  },
  {
    path: '/chips',
    element: <Chips />,
  },
  {
    path: '/sardines',
    element: <Sardines />,
  },
  {
    path: '/soda',
    element: <Soda name={XXX} />,
  },
]);
```

### 接收 url 的參數，使用 useParams()

- Hook 不能再 class 組件內使用
- https://zh-hans.reactjs.org/warnings/invalid-hook-call-warning.html

```javascript
const router = createBrowserRouter([
  {
    path: '/',
    element: <VendingMachine />,
  },
  {
    path: '/chips',
    element: <Chips />,
  },
  {
    path: '/sardines',
    element: <Sardines />,
  },
  {
    path: '/soda/:name',
    element: <Soda />,
  },
]);
```

```javascript
// Soda.js
import { Link, useParams } from 'react-router-dom';

function Soda() {
  const { name } = useParams();
  return <h1>SODAAAAA IS MY FAVORITE {name}</h1>;
}
```

## Route component

-

```javascript
<Route
  path="messages"
  element={<Messages />}
  loader={loadMessages}
  handle={{
    // you can put whatever you want on a route handle
    // here we use "crumb" and return some elements,
    // this is what we'll render in the breadcrumbs
    // for this route
    crumb: () => <Link to="/messages">Messages</Link>,
  }}
>
```

## React Router 的基本設定

```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import { createBrowserRouter, RouterProvider } from 'react-router-dom';
import Navbar from './Navbar';
import App from './App';
import About from './About';

const router = createBrowserRouter([
  {
    path: '/',
    element: <App />,
  },
  {
    path: 'about',
    element: <About />,
  },
]);

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    // 
    <Navbar />
    <RouterProvider router={router} />
  </React.StrictMode>
);
```
