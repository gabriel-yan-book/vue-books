# vue-router [版本3.x]
<table>
  <tr>
    <td bgcolor = "#00FFFF"> SPA  （ single page App ） 单页面应用 </td>
  </tr>
</table>

1. 多页面应用
有多个html文件，通过a标签的连接联通各个页面

- 缺点
  - 开发起来太冗余，编译、压缩很耗时间
  - 页面之间的跳转速度太慢，这个时候就会出现一个严重的问题，白屏

2. 单页面应用
   - 不需要刷新页面，因为它就是一个页面
   - 这个页面内容在切换
   - 单页面内容之间的切换要想实现我们就是用路由了
   - 如今我们的app、后台管理系统 主要的开发形式就是spa

<table>
  <tr>
    <td bgcolor = "#00FFFF"> vue路由功能图示 </td>
  </tr>
</table>

![](http://git.oschina.net/liuyuantao/vue2-WeChat/raw/master/src/assets/images/gif/tab-switch.gif)


![](http://test.fe.ptdev.cn/elm/screenshots/confrimOrder.gif)

![](http://static.oschina.net/uploads/space/2017/1018/074540_IE6W_2720166.gif)


<table>
  <tr>
    <td bgcolor = "#00FFFF"> vue-router-基础 </td>
  </tr>
</table>

1. vue 路由的mode(模式)有几种， 分别是什么？在那些环境下运行？ 【 黄金 】
   - hash: 使用 URL hash 值来作路由。支持所有浏览器，包括不支持 HTML5 History Api 的浏览器。 #/home
   - history: 依赖 HTML5 History API 和服务器配置。【需要后端支持】  /home
   - abstract: 支持所有 JavaScript 运行环境，如 Node.js 服务器端。如果发现没有浏览器的 API，路由会自动强制进入这个模式。【 这个模式不常用 】
   - hash/history 常用于浏览器端，abstract用于服务端
2. 路由的使用步骤
   1. 安装 vue-router   yarn add vue-router
   2. 在src目录下创建一个router目录， 里面创建一个index.js文件  ， 这个目录就是router的模块
   3. 引入第三方的依赖包, 并注册路由
            import Vue from 'vue'
            import VueRouter from 'vue-router'
          
            Vue.use( VueRouter ) //使用vue-router这个第三方插件
      注意： import这个关键字要放在整个文件的上层
   4. 创建了一个router对象实例，并且创建路由表
            const routes = [ 
              {
                path: '/home',
                component: Home
              }//每一个对象就是一个路由
            ]
            const router = new VueRouter({
              routes//路由表  必写的
            })
   5. 导出router实例
export default router
   6. 入口文件main.js中引入路由实例  router , 然后在根实例中注册
            import router from './router/index.js'
            new Vue({
              router,
              render: (h) => App 
            }).$mount('#app')
   7. 给路由一个路由展示区域
      1. 如果是以及路由， 那么我们放在App组件中,用一个  router-view 的组件表示
           <router-view />
   8. 当页面第一次的打开的时候， 需要做一个重定向， 就是要自动跳转到  /home 这个路由上
              const routes = [
                { //我们要求这个路由的配置要放在路由表的最上方
                  path: '/',
                  redirect: '/home'
                }
              ]
   9. 业务： 错误路由匹配
    ```javascript
      const routes = [
        {
          path: '/',
          redirect: '/home'   //重定向
        },
        {
          path: '/home',
          component: Home
        },
        {
          path: '/list',
          component: List
        },
        {
          path: '/detail',
          component: Detail
        },
        {
          path: '/login',
          component: Login
        },
        {
          path: '/register',
          component: Register
        },
        {
          path: '/user',
          component: User
        },
        {
          path: '/shopcar',
          component: Shopcar
        },
        {
          path: '/error',
          component: Error
        },
        {  //这个就是错误路由匹配， vue规定这个必须放在最下面，它必须将上面的路由全找一遍，找不到才用当前这个
          path: '**',
          redirect: '/error'
        }
      ]
    ```
   10. vue路由模式的确定 mode
       1. 如果你使用的是 hash , 那么a标签就可以了、
       2. 如果你使用 history , 那么我们最好将a标签改成  router-link 这个组件  
          - router-link 这个组件 身上必须要有一个 to 属性
          - router-link 这个组件身上加一个 keep-alive属性可以进行浏览器缓存
   11. 二级路由
    ```javascript
               const routes = [
                 {
                   path: '/shopcar',
                   component: Shopcar,
                   children: [
                     {
                       path: 'yyb', //不写  /
                       component: Yyb
                     }
                   ]
                 }
               ]
    ```
   - 注意： 写好配置之后，不要忘记了， 在对应的一级路由的组件中书写 路由展示区域
   12. 命名路由
      - 作用： 就是简写路径了
        ```javascript
             {
                 path: '/shopcar',
                 component: Shopcar,
                 //子路由 
                 children: [
                   { 
                     path: 'yyb', // 容易犯错点  /yyb X 
                     component: Yyb,
                     name: 'yyb' //命名路由
                   },
                   {
                     path: 'junge',
                     component: Junge
                   }
                 ]
                 
               },
        ```
       - 使用： 单项数据绑定to属性
       ```html
               <router-link :to = "{name:'yyb'}"/>
       ```


<table>
  <tr>
    <td bgcolor = "#00FFFF"> vue路由-进阶 </td>
  </tr>
</table>


案例： 移动端

- 【 分类 -》 列表 -》 详情  

路由进阶 - 动态路由 

核心功能

- 路由传参  
- 路由接参 

- 动态路由：
  - url中路由是改变的，但是改变路由公用一个组件
  - 举例： 
    - localhost:3000/detail/001?a=1&b=2       
    - localhost:3000/detail/002?a=2&b=3
    - detail



- vue cli3 配置反向代理         20分钟
  - 在根目录下面新建一个 vue.config.js
  ```javascript

      // vue.config.js中可以默认直接使用  http-proxy-middleware
      module.exports = {
        devServer: {
          proxy: {
            '/douban': { // /douban 是一个标记
              target: 'http://api.douban.com', // 目标源
              changeOrigin: true, // 修改源
              pathRewrite: {
                '^/douban': '' 
              }
            },
            '/siku': {
              target: 'https://android.secoo.com',
              changeOrigin: true,
              pathRewrite: {
                '^/siku': ''
              }
            }
          }
        }
      }
    
    /*
    	注意： 修改了项目配置文件，项目必须重启
    */
  ```
- 路由的传参   10分钟

      <router-link :to = "{name: 'list',params: {id: xxx}, query: {xxx:xxx}}"></router-link>

- 路由的接参
  - 我们发现凡是使用了路由的组件，我们统称为： 路由组件
  - 路由组件身上会自动添加一个  $route的数据
        id: this.$route.params.id
        query: this.$route.query.xxx
- 编程式导航     5分钟
  - push 
    - this.$router.push('/home') 
    - this.$router.push({name,params,query})
    - push可以将我们的操作存放到浏览器的历史记录
  - replace 
    - this.$router.replace('/home')
    - this.$router.replace({name,params,query})
    - replace没有将我们的操作存放到浏览器的历史记录， 效果为返回了二级
  - push/replace的参数就是to属性的参数
- 业务： 
  - 按钮的返回
    - push
    - replace
    - back
    - go

思考： 有一个业务，当我们点击 /mine的时候，要自动跳转到 /mine/login,这个时候我们发现手段不够用了，生命周期钩子函数也实现不了，这个我们想，如果我们能监听到路由的变化，那该有多好？



思考： 如果用户没有登录，那么我们是无法进入首页的，如果登录了，那就可以进入



解决; vue为了能够监听路由的变化情况，给了一个解决方法： 这个就是导航守卫

<table>
  <tr>
    <td bgcolor = "#00FFFF"> vue-router - 进阶 - 导航守卫 </td>
  </tr>
</table>


别名： 

- 导航守卫
- 路由守卫
- 路由钩子
- 路由拦截

1. 作用： --- 类似 【保安】
   - 守卫路由
     - 进 
       - 举例： 携带数据进
     - 出
       - 举例： 事情完成才能出
2. 导航守卫一共有三种形式 【 项目三选一 】
   - A: 全局导航守卫
     针对的整个项目，也就是管理或是监听整个项目
     1. 全局前置守卫 router.beforeEach(fn)
        1. fn中有三个参数
           1. to： 目标路由
           2. from： 当前路由
           3. next： 它是一个拦截，表示是否允许通过
              1. true/false/'/login'/{ name: 'login'}/ vm => {}
        2. 使用场景： 当我们本地存储/cookie中有token，那我们就自动跳转 /mine
     2. 全局的解析守卫 
        1. 在 2.5.0+ 你可以用 router.beforeResolve 注册一个全局守卫。这和 router.beforeEach 类似，区别是在导航被确认之前，同时在所有组件内守卫和异步路由组件被解析之后，解析守卫就被调用。
        2. 必须保证整个项目的守卫和异步路由组件解析完成
     3. 全局的后置守卫
        - 可以做一些用户友好提示
   - B: 路由独享守卫 ·
     路由配置选项中的一个
     - 写在路由表中的守卫钩子
     - 针对的是和当前路由相关的，那么其他与之不相关的路由，它是无法监听它的变化情况的
     - 做路由拦截
       - 案例： 权限验证
         - 数据库： 用户组
           - 普通用户
           - 管理员
           - 超级管理员
         - 我们登录式，后台会返回给我们info信息，通过信息来判断它是哪个类型用户
   - C: 组件内守卫【 王者 】
     当前组件
     - 组件内的前置守卫   beforeRouteEnter((to,from,next)=>{})
       - 导航进入组件时，调用
       - this是访问不到的，如果非要访问this ,必须通过 next(vm=>{})访问
       - 因为组件此时没有创建，所以没有this
       - 案例： 数据预载（进入组件前就获得数据）
               next(vm => { //vm指的就是组件
                  const result = JSON.parse(res.data.slice(7,-1)).rp_result.categorys
                  vm.$set(vm.category,'categorys',result)
                })
     - 组件内的后置守卫
       - 当离开组件时，调用
       - this是可以访问到 
       - 案例： 注册/内容提交，用户交互提示 
     - 组件内的更新守卫（ 路由传参和路由的接参 ）
       - 在当前路由改变，但是该组件被复用时调用
       - 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
       - 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
       - 可以访问组件实例 this
3. 功能： 导航守卫可以监听路由变化情况，做路由跳转拦截
4. 名词
   - 前置： 要进入当前路由 --- 老师进入教室前
   - 后置： 要离开当前路由 --- 老师离开教室
5. 关于next的使用
   - next()  等价于  next( true )  表示可以从当前路由跳转到目标路由
   - next( false ) 表示不通过， 表示从当前路由跳转不到目标路由 
   - next('/login') 等价于  next({path:'/login'}) 跳转指定的路由
   - next('/login') 等价于  next({path:'/login',params,query})
   - next( fn ) 数据预载
         next( vm => {})
     
6. 业务： 当我们进入到一个项目的首页时，但是当我们没有注册账号时，它主动跳转到了注册/登录页
  ```javascript

      router.beforeEach((to,from,next) => {
          // next() // 默认true
          // next( false )
          // next()
    
          // 判断用户是否登录，我们找token值  cookie   localStorage 
    
          const token = localStorage.getItem('token')
    
          if ( to.path === '/home' ) {
            next()
          }
          if ( to.path == '/aa') {
            next()
          }
    
          if ( to.path === '/mine/login' ) {
            if ( token ) {
              // 条件成立，那么我们就可以进行页面跳转了
              next('/home')
            } else {
              // 条件不成立，留在当前页面 
              next()
            }
          } else {
            next( false )
          }
    
        })
  ``` 

7. 业务： 当进入mine页面的时候， 要判断用户是否登录，如果没有登录，跳转登录页
8. 路由导航守卫
   - 3中类型  7个路由监听钩子

- 业务：
  - 监听整个项目的路由变化情况          全局的前置守卫
  - 监听某个路由的变化情况             路由的独享守卫
  - 监听的路由组件的路由变化情况         组件内的导航守卫

9. 动效
   - animate.css   来实现路由动画
10. 路由懒加载
    1. Vue异步组件 + Webpack
11. 动态缓存
    - router-link 加一个keep-alive属性
          <router-link to = "" keep-alive></router-link>
12. 作业： 
    - 我演示内容要写出来，脱离视频写出来 
