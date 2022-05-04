# webpack



+ 使用注意

  ```java
  /*
   1. webstorm在获取路径时 只有 ../ 才会进行路径提示，如果是当前路径则不会进行提示
   2. webpack.config.js 中 test 和exclude 属性指定的是一个正则表达式
   3.module对象下的rule是一个数组，每个索引对应一个对象，每个对象解决了某个功能(如兼容less,兼容语法)，其常用配置项有 
   	test，该loader的作用文件，是一个正则表达式
   	exclude,该loader排除的文件或目录,是一个正则表达式
   	use，该loader的具体配置，如果只用到了loader则可以写为一个字符串(用到一个loader，无额外配置项)或数组    (用到多个loader，无额外配置项)
   4. webpack只能默认处理 js 和json ，当处理其他后转时，需要借助包和module处理
   5. 在新版本的webpack中 url-loader和file-loader已经被弃用，因此无法正确压缩图片，需要添加type: 'javascript/auto'属性
   6. 当代码部署到服务器上后，默认在dist目录下，所以在查找资源时无需以dist起头 (，否则找不到资源)
   	如 原写为 ../dist/img.jpg ，后写为 /img.jpg 即可
   7. 文件处理通用配置 
   	[ext] 保留原有后缀
   	[name] 打包后保留源文件名  ，如 [name].[ext]
   	[hash:5] 给该文件取一个hash值的前5位作为文件名 ，如  [hash:5].[ext]
  */
  ```
  
  

+ webpack遇到的坑

  ```java
  /*
   1. 在使用 html-webpack-plugin 插件时，该模板html文件 不能引入 link(css) 和script(js文件),且作为模板的html文件路径一定要正确，否则会报错
   2. url-loader 版本更新 需要额外配置 type: 'javascript/auto' 属性
   3. devtool更新 ，以前不能使用
   4. 当项目上线后，所有的相对路径都需要改写为绝对路径，所有资源会被放到服务器的public目录下
   5. 当路径为 /xx/ 时，服务器会自动补全根路径
  */
  ```

  

  



## 1.0 构建工具

源代码：程序员写完没有经过任何加工处理的代码

src或lib文件夹里放的都是源代码  ，dist 文件夹里放 的是加工后的代码

构建：将程序员写的源代码，经过编译，压缩，语法检查，兼容性处理，生成浏览器可以高效，稳定运行的代码

常见构建工具：grunt(已经淘汰了的构建工具),gulp(基于node的构建工具),webpack(主流构建工具)

## 1.1 webpack配置

webpack主要是找到自己想要且合适的包或插件，进行引入即可

loader模块读取从下到上

在每次修改代码时都需要重新使用webpack编译(解释)一遍

特点：webpack是一个模块打包器(基于node.js)，前端所有的资源文件都会作为模块处理，它会根据模块的依赖关系进行静态分析，生成对应的静态资源

+ 五个核心概念

  Entry:入口起点(entry point) 指示webpack应该使用哪个模块作为构建依赖图

  Output:output属性告诉webpack在哪输出它所创建的bundles,以及如何命名这些文件

  Loader:loader让webpack能够去处理非Javascript文件(webpack自身只能解析js和json)

  Plugins:插件可以用于执行范围更广的任务，插件范围包括，从打包优化和压缩，一直到重新定义环境中的变量等

  Mode:模式，有生产模式production和开发模式development

+ json文件必须有内容，写法和对象类似，但键值对必须都是字符串，结尾不能有逗号(有逗号标识还有剩余内容)，且无法写注释



