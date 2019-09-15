# Vue基础四
4.0 组件的通信 【 王者 】

为什么要进行组件通信？

	组件是一个聚合体，将来项目要合并，那么必然各个组件之间需要建立联系，这个联系就是数据通信

1. 分类
   - 父子组件通信
     - 理解： data选项为什么是一个函数？
       - 组件是一个聚合体，也是一个整体，它需要一个独立的作用空间，也就是它的数据需要是独立的，目前js的最大特点是函数式编程，而函数恰好提供了一个独立作用域，所以我们data在出根组件外都是函数
     - 理解： 为什么data函数需要返回一个返回值，返回值还是对象，不能是数组吗？
       - Vue通过es5的Object.definePerproty属性对一个对象进行getter和setter设置，而data选项是作为Vue深入响应式核心的选项
     - 过程
       - 父组件将自己的数据同 v-bind 绑定在 子组件身上
       - 子组件通过 props属性接收
             <template id="father">
                 <div>
                     <h3> 这里是father </h3>
                     <Son :money = "money"></Son>
                 </div>
             </template>
             
             <template id="son">
                 <div>
                     <h3> 这里是son </h3>
                     <p> 我收到了父亲给的 {{ money }} </p>
                 </div>
             </template>
             
             <script>
               Vue.component('Father',{
                 template: '#father',
                 data () {
                   return {
                     money: 10000
                   }
                 }
               })
             
               Vue.component('Son',{
                 template: '#son',
                 props: ['money']
               })
             
               new Vue({
                 el: '#app'
               })
             </script>
     - props属性数据验证
       - 验证数据类型
       - 验证数据大小【 判断条件 】
             // props: ['money']
             // 数据验证
             // props: {
             //   'money': Number 
             // }
             props: {
                 'money': {
                     validator ( val ) { // 验证函数
                         return val > 2000
                     }
                 }
             }
       - 工作中： 第三方验证
         - TypeScript   [ TS ] 
         - 插件   vue-validator  等
   - 子父组件通信
     - 是通过自定义事件    
       - 事件的发布  
         - 通过绑定元素身上实现
       - 事件的订阅
         - 通过this.$emit触发
           // html
           <div id="app">
               <Father></Father>
             </div>
             <template id="father">
               <div>
                 <h3> 这里是father </h3>
                 <p> 我现在有 {{ gk }} </p>
                 <Son @give = "fn"></Son>
               </div>
             </template>
           
             <template id="son">
               <div>
                 <h3> 这里是son </h3>
                 <button @click = "giveHongbao"> 给父亲红包 </button>
               </div>
             </template>
           //js
           Vue.component('Father',{
               template: '#father',
               data ( ) {
                 return {
                   gk: 0
                 }
               },
               methods: {
                 fn ( val ) {
                   this.gk = val 
                 }
               }
             })
           
             Vue.component('Son',{
               template: '#son',
               data () {
                 return {
                   money: 5000
                 }
               },
               methods: {
                 giveHongbao () {
                   this.$emit('give',this.money)
                 }
               }
             })
             new Vue({
               el: '#app'
             })
       
   - 非父子组件通信
     - ref链
     
     ![](https://ftp.bmp.ovh/imgs/2019/09/8a1837be1262fdfe.png)

     - bus事件总线
           var bus = new Vue()
           
             Vue.component('Father',{
               template: '#father'
             })
             Vue.component('Son',{
               template: '#son',
               data () {
                 return {
                   flag: false
                 }
               },
               mounted () { // 也是一个选项，表示组件挂载结束 ， 也就是说我们可以在View视图中看到这个组件了
                 // console.log( 'mounted ')
                 // bus.$on(自定义事件名称，执行的事件处理程序)
                 var _this = this
                 bus.$on('cry',function () {
                   _this.flag = true
                 })
           
               }
             })
             Vue.component('Girl',{
               template: '#girl',
               methods: {
                 kick () {
                   bus.$emit('cry')
                 }
               }
             })
             new Vue({
               el: '#app'
             })
   - 非常规手段进行组件通信 【 不推荐 】
     - 兵哥建议： 如果标准方案可以实现，就不要用非常规手段
     - 1. 可以实现子父通信
       - 父组件通过 v-bind 绑定一个方法给子组件
       - 子组件通过 props选项接收这个方法，然后直接调用
     - 2. 父组件 通过  v-bind 绑定 一个对象类型数据 给子组件 
       1. 子组件直接使用，如果更改这个数据，那么父组件数据也更改了
          - 原因： 同一个地址
          - 非常规在哪里？
            - 违背了单向数据流 
   - 多组件状态共享 【 vuex 】

4.1 slot

slot  插槽 

	比如： 插卡游戏机 

1. 分类
   - 普通插槽
   - 具名插槽
     - 给slot加一个name属性
           <slot name = "header"></slot>
   - 注意： 以上内容是 Vue 2.5.x的版本
   - Vue 2.6.x以上的版本将使用 v-slot指令来代替 2.5.x使用方式
2. v-slot指令
   - 作用： 
     - 可以将组件的数据在组件的内容中使用

4.2 过渡效果

Vue框架使用css3过渡效果或是js动画

Vue内部提供了一个叫做transition的过渡组件

使用transition包裹过渡元素，那么会自动添加 6 个类名  8个钩子函数 

- 默认 类名 开头 v
- 如果有name属性，那么使用 这个name属性值作为类名开头

1. 实现方式
   - 在 CSS 过渡和动画中自动应用 class 【 自己写 】
   - 可以配合使用第三方 CSS 动画库，如 Animate.css
   - 在过渡钩子函数中使用 JavaScript 直接操作 DOM 
   - 可以配合使用第三方 JavaScript 动画库，如 Velocity.js
2. 第一种 [ 在 CSS 过渡和动画中自动应用 class 【 自己写 】 ]
3. 第二种： animate.css 【 推荐 】
4. 第三种： Vue提供了8个javascript钩子，我们需要自定义js动画 
5. 第四种： 使用第三方插件： Velocity.js



4.3 自定义指令

业务： 页面开启时自动获得 search 按钮焦点

- 定义形式
  - 全局定义
        // Vue.directive('focus',{
          //   // 5个javascript钩子     5个中掌握2个
          //   bind (el,binding,vnode,oldVnode) { // 当自定义指令和元素绑定时触发
          //     console.log( 'bind', )
          //   },
          //   inserted ( el,binding,vnode,oldVnode ) { // 当自定义指令绑定的元素插入到页面时触发
          //     console.log( 'el',el) 
          //     console.log( 'binding',binding)
          //     console.log( 'vnode',vnode)
          //     console.log( 'oldVnode',oldVnode)
          //     console.log('inserted')
        
          //     if ( binding.modifiers.a ) {
          //       el.style.background = 'red'
          //     } else {
          //       el.style.background = 'blue'
          //     }
        
          //     el.value = binding.expression
        
          //     el.focus()
        
          //   },
          //   updated () { // 当自定义指令的元素或是他的子元素发生变化式触发
        
          //   },
          //   componentUpdate () { //当自定义指令的元素或是他的虚拟子节点 发生变化式触发
        
          //   },
          //   unbind () { // 当自定义指令绑定的元素解绑时触发
        
          //   }
          // })
    
  - 局部定义
         new Vue({
            el: '#app',
            directives: {
              'focus': {
                bind () {
        
                },
                inserted ( el ) {
                  el.focus()
                }
              }
            }
          })
    

4.4 过滤器

- 1. 作用
     - 是对已经有的数据做数据格式化 
- 2. 使用格式
     - 已有数据 | 过滤器名称(arg1,arg2)
           <img 
                :src = "item.img | imgFilter "
                onerror="this.style.visibility='hidden'"
                >
     - | 我们称之为 管道符
- 定义
  - 全局定义
    - Vue.filter
  - 局部定义
    - filters选项
      全局定义
      // Vue.filter('imgFilter',function ( val ) {
        //   //val就是需要格式化的数据
        //   // console.log('val',val)
        //   return val.replace( 'w.h', '128.180')
        // })
      
        new Vue({
          el: '#app',
          data: {
            lists: []
          },
          filters: { // 局部定义
            // 过滤器名称：function () {}
            'imgFilter': function ( val ) {
              return val.replace( 'w.h', '128.180')
            }
          },
          methods: {
            getList () {
              fetch('./data/list.json')
                .then( res => res.json())
                .then( data => {
                  this.lists = data.movieList
                })
                .catch( error => console.log( error ))
            }
          }
        })

4.5 作业

- 自定义指令实现选项卡
- 预习Vue生命周期 并 试着 实现 将 swiper 应用在 Vue中 
- 复习着一周内容
- todolist尝试实现
