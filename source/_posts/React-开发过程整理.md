title: React 开发过程整理
author: TangJing
tags: []
categories: []
date: 2018-04-14 10:35:00
---
组件之间通信的常用3种场景(实际上是4种)
<!-- more -->

父子组件传值

###### 父组件传值于子组件
```javaScript
class Parent extends Component {
    constructor(props) {super(props);this.state = { parentValue: "value" }} //需要传递给子组件的值
    render() { return (<Son parentValue={this.state.Component} />) } //通过组件绑定到子组件的 props 属性中
}


class Son extends Component {
    constructor(props) { super(props);this.state = {}}
    componentDidMount = () => {this.setState({parentValue: this.props.parentValue,});} //子组件挂载时从 props 中取
    render() {return (<div/>)}
}
```
###### 子组件传值于父组件(和子组件调用父组件的方法是同一种实现方式)
```javaScript
class Son extends Componet {
    constructor(props) { super(props); this.state = { sonValue: "sonValue" } } //需要传递给父组件的值
    sonValueChange = (value) => { this.props.callbackParent(this.state.sonValue) } //通过调用父组件的回调方法传递子组件的值
    render() { return (<Button onClick={this.sonValueChange} />); }
}

class Parent extends Component {
    constructor(props) { super(props); this.state = {} }
    callbackParent = (SonValue) => { this.setstate({ SonValue: SonValue }) } //定义父组件的回调方法获取子组件的值
    render() { return (<Son callbackParent={this.callbackParent} /> } //通过组件绑定回调方法
}
```
###### 父组件调用子组件的方法
``` javaScript
class parent extends Component {
    constructor(props) { super(props) }
    componentDidMount=()=>{ this._sonComponet.sonMethod} //父组件调用子组件  
    render() {return (<Son ref={sonComponet=>this._sonComponet=sonComponet}/>)} //组件上将子组件绑定到this上
}

class Son extends Componet {
    constructor(props) { super(props) }
    sonMethod=()=>{console.log("son method")}//子组件方法
    render() {return (<div />)}
}

```