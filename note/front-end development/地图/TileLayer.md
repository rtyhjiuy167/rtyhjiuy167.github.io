`TileLayer`允许使用由`ArcGIS Server REST API`公开的缓存地图服务，并将其作为切片层添加到地图中。

缓存，比`Mapimagelayer`渲染得更快。要创建`TileLayer`的实例，必须引用缓存地图服务的URL。

如果请求跨域了，需要启用CORS的服务器或代理。