+ webpack使用

  ```js
  /*
   在模块化技术中index.js用于汇总各个js模块，也可以汇总js,css,图片，视频等，该index.js为webpack的入口文件
   
  在没有加任何插件时，webpack只能引入js和json文件 
    import {sum} from ./a
    import a from ./b.json
    
    1. 安装webpack
    npm i webpack webpack-cli -D  局部以开发环境安装webpack和webpack-cli
    npm i webpack webpack-cli -g  全局环境安装webpack和webpack-cli
    
    
    2. 文件打包
     开发配置命令：webpack ./src/js/app.js -o ./dist/js/app.js --mode development 能够编译打包js和json文件，并且能将es6的模块话语法转换为浏览器可识别语法
     生产配置命令：webpack src/js/app.js -o dist/js/app.js production 能够在开发配置功能上加上一行压缩代码
     结论：webpack能够编译打包js和json文件，能将es6模块化语法转换为浏览器可识别语法，能压缩代码
     缺点：不能编译打包css,img等文件，不能将js的es6基本语法转换为es5以下语法
    
    3. 配置webpack文件
    在项目根目录下创建webpack.config.js
    内容：
    let {resolve} = require('path')
    module.exports = {
    	entry:'./src/js/index.js',
    	output:{
    		path: resolve(__dirname,'dist'), // 选择路径,需引入path模块
    		filename:'my-webpack.bundle.js'
    	},
    	mode:'production'
    }
    注意
    	此配置文件也是基于node.js的，即没有node.js环境此入口js无法运行 
    	该文件本质上就是向外暴露一个对象，最终由webpack解析执行
    	所有用到的资源需要指定在入口文件中，只有入口文件中的内容才会被编译
    */
  ```

+ loader转换less

  ```js
  /*
  1.loader下的包解析less
    // less引入方法
    在入口文件中引入样式，不用变量接，不用写from
    import '../css/index.less'
    因为webpack只能解析js和json文件,不能解析其他的格式，所以这个时候引入loader，使用里面的less-load,css-loader,less,style-loader
    npm i less-loader css-loader less style-loader -D（下载到开发依赖） 即可安装此分支包
  
   loader下的包依赖使用，在配置完成后只用输入webpack(terminal中)即可完成压缩
  const { resolve } = require('path')
  
  module.exports = {
    entry: './src/js/index.js', // 设置模块入口文件，此时所有包都会集中到此入口
    output: { // 设置模块出口位置
      // path.resolve('初始位置','目标位置')
      path: resolve(__dirname, 'dist'), // 输入路径
      filename: './js/index.js'
    },
    mode: 'production', // 压缩方式
  
    // 所有的loader都要在module对象中的rules属性中
    // rules是一个数组，数组中每一个对象就是一个loader
    // loader 特点：下载后无需引入，只需声明
    module: {
      rules: [{
        // 解析less
        test: /.less$/, // 匹配所有的less文件
        // 数组里可放置 loader: 'xxx-loader'键值对(use:[{loader:'style-loader'},xxx]),也可放置单独的数据,'xxx-loader'此时loader键可以省略
        use: [
          // 以下排列顺序不能改变 ，loader执行顺序从右到左，从下到上
          'style-loader', // 用于在html文档中创建一个style标签,将样式 插入
          'css-loader', // 将less编译后的css转换为CommonJS的一个模块
          'less-loader' // 将less编译为css,但不单独生成css文件,此时css在内存中
        ]
      }]
    }
  }
  
  
  */
  ```





+ webpack语法检查(eslint)

  ```js
  /*
  使用 npm i  eslint-loader eslint -D 下载检查工具，在package.json文件后添加此配置项
  
     "eslintConfig": {
      "parserOptions": { // 设置检查范围
        "ecmaVersion": 6, // 指出es6语法
        "sourceType": "module" // 以模块使用
      },
      "env": {  // 设置代码可运行环境
        "browser": true, // 支持浏览器
        "node": true // 支持node
      },
      "globals": { // 声明使用全局的变量，即使没有定义也不报错(使用jquery用到的$符)
          "$": "readonly" // 设置全局变量$ 为只读
      },
      "rules": { // 设置eslint的检查规则 ，0 为 忽略，1 为 警告，2 为报错
        "no-console": 0, // 不检查console
        "eqeqeq": 2, // 检查做判断时 使用全等 ===
        "no-alert": 2 // 检查alert
      },
      "extends": "eslint:recommended" // 使用eslint推荐的配置
    }
  
  再在webpack.config.js文件中的rules下进行配置
  {
          test: /\.js$/,
          exclude: /node_modules/,
          enforce: "pre", // 提前加载eslint,为了程序的安全
          use:  "eslint-loader"
        }
  
  */
  ```

  

