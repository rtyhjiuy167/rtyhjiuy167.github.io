---
title: 2 ts 工具库
top: 2
tags:
  - webpack
categories:
  - webpack
---

## webpack 配置

```js
// webpack.config.js
const path = require('path')
const glob = require('glob')
const MiniCssExtractPlugin = require('mini-css-extract-plugin')

const isProduction = process.env.NODE_ENV == 'production'

const stylesHandler = MiniCssExtractPlugin.loader
const list = {}
(async function makeList(dirPath, list) {
  const filesPath = glob.sync(`${dirPath}/**/index.ts`)
  for (let filePath of filesPath) {
    const fileName = filePath
      .split(/\//)
      .slice(2)
      .join('/')
      .replace(/(?<=index)\.ts/, '')
    list[`${fileName}`] = `./${filePath}`
  }
  return list
})('packages/lib', list)

const config = {
  entry: list,
  output: {
    filename: '[name].js',
    path: path.resolve(__dirname, 'lib'),
    library: '[name]',
    globalObject: 'this',
    libraryTarget: 'umd',
    assetModuleFilename: 'media/[hash:10][ext][query]',
    clean: true
  },
  devServer: {
    static: {
      directory: path.join(__dirname, 'examples')
    },
    port: 8080
  },
  plugins: [new MiniCssExtractPlugin()],
  module: {
    rules: [
      {
        test: /\.(ts|tsx)$/i,
        loader: 'ts-loader',
        exclude: ['/node_modules/']
      },
      {
        test: /\.s[ac]ss$/i,
        use: [stylesHandler, 'css-loader', 'postcss-loader', 'sass-loader']
      },
      {
        test: /\.css$/i,
        use: [stylesHandler, 'css-loader', 'postcss-loader']
      },
      {
        test: /\.(eot|svg|ttf|woff|woff2|png|jpg|gif)$/i,
        type: 'asset'
      }
    ]
  },
  resolve: {
    extensions: ['.tsx', '.ts', '.jsx', '.js']
  }
}

module.exports = () => {
  if (isProduction) {
    config.mode = 'production'
  } else {
    config.mode = 'development'
  }
  return config
}
```

