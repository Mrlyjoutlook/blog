# React Skills
**react使用技巧和最佳实践**  
本文会根据react动向持续更新！

# Tool
因为`jsx`语法浏览器上不支持，所以使用react技术栈开发都需要借助`babel`，及它的扩展插件来进行转码编译。react搭配的开发构建工具建议使用为`webpack`。

- `babel-loader`:webpack的loader插件
- `babel-preset-es2015`:es5语法
- `babel-preset-react`:react语法
- `babel-preset-stage-0`es7语法提案，已经有多个版本

# CSS
CSS的规范用法在前端发展的领域中不断的演变，从CSS模块，`Css in JavaScript`中可以看出CSS发展，尤其是React对前端HTML,JS,CSS三者一起混合使用。一个组件包含着结构、样式和逻辑，完全违背了"关注点分离"的原则，使得很多人都不适应，早期传统倡导的是html，css，js分离，到最后组件化思路，从另一个角度上思考，react的做法有利于组件的隔离。每个组件包含了所有需要用到的代码，不依赖外部，组件之间没有耦合，更加放便复用。  

但目前`Css in JavaScript`仍然没有更好的解决的方案。目前最好的方式是一个组件一个css文件，再把其引入。

```
import './styles/**.css'
```

# components

**before**

```
var React = require("react")
var Jie = React.createClass({
  render: function() {
    return <h1>Hello, {this.props.name}</h1>;
  }
});    
```

**after**

```
/*class*/
import React, {Component} from 'react'
class Jie extends Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}

/*function 无状态,对于展示类的组件，使用这类写法最好不过了*/
const Greeting = (props) => {
   return <div>{props.name}</div>
};
```

# propTypes 和 defaultProps
`propTypes` 和 `defaultProps` 是静态的属性

**before es5**

```
var Greeting = React.createClass({
  propTypes: {},
  getDefaultProps: function() {
    return {
      name: 'Mary'
    };
  },
});
```

**before es6**

```
/*function和es6 class的静态属性或方法的定义只能单独在name后*/
class JieContainer extends Component {}
JieContainer.propTypes={};
JieContainer.defaultProps={};
```

**after es7**

```
class JieContainer extends Component {
    static propTypes = {}
    static defaultProps = {}
}
```

# state,setState

## state

- es6的构造方法constructor中this关键字则代表实例对象，constructor方法默认返回实例对象（this，所以也可以指定对象返回），constructor指向类本身。
- es6类中的方法都是定义在类的prototype属性上，如同构造函数的prototype属性。
- es6继承，子类是没有自己的this对象，而是继承父类的this对象，然后对其进行加工，故需要在constructor中调用super方法。es5使用构造器方法来继承，实质是先创造子类的实例对象this，然后再将父类的方法添加到this上面（Parent.apply(this)），es6的继承实质是先调用super创造父类的实例对象this，再用子类的构造函数修改this。

**before es5**

```
var Counter = React.createClass({
  getInitialState: function() {
    return {count: this.props.initialCount};
  }
});
```

**before es6**

```
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {count: props.initialCount};
  }
}
```

**after es7**

```
class Counter extends React.Component {
    state = { expanded: false }
}
```

## setState

- setState在调用的过程中不会立即改变state中的值
- 更新state过程来引发重新绘制
- 同时多次调头setState函数所产生的效果会被合并

```
// state.count 当前为 0
this.setState({count: state.count + 1});
this.setState({count: state.count + 1});
this.setState({count: state.count + 1});
// state.count 现在是 1，而不是 3 
```

setState 支持2个参数，1：{} 或者 function(preState,props),2：function 更新成功后的回调函数  

```
上面的解决方法
this.setState((prevState) => ({
  count: prevState.count + 1
}));
```

# function this

为了确保每个方法被调用的时都绑定正确的this，以及避免渲染时重复绑定，在es6时候我们通常这么写。

```
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.toString = this.toString.bind(this);
  }

  toString(){}
}
```

**after**

```
/*由于箭头函数没有自己this，toString方法中调用this是来自类的实例*/
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.toString = this.toString.bind(this);
  }

  toString = ()=>{}
}

```

# Destructuring（解构 state,props）

**before**

```
const name = this.state.name;
//or
const namer = this.state.name;

function MyComponent(props) {
    return (
        <div>{props.name}</div>
    )
}
```

**after**

```
const {name} = this.state;
//or
const {name:namer} = this.state;

function MyComponent({name}) {
    return (
        <div>{name}</div>
    )
}
```

# JSX 三元运算符

当出现大量的判读时候，三元运算符可读性会显得极差

```
...
<div>
{
    props.bool ? ** : ** ? ** : ** ? ...
}
</div>
...
```

抛开一些依赖可以采取另一种解决方式IIFE,虽然会有点性能牺牲，但代码的维护性和可读行就相对提高很多。

...
<div>
{
    (()=>{
        if(){
            return ...
        }else if(){
            return ...
        }else ...
    })()
}
</div>
...
```

若只是判读渲染某个元素的话

```
{
  isTrue && 
    <p>True!</p>
}

//而非

{
  isTrue
   ? <p>True!</p>
   : <none/>
}
```

# Arrow Funciton

**箭头函数没有this**，当=>后面没有{}，则相当于return

```
const fun = () => {}

const fun = ({name}) => (<div>{name}</div>) === const fun = ({name}) => {return (<div>{name}</div>) }
```

# Extended operator

扩展运算符,切勿把大量数据使用扩展运算符传递，因为会造成非常昂贵的计算，应该按需传递

```
const fun = (props) => (<MyComponent {...props} />)

===

const fun = ({name,sex}) => (<MyComponent name={name} sex={sex} />)
```

# 闭包

向子组件传递function时候尽量避免直接写函数，因为每次渲染时候都会创建函数并传递到子组件，造成性能浪费。

```
...

handleChange = ()=>{}

render(){
    return(
        <ChildComponent
            onChang = {()=>{}}  // xx
            onChange = {this.handleChange}
        />
    )
}
```
