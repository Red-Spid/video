# vue核心知识点

## 1、对于Vue是一套渐进式框架的理解

渐进式代表的含义是：没有多做职责之外的事。

`vue.js`只提供了`vue-cli`生态中最核心的`组件系统`和`双向数据绑定`。

像`vuex`、`vue-router`都属于围绕`vue.js`开发的库。

> 比如说，你要使用Angular，必须接受以下东西：

- 必须使用它的模块机制
- 必须使用它的依赖注入
- 必须使用它的特殊形式定义组件（这一点每个视图框架都有，难以避免）

所以Angular是带有比较强的排它性的，如果你的应用不是从头开始，而是要不断考虑是否跟其他东西集成，这些主张会带来一些困扰。

> 比如说，你要使用React，你必须理解：

- 函数式编程的理念，
- 需要知道什么是副作用，
- 什么是纯函数，
- 如何隔离副作用
- 它的侵入性看似没有Angular那么强，主要因为它是软性侵入。

> Vue与React、Angular的不同是，但它是`渐进的`：

- - 你可以在原有大系统的上面，把一两个组件改用它实现，当jQuery用；
  - 也可以整个用它全家桶开发，当Angular用；
  - 还可以用它的视图，搭配你自己设计的整个下层用。
  - 你可以在底层数据逻辑的地方用OO和设计模式的那套理念，
  - 也可以函数式，都可以，它只是个轻量视图而已，只做了最核心的东西。

专注web前端开发（jascript,vue,react,webpack,nodes等）

## 2、vue.js的两个核心是什么？

+ 数据驱动：
  + 在vue中，数据的改变会驱动视图的自动更新。传统的做法是需要手动改变DOM来使得视图更新，而vue只需要改变数据。
