# Best Practice

## Array

### toggle 布林陣列的某個值

題目：布林陣列的第三個值，改為反向值。

```javascript
const arr = [false, false, false, false, false];
console.log(toggleBoolean(arr, 2));

function toggleBoolean(arr, idx) {
  return [...arr.slice(0, idx), !arr[idx], ...arr.slice(idx + 1)];
}
```

### 陣列填入 5 個 false

[`fill()`](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/fill)的其他用法。

```javascript
const arr = Array(5).fill(false);
console.log(arr);
```
