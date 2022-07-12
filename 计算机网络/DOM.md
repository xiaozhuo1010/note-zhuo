# DOM

> ​	文档对象模型（Document Object Model，简称DOM），是W3C组织推荐的处理可扩展标记语言（html或者xhtml）的标准编程接口。
>
> ​	W3C 已经定义了一系列的 DOM 接口，通过这些 DOM 接口可以**改变网页的内容、结构和样式**。



## DOM操作元素

> ​	dom针对于**元素**的操作，主要有创建、增、删、改、查、自定义属性操作、事件操作。

- 创：创建

- 1. document. write
  2. innerHTML
  3. createElement

- 增： 增加

- 1. appendChild
  2. insertBefore

- 删：删除

- 1. removeChild

- 改：修改

- 1. 修改元素属性:src、href、 title等
  2. 修改普通元素内容: innerHTML、 innerText
  3. 修改表单元素: value、type、 disabled等
  4. 修改元素样式: style、 className

- 查：查询/获取

- - dom提供的AP方法: getElementByld、 getElementsByTagName    古老用法不太推荐
  - h5提供的新方法: querySelector、 querySelectorAll    提倡
  - 利用节点操作获取元素:父(parentNode)、子(children)、兄(previousElementsibling、nextElementSibling)  提倡

- 自定义属性操作

- 1. setAttribute:设置dom的属性值
  2. getAttribute:得到dom的属性值
  3. removeAttribute移除属性



# 获取元素

~~~js
// 1、根据ID获取
document.getElementById(id) 
	// 返回值：元素对象 或 null

// 2、根据标签名获取
document.getElementsByTagName('标签名') 
	// 得到文档里面的某些标签 
	// 返回值：元素对象集合（伪数组的形式存储，其元素是元素对象） 
	// 得到的元素对象是动态的

// 3、H5 新增获取元素方式
document.querySelector('选择器'); 
	// 根据选择器返回第一个元素对象。如'.box' 、'#nav'、'li' 
document.querySeletorAll('选择器'); 
	// 根据选择器返回所有符合条件的元素对象

// 4、获取body，html
document.body 
	// 返回body元素对象 
document.documentiElement
	// 返回html元素对象
~~~

### 

# 操作元素

> ​	JavaScript的DOM 可以操作元素来改变元素里面的内容、属性等。（注意：这些操作都是通过修改元素对象的属性实现的）

## 操作元素内容

~~~js
element.innerText 
element.innerHTML  // 常用 

// 1、获取内容时的区别： 
// innerText会去除空格和换行，而innerHTML会保留空格和换行 
// 2、设置内容时的区别： 
// innerText不会识别html标签，而innerHTML会识别
~~~



## 操作常用元素的属性

- 获取属性的值：元素对象.属性名
- 设置属性的值：元素对象.属性名 = 值



## 操作表单元素的属性

- 表单元素中有一些属性如：disabled、checked、selected 的值是布尔型。

- 修改表单内容通过value实现而非innerHtml。



## 操作样式属性

> ​	我们可以通过 JS 修改元素样式。

~~~js
element.style  // 添加行内样式操作。权重较高 
element.className // 添加或修改类名样式操作。样式较多时使用，注意会覆盖原来的class而非添加
~~~

注意：

1. ==Js里的样式采用驼峰命名法==，如：fontSize而不是font-size
2. 元素对象的style属性也是一个对象。元素对象.style.样式属性 = 值;



## 排他思想

​	如果有同一组元素，我们想要某一个元素实现某种样式的同时取消原先的元素的样式，就需要用到循环的排他思想算法：

1. 所有元素全部清除样式（干掉所有人）
2. 给当前元素设置样式 （然后我再上）
3. 注意顺序不能颠倒，首先干掉所有人，再自己上

~~~js
// 1. 获取所有按钮元素      
var btns = document.getElementsByTagName('button');  
// btns得到的是伪数组  遍历里面的每一个元素 btns[i]    
for (var i = 0; i < btns.length; i++) {    
    btns[i].onclick = function() {     
        // (1) 我们先把所有的按钮背景颜色去掉  干掉所有人 
        for (var i = 0; i < btns.length; i++) {   
            btns[i].style.backgroundColor = '';     
        }        
        // (2) 然后才让当前的元素背景颜色为pink 我再自己上
        this.style.backgroundColor = 'pink';  
    }
}
~~~