+ 组件
  + 组件化开发，优点很多，可以很好的降低数据之间的耦合度。将常用的代码封装成组件之后（[vue组件封装方法](https://blog.csdn.net/joyvonlee/article/details/92020547)），就能高度的复用，提高代码的可重用性。一个页面/模块可以由多个组件所组成。

## 3、请问 `v-if` 和 `v-show` 有什么区别

+ v-if：当隐藏结构时该结构会直接从整个dom树中移除；

+ v-show：当隐藏结构时是在该结构的style中加上display:none，结构依然保留。
+ 当组件中某块内容只会显示或隐藏不会被再次改变显示状态，此时用v-if更加合适，例如请求后台接口通过后台数据控制某块内容是否显示或隐藏，且这个数据在当前页不会被修改；
+ 当组件某块内容显示隐藏是可变化的，此时用v-show更加合理，例如页面中有一个toggle按钮,点击按钮来控制某块区域的显示隐藏。
+ 为什么这么说呢？大家都知道频繁操作dom对性能影响很大，v-if如果用在有toggle按钮控制的情况下，相当于在频繁增加dom和删除dom，所以此时用v-show更加合适
+ 如果只是要么显示要么隐藏之后不会再改变显示隐藏状态的情况，v-if更加的合理，因为如果默认就是隐藏，相当于dom一次都不用创建，如果默认是显示，v-if和v-show效果完全一样。

## 4、vue常用的修饰符

+ .lazy

  + 在默认情况下，`v-model` 在每次 `input` 事件触发后将输入框的值与数据进行同步 。你可以添加 `lazy` 修饰符，从而转变为使用 `change` 事件进行同步：

    ```
    <!-- 在“change”时而非“input”时更新 -->
    <input v-model.lazy="msg" >
    ```

+ .number

  + 如果想自动将用户的输入值转为数值类型，可以给 `v-model` 添加 `number` 修饰符

    ```
    <input v-model.number="age" type="number">
    ```

+ ### .trim

  如果要自动过滤用户输入的首尾空白字符，可以给 `v-model` 添加 `trim` 修饰符：

  ```
  <div id='other'>
          <input v-model.trim='trim'>
          <p ref='tr'>{{trim}}</p>
          <button @click='getStr'>获取</button>
  </div>
  ```

+ ### 事件修饰符


  在事件处理程序中调用 `event.preventDefault()` 或 `event.stopPropagation()` 是非常常见的需求。尽管我们可以在方法中轻松实现这点，但更好的方式是：方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节。

  为了解决这个问题，Vue.js 为 `v-on` 提供了事件修饰符。之前提过，修饰符是由点开头的指令后缀来表示的。

  ```
  <!-- 阻止单击事件继续传播/阻止冒泡 -->
  <a v-on:click.stop="doThis"></a>
  
  <!-- 提交事件不再重载页面 -->
  <form v-on:submit.prevent="onSubmit"></form>
  
  <!-- 修饰符可以串联 -->
  <a v-on:click.stop.prevent="doThat"></a>
  
  <!-- 只有修饰符 -->
  <form v-on:submit.prevent></form>
  
  <!-- 添加事件监听器时使用事件捕获模式 -->
  <!-- 即元素自身触发的事件先在此处处理，然后才交由内部元素进行处理 -->
  //假设有个div1,2,3,4,5  div3.capture  如果点击div4首先执行div3再执行div4,其他从内向外执行
  div3 --- div4 --- div5 --- div2 --- div1
  <div v-on:click.capture="doThis">...</div>
  
  <!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
  <!-- 即事件不是从内部元素触发的 -->
  <div v-on:click.self="doThat">...</div>
  <!-- 点击事件将只会触发一次 -->
  <a v-on:click.once="doThis"></a>
  
  注意：
  使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 `v-on:click.prevent.self` 会阻止所有的点击，而 `v-on:click.self.prevent` 只会阻止对元素自身的点击。
  ```

+ ### 按键修饰符

  在监听键盘事件时，我们经常需要检查常见的键值。Vue 允许为 `v-on` 在监听键盘事件时添加按键修饰符：

  ```
  <!-- 只有在 `keyCode` 是 13 时调用 `vm.submit()` -->
  <input v-on:keyup.13="submit">
  ```

  记住所有的 `keyCode` 比较困难，所以 Vue 为最常用的按键提供了别名：

  ```
  <!-- 同上 -->
  <input v-on:keyup.enter="submit">
  
  <!-- 缩写语法 -->
  <input @keyup.enter="submit">
  ```

  全部的按键别名：

  - `.enter`
  - `.tab`
  - `.delete` (捕获“删除”和“退格”键)
  - `.esc`
  - `.space`
  - `.up`
  - `.down`
  - `.left`
  - `.right`

  ### 系统修饰键

  可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。

  - `.ctrl`
  - `.alt`
  - `.shift`
  - `.meta`

  注意：在 Mac 系统键盘上，meta 对应 command 键 (⌘)。在 Windows 系统键盘 meta 对应 Windows 徽标键 (⊞)。在 Sun 操作系统键盘上，meta 对应实心宝石键 (◆)。在其他特定键盘上，尤其在 MIT 和 Lisp 机器的键盘、以及其后继产品，比如 Knight 键盘、space-cadet 键盘，meta 被标记为“META”。在 Symbolics 键盘上，meta 被标记为“META”或者“Meta”。

  例如：

  ```
  <!-- Alt + C -->
  <input @keyup.alt.67="clear">
  
  <!-- Ctrl + Click -->
  <div @click.ctrl="doSomething">Do something</div>
  ```

  注意：

  请注意修饰键与常规按键不同，在和 `keyup` 事件一起用时，事件触发时修饰键必须处于按下状态。换句话说，只有在按住 `ctrl` 的情况下释放其它按键，才能触发 `keyup.ctrl`。而单单释放 `ctrl` 也不会触发事件。如果你想要这样的行为，请为 `ctrl` 换用 `keyCode`：`keyup.17。`

## 5、v-on可以监听多个方法吗？

+ 可以

+ v-on绑定多个方法

  ```
  <p v-on="{click:dbClick,mousemove:MouseClick}"></p>
  ```

+ 一个事件绑定多个函数:

  	<p @click="one(),two()"></p>

+ ```javascript
  <template>
  <div class="about">
  
  <button @click="myclick('hello','world','你好世界',$event)">点我text</button>
  
  <!-- v-on在vue2.x中测试,以下两种均可-->
  
  
  
  <button v-on="{mouseenter: onEnter,mouseleave: onLeave}">鼠标进来1</button>
  
  
  
  <button @mouseenter="onEnter" @mouseleave="onLeave">鼠标进来2</button>
  
  
  
   
  
  
  
  <!-- 一个事件绑定多个函数，按顺序执行，这里分隔函数可以用逗号也可以用分号-->
  
  
  
  <button @click="a(),b()">点我ab</button>
  
  
  
  <button @click="one()">点我onetwothree</button>
  
  
  
   
  
  
  
  <!-- v-on修饰符 .stop .prevent .capture .self 以及指定按键.{keyCode|keyAlias} -->
  
  
  
  <!-- 这里的.stop 和 .prevent也可以通过传入&event进行操作 -->
  
  
  
  <!-- 全部按键别名有：enter tab delete esc space up down left right -->
  
  
  
  <form @keyup.delete="onKeyup" @submit.prevent="onSubmit">
  
  
  
  <input type="text" placeholder="在这里按delete">
  
  
  
  <button type="submit">点我提交</button>
  
  
  
  </form>
  
  
  
  </div>
  
  
  
  </template>
  
  
  
  <script>
  
  
  
  export default {
  
  
  
  methods: {
  
  
  
  //这里是es6对象里函数写法
  
  
  
  a() {
  
  　　console.log("a");
  
  },
  
  
  b() {
  
  　　console.log("b");
  
  },
  
  
  
  one() {
  
  
  
  　　console.log("one");
  
  
  
  　　this.two();
  
  
  
  　　this.three();
  
  
  
  },
  
  
  
  two() {
  
  　　console.log("two");
  
  },
  
  three() {
  　　console.log("three");
  
  },
  
  myclick(msg1, msg2, msg3, event) {
  　　console.log(msg1 + msg2 + "--" + msg3);
  　　console.log(event);
  
  },
  
  onKeyup() {
  　　console.log("you press 'delete'");
  
  },
  
  onSubmit() {
  
  　　console.log("sumited");
  
  
  },
  
  onEnter() {
  　　console.log("mouse enter");
  
  },
  
  onLeave() {
  　　console.log("mouse leave");
  
  }
  },
  };
  </script>
  ```

  　　

## 6、vue中 `key` 值的作用

+ key值：用于 管理可复用的元素。因为Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。

- 两个相同的组件产生类似的DOM结构，不同的组件产生不同的DOM结构。
- 同一层级的一组节点，他们可以通过唯一的id进行区分。
- key的作用主要是为了高效的更新虚拟DOM。

## 7、vue-cli工程升级vue版本 ???

+ 先把项目拷贝一份,以防万一
+ 例如要升级为cli3的版本,先创一个cli3的空项目,然后在图形化界面把2所需要的配置下载下来
+ 最后把2的项目移到3里,运行看好不好使

+ // 在项目目录里运行npm upgrade vue vue-template-compiler , 不出意外的话，可以正常运行和 build。如果有任何问题，删除 node_modules 文件夹然后重新运行npm i即可。

## 8、vue事件中如何使用event对象？

1. 使用不带圆括号的形式，event 对象将被自动当做实参传入；
2. 使用带圆括号的形式，我们需要使用 $event 变量显式传入 event 对象。

## 9、$nextTick的使用

Vue.nextTick([callback,context])

+ 参数 :
  + {function} [callback]
  + {Object} [context]
+ 用法 :
  + 在下次DOM更新循环结束之后执行延迟回调.在修改数据之后立即使用这个方法,获取更新后的DOM

## 10、Vue 组件中 data 为什么必须是函数

因为如果data是一个函数的话,这样每复用一次组件,就会返回一份新的data,类似于给每个组件实例创建一个私有的数据空间,让各个组件实例维护各自的数据,而单纯的写成对象形式,就使得所有组件实例共用了一份data,就会造成一个变了全都会变的结果,所以所vue组件的data必须是函数,这都是因为js的特性带来的,跟vue本身设计无关

## 11、v-for 与 v-if 的优先级

v-for比v-if具有更高的优先级

## 12、vue中子组件调用父组件的方法

1. 通过this.$parent.event来调用
2. 子组件用$emit向父组件触发一个事件,父组件监听这个事件就行了
3. 父组件把方法传入子组件中，在子组件里使用props直接调用这个方法

## 13、vue中 `keep-alive` 组件的作用

keep-alive是vue内置的一个组件，而这个组件的作用就是能够缓存不活动的组件，我们能够知道，一般情况下，组件进行切换的时候，默认会进行销毁，如果有需求，某个组件切换后不进行销毁，而是保存之前的状态，那么就可以利用keep-alive来实现

在keep-alive上有两个属性

字符串或正则表达式。只有匹配的组件会被缓存。

- include 值为字符串或者正则表达式匹配的组件name会被缓存。
- exclude 值为字符串或正则表达式匹配的组件name不会被缓存。

## 14、vue中如何编`写可复用的组`件？

1. 构成组件

   + 需要低耦合高内聚

     + 耦合是出问题,低耦合是不出问题
     + 耦合越弱越好,内聚越强越好

   + 实例中能看出，组件由*状态*、*事件*和嵌套的*片断*组成。状态，是组件当前的某些数据或属性，如 video 中的 src、width 和 height。事件，是组件在特定时机触发一些操作的行为，如 video 在视频资源加载成果或失败时会触发对应的事件来执行处理。片段，指的是嵌套在组件标签中的内容，该内容会在某些条件下展现出来，如在浏览器不支持 video 标签时显示提示信息。

     在 Vue 组件中，状态称为 props，事件称为 events，片段称为 slots。组件的构成部分也可以理解为组件对外的接口。良好的可复用组件应当定义一个清晰的公开接口。

     - **Props** 允许外部环境传递数据给组件
     - **Events** 允许组件触发外部环境的副作用
     - **Slots** 允许外部环境将额外的内容组合在组件中。

2. 组件间通信

   + 在 Vue.js 中，父子组件的关系可以总结为 props down, events up 。父组件通过 props 向下传递数据给子组件，子组件通过 events 给父组件发送消息。看看它们是怎么工作的。

3. 业务无关

   + 命名

     + 组件的命名应该跟业务无关。应该依据组件的功能为组件命名。

   + 业务数据无关

     + 可复用组件只负责 UI 上的展示和一些交互以及动画，如何获取数据跟它无关，因此不要在组件内部去获取数据，以及任何与服务端打交道的操作。可复用组件只实现 UI 相关的功能。

   + 组件职责

     + 组件可以分为通用组件（可复用组件）和业务组件（一次性组件）。

     + 可复用组件实现通用的功能（不会因组件使用的位置、场景而变化）：

       + UI 的展示
       + 与用户的交互（事件）
       + 动画效果

     + 业务组件实现偏业务化的功能：

       + 获取数据
       + 和 vuex 相关的操作
       + 埋点
       + 引用可复用组件

       可复用组件应尽量减少对外部条件的依赖，所有与 vuex 相关的操作都不应在可复用组件中出现。

       组件应当避免对其父组件的依赖，不要通过 this.$parent 来操作父组件的示例。父组件也不要通过 this.$children 来引用子组件的示例，而是通过子组件的接口与之交互。

4. 命名空间

   + 可复用组件除了定义一个清晰的公开接口外，还需要有命名空间。命名空间可以避免与浏览器保留标签和其他组件的冲突。特别是当项目引用外部 UI 组件或组件迁移到其他项目时，命名空间可以避免很多命名冲突的问题。

     ```
     <xl-button></xl-button>
     <xl-table></xl-table>
     <xl-dialog></xl-dialog>
     ...
     ```

   + 业务组件也可以有命令空间，跟通用组件区分开。这里用 st (section) 来代表业务组件。

5. 上下文无关

   + 可复用组件应尽量减少对外部条件的依赖。没有特别需求且单个组件不至于过重的的前提下，不要把一个有独立功能的组件拆分成若干个小组件。
   + 能够降低组件使用的门槛

6. 数据扁平化

   + 定义组件接口时,尽量不要将整个对象作为一个prop传进来
   + 每个prop应该是一个简单类型的数据,这样做有几个好处
     + 组件接口清晰
     + props校验方便
     + 当服务端返回的对象中的key名称与组件接口不一样时,不需要重新构造一个对象
   + 扁平化的props能让我们更直观的理解组件的接口

7. 使用自定义事件实现数据的双向绑定

   + 有时候，对于一个状态，需要同时从组件内部和组件外部去改变它。

     例如，模态框的显示和隐藏，父组件可以初始化模态框的显示，模态框组件内部的关闭按钮可以让其隐藏。一个好的办法是，使用自定义事件改变父组件中的值

     要让组件的 v-model 生效，它必须：

     - 接受一个 value 属性
     - 在有新的 value 时触发 input 事件

     **注意：**由于每个组件的 input 事件只能用来对一个数据进行双向绑定，所以当存在多个需要向上同步的数据时，请不要使用 v-model，请使用多个自定义事件，并在父组件中同步新的值。

8. 使用自定义watcher优化DOM操作

   + 在开发中,有些逻辑无法使用数据绑定,无法避免需要对DOM的操作,例如,视频的播放需要同步video对象的播放操作及组件内的播放状态,可以使用自定义watcher来优化DOM的操作

9. 项目骨架

   + 单组件不异过重，组件在功能独立的前提下应该尽量简单，越简单的组件可复用性越强。当你实现组件的代码，不包括CSS，有好几百行了（这个大小视业务而定），那么就要考虑拆分成更小的组件。

     当组件足够简单时，就可以在一个更大的业务组件中去自由组合这些组件，实现我们的业务功能。因此，理想情况下，组件的引用层级，只有两级。业务组件引用通用组件。

     我们可以得到一个扁平化的结构。

   + 在一个庞大的项目当中，组件间的引用关系会更复杂一些。当单页应用有多个路由，每个路由组件过重，需要拆分模块时。组件结构会变成下图这样。

     ![img](images/JS/1609579-20190509195355648-873558532.png)

## 15、什么是`vue生命周期`？

+ vue每个组件都是独立的，每个组件都有一个属于它的生命周期，从一个组件**创建、数据初始化、挂载、更新、销毁**，这就是一个组件所谓的生命周期。

## 16、vue生命周期钩子函数有哪些？

+ beforeCreate 创建前状态
  + 举个栗子：可以在这加个loading事件 
+ created 创建完毕状态
  + 在此处this.$data和this.message被初始化
  + 在这结束loading,还做一些初始化,实现函数自执行
+ beforeMount 挂载前状态
  + 在此处$el被初始化
+ mounted 挂载结束状态
  + 在这发起后端请求,拿回数据,配合路由钩子做一些事情
+ beforeUpdate 更新前状态
+ updated 更新完成状态
+ beforeDestory 销毁前状态
  + 你确定删除xx嘛?
+ destoryed 销毁完成状态
  + 当前组件已被删除,清空相关内容

## 17、vue如何`监听键盘事件`中的按键？

+ Vue中允许在监听的时候添加关键修饰符 v-on:keyup.13

+ 对于一些常用健还提供了别名

  + .enter
    .tab
    .delete (捕获“删除”和“退格”键)
    .esc
    .space
    .up
    .down
    .left
    .right

+ 修饰键：

  + .ctrl
    .alt
    .shift
    .meta

  + 与按键别名不同的是，修饰键和 keyup 事件一起用时，事件引发时必须按下正常的按键。换一种说法：如果要引发 keyup.ctrl，必须按下 ctrl 时释放其他的按键；单单释放 ctrl 不会引发事件

    ```js
    <!-- 按下Alt + 释放C触发 --> <input @keyup.alt.67="clear">  <!-- 按下Alt + 释放任意键触发 --> <input @keyup.alt="other"> <!-- 按下Ctrl + enter时触发 --> <input @keydown.ctrl.13="submit">
    ```

+ 对于elementUI的input,我们需要在后面加上.native,因为它对input进行了封装,原生的事件不起作用

## 18、vue更新数组时触发视图更新的方法

1. 变异方法

   vue包含一组观察数组的变异方法,所以他们也将会触发视图更新,这些方法如下:

   + push()
   + pop()
   + shift()
   + unshift()
   + splice()
   + sort()
   + reverse()

2. 替换数组

   例如：`filter()`, `concat()`和 `slice()` 。这些不会改变原始数组，但总是返回一个新数组。当使用这些非变异方法时，可以用新数组替换旧数组：

   ```
   example1.items = example1.items.filter(function (item) {
     return item.message.match(/Foo/)
   })
   ```

   > 你可能认为这将导致 Vue 丢弃现有 DOM 并重新渲染整个列表。幸运的是，事实并非如此。Vue 为了使得 DOM 元素得到最大范围的重用而实现了一些智能的、启发式的方法，所以用一个含有相同元素的数组去替换原来的数组是非常高效的操作。

3. 注意事项：

   ------

   由于 JavaScript 的限制，Vue 不能检测以下变动的数组：

   1. 当你利用索引直接设置一个项时，例如：`vm.items[indexOfItem] = newValue`
   2. 当你修改数组的长度时，例如：`vm.items.length = newLength`

   举个例子：

   ```
   var vm = new Vue({
     data: {
       items: ['a', 'b', 'c']
     }
   })
   vm.items[1] = 'x' // 不是响应性的
   vm.items.length = 2 // 不是响应性的
   ```

   为了解决第一类问题，以下两种方式都可以实现和`vm.items[indexOfItem] = newValue` 相同的效果，同时也将`触发状态更新：`

   ```
   // Vue.set
   Vue.set(vm.items, indexOfItem, newValue)
   // Array.prototype.splice
   vm.items.splice(indexOfItem, 1, newValue)
   ```

   你也可以使用`vm.$set`实例方法，该方法是全局方法 `Vue.set` 的一个别名：

   ```
   vm.$set(vm.items, indexOfItem, newValue)
   ```

   为了解决第二类问题，你可以使用 splice：

   ```
   vm.items.splice(newLength)
   ```

## 19、vue中`对象更改检测`的注意事项

对于已经创建的实例，Vue 不能动态添加根级别的响应式属性。但是，可以使用 `Vue.set(object, key, value)`方法向嵌套对象`添加响应式属性`。

```vue
var vm = new Vue({
  data: {
    userProfile: {
      name: 'Anika'
    }
  }
})
你可以添加一个新的 age 属性到嵌套的 userProfile对象：
Vue.set(vm.userProfile, 'age', 27)

你还可以使用 vm.$set实例方法，它只是全局Vue.set 的别名：
vm.$set(vm.userProfile, 'age', 27)

有时你可能需要为已有对象赋予多个新属性，比如使用 Object.assign()或 _.extend()。在这种情况下，你应该用两个对象的属性创建一个新的对象。所以，如果你想添加新的响应式属性，不要像这样：
Object.assign(vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})

应该这样做：
vm.userProfile = Object.assign({}, vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```

## 20、解决非工程化项目初始化页面闪动问题

使用v-clock指令,vue页面在加载的时候闪烁花括号{}}，v-cloak指令和css规则如[v-cloak]{display:none}一起用时，这个指令可以隐藏未编译的Mustache标签直到实例准备完毕。

