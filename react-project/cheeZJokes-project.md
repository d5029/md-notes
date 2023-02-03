# 笑話票選 App
## 說明
- 從api抓出10則笑話
- 可投贊成或反對票，icon和顏色會依據級距而改變
- 數據存在local storage， 每次刷新都是上次那10則笑話
- 按"New Jokes"會有laoding動畫
- api一次只能請求到一則笑話，因次要做10次的請求
- 笑話不能重複，所以請求可能會超過10次
- 按得分高低排列笑話
- https://icanhazdadjoke.com/api#fetch-a-random-dad-joke
## Components
- joke board
    - JokeList
        - joke item
            - up btn
            - down btn
            - score
            - emoji icon
- loading
### 從api取新笑話
- 總笑話數量放在 `defaultProps`
- 在 `componentDidMount()` 執行api
- dadjoke的api要設定 `{ headers: { "Accept": "application/json" }}`
```javascript
// JokeList.js
import React, { Component } from 'react'
import axios from 'axios';

export default class JokeList extends Component {
    static defaultProps = {
        numJokesToGet: 10
    }
    constroctor(props){
        super(props);
        this.state = {
            jokes: [],
        }
    }
    // component載入
    async componentDidMount(){
        const api_url = 'https://icanhazdadjoke.com/';
        const jokes = []

        while (jokes.length < this.props.numJokesToGet){
            let res = await axios.get(api_url, { headers: { "Accept": "application/json" }});
            jokes.map(j=>(
                if(res.data.id != j.id){
                    jokes.push(res.data);
                }
            ))
        }
        this.setState({
            jokes: jokes;
        })
    }
    render() {
        const jokes = this.state.jokes.map(j=>(
            return `<div>${j.joke}</div>`
        ))
        return (
        <div>{jokes}</div>
        )
    }
}
```
### 
```javascript
```
### 
```javascript
```
### 
```javascript
```