# 自定义属性操作

> ​	自定义属性目的：是为了保存并使用数据。有些数据可以保存到页面中而不用保存到数据库中。

## 获取属性值

~~~js
// 获取元素的属性值   
// element.属性   
console.log(div.id);

// 获取自定义或内置属性值 element.getAttribute('属性')   
console.log(div.getAttribute('index'));
~~~



## 设置属性值

~~~js
// 设置元素属性值  
// element.属性= '值' 
div.id = 'test';     
div.className = 'navs';    

// element.setAttribute('属性', '值');  主要针对于自定义属性   
div.setAttribute('index', 2);   
div.setAttribute('class', 'footer'); 
// class 特殊  这里面写的就是class 不是className
~~~



## 移除属性

~~~js
// 移除属性 removeAttribute       
div.removeAttribute('index');
~~~



## H5自定义属性

> ​	有些自定义属性很容易引起歧义，不容易判断是元素的内置属性还是自定义属性。
>
> ​	所以H5有个规范：自定义属性以data-开头
>
> ​	H5新增了获取自定义属性的方法：dataset

~~~js
// 兼容性获取    
console.log(div.getAttribute('data-index'));   

// h5新增的获取自定义属性的方法（ie11） 它只能获取data-开头的属性 
// dataset 是一个集合里面存放了所有以data开头的自定义属性（以对象的形式） 
console.log(div.dataset);   
console.log(div.dataset.index); 
console.log(div.dataset['index']);

// 如果自定义属性里面有多个-链接的单词，我们获取的时候采取 驼峰命名法 
console.log(div.dataset.listName);      
console.log(div.dataset['listName']);
~~~





# 事件操作

> ​	JavaScript 使我们有能力创建动态页面，而事件是可以被 JavaScript 侦测到的行为。即 **触发--— 响应机制**。

## 事件三要素

- 事件源：触发事件的元素
- 事件类型： 触发时间的条件
- 事件处理程序：事件触发后执行的代码(函数赋值形式)



## 回调函数的概念

> ​	一般情况下，应用程序（application program）会时常通过API调用库里所预先备好的函数。
>
> ​	而有些库函数（library function）却要求应用先传给它一个函数，好在合适的时候调用，以完成目标任务。这个被传入的、后又被调用的函数就称为**回调函数**（callback function）
>
> ​	如鼠标点击会发生事件，这就是API里预先设置好的函数，但是会发生什么是由回调函数决定的。

​	打个比方，有一家旅馆提供叫醒服务，但是要求旅客自己决定叫醒的方法。可以是打客房电话，也可以是派服务员去敲门。这里，“叫醒”这个行为是旅馆提供的，相当于库函数，但是叫醒的方式是由旅客决定并告诉旅馆的，也就是**回调函数**。而旅客告诉旅馆怎么叫醒自己的动作，也就是把回调函数传入库函数的动作，称为**登记回调函数**（to register a callback function）





## 注册事件

### 注册事件两种方式

#### 传统注册方式

`eventTarget.onclick = function(){}`

1. 利用on开头的事件
2. 注册事件的唯一性：同一个元素的同一个事件，只能设置一个处理函数。再设置会进行覆盖



#### 事件监听

`eventTarget.addEventListener(type, listener，[useCapture])（IE9以后支持）`

- type ： 事件类型字符串，注意不带on
- listener：事件处理函数（监听函数）
- useCapture ： 布尔值，默认是false，表示是在捕获阶段（true）或者冒泡阶段（fasle）调用事件处理函数

​	该方法将指定的监听器注册到 eventTarget（目标对象）上，当该对象触发指定的事件时，就会执行事件处理函数。





`eventTarget.attacheEvent(eventNameWithOn, callback)事件监听（IE678支持）`

​	该方法将指定的监听器注册到 eventTarget（目标对象）上，当该对象触发指定的事件时，指定的回调函数就会被执行。注意事件字符串是带on的（不推荐使用）



```js
var btns = document.querySelectorAll('button');  
// 1. 传统方式注册事件   
btns[0].onclick = function() {  
    alert('hi');  
}  
// 2. 事件侦听注册事件 addEventListener 
// (1) 里面的事件类型是字符串 必定加引号 而且不带on  
// (2) 同一个元素 同一个事件可以添加多个侦听器（事件处理程序）     
btns[1].addEventListener('click', function() {    
    alert(22);
})  
btns[1].addEventListener('click', function() {  
    alert(33);  
})    
// 3. attachEvent ie9以前的版本支持   
btns[2].attachEvent('onclick', function() {   
    alert(11);   
}) 
```



