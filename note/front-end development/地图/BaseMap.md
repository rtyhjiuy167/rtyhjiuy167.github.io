众多 ArcMap 应用程序中都包括可用于显示和使用操作性信息、观测值和从分析模型中获取的信息的底图。例如：

- 正射影像通常会被用作常规底图，上方可叠加业务性信息。
- 在与公共设施相关的应用中，通常将宗地边界、建筑物以及其他构建要素的土地基图作为底图。
- 许多城市地图使用街道网络作为底图，上方可显示事件点或事件等图层。

底图用于位置参考，并为用户提供叠加或聚合业务图层、执行任务以及可视化地理信息的框架。底图是执行所有后续操作和地图制图的基础，它为地理信息的使用提供了环境和框架。

官网：https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html#basemap

### 使用 ArcGIS 提供的底图

ArcGIS提供了`satellite`、`hybrid`、`oceans`、`osm`、`terrain`、`dark-gray-vector`、`gray-vector`、`streets-vector`、`streets-night-vector`、`streets-navigation-vector`、`topo-vector`、`streets-relief-vector`。

```html
<html>
  <head>
    <meta charset="utf-8" />
    <meta
      name="viewport"
      content="initial-scale=1,maximum-scale=1,user-scalable=no"
    />
    <title>demo</title>
    <style>
      html,
      body,
      #viewDiv {
        padding: 0;
        margin: 0;
        height: 100%;
        width: 100%;
      }
    </style>

    <link
      rel="stylesheet"
      href="https://js.arcgis.com/4.26/esri/themes/light/main.css"
    />
    <script src="https://js.arcgis.com/4.26/"></script>

    <script>
      require([
        'esri/Map',
        'esri/views/MapView'
      ], function (Map, MapView) {
        const map = new Map({
          basemap: 'dark-gray'
        })
        const view = new MapView({
          container: 'viewDiv',
          map: map,
        })
      })
    </script>
  </head>

  <body>
    <div id="viewDiv"></div>
  </body>
</html>
```

### 使用 url 加载底图

```html
<html>
  <head>
    <meta charset="utf-8" />
    <meta
      name="viewport"
      content="initial-scale=1,maximum-scale=1,user-scalable=no"
    />
    <title>demo</title>
    <style>
      html,
      body,
      #viewDiv {
        padding: 0;
        margin: 0;
        height: 100%;
        width: 100%;
      }
    </style>

    <link
      rel="stylesheet"
      href="https://js.arcgis.com/4.26/esri/themes/light/main.css"
    />
    <script src="https://js.arcgis.com/4.26/"></script>

    <script>
      require([
        'esri/layers/TileLayer',
        'esri/Basemap',
        'esri/Map',
        'esri/views/MapView'
      ], function (TileLayer, Basemap, Map, MapView) {
        
        const layer = new TileLayer({
          url: 'https://services.arcgisonline.com/arcgis/rest/services/World_Street_Map/MapServer'
        })
      
        const baseLayers = [layer]
        const basemap = new Basemap({
          baseLayers
        })
        const myMap = new Map({
          basemap
        })
        const view = new MapView({
          container: 'viewDiv',
          map: myMap
        })
      })
    </script>
  </head>

  <body>
    <div id="viewDiv"></div>
  </body>
</html>
```