## 21、v-for产生的列表，实现active的切换

```HTML
<ul>
      <li v-for="(info,index) in list" :key="info.id" @click="select(index)" v-bind:class="{'active':info.active}">
          {{info.name}}
    </li>
</ul>

list:[
	{name:'a',id:1,active:false},
	{name:'b',id:2,active:false},
	{name:'c',id:3,active:false},
	{name:'d',id:4,active:false},
]

select(d){
	this.list.map(s=>s.active=false);
	this.list[d].active=true;
},
```

## 22、v-model语法糖在组件中的使用

+ v-bind => :
+ v-on => @

## 23、十个常用的自定义过滤器 ???

1. 去除空格 1-所有空格 2-前后空格 3-前空格 4-后空格

```js
function trim(value, trim) {
    switch (trim) {
        case 1:
            return value.replace(/\s+/g, “”);
        case 2:
            return value.replace(/(^\s*)|(\s*$)/g, "");
        case 3:
            return value.replace(/(^\s*)/g, "");
        case 4:
            return value.replace(/(\s*$)/g, "");
        default:
            return value;
    }
}
```

2. 字符串循环复制,count->次数

```js
function repeatStr(str, count) {
    var text = '';
    for (var i = 0; i < count; i++) {
    	text += str;
    }
    return text;
}
```

