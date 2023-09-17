### MapView.hitTest

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
          }
        })
        const featureLayer = new FeatureLayer({
          id: 'hhh', //自定义 id
          outFields: ['*'],
          url: 'https://services.arcgis.com/V6ZHFr6zdgNZuVG0/arcgis/rest/services/Landscape_Trees/FeatureServer/0'
        })
        view.on('click', function (event) {
          view.hitTest(event).then(function (response) {
            if (response.results.length) {
              const feature = response.results[0]
              console.log(feature)
              const { graphic, layer } = feature
              const { geometry, attributes } = graphic
              view.goTo(graphic.geometry)
              console.log(layer.id, geometry.x, geometry.y)
              console.log(attributes)
            } else {
              console.log('Do not hit on feature')
            }
          })
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

