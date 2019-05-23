# webpack-note

## webpack-learning-notes

### install

```js
//mkdir webpack-demo
//生成package.json文件
npm init
//npm init -y(自动生成配置项，不会让你自己一个一个选择了)
```

```js
//全局安装webpack(不推荐)，假如有两个项目都用webpack打包的话，可能会出现版本不一样的问题
npm install webpack webpack-cli -g
//查看是否安装上(命令行)
webpack -v
//卸载webpack
npm uninstall webpack webpack-cli -g

//在项目内安装webpack(好处：可以在不同的项目中使用不同的webpack版本)
cd webpack-demo
npm install webpack webpack-cli -D(和--save-dev等价)
//node提供了一个命令npx,npx会在当前项目目录的node-modules里找webpack
npx webpack -v

//查看安装包的版本号是否存在：npm info webpack
//安装指定版本号的包
npm install webpack@4.16.5 webpack-cli -D

//webpack默认配置文件：webpack.config.js
//webpack来打包，以webpackconfig.js为配置文件(修改默认配置文件名字)
npx webpack --config webpackconfig.js

//打包命令
npx webpack
//在package.json文件中配置好之后可以使用npm run bundle

//webpack-cli的作用：可以让我们在命令行里使用webpack命令 
```

### webpack的配置文件

与package.json同级目录下新建webpack.config.js文件：

```js
//webpack.config.js
const path = require('path')
module.export = {
  mode: 'production', //默认模式production，打包后的文件会被压缩，如果改成development，不会被压缩
  //从哪一个文件开始打包
  entry: './src/index.js'
  //打包文件放到哪里去
  output: {
        //打包好的文件命名
        filename: 'bundle.js',
        //打包好的文件放到哪一个文件夹下,path后面要写绝对路径,要引入node的核心模块path，调用模块的resolve方法，__dirname变量指的是：webpack.config.js所在的当前目录的路径，把它和dist做一个结合
        //从index.js开始打包，打包生成的文件放到bundle文件夹下
        path: path.resolve(__dirname,'dist')
    }
}
```

### webpack打包输出内容

> Hash：本次打包对应的唯一hash值
>
> Version：本次打包使用的是webpack的第几个版本
>
> Time：整体的打包耗时
>
> Built at：



> Asset(打包出来的文件)	Size(文件大小)	Chunks(打包的js文件对应的id)	Chunk Names(文件对应的名字)
>
> bundle.js						   1.36Kib				0													  main

> Chunk Name：main
>
> main的来源：
>
> ```js
> module.exports = {
>   entry: {
>     main: './src/index.js' //上面的是简写
>   }
> }
> ```

> entrypoint main=bundle.js 
>
> (整个打包过程中的入口文件)

```js
module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js' 
  }
}
```


