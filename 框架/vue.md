

# MVVM

## MVC 模型

- View 用户看到的视图，视图从模型中直接获取数据去渲染；
- Model 一般就是本地数据。
- Controler  是应用程序中处理用户交互的部分。

Controller 负责将 Model 的数据用 View 显示出来，或者从 view 中读取用户输入，并向 model 发送数据



## MVVM 模型

- VM - ViewModel  (vue.js源码) 实现数据的双向绑定： 够监听到数据的变化，然后通知视图进行自动更新；用户操作视图时，VM也能监听到视图的变化，然后通知数据做相应改动。
- M  指的是 前端的一些数据
- V 指的是视图



vue 官网解释到 vue 并不是完全遵守的 MVVM 模型，因为提供了 \$ref 来直接操作 DOM 元素，而MVVM 中 M 与 V 无法直接通信。

MVVM 与 MVC 最大的区别就是：它实现了 View 和 Model 的==自动同步==，不用再自己手动操作 Dom 元素改变 View 。





# vue 双向数据绑定

react 中是基于 props 的单向数据绑定，vue 中的双向数据绑定主要是 v-model。

- v-model 相当于是 `:value`和`@input` 的语法糖

- 在表单中会自动根据不同表单元素绑定不同的事件和属性实现。

- 还可以用于自定义组件。

  - 组件上的 `v-model` 使用 `modelValue` 作为 prop 和 `update:modelValue` 作为事件

  - ~~~vue
    <my-component v-model:title="bookTitle"></my-component>
    ~~~



# vue 响应式原理

## vue 2.0

数据劫持+发布订阅模式：

- 使用 Object.defineProperty 的 getter 和 setter 将属性进行数据劫持。
  - get 里会做依赖收集（watcher）
  - set 里会实现了通知这些依赖更新

- 数组由于性能原因，没有使用 Object.defineProperty 进行数据劫持，而是重写了数组的（push,shift,pop,splice,unshift,sort,reverse）方法。==在 Vue 中修改数组的索引和长度是无法监控到的==。需要通过以上 7 种变异方法修改数组才会触发数组对应的 watcher 进行更新



## vue 3.0 

代理 porxy ：

通过代理对象实现对目标对象的监听和拦截修改。

使用 Proxy 的好处是它可以完美的监听到任何方式的数据改变，唯一的缺点是兼容性的问题，因为这是 ES6 的语法。



# key 的作用

在实际的算法中，为了识别改动的是哪个节点，引入了 `key `的概念。它用于给每一个节点==打标记，用于判断是否是同一个节点==。==v-for默认不会移动DOM, 而是尝试复用, 就地更新==，如果==需要v-for移动DOM==, 你需要用特殊的属性 `key` 来提供一个排序提示。diff 算法在判断完新旧虚拟 DOM 的差异之后，就会将差异记录下来。实现局部更新 DOM。



# v-for和v-if哪个优先级更高？

v-for 和 v-if 不建议一起使用。

vue2 中 v-for 优先级高，会导致大量不必要的渲染。

vue3 中 v-if 优先级高，会导致 v-if 的参数如果是从 v-for 中取，会无法读取到。

解决：

- vue3 可以使用一个 template 进行 v-for ，然后子元素 v-if
- 如果需要筛选列表推荐使用 filter 返回新数组





# 生命周期钩子函数

分类: 4大阶段8个方法

| **阶段** | **方法名**    | **方法名** |
| -------- | ------------- | ---------- |
| 初始化   | beforeCreate  | created    |
| 挂载     | beforeMount   | mounted    |
| 更新     | beforeUpdate  | updated    |
| 销毁     | beforeDestroy | destroyed  |

1.  `beforeCreate` 获取不到 `props` 或者 `data` 中的数据的，因为数据没有初始化
2.  `created` 可以访问到之前不能访问到的数据。
3.  `beforeMount` 
4.   `beforeDestroy` （vue3 中是 unmounted）适合移除事件、定时器等等，否则可能会引起内存泄露的问题。

- 此外还有 keep-alive 组件的 activated 和 deactived
- 组件中如果有子组件的话
  - 会递归挂载子组件，所有子组件都挂载完毕后才会执行根组件的 `mounted`钩子。
  - 会递归销毁子组件，所有子组件都销毁完毕后才会执行根组件的 `destroyed` 钩子。




# 组件通信

- emit / props

- provide / inject

- EventBus 解决 (vue2)，利用的是 vue 的发布订阅模式，但是vue3中 \$on 被删除。

专门创建一个文件，里面导出一个空白的的 Vue 对象进行监听和触发事件。

~~~js
import eventBus from '../EventBus'
created() {
    eventBus.$on("send", (index) => {
      // 传递进来的数据是回调函数的参数
    });
  }
~~~

~~~js
import eventBus from '../EventBus'
eventBus.$emit("send", this.index) // 触发跨组件 send 事件
~~~

EventBus

~~~js
import Vue from 'vue'
// 空白vue对象
export default new Vue()
~~~



## 为什么 data 返回的是一个函数而非对象

复用一次组件，就会返回一份新的 data；而单纯的写成对象形式，就使得所有组件实例共用了一份 data。



## nextTick 的原理

- nextTick中的回调是在下次Dom更新循环结束之后执行的延迟回调，是一个微任务
- 它用于获取更新后的 DOM
- vue 的数据更新是异步的，因此 nextTick 保证了该逻辑会在更新之后执行



## computed 和 watch 的区别

computed 具有缓存。

watch 是一个值改变时的回调函数，适合触发异步操作。



## v-if 和 v-show 的区别

v-show ： 通过 display : none 实现的，适用于频繁切换显示与隐藏

v-if ：直接在 DOM 树中删除这个 DOM 元素。优点是可触发组件的生命周期函数



## computed 和 watch 区别

- watch 监听到值的变化就会执行回调，在回调中可以进行一些复杂的逻辑操作。
- computed 是计算属性，依赖其他属性计算值，并且 computed 的值有缓存，只有当计算值变化才会返回内容，它可以设置 getter 和 setter，一般用于渲染页面。



## 虚拟DOM和 diff 算法

虚拟 DOM：

在浏览器中操作 DOM 性能是很差的，而操作 js 对象的效率会高很多。因此虚拟 DOM 就是一个描述 DOM 的==js 对象==，然后再通过这个对象渲染出真实的 DOM 。

- 虚拟DOM 性能高，且不依赖于真实的平台
- 其中有一种 diff 算法，进行新旧虚拟 DOM 的对比。



diff 算法：

首先 DOM 是一个多叉树的结构，如果需要完整的对比两颗树的差异，那么需要的时间复杂度会是 O(n ^ 3)，这个复杂度肯定是不能接受的。于是 React / Vue 团队优化了算法，实现了 O(n) 的复杂度来对比差异。 

实现 O(n) 复杂度的关键就是只对比同层的节点。这也是因为在实际业务中很少会去跨层的移动 DOM 元素。 所以判断差异的算法就分为了两步：

- 首先进行树的深度遍历，这一步中会给每个节点添加索引，便于最后渲染差异。此外会进行新旧节点的相同判断，如果不同，代表节点被替换了；反之则没有修改，继续进行第二步操作。
- 一旦节点有子元素，就去判断子元素是否有不同，递归下去。



我们不会通过数据劫持这个操作取代diff算法，因为每个属性都添加 watcher 的话性能低下。

首次渲染大量 DOM 时，由于多了一层虚拟 DOM 的计算，会比 innerHTML 插入慢。因为 虚拟 DOM 的优势主要在于更新 DOM 元素。

