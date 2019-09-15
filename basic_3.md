# Vue基础三
3.1 axios && fetch

目的： 是在框架中使用数据请求

回顾： 

1. 封装ajax
2. jquery 【  .get   .post   .ajax      .load 】

框架： 

数据请求

1. 使用原生js提供的fetch
2. 使用第三方封装库： axios
   - Vue中可以统一对axios进行挂载

         Vue.prototype.$http = axios

3. fetch vs axios
   - axios 对已获得的数据进行了一层封装 XSRF 
     - axios底层自动对数据进行了格式化
   - fetch并没有进行封装，拿到就是格式化后的数据 
     - fetch进行了多一层的格式化   
       - res.json()
       - res.blob() 格式化二进制
       - res.text()

    Axios总结
        1.get方法
    
    
        A: 无参数
            axios.get(url).then(res=>console.log(res).catch(error=>conosle.log(error))
        B: 有参数
            axios({
                url: 'http://xxx',
                method: 'get' //默认就是get,这个可以省略，
                params: {
                    key: value
                }
            })
    
        2.post
            注意： axios中post请求如果你直接使用npmjs.com官网文档， 会有坑
            解决步骤： 
                    1. 先设置请求头 
                    2. 实例化 URLSearchParams的构造器函数得到params对象
                    3. 使用params对象身上的append方法进行数据的传参
    
    
    // 统一设置请求头
    axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded'; 
    let params = new URLSearchParams()
    
    // params.append(key,value)
    
    params.append('a',1)
    params.append('b',2)
    
    axios({
        url: 'http://localhost/post.php',
        method: 'post',
        data: params，
          headers: {  //单个请求设置请求头
           'Content-Type': "application/x-www-form-urlencoded"
        }
    })
    .then(res => {
        console.log( res )
    })
    .catch( error => {
        if( error ){
        throw error
    }
    })
    
    
    Fetch
    1.get
    
    fetch('http://localhost/get.php?a=1&b=2')
        .then(res=> res.text()) // 数据格式化 res.json() res.blob()
        .then(data => {
            console.log( data )
        })
        .catch(error => {
            if( error ){
            throw error
        }
    })
    
    注意事项：
        A: fetch 的 get 请求的参数是直接连接在url上的， 我们可以使用Node.js提供的url或是qureystring模块来将
            Object --> String
        B: fetch 的请求返回的是Promise对象，所以我们可以使用.then().catch(),但是要记住.then()至少要写两个， 第一个then是用来格式化数据的，第二个then是可以拿到格式化后的数据
            格式化处理方式有
    fetch('./data.json')
    .then(res=>{
        res.json() //res.text() res.blob()
    })
    .then( data => console.log(data))
    .catch( error => console.log( error ))
    
    
    2.post
    fetch 文档
    https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch#%E8%BF%9B%E8%A1%8C_fetch_%E8%AF%B7%E6%B1%82
    
    fetch项目使用的博客
    https://blog.csdn.net/hefeng6500/article/details/81456975

- 历史
  - Vue1.0
    - Vue1.0数据请求我们使用的是一个第三方的封装库，这个封装库叫做 vue-resource
    - vue-resource现在已经淘汰了,它的作者也推荐我们使用axios
      - vue-resource使用形式和axios一样的
        - this.$http.get
        - this.$http.post
        - this.$http({})
        - vue-resource有jsonp方法,而axios是没有的
  - Vue2.0
    - axios [ 可以说是目前最好的数据请求的封装库 ]
    - fetch

3.2 计算属性

computed    是Vue中的一个选项

作用： ？

	业务： 如果我想让一个字符串反向，如何实现？

		分析： 反向 -> 数组【 reverse 】

		

3.2.1 侦听属性

watch  是Vue中一个选项

作用： ？

	监听数据的变化，当数据发生改变时，我们完成一些操作

业务： 

    watch: {
          firstName ( val ) {
            this.fullName = val + this.lastName
          },
          lastName ( val ) {
            this.fullName = this.firstName + val 
          },
          num: {
            deep: true, // 深度监听      
            handler ( val ) {
              //  当num发生改变时，触发的方法
              console.log( val )
            }
          }
        }

- 总结： methods  vs computed  vs watch
  - 项目中如何使用
    - 1. 事件处理程序： methods
    - 2. watch
         有大量数据交互和异步处理时进行
    - 3. computed
         - 有逻辑处理
         - V中像全局变量一样使用

3.3 混入 【 青铜 】

minxin

1. 混入的形式
   - 全局混入 【 不推荐 】
   - 局部混入
2. 混入的作用： 
   - 1. 将选项中某一个或是多个单独分离出去管理，让职能更加单一高效，符合模块化思想
3. 局部混入的使用
       选项  minxins
4. 全局混入
       Vue.mixin({})

3.4 组件 【 王者 】

1. 了解前端组件化发展历史
   - 前后端耦合
     - 前后端不分离项目
       - 1. 找后台搭建项目开发环境
       - 2. 寻找项目目录中的静态资源目录
            - js
            - img
            - css
       - 3. 同步修改css
   - 前后端分离
   - 前端团队合作项目的出现
     - 组件化为了解决多人协作冲突问题
     - 复用
2. 组件的概念
   - 组件是一个html 、 css 、js 、img 等的一个聚合体
3. Vue中的组件属于扩展性功能
   - 通过 Vue.extend() 来扩展的
         ƒ Vue (options) {
             if (!(this instanceof Vue)
                ) {
                 warn('Vue is a constructor and should be called with the `new` keyword');
             }
             this._init(options);
         }
         ƒ VueComponent (options) {
             this._init(options);
         }
   - VueComponet这个构造函数我们不进行new实例化，我们希望组件是以标签化的形式展示
         <Hello/> -->  <div></div>
         <Banner></Banner>
   - 组件要想合法化，必须注册解析
   - 组件的注册 【 创建 】
     - 全局注册
     - 局部注册
   - 组件的规则
   - is属性 - 动态组件 - 动态缓存组件
   - template模板标签
     - 直接子元素有且仅有一个
   - 组件的嵌套

3.5 作业

1. 面试题： ajax  和  fetch  有什么区别？ 【 提问率： 60% 】
2. 进行数据请求，然后布局一个移动端列表 【 亲亲网 】
   - 弹性盒
   - rem
3. 了解前后端耦合，前后端分离
4. 进行页面布局
   - 寺库登录页面布局
   - 卖座首页布局 
