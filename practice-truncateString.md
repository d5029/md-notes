# 截斷文字

- 變數1：輸入字串
- 變數2：字串總量
- 截斷的字串以 ... 結尾
- num若大於str的總長度，則結尾不加 ...
```javascript
function truncateString(str, num) {
  const trunStr = str.slice(0, num);
  let result='';
  
  (str.length > num) ? result =  `${trunStr}...` : result = trunStr;
  return result 
}

truncateString("A-tisket a-tasket A green and yellow basket", 8);
truncateString("A-tisket a-tasket A green and yellow basket", "A-tisket a-tasket A green and yellow basket".length)
truncateString("A-tisket a-tasket A green and yellow basket", "A-tisket a-tasket A green and yellow basket".length + 2)

// 輸出結果：
// A-tisket...
// A-tisket a-tasket A green and yellow basket
// A-tisket a-tasket A green and yellow basket
```