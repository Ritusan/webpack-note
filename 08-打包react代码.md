## 配置React代码的打包

```js
//index.js
import '@babel/pollfill'
//安装react：npm install react react-dom -D

import React,{ Component } from 'react'
import ReactDom from 'react-dom'

class App extends Component {
  render(){
    return <div>Hello World</div>
  }
}

ReactDom.render(<App />, document.getElementById('root'))
```

https://babeljs.io/docs/en/babel-preset-react

```js
//.babelrc代码：
//babel做打包的时候转换一下react语法，执行是有顺序的，从下到上执行，先执行preset-react，再执行preset-env
{
  "plugins": [
    [
      "@babel/preset-env",{
    	 targets: {
       		chrome: '67'
       },
       useBuiltIns: 'usage'
     }
    ],
    "@babel/preset-react"
  ]
}
```

