# Context

**Referrer: reactjs.org advanced [Context – React (reactjs.org)](https://reactjs.org/docs/context.html)**

example

```react
// 初始化 context
const ThemeContext = React.createContext('light')
ThemeContext.displayName = 'testContext' // devTool 中显示名称
console.log(ThemeContext, 'context')

class App extends React.Component {
	render() {
		return (
      {/* 包裹provider */}
			<ThemeContext.Provider value='dark'>
				<Toolbar />
			</ThemeContext.Provider>
		)
	}
}

class Toolbar extends React.Component {
	componentDidMount() {
		let value = this.context
		console.log(value, 'did mount')
	}
	render() {
		return (
			<>
				<div className={this.context}>Toolbar</div>
				<ThemedButton />
      
      	{/* 函数组件如何获取 Context */}
				<ThemeContext.Consumer>
					{value => (<div className={value}>test</div>)}
				</ThemeContext.Consumer>
			</>
		)
	}
}
Toolbar.contextType = ThemeContext // 只有赋值 contextType 后，才可以在类组件中使用 this.context

class ThemedButton extends React.Component {
  // Assign a contextType to read the current theme context.
  // React will find the closest theme Provider above and use its value.
  // In this example, the current theme is "dark".
  static contextType = ThemeContext; // 等价于 ThemedButton.contextType = ThemeContext
  render() {
    return <button className={this.context}>I am a Button</button>;
  }
}

ReactDOM.render(<App />, document.querySelector('#root'))
```

