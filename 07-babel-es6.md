## 使用Babel处理ES6语法

https://babeljs.io/setup#installation

```js
module: {
  rules: [
    { 
      test: /\.js$/, 
     	exclude: /node_modules/, //排除node-modules目录下的文件
     	loader: "babel-loader", //webpack和babel做通信的桥梁，并不会把js中的ES6语法翻译成ES5语法
      options: {
        presets: [["@babel/preset-env",{
          targets: {
            /*edge: "17",
            firefox: "60",
            chrome: "67",
            safari: "11.1",*/
            chrome: '67', //项目打包会运行在大于67这个版本的chrome浏览器下，babel在打包的过程中使用preset-env结合polyfill要去把ES6的语法变成ES5的语法，这个过程中你来看是否有必要做ES6到ES5的转换，如果chrome大于67这个版本，浏览器已经对ES6代码支持的很好了，就没有必要做ES6转ES5的操作
          },
          //配置参数
          useBuiltIns: "usage" //当页面上加了一些低版本浏览器可能不存在的特性时，不是把所有的特性都加进来，而是根据你的业务代码来决定加什么，(按需引入)
        }]], //包含了所有ES6转换成ES5的翻译规则，可以在打包的过程中把ES6代码翻译成ES5的语法
      }
    }
  ]
}
```

https://babeljs.io/docs/en/babel-polyfill

```js
//变量或函数在低版本浏览器的补充
//安装babel-polyfill
//在src的index.js文件中引入：
import "@babel/polyfill";
```

https://babeljs.io/docs/en/babel-plugin-transform-runtime

```js
//babel-polyfill会污染全局环境，在写业务代码的时候可以使用，如果开发类库的话，用下面这种配置，下面的配置不存在全局污染
module: {
  rules: [
    { 
      test: /\.js$/, 
     	exclude: /node_modules/, 
     	loader: "babel-loader", 
      options: {
        "plugins": [["@babel/plugin-transform-runtime",{
          "absoluteRuntime": false,
          "corejs": 2,
          "helpers": true,
          "regenerator": true,
          "useESModules": false
        }]]
      }
    }
  ]
}
```

```js
//babel里面的options配置可能会非常长，这时可以在根目录下新建.babelrc文件
//.babelrc文件代码：
{
  "plugins": [["@babel/plugin-transform-runtime",{
    "absoluteRuntime": false,
    "corejs": 2,
    "helpers": true,
    "regenerator": true,
    "useESModules": false
  }]]
}
```