​	事件监听兼容性解决方案：

```js
 function addEventListener(element eventName, fn){
//判断当前浏览器是否支持 addEventListener方法
 if (element. addEventListener){
 element. addEventListener(eventName,fn);
 } else if (element. attachEvent){
 element. attachEvent ('on'+ eventName, fn);
 } else{
//相当于element. onclick=fn;
 element['on+ eventName]=fn
         }
 }
```







### 注册事件三要素

- 事件源：触发事件的元素
- 事件类型： 触发时间的条件
- 事件处理程序：事件触发后执行的代码(函数赋值形式)

**案例代码**

```js
// 点击一个按钮，弹出对话框   
//(1) 获取事件源 事件被触发的对象——按钮    
var btn = document.getElementById('btn');  
//(2) 事件类型： 触发时间的条件。比如鼠标点击(onclick) 
//(3) 事件处理程序  通过一个匿名函数赋值的方式完成   
btn.onclick = function() {      
    alert('点秋香'); 
}
```



### 常见的鼠标、键盘事件

| 鼠标、键盘事件             | 触发条件                                               |
| -------------------------- | ------------------------------------------------------ |
| onclick                    | 鼠标点击左键触发                                       |
| onmouseover / onmouseenter | 鼠标经过触发                                           |
| onmouseout / onmouseleave  | 鼠标离开触发                                           |
| onfocus                    | 获得鼠标焦点触发                                       |
| onblur                     | 失去鼠标焦点触发                                       |
| onmousemove                | 鼠标移动触发                                           |
| onmouseup                  | 鼠标弹起触发                                           |
| onmousedown                | 鼠标按下触发                                           |
|                            |                                                        |
| onkeyup                    | 键盘按键松开时触发                                     |
| onkeydown                  | 键盘按键按下时触发                                     |
| onkeypress                 | 键盘按键按下时触发（但是不识别功能键，区分字母大小写） |

#### mouseenter和mouseover的区别

- mouseover 鼠标经过自身盒子会触发，经过子盒子还会触发。
- mouseenter  只会经过自身盒子触发。因为mouseenter不会冒泡
- 跟mouseenter  搭配   鼠标离开 ==> mouseleave  ，  同样不会冒泡



#### 使用e.keyCode属性判断用户按下哪个键

```js
// 键盘事件对象e中的keyCode属性可以得到相应键的ASCII码值    
// 我们可以利用keycode返回的ASCII码值来判断用户按下了那个键   
if (e.keyCode === 65) {      
    alert('您按下的a键');  
} else {      
    alert('您没有按下a键') 
}
```







## 删除事件（解绑事件）

```js
// 1. 传统方式删除事件    
divs[0].onclick = null;
// 2. removeEventListener 删除事件 
divs[1].addEventListener('click', fn) // 里面的回调函数就不能使用匿名函数了，不然不便删除
function fn() {       
    alert(22);      
    divs[1].removeEventListener('click', fn);  
}   
// 3. detachEvent    
divs[2].attachEvent('onclick', fn1);  
function fn1() {     
    alert(33);      
    divs[2].detachEvent('onclick', fn1);   
}
```



### 删除事件兼容性解决方案

```js
 function removeEventListenerelement, eventName, fn){
//判断当前浏览器是否支持 removeEventListener方法
	 if (element. removeEventListener){
 		element. removeEventListener(eventName,fn);
	 }//第三个参数默认是
	 else if (element. detachEvent){
		 element. detachEvent('on'+ eventName, fn);
	 }
 	else{
		 element['on' eventName] =null;
	}
}
```



# DOM事件流

> ​	html中的标签都是相互嵌套的，当你单击一个div时，同时你也单击了div的父元素，甚至整个页面。
>
> ​	事件流描述的是从页面中接收事件的顺序
>
> ​	事件发生时会在元素节点之间按照特定的顺序传播，这个传播过程即DOM事件流



## DOM 事件流3个阶段

1. 捕获阶段
2. 当前目标阶段
3. 冒泡阶段

![DOM_06](../../../../../%E5%9F%BC%E7%8E%89/Desktop/%E7%AC%94%E8%AE%B0/%E7%9F%A5%E8%AF%86%E7%82%B9/DOM%E3%80%81BOM%E3%80%81JQ/images/DOM_06.png)