3. 数字保留小数,默认保留两位且四舍五入,可根据需求变更参数

```js
function keepDecimal(value, few = 2, isRound = true) {
	if(isRound) {
		return value.toFixed(2)
	} else {
		value = value.toString()
		return Number(value.slice(0, value.indexOf('.') + (few + 1)))
	}
}
```

4. 首字母大写

## 24、vue等单页面应用及其优缺点

+ 优点

  用户体验好，快，内容的改变不需要重新加载整个页面，对服务器压力较小。

  前后端分离，比如vue项目

  完全的前端组件化，前端开发不再以页面为单位，更多地采用组件化的思想，代码结构和组织方式更加规范化，便于修改         和调整；

+ 缺点：

  首次加载页面的时候需要加载大量的静态资源，这个加载时间相对比较长。

  不利于 SEO优化，单页页面，数据在前端渲染，就意味着没有 SEO。

  页面导航不可用，如果一定要导航需要自行实现前进、后退。（由于是单页面不能用浏览器的前进后退功能，所以需要自        己建立堆栈管理）

## 25、什么是vue的计算属性？

模板内的表达式非常便利，但是设计它们的初衷是用于**简单运算的**。在模板中放入太多的逻辑会让模板过重且难以维护。所以，对于任何复杂逻辑，都应当使用计算属性。

## 26、vue-cli提供的几种脚手架模板

1. webpack-simple模板

   ```
   ├── README.md
   ├── index.html
   ├── ``package``.json
   ├── src
   │  ├── App.vue
   │  ├── assets
   │  │  └── logo.png
   │  └── main.js
   └── webpack.config.js
   ```

   webpack-simple只配置了Babel和Vue的编译器，其他的一无所有。这个模板值得一提的就是src目录，所有的Vue代码源程序都放置在这个目录中，五个模板构建出来的这个src目录都是一样的，只是在webpack模板中多了components目录用于存放公用组件。

2. webpack模板

   ```
   README.md
   ├── build
   │  ├── build.js
   │  ├── check-versions.js
   │  ├── dev-client.js
   │  ├── dev-server.js
   │  ├── utils.js
   │  ├── webpack.base.conf.js
   │  ├── webpack.dev.conf.js
   │  └── webpack.prod.conf.js
   ├── config
   │  ├── dev.env.js
   │  ├── index.js
   │  ├── prod.env.js
   │  └── test.env.js
   ├── index.html
   ├── ``package``.json
   ├── src
   │  ├── App.vue
   │  ├── assets
   │  │  └── logo.png
   │  ├── components
   │  │  └── Hello.vue
   │  └── main.js
   ├── ``static
   └── test
      ``├── e2e
      ``│  ├── custom-assertions
      ``│  │  └── elementCount.js
      ``│  ├── nightwatch.conf.js
      ``│  ├── runner.js
      ``│  └── specs
      ``│     └── test.js
      ``└── unit
         ``├── index.js
         ``├── karma.conf.js
         ``└── specs
            ``└── Hello.spec.js
   ```

   这个webpack模板的结构是非常合理的，而且配置的工具也相当丰富，当投入真正的项目开发时会觉得模板的实用性很强。

   在项目的根目录下多了4个目录，它们的作用分别如下：

   ​	●　build——存放用于编译用的webpack配置与相关的辅助工具代码；

   ​	●　config——存放三大环境配置文件，用于设定环境变量和必要的路径信息；

   ​	●　test——存放E2E测试与单元测试文件以及相关的配置文件；

   ​	●　static——存放项目所需要的其他静态资源文件；

   ​	●　dist——存放运行npm run build指令后的生产环境输出文件，可直接部署到服务器对应的静态资源文件夹内，该文件夹只有在运行build之后才会生成。

   可见，这些目录的存在是依赖于模板内配置的开发工具的

## 27、vue父组件如何向子组件中传递数据？

1. 父组件向子组件传递数据，首先要在父组件中使用`v-bind`命令将要传递的数据绑定到子组件上。父组件中完成数据绑定之后，在子组件中的props属性接收一下父组件传递过来的数据，要接收多少数据，就在pros属性中写多少数据。
2. 注意 :子组件需要嵌套到父组件内

## 28、vue-cli开发环境使用全局常量

开发环境的全局常量定义在.env里

## 29、vue-cli生产环境使用全局常量

生产环境的全局常量定义在.env.production里

## 30、vue弹窗后如何禁止滚动条滚动？

```vue
methods : {
    //禁止滚动
    stop(){
        var mo=function(e){e.preventDefault();};
        document.body.style.overflow=‘hidden’;
        document.addEventListener(“touchmove”,mo,false);//禁止页面滑动
    },
    /***取消滑动限制***/
    move(){
        var mo=function(e){e.preventDefault();};
        document.body.style.overflow=’’;//出现滚动条
        document.removeEventListener(“touchmove”,mo,false);
    }
}
```

## 31、计算属性的缓存和方法调用的区别

- 计算属性必须**返回结果**
- 计算属性是基于它的依赖缓存的。一个计算属性所依赖的数据发生变化时，它才会重新取值。
- 使用计算属性还是methods取决于是否需要**缓存**，当遍历大数组和做大量计算时，应当使用计算属性，除非你不希望得到缓存。
- 计算属性是根据依赖自动执行的，methods需要事件调用

32、vue-cli中自定义指令的使用

1. 全局注册

   ```vue
   // 注册一个全局自定义指令 `v-focus`
   Vue.directive('focus', {
     // 当被绑定的元素插入到 DOM 中时……
     inserted: function (el) {
       // 聚焦元素
       el.focus()
     }
   })
   ```

2. 局部注册

   ![image-20201015195650495](images/JS/image-20201015195650495.png)

# vue-router

## 1、vue-router如何响应 路由参数 的变化？

