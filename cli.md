# cli
> vue项目的快速构建工具    cli 【 脚手架 】
> 底层 webpack   
> 在市场上
> 	- cli2 【 扩展 】
> 	- cli3 【 大纲 】

1. 什么是cli?
> cli是vue提供的一个用来快速构建项目环境的一个工具，底层使用的是webpack 
2. cli目前有哪些版本？
> cli2   cli3 
>  * cli3对电脑的配置有一定要求
1. cli如何使用？
   1. cli的安装  【 推荐使用yarn 】
      * yarn 安装和配置
        * 安装: `$ npm i yarn -g`
        * 配置国内镜像源： `$ yarn config set registry https://registry.npm.taobao.org`
   2. 验证是否安装成功
      * 命令行输入： $ vue 看是否有东西输出，如果输出命令提示，证明安装成功
   3. 创建项目
      - cli3版本创建
        * 命令创建  【 推荐 】
          - `$ vue create 项目名称`
            - 手动选择配置
            - 如果安装node-sass出问题，如何解决： 
              - 先切换成npm源   nrm use npm
              - 使用cnpm 安装  cnpm i node-sass sass-loader -D
        * 图形界面创建
          * `$ vue ui` 
          * 创建完成的结果和使用命令创建时一样的
        * cli3版本配置介绍
          * please pick a preset( user arrow keys ) 使用键盘上下键来选择一个配置
            * default 默认配置
            * Manually select features 手动选择配置
              * babel 优雅降级   es6   --->  es5 
              * eslint js语法检测
              * CSS Pre-processors css 预处理语言  less  sass/scss  stylus
              * Linter / Formatter    eslint  /   jslint
                * Unit Testing   单元测试
                * E2E Testing    端到端的测试
              * In dedicated config files   将所选的每一个选项用一个文件来保存（ 配置 ）
              * PWA  (web app ) 在浏览器中使用的app
              * Save this as a preset for future projects?  将上面所选的配置保存下来，以备将来的项目使用

      - cli2版本创建
        * 标准版
          * `$ vue init webpack project`
        * 简易版
          * `$ vue init webpack-simple project` 

4. 分析几个版本的目录

   - cli3
      - node_modules  项目的依赖包
        - cli3 webpack配置放在node_modules中
      - public  静态资源目录（ 生产环境 ）【 这个目录下的静态资源不会被webpack 编译 】
        - img
        - js
        - css
        - favicon.ico  项目标题的logo
        - index.html   根实例组件的模板，也是整个项目的容器
      - src  源代码开发目录（ 也是开发者主要开发的目录文件夹 ）
        - assets  开发环境的静态资源目录  （ 这个目录中的资源会被webpack编译）
          - assets中如果图片的大小 > 4k 我们就原样拷贝到dist目录
          - assets中如果图片的小小 < 4K 我们就将这个图片转化成 base64 
          - base64它是我们前端处理图片的一种形式，将图片路径进行编码，它可以减少一次ajax请求，达到前端性能优化的一个方案，但是base64有一个弊端，这个弊端就是会污染html结构
        - components   组件存储目录
          - xxx.vue文件   单文件组件   一个vue文件就是一个组件
            - 组成部分
              - template模板（ 必须写的 ）
              - script脚本 （ 可写可不写）
              - style样式 （ 可写可不写 ）
                - scoped 作用是可以减少命名冲突，规定一个样式的作用域
        - .gitignore   git上传忽略文件配置
        - babel.config.js   优雅降级配置文件  用来将es6 --> es5
        - package.json  整个项目的依赖配置文件
        - README.md 整个项目启动的说明性文件
        - yarn.lock 整个项目的依赖性文件的信息
        - postcss.config.js  项目css预处理的配置
        - .browserslistrc  浏览器版本的支持
    - cli2 标准版
      - build  webpack配置
      - config  webpack配置
      - node_modules
      - src
        - static 静态资源配置
        - .babelrc  优雅降级配置文件
        - .postcssrc  css预处理配置文件
        - .editorconfig  编辑器配置文件
   - cli2 简易版
     - src 源代码开发目录
     - webpack.config.js webpack配置文件

5. 学习cli使用
   - 读懂项目结构
   - 学会创建单文件组件
     - 创建
     - 导出
     - 导入

6. cli   它是  webpack  +   es6模块化来集中实现的
  - es6模块化
    - 模块的定义
          const obj = {}
          const fn = function(){}
      
    - 模块的导出
          // 模块的导出有两种形式
          export default //默认导出一个
          
          //export // 批量导出，导出多个
      
    - 模块的引入
          // 如果是export  default 导出
          import xxx from '../xxx.xx'
          
          // 如果是export 导出
          
          improt  { xx } from '../xx.xx'  || import * as xx from '../xx.xx'
   
7. 特殊说明
   
  <font color = "#FF7F50">
    cli3 将webpack配置放在 node_modules,所以Vue并不希望大家去修改这部分webpack配置
    如果我们将来需要更改webpack配置，那么我们需要在 项目 根目录 下创建一个  vue.config.js文件
  </font>

8. 作业： 
  - 在cli3/cli2项目中完成todolist 
  - cli3 
