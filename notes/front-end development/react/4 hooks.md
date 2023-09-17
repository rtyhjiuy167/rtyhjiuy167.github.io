## Hooks

经过多年的实战，函数组件是一个更加匹配React的设计理念 `UI = f(data)`，也更有利于逻辑拆分与重用的组件表达形式，而先前的函数组件是不可以有自己的状态的，为了能让函数组件可以拥有自己的状态，所以从react v16.8开始，Hooks应运而生。

#### useState

useState为函数组件提供状态（state）

```jsx
import React, { useState } from 'react'
const App = () => {
  const [count, setCount] = useState(0)
  return (
    <button onClick={() => { setCount(count + 1) }}>{count}</button>)
}

export default App
```

`useState` 注意事项 ：

1. 1.  只能出现在函数组件或者其他hook函数中 
   2.  不能嵌套在if/for/其它函数中（react按照hooks的调用顺序识别每一个hook） 

回调函数的参数：

```jsx
import { useState } from 'react'

function Counter (props) {
  const [count, setCount] = useState(() => {
    return props.count
  })
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>{count}</button>
    </div>
  )
}

function App () {
  return (
    <>
      <Counter count={10} />
      <Counter count={20} />
    </>
  )
}

export default App
```

#### useEffect

副作用是相对于主作用来说的，一个函数除了主作用，其他的作用就是副作用。对于 React 组件来说，**主作用就是根据数据（state/props）渲染 UI**，除此之外都是副作用（比如，手动修改 DOM）

不添加依赖项时，组件首次渲染执行一次，以及不管是哪个状态更改引起组件更新时都会重新执行：

```jsx
import { useState, useEffect } from 'react'
const App = () => {
  const [count, setCount] = useState(0)
  useEffect(() => {
    console.log('副作用执行了')
  }) 
  return (
    <button onClick={() => { setCount(count + 1) }}>{count}</button>)
}

export default App
```

添加空数组依赖项时，组件只在首次渲染时执行一次：

```jsx
import { useState, useEffect } from 'react'
const App = () => {
  const [count, setCount] = useState(0)
  useEffect(() => {
    console.log('副作用执行了')
  }, [])
  return (
    <button onClick={() => { setCount(count + 1) }}>{count}</button>)
}

export default App
```

添加特定依赖项，副作用函数在首次渲染时执行，在依赖项发生变化时重新执行：

```jsx
import { useState, useEffect } from 'react'
const App = () => {
  const [count, setCount] = useState(0)
  const [flag, setFlag] = useState(false)

  useEffect(() => {
    console.log('副作用执行了')
  }, [count])

  return (
    <>
      <button onClick={() => { setCount(count + 1) }}>{count}</button>
      <button onClick={() => { setFlag(!flag) }}>{flag ? "true" : 'flase'}</button>
    </>
  )
}

export default App
```

useEffect 回调函数中用到的数据（比如，count）就是依赖数据，就应该出现在依赖项数组中。

#### 清理副作用

如果想要清理副作用 可以在副作用函数中的末尾return一个新的函数，在新的函数中编写清理副作用的逻辑

执行时机：

1. 组件卸载时自动执行
2. 组件更新时，下一个useEffect副作用函数执行之前自动执行

```jsx
import { useEffect, useState } from "react"


const App = () => {
  const [count, setCount] = useState(0)
  useEffect(() => {
    console.log("副作用生效了")
    const timerId = setInterval(() => {
      setCount(count + 1)
    }, 1000)
    return () => {
      // 用来清理副作用的事情
      clearInterval(timerId)
    }
  }, [count])
  return (
    <div>
      {count}
    </div>
  )
}

export default App
```

#### 自定义 hook

```jsx
import { useEffect, useState } from "react"

function useWindowScroll () {
  const [y, setY] = useState(0)
  window.addEventListener('scroll', () => {
    const h = document.documentElement.scrollTop
    setY(h)
  })
  return y
}

const App = () => {
  const y = useWindowScroll()
  return (
    <>
      <div style={{ position: 'fixed', background: 'red', width: '100%' }}>
        {y}
      </div>
      <div style={{ height: '12000px' }}>
      </div>
    </>
  )
}

export default App
```

#### useRef

在函数组件中获取真实的dom元素对象或者是组件对象

```jsx
import { useEffect, useRef } from 'react'
function App () {
  const h1Ref = useRef(null)
  useEffect(() => {
    console.log(h1Ref)
  }, [])
  return (
    <div>
      <h1 ref={h1Ref}>this is h1</h1>
    </div>
  )
}
export default App
```

函数组件由于没有实例，不能使用ref获取，如果想获取组件实例，必须是类组件：

```jsx
import React, { useEffect, useRef } from 'react'
class Foo extends React.Component {
  render () {
    return <div>Foo</div>
  }
}

function App () {
  const h1Foo = useRef(null)
  useEffect(() => {
    console.log(h1Foo)
  }, [])
  return (
    <div> <Foo ref={h1Foo} /></div>
  )
}
export default App
```