> ​	我们向水里面扔一块石头，首先它会有一个下降的过程，这个过程就可以理解为从最顶层向事件发生的最具体元素（目标点）的捕获过程；
>
> ​	之后会产生泡泡，会在最低点（ 最具体元素）之后漂浮到水面上，这个过程相当于事件冒泡。
>
> ​	JS代码只能执行捕获或者冒泡其中的一个阶段

- onclick 和 attachEvent（ie）只能在冒泡阶段触发
- addEventListener 看最后一个参数：
  - true 表示在事件捕获阶段调用事件处理程序；
  - false 表示在事件冒泡阶段调用事件处理程序（默认）



```html
<div class="father">    
       <div class="son">son盒子</div>  
</div> 
<script>  
    // onclick 和 attachEvent（ie） 只能在冒泡阶段触发   
    // 冒泡阶段 addEventListener 第三个参数是 false 或者 省略   
    // son -> father ->body -> html -> document   
    var son = document.querySelector('.son'); 
    // 给son注册单击事件  
    son.addEventListener('click', function() {    
        alert('son');    
    }, false); 
    // 给father注册捕获阶段触发单击事件   
    var father = document.querySelector('.father'); 
    father.addEventListener('click', function() {  
        alert('father');     
    }, true); 
</script>
```





# 事件对象

> ​	事件发生后，跟事件相关的一系列信息数据的集合都放到这个对象里面，这个对象就是事件对象。
>
> ​	事件触发发生时就会产生事件对象，并且系统会以实参的形式传给事件处理函数。
>
> ​	所以，在事件处理函数中声明1个形参用来接收事件对象。



```js
div.onclick = function(e){
    // e就是事件对象
}
div.addEventListener('click',function(e){
    // e就是事件对象
})
```





## 事件对象的兼容性处理

 事件对象本身的获取存在兼容问题：

1. 标准浏览器中是浏览器给方法传递的参数，只需要定义形参 e 就可以获取到。

2. 在 IE6~8 中，浏览器不会给方法传递参数，如果需要的话，需要到 window.event 中获取查找。

   解决：

` e = e || window.event;“||”前面为false, 返回 “||” 后面的值。“||”前面为true, 返回 “||” 前面的值。`



## 事件对象的属性和方法

| 事件对象属性和方法    | 说明                                                  |
| --------------------- | ----------------------------------------------------- |
| e.target              | 返回触发事件的对象（标准）                            |
| e.srcElement          | 返回触发事件的对象（非标准）                          |
| e.type                | 返回事件的类型（如  click）                           |
| e.cancelBubble        | 组织冒泡，非标准 ie6-8使用                            |
| e.returnValue         | 该属性阻止默认事件，非标准，ie6-8使用，如阻止链接跳转 |
|                       |                                                       |
| e.preventDefault();   | 该方法阻止默认事件，DOM标准写法，如阻止链接跳转       |
| e.stopPropagation（） | 阻止冒泡，标准                                        |

​		==return false 也能阻止默认行为==



#### e.target 和 this 的区别

- this 是事件绑定的元素（绑定这个事件处理函数的元素） 。

- e.target 是事件触发的元素。

- 常情况下terget 和 this是一致的，但有一种情况不同，那就是在事件冒泡（事件委托）时。单击子元素，父元素的事件处理函数也会被触发

  - 这时候this指向的是父元素，因为它是==绑定事件的元素对象==
  - 而target指向的是子元素，因为他是==触发事件的具体元素对象==

  ```html
  <ul>    
          <li>abc</li>  
          <li>abc</li>  
          <li>abc</li>   
  </ul>    
  <script>   
      var ul = document.querySelector('ul');  
      ul.addEventListener('click', function(e) {   
          // 我们给ul 绑定了事件  那么this 就指向ul   
          console.log(this);    // ul     
          // e.target 触发了事件的对象 我们点击的是li e.target 指向的就是li  
          console.log(e.target);   // li      
      });
  </script>
  ```



#### 阻止默认跳转

> ​	html中一些标签有默认行为，例如a标签被单击后，默认会进行页面跳转。

