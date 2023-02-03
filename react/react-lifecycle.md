# 組件生命週期 Component Lifecycle
## componentDidUpdate()
- 這個method每次組件render都會被呼叫
- 可以比對前後值 (`prevProps`, `prevState`)
- 參考：[68. State Exercise2: Color Boxes Solution](https://www.udemy.com/course/modern-react-bootcamp/learn/lecture/14375646#overview)
```javascript
componentDidUpdate(prevProps, prevState){
    ...
}
```
## componentWillUnmount()
- 當組件被卸載或destroy前會呼叫此method
- 組件被卸載後setState會無效，不會re-rendering

```javascript
// 當Clock被移除時，同時也清除計時器
class Clock extends Component {
    componentDidMount(){
        this.timerID = setInterval(() => {
            this.tick();
        }, 1000)
    }
    componentWillUnmount(){
        clearInterval(this.timerID);
    }
}
```