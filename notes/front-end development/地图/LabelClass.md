LabelClass官网介绍：https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-support-LabelClass.html

```
labelClass.deconflictionStrategy = "static"; // static or none
```

默认情况下，标签有一个`static`以消除冲突的策略，这意味着重叠的标签将被删除，以使它们更容易阅读。目前，此属性仅适用于2D mapview中的FeatureLayer, CSVLayer和StreamLayer。

```html
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta
      name="viewport"
      content="initial-scale=1,maximum-scale=1,user-scalable=no"
    />
    <title>
      Multi-line labels | Sample | ArcGIS Maps SDK for JavaScript 4.26
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
      require([
        'esri/WebMap',
        'esri/views/MapView',
        'esri/layers/FeatureLayer',
        'esri/layers/support/LabelClass',
        'esri/symbols/TextSymbol',
        'esri/symbols/SimpleMarkerSymbol'
      ], (
        WebMap,
        MapView,
        FeatureLayer,
        LabelClass,
        TextSymbol,
        SimpleMarkerSymbol
      ) => {
        const minScale = 250000000
        const serviceUrl =
          'https://services.arcgis.com/V6ZHFr6zdgNZuVG0/arcgis/rest/services/weather_stations_010417/FeatureServer/0'
        const labelClass = new LabelClass({
          labelExpressionInfo: {
            expression: `
            var DEG = $feature.WIND_DIRECT;
            var SPEED = $feature.WIND_SPEED;
            var DIR = When( SPEED == 0, null,
              (DEG < 22.5 && DEG >= 0) || DEG > 337.5, 'N',
              DEG >= 22.5 && DEG < 67.5, 'NE',
              DEG >= 67.5 && DEG < 112.5, 'E',
              DEG >= 112.5 && DEG < 157.5, 'SE',
              DEG >= 157.5 && DEG < 202.5, 'S',
              DEG >= 202.5 && DEG < 247.5, 'SW',
              DEG >= 247.5 && DEG < 292.5, 'W',
              DEG >= 292.5 && DEG < 337.5, 'NW', null );
            var WIND = SPEED + ' mph ' + DIR;
            var TEMP = Round($feature.TEMP) + '° F';
            var RH = $feature.R_HUMIDITY + '% RH';
            var NAME = $feature.STATION_NAME;
            var labels = [ NAME, TEMP, WIND, RH ];
            return Concatenate(labels, TextFormatting.NewLine);
            `
          },
          labelPlacement: 'above-center', // 标注的位置 在点的正上方
          minScale: minScale,
          symbol: new TextSymbol({
            color: 'green',
            backgroundColor: [213, 184, 255, 0.75],
            borderLineColor: 'green',
            borderLineSize: 1,
            yoffset: 5,
            font: {
              family: 'Playfair Display', // 设置字体
              size: 12,
              weight: 'bold'
            }
          })
        })
        const view = new MapView({
          container: 'viewDiv',
          map: new WebMap({
            portalItem: {
              id: '372b7caa8fe340b0a6300df93ef18a7e'
            },
            layers: [
              new FeatureLayer({
                url: serviceUrl,
                renderer: {
                  type: 'simple',
                  symbol: {
                    type: 'simple-marker',
                    color: [75, 75, 75, 0.7],
                    size: 4,
                    outline: null
                  }
                },
                labelingInfo: [labelClass]
              })
            ]
          }),
          center: [-117.842, 33.799],
          zoom: 9
        })
      })
    </script>
  </head>

  <body>
    <div id="viewDiv"></div>
  </body>
</html>

```

