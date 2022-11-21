> React

React 是一个声明式，高效且灵活的用于构建用户界面的 JavaScript 库。使用 React 可以将一些简短、独立的代码片段组合成复杂的 UI 界面，这些代码片段被称作“组件”。

## 初始化引入React
```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.js'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
    <App />
);

//严格模式
/*
<React.StrictMode>
    <App />
</React.StrictMode>
*/
```

## 类组件
```jsx
import React, { Component } from 'react'

export default class App extends Component {
  render() {
    return (
      <div>App</div>
    )
  }
}

```

## 函数式组件
```jsx
function App(){
    return (
        <div>
            App
        </div>
    )
}

export default App
```
## className
在React中, 设置class时, 需要使用className来设置

## React渲染HTML
```jsx
import React, { Component } from 'react'

export default class App extends Component {
  state = {
    myhtml: `
      <h1>innerHTML</h1>
    `
  }
  render() {
    return (
      <div>
        <p dangerouslySetInnerHTML={
          {
            __html: this.state.myhtml
          }
        }></p>
      </div>
    )
  }
}
```

## ref
```jsx
import React, { Component } from 'react'

export default class App extends Component {
  mytext = React.createRef()
  render() {
    return (
      <div>
        <input type="text" ref={this.mytext} />
        
        <button onClick={() => this.handClick()}>Add4</button>
      </div>
    )
  }
  handClick = () => {
    console.log(this.mytext.current.value);
  }
}


/**
 * 
 * React并不会真正的绑定事件到每一个具体的元素上, 而是采用事件代理的模式
 */

```

## React 同步执行setState

### react18以前
```jsx
import React, { Component } from 'react'
export default class App extends Component {
  state = {
    count: 0
  }
  render() {
    return (
      <div>
        {this.state.count}
        <br />
        <button onClick={this.handClick.bind(this)}>add1</button>
        <button onClick={this.handClick1.bind(this)}>add2</button>
      </div>
    )
  }
  handClick(){
    this.setState({
      count: this.state.count+1
    })
  }
  handClick1(){
    setTimeout(()=>{
		this.setState({
		  count: this.state.count+1
		})
		this.setState({
		  count: this.state.count+1
		})
		this.setState({
		  count: this.state.count+1
		})
		console.log(this.state.count)
	})
  }
}

```

### react18后

需要使用flushSync包裹才可以同步执行
```jsx
import React, { Component } from 'react'
import {flushSync} from 'react-dom';
export default class App extends Component {
  state = {
    count: 0
  }
  render() {
    return (
      <div>
        {this.state.count}
        <br />
        <button onClick={this.handClick.bind(this)}>add1</button>
        <button onClick={this.handClick1.bind(this)}>add2</button>
      </div>
    )
  }
  handClick(){
    this.setState({
      count: this.state.count+1
    })
  }
  handClick1(){
    flushSync(()=>{
      this.setState({
        count: this.state.count+1
      })
    })
    flushSync(()=>{
      this.setState({
        count: this.state.count+1
      })
    })
    flushSync(()=>{
      this.setState({
        count: this.state.count+1
      })
    })
	console.log(this.state.count)
  }
}

```

## React props
```jsx
import React, { Component } from 'react'
import PropTypes from 'prop-types'

export default class Navbar extends Component {
  //属性类型校验
  static propTypes = {
    title: PropTypes.string,
    leftShow: PropTypes.bool
  }
  //属性设置默认值
  static defaultProps = {
    title: 'default',
    leftShow: true
  }
  render() {
    let {title, leftShow} = this.props
    return (
      <div>
        {leftShow && <button>返回</button>}
        {title}
        <button>Home</button>
      </div>
    )
  }
}

```

## 非受控组件
```jsx
import React, { Component } from 'react'

export default class App extends Component {
  username = React.createRef()
  render() {
    return (
      <div>
        <input type="text" ref={this.username} defaultValue="默认值" />
        <button onClick={()=>{
          console.log(this.username.current.value);
        }
        }>打印input值</button>
        <button onClick={()=>{
          this.username.current.value = ''
        }}>重置</button>
      </div>
    )
  }
}
```
## 受控组件
```jsx
import React, { Component } from 'react'

export default class App extends Component {
  username = React.createRef()
  state = {
    username: 'cl-ddup'
  }
  render() {
    return (
      <div>
        <input type="text" ref={this.username} value={this.state.username} onChange={(evt)=>{
          this.setState({
            username: evt.target.value
          })
        }} />
        <button onClick={(evt)=>{
          console.log(this.username.current.value);
        }
        }>打印username</button>
        <button onClick={()=>{
          this.username.current.value = ''
        }}>重置</button>
      </div>
    )
  }
}

```