`VectorTileLayer`访问缓存的数据块，并以矢量格式呈现。它类似于缓存上下文中的`WebTileLayer`。`WebTileLayer`呈现的是一系列图像，而不是矢量数据。`VectorTileLayer`可以适应其显示设备的分辨率，并且可以为多种用途重新设置样式。`VectorTileLayer`提供有样式的地图，同时利用缓存的栅格地图瓷砖和矢量地图数据。

```html
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=Edge" />
    <meta
      name="viewport"
      content="initial-scale=1,maximum-scale=1,user-scalable=no"
    />
    <title>
      VectorTileLayer - update style layers | Sample | ArcGIS Maps SDK for
      JavaScript 4.26
    </title>
    <style>
      html,
      body,
      #viewDiv {
        padding: 0;
        margin: 0;
        height: 100%;
        width: 100%;
      }

      #topbar {
        padding: 10px;
      }

      .action-button {
        font-size: 14px;
        height: 32px;
        margin-top: 10px;
        border: 1px solid #0079c1;
        color: #0079c1;
      }

      .action-button:hover {
        background: #0079c1;
        color: #e4e4e4;
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
        'esri/views/MapView',
        'esri/layers/VectorTileLayer'
      ], (Map, MapView, VectorTileLayer) => {
        const map = new Map()

        const view = new MapView({
          container: 'viewDiv',
          map: map,
          center: [38.5795, 39.8282],
          zoom: 2
        })

        // change the font case for the countries(Admin0 point/large) labels layer
        document
          .getElementById('layoutButton')
          .addEventListener('click', () => {
            // get the layout properties for the Admin0 point/large layer
            const styleLayer = vtLayer.getStyleLayer('Admin0 point/large')

            // change the text-transform layout property
            styleLayer.layout['text-transform'] =
              styleLayer.layout['text-transform'] == 'uppercase'
                ? 'none'
                : 'uppercase'
            vtLayer.setStyleLayer(styleLayer)
          })

        // change the fill-color for the water(Marine area/1) layer
        document.getElementById('paintButton').addEventListener('click', () => {
          // get the paint properties for the marine area/1 layer
          const paintProperties = vtLayer.getPaintProperties('Marine area/1')

          // change the fill-color paint property for the layer.
          paintProperties['fill-color'] =
            paintProperties['fill-color'] == '#93CFC7' ? '#0759C1' : '#93CFC7'
          vtLayer.setPaintProperties('Marine area/1', paintProperties)
        })

        // load a new vector tile layer from JSON object
        const vtLayer = new VectorTileLayer({
          style: {
            layers: [
              {
                layout: {},
                paint: {
                  'fill-color': '#F0ECDB'
                },
                source: 'esri',
                minzoom: 0,
                'source-layer': 'Land',
                type: 'fill',
                id: 'Land/0'
              },
              {
                layout: {},
                paint: {
                  'fill-pattern': 'Landpattern',
                  'fill-opacity': 0.25
                },
                source: 'esri',
                minzoom: 0,
                'source-layer': 'Land',
                type: 'fill',
                id: 'Land/1'
              },
              {
                layout: {},
                paint: {
                  'fill-color': '#93CFC7'
                },
                source: 'esri',
                minzoom: 0,
                'source-layer': 'Marine area',
                type: 'fill',
                id: 'Marine area/1'
              },
              {
                layout: {},
                paint: {
                  'fill-pattern': 'Marine area',
                  'fill-opacity': 0.08
                },
                source: 'esri',
                'source-layer': 'Marine area',
                type: 'fill',
                id: 'Marine area/2'
              },
              {
                layout: {
                  'line-cap': 'round',
                  'line-join': 'round'
                },
                paint: {
                  'line-color': '#cccccc',
                  'line-dasharray': [7, 5.33333],
                  'line-width': 1
                },
                source: 'esri',
                minzoom: 1,
                'source-layer': 'Boundary line',
                type: 'line',
                id: 'Boundary line/Admin0/0'
              },
              {
                layout: {
                  'text-font': ['Risque Regular'],
                  'text-anchor': 'center',
                  'text-field': '{_name_global}'
                },
                paint: {
                  'text-halo-blur': 1,
                  'text-color': '#AF420A',
                  'text-halo-width': 1,
                  'text-halo-color': '#f0efec'
                },
                source: 'esri',
                'source-layer': 'Continent',
                type: 'symbol',
                id: 'Continent'
              },
              {
                layout: {
                  'text-font': ['Atomic Age Regular'],
                  'text-field': '{_name}',
                  'text-transform': 'none'
                },
                paint: {
                  'text-halo-blur': 1,
                  'text-color': '#AF420A',
                  'text-halo-width': 1,
                  'text-halo-color': '#f0efec'
                },
                source: 'esri',
                minzoom: 2,
                'source-layer': 'Admin0 point',
                maxzoom: 10,
                type: 'symbol',
                id: 'Admin0 point/large'
              }
            ],
            glyphs:
              'https://basemaps.arcgis.com/arcgis/rest/services/World_Basemap_v2/VectorTileServer/resources/fonts/{fontstack}/{range}.pbf',
            version: 8,
            sprite:
              'https://www.arcgis.com/sharing/rest/content/items/7675d44bb1e4428aa2c30a9b68f97822/resources/sprites/sprite',
            sources: {
              esri: {
                url: 'https://basemaps.arcgis.com/arcgis/rest/services/World_Basemap_v2/VectorTileServer',
                type: 'vector'
              }
            }
          }
        })
        map.add(vtLayer)

        view.ui.add('topbar', 'top-right')
      })
    </script>
  </head>

  <body>
    <div id="viewDiv">
      <div id="topbar" class="esri-widget">
        <button
          class="action-button"
          id="layoutButton"
          type="button"
          title="Change font case for countries"
        >
          Change font case
        </button>
        <button
          class="action-button"
          id="paintButton"
          type="button"
          title="Change water fill color"
        >
          Change water color
        </button>
      </div>
    </div>
  </body>
</html>
```