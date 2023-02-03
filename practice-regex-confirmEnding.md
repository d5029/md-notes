# Practice
## 判斷字串的字尾是否符合正則表達式
如何在正則表達式中使用變數
- 要使用 `new RegExp()`
- 使用樣板字串(template string)
```javascript
function confirmEnding(str, target){
    const regex = new RegExp(`${target}$`);
    return regex.test(str);
}
confirmEnding('Bastian', 'n');
```