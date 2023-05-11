## 组件概念

#### 函数组件

使用 JS 的函数（或箭头函数）创建的组件，就叫做`函数组件`

```js
// 定义函数组件
function HelloFn () {
  return <div>函数组件!</div>
}

// 定义类组件
function App () {
  return (
    <div className="App">
      {/* 渲染函数组件 */}
      <HelloFn />
      <HelloFn></HelloFn>
    </div>
  )
}
export default App
```

#### 类组件

使用 ES6 的 class 创建的组件，叫做类（class）组件

```js
// 引入React
import React from 'react'

// 定义类组件
class HelloC extends React.Component {
  render () {
    return <div>类组件!</div>
  }
}

function App () {
  return (
    <div className="App">
      {/* 渲染类组件 */}
      <HelloC />
      <HelloC></HelloC>
    </div>
  )
}
export default App
```

## 组件状态

```js
import React from 'react'

class HelloC extends React.Component {
  // 必须为 state 
  state = {
    name: 'abc'
  }
  changeName = () => {
    // this.setState 来修改数据并响应式更新 UI
    this.setState({
      name: "123"
    })
  }

  render () {
    return <div>{this.state.name}
      <button onClick={this.changeName}>修改</button>
    </div>
  }
}

function App () {
  return (
    <div className="App">
      <HelloC />
    </div>
  )
}
export default App
```

## this 问题

使用`bind`指定`this`：

```js
import React from 'react'

class HelloC extends React.Component {
  constructor() {
    super()
    this.handleClick = this.handleClick.bind(this)
  }

  handleClick () {
    console.log(this)
  }

  render () {
    return (
      <div>
        <button onClick={this.handleClick}>点击</button>
      </div>
    )
  }
}

function App () {
  return (
    <div className="App">
      <HelloC />
    </div>
  )
}
export default App
```

通过箭头函数，直接沿用了父函数中的 this 的指向：

```js
import React from 'react'

class HelloC extends React.Component {

  handleClick () {
    console.log(this)
  }

  render () {
    return (
      <div>
        <button onClick={() => this.handleClick()}>点击</button>
      </div>
    )
  }
}

function App () {
  return (
    <div className="App">
      <HelloC />
    </div>
  )
}
export default App
```

## 表单处理

#### 受控表单组件

React组件的状态的地方是在state中，input表单元素也有自己的状态是在value中，React将state与表单元素的值（value）绑定到一起，由state的值来控制表单元素的值，从而保证单一数据源特性

```js
import React from 'react'

class InputComponent extends React.Component {
  // 声明组件状态
  state = {
    message: 'input value',
  }
  // 声明事件回调函数
  changeHandler = (e) => {
    this.setState({ message: e.target.value })
  }
  render () {
    return (
      <div>
        {/* 绑定value 绑定事件*/}
        <input value={this.state.message} onChange={this.changeHandler} />
      </div>
    )
  }
}


function App () {
  return (
    <div className="App">
      <InputComponent />
    </div>
  )
}
export default App
```

#### 非受控表单组件

非受控组件就是通过手动操作dom的方式获取文本框的值，文本框的状态不受react组件的state中的状态控制，直接通过原生dom获取输入框的值

```js
import React, { createRef } from 'react'

class InputComponent extends React.Component {
  // 使用createRef产生一个存放dom的对象容器
  msgRef = createRef()

  changeHandler = () => {
    console.log(this.msgRef.current.value)
  }

  render () {
    return (
      <div>
        {/* ref绑定 获取真实dom */}
        <input ref={this.msgRef} />
        <button onClick={this.changeHandler}>click</button>
      </div>
    )
  }
}

function App () {
  return (
    <div className="App">
      <InputComponent />
    </div>
  )
}
export default App
```

## React组件通信

#### 父传子

类组件：

```js
import React from 'react'

class Son extends React.Component {
  render () {
    return (
      <div> {this.props.msg}</div>
    )
  }
}

class App extends React.Component {
  state = {
    message: 'this is message'
  }
  render () {
    return (
      <Son msg={this.state.message} />
    )
  }
}
export default App
```

函数组件：

```js
import React from 'react'

function Son (props) {
  const { msg } = props
  return (
    <div>
      {msg}
    </div>
  )
}
function App () {
  const state = {
    message: 'this is message'
  }
  return (
    <div className="App">
      <Son msg={state.message} />
    </div>
  )
}

export default App
```

#### 子传父

```js
import React from 'react'

function Son (props) {
  const { getSonMsg } = props
  return (
    <div>
      <button onClick={() => getSonMsg("Son")}>点击</button>
    </div>
  )
}
function App () {
  const handleClick = (msg) => {
    console.log(msg)
  }
  return (
    <div className="App">
      <Son getSonMsg={handleClick} />
    </div>
  )
}

export default App
```

#### 兄弟通信

```js
import React from 'react'

function SonA (props) {
  const { getSonMsg } = props
  return (
    <div>
      SonA：<button onClick={() => getSonMsg("SonA中的数据")}>点击</button>
    </div>
  )
}
function SonB (props) {
  const { msg } = props
  return (
    <div>
      SonB: {msg}
    </div>
  )
}
class App extends React.Component {
  state = {
    msg: '默认数据'
  }
  handleClick = (msg) => {
    this.setState({
      msg
    })
  }
  render () {
    return <div className="App">
      <SonA getSonMsg={this.handleClick} />
      <SonB msg={this.state.msg} />
    </div>

  }
}

export default App
```

#### 跨组件通信

Context 提供了一个无需为每层组件手动添加 props，就能在组件树间进行数据传递的方法

类组件：

```jsx
import React, { createContext } from 'react'

// 1. 创建Context对象 
const { Provider, Consumer } = createContext()


// 3. 消费数据
function ComC () {
  return (
    <Consumer >
      {value => <div>{value}</div>}
    </Consumer>
  )
}

function ComA () {
  return (
    <ComC />
  )
}

// 2. 提供数据
class App extends React.Component {
  state = {
    message: 'this is message'
  }
  render () {
    return (
      <Provider value={this.state.message}>
        <div className="app">
          <ComA />
        </div>
      </Provider>
    )
  }
}

export default App
```

函数组件：

```jsx
import { createContext, useContext } from 'react'
// 创建Context对象
const Context = createContext()

function Foo () {
  return <div>Foo <Bar /></div>
}

function Bar () {
  // 底层组件通过useContext函数获取数据  
  const name = useContext(Context)
  return <div>Bar {name}</div>
}

function App () {
  return (
    // 顶层组件通过Provider 提供数据    
    <Context.Provider value={'this is name'}>
      <div><Foo /></div>
    </Context.Provider>
  )
}

export default App
```

