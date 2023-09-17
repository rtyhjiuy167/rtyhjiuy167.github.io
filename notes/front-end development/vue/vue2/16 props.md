---
title: 17 props
top: 17
tags:
  - Vue
categories:
  - Vue
---

## props

props 用于在子组件中接收父组件传入的数据。

```javascript
export default {
  props: ["name", "gender", "age"],
};
```

```html
<子组件 name="张三" gender="男" age="10">
```

#### 接收数据

对接收数据进行类型、默认值、检验设置：

```javascript
export default {
    props:{
        size: {
            type: String,
            default: 'medium',
            required: false,
            validator: v => {
                return ['large', 'medium', 'small'].includes(v);
            }
        },
    }
};
```

#### 接收的数据在子组件中修改

在`data`中定义新的变量，初值赋值为接收的数据。

```javascript
export default {
    props:{
       age: {
            type: Number,
            default: 0,
        },
    },
    data(){
        return {
            myAge: this.age
        }
    }
};
```

#### 接收的数据双向绑定

使用`this.$emit()`触发事件，调用父组件的相关方法去修改值。
