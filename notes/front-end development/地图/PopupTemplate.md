PopupTemplate官网介绍：https://developers.arcgis.com/javascript/latest/api-reference/esri-PopupTemplate.html

```html
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta
      name="viewport"
      content="initial-scale=1,maximum-scale=1,user-scalable=no"
    />
    <title>Popup actions | Sample | ArcGIS Maps SDK for JavaScript 4.26</title>

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
        'esri/layers/FeatureLayer',
        'esri/views/MapView',
        'esri/geometry/geometryEngine'
      ], (Map, FeatureLayer, MapView, geometryEngine) => {
        const map = new Map({
          basemap: 'gray-vector'
        })

        const view = new MapView({
          container: 'viewDiv',
          map: map,
          center: [-117.08, 34.1],
          zoom: 11
          // popup: {
          //   defaultPopupTemplateEnabled: true
          // }
        })

        //
        const measureThisAction = {
          title: '测量', // 按钮文本
          id: 'measure-this', //  id
          // 按钮图标
          image:
            'https://developers.arcgis.com/javascript/latest/sample-code/popup-actions/live/Measure_Distance16.png'
        }

        const template = {
          title: 'Trail run',
          content: getContent,
          actions: [measureThisAction]
        }
        function getContent(feature) {
          console.log(feature)
          return `
          <div class="esri-feature__fields esri-feature__content-element">
            <table class="esri-widget__table" summary="属性和值列表">
              <tbody>
                <tr>
                  <th class="esri-feature__field-header">名称</th>
                  <td class="esri-feature__field-data">{name}</td>
                </tr>
                <tr>
                  <th class="esri-feature__field-header">日期</th>
                  <td class="esri-feature__field-data">{time}</td>
                </tr>
              </tbody>
            </table>
          </div>
            `
        }
        const featureLayer = new FeatureLayer({
          url: 'https://services.arcgis.com/V6ZHFr6zdgNZuVG0/arcgis/rest/services/TrailRuns/FeatureServer/0',
          outFields: ['*'],
          popupTemplate: template
        })

        map.add(featureLayer)

        function measureThis() {
          const geom = view.popup.selectedFeature.geometry
          const initDistance = geometryEngine.geodesicLength(geom, 'miles')
          const distance = parseFloat(
            Math.round(initDistance * 100) / 100
          ).toFixed(2)
          view.popup.content =
            view.popup.selectedFeature.attributes.name +
            "<div style='background-color:DarkGray;color:white'>" +
            distance +
            ' miles.</div>'
        }

        // 对 id为trigger-action进行事件绑定
        view.popup.on('trigger-action', (event) => {
          if (event.action.id === 'measure-this') {
            measureThis()
          }
        })
      })
    </script>
  </head>

  <body>
    <div id="viewDiv"></div>
  </body>
</html>

```

