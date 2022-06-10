# React学习笔记 Main Concepts

### 2.JSX

```react
const element = (
  <h1>
    Hello, {formatName(user)}!  
  </h1>
);
```

在 JSX 的大括号中可以放入任何 Js 表达式，如变量、类成员和函数

为了易读性，可以将 JSX 分为多行，并使用圆括号包裹防止自动插入分号

元素属性使用驼峰命名法，用 `className` 替代 `class`

**JSX 代表一个 Object**

These two examples are identical:

```react
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

it creates an object like this:

```js
// Note: this structure is simplified
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

These objects are called “React elements”



## 3.Rendering Elements

React element 是 React App 的基本组成单元, 类似于 DOM Element. 

Elements are the smallest building blocks of React apps.

An element describes what you want to see on the screen:

```
const element = <h1>Hello, world</h1>;
```

Unlike browser DOM elements, **React elements are plain objects, and are cheap to create**. React DOM takes care of updating the DOM to match the React elements.



## 4.Components and Props

可以使用函数或类创建组件

**Function and Class Components**

The simplest way to define a component is to write a JavaScript function:

```react
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

This function is a valid React component because it accepts a single “props” (which stands for properties) object argument with data and returns a React element. We call such components “function components” because they are literally JavaScript functions.

You can also use an [ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) to define a component:

```react
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

The above two components are equivalent from React’s point of view.

所有 React 组件必须是纯函数. 换句话说, `props` 是只读的, 不允许在组件函数内修改 `props`



## 5.State and Lifecycle

`state` 是组件的私有属性，类似微信小程序中的  `data` 属性

State is similar to props, but it is private and fully controlled by the component.

**Example 时钟**

It will use `this.setState()` to schedule updates to the component local state:

```react
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {    
      this.setState({      
          date: new Date()    
      });  
  }
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

使用 `this.setState()` 时需要注意

1. 不要直接修改 state ` this.state.date = new Date();`
2.  `this.setState()` 是异步行为
3.  多个 `this.setState()` 可能被合成为一个



## 6.Handling Event

React event 和 DOM 原生事件的不同:

1. 纯小写 `onclick` -- 驼峰写法 `onClick` 
2. 不能通过 return false 阻止默认行为, 必须使用 preventEvent

> Tip:
>
> 在 body 标签使用下列属性可以阻止用户鼠标选中, 右键打开菜单, 
>
> ```html
> <body oncontextmenu="return false" ondragstart="return false" onselectstart="return false" onselect="document.selection.empty()" oncopy="document.selection.empty()" onbeforecopy="return false" onmouseup="document.selection.empty()" onpaste="return false">
> ```

教程很大的篇幅都在讲如何处理回调函数 (setInterval 和 onClick 都是回调函数) 中 `this` 指向的问题, 有三种解决方法:

1. 手动绑定 如下例 **切换按钮** (官方推荐)
2. 将 handleClick 定义为箭头函数 
3. 在回调中使用箭头函数, 类似上例 **Clock** `onClick = { () => this.handleClick() } `

**Example 切换按钮**

the user toggle between “ON” and “OFF” states:

```react
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback    
    this.handleClick = this.handleClick.bind(this);  
  }

  // handleClick = () => { // ...function body }
  handleClick() {    
      // this.setState({ isToggleOn: !this.state.isToggleOn });
      this.setState(prevState => ({      
      	isToggleOn: !prevState.isToggleOn    
      }));  
  }
  render() {
    return (
      <button onClick={this.handleClick}>        
      	{this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```



接下来两节是关于 条件渲染 和 列表渲染 的, 在 vue 中通过 v:if 和 v:for 实现

## 7.Conditional Rendering

React 文档介绍了三种方法进行条件渲染

方法一: 通过声明一个变量并使用 if 赋值进而实现条件渲染

```react
render() {
    const isLoggedIn = this.state.isLoggedIn;
    let button; // 声明变量
    if(isLoggedIn) {
        button = <LoginButton />
    } else {
        button = <LogoutButton />
    }
    
    return (
    	<div>
        	{ button }
        </div>
    )
    
}
```

方法二: 在内联条件渲染

1 使用 `&&` 运算符

```react
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
          <h2>
          	You have {unreadMessages.length} unread messages.
          </h2>      
      }    
   </div>
  );
}

const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('root')
);
```

2 使用三目运算符 ? :

方法三: 在 `render` 方法中 return null 可以不进行任何渲染



## 8 Lists and Keys

本节很大一部分在介绍 `keys` 的使用场景和注意事项



## 9 Forms

In HTML, form elements such as `<input>`, `<textarea>`, and `<select>` typically maintain their own state and update it based on user input. In React, mutable state is typically kept in the state property of components, and only updated with [`setState()`](https://reactjs.org/docs/react-component.html#setstate).

TODO: 需要了解原生 HTML 如何开发 form 中的 Controlled Components <input> <select> <textarea>. 这样才能知道如何react 中的不同

react 将这些组件的 value 放在 this.state 中, 数据流向: Controlled Component => this.state => 显示



## 10 Lift State Up