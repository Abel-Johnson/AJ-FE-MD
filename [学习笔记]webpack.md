# webpack

前端要构建自己的ui组件库, 所以回顾了一下webpack相关的东西, 有助于下周将要进行的ui组件项目的搭建

传统项目构建的问题:

1. 不能立即,明确的体现js依赖之间的依赖关系(依赖显示)
2. 一个依赖包出现问题, 应用就会崩溃(独立)
3. 一个依赖包引入后没有被使用, 浏览器依然会去下载不必要的代码(按需加载)


> WebPack可以看做是模块打包机：它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其打包为合适的格式以供浏览器使用。


> Gulp/Grunt: 规定一个步骤路径, 指明一步一步做什么(编译, 组合, 压缩)
> webpack: 给一个入口, 找依赖, 使用loaders处理, 最后打包为一个js


## 正式使用Webpack打包,几种方法:


### 方法一: 
webpack可以在终端中使用，其最基础的命令是

`webpack {entry file/入口文件} {destination for bundled file/存放bundle.js的地方}`
只需要指定一个入口文件，webpack将自动识别项目所依赖的其它文件，不过需要注意的是如果你的webpack没有进行全局安装，那么当你在终端中使用此命令时，需要额外指定其在node_modules中的地址，继续上面的例子，在终端中属于如下命令

//webpack非全局安装的情况
`node_modules/.bin/webpack app/main.js public/bundle.js`

### 方法二:
`webpack.config.js`

```javascript
module.exports = {
  entry: __dirname + '/src/main.js',
  output: {
    path: __dirname + '/public',
    filename: 'bundle.js'
  }
}
```

### 方法三:
配置脚本, 直接类似npm start
在 package.json里, 配置script参数, 编写自己的脚本, 特殊的, 
start=>`npm start` // otherName => `npm run otherName`

## Source Map

作用: 错误发生的源代码定位
方法: 配置webpack.config.js里的devtool

devtool选项|配置结果
---|---
source-map|最好/慢/单独完美的文件；
cheap-module-source-map	|不带列映射(一定程度影响调试)/较快
eval-source-map|使用eval, 同一个文件生成sourcemap/不影响构建速度/对打包后的js有安全和性能的隐患 ,    开发阶段**推荐**/生产环境**一定**不要用
cheap-module-eval-source-map|最快/同行显示/没有列映射


## 本地服务器,实现热更新
1. 安装webpack-dev-server
2. webpack配置里: 

	```javascript
	devServer: {
	    contentBase: "./public",//本地服务器所加载的页面所在的目录
	    colors: true,//终端中输出结果为彩色
	    historyApiFallback: true,//不跳转
	    inline: true//实时刷新
	} 
	```

## Loaders
就是外部引入的一些工具模块, 像babel之类的

1. 需要单独安装
2. 需要在webpack.config.js下的modules关键字下的loaders数组进行配置:
	* test：一个匹配loaders所处理的文件的拓展名的正则表达式（必须）
	* loader：loader的名称（必须）(webpack2以后不能省略后缀-loader)
	* include/exclude:手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）；
	* query：为loaders提供额外的设置选项（可选）
	


### 典型的Babel

> webpack只支持import和export两种es6语法, 其他的就要用到babel转义了

```
// npm一次性安装多个依赖模块，模块之间用空格隔开
npm install --save-dev babel-core babel-loader babel-preset-es2015 babel-preset-react
```
webpack配置:

```javascript
{
  test: /\.js$/,
  exclude: '/node_modules/',
  loader: 'babel-loader',
  query: {
    presets: ['es2015', 'react']
  }
}
```

### css

`npm install --save-dev style-loader css-loader`
css-loader: 通过@import/url() 引入别的css文件
style-loader: 合并所有样式到js中

配置: 

```javascript
{
	test: /\.css$/,
	loader: 'style-loader!css-loader',//添加对样式表的处理, !同时使用多个loader处理同一种文件,注意严格的先后顺序,先style,后css
}

```
> 小插曲, npm 突然不能用了,
> sudo: npm: command not found

> $ brew update
> $ brew uninstall node
> $ brew install node
> $ brew postinstall 
重装node, 顺利解决