```html
<a href="http://www.baidu.com">百度</a> 
<script>   
    // 2. 阻止默认行为 让链接不跳转 
    var a = document.querySelector('a');
    a.addEventListener('click', function(e) {   
        e.preventDefault();  //  dom 标准写法  
    });    
    // 3. 传统的注册方式
    a.onclick = function(e) {
        // 普通浏览器 e.preventDefault();  方法  
        e.preventDefault();   
        // 低版本浏览器 ie678  returnValue  属性   
        e.returnValue = false;   
        // 我们可以利用return false 也能阻止默认行为 没有兼容性问题 
        return false;     
    }
```





#### 阻止事件冒泡

```js
// 给son注册单击事件，同时阻止冒泡影响父元素     
son.addEventListener('click', function(e) {  
    alert('son');      
    e.stopPropagation();
    // 阻止事件冒泡
}, false);

// 兼容性写法
window.event.cancelBubble = true; // 非标准 (兼容性ie678)
```



# 事件委托

> ​	事件委托也称为事件代理，在 jQuery 里面称为事件委派。
>
> ​	说白了就是，不给子元素注册事件，给父元素注册事件，把处理代码在父元素的事件中执行。

- js事件中的代理： 如在ul 中有很多 li 
  - 按照传统方法，每个li都绑定点击事件，不仅动态生成的 li 标签无法绑定，而且访问DOM的次数更多，交互就绪时间慢
  - 事件委托：给 ul（父元素） 添加侦听器，利用事件冒泡。当子元素的事件触发，会冒泡到父元素，然后去操作响应的子元素





### 事件委托的作用

- 我们只操作了一次 DOM ，提高了程序的性能。
- 动态新创建的子元素，也能拥有事件。

```js
// 事件委托的核心原理：给父节点添加侦听器， 利用事件冒泡影响每一个子节点   
var ul = document.querySelector('ul');    
ul.addEventListener('click', function(e) { 
    // e.target 这个可以得到我们点击的对象，而非this
    e.target.style.backgroundColor = 'pink';  
})
```



# 节点操作

节点：DOM 树里每一个元素都是一个节点。

节点分为元素节点，属性节点，文本节点

~~~js
// 父节点
DOM.parentNode
// 子节点
DOM.children
// 查找兄弟节点
DOM.nextElementSibling // 下一个节点
DOM.previousElementSibling // 上一个节点
// 增加节点
document.createElement('标签名')
// 将节点插入到指定元素的最后一位中
DOM.appendChild('要插入的元素')
// 将节点插入到指定元素的前一位中
DOM.appendChild('要插入的元素', '在哪一个元素前面')
// 删除节点
DOM.removeChild('要删除的元素')
~~~



#  移动端触屏事件

> ​	移动端浏览器兼容性较好，我们不需要考虑以前 JS 的兼容性问题，可以放心的使用原生 JS 书写效果。	但是移动端也有自己独特的地方。比如触屏事件 touch（也称触摸事件），Android 和 IOS 都有。touch 对象代表一个触摸点。触摸点可能是一根手指，也可能是一根触摸笔。触屏事件可响应用户手指（或触控笔）对屏幕或者触控板操作。

常见的触屏事件：

- touchstart 手指摸到一个DOM元素时触发
- touchmove 手指在一个DOm元素上滑动时触发
- touchend 手指从一个DOM元素上移开时触发



## 触摸事件对象（TouchEvent）

> ​	TouchEvent 是一类描述手指在触摸平面（触摸屏、触摸板等）的状态变化的事件。这类事件用于描述一个或多个触点，使开发者可以检测触点的移动，触点的增加和减少，等等

​	触摸事件对象（e）重点我们看三个常见对象列表（注意是列表，返回的是为数组）：

- touches 正在触摸屏幕的所有手指的一个列表
- targetTouches 正在触摸当前DOM元素上的手指的一个列表
- changedTouches 手指状态发生了改变的列表，从无到有，从有到无变化

因为平时我们都是给元素注册触摸事件，所以重点记住 targetTocuhes



## 移动端拖动元素

1. touchstart、touchmove、touchend 可以实现拖动元素
2. 拖动元素需要当前手指的坐标值 我们可以使用  targetTouches[0] 里面的pageX 和 pageY
3. 手指移动的距离： 手指滑动中的位置 减去 手指刚开始触摸的位置
4. 移动端拖动：手指移动中，计算出手指移动的距离。然后用盒子原来的位置 + 手指移动的距离

拖动元素三步曲：

