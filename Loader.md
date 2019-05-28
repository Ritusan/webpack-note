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

## 使用Loader打包静态资源(图片)

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

## 使用Loader打包静态资源(样式)

> webpack默认只支持打包js文件，当直接打包css文件时就会报错，需要在webpack.config.js中做一个配置

```js
module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js' 
  },
  module: {
    rules: [
      {
        test: /\.scss$/,
        //css-loader作用：帮我们分析出几个css文件之间的关系，最终把多个css文件合并成一个css
        //style-loader作用：在得到css-loader生成的css内容之后，style-loader会把这段内容挂载到页面的<head>部分，<head>部分就会多一个<style>标签
        //在配置中loader是有先后顺序的，执行顺序是从下到上，从右到左
        //当打包一个sass文件时，首先会执行sass-loader，对sass代码进行翻译，翻译成css代码之后给到css-loader，都处理好之后再给style-loader挂载到页面上
        //自动添加厂商前缀(-webkit-transform之类)：postcss-loader，在根目录下创建postcss.config.js文件进行配置，具体参照官方文档(或者下方代码)
        use: [
          'style-loader',
          'css-loader',
          'sass-loader',
          'postcss-loader'
        ],
        
      }
    ]
  }
}
```

```js
//postcss.config.js文件配置
//在命令行里安装：npm install autoprefixer -D
module.exports = {
  plugins: [
    require('autoprefixer')
  ]
}
```

```js
//css-loader中常用的配置项
module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js' 
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
              //在@import语法中再引入其它sass文件时，如果不进行配置，在scss文件中用@import再引入的scss文件就会直接走css-loader，不会从postcss-loader开始执行，如果想scss文件中嵌套的scss文件也从postcss-loader开始执行的话，就用下面的配置，作用是通过@import引入的scss文件，在引入之前也要去走两个loader，也就是postcss-loader和sass-loader
              importLoaders: 2
            }
          },
          'sass-loader',
          'postcss-loader'
        ],
        
      }
    ]
  }
}
```

### css打包模块化

```js
//在index.js文件中直接引入一个scss文件的话，会作用于当前js文件，也会作用于当前js文件引入的js文件，相当于样式是全局的，很容易出现样式冲突的问题，这时需要用css模块化，模块化指的是这个css只在这个模块里有效
module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js' 
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
              //开启css的模块化打包
              modules: true
            }
          },
          'sass-loader',
          'postcss-loader'
        ]
      }
    ]
  }
}
```

```js
//index.js文件代码
import avatar from './avatar.jpg'
import style from './index.scss'
import createAvatar from './createAvatar'
createAvatar()

var img = new Image()
img.src = avatar
//img.classList.add('avatar')
img.classList.add(style.avatar)

var root = document.getElementById('root')
root.append(img)
```

### 使用webpack打包字体文件

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
      },
      {
        //打包字体font文件
        test: /\.(eot|ttf|svg)$/,
        use: {
          loader: 'file-loader'
        }
      }
    ]
  }
}
```

