# Vue基础五

5.1 todolist案例

- sui - ui库 + Vue + OOCSS

5.2 虚拟DOM & DIff算法

- 掌握程度： 了解	
- 案例
  - 操作真实DOM越少越好，尽量的去操作数据
  - 所以总结出来虚拟dom，
  - 所以Vue利用VDOM的对象模型来模拟DOM结构
  - 但是当一个页面很复杂式，DOM结构的模拟就变的很复杂了，所以Vue使用了一个新的语法糖，叫做JSX
- jsx
  - javascript  +  xml         让我们可以在js中书写dom结构
          <template id="mask">
            <div class="mask-box">
              <div class="mask-bg"></div>
              <div class="mask-content">
                <p> 您确定要离开吗？ </p>
                <button 
                  class="button button-warning button-fill pull-right"
                  @click = "removeItem( activeIndex )"
                > 确定 </button>
              </div>
            </div>
          </template>
- render 
  - （ createElement => VNode ）
  - 将 jsx  通过  render  方法解析为对象模型
- 流程
  - 第一次时
  - template模板使用jsx语法进行编辑
  - 通过render函数将jsx解析为 vdom 对象模型 
  - 将VDOM对象模型渲染为真实DOM,然后挂载到页面中
  - 当我们的数据发生改变时
  - 重新生成VDOM 
    
  ![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568553990161&di=32a264fbef5e3548cc0551bafd0ca522&imgtype=0&src=http%3A%2F%2Fwww.th7.cn%2Fd%2Ffile%2Fp%2F2017%2F12%2F20%2F23adbd3623c9bb54c385f62fc93c9225.jpg)

  - 总结： 
    - 1. 为什么Vue中要使用VDOM?
    - 2. VDOM为什么可以优化Vue ？
    - 3. VDOM渲染流程
    - 4. JSX语法
    - 5. render函数

5.3 生命周期 [ 王者 ]

掌握程度

1. 会写
2. 会念
3. 明白和了解每一个钩子函数的作用和意义

特别注意： 

	钩子函数不要写成箭头函数，箭头函数可能会改变this指向

- 理解： 为什么要有生命周期 ？
- Vue为了在一个组件的从创建到销毁的一系列过程中添加一些功能，方便我们更好的去控制组件  
- 类比： 人
  - 出生     -         哭
  - 小学      -        小学
  - 中学 
  - 高中
  - 大学   /   专科
  - 工作
  - 。。。
- 生命周期图示
  
![](http://www.ailbk.com/uploads/allimg/180928/1-1P92R11500602.jpg)

- Vue的生命周期分为三个阶段，8个钩子函数
  - 初始化
    自动执行
    研究方向
    	数据
    	真实DOM
    - beforeCreate 
      组件创建前
      - 作用： 为整个生命周期做准备工作，初始化事件和自身或是子组件的生命周期做准备
      - 类比： 父母为子女的相亲做准备 
      - 意义： 
        - 数据拿不到
        - 真实dom拿不到
      - 项目中
        - 不常用
    - created
      组件创建结束
      - 作用： 初始化注入其他选项 和 激活 选项
      - 类比： 我们本人知道了父母准备给我们相亲这件事，父母通知你了
      - 意义：
        - 数据可以拿到
        - 真实dom拿不到
      - 项目中：
        - 数据请求，也可以修改一次数据
    - beforeMount
      组件挂载前
      	挂载： 组件插入到页面
      - 类比：两人初步联系
      - 意义： 
        - 数据可以拿到
        - 真实dom没有拿到
      - 在项目中
        - 数据请求，数据修改 
        - 建议不要去使用它了，让它完成内部事务，不给加负担
      - 内部完成事务
        - 判断el选项 -  new Vue   是否有 el 
          - 有，继而判断是否有template选项，证明有子组件
            - 有template,那么我们通过render函数将jsx解析为VDOM对象模型 
            - 无template,那么我们需要通过outerHTML手动渲染
              - outerHTML，元素外进行渲染，并且会代替原来的容器盒子，并且template在实例内会被解析，将来不会渲染到页面
          - 无： 那么我们需要手动挂载： new Vue().$mount('#app')
    - mounted
      组件挂载结束，也就是已经插入到页面中了
      - 类比： 两人约见面 【 奔现 】
      - 意义： 
        - 数据可以拿到
        - 真实DOM也可以拿到
      - 项目中
        - 数据修改，数据请求
        - 真实DOM操作 【 不推荐 】
          - 理由： 我们要用数据驱动视图 
          - 应该做的： 第三方实例化 【 静态数据 】
  - 运行中
    运行中触发条件是： 数据改变
    	只要数据改变，就会触发这两个钩子函数
    - beforeUpdate
      组件更新前
      - 类比： 奔现失败后，再一次在一次进行
      - 意义： 
        - 数据是可以拿到更新后的数据
        - 也可以拿到更新后的真实DOM 
        - 兵哥解析： 
          - 为在一次的更新准准备工作
          - 生成Virtual DOM 
      - 项目中
        - 不常用
    - updated
      组件更新结束
      - 类比： 两人看对眼了 / 两人看不对眼 
        - 看对眼： 相亲这件事就有一个结果
        - 看不对眼： 相亲这件事继续
      - 意义： 
        - 可以拿到修改后的数据
        - 也可以拿到真实DOM
      - 在项目中：
        - 真实DOM操作 【 不推荐 】
        - 第三方库动态实例化 【 动态数据 】
      - 内部
        - VDOM重新渲染，然后通过render函数解析为VDOM对象模型，在通过Diff进行比对生成patch补丁对象，然后重新渲染为真实DOM
          - 只改变变化的部分，其他部分不改变
  - 销毁
    触发条件：组件被删除
    - 外部开关销毁
    - 内部调用$destroy()
    这两个钩子函数没有太大区别，所以我们统一说
    - beforeDestroy
    - destroyed
    - 外部销毁
      - 通过开关完成
        - DOM被删除了，组件也被删除了
    - 内部销毁
      - 通过调用$destroy()来完成
        - DOM没有被删除，但是组件被删除了
        - Dom需要手动删除
    - 项目中如何使用： 
      - 善后
        - 比如： 计时器，比如滚动事件等 
  5.4 生命周期案例
  - Swiper

    - swiper是一个实现滑动操作的一个第三方库，目前最好的滑动操作库

    ![](http://www.jqhtml.com/wp-content/uploads/2017/10/swiper1017-17.gif)

    - 静态数据
    - 动态数据
      - updated中写式，会有重复实例化问题
        - 第一个解决方案： 加判断条件
        - 第二个解决方案： setTimout
          - 放在主线程后执行，异步队列中，保证真实DOM渲染完成
        - 第三种解决方案： 推荐    Vue内部提供的  nextTick
          - nextTick表示真实DOM渲染完成之后才执行
            - Vue.nextTick( callback )
            - this.$nextTick( callback )
  



作业： 

1. todolist
2. VDOM & Diff

