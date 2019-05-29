## entry与output的基本配置

> entry：打包的入口文件
>
> output：打包生成的文件，默认是main.js，可以在output中进行配置

```js
module.exports = {
  entry: {
    //将index.js文件打包两次
    main: './src/index.js',
    sub: './src/index.js'
  },
  output: {
    //配置了publicPath之后，在打包过的js代码的路径前面都会加上publicPath，适合后台用index.html，静态资源放cdn的情况
    publicPath: 'http://cdn.com.cn'
    path: path.resolve(__dirname, './dist'),
    //因为要打包两个文件，所以文件名不能写成bundle.js之类，得用一个占位符，比如name/hash，name就对应entry对象的key值
    filename: '[name].js'
  },
}
```

