## 使用plugins(插件)让打包更便捷

> plugins作用：可以在webpack运行到某个时刻的时候帮你做一些事情，很像生命周期函数

### 安装html-webpack-plugin

> html-webpack-plugin作用：自动在打包的dist目录里生成index.html文件，把打包好的bundle.js自动引入index.html中

```js
npm install html-webpack-plugin -D
```

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

module.exports = {
  entry: 'index.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'bundle.js'
  },
  //插件实例化
  plugins: [new HtmlWebpackPlugin({
    //在src目录下创建一个index.html的模板，模板里面加入id='root'的标签
    template: 'src/index.html'
  })]
}
```

当打包的时候，希望把之前生成的dist目录先删除，然后再执行打包，可以用这个插件：clean-webpack-plugin，这个是第三方的插件，webpack官方文档上可能没有。

### 安装clean-webpack-plugin

```js
npm install clean-webpack-plugin -D
```

```js
const CleanWebpackPlugin = require('clean-webpack-plugin')
module.exports = {
  entry: 'index.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'bundle.js'
  },
  plugins: [new HtmlWebpackPlugin({
    template: 'src/index.html'
  }),
    //当在打包之前，会使用CleanWebpackPlugin这个插件，帮助我们删除dist目录下的所有内容(在打包之前运行)，而HtmlWebpackPlugin(在打包之后被运行)
    new CleanWebpackPlugin(['dist'])]
}
```