+ webpack语法转换(兼容处理)

  ```js
  /*
   安装检查loader包
   npm i babel-loader @babel/core @babel/preset-env @babel/polyfill -D
   @babel/polyfill 可以转换promise语法 引入：在index.js文件中 import '@babel/polyfill' 即可
   @babel/preset-env 可以转换es6常规语法 ， @babel/polyfill和 @babel/preset-env一起使用可以解决大多兼容问题
   
   注意
   	直接引入 @babel/polyfill 则是引入了所有的兼容处理方法，此方式会导致文件体积较大，因此需要按需引入
   
   方法一： （此方法须在index.js中引入,可会导致文件体积较大，因此需要使用方式二）
   webpack配置：
   {
          test: /\.js$/, //只检测js文件
          exclude: /node_modules/,
          use: {
            loader: "babel-loader",
            options: {
              presets: ['@babel/preset-env']
            }
          }
        }
        
  此方式虽可坚决兼容性问题，但文件体积较大(引入了所有兼容方式)，需要引入 core-js包，实现按需加载（需注销掉主文件引入的 import '@babel/polyfill'）
  
  方法二：(最终版)
  安装 npm i core-js
   配置信息
  {
          test: /\.js$/, //只检测js文件
          exclude: /node_modules/,
          use: {
            loader: "babel-loader",
            options: {
              presets: [
                ['@babel/preset-env',  
                  {
                    useBuiltIns: 'usage', // 按需引入用到的polyfill
                     corejs: { version: 3 },//解决找不到corejs问题
                    targets: { // 配置兼容版本
                      "chrome": "58",
                      "ie": "9",
                    }
                  }]
              ],
              cacheDirectory: true, // 缓存已经使用的模块
            }
          }
        }
  */
  ```

+ loader处理在样式中引入图片

  ```js
  /*
   base64 ，大于8kb不适用此方式，小于8kb使用此方式
   经过样式插入的图片webpack也解析不了，需要安装url-loader和file-loader进行加工处理，url-loader是对file-loader的上层封装，使用时需配合使用
   下载 url-loader及 file-loader
   npm i file-loader url-loader -D
   
    {
          test: /\.(png|jpg|gif)$/, // 匹配资源
          use: [
                {
                    loader: "url-loader", //
                    options: {
                        limit: 8192, // 8kb 以上转base64(这样会占用更少体积)
                        publicPath: "../dist/images", // 引入图片的位置(以当前图片文件夹位置为参考点)
                        outputPath: "images", // 输出文件夹
                        name: "[hash:5].[ext]", // 图片名改为 5位哈希值
                        esModule: false // 兼容 node和html引入
                          }
                      }
                  ],
  
                  type: 'javascript/auto'
              },
  */
  ```

+ loader打包html文件

  ```js
  /*
  注意
  	使用此插件时，不能在 模板html页面中 引入css和js 否则会报错，该插件会帮我们引入
  
   html文件webpack不能解析需要借助插件编译
   安装插件 npm i html-webpack-plugin -D
   与module同级生成一个html配置对象
    plugins: [
      new HtmlWebpackPlugin(
        {
          template: './src/index.html' //以当前文件的html为原型生成html模板
        }
      )
    ]
    这时可以将html完美复刻到dist目录下，可是此时img标签引入的图片路径会出现问题，所以需要用到 html-loader包
    npm i html-loader -D 安装
    在module里的rules下编写
    {
          test: /\.(html)$/,
          use: 'html-loader'
        }
  */
  ```

+ loader打包其他资源

  ```js
  /*
   使用之前安装的file-loader配置其他资源,(如果是css资源，则修改其后缀为less即可，less和css语法大致一致)
   在module的rules下写
    {
          test: /\.(eot|svg|woff|woff2|ttf|mp3|m4p|avi)$/,
          loader: 'file-loader',
          options: {
            outputPath: 'media', // 输出的文件路径
            name: '[hash:8].[ext]' // 限制文件名哈希值个数
          }
        }
  */
  ```

+ 自动编译打包运行

  ```js
  /*
   安装loader
   	npm i webpack-dev-server -D
   
   配置
      将package.json下的script对象下的值替换为 "start":"webpack-dev-server"即可一键开启静态服务器，在这个服务器里存放的是你关联的依赖资源(js,css等)，服务器内容存放再内容里
      此服务器会在代码修改后实时自动刷新(全部)，有时候我们只是想修改的哪个模块刷新(模块热更新 英文名HMR)，就要添加一个选项 （hot:true）
   
  devServer: {
      open: true, // 自动打开浏览器
      compress: true, // 启动zip压缩
      port: 4000,//端口号
      hot: true // 开启了模块热更新
    }
  
  注意
  	当开启了模块热更新后,此时在浏览器不刷新的情况下实时监控 /src/js/index.js的变化(监听entry内的文件)，当其改变后就会自动渲染页面。可此时因为html页面并未监听(改变html标签不会进行热更新)，因此需要添加html地址，此时才完成了完全模块热更新
    entry: ['./src/js/index.js', './src/index.html'] (指定js和html地址)
  */
  ```

  

