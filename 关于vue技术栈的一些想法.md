# 关于Vue技术栈的一些建议 #

## &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_______2017年我们应该怎样做前端开发 ##

最近，我们Cap4选取了Vue作为我们的前端开发框架。但是，由于我们是初次使用Vue，实践经验还比较少，所以对于Vue相关的工程栈实际上还存在一些盲点，或者一些可优化的点。

vue本身其实比较简单，看一下官方文档，两天时间就能使用vue进行开发。但是，真正比较困难和麻烦是vue全家桶的使用，以及配套的周边工具的使用。

大家都知道，前端更新换代太快，几乎一年一个样。每年都有不同的开发工具，开发技术，开始思想，开发模式。那么我们在2017年应该怎样做前端开发？

去年，流传着一片很火的文章《在2016年，做前端开发是一种怎样的体验？》[原文](https://hackernoon.com/how-it-feels-to-learn-javascript-in-2016-d3a717dd577f)，[译文](https://zhuanlan.zhihu.com/p/22782487)。从这篇文章，我们可以看到，前端开发拥有一个庞大的工程栈。

那么在2017年，我们是怎样做前端开发的呢？我们来听听这些名词：
    
关于JavaScript： ES6、ES7、ES8，异步编程、promise、async await，柯里化，函数式编程，纯函数，高阶函数

关于css： 下一代CSS，即cssnext（css4），css grid布局，postcss，sass-like，BEM与SUIT规范，模块化css，css in line

关于模块化与打包： webpack，gulp，browsersync，各种loader

关于前端优化： 动态路由，按需加载，tree shaking，inline svg，服务端渲染，虚拟dom

关于包管理工具： npm，nvm，yarn

关于模块化管理工具：lerna

关于开发方式： 热重载，热更新，多端调试，热部署

关于react全家桶： react，react-router，redux，ImmutabilityJS

关于Vue全家桶： vue，vuex，vue-router，vue-resource，axios

关于接口请求： fetch，restful

关于架构模式： spa，前后端分离

关于js编译工具： babel，polyfill

代码规范工具： eslint，stylelint，editorconfig

前端单元测试相关： mocha，karma，webdriver（selenium 2），chai，jasmine

工程化相关：git，jenkens，自动构建，自动部署，持续集成

一些新概念： 虚拟dom，自定义元素，jsx

从上面这个庞大的体系来看，前端已经完全不能用html、css、JavaScript三个词语来概括了。

> 在这里我需要啰嗦一下，在现阶段，仍然有很多的人包括一些team leader认为前端就是切切图、写写HTML、写写CSS、用JS做点动效这种工作。实际上，前端早已成为一种工程化概念，做前端就是在做一项实实在在的工程。现在，模板渲染在前端，业务逻辑在前端，权限验证在前端，路由控制在前端，数据状态在前端等等。前端能做的事多了，需要完成的任务多了，所以，在相应的工具、技术和规范上我们也应该行动起来。

### 关于我建议的vue技术栈 ###

首先，我们现在明确不再支持IE8，那么对于Vue的技术栈就有了很大的选择空间。

目前，我们已经选型Vue作为我们的前端*VM框架，那么我们在vue的全家桶、新技术以及周边工具上也应该做出一些思考和选择。

我建议的Vue技术栈包括：

![](https://i.imgur.com/0UCtdgj.png)

以上就是我建议的Vue技术栈，其中包含了许多技术点，本篇文章并不会详细介绍这些技术点，而是站在更高一层（装逼一点的说法是站在架构的层面上）来全局看待这些技术。

### 关于Vue框架 ###

首先，Vue框架是2017年最火爆的前端框架（目前在国内的火爆程度已经超过React）。Vue框架本身非常轻量、简单，其源代码才仅仅1W行左右（汗，我第一次接触vue是8K行左右）。

Vue的优点是显而易见的：

1. 作为*vm层，避免我们直接操作DOM，不再使用jQuery
2. 虚拟DOM（Vue2.0特性），节点复用，提升渲染性能
3. 天然支持组件化开发思想
4. 类似自定义元素，让我们像写HTML一样去组织控件，可以说只要会html、css、js就能用vue进行开发
5. 双向绑定特性，让表单处理更加简单。我们可以更多的去关注如何改变model层，而不去关注view会如何显示
6. 指令系统，极大的简化了我们的开发，如：动态插值、绑定class和style、事件处理器等等
7. 单向数据流让我们的数据状态可控，让我们复杂逻辑更加可控（有些童鞋会奇怪双向绑定和单向数据流怎么共存的，这里需要明白组件props和组件data）
8. ...

关于Vue的讨论到此为止，更多的信息请参考官方文档。

### 如何做组件化开发？ ###

使用vue做组件化开发，应该来说是相当简单，Vue框架鼓励我们使用单文件组件，我们这里不讨论怎么使用Vue来写组件，而是来讨论下怎么做组件化开发。

首先，我们页面上的任何一个可见区域都应该是一个组件。什么意思？请看下面这张图：

![](https://i.imgur.com/6lSlVB0.png)

上面这张图中的组件还并没有标注完全，但是从这张图我们可以看出，页面中的任何一块独立的区域、一个可见元素都应该最小化的抽象为一个组件，这些组件可能会被嵌套组合，从而形成一个完整的页面。

经过这样设计与划分后，几乎所有的页面都可以抽象为一个组件树：

![](https://i.imgur.com/ZY2p7b7.png)

这些组件中，有些可能仅仅只是作为一个UI组件，只是用来渲染，如icon组件。有些组件，会包含一些状态，如有点击状态的button，选中状态的table，切换状态的tab。有些组件会作为一个container，用来包裹其他小组件，从而形成一个更大的组件。

上面这些组件是构成我们整个应用的基础组件，因此，他们也常常会被单独提取出来作为基础组件库。

> 关于基础组件库，这里还有许多需要讨论的内容，比如：哪些属于基础组件库？怎么写这些基础组件？怎么才能做到通用、可复用？怎么便于管理？怎么打包发布？怎么提供组件文档？怎么维护？等等这些内容，后续我会抽时间来总结。

除了这些组件，还有一类就是我们的业务组件。这类组件，通常都是某一个块的container组件来充当。业务组件，通常用来处理某一个块的特定的业务逻辑。比如，上面的table区域，这里就应该是一个完整的业务组件，它是一个container组件，其中包括了table组件。

业务组件的划分和组成一般没有明确的定义，都是依靠经验来做。一般情况是，可独立完成的业务逻辑，就在本组件内完成，需要与其他组件进行交互配合的，则需要抛出自定义事件（或者通过vuex状态）来传递。

因此，我们的组件通常可以进行如下划分：

![](https://i.imgur.com/vtvhrTt.png)

其中，无状态组件应该是纯函数组件。建议采用render函数和jsx语法来写。

而有状态组件，因为这些状态只属于本组件，因此这些状态应该只在本组件来管理。对于状态的改变，可以抛出自定义事件。

container组件，通常都会是业务组件，这里应该是处理特定业务逻辑的地方。

### 怎么管理组件？ ###

我们已经做组件化开发了，那么我们应该怎么管理这些组件呢？这又是一个很大的话题，这里我只是提一下思路，后续再来补充。

首先，Vue的单文件组件，就给我们提供了一个很好的组件管理方式，所有的组件都独立作为一个文件，组件相关的js、css、资源文件都就近维护。这样，便于我们单独管理某一个组件。

但是，仅仅做到这些还不够。比如：其他开发组想使用我们的组件怎么办？抽象独立组件库。

独立组件库是独立于我们业务项目工程的一个项目。这个独立组件库，应该只是UI组件的独立库，而不是包含业务逻辑的组件，当然高度可独立的业务组件也可以抽象为一个基础组件库。

独立组件库应该满足这些特点：

1. 可直接使用。提供的永远是已经编译完成的vue组件，可直接下载安装使用的，不用关系该组件源码的实现方式。
2. 可发布。提供的组件有独立公共的托管仓库，组件有了更新可以发布到托管仓库。
3. 方便下载安装。用户可以通过一些简单的命令就能下载安装组件，而不是从托管仓库直接拷贝文件。
4. 可按需引入。用户可以选择一次性引入所有组件，也可以选择引入其中的某一些组件。
5. 有实例，可预览。有了组件，你总要让别人知道你的组件能不能满足他的要求吧
6. 有文档。你总要让别人知道怎么使用你的组件吧
7. 可维护。发现了BUG，你的组件要能花最少的代码来维护

关于组件管理，这里暂时只讨论这么多，要满足上面这些条件，实际上又会涉及很多话题，比如用lerna、git、sinopia来配合实现，有空我再详细介绍。

### CSS应不应该模块化？ ###

css应不应该模块化？答案，当然应该，而且必须。

我们都有过维护别人的代码的经历。对于有些老的css代码，没有命名空间，复杂的嵌套，排版混乱，定位困难。因此，我们的css代码也应该有效的管理起来。

css的模块化应该是伴随着UI组件化的。Vue的单文件组件，给我们提供了一种很好的css module方案，它允许我们直接在组件中，通过style标签来写与本组件相关联的css代码。比如：

	<template>
	  <div class="customPortal">
		  ...
		  <div class="tabs">
		  	 <div class="header"></div>
		  </div>
	      ...
	  </div>
	</template>
	
	<script>
	  ...
	</script>
	
	<style scoped>
	    .customPortal {
			...
	        .header{
	            padding: 0 15px;
	        }
			...
	    }
	</style>

并且，我们可以使用scoped属性来限定，这段css代码只能在本组件中使用。生成的css代码如下：
	
	.customPortal .header[data-v-6a594c00]{
	    padding: 0 15px;
	}

实际上，这种css模块化方案，仍然存在一些问题：可能存在较长的嵌套层次；生成对于我们开发者来说根本无用的标识符号，而且很丑；这段css代码没有与组件结构对应起来。

比如：我们现在发现这个内边距有兼容问题，要来修改。我们在firebug看到这段css代码，我们只能知道一个.customPortal的div下有一个.header具有15px的左右内边距。我们并不能直观的知道，这段css对应的组件结构，它到底是影响的.customPortal组件的header？还是影响的tabs组件的header？

这里，我建议在css模块化方案和组件化方案上，再遵循SUIT（BEM）规范。关于BEM和SUIT的解释和用法，请参考这篇[文章](http://www.w3cplus.com/PostCSS/using-postcss-with-bem-and-suit-methodologies.html)，这里不详细使用介绍用法。

SUIT规范，主要是通过类名来唯一表示一个html元素，一个类名表明它所属于的组件，以及组件的结构。SUIT规范的类名通过“-” 、“\__”和“--”连接符来组合，组件和元素之间通过__连接，组件或者元素的状态通过--来连接。

比如：.el-tabs__header，可以表示为命名空间为el，有一个tabs组件，tabs有一个header子元素。我们通过这样一个类名就能直接分析出它的组件结构。那如果我们用常规sass，要达到上面这种结构清晰的效果会是怎么样的呢？

	.el{
		.tabs{
			.header{}
		}
	}
	// 编译后：.el .tabs .header{}

上面这段sass代码会生成较长的嵌套结构，而且也不能直接分析出组件的结构关系。但是SUIT规范只需要一个类名就能搞定了。

使用BEM规范编写上面的组件css，然后编译后的css代码会是怎么样的呢？

	.el-customPortal .el-tabs__header{
	    padding: 0 15px;
	}

通过BEM规范来解读这段css：有一个customPortal组件；这个组件下有一个tabs组件；tabs组件有一个header子元素；这个header子元素有15px的左边内边距。

我们手动来写这些SUIT规范类名可能比较麻烦，但是，我们有postcss插件可以来帮助我们完成。

### 使用postcss ###

在2017年，你还不会使用postcss来处理你的css，那你已经out了。

> 我们的前端架构师@杨超桌上放了一本《深入PostCSS Web设计》。可见，我们确实有人在关注postcss，既然我们有人在关注，那么我们应该推广使用。这里也为大家推荐这本书，同时推荐大漠的系列文章[《深入postcss学习》](http://www.w3cplus.com/blog/tags/517.html?page=1)

postcss是继sass、less工具之后的css处理平台。注意，这里说postcss是一个平台，而不是工具。因为，我们可以为postcss配置各种各样的插件，让他来完成不同的css处理任务。

比如：

1. precss插件，提供类似sass的语法，叫做sass-like。
2. postcss-css-reset插件，基于normalize.css来重置浏览器默认样式
3. postcss-bem插件，提供SUIT（SUIT）规范
4. autoprefixer插件，自动完成浏览器前缀补全
5. postcss-cssnext插件，允许我们使用下一代css语法
6. ...

这里，我比较推荐饿了么团队的postcss-salad工具，这个工具已经集成了上面所说的所有插件，能够帮助我们快速的开发一套简洁、优雅的css代码。具体用法参考[这里](https://github.com/ElemeFE/postcss-salad)。

它主要提供了以下特性：

- 不同平台下的标准的 reset 样式的引入，以及单个元素的常用属性重置功能。
- 提供额外的属性的顺时针简写方式，如 position、border-radius 和 font 等等。
- 下一代 CSS 的语法规则支持，如自定义的变量、媒体查询、选择器等等。
- 常用的 CSS 基础图形绘制的快捷声明，如三角、圆、矩形、环等等。
- 在 CSS 下可自定义 path 颜色的 inline-svg 引入功能。
- rem to px 的转换功能
- 常用的 CSS 功能片段的快捷声明，如 clearfix、超长溢出省略号、图片替换文字等。
- BEM 类名的转换
- 网格系统的快速搭建

这些工具你都可以放心的使用，因为他们都只是一些css任务处理工具，并不会打包到你的代码中去，代价只是你需要去学习。

### 使用webpack进行打包 ###

关于webpack的使用和介绍，我这里也不打算多讲。实际上，vue-cli工具，已经提供了比较完善的webpack打包方案，省去了我们自己去配置的麻烦。

> 汗，当时做react开发时，没有这么智能的工具，webpack的打包配置和使用可折腾死我了。webpack的使用，可以参考这个[系列文章](https://segmentfault.com/a/1190000006843916)和[官网](https://doc.webpack-china.org/guides/installation/)

这里，我提供一个在vue-cli构建的webpack工程基础上的第三方依赖包静态化方案。具体就是说，优化打包速度，将第三方依赖包单独打包。这些第三方依赖包如果在每次构建时都进行打包，会非常耗费时间，而且，第三方依赖并不会经常改变，我们希望能够在客户端进行缓存，并不需要每次都加载这些依赖。


在build目录下，新建一个webpack.dll.config.js文件：

	const webpack = require('webpack');
	const { join } = require('path');
	const HtmlWebpackPlugin = require('html-webpack-plugin');
	const CleanWebpackPlugin = require('clean-webpack-plugin');
	
	// 这里是需要打包的第三方依赖
	const vendors = [
	    "vue/dist/vue.esm.js",
	    "vue-resource",
	    "vue-router",
	    "vuex",
	    "babel-polyfill"
	];
	
	module.exports = {
	    output: {
	        path: join(__dirname,'../static/js/'),
	        filename: '[name].[chunkhash].js',
	        library: '[name]_[chunkhash]',
	    },
	    entry: {
	        vendor: vendors,
	    },
	    plugins: [
	        // 动态链接库打包
	        new webpack.DllPlugin({
	            path: 'manifest.json',
	            name: '[name]_[chunkhash]',
	            context: __dirname,
	        }),
	        // 压缩丑化代码
	        new webpack.optimize.UglifyJsPlugin({
	            compress: {
	                warnings: false,
	                drop_debugger: true,
	                drop_console: true
	            }
	        }),
	        // 删除上一次dll打包的结果
	        new CleanWebpackPlugin(
	            ['static/vendor.*.js'],　 //匹配删除的文件
	            {
	                root: __dirname,       　　　　　　　　　　//根目录
	                verbose:  true,        　　　　　　　　　　//开启在控制台输出信息
	                dry:      false        　　　　　　　　　　//启用删除文件
	            }
	        ),
	        // 让index.html自动引用生成的文件（暂时没办法解决删除上一次输出结果的问题，先采用手动引用）
	        // new HtmlWebpackPlugin({
	        //     template: 'index.html',// index.html文件，静态资源引用模板
	        //     filename: '../../index.html', // 输出到根目录下
	        //     inject: true,
	        //     minify: { // dll打包不用去掉html中的格式
	        //         removeComments: false,
	        //         collapseWhitespace: true,
	        //         preserveLineBreaks: true,
	        //         removeAttributeQuotes: false
	        //         // more options:
	        //         // https://github.com/kangax/html-minifier#options-quick-reference
	        //     },
	        //     // necessary to consistently work with multiple chunks via CommonsChunkPlugin
	        //     chunksSortMode: 'dependency'
	        // }),
	    ],
	};

在package.json文件添加如下script命令：

	 "dll": "webpack --config build/webpack.dll.config.js --progress --display-modules --profile"

在webpack.base.config.js文件添加如下plugin：

	 plugins:[
        new webpack.DllReferencePlugin({
            context: __dirname,
            manifest: require('../manifest.json')
        }),
    ]

使用方式，先进行dll第三方依赖库打包。

	yarn run dll

这个命令会在static目录下生成一个vendor.***.js文件，这里面包含了上面配置的所有第三方依赖文件。这个文件会在运行构建命令时，自动复制到dist目录下。

打包一次第三方依赖后，只要你的第三方依赖不改变，那么你就永远也不用再打包这些文件了。以后的打包，就只需要打包你的业务代码就行了。

> 这里我们没有详细介绍dll打包的原理，请比较关心这个问题的童鞋自己去百度，或者来问我。

### 使用vuex做状态管理 ###

实际上，对于我们的cap4这种具有大量表单的应用来说，使用vuex来做状态管理是不是很方便，我现在也没有实践经验，这个需要我在实践中去验证。但是，常规来说，我们应该做状态管理分离。

为什么要使用vuex？请考虑下面这些情形。

传统的开发中，我们一个组件需要数据时，我们会在这个组件中发起一个ajax请求，将拿到的数据结构存为一个临时变量，然后就近进行解析，再渲染到页面。当页面的表单有修改时，我们监听表单的事件，将新值赋值给这个变量结构的某一项。当另外一个表单域有变化时，再次修改这个变量。最后进行提交。

上面这种情形，实际上存在一些问题。接口请求的代码放在了组件中，导致这部分代码没有提取出来，没有复用性；页面组件中存有一个数据结构的临时变量，然后存在大量篇幅的修改这个变量的代码，导致代码臃肿；在修改这个变量的过程，我们没办法监视这个变量的改变过程，我们无法得知他在上一步是什么状态，下一步是什么状态；当然也没办法做诸如时间旅行，undo、redo等功能。

再考虑另外一个问题，当我们有多个组件可能会需要同一份数据怎么办？难道在这几个组件中，都去做一次请求，维持多个数据状态？或者存为全局变量？有经验的程序员都不会这么做。然后，当我们的不同嵌套层次结构下的组件，需要互相通信，传递数据怎么办呢？难道层层传递？

上面这些问题，都会迫切的需要我们把数据状态从页面组件中提取出来，单独进行管理，这个独立的状态叫做store。在页面组件中，我们不关心这些数据要怎么请求，我们只要拿到数据就能渲染。当需要更新数据时，我们只需要向store提交一个改变的命令，他就会自己去改变数据结构，我们不需要关心它到底是怎么改变的。必要的时候，我们可以查看这些改变在各个步骤下的状态值，甚至可以回退到任何一步，方便调试和做时间旅行等功能。当多个组件需要传递数据进行通信时，我们可以把数据保存到store中，然后在另外一个组件获取就行。

Vuex就是一个做数据状态管理的工具，而且它专适用于vue。

> vuex的使用，我们这里不详细介绍，官方文档已经有了解释。但是，我们仍然需要做一些封装，具体怎么抽象和封装请参考我的示例工程。这里主要是一种思维的改变，估计许多童鞋刚开始会比较迷茫。另外，我比较推荐vuex的源码，短小而精干。另外一个优质的状态管理模式是redux，其源码更具有可读价值。关于他们的源码介绍我后续会补充上。

### 关于异步请求 ###

我们目前选择vue-resource作为我们的异步请求工具，但是，怎么更好的做异步请求，仍然是一个需要讨论的话题。

首先，我们需要对vue-resource再进行一次抽象封装，让vue-resource随时可被替换，比如某一天我想用fetch来发起异步请求怎么办？这也是在cap4正式开发之前我就提出来的，事实证明我的建议是对的，因为最后遇到了使用vue-resource post数据不成功的问题。

对vue-resource进行抽取和独立也非常简单。参考代码如下：

	import Vue from 'vue';
	import { api } from './config';
	
	// http method mapping to vue-resource action
	const methodMapAction = {
	    get : 'query', // query or get
	    post : 'save',
	    put : 'update',
	    patch : 'update',
	    delete : 'delete'// delete or remove
	};
	
	const resource = function ({ url, params, actions = {}, body, ...options }) {
	    if (url.match(/^http/) === null) url = `${api}${url}`;
	    // 忽略自定义的action，统一使用标准的action
	    const resource = Vue.resource(url, {}, {}, options);
	    let action = options.method || 'get';
	    action = action.toLowerCase();
	    return resource[methodMapAction[action]](params, body);
	};
	
	const http = function ({ url, params, actions = {}, ...options }) {
	    if (url.match(/^http/) === null) url = `${api}${url}`;
	    return Vue.http({ url, params, ...options });
	};
	
	export default { http, resource };

resource和http方法返回的都是一个promise，因此，我们可以使用ES8的async、await语法来做异步调用。

	/**
	 * @fileOverview vuex根action，将各module中异步请求样板代码统一提取为一个独立的action。
	 * 使用方式，在module中，发起request action：
	 *  dispatch('request', {
	 *      //异步请求各阶段的mutations type数组
	        types : [types.DISABLED_TEMPLATE, types.DISABLED_TEMPLATE_SUCCESS, types.DISABLED_TEMPLATE_FAILURE],
	        //请求配置参数
	        fetch : options,
	        // 请求成功回调
	        resolved : {
	            action : 'getTemplateList',
	            ...actionPayload
	        },
	        rejected : 'getTemplateList',
	        ...otherPayload
	    });
	 *
	 *  其中rejected和resolved为可选回调参数，可接受字符串类型，对象类型，或者函数。
	 *  字符串将作为回调action名字使用，回调发生时，触发该action。
	 *  如果想给action传递参数，则可以使用对象形式，其中‘action’属性为将要触发的action名字，其余值将作为参数。
	 *  也可以自定义触发一个函数。
	 *  request action中的剩余数据，会作为负载传递到types中规定的各mutation中去。
	 */
	import Api from '../common/api';
	import { isObject, isFunction } from '../assets/js/utils/util';
	
	export const request = async ({ commit, state, dispatch }, payload = {}) => {
	    const { types = [], fetch = {}, resolved, rejected, ...rest } = payload;
	    const [BEGIN, SUCCESS, FAILURE] = types;
	    try {
	        commit(BEGIN);
	        const res = await Api.resource(fetch);
	        const { status, data, ok } = res;
	        const isExecuteSuccess = ok && data && parseInt(data.code, 10) === 0 && parseInt(data.data.code, 10) === 1000;
	        if (isExecuteSuccess) {
	            commit(SUCCESS, { ...rest, status, data, ok });
	            if (typeof resolved === 'string') {
	                dispatch(resolved);
	            } else if (isFunction(resolved)) {
	                resolved(data);
	            } else if (isObject(resolved)) {
	                const { action, ...resolvedPayload } = resolved;
	                dispatch(action, resolvedPayload);
	            }
	            return;
	        }
	        commit(FAILURE, { ...rest, status, data, ok });
	        if (typeof rejected === 'string') {
	            dispatch(rejected);
	        } else if (isFunction(rejected)) {
	            rejected(data);
	        } else if (isObject(rejected)) {
	            const { action, ...rejectedPayload } = rejected;
	            dispatch(action, rejectedPayload);
	        }
	    } catch (e) {
	        console.log('action "request" got a error ：', e);
	        const { status, data, ok } = e;
	        commit(FAILURE, { ...rest, status, data, ok, ...e });
	        if (typeof rejected === 'string') {
	            dispatch(rejected);
	        } else if (isFunction(rejected)) {
	            rejected(data);
	        } else if (isObject(rejected)) {
	            const { action, ...rejectedPayload } = rejected;
	            dispatch(action, rejectedPayload);
	        }
	    }
	};

上面这段代码，我是在vuex的action中调用了vue-resource，然后使用了es6、8的语法。具体使用方法，参考我的示例工程。

### 关于代码规范 ###

代码规范可以说是一个项目质量的重要指标，因为它会直接决定重构和维护的难度。

一个具有良好代码规范的项目，其结构清晰，语法严谨，风格统一。实际上，致远现在并没有沉淀下来一套自己的代码规范。

代码规范，不是说公布一个其他大公司总结的规范文档，然后让大家都去遵守就能完事的。你以为，每个程序员都会那么听话？

关于代码规范，我们应该实实在在的通过工具来检测，来约束，让不符合规范的代码不准提交，不进入版本库。

这里我比较推荐，使用eslint来做代码约束，具体的使用方式参考[官网](http://eslint.cn/)。下面贴一些比较常见的规范：

	// allow debugger during development
        'no-debugger': process.env.NODE_ENV === 'production' ? 2 : 0,
        //缩进采用4个空格
        "indent": [2, 4, { "SwitchCase": 1 }],
        //换行符编码采用unix编码。尤其是Git应该统一采用unix编码，防止不必要的提交。
        "linebreak-style": [2, "unix"],
        //使用单引号表示字符串
        "quotes": [2, "single",{ "allowTemplateLiterals": true }],
        //不能遗漏分号，一个完整代码语句应该以分号结尾
        "semi": [2, "always"],
        //在定义对象或数组时，最后一项不能加逗号
        "comma-dangle": [2, "never"],
        //在写逗号时，逗号前面不需要加空格，而逗号后面需要添加空格
        "comma-spacing": [2, { "before": false, "after": true }],
        //该规则规定了在对象字面量语法中，key和value之间的空白，冒号前和冒号后面都需要一个空格
        "key-spacing": [2, { "beforeColon": true, "afterColon": true }],
        //如果逗号可以放在行首或行尾时，那么请放在行尾
        "comma-style": [2, "last"],
        //非空数组中不应该有多余的空格
        'array-bracket-spacing': [2, 'never'],
        //this的别名应该为that
        'consistent-this': [2, 'that','self','me'],
        //每一行最大字符数为110
        'max-len': [1, 110], // airbnb use 100, wishlist, one day
        // 规定callback如果具有err参数，那么应该处理它，打印err 或者 error，一般在node环境下出现。
        "handle-callback-err": [2, "^(err|error)$" ],
        //构造函数首字母大写
        "new-cap": [2, { "newIsCap": true, "capIsNew": false }],
        //禁止使用Array构造函数。视情况而定，一般情况下应该禁止。
        "no-array-constructor": 2,
        //禁止使用arguments.caller和arguments.callee，一般情况下应该禁止。
        "no-caller": 2,
        //label和var申明的变量不能重名
        "no-label-var": 2,
        //禁止使用label语句，主要防止循环复杂化，禁止go语句。
        "no-labels": [2, {"allowLoop": false, "allowSwitch": false}],
        //不要使用__proto__
        "no-proto": 2,
        //不要和自身作比较
        "no-self-compare": 2,
        //禁止对一些关键字或者保留字进行赋值操作，比如NaN、Infinity、undefined、eval、arguments等。
        "no-shadow-restricted-names": 2,
        //不要使用with语句
        "no-with": 2,
        //换行时，运算符在行尾。一般在出现在字符串拼接时
        'operator-linebreak': [2, 'after'], // aibnb is disabling this rule

        //生产环境应该禁止console，错误级别为2
        'no-console': 0, // airbnb is using warn .products should error
        //给参数重新赋值不会报告错误。但是，建议不要给参数重新赋值。
        'no-param-reassign': 0,
        //直接使用Object.prototype上的方法不会报错。但是，建议不要直接使用，应该以Object.prototype.method.call(）形式来使用
        'no-prototype-builtins': 0,

        //关于模块的引入方法，现阶段多为混用模块，故禁止提示。
        'import/no-unresolved': 0,
        'import/no-named-as-default': 0,
        'import/extensions': 0,
        'import/no-extraneous-dependencies': 0,
        'import/prefer-default-export': 0,
        'import/first' : 0,

        //允许下划线变量，underscore.js
        'no-underscore-dangle': 0,
        //不建议使用二元混合操作符，避免歧义：let a=1+2*3
        'no-mixed-operators': 1,
        //不建议使用多重赋值，比如:a=b=c=1
        'no-multi-assign': 1,
        //允许使用++号
        'no-plusplus': 0,
        //禁止使用var变量
        'no-var':2,
        //禁止直接使用arguments变量
        'prefer-rest-params': 1,
        //允许使用continue语句
        'no-continue': 0,
        //允许使用new 语句
        'no-new': 0,
        //允许使用短路求值表达式（包括三元操作符的短路求值）
        'no-unused-expressions': [2,{ "allowShortCircuit": true, "allowTernary": true } ],
        //不建议嵌套的三元操作符
        'no-nested-ternary': 1,
        //不建议声明与外层作用域同名的变量。后续应该逐渐禁止此类用法。
        'no-shadow': 1,
        //声明后不再改变的变量应该使用const进行声明，解构中不强制。
        "prefer-const": [2, {
            "destructuring": "all",
            "ignoreReadBeforeAssign": false
        }],
        //箭头函数只有一个参数时，允许省略圆括号
        "arrow-parens": [2, "as-needed"],
        //允许使用let或const为多个变量赋值
        "one-var": [1],
        //不强制检查函数是否有返回值，但是，建议每个函数都应该有返回值。
        "no-useless-return": [1],
        //不应该存在未使用的变量声明。但是，在解构赋值中，有时候确实需要剔除一些属性，然后获取剩余属性。
        //如：let { params, ...rest } = options；我们只需要options中除params外的属性，这时候，params可能就成了未使用的变量
        "no-unused-vars": [1]
        //规则太多 后面再补充

eslint的规则太多，这里不一一介绍，配合airbnb规范，可以让我们写出更加统一的代码。

在vue工程中，使用eslint也比较简单，只需要在初始化工程时，选择使用eslint即可。开启eslint后，你的每一个不符合规范的代码都会直接编译不通过。就像这样：

![](https://i.imgur.com/bcyODJE.png)

它会告诉我，我的代码缺少了一个逗号，然后编译不通常，程序不能正常运行。我们可以把这个机制集成到版本库中，让不符合规范的代码不准进行提交。

> 代码规范的东西，其实也挺多的，还有配合版本库的使用我这里也只是简单的提了一下，具体实现这里不赘述。

# 暂时先写到这里吧，许多技术点我都只点到即止，也还有许多技术点没有提到，比如：使用ES6、7、8做开发，转码工具babel，单元测试mocha和karma，包管理工具，路由机制等等，后续我再抽空补充完整。 #