# 笑話票選 App

## 說明

- 從 api 抓出 10 則笑話
- 可投贊成或反對票，icon 和顏色會依據級距而改變
- 數據存在 local storage， 每次刷新都是上次那 10 則笑話
- 按"New Jokes"會有 laoding 動畫
- api 一次只能請求到一則笑話，因次要做 10 次的請求
- 笑話不能重複，所以請求可能會超過 10 次
- 按得分高低排列笑話
- https://icanhazdadjoke.com/api#fetch-a-random-dad-joke

## Components

- JokeList
  - Joke
    - up btn
    - down btn
    - votes
    - emoji icon
- loading

### 從 api 取新笑話

- 總笑話數量放在 `defaultProps`
- 在 `componentDidMount()` 執行 api
- dadjoke 的 api 要設定 `{ headers: { "Accept": "application/json" }}`

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

### 投票(贊成或反對)

- 贊成或反對的處理程序是從父層傳遞的
- 還不熟悉父層 `handleVote` 的 setState 用 map 走訪
- `handleVote` 同時處裡分數的+-

```javascript
// JokeList.js
import React, { Component } from 'react'
import Joke from './Joke';
import axios from 'axios';
import { v4 as uuidv4 } from 'uuid';


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

            jokes.push({
                id: uuidv4(),
                text: res.data.joke,
                votes: 0
            });

        }
        this.setState({
            jokes: jokes
        })
    }

    handleVote(id, delta){
        this.setState(
            st => ({
                jokes: st.jokes.map(j=>(
                    (j.id == id) ? {...j, vote: j.votes+delta} : j
                ))
            })
        )
    }
    render() {
        return (
        <div className="JokeList-jokes">
            {this.state.jokes.map(j => (
                <div>{j.joke}-{j.votes}</div>
                <Joke key={j.id} id={j.id} text={j.joke} votes={j.votes} upvote={()=>this.handleVote(j.id, 1)} downvote={()=>this.handleVote(j.id, -1)}>
            ))}
        </div>
        )
    }
}
```

```javascript
// Joke.js
import React, { Component } from 'react';

export default class Joke extends Component {
  constroctor(props) {
    super(props);
  }
  render() {
    return (
      <div class='Joke'>
        <div className='Joke-buttons'>
          <i onClick='this.props.upvote' className='fas fa-arrow-up'></i>
          <span>{this.props.votes}</span>
          <i onClick='this.props.downvote' className='fas fa-arrow-down'></i>
        </div>
        <div className='Joke-text'>{this.props.text}</div>
      </div>
    );
  }
}
```

### 動態加入 emoji 和顏色

- https://emoji-css.afeld.me/

```javascript
// Joke.js
import React, { Component } from 'react'

export default class Joke extends Component {
    getColor(){
        if(this.props.votes>=15){
            return "#4CAF50";
        }else if(this.props.votes>=15){
            return ..
        }else if(this.props.votes>=12){
            return ..
        }else if(this.props.votes>=9){
            return ..
        }else if(this.props.votes>=6){
            return ..
        }else if(this.props.votes>=3){
            return ..
        }else{
            return ...
        }
    }
    getEmoji(){
        if(this.props.votes>=15){
            return "em em-rolling_on_the_floor_laughing";
        }else if(this.props.votes>=15){
            return ..
        }else if(this.props.votes>=12){
            return ..
        }else if(this.props.votes>=9){
            return ..
        }else if(this.props.votes>=6){
            return ..
        }else if(this.props.votes>=3){
            return ..
        }else{
            return ...
        }
    }
    render() {
        return (
            <div class="Joke">
                <div className="Joke-buttons">
                    <i onClick="this.props.upvote" className="fas fa-arrow-up"></i>
                    <span className="Joke-votes" style={{border-color: this.getColor}}>{this.props.votes}</span>
                    <i onClick="this.props.downvote" className="fas fa-arrow-down"></i>
                </div>
                <div className="Joke-text">{this.props.text}</div>
                <div className="Joke-smiley">
                    <i className={this.getEmoji}>
                </div>
            </div>
        )
    }
}
```

### 同步 LocalStorage

- 執行 `window.localStorage.setItem()` 是在 `setState()` 裡面執行，用 inline arrow function。