1. 触摸元素 touchstart： 获取手指初始坐标，同时获得盒子原来的位置
2. 移动手指 touchmove： 计算手指的滑动距离，并且移动盒子
3. 离开手指 touchend:

- 注意： 手指移动也会触发滚动屏幕所以这里要阻止默认的屏幕滚动 e.preventDefault();



## classList 属性

> ​	classList属性是HTML5新增的一个属性，返回元素的类名。但是ie10以上版本支持。该属性用于在元素中添加，移除及切换 CSS 类。

```js
element.classList.add('类名'); // 添加类 
element.classList.remove('类名'); // 移除类 
element.classList.toggle('类名'); //切换类（如果没有这个类就添加，有就移除）
```



## click 延时解决方案

移动端 click 事件会有 300ms 的延时，原因是移动端屏幕双击会缩放(double tap to zoom) 页面。

解决方案：

- 禁用缩放。 浏览器禁用默认的双击缩放行为并且去掉300ms 的点击延迟。

<metaname="viewport"content="user-scalable=no">

- 利用touch事件自己封装函数来代替'click'事件，解决300ms 延迟。
- - 当我们手指触摸屏幕，记录当前触摸时间
  - 利用手指移动事件判断是否滑动屏幕，有的话就不算点击
  - 当我们手指离开屏幕， 用离开的时间减去触摸的时间
  - 如果时间小于150ms，并且没有滑动过屏幕， 那么我们就定义为点击（执行回调函数）

代码如下:

```js
//封装tap，解决click 300ms 延时 
function tap (obj, callback) {  
    var isMove = false;   
    var startTime = 0; // 记录触摸时候的时间变量  
    obj.addEventListener('touchstart', function (e) { 
        startTime = Date.now(); // 记录触摸时间 
    });     
    obj.addEventListener('touchmove', function (e) {  
        isMove = true; 
        // 看看是否有滑动，有滑动算拖拽，不算点击  
    });     
    obj.addEventListener('touchend', function (e) {
        if (!isMove && (Date.now() - startTime) < 150) { 
            // 如果手指触摸和离开时间小于150ms 算点击  
            callback && callback(); // 执行回调函数   
        }        
        isMove = false;  //  取反 重置  
        startTime = 0;    
    }); 
}
//调用  
tap(div, function(){   // 执行代码  });
```

- 使用插件。fastclick 插件解决300ms 延迟。

```js
// 引入插件
<script type='application/javascript' src='/path/to/fastclick.js'>  </script> 

if ('addEventListener' in document) { 
    document.addEventListener('DOMContentLoaded', function() { 
        FastClick.attach(document.body); 
    }, false); 
}   // 引入这段代码，页面中的click事件就都没有延迟了
```





# Ajax

在网页中利用XMLHttpRequest对象和服务器进行数据交互的方式，就是Ajax。



## 原生 js 的 ajax

- XMLHttpRequest（简称xhr）是Javascript对象，可以发起Ajax请求，请求服务器上的资源。

- xhr对象的readyState属性用来表示**当前Ajax请求所处的状态**。

| ajax.readyState的值 | 描述             | 描述                                               |
| ------------------- | ---------------- | -------------------------------------------------- |
| 0                   | UNSENT           | XMLHttpRequest 对象已被创建，但尚未调用 open方法。 |
| 1                   | OPENED           | open() 方法已经被调用。                            |
| 2                   | HEADERS_RECEIVED | send() 方法已经被调用且响应头已经被接收。          |
| 3                   | LOADING          | 正在解析响应内容                                   |
| 4                   | DONE             | 一次Ajax 请求完成（不论成功与否）                  |

- 注意：http 状态码是通过 `ajax.status`获得的；ajax 的状态值是 `ajax.readyState`获得的



## 使用原生的 XHR 对象发起 ajax 请求

- get 请求（注意 get 请求的参数是通过查询字符串的形式传输的）

~~~js
// 1. 创建 XHR 对象
let xhr = new XMLHttpRequest();
// 2. 调用 open 方法，指定请求方式与 URL 地址
xhr.open('GET', 'http://www.example.com');
// 3. 监听请求状态改变的事件，注意xhr对象不是DOM对象，无法添加事件侦听器
xhr.onreadyStatechange = function(){
    // 因为状态值每次改变都会触发这个事件，因此需要判断
    if(xhr.readyState === 4 && xhr.status === 200){
        // do something... 
    }
}
// 设置请求失败时的监听函数
xhr.onerror = function() {
  console.error(this.statusText);
};
~~~