+ node初始化包(package)下的script

  ```js
  /*
   node初始化包(package)里的script可以快速访问一些指令，如
   "scripts": {
   // npm start 可以直接执行 start对应的值 命令安装express，其余的键值对命令只能使用 npm run 键 执行对应的 值 内容
   	"start":"webpack-dev-server",
      "express": "npm i express", // npm run express
      "mongoose": "npm i mongoose" // npm run mongoose
    }
  */
  ```

  

+ devtool

  ```js
  /*
   将一种压缩/编译文件中的代码映射会源文件中原始位置的技术，让错误更加明显清晰
   devtool: "operate"
   开发环境： cheap-module-eval-source-map , 如 devtool:"cheap-module-eval-source-map "
   生产环境: cheap-module-source-map  , 如 devtool: "cheap-module-source-map"
  */
  ```
  
  
  
+ 准备生产环境

  ```js
  /*
  1. 创建文件夹config,将webpack.config.js复制两份
   ./config/webpack.dev.js(开发)
   ./config/webpack.prod.js(生产)删除webpack-dev-server配置
   
   2.在package.json中设置添加script描述
   // --config 选择执行的webpack.js目录
   "scripts": {
      "start": "webpack-dev-server --config ./config/webpack.dev.js",
      "dev": "webpack-dev-server --config ./config/webpack.dev.js",
      "build": "webapck --config ./config/webpack.prod.js"
    },
    
   3.快速部署服务器
   安装
   	npm i serve -g 
   使用
   	serve (引入的资源文件夹) -p 5000 。如 serve dist -p 5000 ,此时就把指定目录 部署到了本地服务器
  */
  ```

  

+ 清空打包文件目录

  ```js
  /*
   每次打包生成了文件，都需要手动删除，引入插件帮助快速清空文件夹
   安装：npm i clean-webpack-plugin -D
   使用：const {CleanWebpackPlugin} = require('clean-webpack-plugin')
   在plugins模块下追加一个CleanWebpackPlugin实例对象  
   plugins:[new CleanWebpackPlugin()]
  */
  ```

+ 提取css为单独文件

  ```js
  /*
   安装: npm i mini-css-extract-plugin -D
   loader模块读取从下到上，想外部引入css文件需要改变less配置信息
   {
   	test:/\.less$/,
   	use: [
            // 以下排列顺序不能改变
            // 'style-loader', 不在生成单独的style样式 // 用于在html文档中创建一个style标签,将样式 插入
            MiniCssExtractPlugin.loader, // 引用此模块里的loader即可生成.css文件然后link:css引入即可
            'css-loader', // 将less编译后的css转换为CommonJS的一个模块
            'less-loader' // 将less编译为css,但不单独生成css文件,此时css在内存中
          ]
   }
   plagins:[
       new MiniCssExtractPlugin({
            filename: 'css/[hash:5].css' // 把该css文件放入css文件夹，[hash:5]标识用哈希值代替其之前的名字
          })
   ]
   
   注意
   	此时不能直接打开index.html文件，而是需要以服务器打开，因为直接打开路径会出现问题
  */
  ```

  

+ 处理css兼容

  ```js
  /*
   安装
   npm i postcss-loader postcss-flexbugs-fixes postcss-preset-env postcss-normalize autoprefixer -D
   
   然后再less-loader和css-loader之间配置
     {
                          loader: "postcss-loader",
                          options: {
                              postcssOptions:{
                                  plugins: [
                                      require("postcss-flexbugs-fixes"),
                                      require("postcss-preset-env")({
                                          autoprefixer: {
                                              flexbox: "no-2009" // 不支持2009年的flex写法
                                          },
                                          stage: 3  // 兼容最新写法
                                      }),
                                      require("postcss-normalize")() // 支持老浏览器
                                  ],
                              },
  
                              sourceMap:true
                          }
                      },
        
  =======================================
  配置完loader后，还需创建一个 .browserslistrc 文件来联合完成控制
    内容
  	last 1 version
  	> 1%
  	IE 9 # sorry
  */
  ```

  

