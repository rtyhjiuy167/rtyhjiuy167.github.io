Graphic官网介绍：https://developers.arcgis.com/javascript/latest/api-reference/esri-Graphic.html

Symbol官网介绍：https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-Symbol.html

Symbol在线调试：https://developers.arcgis.com/javascript/latest/sample-code/playground/

```html
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta
      name="viewport"
      content="initial-scale=1,maximum-scale=1,user-scalable=no"
    />
    <title> </title>
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
      require([
        'esri/Map',
        'esri/views/MapView',
        'esri/Graphic',
        'esri/geometry/Point',
        'esri/geometry/Polyline',
        'esri/geometry/Polygon'
      ], (Map, MapView, Graphic, Point, Polyline, Polygon) => {
        const map = new Map({
          basemap: 'hybrid'
        })

        const view = new MapView({
          center: [-80, 35],
          container: 'viewDiv',
          map: map,
          zoom: 3
        })

        // 创建点图形
        const point = new Point({
          longitude: -49.97,
          latitude: 41.73
        })

        // 创建一个用于绘制点的符号
        const markerSymbol = {
          type: 'simple-marker',
          color: [226, 119, 40],
          outline: {
            color: [255, 255, 255],
            width: 2
          }
        }

        // 创建图形，并将几何图形和符号添加到其中
        const pointGraphic = new Graphic({
          geometry: point,
          symbol: markerSymbol
        })

        // 创建线图形
        const polyline = new Polyline({
          paths: [
            [-111.3, 52.68],
            [-98, 49.5],
            [-93.94, 29.89]
          ]
        })

        const lineSymbol = {
          type: 'simple-line', // autocasts as SimpleLineSymbol()
          color: [226, 119, 40],
          width: 4
        }

        // 线属性，可用于弹窗展示
        const lineAtt = {
          Name: 'Keystone Pipeline',
          Owner: 'TransCanada',
          Length: '3,456 km'
        }

        // 创建线的相关要素
        const polylineGraphic = new Graphic({
          geometry: polyline, // 线的位置
          symbol: lineSymbol, // 线的样式
          attributes: lineAtt,
          popupTemplate: {
            title: '{Name}',
            content: [
              {
                type: 'fields',
                fieldInfos: [
                  {
                    fieldName: 'Name'
                  },
                  {
                    fieldName: 'Owner'
                  },
                  {
                    fieldName: 'Length'
                  }
                ]
              }
            ]
          }
        })

        // 创建一个多边形几何
        const polygon = new Polygon({
          rings: [
            [-64.78, 32.3],
            [-66.07, 18.45],
            [-80.21, 25.78],
            [-64.78, 32.3]
          ]
        })

        const fillSymbol = {
          type: 'simple-fill',
          color: [227, 139, 79, 0.8],
          outline: {
            color: [255, 255, 255],
            width: 1
          }
        }

        const polygonGraphic = new Graphic({
          geometry: polygon,
          symbol: fillSymbol
        })

        view.graphics.addMany([pointGraphic, polylineGraphic, polygonGraphic])
      })
    </script>
  </head>

  <body>
    <div id="viewDiv"></div>
  </body>
</html>

```

### SimpleLine

使用多个`Graphic`创建重叠的线实现铁路示例代码：

```html
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta
      name="viewport"
      content="initial-scale=1,maximum-scale=1,user-scalable=no"
    />
    <title> </title>
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
      require([
        'esri/Map',
        'esri/views/MapView',
        'esri/Graphic',
        'esri/geometry/Polyline',
        'esri/symbols/SimpleLineSymbol'
      ], (Map, MapView, Graphic, Polyline, SimpleLineSymbol) => {
        const map = new Map({
          basemap: 'hybrid'
        })

        const view = new MapView({
          center: [-80, 35],
          container: 'viewDiv',
          map: map,
          zoom: 3
        })

        const polyline = new Polyline({
          paths: [
            [-111.3, 52.68],
            [-98, 49.5],
            [-93.94, 29.89]
          ]
        })

        function createRailwayLine(polyline) {
          const width = 5
          return [
            new Graphic({
              geometry: polyline,
              symbol: new SimpleLineSymbol({
                cap: 'butt',
                join: 'round',
                color: [0, 0, 0],
                width: width
              })
            }),
            new Graphic({
              geometry: polyline,
              symbol: new SimpleLineSymbol({
                style: 'dash',
                cap: 'butt',
                join: 'round',
                color: [255, 255, 255],
                width: width - 2
              })
            })
          ]
        }
        const railwayLine = createRailwayLine(polyline)
        view.graphics.addMany(railwayLine)
      })
    </script>
  </head>

  <body>
    <div id="viewDiv"></div>
  </body>
</html>

```