- post 请求

~~~js
// 1. 创建 xhr 对象
let xhr = new XMLHttpRequest();
// 2. 调用 open 方法，填入请求类型和URL地址
xhr.open('POST'， 'http://www.example.com');
// 3. 设置 http 请求头 和 请求体
// send 方法的参数就是请求体中的数据
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded') 
xhr.send('bookname=水浒传&author=施耐庵&publisher=天津图书出版社')
// 4. 监听请求状态的改变事件
xhr.onreadyStatechange = function(){
    // 因为状态值每次改变都会触发这个事件，因此需要判断
    if(xhr.readyState === 4 && xhr.status === 200){
        // do something... 
    }
}
// 设置请求失败时的监听函数
xhr.onerror = function() {
  console.error(this.statusText);
};
~~~



## XHR level2 

1. 新版 XHR 对象的 timeout 属性可以设置 http 请求时限。ontimeout 属性存储回调函数

~~~js
xhr.timeout = 3000 
xhr.ontimeout = function(event){  
    alert('请求超时！')
}
~~~

2. 可以上传 HTML5 新增的 FromData 对象模拟表单的数据，与直接提交表单数据无异，只是需要注意发送数据的格式不一样。

~~~js
// 新建 FormData 对象   
var fd1 = new FormData()   
// 如果参数填了某表单，会自动将该表单数据填充到 FormData 对象中  
var fd2 = new FormData(form)     
// 可以为 FormData 添加表单项  
fd1.append('uname', 'zs') 
fd1.append('upwd', '123456')  
// FromData 对象可以被当成请求体进行发送，注意请求头中数据格式设置成 FromData 格式
xhr.setRequestHeader('Content-Type', 'multipart/form-data')
xhr.send(fd1)
~~~

3. 可以上传文件，但是只能通过 FromData 对象及其数据格式上传。

~~~html
<!-- html 结构中需要先定义一个文件上传的表单元素 -->
	<input type="file" id="file1" />
    <button id="btnUpload">上传文件</button>
~~~

~~~js
// 监听按钮的点击事件
var btnUpload = document.querySelector('#btnUpload') 
btnUpload.addEventListener('click', function() {
    // 通过上传文件的表单元素的 files 属性获取到该需要上传的文件
    var files = document.querySelector('#file1').files  
    if (files.length <= 0) {    
        return alert('请选择要上传的文件！') 
    }
    else{     
            // 3、创建 FormData 对象，向其追加文件 
            var fd = new FormData()；
            fd.append('avatar', files[0]) 
            var xhr = new XMLHttpRequest()      
            xhr.open('POST',     
					'http://www.liulongbin.top:3006/api/upload/avatar')
        	xhr.setRequestHeader('Content-Type', 'multipart/form-data')
            xhr.send(fd)
            xhr.onreadystatechange = function() {
                if (xhr.readyState === 4) { 
                    let data = JSON.parse(xhr.responseText)  
                    if (xhr.status === 200) {   
                        // do something... 
                    } else {
                         // 上传文件失败   
                        console.log(data.message)     
                    }  
                }  
            } 
        }
}
~~~

4. 只有 level 2 的 XHR 对象才支持跨域的发起 ajax 请求。注意：请求能否成功也与服务器、浏览器有关。
   - 服务器方面：得允许接受该跨域请求
   - 浏览器方面，浏览器能接收到服务器跨域响应的数据，但是会默认采用同源策略对该数据进行拦截，使得我们无法获取到该数据。

5. 进度信息：

~~~js
function updateProgress(e) {
    // event.total是需要传输的总字节，event.loaded是已经传输的字节。如果event.lengthComputable不为真，则event.total等于0。
　　　　if (e.lengthComputable) {
　　　　　　console.log(e.loaded / e.total);
　　　　}
　　}
// 对下载的 progress 事件进行监听
xhr.onprogress = updateProgress;
// 对上传的事件进行监听
xhr.upload.onprogress = updateProgress;
~~~

~~~js
// 此外，还有几个事件可供监听
load事件：传输成功完成。
abort事件：传输被用户取消。
error事件：传输中出现错误。
loadstart事件：传输开始。
loadEnd事件：传输结束，但是不知道成功还是失败。
~~~





