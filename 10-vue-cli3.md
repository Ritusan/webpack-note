## Vue Cli 3

```js
//在根目录下创建vue.config.js文件
//vue cli3对webpack做了大量的封装，不能按照webpack原本的配置，需要到vue-cli3官网上找到配置参考，它提供了一套自己的配置参数

//vue.config.js文件代码：
const path = require('path')

module.exports = {
  outputDir: 'bundle', //打包输出的文件夹名字
  css: {
    modules: true
  },
  //configureWebpack这个配置参数允许我们写原生的webpack配置
  configureWebpack: {
    devServer: {
      //可以在浏览器地址栏访问static目录下的文件
      contentBase: [path.resolve(__dirname,'static')]
    }
  },
}
```

> //打包时webpack生成配置文件的入口，把vue的配置参数最终转换成webpack标准的配置文件，要深入理解vue底层的文件和webpack之间的转换可以看下面的文件：
>
> node-modelus目录 > @vue > @cli-service > lib > Service.js