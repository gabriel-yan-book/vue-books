# Vue基础一
1.0 前端开发流程规范

-  [ 前端开发流程规范 ](https://blog.csdn.net/weixin_41640944/article/details/89462053)

1.1 前端框架发展历史

    html
    
      html [1990]----> html5 [2008.1.12]
    
    css
    
      css 1.0 1996 
      css 2.0 1998
      css 3.0 2001
    
    EcmaScript 
    
      1997年诞生
      2015  EcmaScript 2015
      2016  EcmaScript 2016          dart语言  vs  javascript
    
    随着前端项目的逻辑越来越复杂和难以维护，那么前端这边引进了后端的架构思想（ MV* ）
    
        M  Model      数据层
        V  View       视图层
        C  Controller 控制器 ( 业务逻辑 )        MVC
     	P  Presenter  提出者（ Controller 改名得来的 ） MVP
        VM ViewModel  视图模型（ 业务逻辑  VM 是 由  P 改名得来的） MVVM
       
     
    
        Backbone.js  MVP    2010.10
    
        Angular.js( 1.0 )   MVC    2010.10
    
        Angular.ts ( 2.0 )  MVC -> MVVM 2016 目前已经更新到了 Angular9 ( 也属于angular2.0 版本 )
    
        Vue 1.0   MVVVM  2014/07
    
        Vue 2.0   MVVM   2016/09
    
        React 2012 不太认可前端MVC这种架构思想， 你可以将React单纯看做是MVC中V
    
        github统计量 （ 国际使用量 ）不代表大陆地区       单位是： K
    
        angular.js   angular.ts       vue             React  
    
          59.6          49.1          146              134	
    
        学习难度： Vue < React < Angular( 2.0 )
    
        前端流行
    
          移动  web    &&  hybird app( 混合app )
    
          app
            1. native app ( 安卓  ios  java ME)
            2. webapp ( 应用在浏览器中的app )
            3. Hybird app ( 混合app ) 
               1. webapp 嵌入 第三方原生应用库（ 可以访问原生设备（手机） 的接口权限，比如：照相机 ）
    
    	2016年： 	
    		   1. es6
    		   2. vue2.0
    		   3. angular2.0x
               1. 微信小程序 /  微信小游戏
    
总结表：

![](https://ftp.bmp.ovh/imgs/2019/09/5a0e75fe7e6c8d16.png)

- 前端js框架到底在干嘛!   为什么要用？
  - js框架帮助开发者写js逻辑代码，在开发应用的时候js的功能划分为如下几点：
    1. 渲染数据
       ![](https://ftp.bmp.ovh/imgs/2019/09/41a7e07a92925028.png)
    2. 操作DOM 
       ![](https://ftp.bmp.ovh/imgs/2019/09/b00c66d4e579a1cf.gif)
    3. 操作cookie等存储机制api
       ![](https://ftp.bmp.ovh/imgs/2019/09/6430c0c8a003f0ad.png)
  - 在前端开发中
    - 难题: 如何高效的操作dom、渲染数据是一个前端工程师需要考虑的问题，而且当数据量大，流向较乱的时候，如何正确使用数据，操作数据也是一个问题？？？
      - 解决: 
        - 而js框架对上述的几个问题都有自己趋于完美的解决方案，
        - 开发成本降低。高性能高效率。
        - 唯一的缺点就是需要使用一定的成本来学习。
  

1.2 初始Vue.js

- 官网地址： 英文官网     中文官网
- Vue.js框架项目介绍
  - 作者： 尤雨溪
  
    ![](https://ftp.bmp.ovh/imgs/2019/09/907e8e38dfbe41d0.jpg)
  - Vue.js是尤雨溪的个人项目
  - Vue.js也是一个MVVM框架
  - Vue.js它是一个单项数据流的框架
  - Vue.js是一个Js渐进式框架
    - 渐进式： 越学越难
  - 学习Vue的必要性
  
    <table>
      <tr>
        <td bgcolor = "#7FFFD4">
        	Vue近几年来特别的受关注，三年前的时候angularJS霸占前端JS框架市场很长时间，
          接着react框架横空出世，因为它有一个特性是虚拟DOM，从性能上碾轧angularJS，
          这个时候，vue1.0悄悄的问世了，它的优雅，轻便也吸引了一部分用户，开始收到关注，
          16年中旬，VUE2.0问世，这个时候vue不管从性能上，还是从成本上都隐隐超过了react，火的一塌糊涂
        
        	学习vue是现在前端开发者必须的一个技能
        </td>
      </tr>
    </table>

1.3 MV*模式介绍

[MV*模式图示](http://www.ruanyifeng.com/blog/2015/02/mvcmvp_mvvm.html)

1.4 Vue实现数据绑定的原理

1. 书写第一个Vue案例
2. Vue深入响应式原理图
   ![](https://ftp.bmp.ovh/imgs/2019/09/d4c2de951720c006.png)
        // Vue 底层原理 
       
         // 目的： 使用原生js来实现Vue深入响应式 
       
         var box = document.querySelector('.box')
       
         var button = document.querySelector('button')
       
         var data = {
           name: 'Jick'
         }
       
         // 观察者对象
       
         var observer = {...data} 
       
         // es5提供的api方法,这个方法不兼容ie8以及以下
         // Object.defineProperty(对象，对象的属性，对象属性的修饰符 )
         Object.defineProperty(  data,'name',{
           // get/set  统称为: '存储器'
           get () {
             return  observer.name // 初始化赋值一个值给name属性
           },
           set ( val ) {
             console.log( val )
             box.innerHTML = val
           }
         })
       
         button.onclick = function () {
           data.name = "Rose"
         }
       
         box.innerHTML = data.name 
   - 面试题/理解： 如何理解深入响应式原理？
     - Vue是通过数据劫持和事件的订阅发布来实现的，数据劫持指的是Vue通过observer观察者对象对data选项中的数据进行getter和setter设置【 Object.defineProperty 】，事件的订阅发布指的是Vue通过事件来监听，通知Vue进行视图更新
       - 监听： 选项/watch
