## 使用webpackDevServer提升开发效率

> 在package.json文件中修改：
>
> ```js
> "script": {
>   "watch": "webpack --watch",  //只要文件发生变化，webpack就会重新打包，但是不会帮我们启一个服务器，意味着打包生成的文件没办法去做一些ajax请求方面的调试，每一次打包结束之后都要我们重新刷新浏览器比较麻烦
>   "start": "webpack-dev-server", //不但会监听到文件发生了改变重新打包，还会自动地帮我们重新刷新浏览器，用它可以更方便地提升代码开发效率，借助它可以帮我们开启一个服务器，因为如果想发ajax请求，要求浏览器地址必须在一个服务器上，通过http协议的方式打开，通过file协议打开是不行的
>   "server": "node server.js", //当运行npm run middleware时，想自己写一个服务器
> }
> ```

```js
module.exports = {
  entry: {
    main: './src/index.js'
  },
  output: {
    publicPath: '/', //所有打包文件之间的引用前面都加一个根路径，确保打包生成的文件路径上不会有问题
    path: path.resolve(__dirname, './dist'),
    filename: '[name].js'
  },
  devServer: {
    contentBase: './dist', //服务器要启动在哪一个文件夹下，要借助webpackDevServer帮我们启动一个服务器，服务器的根路径就是在当前路径下的dist文件夹下
    open: true, //在启动webpackDevServer时，也就是当你运行npm run start时，webpack-dev-server会被启动，启动时会自动帮你打开一个新的浏览器窗口，自动访问你的服务器地址
    proxy: {
      '/api': 'http://localhost:3000'
    }, //跨域接口模拟时使用的接口代理
    port: 8090, //配置端口号
  }
}
```

> 安装devServer:
>
> `npm install webpack-dev-server -D`
>
> npm run start
>
> 直接访问8080端口地址

> 在src同级目录下创建server.js文件

```js
//server.js文件
//安装
npm install express webpack-dev-middleware -D

const express = require('express')
const webpack = require('webpack')
const webpackDevMiddleware = require('webpack-dev-middleware')
const config = require('./webpack.config.js') //引入webpack配置文件
//在node中直接使用webpack
const complier = webpack(config) //webpack编译器，node下面的一些api，用webpack结合配置文件可以随时进行代码的编译，让编译器执行一次，就会帮你重新打包一次代码

//用express创建一个服务器的实例
const app = express()
//在应用中使用编译器
//通过use方法可以使用中间件
//只要文件发生改变，complier就会重新运行，重新运行生成的文件对应的打包输出内容publicPath就是webpack.config.js中的publicPath
app.use(webpackDevMiddleware(complier,{
  publicPath: config.output.publicPath
}))
//3000后面接收一个回调函数，当服务器启动成功之后执行
app.listen(3000,()=>{
  console.log('server is running')
})
```



