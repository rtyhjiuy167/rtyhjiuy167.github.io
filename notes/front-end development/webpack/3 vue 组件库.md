---
title: 3 vue 组件库
top: 3
tags:
  - webpack
categories:
  - webpack
---

### webpack 配置

```js
// webpack.config.js
const path = require('path')
const glob = require('glob')
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
const { VueLoaderPlugin } = require('vue-loader')
const isProduction = process.env.NODE_ENV == 'production'

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
})('packages/components', list)
function getStyleLoader(pre) {
  return [
    MiniCssExtractPlugin.loader,
    'css-loader',
    {
      loader: 'postcss-loader',
      options: {
        postcssOptions: {
          plugins: ['postcss-preset-env']
        }
      }
    },
    pre
  ].filter(Boolean)
}

const config = {
  entry: list,
  devtool: 'cheap-module-source-map',
  output: {
    filename: '[name].js',
    path: path.resolve(__dirname, 'lib/components'),
    library: '[name]',
    globalObject: 'this',
    libraryTarget: 'umd',
    assetModuleFilename: 'media/[hash:10][ext][query]',
    clean: true
  },
  // devServer: {
  //   open: true,
  //   host: 'localhost',
  //   port: '3000',
  //   static: path.resolve(__dirname, 'play')
  // },
  externals: {
    vue: 'Vue'
  },
  plugins: [new VueLoaderPlugin(), new MiniCssExtractPlugin()],
  module: {
    rules: [
      {
        test: /\.vue$/,
        use: [
          {
            loader: 'vue-loader',
            options: {
              esModule: true
            }
          }
        ]
      },
      { test: /\.(ts|js)x?$/, exclude: /node_modules/, loader: 'babel-loader' },

      {
        test: /\.css$/i,
        use: getStyleLoader()
      },
      {
        test: /\.s[ac]ss$/i,
        use: getStyleLoader('sass-loader')
      },
      {
        test: /\.(eot|svg|ttf|woff|woff2|png|jpg|gif)$/i,
        type: 'asset'
      }
    ]
  },
  resolve: {
    alias: {
      '@packages': path.resolve(__dirname, 'packages')
    },
    extensions: ['.tsx', '.ts', '.js', '.vue', '.json']
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

### babel 配置

```js
// babel.config.js
module.exports = {
  presets: ['@babel/preset-env', '@babel/preset-typescript'],
  overrides: [
    {
      // .vue文件中使用了ts，对ts代码进行转化
      test: /\.vue$/,
      plugins: ['@babel/transform-typescript']
    }
  ]
}
```

