# Vue基础二
2.1 模板语法

mustache 语法中是支持写js的

1. 用法：
   - 内容： 必须加 {{ js语法 }}
   - 属性:  属性中属性值可以直接写js语法，并且属性值中的数据相当于全局变量
     - 给一个标签加一个自定义属性/已有属性
           img中的src就是已有属性
           <img src = "" /> 
           
           //data-index就是自定义属性 ， web网页中建议我们使用data-形式来定义自定义属性
           <img data-index = "0" />
           
     - 思考： Vue现在想要在html中使用自己的属性，并且要和他的语法和数据结合？
       - 咋整？
       - 分析： 如何我能够标识出哪一个属性是具有vue标志的那就好了,也就是属性前加 v
         - Vue给这种带v标识的属性，起了一个名字： 指令【 借鉴angular 】
             <div v-html = "msg">
                 
             </div>
         
2. 研究它js的支持性
   - 数据类型
     - 市场上js的数据类型分类有两种？
       - 第一种
         - 初始数据类型： number  string null undefine boolean
         - 引用数据类型: Object  [ function array ... ]
       - 第二种
         - 基础数据类型: number string boolean
         - 特殊数据类型： null undefine
         - 复杂数据类型； Object [ function array ...]
   - 输出语法
     - console
     - alert
   - 表达式 / 运算符
     - 三元表达式
3. 总结; 
   - null 和 undefined 是不会显示的，其他数据类型都是支持的，可以显示的
   - 挂载在window身上的全局属性，我们都不能用的： 比如; console  alert 
   - mustache语法中 不写流程控制 
     - for
     - if
     - while
     - do...while
   - mustache语法中支持三元表达式，同样也支持运算符 
     - 短路原则也是支持的

2.2 指令

指令的目的是做什么： 操作DOM

    解释 : MVVM    vm -> v    数据驱动

    所以： 今天开始，我们不想二阶段一样操作dom，改成操作数据，数据要想操控DOM,那么我们需要依赖指令，因为指令是直接绑定在dom身上的

1. v-html   转义输出，也就是可以解析 xml 数据
2. v-text： 非转义输出，也就是无法解析  xml 类型数据
3. v-bind 
   - 将数据和属性进行单向数据绑定： 将vue中数据赋值给属性值
         <img v-bind:src = "src" />
         <div v-bind:class = "">
             
         </div>
         <div v-bind:style = "">
             
         </div>
   - 简写形式
         <img v-bind:src="src" alt="">
         <img :src="src" alt="">
   - 类名绑定
     - 用法
       - 对象形式用法
                 <p :class = "{ bg: true,size: true }"></p>
                 <p :class = "{ bg: true,size: false }"></p>
                 <p :class = "{ [classA]: true,[classB]: true }"></p>
       - 数组形式用法
                 <p :class = "[ 'size','bg' ]"></p>
                 <p :class = "[ classA,classB ]"></p>
                 <p :class = "[ classA,classB,5>3?'a':'b']">  </p>
         
   - 样式绑定
     - 用法
       - 对象形式用法
             <p :style = "{width: '100px',height: '100px',background: 'yellow'}"></p>
             <p :style = "styleObj"></p>
       - 数组形式用法
             <p :style = "[{width:'100px',height: '100px'},{ background: 'green'}]"></p>
             <p :style = "[size,bg]"></p>

2.3 条件渲染

1. v-if 
2. v-else-if
3. v-else
4. v-show 条件展示
        <h3> 条件渲染 - 单路分支 </h3>
       <p v-if = "flag"> A </p>
       
       <h3> 条件渲染 - 双路分支 </h3>
       <p v-if = "flag"> A </p>
       <p v-else > B </p>
       
       <h3> 条件渲染 - 多路分支 </h3>
       <p v-if = "type === '美食'"> 美食 </p>
       <p v-else-if = " type === '游戏' "> 游戏 </p>
       <p v-else> 睡觉 </p>
       
       <h3> 条件展示 </h3>
       
       <p v-show = " showFlag "> 条件展示 </p>
5. 思考总结
       思考： v-if  vs  v-show  
             1. 效果看起来一样
             2. why Vue要出两个相似的指令？
               v-if控制的是元素的存在与否
               v-show控制的是元素的display:none属性
       
       思考？ 如果出事条件为假时？ v-if   v-show 谁的性能损耗较高？
       v-show
       
       总结： 项目中如何选择哪一个？
       频繁切换用  v-show
       如果不是很频繁的切换，那我们用 v-if   

2.4 列表渲染

- v-for 指令
      <h3> 数组 </h3>
      <ul>
          <li v-for = "(item,index) in arr" :key = " index ">
              {{ item }} -- index{{ index }}
          </li>
      </ul>
      <h3> 对象 </h3>
      <ul>
          <li v-for = "(item,key,index) of obj" :key = "index">
              {{ item }} -- {{ key }} -- {{ index }}
          </li>
      </ul>
      <h3> json </h3>
      <ul>
          <li v-for = "item in json" :key = "item.id">
              <span> 商品名称： {{ item.shop_name }} </span>
              <span> 商品价格： {{ item.price }} </span>
          </li>
      </ul>
      
      <h3> 循环嵌套 </h3>
      
      <ul>
          <li v-for = "item in lists" :key = "item.id">
              <h3>  商品类型： {{ item.shop_name }} </h3>
              <ul>
                  <li v-for = "item in item.type" :key = "item.id">
                      <p> 制造商： {{ item.maker }} </p>
                  </li>
                  <!-- <li v-for = "ele in item.type" :key = "ele.id">
      <p> 制造商： {{ ele.maker }} </p>
      </li> -->
              </ul>
          </li>
      </ul>
      
      <h3> 循环number / string  </h3>
      
      <p v-for = "item in 10"> {{ item }} </p>
      <p v-for = "item in 'abc'"> {{ item }} </p>
- 总结： 
  - 1. 列表渲染参数可以写三个，分别为  item  key  index 
  - 2. 列表渲染，要在渲染的元素身上加一个key，作为这个元素唯一的标识 ,
       1. 思考: 这是为什么？
       2. 这个key最好是id,因为id唯一？思考： 为什么不能是index
  - 3. 循环嵌套式，参数名称是可以一致的
  - 4. in / of 都可以使用

2.5 事件处理器

- v-on
- key的重要性
  给列表渲染的每一层Vdom添加一个唯一标识，以便diff算法进行同层级比较
  扩展： 理解key

2.6 表单控件绑定

- v-model
  - 双向数据绑定
    - VM 改变   V随之改变
    - V改变， VM也随之改变
  - v-model只用于表单
    - 理由： v-model默认绑定value属性
  - 技巧： 看到表单直接 v-model

2.7 作业

1. 整理好笔记，发送博客
2. 用深入响应式原理来解释  双向数据绑定原理 【 思考 】
3. 使用单向数据绑定来实现  v-model 效果 
4. 今天讲的知识点在官网上对一对，看一看，
