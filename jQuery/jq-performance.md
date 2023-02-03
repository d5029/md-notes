# jQuery的效能議題
jquery 提供 selector 的機制，類似 CSS 抓取 DOM 元素的方式，針對網頁元素進行操控，選擇器背後的原理是 jQuery 寫定的 Regular Expression 方法，所以每次選擇都要跑一次 Regular Expression ，對於效能也有一定的影響。
## 選擇器效能 (Performance) 議題
在特殊的環境中 (Ex. 大型系統，手機 App 等)情況，效能會變成一個很重要的議題，所以平常寫 jQuery Selector 把握幾個重點，就可以大幅加速網頁的運作效率，如下：
### 【減少不必要的選擇器】
例如對於 #id2 #id1 或 tag#id1 還不如直接寫 #id1 就好。因為文件中的 id 是唯一的，前面的父元素 #id2 沒有任何必要性。
### 【多用 ID 選擇 (#id) 器來取代其他選擇器】
因為 id 選擇器可以直接呼叫 Javascript 的 getElementById() 直接定位找出該 HTML 元素，效率最高。
### 【盡量少用 Class 選擇器】
class 選擇器必須解析整份文件，效能很差。但可以配合 tag, id 等組合的選擇器，將範圍縮小後，便能更有效率的使用 class 選擇器
### 【使用 parent>child 取代 parent child】
前者是父子關係。而後者是所有後代的關係，會遞迴處理子節點和子節點所有的子節點，和其後的所有子節點。
### 【將選取出來的元素快取到記憶體中】
如果元素會重複使用到，而且選取出來的結果不會產生變化時，盡量將已經選取出來的元素儲存到變數中，避免每次使用時都必須再選取一次、再重新解析一次文件。
ps. 最常用的應該是：$this = $(this);
### 【盡量使用 Javascript 原生選取方法】
如果你只需要選取 #id1，不需要進行複雜的 jQuery selector 選取模式，盡量使用 JS 原生方法，例如 getElementById() 和 getElementByTagName() 取代 jQuery ，例如想要取用 id=element1 這個 div，可以使用以下語法：
```javascript
var elem= document.getElementById("element1");
$(elem).css("color","blue");
```