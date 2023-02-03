# 句子每的單字字首為大寫
- 例如
    - I'm A Little Tea Pot
    - Short And Stout
    - Here Is My Handle Here Is My Spout
- 作法：
    - 字串先轉為小寫
    - 利用正則表示式來取代
### 網路超厲害的解法
- 
```javascript
function titleCase(str) {
  return str
            .toLowerCase()
            .replace(/(^|\s)\S/g, L => L.toUpperCase());
}

titleCase("I'm a little tea pot");
```
### 自己的解法
```javascript
function titleCase(str) {
  const strArr = str.toLowerCase().split(' ');
  for(let i in strArr){
    strArr[i] = `${strArr[i][0].toUpperCase()}${strArr[i].slice(1)}`
  }
  return strArr.join(' ')
}

titleCase("I'm a little tea pot");
```