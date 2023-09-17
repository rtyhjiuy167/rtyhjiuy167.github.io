官网：https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-MapImageLayer.html

`MapImageLayer`允许显示和分析来自地图服务中定义的子层的数据，导出图像而不是特征。

地图服务包含一个或多个子层。子层甚至可以包含嵌套的子层。如果未指定`MapImageLayer`的`sublayers`属性，则将服务中所有子层的图像导出到客户端。如果指定了来自服务的子层的子集，则在客户机上只呈现子层的子集。

```html
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta
      name="viewport"
      content="initial-scale=1,maximum-scale=1,user-scalable=no"
    />
    <title>
      Intro to MapImageLayer | Sample | ArcGIS Maps SDK for JavaScript 4.26
    </title>

    <link
      rel="stylesheet"
      href="https://js.arcgis.com/4.26/esri/themes/light/main.css"
    />
    <script src="https://js.arcgis.com/4.26/"></script>

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

    <script>
      require(['esri/Map', 'esri/views/MapView', 'esri/layers/MapImageLayer'], (
        Map,
        MapView,
        MapImageLayer
      ) => {
        const permitsLayer = new MapImageLayer({
          portalItem: {
            id: 'd7892b3c13b44391992ecd42bfa92d01'
          }
        })

        const map = new Map({
          layers: [permitsLayer]
        })

        const view = new MapView({
          container: 'viewDiv',
          map: map
        })
        view.when(() => {
          view.goTo(permitsLayer.fullExtent).catch((error) => {
            if (error.name != 'AbortError') {
              console.error(error)
            }
          })
        })
      })
    </script>
  </head>

  <body>
    <div id="viewDiv"></div>
  </body>
</html>
```