+ 压缩css

  ```js
  /*
   npm i css-minimizer-webpack-plugin -D
   引入模块
   const OptimizeCssAssetsPlugin = require('optimize-css-assets-webpack-plugin')
   配置模块
   new OptimizeCssAssetsPlugin()
        
   
  */
  ```
  
  
  
+ 压缩html

  ```js
  /*
  
  
   修改插件，添加一些配置
   new HtmlWebpackPlugin({
        template: './src/index.html', // 以当前文件为模板创建新的HtML(1. 结构和原来一样 2. 会自动引入打包的资源)
        minify: {
          removeComments: true, //移除注释
          collapseWhitespace: true, //折叠所有留百
          removeRedundantAttributes: true, //移除无用的标签
          useShortDoctype: true,//使用短的文档声明
          removeEmptyAttributes: true,//移除空标签
          removeStyleLinkTypeAttributes: true,//移除rel="stylesheet"
          keepClosingSlash: true,//自结束
          minifyJS: true, // 压缩js
          minifyCSS: true, // 压缩 css
          minifyURLs: true, // 压缩 url
        }
      }),
  */
  ```
  
  

## 1.2 完整配置

+ webpack.js

  ```js
  // 生产环境
  const {resolve} = require("path");
  const HTMLWebpackPlugin = require("html-webpack-plugin");
  const {CleanWebpackPlugin} = require("clean-webpack-plugin");
  const MiniCssExtractPlugin = require("mini-css-extract-plugin");
  const CssMinimizerWebpackPlugin = require("css-minimizer-webpack-plugin");
  
  
  module.exports = {
      entry: ["./src/js/index.js", "./src/index.html"], // 指定入口文件(路径)和devServer的模块热更新范围
      output: {
          path: resolve(__dirname, "../build"), // 指定出口路径，需要是一个绝对路径
          filename: "./js/app.js",
          publicPath: "/" // 所有输出资源在引入时的公共路径
      },
  
      // 所有的loader都要放在module对象的rules数组中
      // loader的运行顺序为 从右到左，从下到上
      module: {
          rules: [
              //此模块用于将Less编译为css文件
              {
                  test: /\.less$/, // 匹配所有less结尾的文件
                  use: [
                      // "style-loader", // 用于在html文档中创建一个style标签将该样式进行载入
  
                      // 注意：如果想生成一个单独的css文件，则需要使用MiniCssExtractPlugin.loader
                      //    并注释掉 style-loader ,因为其会将该css直接插入指定html文件。而不能生成文件
                      MiniCssExtractPlugin.loader, // 生成css文件并自动载入app.js
                      "css-loader", // 将less编译的css转换为Commonjs的一个模块
  
                      // 解决css兼容问题 ，需要在css模块化之前，因此因放到此位置进行
                      {
                          loader: "postcss-loader",
                          options: {
                              postcssOptions:{
                                  plugins: [
                                      require("postcss-flexbugs-fixes"),
                                      require("postcss-preset-env")({
                                          autoprefixer: {
                                              flexbox: "no-2009" // 不支持2009年的flex写法
                                          },
                                          stage: 3  // 兼容最新写法
                                      }),
                                      require("postcss-normalize")() // 支持老浏览器
                                  ],
                              },
  
                              sourceMap:true
                          }
                      },
                      "less-loader" // 将 less 编译为css文件放到内存中
                  ],
              },
  
              //此模块用于语法检查,再次只做loader引入，具体检查规则需写在package.json中(效率高)
              {
                  test: /\.js$/,  // 设置此loader对什么文件生效
                  exclude: /node_modules/, // 设置不匹配的文件或目录
                  enforce: "pre",
                  use: "eslint-loader"
              },
  
              // 语法兼容loader
              {
                  test: /.js$/,
                  exclude: /node_modules/,
                  use: {
                      loader: "babel-loader",
                      options: {
                          presets: [
                              ["@babel/preset-env",
                                  {
                                      useBuiltIns: "usage",
                                      corejs: {version: 3}, // 解决webpack无法找到core-js
                                      targets: {
                                          "chrome": "60",
                                          "ie": "10"
                                      }
                                  }]
                          ]
                      }
                  }
              },
  
              // 处理css样式中引入的图片路径问题
              {
                  test: /\.(png|jpg|gif)$/, // 匹配资源
                  use: [
                      {
                          loader: "url-loader", //
                          options: {
                              limit: 8192, // 8kb 以上转base64(这样会占用更少体积)
                              publicPath: "/images", // 引入图片的位置(以当前图片文件夹位置为参考点)
                              outputPath: "images", // 输出文件夹
                              name: "[hash:5].[ext]", // 图片名改为 5位哈希值
                              esModule: false // 兼容 node和html引入
                          }
                      }
                  ],
  
                  type: 'javascript/auto' // 解决url-loader 的版本问题
              },
  
              // 处理html 中 img标签引入的图片路径问题
              {
                  test: /\.(html)$/,
                  use: 'html-loader'
              },
  
              // 匹配多媒体资源(包括字体)
              {
                  test: /\.(eot|svg|woff|woff2|ttf|mp3|m4p|avi)$/,
                  loader: 'file-loader',
                  options: {
                      outputPath: 'media', // 输出的文件夹
                      name: '[hash:8].[ext]' // 限制文件名哈希值个数
                  }
              }
          ]
      },
  
  
      plugins: [
          // 打包一个html到指定位置
          new HTMLWebpackPlugin({
              template: "./src/index.html", // 选择一个模板作为复制样本
              title: "test",  // 设置该html文件的标题
  
              // 压缩html
              minify: {
                  removeComments: true, //移除注释
                  collapseWhitespace: true, //折叠所有留百
                  removeRedundantAttributes: true, //移除无用的标签
                  useShortDoctype: true,//使用短的文档声明
                  removeEmptyAttributes: true,//移除空标签
                  removeStyleLinkTypeAttributes: true,//移除rel="stylesheet"
                  keepClosingSlash: true,//自结束
                  minifyJS: true,
                  minifyCSS: true,
                  minifyURLs: true,
              }
          }),
  
          // 每次打包文件到指定目录时，先将其清空
          new CleanWebpackPlugin(),
  
          // 提取css为一个单独文件
          new MiniCssExtractPlugin({
              filename: "css/[hash:5].css" // 会将生成的css文件放入到指定目录，[name]属性 会保留源文件的名字
          }),
  
          // 压缩css代码
          new CssMinimizerWebpackPlugin(),
  
  
      ],
      mode: "production",// 配置工作环境
  
  }
  
  ```

  

