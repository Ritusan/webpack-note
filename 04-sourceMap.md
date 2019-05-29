## sourceMap

> sourceMap：在开发调试时，如果代码有错误，希望提示的是(源代码)具体的某个文件下的某一行哪里出错了，而不是提示的错误是在打包之后的main.js文件里的某一行

> 现在已知dist目录下main.js第100行出错了，
>
> sourceMap是一个==映射==关系，它知道dist目录下main.js第100行实际上对应的是src目录下index.js文件中的第1行，
>
> 所以当前其实是src目录下index.js文件的第1行出错了

```js
module.exports = {
  mode: 'development', //在开发者模式下sourceMap默认是开启的
  //devtool: 'none', //关闭sourceMap
  //devtool: 'source-map', //开启sourceMap,打包过程会变慢，因为打包过程中需要构建映射关系，在dist目录下会多出main.js.map文件
  //devtool: 'inline-source-map', //通过dataUrl的方式直接写入dist目录下的main.js文件中，不会再生成.map文件，map文件会变成base64de字符串放到main.js的底部
  //devtool: 'cheap-inline-source-map', //当遇到代码量很大时，如果没加cheap，代码出错时会提示你代码在哪个文件的第几行的某个字符出了问题，会精确到哪一行哪一列，但是这样的映射比较耗费性能，实际上只需要告诉代码的哪一行出错就行，加上cheap的作用就是告诉哪一行出错了不用具体到列，使用cheap之后打包的性能会得到一定的提升，打包速度会更快，cheap只针对业务代码(不会管引入的第三方模块的问题)，如果也想知道第三方模块的问题，把配置改成：cheap-module-inline-source-map
  //devtool: 'eval', //打包速度最快的一种方式，打包生成的main.js文件里没有base64的东西，也没有.map文件
  devtool: 'cheap-module-eval-source-map', //最佳实践:提示的错误比较全，同时打包速度又比较快
  entry: {
    main: './src/index.js'
  },
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: '[name].js'
  },
}
```

```js
module.exports = {
  mode: 'production', //线上代码
  devtool: 'cheap-module-source-map', //线上代码一般不用devtool，如果要定位错误，用这个配置
    main: './src/index.js'
  },
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: '[name].js'
  },
}
```

