```vue
<template>
  <div class="map-div" ref="mapDiv"></div>
</template>
<script setup lang="tsx">
import WebMap from '@arcgis/core/WebMap';
import MapView from '@arcgis/core/views/MapView';
import esriConfig from '@arcgis/core/config.js';
import Basemap from '@arcgis/core/Basemap';
import WebTileLayer from '@arcgis/core/layers/WebTileLayer';
import { onMounted, ref } from 'vue';

// arcgis 的key
esriConfig.apiKey = 'AAPK70284ba893e54cea88a29cc398e8e7c440V65fRd5UFjDP_foFYfjHlpLd8oQMa6DhFPJkl7yJphDQ6sQ7sLF-Zd562ZBh6y';

// 天地图的key
const tiandituKey = 'c5c56f66ba71196a73b2d6060c06e586';

// 取地图容器的dom
const mapDiv = ref<HTMLDivElement>();

// 矢量地图
var tiledLayerVec = new WebTileLayer({
  urlTemplate: `http://{subDomain}.tianditu.gov.cn/vec_w/wmts?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=vec&STYLE=default&TILEMATRIXSET=w&FORMAT=tiles&TILEMATRIX={level}&TILEROW={row}&TILECOL={col}&tk=${tiandituKey}`,
  subDomains: ['t0', 't1', 't2', 't3', 't4', 't5', 't6', 't7']
});

let basemapVec = new Basemap({
  baseLayers: [tiledLayerVec],
  title: 'basemapVec',
  id: 'basemapVec'
});

// 创建底图,默认是矢量地图
const map = new WebMap({
  basemap: basemapVec
});
// 创建地图
const view = new MapView({
  map: map,
  center: [124, 46],
  zoom: 13
});

onMounted(() => {
  // 把地图挂到容器上去
  view.container = mapDiv.value!;
});
</script>
<style>
.map-div {
  padding: 0;
  margin: 0;
  height: 600px;
  width: 1000px;
}
</style>

```

```
import '@arcgis/core/assets/esri/themes/dark/main.css';
```

