nuxt有默认的 loading，可以在`auxt.config.js`中更改其样式：

```js
export default {
	//...
    loading: {
        color:'blue',
        height:'50px'
    },
	//...
}
```

也可以禁用其默认的 loading:

```js
export default {
	//...
    loading: false
	//...
}
```

也可以自己设计 loading：

```js
export default {
	//...
    loading: '~/components/组件名'
	//...
}
```

