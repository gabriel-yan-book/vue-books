# vuex
Vue高级 - Vuex

Vuex称为Vue的状态管理工具，也是多组件状态共享的工具

Vuex相当于是Vue的一个集中式的存储仓库

- 它存储的是数据 【 状态 】
- 存储仓库： 本地存储  cookie  数据库



  什么时候用： 打算开发中大型应用

  集中式数据管理, 一处修改，多处使用

  思维流程:
                                      store.js
              this.store.commit('increment')    -> mutations
              this.store.dispatch('jia')        -> actions            
                mapActions() ->actions                                mapGetters()->getters

          学生          代课老师         校长           财务      班主任             学生

 (view）component - dispatch >  action   ->  mutation ->    state  <- getter    <-    component
          发送请求      处理            修改状态         
                        业务逻辑        修改state                读取state
                        异步



案例： 比如我们报名考科一

- 取号
- 排队
- 窗口
- 走流程

为什么要集中式的车管所 ？

- 统一管理、集中管理
- 数据共享 

为什么要走流程？

- 控制执行 ， 比如 100000个人同时进入会怎么样？

理解为什么要使用vuex

- 它能够实现状态共享 
- 实现流程化，让项目的运行更加流程和优化

市场上出现了一个情况，不知道什么情况下使用vuex? 

- 中大型应用 
- 当你不确定你是否要使用vuex的时候，那就不要用了         flux作者说的

学习阶段： 必须要用

公司： 可用可不用 



1. 什么是状态

	我们使用一条数据去管理一个视图，那么这个数据我们就称之为 ‘状态’

2. vuex是做什么的？

	Vuex是一个集中式的存储管理中心，vuex中可以用来存储 数据（ 状态 ）

	vuex也是一个状态管理中心，它也可以进行状态的管理

3. 什么是状态管理模式？

	我们使用一条数据去管理一个视图，那么这种管理模式就称之为  状态管理 

4. 什么时候使用vuex

  中大型应用使用  （使用的时间）

  当你不确定你是否要使用vuex的时候，那就不要用了 

5. vuex的开发流程



看图说话： 

![](https://vuex.vuejs.org/vuex.png)

- Vuex的核心组成部分有三个，分别为： actions 、 state 、 mutations
  - actions表示动作的创建者，它的作用是创建动作，然后发送动作， 用于用户交互、后端数据交互
    - 距离： 一个用户点击了登陆按钮
  - mutations 表示动作的出发者，它的作用是用来修改数据 -更新视图
  - state它是数据的存储者，它的作用是定义数据【 状态 】
- 后端数据交互写在actions中
- vuex调试工具主要调试的mutations
- vuex是流程化执行的，符合单向数据流思维

vuex - 基础操作流程

1. 安装vuex 
   $ yarn add vuex
2. 在 src / store / index.js，【 数据不分块 】
       import Vuex from 'vuex'
       
       import Vue from 'vue'
       
       Vue.use( Vuex )
       
       // 1. 定义store 模块 
       
       // const store = new Vuex.Store( options )
       
       const store = new Vuex.Store({
         state：{
           count: 0 
         },
         actions：
           /* 
             1. actions是一个对象
             2. acitons里面放的都是方法
             3. 方法的作用是创建动作，发送动作
       
              increment ( store对象,组件发来的实际参数1，参数2 ) {}
           */
       
           increment ( { commit }, val ) {
             console.log('increment执行了')
             console.log('val', val )
             // 动作创建
             const action = {
               type: INCREMENT,
               val
             }
             // 发送动作
             commit( action )
           }
       
         },
         mutations：{
           /* 
             * 也是一个对象
             * 里面存放的也是方法
             * 方法名称是actions发送来的动作的类型
             * 接收两个参数
             *   state就是数据 ， action就是actions发来的动作
             * mutations作用是用来修改数据的
             * payload表示从组件传递过来的参数   负载
           */
           [ INCREMENT ] ( state,action ) {
             console.log('mutations执行了')
             console.log( 'state',state)
             console.log( 'action',action )
             //修改数据
             state.count ++
           }
         },
         getters: {}, //getters表示帮助 视图【 组件 】  获得store中的 state 
         modules // 用来实现数据分块的
           /*
           	数据分块： 
               	一个项目中是有多个功能或是页面的，比如有
               		home
               		分类
               		列表
               		详情
               		用户
               			普通用户
               			会员
               			超级会员
                       底部栏
                       头部栏
                       图表数据
                       
                   一个state管理所有的这些数据，就会变的杂乱，和不好维护，所以我们希望将数据分块，单一管理，一个数据一个模块
           */
       })
       
       // 2. 导出store模块
       
       export default store 
3. 在main.js中注入store
       import store from './store'
       new Vue({
         router, // 在项目中注入路由，让所有的子组件都用于路由属性 $route  $router
         store, // 在项目中注入store,让所有的子组件拥有一个属性  $store ， 用于和vuex进行通信
         render: h => h(App),
       }).$mount('#app')
       
4. 在组件内使用
       <template>
         <div>
           vuex - 基础
       
           <hr/>
       
           <button @click = "add"> + </button>
           <p> {{ $store.state.count }} </p>
         </div>
       </template>
       
       <script>
       export default {
         methods: {
           add () {
             // 执行actions中的increment方法
             // this.$store.dispatch( actions中方法的名称 )
             this.$store.dispatch('increment',100)
           }
         },
         created () {
           console.log( this.$store )
         }
       }
       </script>
       

vuex操作流程 - 【 数据分块 】

1. 安装vuex
   $ yarn add vuex
2. 在 src / store /index.js
       import Vuex from 'vuex'
       import Vue from 'vue'
       import * as todos from '../pages/vuex_basic/store'
       
       Vue.use( Vuex )
       
       
       const store = new Vuex.Store({
         modules: {
           //每一个分块出去的数据包
           vue_basic: {
             state: todos.state,
             actions: todos.actions,
             mutations: todos.mutations,
             getters: todos.getters
           }
         }
       })
       
       export default store 
3. 在main.js中注册
       import store from './store'
       new Vue({
         store, // 在项目中注入store,让所有的子组件拥有一个属性  $store ， 用于和vuex进行通信
         render: h => h(App),
       }).$mount('#app')
       
4. 在 vue_basic/store.js中打造 state  actions  mutations  getters
       /* 
         核心组成部分是三个   +  getters 
       
         store 导出的不止一个
       
       */
       import axios from 'axios'
       const ADD_TODOS = 'addTodos'
       const GET_CATEGORY = 'getCategory'
       
       
       export const state = {
         todos: [
           {
             id: 1,
             task: '任务一'
           },
           {
             id: 2,
             task: '任务二'
           }
         ],
         category: null
       }
       
       export const actions = {
         addTodos ({ commit }, val ) {
           const action = {
             type: ADD_TODOS,
             val
           }
           commit( action )
         },
         getCategory ( {commit} ) {
           axios({
             url: '/index.php',
             params: {
               r: 'class/category',
               type: 1
             }
           }).then( res => {
             
             // 动作创建
       
             const action = {
               type: GET_CATEGORY,
               payload: res.data.data.data
             }
       
             commit( action )
       
           }).catch( error => console.log( error ))
         }
       }
       
       export const mutations = {
         [ ADD_TODOS ] ( state,action ) {
           state.todos.push({
             id: state.todos.length + 1,
             task: action.val
           })
         },
         [ GET_CATEGORY ] ( state,action ) {
           state.category = action.payload
         }
       }
       
       export const getters = {
         getTodos ( state ) {
           return state.todos 
         } 
       }
       
5. 在 vue_basic/index.vue使用
       <template>
         <div>
           <h3> vuex - 数据分块 -  todolist增加功能 </h3>
           <input type="text" v-model = "val" @keyup.enter="add">
           <ul>
             <li v-for = "item in todos" :key = "item.id">
               {{ item.task }}
             </li>
           </ul>
       
           <button @click = "getCategory"> 点击获取数据 </button>
       
           <ul>
             <li v-for ='item in category' :key = "item.cid">
               {{ item.name }}
             </li>
           </ul>
       
         </div>
       </template>
       
       <script>
       import { mapState,mapActions } from 'vuex'
       export default {
         data () {
           return {
             val: ''
           }
         },
         methods: {
           ...mapActions(['addTodos','getCategory']), // 容易忘记
           add () {
             this.addTodos( this.val )
             this.val = ''
           }
         },
         computed: {
           ...mapState({
             todos: state => state.vue_basic.todos, // 这里是一个箭头函数来取数据
             category: state => state.vue_basic.category
           })
         }
       }
       </script>
       
   

四个方案： 

    1. 前： 标准    后： 标准  √
    
    2. 前： 标准    后： 非标准  √
    
    3. 前:  非标准  后： 非标准  
    
    4. 前： 非标准  后： 标准  √
    
    component ---dispatch---> actions ---commit--->mutations---state <----getters----component

思考： 

1. 数据的获取无论是标准还是非标准，都是很麻烦的，并且有些有些违背关注点分离
2. actions或是mutations的通信会出现多个  this.store.dispatch / this.store.commit

解决方案： 、

	使用vuex辅助工具

6. 辅助工具

mapActions   

mapMutations 

mapGetters 

mapState

export default 默认导出一个

export  叫批量导出，可以导出多个



7. vuex数据分块

将来我们的数据

	希望是分块管理的，这样方便我们将来为何和更新

vue是通过一个叫做 module  的模块来管理的

	vue项目中 store下的一个目录就是一个数据包

	案例： todolist的添加操作

作业： 

1. 使用vuex实现计数
2. 使用vuex实现todolist中添加（ 使用数据分块  module ）
