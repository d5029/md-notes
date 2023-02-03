# 函式當作參數傳遞(Function as Parameter)
將獨立的功能封裝成function，再透過參數傳遞給其他function使用。
## Say Hello
```javascript
function greet() {
    return 'Hello';
}

function name(user, func) {
    const message = func();
    console.log(`${message} ${user}`);
}

name('John', greet);
name('Jack', greet);
name('Sara', greet);
```
## 第一個偶數是誰？
- 回傳陣列中第一個符合條件的元素
### 網路厲害的解法-1
```javascript
function isEvenNumber(num){
    return num % 2 === 0
}

function findElement(arr, func) {
  return arr.find(func);
}

findElement([1, 2, 3, 4], isEvenNumber);
// 2
```
### 網路厲害的解法-2
- 使用 .map() 方法查看第一個參數“arr”中給出的陣列
- 使用第二個參數中的函式作為 arr.map() 中的callback function
- 獲取函式中第一個滿足條件的數字的index值。
- 使用該index值顯示滿足條件的第一個可用數字。
```javascript
function isEvenNumber(num){
    return num % 2 === 0
}

function findElement(arr, func) {
  return arr[arr.map(func).indexOf(true)];
}

findElement([1, 2, 3, 4], isEvenNumber);
// 2
```
### 我的解法
```javascript
function isEvenNumber(num){
    return num % 2 === 0
}

function findElement(arr, func) {
  let num = 0;
  for(let i=0; i<arr.length; i++){
    num = arr[i];
    if(func(num)){
      return num;
    }
  }
  return undefined;
}

findElement([1, 2, 3, 4], isEvenNumber);
// 2
```