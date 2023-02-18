# 模板template
## 如何製作Layout模板
```javascript
// Layout.js
import React from "react"
export default ({ children }) => (
  <div>
    <h1>我是header</h1>
    {children}
    <h1>我是footer</h1>
  </div>
)
```
```javascript
// App.js
import React from "react"
import Layout from "../components/layout"
export default () => (
  <Layout>
    <p>
      我是內容
    </p>
  </Layout>
)
```
## children
- 要用 `<元素名稱></元素名稱>` 的形式
- 使用 `this.props.children`
- 在react component中，把包在標籤中間的東西，稱為children。
- children是props之一
- component是一個模板，闢了一個位子置放透過props傳入的東西
```javascript
// Message.js
import React, { Component } from "react";
import "./Message.css";

class Message extends Component {
  render() {
    return <div className='Message'>{this.props.children}</div>;
  }
}
export default Message;
```
```javascript
// Soda.js
<Message>
    <h1>hello i am a vending machine. what would you like to eat?</h1>
</Message>
<Message>
    <h1>
    <Link to='/soda'>Soda</Link>
    </h1>
    <h1>
    <Link to='/chips'>Chips</Link>
    </h1>
    <h1>
    <Link to='/sardines'>Sardines</Link>
    </h1>
</Message>
```