```javascript
// JokeList.js
import React, { Component } from 'react'
import Joke from './Joke';
import axios from 'axios';
import { v4 as uuidv4 } from 'uuid';

export default class JokeList extends Component {
    static defaultProps = {
        numJokesToGet: 10
    }
    constroctor(props){
        super(props);
        this.state = {
            jokes: JSON.parse(window.localStorage.getItem("jokes") || "[]"),
        }
        this.handleClick=this.handleClick.bind(this);
    }
    // component載入
    componentDidMount(){
        if(this.state.joke.length==0)  this.getJokes();
    }

    // 取joke
    async getJokes(){
        const api_url = 'https://icanhazdadjoke.com/';
        const jokes = []
        while (jokes.length < this.props.numJokesToGet){
            let res = await axios.get(api_url, { headers: { "Accept": "application/json" }});

            jokes.push({
                id: uuidv4(),
                text: res.data.joke,
                votes: 0
            });

        }
        this.setState(st=>({
            jokes: [...st.jokes, ...jokes]
        }),
        () => window.localStorage.setItem("jokes", JSON.stringify(this.state.jokes)));
    }

    // 投票
    handleVote(id, delta){
        this.setState(
            st => ({
                jokes: st.jokes.map((j,i)=>(
                    (j.id == id) ? {...j, vote: j.votes+delta} : j
                ))
            }),
            () => window.localStorage.setItem("jokes", JSON.stringify(this.state.jokes))
        );
    }

    // 取更多
    handleClick(evt){
        this.getJokes();
    }

    render() {
        return (
        <div className="JokeList">
            <button className="JokeList-getmore" onClick={this.handleClick}>New Jokes</button>
            <div className="JokeList-jokes">
                {this.state.jokes.map(j => (
                    <div>{j.joke}-{j.votes}</div>
                    <Joke key={j.id} id={j.id} text={j.joke} votes={j.votes} upvote={()=>this.handleVote(j.id, 1)} downvote={()=>this.handleVote(j.id, -1)}>
                ))}
            </div>
        </div>
        )
    }
}
```

### Loading 動畫

- `setState()` 可以放多個 function，react 會依序執行他們

```javascript
// JokeList.js

this.state={
    ...
    loading: false
}
...
handleClick(){
    // 確保loading是true才會執行getJokes
    this.setState({loading: true}, this.getJokes)
}
async getJokes(){
    ...
    this.setState(
        st=>({
            loading: false,
            jokes: [...st.jokes, ...jokes]
        }),
        () => ...
    )
}

render(){
    if(this.state.loading){
        return (
            <div className="JokeList-spinner">
                <i className="far fa-8x fa-laugh fa-spin"></i>
                <h1 className="JokeList-title">Loading...</h1>
            </div>
        )
    }
}
```

### 檢查 joke 是否重複

- 用 Set 集合的方式

```javascript
// JokeList

...
// 取joke
async getJokes(){
    const api_url = 'https://icanhazdadjoke.com/';
    const jokes = new Set();
    while (jokes.length < this.props.numJokesToGet){
        let res = await axios.get(api_url, { headers: { "Accept": "application/json" }});

        jokes.add({
            id: uuidv4(),
            text: res.data.joke,
            votes: 0
        });

    }
    this.setState(st=>({
        loading: false
        jokes: [...st.jokes, ...jokes]
    }),
    () => window.localStorage.setItem("jokes", JSON.stringify(this.state.jokes)));
}

```

### 排序

- 在 render()的 return 前先做好排序
- 排序是使用陣列的 sort()迴調函式
- 不動 state，而是複製一份內容再執行 sort

```javascript
render(){
    if(this.state.loading){
        return (
            <div className="JokeList-spinner">
                <i className="far fa-8x fa-laugh fa-spin"></i>
                <h1 className="JokeList-title">Loading...</h1>
            </div>
        )
    }
    let jokes = this.state.jokes.sort((a,b)=>b.votes-a.votes);

    return (
        ...
        <div className="JokeList">
            <button className="JokeList-getmore" onClick={this.handleClick}>New Jokes</button>
            <div className="JokeList-jokes">
                {jokes.map(j => (
                    <div>{j.joke}-{j.votes}</div>
                    <Joke
                        key={j.id}
                        id={j.id}
                        text={j.joke}
                        votes={j.votes}
                        upvote={()=>this.handleVote(j.id, 1)}
                        downvote={()=>this.handleVote(j.id, -1)}
                    />
                ))}
            </div>
        </div>
    )
}
```

###

```javascript

```
