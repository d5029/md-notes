# 過濾陣列中的falsy value(虚值)
JavaScript的虚值是：false, null, 0, "", undefined, NaN
## 我的寫法
```javascript
function bouncer(arr) {
  return arr.filter(v => { if(v) return v});
}

bouncer([7, "ate", "", false, 9]);
// [7, "ate", 9]

bouncer(["a", "b", "c"]); 
// ["a", "b", "c"]

bouncer([false, null, 0, NaN, undefined, ""]);
// []

bouncer([null, NaN, 1, 2, undefined]);
// [1, 2]
```
## 網路厲害的寫法
```javascript
function bouncer(arr) {
  return arr.filter(Boolean);
}
```