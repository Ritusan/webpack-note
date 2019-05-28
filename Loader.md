## Loader是什么

> webpack是什么，
>
> 模块是什么，
>
> webpack的配置文件是什么，

> 模块不仅可以是js文件，也可以是图片，css文件
>
> webpack不能识别非js后缀结尾的模块，就需要通过Loader让webpack识别出来。
>
> Loader是一个打包的方案，对于某一个特定的文件，webpack应该如何地进行打包
>
> webpack本身是不知道对于一些文件该如何处理的，但是Loader知道

> ulr-loader：除了能做file-loader的事情以外，还能做一些额外的事情
>
> url-loader和file-loader不一样，它会把你的图片转换成base64位的字符串，然后直接放到bundle.js里面而不是单独生成一个图片文件，不用再额外加载一次图片文件，省了一次http请求，但是如果图片特别的大，打包生成的js文件也会特别大，那么加载js的时间就会很长，那么在一开始的很长一段时间里页面上什么也显示不出来，
>
> 所以url-loader的最佳使用方式是：如果图片非常小，只有1/2kb左右，那么打包生成到js文件里是一个非常好的选择，没必要让一两kb的图片再去发一个http请求，很浪费时间，但如果图片很大时，就像file-loader一样，把图片打包到dist目录下的images文件夹里，可以在配置项中使用limit，如果图片的大小超过2048字节，就像file-loader打包到images目录下，如果小于2048就会打包到bundle.js中。
>
> 2048是2kb，这个参数可以自己设置

> url-loader和file-loader很像，只是多了一个limit的配置项

```js
module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js' 
  },
  //当去打包一个模块时，不知道该如何打包时，就到模块的配置里去找该怎么打包
  module: {
    rules: [
      {
        test: /\.(jpg|png|gif)$/,
        use: {
          //用file-loader来帮我们做jpg文件的打包
          //需要先安装：npm install file-loader -D
          loader: 'file-loader',
          //使用loader时可以配置一些参数，这些参数可以额外放到一个options的配置项里
          options: {
            //希望打包生成的文件名：和之前的文件名一样，加一个hash值，后缀也一样
            //配置语法：placeholder 占位符
            name: '[name]_[hash].[ext]',
            //遇到以jpg|png|gif结尾的文件时，会打包到dist目录下的images文件夹中
            outputPath: 'images/'
          }
        }
      },
      {
        test: /\.vue$/,
        use: {
          //需要先安装：npm install vue-loader -D
          loader: 'vue-loader'
        }
      },
    ]
  }
}
```

## 使用Loader打包静态资源(图片篇)

```js
module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js' 
  },
  module: {
    rules: [
      {
        test: /\.(jpg|png|gif)$/,
        use: {
          loader: 'url-loader',
          options: {
            name: '[name]_[hash].[ext]',
            outputPath: 'images/',
            limit: 2048
          }
        }
      }
    ]
  }
}
```

