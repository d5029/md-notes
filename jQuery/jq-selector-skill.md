# 選擇器技巧
## 確認有選取到元素
```javascript
if ($("#someDiv").length) {
    //hooray!!! it exists... 
}​
```
## 檢查頁面當中是否有隱藏元素
```javascript
if($(element).is(":visible") == "true") {
    //The element is Visible
}​
```
## 取得最鄰近的父元素
```javascript
$("#searchBox").closest("div");​
```