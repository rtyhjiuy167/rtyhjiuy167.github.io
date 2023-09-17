网址：https://www.npmjs.com/package/echarts-for-vue

##### 安装

```bash
npm i echarts-for-vue
```

`main.js`或`main.ts`：

```javascript
import { createApp, h } from "vue";
import App from "./App.vue";
import { plugin } from "echarts-for-vue";
import * as echarts from "echarts";
const app = createApp(App);
app.use(plugin, { echarts, h });
app.mount("#app");
```

## 折线图

```vue
<template>
  <ECharts ref="chart" :option="option" />
</template>

<script setup>
import { createComponent } from "echarts-for-vue";
import { h } from "vue";
import * as echarts from "echarts";
const ECharts = createComponent({ echarts, h });
const option = {
  xAxis: {
    type: "category",
    // 这个 data 是X轴上的数据
    data: ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
  },
  yAxis: {
    type: "value",
  },
  series: [
    {
      // 这个是Y轴上的数据
      data: [150, 230, 224, 218, 135, 147, 260],
      type: "line",
    },
  ],
};
</script>
```

##### 饼图

```javascript
const option = {
    series: [
        {
            // 图形
            type: "pie", // 饼图
            // 内径 外径
            radius: ["90%", "100%"], //  去容器的宽和高的最大值的百分比作为半径
            // 每个 item 的标签
            label: false, // 不展示
            // 鼠标移至图形上的动画
            emphasis: {
                scale: 0, // 加倍
            }, 
            data: [{ value: 484 }, { value: 300 }],
        }
    ]
}
```

