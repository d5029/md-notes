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
## 使用 Object.create 建立多層繼承
- 使用這方法不能繼承父層的constructor，必須透過call()把父層的建構函式帶進來
```javascript
function Animal(family) {
 this.kingdom = '動物界';
 this.family = family || '人科';
}
Animal.prototype.move = function() {
 console.log(this.name + ' 移動');
}
function Dog(name, color, size) {
 this.name = name;
 this.color = color || ' 白色';
 this.size = size || ' 小';
}
// 這一行代表的是：Dog的原型繼承Animal的原型
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.bark = function() {
 console.log(this.name + " 吠叫");
};
var Bibi = new Dog('比比', '棕色', '小');
console.log(Bibi);
```
- Bibi console出來並沒有繼承到 `kingdom` 與 `family`
- 這時要在Dog建構子加入 `call()`，現在Bibi也有了原型Animal的屬性
```javascript
function Dog(name, color, size) {
 Animal.call(this, '犬科');
 this.name = name;
 this.color = color || ' 白色';
 this.size = size || ' 小';
}
...
console.log(Bibi);
```
## eat的動作如何共享給狗和鳥類
```javascript
function Animal() { }

Animal.prototype = {
  constructor: Animal,
  eat: function() {
    console.log("nom nom nom");
  }
};

// Only change code below this line

let duck=Object.create(Animal.prototype); // Change this line
let beagle=Object.create(Animal.prototype); // Change this line
```
## 用 `Object.create()` 記得 `constructor` 要回到各自身上
```javascript
function Animal() { }
function Bird() { }
function Dog() { }

Bird.prototype = Object.create(Animal.prototype);
Dog.prototype = Object.create(Animal.prototype);

// Only change code below this line
Bird.prototype.constructor = Bird;
Dog.prototype.constructor = Dog;


let duck = new Bird();
let beagle = new Dog();
```
## 繼承後添加 Method
- Dog object繼承自Animal
- Dog.prototype的constructor要設定成Dog
- 新增一個bark()方法給Dog object
- 因此beagle能繼承到eat()、bark()
```javascript
function Animal() { }
Animal.prototype.eat = function() { console.log("nom nom nom"); };

function Dog() { }
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor=Dog;
Dog.prototype.bark = function() { console.log("Woof!"); };

let beagle = new Dog();
console.log(beagle.eat());
// nom nom nom
console.log(beagle.bark());
// Woof!
```
## 使用 Mixin 在不相關的obj之間添加共同的行為
- 將 `glide` 的行為給 `bird` 和 `boat`
```javascript
let bird = {
  name: "Donald",
  numLegs: 2
};

let boat = {
  name: "Warrior",
  type: "race-boat"
};

function glideMixin(obj){
  obj.glide=function(){
    console.log('glide!')
  }
}
glideMixin(bird);
glideMixin(boat);
```
## 使用閉包保護對象內的屬性不被外部修改
- 在這裡 `weight` 是私變數不能被外部修改
```javascript
function Bird() {
  let weight = 15;
  this.getWeight = function() { 
    return weight;
  }
}
```
## 立即函式(IIFE)
- 宣告完馬上執行
```javascript
(function(){
  console.log("A cozy nest is ready");
})();
```
## 使用 IIFE 建立module
- `funModule` 模組有 `isCuteMixin` 與 `singMixin` 的兩個medthod
- `funModule` 應該 `return` 一個object
```javascript
let funModule = (function(){
  return {
    isCuteMixin: function(obj) {
      obj.isCute = function() {
        return true;
      };
    },
    singMixin: function(obj) {
      obj.sing = function() {
        console.log("Singing to an awesome tune");
      };
    }
  }
})();
```