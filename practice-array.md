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
### 手刻map
- callback
```javascript
Array.prototype.myMap = function(callback) {
  const newArray = [];
  for(let i=0; i<this.length; i++){  
    newArray.push(callback(this[i], i, this))
  }
  return newArray;
};

[23, 65, 98, 5, 13].myMap(item => item * 2);
// [46, 130, 196, 10, 26]
["naomi", "quincy", "camperbot"].myMap(element => element.toUpperCase());
// ["NAOMI", "QUINCY", "CAMPERBOT"]
[1, 1, 2, 5, 2].myMap((element, index, array) => array[index + 1] || array[0]) 
//[1, 2, 5, 2, 1]
```
### 手刻filter
- callback
```javascript
Array.prototype.myFilter = function(callback) {
  const newArray = [];
  this.forEach((a,index)=>{
    if(callback(a,index,this)){
      newArray.push(a)
    }
  })
  return newArray;
};

[23, 65, 98, 5, 13].myFilter(item => item % 2);
//[23, 65, 5, 13].
["naomi", "quincy", "camperbot"].myFilter(element => element === "naomi");
//["naomi"].
[1, 1, 2, 5, 2].myFilter((element, index, array) => array.indexOf(element) === index);
//[1, 2, 5]
```
