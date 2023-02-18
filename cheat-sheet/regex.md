# Regex小抄 (Regex cheat sheet)
## 檔名整理
- 善用 `replace()` 快速整理檔名
### 連接符號'-'
- 選取[非數字、字母與空白]的字符， /[^0-9a-zA-Z ]/g  
- 選取[空白]的字符，/\s/g
```javascript
function titleCase(str) {
    const cleanRE = /[^0-9a-zA-Z ]/g;
    const spaceRE = /\s/g;
    return str
        .toLowerCase()
        .replace(cleanRE, L => '')
        .replace(spaceRE, L => '-');
}

titleCase("I'm a little tea pot.");
// 'im-a-little-tea-pot'
```
### 駝峰命名法 camelCase
- 選取[非數字、字母]的字符， /[^0-9a-zA-Z]/g
- 選取[前面有空隔]的字符，/\s\S/g
```javascript
function titleCase(str) {
    const cleanRE = /[^0-9a-zA-Z]/g;
    const camelCaseRE = /\s\S/g;
    return str
        .toLowerCase()
        .replace(camelCaseRE, L => L.toUpperCase())
        .replace(cleanRE, L => '')
}

titleCase("I'm a little tea pot.");
// 'imALittleTeaPot'
```
### 帕斯卡命名法 PascalCase
- 選取[非數字、字母]的字符， /[^0-9a-zA-Z]/g
- 選取[句首或前面有空隔]的字符，/\s\S/g
```javascript
function titleCase(str) {
    const cleanRE = /[^0-9a-zA-Z]/g;
    const pascalCaseRE = /(^|\s)\S/g;
    return str
        .toLowerCase()
        .replace(pascalCaseRE, L => L.toUpperCase())
        .replace(cleanRE, L => '')
}

titleCase("I'm a little tea pot.");
// 'ImALittleTeaPot'
```
## 駝峰命名拆解
- `?=` 設定被某字元(字串)接續在後
```javascript
function spinalCase(str) {
  return str.split(/[^0-9a-zA-Z]|(?=[A-Z])/g).join('-').toLowerCase();
}

spinalCase('This Is Spinal Tap'); 
//this-is-spinal-tap
spinalCase("The_Andy_Griffith_Show"); 
//the-andy-griffith-show
spinalCase("Teletubbies say Eh-oh"); 
//teletubbies-say-eh-oh
spinalCase("AllThe-small Things");
//all-the-small-things
```