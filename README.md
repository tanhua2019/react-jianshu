#ref的使用
```JavaScript
import React, { Component } from 'react'

export default class input extends Component {
  constructor(props) {
    super(props)
    this.state = ({
      inputValue: '',
      list: []
    })
    this.inputChange = this.inputChange.bind(this)
    this.handleClick = this.handleClick.bind(this)
  }
  render() {
    return (
      <div>
        <input ref={(input) => this.input = input} value={this.state.inputValue} onChange={this.inputChange}></input>
        <button onClick={this.handleClick}>按钮</button>
        <ul ref={(ul) => this.ul = ul}>{this.getList()}</ul>
      </div>
    )
  }
  inputChange(e) {
    // console.log(e.target.value);
    // this.setState({
    //   inputValue: e.target.value
    // })
    console.log(this.input.value);
    this.setState({
      inputValue: this.input.value
    })
  }
  handleClick() {
    this.setState((prevState) => ({
      list: [...prevState.list, prevState.inputValue],
      inputValue: ''
    }), () => {
      console.log(this.ul.querySelectorAll('li').length);
    })
    // console.log(this.ul.querySelectorAll('li').length);
  }
  getList() {
    return this.state.list.map(item => {
      return <li key={item}>{item}</li>
    })
  }
}


```
- 当input的值发生变化的时候 inputChange()就会调用  e.target 他就是一个dom元素节点 可以用另外一种方式获取到dom节点
- 在input中可以使用一个ref的参数 在react16中ref 等于一个箭头函数，会自动接收一个参数，可自定义 ref={(input) => {this.input = input}}
- 我们构建了一个ref的引用，引用叫做this.input，指向input对应的dom节点

![关于setState参数问题](https://img-blog.csdnimg.cn/2019052622004465.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2R1Z3VzaGFuZ2xpYW5n,size_16,color_FFFFFF,t_70)

##实际上不推荐使用ref 有时候会遇到问题 react建议用数据驱动的方式编写代码 
 - button点击获取li的长度 会发现每次长度都会少一个 因为setState是一个异步函数不会立即执行，log可能在setState之前就执行了
 - 如果需要等setState异步执行完了之后再执行，setState有第二个参数