+ 什么是路由参数的变化
  + 当使用路由参数时，例如从 `/user/foo` 导航到 `/user/bar`，原来的组件实例会被复用。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。不过，这也意味着组件的生命周期钩子不会再被调用。

+ 监测路由参数变化的方法
+ 方法一watch监听：

```
watch: { // watch的第一种写法
    $route (to, from) {
        console.log(to)
        console.log(from)
    }
},
======================================
watch: { // watch的第二种写法
    $route: {
        handler (to, from){
            console.log(to)
            console.log(from)
        },
        // 深度观察监听
        deep: true
    }
},
======================================
watch: { // watch的第三种写法
    '$route':'getPath'
},
methods: {
    getPath(to, from){
        console.log(this.$route.path);
    }
},
```

+ 方法二：导航守卫

```
beforeRouteEnter (to, from, next) {
    console.log('beforeRouteEnter被调用：在渲染该组件的对应路由被 confirm 前调用')
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this` 因为当守卫执行前，组件实例还没被创建
    // 可以通过传一个回调给 next来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。
    next(vm => {
        // 通过 `vm` 访问组件实例
        console.log(vm)
    })
},
// beforeRouteEnter 是支持给 next 传递回调的唯一守卫。
// 对于 beforeRouteUpdate 和 beforeRouteLeave 来说，this 已经可用了，所以不支持传递回调，因为没有必要了。
beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
    console.log('beforeRouteUpdate被调用：在当前路由改变，但是该组件被复用时调用')
    next()
},
beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
    const answer = window.confirm('是否确认离开当前页面')
    if (answer) {
        console.log('beforeRouteLeave被调用：导航离开该组件的对应路由时调用')
        next()
    } else {
        next(false)
    }
},
```

## 2、完整的 vue-router 导航解析流程

- 1.导航被触发。

- 2.在失活的组件里调用离开守卫。

- 3.调用全局的 `beforeEach` 守卫。

- 4.在`重用的组件`里调用 `beforeRouteUpdate` 守卫（2.2+）。

- 5.在路由配置里调用 `beforeEnter`。

- 6.解析异步路由组件。

- 7.在被激活的组件里面调用 `beforeRouterEnter`。

- 8.调用全局的 `beforeResolve` 守卫（2.5+）。

- 9.导航被确认。

- 10.调用全局的 `afterEach`钩子。

- 11.触发 `DOM` 更新。

- 12.用创建好的实例调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数。

- 如何解决路由参数或查询的改变并不会触发进入/离开的导航守卫？

  通过 watch 观察 `$route`

  ```js
  /* 组件 */
  export default {
    watch: {
      $route(to, from) {
        // 对路由变化作出响应...
      }
    }
  };
  ```

## 3、vue-router有哪几种导航钩子（ 导航守卫 ）？

1. 全局的

   全局导航钩子主要有两种钩子：

   + 前置守卫

     + ```js
       router.afterEach((to, from) => {
           // do someting
       });
       ```

     + 这三个参数 to 、from 、next 分别的作用：

       + to: Route，代表要进入的目标，它是一个路由对象
       + from: Route，代表当前正要离开的路由，同样也是一个路由对象
       + next: Function，这是一个必须需要调用的方法，而具体的执行效果则依赖 next 方法调用的参数
         + next()：进入管道中的下一个钩子，如果全部的钩子执行完了，则导航的状态就是 confirmed（确认的）
         + next(false)：这代表中断掉当前的导航，即 to 代表的路由对象不会进入，被中断，此时该表 URL 地址会被重置到 from 路由对应的地址
         + next(‘/’) 和 next({path: ‘/’})：在中断掉当前导航的同时，跳转到一个不同的地址
         + next(error)：如果传入参数是一个 Error 实例，那么导航被终止的同时会将错误传递给 router.onError() 注册过的回调

       > 注意：next 方法必须要调用，否则钩子函数无法 resolved

   + 后置钩子

     + ```js
       router.afterEach((to, from) => {
           // do someting
       });
       ```

     + 不同于前置守卫，后置钩子并没有 next 函数，也不会改变导航本身

2. 单个路由独享的钩子

   顾名思义，即单个路由独享的导航钩子，它是在路由配置上直接进行定义的：

   ```
   cont router = new VueRouter({
       routes: [
           {
               path: '/file',
               component: File,
               beforeEnter: (to, from ,next) => {
                   // do someting
               }
           }
       ]
   });1234567891011
   ```

   至于他的参数的使用，和全局前置守卫是一样的

3. 组件内的导航钩子

   组件内的导航钩子主要有这三种：beforeRouteEnter、beforeRouteUpdate、beforeRouteLeave。他们是直接在路由组件内部直接进行定义的

   我们看一下他的具体用法：

   ```
   const File = {
       template: `<div>This is file</div>`,
       beforeRouteEnter(to, from, next) {
           // do someting
           // 在渲染该组件的对应路由被 confirm 前调用
       },
       beforeRouteUpdate(to, from, next) {
           // do someting
           // 在当前路由改变，但是依然渲染该组件是调用
       },
       beforeRouteLeave(to, from ,next) {
           // do someting
           // 导航离开该组件的对应路由时被调用
       }
   }123456789101112131415
   ```

   需要注意是：

   > beforeRouteEnter 不能获取组件实例 this，因为当守卫执行前，组件实例被没有被创建出来，剩下两个钩子则可以正常获取组件实例 this

   但是并不意味着在 beforeRouteEnter 中无法访问组件实例，我们可以通过给 next 传入一个回调来访问组件实例。在导航被确认是，会执行这个回调，这时就可以访问组件实例了，如：

   ```
   beforeRouteEnter(to, from, next) {
       next (vm => {
           // 这里通过 vm 来访问组件实例解决了没有 this 的问题
       })
   }12345
   ```

   > 注意，仅仅是 beforRouteEnter 支持给 next 传递回调，其他两个并不支持。因为归根结底，支持回调是为了解决 this 问题，而其他两个钩子的 this 可以正确访问到组件实例，所有没有必要使用回调

## 4、vue-router的几种实例方法以及参数传递

实例方法：

| 实例方法                                              | 说明                                                         |
| ----------------------------------------------------- | ------------------------------------------------------------ |
| this.$router.push(location, onComplete?, onAbort?)    | 这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。并且点击 `<router-link :to="...">`等同于调用 `router.push(...)`。 |
| this.$router.replace(location, onComplete?, onAbort?) | 这个方法不会向 history 添加新记录，而是跟它的方法名一样 —— 替换掉当前的 history 记录，所以，当用户点击浏览器后退按钮时，并不会回到之前的 URL。 |
| this.$router.go(n)                                    | 这个方法的参数是一个整数，意思是在 history 记录中向前或者后退多少步，类似 `window.history.go(n)`。 |

注意：

1、在 2.2.0+，可选的在 `router.push` 或 `router.replace` 中提供 `onComplete` 和 `onAbort` 回调作为第二个和第三个参数。这些回调将会在`导航成功完成` (在所有的异步钩子被解析之后) 或`终止` (导航到相同的路由、或在当前导航完成之前导航到另一个不同的路由) 的时候进行相应的`调用`。

2、如果`目的地`和`当前路由相同`，只有`参数发生了改变` (比如从一个用户资料到另一个 `/users/1 -> /users/2`)，你需要使用 `beforeRouteUpdate` 来响应这个变化 (比如抓取用户信息)。

参数传递方式：

vue-router提供了`params`、`query`、`meta`三种页面间传递参数的方式。

在`组件`中使用：

```
//通过 $route 对象获取，注意是route，么有r
this.$route.params

this.$route.query

