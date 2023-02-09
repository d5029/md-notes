# 建構函式(constructor function)
## 定義
- 命名開頭要大寫，以區別於其他非建構函式的函式
- 使用關鍵字 `this` 來設置它們將創建的屬性，在構造函數內部，this 指的是它將創建的新對象。
- 建構函式定義屬性和行為，而不是像其他函數那樣返回一個值。
```javascript
function Bird() {
    this.name = "Albert";
  this.color = "blue";
  this.numLegs = 2;
}
```
## 檢查是否為建構函式的實例 `instanceof`
- 回傳 true or false
```javascript
let Bird = function(name, color) {
  this.name = name;
  this.color = color;
  this.numLegs = 2;
}

let crow = new Bird("Alexis", "black");

crow instanceof Bird;
```
## 檢查屬性是否存在 `hasOwnProperty`
- 回傳 true or false
```javascript
let ownProps = [];

for (let property in duck) {
  if(duck.hasOwnProperty(property)) {
    ownProps.push(property);
  }
}

console.log(ownProps);
// ["name", "numLegs"]
```
## 使用 `prototype` 新增原型的屬性
- 已經instance的也能繼承新增的屬性
```javascript
function Dog(name) {
  this.name = name;
}

let beagle = new Dog("Snoopy");
Dog.prototype.numLegs = 4;
console.log(beagle.numLegs);
// 4
```
## 屬性有兩種類型
- own property，在定義 `constructor` 時設定的屬性
- prototype property，透過 `prototype` 定義的屬性
- own property的層級高於prototype property，當在own property找不到指定的屬性時，才會往上一階繼續找
- 參考：[繼承屬性](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Inheritance_and_the_prototype_chain#%E7%B9%BC%E6%89%BF%E5%B1%AC%E6%80%A7)
```javascript
function Bird(name) {
  this.name = name;  //own property
}

Bird.prototype.numLegs = 2; // prototype property

let duck = new Bird("Donald");


let ownProps = [];
let prototypeProps = [];

for (let property in duck) {
  if(duck.hasOwnProperty(property)) {
    ownProps.push(property);
  } else {
    prototypeProps.push(property);
  }
}

console.log(ownProps);
// ["name"]
console.log(prototypeProps);
// ["numLegs"]
```
## 另一個檢查實例的方法
- own property，在定義 `constructor` 時設定的屬性
- prototype property，透過 `prototype` 定義的屬性
```javascript
function Dog(name) {
  this.name = name;
}

function joinDogFraternity(candidate) {
 if (candidate.constructor === Dog) {
    return true;
  }else{
    return false;
  };
}

const dog1 = new Dog('dog1');
const dog2 = new Dog('dog2');

console.log(joinDogFraternity(dog1));
// true
console.log(joinDogFraternity(dog2));
// true
```
## 手動一次增加很多屬性
```javascript
function Dog(name) {
  this.name = name;
}

Dog.prototype = {
  numLegs: 4,
  eat: function(){
    console.log("nom nom nom")
  },
  describe: function(){
    console.log("My name is " + this.name)
  }
};
```
## 給prototype設定新obj的副作用(side effect)
- f.prototype = {b:3,c:4}; 
- 這樣的寫法是給f函式的原型設定一個新的obj，這樣的寫法會破壞原型鏈
- 因此要在定義新的obj時需加入 `constructor` 屬性，才能用 `dog1.constructor === Dog` 的方法做檢查
```javascript
function Dog(name) {
  this.name = name;
}

Dog.prototype = {
  constructor: Dog,
  numLegs: 4,
  eat: function() {
    console.log("nom nom nom");
  },
  describe: function() {
    console.log("My name is " + this.name);
  }
};

const dog1 = new Dog('dog1');
console.log(dog1.constructor === Dog)
// true
console.log(dog1.constructor === Object)
// false
console.log(dog1 instanceof Dog)
// true
```
## 檢查A繼承的原型(prototype)是否來自B建構函式(constructor function)
- isPrototypeOf
```javascript
function Bird(name) {
  this.name = name;
}

let duck = new Bird("Donald");
Bird.prototype.isPrototypeOf(duck);
// true
```
## 原型鍊(Prototype Chain)
```javascript
let f = function () {
   this.a = 1;
   this.b = 2;
}
let o = new f(); // {a: 1, b: 2}

// 接著針對 f 函式的原型添加屬性
f.prototype.b = 3;
f.prototype.c = 4;

console.log(o.a); // 1
console.log(o.b); // 2
// o 有屬性「b」嗎？有，該數值為 2。
// o 還有個原型屬性「b」，但這裡沒有被訪問到。
// 這稱作「property shadowing」
console.log(o.c); // 4
// o 有屬性「c」嗎？沒有，那就找 o 的原型看看。
// o 在「o.[[Prototype]]」有屬性「c」嗎？有，該數值為 4。
console.log(o.d); // undefined
// o 有屬性「d」嗎？沒有，那就找 o 的原型看看。
// o 在「o.[[Prototype]]」有屬性「d」嗎？沒有，那就找 o.[[Prototype]] 的原型看看。
// o.[[Prototype]].[[Prototype]] 是 Object.prototype，預設並沒有屬性「d」，那再找他的原型看看。
// o 在「o.[[Prototype]].[[Prototype]].[[Prototype]]」是 null，停止搜尋。
// 找不到任何屬性，回傳 undefined。
```
- 不要寫 f.prototype = {b:3,c:4}; 因為它會破壞原型鏈
- o.[[Prototype]] 有 b 與 c 的屬性：{b: 3, c: 4}
- o.[[Prototype]].[[Prototype]] 是 Object.prototype.
- 最後 o.[[Prototype]].[[Prototype]].[[Prototype]] 成了 null
- 這是原型鏈的結末，因為 null 按照定義並沒有 [[Prototype]]。
- 因此，整個原型鏈看起來就像：
- {a: 1, b: 2} ---> {b: 3, c: 4} ---> Object.prototype ---> null