+ package.json

  ```json
  {
    "scripts": {
      "start": "webpack-dev-server --config ./config/webpack.dev.js",
      "build": "webpack --config ./config/webpack.prod.js"
    },
    "devDependencies": {
      "@babel/core": "^7.17.8",
      "@babel/polyfill": "^7.12.1",
      "@babel/preset-env": "^7.16.11",
      "autoprefixer": "^10.4.4",
      "babel-loader": "^8.2.4",
      "clean-webpack-plugin": "^4.0.0",
      "core-js": "^3.21.1",
      "css-loader": "^6.7.1",
      "css-minimizer-webpack-plugin": "^3.4.1",
      "eslint": "^7.32.0",
      "eslint-loader": "^4.0.2",
      "file-loader": "^6.2.0",
      "html-loader": "^3.1.0",
      "html-webpack-plugin": "^5.5.0",
      "less": "^4.1.2",
      "less-loader": "^10.2.0",
      "mini-css-extract-plugin": "^2.6.0",
      "postcss-flexbugs-fixes": "^5.0.2",
      "postcss-loader": "^6.2.1",
      "postcss-normalize": "^10.0.1",
      "postcss-preset-env": "^7.4.3",
      "style-loader": "^3.3.1",
      "url-loader": "^4.1.1",
      "webpack": "^5.70.0",
      "webpack-cli": "^4.9.2",
      "webpack-dev-server": "^4.7.4"
    },
    "eslintConfig": {
      "parserOptions": {
        "ecmaVersion": 6,
        "sourceType": "module"
      },
      "env": {
        "browser": true,
        "node": true
      },
      "globals": {
        "$": "readonly",
        "Promise": "readonly"
      },
      "rules": {
        "no-console": 0,
        "eqeqeq": 2,
        "no-alert": 2
      },
      "extends": "eslint:recommended"
    },
    "browserslist": {
      "development": [
        "last 1 chrome version",
        "last 1 firefox version",
        "last 1 safari version"
      ],
      "production": [
        ">0.1%",
        "not dead",
        "not op_mini all"
      ]
    }
  }
  
  ```

  

