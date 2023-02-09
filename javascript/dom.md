# DOM
## data-* 透過dataset讀取自訂資料
```html
<ul class="list">
   <li data-num="0" data-runner="1">穩哥</li>
</ul>
```
```javascript
var list = document.querySelector('.list');

function checkList(e){
  var num = e.target.dataset.num;
  var runner = e.target.dataset.runner;
  console.log('第 '+num+ ' 位跑者');
  console.log('跑者標號是 '+runner);
}
list.addEventListener('click',checkList,false);
```