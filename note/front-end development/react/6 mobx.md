Mobx是一个独立的响应式的库，可以独立于任何UI框架而存在，但是通常人们把它和React来绑定使用，用Mobx来做响应式数据建模，React作为UI视图框架渲染内容。

```bash
yarn add mobx 
```

```
# mobx配合react需要依赖，mobx-react-lite作为链接包，只能和函数组件配合
yarn add mobx-react-lite
```

`store/counter.Store.js`：

```js
import { makeAutoObservable } from 'mobx'
class CounterStore {
  // 定义数据
  count = 0
  list = [1, 2, 3, 4, 5, 6]
  // 计算属性 方法前加get
  get filterList () {
    return this.list.filter(i => i > 2)
  }
  constructor() {
    // 把数据弄成响应式
    makeAutoObservable(this)
  }
  // 定义 action 函数
  addCount = () => {
    this.count++
  }
}
// 实例化并导出
export { CounterStore }
```

`store/list.Store.js`：

```js
import { makeAutoObservable } from 'mobx'
class ListStore {
  constructor() {
    makeAutoObservable(this)
  }
}

export { ListStore }
```

`store/index.js`：

```js
import React from 'react'
import { ListStore } from './list.Store'
import { CounterStore } from './counter.Store'

// 声明 rootStore
class RootStore {
  constructor() {
    // 对子模块进行实例化操作
    this.counterStore = new CounterStore()
    this.listStore = new ListStore()
  }
}
// 实例化操作 
const rootStore = new RootStore()

//使用 react context
const context = React.createContext(rootStore)
// 通过 useContext 拿到 rootStore 实例对象
const useStore = () => React.useContext(context)
export { useStore }
```

`App.js`：

```jsx
import { useStore } from './store/index'
import { observer } from 'mobx-react-lite'
function App () {
  const rootStore = useStore()
  return (
    <div>
      {rootStore.counterStore.count}
      <button onClick={rootStore.counterStore.addCount}>+</button>
    </div>
  )
}

export default observer(App)
```

