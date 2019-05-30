## Hot Module Replacement热模块替换(HMR)

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')
const CleanWebpackPlugin = require('clean-webpack-plugin')
const webpack = require('webpack')

module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js' 
  },
    output: {
      path: path.resolve(__dirname, './dist'),
      filename: '[name].js'
  },
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: [
          'style-loader',
          {
            loader: 'css-loader',
            options: {
              importLoaders: 2,
            }
          },
          'sass-loader',
          'postcss-loader'
        ]
      },
      {
        test: /\.css$/,
        use: [
          'style-loader',
          'css-loader',
          'postcss-loader'
        ]
      }
    ],
    devServer: {
      contentBase: './dist', 
      open: true, 
      proxy: {
        '/api': 'http://localhost:3000'
      },
      port: 8090,
      hot: true, //让webpack-dev-derver开启 hot module replacement的功能
      hotOnly: true, //即便是HMR的功能没有生效，也不让浏览器自动地重新刷新 
    },
    plugins: [new HtmlWebpackPlugin({
      template: 'src/index.html'
    }),
      new CleanWebpackPlugin(['dist']),
      new webpack.HotModuleReplacementPlugin
      ],
  }
}
```

> 当改变样式代码时希望不要重新刷新页面，只是把样式代码替换，页面上之前渲染出的内容不要刷新，这时就可以借助HMR的功能帮我们实行想要的效果
>
> 在控制台里打开Network下面的Doc 
>
> HMR可以方便我们调试css