this.$route.meta
```

`$route`是“路由信息对象”，包括path，params，hash，query，fullPath，matched，name等路由信息参数。
`$router`是“路由实例”对象包括了路由的跳转方法，钩子函数等。

## 5、vue-router的动态路由匹配以及使用

1. 我们可以在src——routers——index.js文件中

```javascript
export default new Router({
  routes: [  //配置路由，这里是个数组
    {        //每一个链接都是一个对象
      path: '/', //链接路径
      name: 'HelloWorld',  //路由名称
      component: HelloWorld  //对应的组件模版
    },
    {        //每一个链接都是一个对象
      path: '/hi/:id', //链接路径
      name: 'hi',  //路由名称
      component: hi  //对应的组件模版
    }
  ]
})
```

2. 我们在 vue文件组件中接收

```javascript
<template>
    <div class="hello">
        <h1>{{msg}}</h1>
        <p>{{$route.params.id}}</p>
    </div>
</template>
```

3. 在src——App.vue中使用传参数

```html
<p>
	导航：
	<router-link :to="'/hi/'+'judy'">hi页面</router-link>
	<router-link to="/">首页</router-link>
</p>
```

也可以使用此方法传递参数

```html
<router-link :to="{name:'hi',params:{username:'judy'}}">hi页面</router-link>
```

你可以在一个路由中设置多段『路径参数』，对应的值都会设置到 `$route.params` 中。例如：

| 模式                          | 匹配路径            | $route.params                        |
| :---------------------------- | :------------------ | :----------------------------------- |
| /user/:username               | /user/evan          | `{ username: 'evan' }`               |
| /user/:username/post/:post_id | /user/evan/post/123 | `{ username: 'evan', post_id: 123 }` |

## 6、vue-router如何定义嵌套路由？

https://www.cnblogs.com/shenjianping/p/11313410.html

## 7、`<router-link></router-link>`组件及其属性

https://blog.csdn.net/qq_39207948/article/details/81484165

## 8、vue-router实现路由懒加载（ 动态加载路由 ）

https://www.cnblogs.com/art-poet/p/12509327.html

## 9、vue-router路由的两种模式

https://www.cnblogs.com/ceceliahappycoding/p/10552620.html

## 10、history路由模式与后台的配合

1. hash ——即地址栏URL中的#符号。
   hash 虽然出现URL中，但不会被包含在HTTP请求中，对后端完全没有影响，因此改变hash不会重新加载页面。

2. history ——利用了HTML5 History Interface 中新增的pushState() 和replaceState() 方法。需要特定浏览器支持
   **history模式，会出现404 的情况，需要后台配置。**

3. 1、hash模式下，仅hash符号之前的内容会被包含在请求中，如 [http://www.baidu.com](http://www.baidu.com/), 因此对于后端来说，即使没有做到对路由的全覆盖，也不会返回404错误；

4. history模式下，前端的url必须和实际向后端发起请求的url 一致，如http://www.baidu.com/a/ 。如果后端缺少对/a 的路由处理，将返回404错误。

   **history模式下配置nginx**

   ```
   location / {
     try_files $uri $uri/ /index.html;
   }
   ```

   **history模式下配置Apache**

   ```
   <IfModule mod_rewrite.c>
     RewriteEngine On
     RewriteBase /
     RewriteRule ^index\.html$ - [L]
     RewriteCond %{REQUEST_FILENAME} !-f
     RewriteCond %{REQUEST_FILENAME} !-d
     RewriteRule . /index.html [L]
   </IfModule>
   ```

   **history模式下配置Node.js**

   ```
   const http = require('http')
   const fs = require('fs')
   const httpPort = 80
   
   http.createServer((req, res) => {
     fs.readFile('index.htm', 'utf-8', (err, content) => {
       if (err) {
         console.log('We cannot open "index.htm" file.')
       }
   
       res.writeHead(200, {
         'Content-Type': 'text/html; charset=utf-8'
       })
   
       res.end(content)
     })
   }).listen(httpPort, () => {
     console.log('Server listening on: http://localhost:%s', httpPort)
   })
   ```

# vuex

## 1、什么是vuex？

Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

官方文档是这么说的，反正我是没看明白

以自己的话说vuex是一个把多个组件通用的数据我们把它拿出来，放到一个叫store里面管理，在需要使用的组件里，我们可以拿出来使用 简单来说就是data的共享属性

## 2、使用vuex的核心概念

+ state 数据源，载体
  + 通过计算属性改变值
+ getters 用于改变state的值，派生出多个数据源
  + 通过getters可以派生出一些新的状态
+ mutation 唯一可以提交可以改变state的状态，也就是数据的属性值
  + 更改Vuex的store中的状态的唯一方法，也只能通过mutations去改变值
+ actions 提交的是mutation,用commit提交 而不是直接变更状态，可以包含任意异步出操作
+ modules 拆分成多个模块
  + 当需要管理的状态比较多时，我们需要将vuex的stroe对象分割成模块

## 3、vuex在vue-cli中的应用

https://www.cnblogs.com/ichenchao/articles/10876717.html

https://www.cnblogs.com/hopesthwell/p/9256982.html

4、组件中使用 vuex 的值和修改值的地方？???

## 5、在vuex中使用异步修改

https://www.jianshu.com/p/a9f3fb2f7426

https://www.cnblogs.com/Elva3zora/p/12714546.html

## 6、pc端页面刷新时实现vuex缓存

1. 首先在app.vue 中created周期里写入监听页面刷新的方法

   ![页面刷新时如何实现vuex数据缓存](https://exp-picture.cdn.bcebos.com/2e66f9ef28066b01ced5f4f53df39187031cf35d.jpg?x-bce-process=image%2Fresize%2Cm_lfit%2Cw_500%2Climit_1)

2. 在app.vue 中created下写入页面加载时读取sessionStorage里的状态信息

   ![页面刷新时如何实现vuex数据缓存](https://exp-picture.cdn.bcebos.com/32fbcd41037de1376a7ee509f6c5cf672a5f2aa2.jpg?x-bce-process=image%2Fresize%2Cm_lfit%2Cw_500%2Climit_1)

3. 这样刷新页面时 ，vuex 中的数据就能存入session，页面刷新完又从新从session中获取

# http请求

## 1、Promise对象是什么？

Promise对象是ES6（ ECMAScript 2015 ）对于异步编程提供的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。

> 详细解答：

传统回调：

```
// 当参数a大于10且参数func2是一个方法时 执行func2
function func1(a, func2) {
   if (a > 10 && typeof func2 == 'function') {
       func2()
   }
}

func1(11, function() {
   console.log('this is a callback')
})
```

Promise对象改写：

```
function func1(a){
	return new Promise((resolve,reject) => {
    	if(a > 10){
        	resolve(a)
        }else{
        	reject(b)
        }
    })
};