### css模块化 避免命名冲突

> 如果用的脚手架-比如create-react-app, reject以后, 在config里加一条就可以了
> 搜索css-loader，找到这段代码 
> ![代码图片](https://img-blog.csdn.net/2018051417095465?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dvcGVsbw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
> 
> 如果没有配置css modules的话，options中是没有modules选项的，将其设置上即可


---

[webpack 3.X学习之CSS处理](https://www.cnblogs.com/hawk-zz/p/7884354.html)

#### 使用babel-plugin-react-css-modules
`babel-plugin-react-css-modules` 可以实现使用styleName属性自动加载CSS模块。只需要把className换成styleName即可获得CSS局部作用域的能力，babel插件来自动进行语法树解析并最终生成className。改动成本极小，不会增加JSX的复杂度，也不会给项目带来额外的负担。

```
import React from 'react';
import styles from './table.css';
 
class Table extends React.Component {
    render () {
        return <div styleName='table'>
        </div>;
    }
}
 
export default Table；
```


```javascript
{
	test: /\.css$/,
	loader: 'style-loader!css-loader?modules',//后边加一个?modules, 会把class编码为一长串字符,带来的唯一性避免了引入不同css的变量命名冲突
}

```

### 预处理器 lessSass autoprefixer...
`npm install --save-dev postcss-loader autoprefixer`

2.0以后的版本,
*只要是在css中使用了@import，无论是在哪里配置postcss-loader都报错*

必须在根目录创建一个postcss专门的配置文件
在项目根目录下创建一个`postcss.config.js`文件，配置如下

```javascript
module.exports = {
    plugins: [
        require('autoprefixer')({
            browsers: ['last 5 versions']
        })
    ]
}

```

### 插件
对整个过程起作用,而不是对单个文件
使用: 
1. 安装
2. 配置plugins数组,添加一个该插件的实例
>	如: plugins: [
    new webpack.BannerPlugin("Copyright Flying Unicorns inc.")//在这个数组中new一个就可以了,这个插件会在js文件开头添加一行版权声明
  ], 


### 多个webpack配置文件的关联
在`package.json`里

```json
{
  "scripts": {
    "start": "webpack-dev-server --progress",
    "build": "NODE_ENV=production webpack --config ./webpack.production.config.js --progress"
  },
}
```
### 多入口同时打包

```javascript
module.exports = {
	entry: {
	 index: './src/index.js',
	 app: './src/index.js',
	 print: './src/print.js'
	},
	output: {
	 filename: 'bundle.js',
	 filename: '[name].bundle.js',
	  path: path.resolve(__dirname, 'dist')
	}
};
```

### HMR(hot module replacement)

```javascript
 devServer: {
      contentBase: './dist',
+     hot: true
    },
    plugins: [
      new HtmlWebpackPlugin({
        title: 'Hot Module Replacement'
      }),
+     new webpack.HotModuleReplacementPlugin()
    ],
```

```javascript
if (module.hot) {
    module.hot.accept('./print.js', function() {
      console.log('Accepting the updated printMe module!');
-     printMe();
+     document.body.removeChild(element);
+     element = component(); // Re-render the "component" to update the click handler
+     document.body.appendChild(element);
    })
  }
```

## 环境变量

- 用来区分生产环境和开发环境

使用node的process.env挂载表示环境的变量, NODE_ENV已经成为一个事实标准
也即 **`process.env.NODE_ENV`**



----

“hot” Vs “inline” webpack-dev-server options
“inline”选项为整个页面提供了“Live reloading”功能。“hot”选项提供了“模块热重载”功能，它会尝试仅仅更新组件被改变的部分（而不是整个页面）。如果我们把这两个选项都写上，那么当文件被改动时，webpack-dev-server会先尝试HMR，如果这不管用，它就会重新加载整个页面。

//当文件被改动后，下面的三个选项都会生成新的bundle，但是，
 
//1. 页面不会刷新
$ webpack-dev-server

//2. 刷新整个页面
$ webpack-dev-server --inline

//3. 仅仅刷新被改动的部分（HMR），如果HMR失败则刷新整个页面
$ webpack-dev-server  --inline --hot




# 缓存