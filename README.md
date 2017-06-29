# webpack-demo
##一、Node 安装与使用
<p>官网下载node lts版本（稳定版）</p>

##二、webpack 安装与使用
找个目录建立文件夹

$ cd webpack-demo

$ mkdir 003

进入文件夹

$ cd 003

查看文件夹

$ ls

然后进行npm 初始换

$ npm init -y

-y是意思生成package.json 文件 这是npm自带。

进行webpack 安装

本地安装命令

$ npm install --save-dev webpack

指定版本安装

$ npm install --save-dev webpack@<version>

全局安装

$ npm install --global webpack

我们建议大多数项目是本地安装。这使得在引入更改时更容易单独升级项目。

要运行本地安装的Webpack，您可以访问其bin版本作为 
命令:
$ node_modules/.bin /webpack

此时报：No configuration file found and no output filename configured via CLI option.
A configuration file could be named 'webpack.config.js' in the current directory.
Use --help to display the CLI options.

意思是报没有发现webpack.config.js文件

但是我们先不配置，先来尝鲜

首先在003里面建立文件夹

$ app mkdir app

然后在里面放一个叫index.js

index.js里面很简单写一个语句。

```console.log('hello world!');```
  
然后保存。之后执行命令

$ node_modules/.bin/webpack app/index.js build/build.js

这个是使用webpack的打包方式 将app里面的index.js 打包输出到 build文件里build.js

##三、webpack 初体验

重新创建一个项目目录。eg:004

讲003里面的项目全部copy到004

再将app目录里面的index.js中 console 语句删除

并在app目录里建立component.js

在component.js中写入

```export default (text="hello world") => {
	const element = document.createElement('div');
	element.innerHTML = text;

	return element;
}
```

在index.js中写入

```import component from './component.js';
document.body.appendChild(component());
```

再安装一个自动生成html的组件

$ npm install --save-dev html-webpack-plugin

组件安装完成之后，需要建一个webpack.config.js来进行webpack的配置。

之前我们是通过命令行的形式给webpack传一些参数，但是你想用webpack完整的配置。还是必须要有这么一个文件叫webpack.config.js 
配置如下：

```
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

const PATHS = {
	app: path.join(__dirname, 'app'),
	build: path.join(__dirname, 'build')
};

module.exports = {
	entry: {
		app: PATHS.app
	},
	output: {
		path: PATHS.build,
		filename: '[name].js'
	},
	plugins: [
		new HtmlWebpackPlugin({
			title: 'Webpack demo'
		})
	]
}
```

好现在使用webpack 命令进行打包，还是使用上面的webpack/bin里的
命令： 

$ node_modules/.bin/webpack

执行成功之后，会在build文件夹中index.html


build中的build.js 是从003里面复制过来的，删掉不影响

##四、webpack 编译输出的日志
```
eg:
Hash: dba4fb3819dee0d8ec5a
Version: webpack 3.0.0
Time: 685ms
     Asset       Size  Chunks             Chunk Names
    app.js    3.07 kB       0  [emitted]  app
index.html  180 bytes          [emitted]
   [0] ./app/index.js 82 bytes {0} [built]
   [1] ./app/component.js 140 bytes {0} [built]
Child html-webpack-plugin for "index.html":
       [2] (webpack)/buildin/global.js 509 bytes {0} [built]
       [3] (webpack)/buildin/module.js 517 bytes {0} [built]
        + 2 hidden modules
```
		
Hash: 表示这次编辑唯一使用的值。

Version: 当前使用webpack的版本号

Time: 编译耗时

编译成app.js 及 index.html。用到index.js 和component.js 、html-webpack-plugin插件等资源进行的编译的结果

##五、使用快捷方式进行编译

如果全局安装的webpack可以直接使用命令：

$ webpack

进行编译

但是全局安装，项目的差异化比较大，还是本地安装比较好，我们使用的是本地安装。那么本地安装的进行编译时。每次都得输入很长的命令：

$ node_modules/.bin/webapck

还是比较麻烦的

可以在package.json 里面使用scripts 的配置。

打开package.json,在里面的scripts 配置中增加：

```
'build': 'webpack'
```

这样的语句。体现出来是这样：
```
"scripts":{
	"build": "webpack"
}
```
build 表示命令

刚才不是讲了，只有全局安装的webpack的时候，才能使用命令webpack。但是这里直接使用webpack的原因是，在package.json中会临时将node_modules里的命令加载进来。当然也可以这样写：
```
"scripts":{
	"build": "node_modules/.bin/webpack"
}
```

如果简单一点，就直接调用webpack命令了

下来就可以使用 npm run build 命令进行编译了。run后面跟的是命令。我们上面定义的命令是build

如果你想看 npm run 支持哪些命令。可以使用命令：

$ npm run

控制台日志：
```
Lifecycle scripts included in 003:
  test
    echo "Error: no test specified" && exit 1

available via `npm run-script`:
  build
    webpack
```
表示命令包括，test和 build 。build是我们刚才配置的

这节主要讲了，使用快捷命令 npm run build 进行编译，及快捷命令的配置

##六、浏览器自动刷新

浏览器自动刷新首先需要安装一个组件命令为：

$ npm install --save-dev webpack-dev-server