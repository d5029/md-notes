# 撲克牌
## 說明
- 隨機取牌(deal me a card!)，每次取一張，不會重複發同一張牌
- 牌面是透過API設定
- 52張徘取完時會跳出alert，提示沒有剩餘的牌了
- 網頁refresh又會得到一副全新的牌
- https://www.deckofcardsapi.com/

## 請求一個deck id
- 在 `componentDidMount()` 做請求
- 使用axios
- `componentDidMount()` 設 `async`
- 回傳的data存到state的deck，包含deck_id, remaining等

```javascript
// Deck.js
import React, { Component } from 'react'
import axios from 'axios';
const api_url = 'https://www.deckofcardsapi.com/api/deck/new/shuffle/'

export default class Deck extends Component {
    constroctor(props){
        super(props);
        this.state = {
            deck: null,
        }
    }
    // component載入
    async componentDidMount(){
        let deck = await axios.get(api_url);
        this.setState({
            deck: deck.data;
        })
    }
    render() {
        return (
        <div>Card Dealer</div>
        )
    }
}
```
## 透過Ajax取新牌
- 使用try...catch與throw以避免程式爆掉，原因是發到最後一張牌時會出錯
- 兩隻api共用 `api_base_url`
```javascript
// Deck.js
import React, { Component } from 'react'
import axios from 'axios';
const api_base_url = 'https://www.deckofcardsapi.com/api/deck';

constroctor(props){
    super(props);
    this.state = {
        deck: null,
        drawn: [],
    }
    this.getCard = this.getCard.bind(this);
}
// component載入
async componentDidMount(){
    // 修改url
    let deck = await axios.get(`${api_base_url}/new/shuffle/`);
    ...
}
// 取卡
async getCard(evt){
    evt.preventDefault();    
    const deck_id = this.state.deck.deck_id;
    try{
        const api_url = `${api_base_url}/${deck_id}/draw/`;
        let cardRes = await axios.get(api_url);
        if(!cardRes.data.success){
            throw new Error("No card remaining");
        }
        let card = cardRes.data.cards[0];
        this.setState(st => ({
            drawn: [
                ...st.drawn, 
                {
                    id: card.code,
                    imgae: card.image,
                    name: `${card.value} of ${card.suit}`
                }
            ],
        }));
    } catch(err){
        alert(err)
    }
}

render() {
    return (
      <div>
          <div>Card Dealer</div>
          <button onClick={this.getCard}>Get Card!</button>
      </div>
    )
}
```
## 新增Card組件
```javascript
// Card.js
import React, { Component } from 'react'
import axios from 'axios';

export default class Card extends Component {
    constroctor(props){
        super(props);
    }
    render() {
        return (
            <img className="Card" src={this.props.imgae} alt={this.props.name} />
        )
    }
}
```
```javascript
// Deck.js
import React, { Component } from 'react'
import Card from './Card'
import axios from 'axios';
const api_base_url = 'https://www.deckofcardsapi.com/api/deck';

export default class Deck extends Component {
    ...    
    render() {
        const cards = this.state.drawn.map(card => (
                    <Card key={card.id} imgae={card.imgae} name={card.name} />
                ))
        return (
        <div>
            <div>Card Dealer</div>
            <button onClick={this.getCard}>Get Card!</button>
            {Cards}
        </div>
        )
    }
}
```
## 紙牌亂數旋轉
- 每發一張牌card都會重新render，為確保和前一次render的狀態一致，因此需要將的資料記錄在constructor。
- 紙牌自動置中的秘密
    - left: 0;
    - right: 0;
    - margin: auto;
```javascript
// Card.js
import React, { Component } from 'react'
import axios from 'axios';

export default class Card extends Component {
    constroctor(props){
        super(props);
        let angle = this.randomNum(90,45);
        let xPos = this.randomNum(40,20);
        let yPos = this.randomNum(40,20);
        this._transform = `transform: translate(${xPos}px ${yPos}px) rotate(${angle}deg);`
    }
    randomNum(num, buffer){
        return Math.floor(Math.random()*num-buffer)
    }
    
    render() {
        return (
            <img className="Card" src={this.props.imgae} alt={this.props.name} style={{transform: this._transform}} />
        )
    }
}
```
```css
.Card{
    position: absolute;
    left: 0;
    right: 0;
    margin: auto;
}
```
## 設定紙牌與Deck的Style
```javascript
```