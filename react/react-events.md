# React Events

## 目的

- 如何附加 event handler 給組件
- 使用 bind()以確保帶有 event handler 的 this 是我們要的
- 將 event handler 當作 prop 傳遞給子組件
- 理解當 map 資料時，React 對 key prop 的要求

## React Events 檢閱

- 透過 html 元素來附加 event handler 給組件
- 任何使用在 JS 的監聽事件，都能在 React 使用

例如：

- 滑鼠事件：`onClick`, `onMouseOver`等
- 表單事件：`onSubmit`等
- 鍵盤事件：`onKeyDown`, `onKeyUp`, `onKeyPress`等
- [全部列表](https://reactjs.org/docs/events.html#supported-events)

```javascript
import React, { Component } from 'react';
import './WiseSquare.css';

class WiseSquare extends Component {
  dispenseWisdom() {
    let messages = [
      /* wise messages go here */
    ];
    let rIndex = Math.floor(Math.random() * messages.length);
    console.log(messages[rIndex]);
  }

  render() {
    return (
      <div className='WiseSquare' onMouseEnter={this.dispenseWisdom}>
        😃
      </div>
    );
  }
}

export default WiseSquare;
```

## 方法綁定 Method Binding

- 當將 function 作為 handler 傳遞時，將失去 this 的前後關係

## 父層傳遞 function 給子層

Passing functions to child components

- 這在 React 是常見的模式。
- 這情況 child 通常沒有 state，但需要告知 parent 要改變 state。

### 資料如何流動 How data flows

- 父組件定義一個 function
- 該 function 透過 prop 傳遞給子組件
- 子組件調用(invoke)props
- 父層 function 被呼叫(called)，這通常是執行 setSate
- 父組件與其子組件一起重新渲染

```javascript title="demo/numbers-app/src/BetterNumList.js"
class BetterNumList extends Component {
  constructor(props) {
    super(props);
    this.state = { nums: [1, 2, 3, 4, 5] };
    this.remove = this.remove.bind(this);
  }

  remove(num) {
    this.setState((st) => ({
      nums: st.nums.filter((n) => n !== num),
    }));
  }

  render() {
    let nums = this.state.nums.map((n) => (
      <BetterNumItem value={n} remove={this.remove} />
    ));
    return <ul>{nums}</ul>;
  }
}
```

```javascript title="子層" title="demo/numbers-app/src/BetterNumItem.js"
class NumberItem extends Component {
  constructor(props) {
    super(props);
    this.handleRemove = this.handleRemove.bind(this);
  }

  handleRemove() {
    this.props.remove(this.props.value);
  }

  render() {
    return (
      <li>
        {this.props.value}
        <button onClick={this.handleRemove}>X</button>
      </li>
    );
  }
}
```

### 命名約定

- parent/child 函數傳遞的命名淺規則，
- 為保持一致性，建議遵循`action`/`handleAction`模式。
  - 在父層，function 給一個與動作相對應的名稱，如：`remove`, `add`, `open`, `toggle`等。
  - 在子層，接受來自父層傳遞的函式，使用 handle+動作來命名，如`handleRemove`, `handleAdd`, `handleOpen`, `handleToggle`等。

## Key

- map 資料和 return 組件時，會收到有關元素 list(list of elements)的警告
- 當要建立元素 list 時，key 是必須包含的特殊字串屬性(string attr)
- key 幫助 React 識別哪些項目被更改/添加/刪除
- 應該為重複的元素提供 key 以提供穩定的標識(就像 ID 那樣)
- 當沒有穩定的 ID 給渲染的項目時，可以使用迭代索引(iteration index)作為最後的手段
- 如果項目順序可能更改或項目可以刪除，請不要對 key 使用 index，這可能會導致性能問題或組件狀態錯誤
- 可以使用[UUID](https://www.npmjs.com/package/uuid), [shortid](https://www.npmjs.com/package/shortid)來為 key 提供穩定的值
