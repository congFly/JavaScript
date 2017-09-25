# webpack学习之路
webpack 是一个模块打包工具，输入为包含依赖关系的模块集，输出为打包合并的前端静态资源。
webpack的loader:处理各种需要被处理的静态文件
webpack支持CommonJs、AMD规范

__模块系统的几大类型：__

__<script>标签类型：__
```
	缺点：
		全局作用域下造成变量的冲突		文件加载的顺序很重要
		加载文件越多，网页失去响应的时间越长
		模块与模块之间的依赖很重要
		在大型项目中难以维护和管理
		目前渐渐淡出开发者视野
```
__CommonJs（Nodejs）：__
```
	优点：
		所有代码都运行在模块作用域、不会污染全局作用域
		服务端模块能够重复利用
		模块可以多次加载，但是只会在第一次加载时运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果。要想让模块再次运行，必须清除缓存。
		模块加载的顺序，按照其在代码中出现的顺序。
		有优秀的包管理工具
		简单，上手容易
	
	缺点：
		同步加载的
		不适合浏览器端的使用
		不能做到并行加载模块
```
__AMD（requirejs异步加载，异步模块定义）：__
```
	优点：
		适合浏览器的异步加载机制
		并行加载模块

	缺点：
		加载顺序不一定，可能会造成一些困扰
	 	代码难以经营和维护
```
__CMD（seajs，通用模块定义）：__
```
	优点：
		只有在使用的时候才会解析js文件
		js文件的执行顺序是有体现的，是可控的

	缺点：
		同步执行，执行等待时间会叠加
```
__ES6（import）：__
```
	优点：
		未来的ES规范

	缺点：
		浏览器对ES6的支持还不完全支持
		能够依赖现有的模块少
```

__webpack的目标是什么？__
```
	1.将依赖的模块分片化，并且按需加载
	2.解决大型项目初始化加载慢的问题
	3.每一个静态文件都可以看成一个模块
	4.可以整合第三方库
	5.能够在大型项目中使用
	6.可以自定义切割模块的方式
```

__webpack优点：__
```
	1.有两种不同的加载方式
	2.loader，加载器可以将其他资源整合到js文件中，通过这种方式，可以吧所有文件打包到一个文件中
	3.优秀的语法分析的能力，支持CommonJs、AMD规范
	4.有丰富的开源插件库，可以根据自己的需求自定义webpack的配置
```

__webpack安装：__
```
	npm install webpack -g/--save-dev
```
__webpack实时编译：__
```
	webpack --watch
```
默认配置文件webpack.config.js改成自定义文件

	webpack --config customconfig.js

__webpack用法：__
一个完整的文件：webpack.config.js
```
var webpack = require('webpack');
var ExtractTextPlugin = require('extract-text-webpack-plugin');
var CommonsChunkPlugin = webpack.optimize.CommonsChunkPlugin;
module.exports = {
    devtool: 'eval-source-map',
    module: {
        loaders: [
            {test: /\.css$/, loader: 'style-loader!css-loader'},
            {test: /\.(jpg|png|gif|svg)$/, loader: "url-loader?limit=8192&name=images/[hash:8].[name].[]"}
        ]
    },
    entry: {
        jquery: ['jquery'],
        lodash: ['lodash'],
        jstree: ['jstree'],
        index: ["./source/page/index"],
        designer: './source/cmd/designer'
    },
    output: {
        filename: "./deploy/[name].js"
    },
    plugins: [
        new CommonsChunkPlugin({
            name: "./commons",
            chunks: ["index", 'designer']
        }),
        new ExtractTextPlugin("styles.min.css"),
        new webpack.ProvidePlugin({
            "$": "jquery",
            "jQuery": "jquery",
            "window.jQuery": "jquery"
        })
    ],
    devServer: {
        historyApiFallback: true,
        hot: true,
        inline: true,
        progress: true,
        port: 8000
    }
};
```

```
entry:入口文件的配置项，它是一个数组，webpack允许有多个入口点。
output：输出文件的文件名
	1.path——输出文件的路径
	2.filename——输出文件的文件名
plugins：给webpack可以添加更多的插件，可以丰富webpack的功能。他有两种插件：
	1.webpack 内置插件（需要安装webpack模块）
	2.webpack外置插件（需要npm install component-webpack-plugin）
modules：配置文件的处理选项
	1.loaders：处理不同文件的加载器
	    test：用来匹配相对应文件的正则表达式
		loaders：告诉webpack要利用那种加载器来处理test所匹配的文件
```
如下：
```
DemoOne
|- dist
|- src
	|- index.js
	|- index.html
	|- style.css
	|- demo.png(image)
|- package.json
|- webpack.config.js
```
index.html:

```
 	<!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>demo1</title>
	</head>
	<body>
		<div>Hello,world</div>
		<img src="./demo.png" alt="">
		<script src="../dist/bundlle.js"></script>
	</body>
	</html>
```

style.css:
```
body{
	background:#ddd;
}
```

配置webpack.config.js:
```
var path = require('path')   //path是node.js内置的package，用来处理路径的。用于连接路径。该方法的主要用途在于，会正确使用当前系统的路径分隔符，Unix系统是/，Windows系统是\
var webpack = require('webpack');
module.exports = {
  entry: ['./src/index'],
  output: {
    path: path.join(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin({
      compressor: {
        warnings: false,
      },
    })
  ],
  module: {
    loaders: [{
      test: /\.css$/,
      loaders: ['style', 'css']
    },
    {
        test: /\.(png|jpg)$/,
        loaders: [
            'file?hash=sha512&digest=hex&name=[hash].[ext]',
            'image-webpack?bypassOnDebug&optimizationLevel=7&interlaced=false'
        ]
    }]
  }
}
```

在入口文件引入内容
```
require（'./style.css'）
require（'./demo.png'）
```
再次运行，dist文件内会有两个文件，是webpack打包后的文件。

__webpack不仅有单一的入口文件也有多个入口文件，以及多个打包目标。__
```
    entry: {
        index: ["./source/page/index"],
        designer: './source/cmd/designer'
    },
    output: {
        filename: "./deploy/[name].js"
    },
```

最终打包出:

```
├── index.js
└── designer.js
```

>[name] entry 对应的名称
[hash] webpack 命令执行结果显示的 Hash 值
[chunkhash] chunk 的 hash


webpack如何解析JSX和ES6语法？
那就用Babel吧！
在module里面添加：
```
module: {
	loaders: [{
		test: /.js$/,
		exclude: /node_modules/,
		loader: 'babel',
		query: {
		presets: ['es2015', 'stage-0', 'react']
		}
	}]
	}
}
```
强调：.css 文件应用  "style" 和 "css" loader  
```
{
	test: /.css$/,
	loader: "style-loader!css-loader"
}
```
webpack其实还有好多插件可以供我们使用。详细的可以去[官网](https://webpack.github.io/docs/)