func1('11').then(res => {
	console.log('success');
}).catch(err => {
	console.log('error');
})
```

> Promise对象的两个特点：

1、对象的状态不受外界影响。

Promise对象有三种状态：`pending（进行中）、fulfilled（已成功）和rejected（已失败）。`

只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。

这也是Promise这个名字的由来，它的英语意思就是`“承诺”`，表示其他手段无法改变。

2、一旦状态改变，就不会再变，任何时候都可以得到这个结果。

Promise对象的状态改变，只有两种可能：

从pending变为fulfilled和从pending变为rejected。

只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。

如果改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

> Promise对象实例的两个参数，resolve 和 reject：

Promise构造函数接受一个函数作为参数，该函数的两个参数分别`resolve` 和 `reject`。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。

resolve函数的作用是： 将Promise对象的状态从“未完成”变为“成功”（即从 pending 变为 fulfilled），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；

reject函数的作用是： 将Promise对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

> Promise对象实例的方法，then 和 catch：

.then方法： 用于指定调用成功时的回调函数。

then方法返回的是一个新的Promise实例（注意，不是原来那个Promise实例），因此可以采用链式写法，即then方法后面再调用另一个then方法。

.catch方法： 用于指定发生错误时的回调函数。

## 2、axios、fetch与ajax有什么区别？

https://www.jianshu.com/p/8bc48f8fde75

## 3、什么是JS的同源策略和跨域问题？

所谓同源策略，指的是浏览器对不同源的脚本或者文本的访问方式进行的限制。比如源a的js不能读取或设置引入的源b的元素属性。同源策略是浏览器上为安全性考虑实施的非常重要的安全策略。

**何谓同源:**

就是指两个页面具有相同的协议，主机（也常说域名），端口，三个要素缺一不可。

URL由协议、域名、端口和路径组成，如果两个URL的协议、域名和端口相同，则表示他们同源。

同源策略限制了不同源之间的交互，但是有人也许会有疑问，我们以前在写代码的时候也常常会引用其他域名的js文件，样式文件，图片文件什么的，没看到限制啊，这个定义是不是错了。**其实不然，同源策略限制的不同源之间的交互主要针对的是js中的XMLHttpRequest等请求，下面这些情况是完全不受同源策略限制的。**

- **页面中的链接，重定向以及表单提交是不会受到同源策略限制的。**链接就不用说了，导航网站上的链接都是链接到其他站点的。而你在域名`www.foo.com`下面提交一个表单到`www.bar.com`是完全可以的。
- **跨域资源嵌入是允许的，当然，浏览器限制了Javascript不能读写加载的内容**。如前面提到的嵌入的`<script src="..."></script>，<img>，<link>，<iframe>等`。当然，如果要阻止iframe嵌入我们网站的资源(页面或者js等)，我们可以在web服务器加上一个`X-Frame-Options DENY`头部来限制。nginx就可以这样设置`add_header X-Frame-Options DENY;`。

**出现跨域问题前置条件是我们在WEB服务器或者服务端脚本中设置`ACCESS-CONTROL-ALLOW-ORIGIN`头部，如果设置了这些头部并允许某些域名跨域访问，则浏览器就会跳过同源策略的限制返回对应的内容。此外，如果你不是通过浏览器去获取资源，比如你通过一个python脚本去调用接口或者获取js文件，也不在这个限制之内。**

通过ajax调用其他域的接口会有跨域问题。比如在`http://www.foo.com/index.html`中通过ajax调用请求`http://www.bar.com/js/test.js`页面，此时是会报错的。

https://www.cnblogs.com/chaoyuehedy/p/5556557.html

## 4、如何解决跨域问题？

https://blog.csdn.net/yup1212/article/details/87633272

## 5、vue-cli中如何使用`JSON数据模拟`？

http://www.manongjc.com/article/2707.html

## 6、vue-cli中http请求的统一管理。

https://blog.csdn.net/weixin_30820151/article/details/99162766

## 7、axios有什么特点？

1.从浏览器中创建XMLHttpRequests
2.node.js创建http请求
3.支持Promise API
4.拦截请求和响应
5.转换请求数据和响应数据
6.取消请求
7.自动换成json
axios中的发送字段的参数是data跟params两个
两者的区别在于params是跟请求地址一起发送的，data的作为一个请求体进行发送
params一般适用于get请求
data一般适用于post put 请求

# UI样式

## 1、`.vue组件的scoped属性`的作用

当style标签具有该scoped属性时，其CSS将仅应用于当前组件的元素

## 2、如何让CSS只在当前组件中起作用？

当前组件<style>写成<style scoped>

## 3、vue-cli中常用的UI组件库

https://www.jianshu.com/p/f065daed4356

## 4、如何适配移动端？【 经典 】

https://blog.csdn.net/chenjuan1993/article/details/81710022

## 5、移动端常用媒体查询的使用

```css
@media (max-width: 1500px) {
	body{
		background: red;
		font-size: 160px;
	}
}
@media (min-width: 700px) and (max-width: 1000px) {
	body{
		background: pink;
		font-size: 160px;
	}
}
/* 如果是横屏，背景将是浅蓝色 竖屏portrait*/
@media only screen and (orientation: landscape) {   
	body{
		background-color: lightblue;
	}
}
/*屏幕小于400px,显示小图*/
body{
	background-image: url(img_smalltu.jpg);
}
/*屏幕大于等于400px,显示大图*/
@media only screen and (min-width:400px) {
	body{
		background-image: url("img_tu.jpg");
	}
}
```

6、垂直居中对齐

1. margin:0 auto
2. 设置div元素的祖先元素html和body的高度为100%（因为他们默认是为0的），并且清除默认样式，即把margin和padding设置为0（如果不清除默认样式的话，浏览器就会出现滚动条)
3. 给需要垂直居中的按钮添加相对定位
4. top偏移50%
5. margin-top 设置-(盒子的一半高度)

## 7、vue-cli中如何使用背景图片？

https://segmentfault.com/a/1190000020983081?utm_source=tag-newest

## 8、使用表单禁用时移动端样式问题

```css
input:disabled{
    color:xxx;
    opacity:1;
    //text-fill-color文本填充颜色，只兼容webkit内核
    -webkit-text-fill-color:xxx;
    -webkit-opacity:1;
    font-size:16px;
}
```

## 9、多种类型文本超出隐藏问题

https://www.cnblogs.com/kzxiaotan/p/11677935.html

# 常用功能

## 1、vue中如何实现tab切换功能？

> 		vue有3种实现tab切换功能
> 		
> 					1.v-show控制内容切换
> 		
> 					2.组件切换
> 		
> 					3.路由切换（对地址栏和数据请求友好）

