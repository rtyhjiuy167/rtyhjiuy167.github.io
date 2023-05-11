---
title: 7 css样式
top: 7
tags:
  - Vue
categories:
  - Vue
---

## class 样式

`:class`与`class`可共存。

1）

```html
<div :class="{'is-active':active}"></div>
```

```javascript
new Vue({
    el: '#APP',
    data: {
        active: true
    },
});
```

2）

```html
<div :class="`is-${direction}`"></div>
```

```javascript
new Vue({
    el: '#APP',
    data: {
        direction: "left"
    },
});
```

......

## style 样式

`:style`与`style`可共存。

1）

```html
<div :style="{backgroundColor:bgColor}"></div>
```

```javascript
new Vue({
    el: '#APP',
    data: {
        bgColor: 'red'
    },
});
```

2）

```html
<div :style="[style1,style2]"></div>
```

```javascript
new Vue({
    el: '#APP',
    data: {
        style1: {
            fontSize: '14px',
            color: 'red'
        },
        style2: {
            backgroundColor: 'yellow'
        }
    },
});
```

3）

```html
<div :style="styleObj"></div>
```

```javascript
new Vue({
    el: '#APP',
    data: {
        styleObj: {
            fontSize: '14px',
            color: 'red'
        },
    },
});
```

......