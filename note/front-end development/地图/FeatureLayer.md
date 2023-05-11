官网：https://developers.arcgis.com/javascript/latest/sample-code/layers-featurelayer/

FeatureLayer 是一个可以从Map Service或Feature Service创建的单图层。该图层可以是空间的(具有地理特征)，也可以是非空间的(表)。

空间层由离散的特征组成，每个特征都有一个几何体，允许它在 2D MapView或 3D SceneView 中呈现为具有空间上下文的图形。特征还包含数据属性，提供关于它所代表的真实世界特征的附加信息，且属性可以在弹出窗口中查看，并用于渲染层。可以对 Featurelayer 进行查询、分析和渲染，在空间上下文中展示可视化数据。

非空间层是指没有表示地理特征的空间列的表。

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
        'esri/layers/FeatureLayer',
        'esri/Basemap',
        'esri/Map',
        'esri/views/MapView'
      ], function (TileLayer,FeatureLayer, Basemap, Map, MapView) {
        const map = new Map({
          basemap: 'hybrid'
        })
        const view = new MapView({
          container: 'viewDiv',
          map: map,
          extent: {
            xmin: -9177811,
            ymin: 4247000,
            xmax: -9176791,
            ymax: 4247784,
            spatialReference: 102100
          },
          popup:{
            defaultPopupTemplateEnabled:true
          }
        })

        const featureLayer = new FeatureLayer({
          url: 'https://services.arcgis.com/V6ZHFr6zdgNZuVG0/arcgis/rest/services/Landscape_Trees/FeatureServer/0'
        })
        map.add(featureLayer)
      })
    </script>
  </head>

  <body>
    <div id="viewDiv"></div>
  </body>
</html>
```



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
        'esri/layers/FeatureLayer',
        'esri/Basemap',
        'esri/Map',
        'esri/views/MapView'
      ], function (TileLayer, FeatureLayer, Basemap, Map, MapView) {
        const map = new Map({
          basemap: 'hybrid'
        })
        const view = new MapView({
          container: 'viewDiv',
          map: map,
          extent: {
            xmin: -9177811,
            ymin: 4247000,
            xmax: -9176791,
            ymax: 4247784,
            spatialReference: 102100
          },
        })
  
        const featureLayer = new FeatureLayer({
          url: 'https://services.arcgis.com/V6ZHFr6zdgNZuVG0/arcgis/rest/services/Landscape_Trees/FeatureServer/0',
          popupTemplate: {
            title: "主題",
            content:"<h2>高:{Height}</h2>",
          }
        })
        map.add(featureLayer)
      })
    </script>
  </head>

  <body>
    <div id="viewDiv"></div>
  </body>
</html>
```

