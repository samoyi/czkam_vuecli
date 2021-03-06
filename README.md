# Vue-cli多页面应用Demo
* 因为Vue-cli默认是构建单页面应用，因此需要做一些修改。
* 使用Vue-cli 2.x版本，模板为`webpack-simple`。


## 概述
### `webpack-simple`默认设定
* 单页面，页面入口为`index.html`
* 单一entry，entry文件为`src/main.js`
* 单一output，output文件为`dist/build.js`，被`index.html`默认引用

### 多页面情况下需要进行的修改
* 每个页面都需要一个html入口文件
* 每个页面都有自己对应的entry文件
* build之后每个页面都有自己的output文件


## 创建`src/pages`目录
* 该目录下每个目录对应一个页面
* 每个页面目录下至少有两个文件：`main.js`和`main.vue`。
    这两个文件对应单页面模式下的`src/main.js`和`src/App.vue`文件。在本例中，直接把
    `src/main.js`和`src/App.vue`剪切进`src/pages/home`中，并改名为`main.js`和
    `main.vue`。
* 因为改名了，所以要把`main.js`的引用也从`App`改成`main`。


## 多html入口文件
1. 虽然可以仿照默认的`index.html`手动添加诸如`news.html`、`products.html`等文件，不过
比较麻烦，容易造成混乱。
2. 使用[html-webpack-plugin](https://www.npmjs.com/package/html-webpack-plugin)自
动生成dev和build时的html文件。
3. 安装并在`webpack.config.js`中引入，配置方法见`webpack.config.js`中注释。
4. 因为每个html入口文件都差不多，所以可以使用同一模板文件。把`index.html`改成模板文件
`template.html`：
    1. `title`标签使用模板语法，通过html-webpack-plugin来自动插入对应的`title`
    2. 删除`script`标签，通过html-webpack-plugin来自动引用对应的`build.js`文件
    3. `div`的`id`改为`wrapper`，作为每个页面的最外层节点，比`app`语义正确。
    4. 把`main.js`和`main.vue`中的`id`都从`app`改成`wrapper`
5. 因为是用同一个模板文件，所以需要在配置中给每个生成的html文件指定的对应的页面title。  
    `src/pageConfig.json`中保存每个页面的title，使用html-webpack-plugin来设置。
6. 少数情况下可能需要引用外部的css或js文件。如果不是每个页面都引用，则不能直接写在
`template.html`里。因此也要在`src/pageConfig.json`设置每个页面需要引用的外部css和js
文件路径。设置方式参考现在的`src/pageConfig.json`文件。css和js文件路径都要以数组的形式。
如果没有，可以写空数组，也可以直接不写该属性。


## 多entry
* [文档](https://webpack.js.org/concepts/entry-points/#multi-page-application)
* `/src/pages`内部的每个文件夹都会被认为是一个页面组件，内部应该都有一个`main.js`作
为入口文件。
* 具体配置见`webpack.config.js`中注释。


## 多output
* 每个页面都生成一个对应的js文件。具体配置见`webpack.config.js`中注释。


## dev
在`webpack.config.js`通过设定`cur_page`常量来决定当运行`npm run dev`时打开哪一页


## build
1. 理想的build情况是，所有的html文件在`/dist`目录，其他文件在`/dist/static`目录。
2. 不过不知道怎么实现，现在只有html文件可以存在子目录里。
3. 因此现在html文件生成在`/dist/html`目录，其他文件生成在`/dist`目录。