+ **一、v-show控制内容切换**

  > 1.简单版原理：用点击事件改变num值作为开关，控制tab样式和内容显示隐藏。

  	![点击切换vue的tab](https://img-blog.csdnimg.cn/20190509110907433.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjkwNTQ3,size_16,color_FFFFFF,t_70)

  > 2.数据渲染原理：主要利用v-for绑定的index来控制，跟上面差不多

  

  ![for-tab](images/JS/20190511164316511.png)

+ **二、组件切换**

  > 1.知识点主要是vue中js的特性，和keep-alive缓存

  ![vue组件切换](images/JS/20190511170840371.png)

+ **三、路由切换(对地址栏和数据请求友好)**

  > 通过router-link实现

![在这里插入图片描述](images/JS/20190511174550481.png)

## 2、vue中如何利用 keep-alive 标签实现某个组件缓存功能？

> <keep-alive>是Vue的内置组件，能在组件切换过程中将状态保留在内存中，防止重复渲染DOM.
>
> <keep-alive> 包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。和 <transition> 相似，<keep-alive> 是一个抽象组件：它自身不会渲染一个 DOM 元素，也不会出现在父组件链中.
>
> **prop:**
>
> - include: 字符串或正则表达式。只有匹配的组件会被缓存。
>
> - exclude: 字符串或正则表达式。任何匹配的组件都不会被缓存。
>
>   当我们在开发vue的项目过程中，避免不了在路由切换到其他的component再返回后该组件数据会重新加载，处理这种情况我们就需要用到keep-alive来缓存vue的组件信息，使其不再重新加载。

**一、在app.vue里**

```vue
<keep-alive>
 <router-view></router-view>
</keep-alive>
```

但是这种情况会对所有的组件进行缓存，不能达到单个组件缓存的结果

那么我们会给部分组件加上，实现方法如下：

```vue
<!--这里是需要keepalive的-->
<keep-alive>
 <router-view v-if="$route.meta.keepAlive"></router-view>
<keep-alive>
 
<!-- 这里不会被keepAlive -->
 
<router-view v-if="!$route.meta.keepAlive"></router-view>
```

**二、在路由的index.js页面里**

```js
{
 path: '',
 name: '',
 component: '',
 meta: {keepAlive: true}  // 这个是需要keepalive的
}，
{
 path: '',
 name: '',
 component: ,
 meta: {keepAlive: false} // 这是不会被keepalive的
}
```

这就实现了部分组件的缓存功能

如果缓存的组件想要清空数据或者执行初始化方法，在加载组件的时候调用activated钩子函数，如下：

```js
activated: function () {
 this.data = ‘'
}
```

## 3、vue中实现切换页面时为左滑出效果

## 4、vue中父子组件如何相互调用方法？

+ ###### 一、Vue中在父调子

  > **通过ref直接调用子组件的方法**

```vue
 <template>  
	<div class="parent"> 
        <child ref="children">子组件</child> 
    </div>
</template> 
<script> 
    export default {    
        name: "parent",    
        data(){   
            return{  }
        } methods:{   
            edit(){ 	
                this.$refs.children.save()  *// save为子组件中的函数*   
            } 
        } 
    } 
</script>
```

+ ###### 二、Vue中在子调父

  > **第一种方法：在子组件里用$emit向父组件触发一个事件，父组件监听这个事件就行了**

```vue
  <!---父组件--->
	<template>  
		<div>   
            <child @fatherMethod="fatherFunction"></child> 
       </div> 
    </template> 
    <script>  
        import child from 'components/child'; 
        export default {  
            components: {    
                child   
            },    
            methods: {    
                fatherFunction(val) {  
                    console.log('子组件中传过来的值',val);   
                }   
            } 
        }; 
    </script>
```


```vue
<!---子组件--->
<template> 
    <div>   
        <button @click="childMethod()">点击</button> 
    </div> 
</template> 
<script>
    export default {  
        methods: {    
                childMethod() {  
                    *// 子组件的方法被触发*    
                    this.$emit('fatherMethod',value);  *// 传递参数* 
                }  
         } 
   };
</script>	
```

> **第二种方法：直接在子组件中通过this.$parent.event来调用父组件的方法**	

```vue
<!--父组件-->
<template> 
	<div>  
        <child></child> 
    </div> 
</template> 
<script> 
    import child from 'components/child'; 
    export default {   
        components: {    
            child   
        },   
        methods: {   
            fatherMethod() {   
                console.log('测试');  
            }   
        }  
    };
</script>
```

```vue
<!--子组件-->
<template> 
	<div>    
    	<button @click="childMethod()">点击</button>  
    </div>
</template> 
<script>  
    export default {   
        methods: {    
            childMethod() {  
                this.$parent.fatherMethod(); 
            }   
        }  
    };
</script>
```

> **第三种方法：父组件把方法传入子组件中，在子组件里直接调用这个方法**

```vue
<!--父组件-->
<template> 
    <div>   
        <child :fatherMethod="fatherMethod"></child>
    </div> 
</template> 
<script>  
    import child from 'components/child'; 
    export default {   
        components: {  
            child  
        },  
        methods: {   
            fatherMethod() {   
                console.log('测试');  
            }   
        }  
    };
</script>
```

```vue
<!--子组件-->
<template> 
	<div>   
    	<button @click="childMethod()">点击</button> 
    </div> 
</template> 
<script> 
    export default {   
        props: {    
            fatherMethod: {   
                type: Function,   
                default: null  
            }   
        },  
        methods: {    
            childMethod() {  
                if (this.fatherMethod) { 
                    this.fatherMethod();   
                }     
            }   
        } 
    };
</script>
```

## 5、vue中央事件总线的使用

> ###### 第一步：项目中创建一个js文件，引入vue，创建一个vue实例，导出这个实例，代码如下：
>
> ```vue
> 1 import Vue from 'Vue'
> 2 export default new Vue
> ```

> ###### 第二步：在两个需要通信的的两个组件中分别引入这个bus.js
>
> 1 import Bus from '这里是你引入bus.js的路径' // Bus可自由更换喜欢的名字

> ###### 第三步：传递数据的组件里通过vue实例方法$emit发送事件名称和需要传递的数据(发送数据组件)
>
> 1 Bus.$emit('click',data) // 这个click是一个自定义的事件名称，data就是你要传递的数据。

> **第四步：被传递数据的组件内通过vue实例方法$on监听到事件和接收到数据(接收数据的组件)这里通常挂载监听在vue生命周期created和mounted当中的一个，具体使用场景需要具体的分析**

```js
1 Bus.$on('click',target => {
2 　　console.log(target)　　//　注意：发送和监听的事件名称必须一致，target就是获取的数据，可以不写						target。只要你喜欢叫什么都可以（当然了，这一定要符合形参变量的命名规范）
3 })
```

通过以上的四步其实就已经实现了最简单的eventbus的实际应用了

但是到这后，一定要注意一个最容易忽视，又必然不能忘记的东西，那就是清除事件总线eventBus，不手动清除，它是一直会存在的，这样的话，有个问题就是反复进入到接收数据的组件内操作获取数据，原本只执行一次的获取的操作将会有多次操作。

> **第五步：在vue生命周期beforeDestroy或者destroyed中用vue实例的$off方法清除eventBus**

```js
1 beforeDestroy(){
2     bus.$off('click')
3  }
```

# 生产环境

## 1、vue打包命令是什么？

npm run build

## 2、vue打包后会生成哪些文件？

生成一个dist 里面有static文件夹和一个index.html文件

## 3、如何配置 vue 打包生成文件的路径？

https://blog.csdn.net/qq_39985511/article/details/83141040

## 4、vue如何优化首屏加载速度？

+ **第一步：webpack-bundle-analyzer分析**

+ **第二步：初步优化**

  + 1.仔细考虑组件是否需要全局引入
  + 2.手动引入ECharts各模块
  + 3.使用更轻量级的工具库

+ **第三步：CDN优化**

  + 1.首先我们在index.html中，添加CDN代码
  + 2.在vue.config.js中加入webpack配置代码，关于webpack配置中的externals，请<a href="https://webpack.js.org/configuration/externals/">参考地址</a>
  + 3.去除vue.use相关代码
    - 使用CDN的好处有以下几个方面
      - （1）加快打包速度，分离公共库以后，每次重新打包就不会再把这些打包进vendors文件中
      - （2）CDN减轻自己服务器的访问压力，并且能实现资源的并行下载，浏览器对src资源的加载是并行的(执行是按照顺序的)

+ **第四步：检查Nginx 是否开启了gzip**

  

  ![img](images/JS/1071709-20190422175218054-1158680500.png)



+ **第五步：检查路由懒加载**

  路由组件如果不按需加载的话，就会把所有的组件一次性打包到app.js中，导致首次加载内容过多，vue官方文档中也有提到，<a href="[https://cn.vuejs.org/v2/guide/components-dynamic-async.html#%E5%BC%82%E6%AD%A5%E7%BB%84%E4%BB%B6](https://cn.vuejs.org/v2/guide/components-dynamic-async.html#异步组件)">地址</a>

  + ```js
    {
      name: 'vipBoxActivity',
      path:'vipBoxActivity',
      component(resolve) {
        require(['COMPONENTS/vipBox/vipBoxActivity/main.vue'], resolve)
      }
    },
    ```

  + ```js
    {
      path: 'buyerSummary',
      name: 'buyerSummary',
      component: () => import('VIEWS/buyer/buyerSummary/index'),
    },
    ```

  以上的两种引入组件的方法都是正确的，都能实现路由的懒加载