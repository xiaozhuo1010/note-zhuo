# BOM

> ​	BOM（Browser Object Model）即浏览器对象模型，它提供了独立于内容而与浏览器窗口进行交互的对象，其核心对象是 window。
>
> ​	BOM 由一系列相关的对象构成，并且每个对象都提供了很多方法与属性。
>
> ​	BOM 缺乏标准，JavaScript 语法的标准化组织是 ECMA，DOM 的标准化组织是 W3C，BOM 最初是Netscape 浏览器标准的一部分。
>
> ​	BOM 比 DOM 更大，它包含 DOM。（window包含document）
>

| DOM                     | BOM                                       |
| ----------------------- | ----------------------------------------- |
| 文档对象模型            | 浏览器对象模型                            |
| 把文档当作一个对象来看  | 把浏览器当一个对象看                      |
| DOM的顶级对象是document | BOM的顶级对象是Windows                    |
| DOM学习的是操作页面元素 | BOM学习的是浏览器窗口交互的对象           |
| DOM是W3C标准            | BOM是各厂商在各自浏览器定义的，兼容性较差 |



# window对象

> ​	window对象是浏览器的顶级对象。
>
> ​	首先他是js访问浏览器窗口的接口。
>
> ​	其次他是一个全局对象。全局变量和函数是window对象的属性和方法，但是调用时可以省略windows。
>



## 页面（窗口）加载事件

### load

~~~js
window.onload = function(){} ; 
window.addEventListener('load',function(){});
~~~

1. load 是窗口 (页面）加载事件，**当文档内容完全加载完成**会触发该事件(包括图像、脚本文件、CSS 文件等)并调用的处理函数。
2. 通过它可以把JS代码写到页面元素的上方。
3. windows.onload只能写一次，下面的会覆盖上面的（事件监听则没有限制）



### DOMContentLoaded

~~~js
document.addEventListener('DOMContentLoaded',function(){});
~~~

1. DOMContentLoaded 事件触发时，仅当DOM加载完成，不包括样式表，图片，flash等等。
2. IE9以上才支持！！！
3. 如果页面的图片很多的话, 从用户访问到onload触发可能需要较长的时间, 交互效果就不能实现，必然影响用户的体验，此时用 DOMContentLoaded 事件比较合适。
4. ==注意它的对象是document==



## 调整窗口大小事件

~~~js
window.onresize = function(){}; 
window.addEventListener('resize', function() {})
~~~

1. 只要窗口大小发生像素变化，就会触发这个事件。
2. 我们经常利用这个事件并配合 window.innerWidth （当前屏幕的宽度，没有单位）完成响应式布局。



## 定时器

### setTimeout() 定时炸弹

~~~js
setTimeout(回调函数, 延迟的毫秒数); 
clearTimeout(定时器的标识符); // 停止定时器
~~~



### setInterval() 反复提醒的闹钟

~~~js
setInterval(回调函数, 提醒间隔的毫秒数); 
clearInterval(定时器的标识符); // 停止定时器
~~~



### 案例：倒计时

![BOM_01.01](images/BOM_01.01.png)

1. 结构中设置三个盒子，装时、分、秒
2. 设置每秒一次的Interval定时器
3. 定时器的回调函数是之前计算距离设置时间还剩多少时分秒的案例：当前时间总的毫秒数减去设定时间的总毫秒数，再分别求出剩余多少时分秒，并修改盒子里的内容



## this指向问题

> ​	this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁，一般情况下this的最终指向的是那个调用它的对象。
>

现阶段，我们先了解一下几个this指向

1. 全局作用域或者普通函数中this指向全局对象window（如定时器里面的this指向window）
2. 方法调用中哪个对象调用this指向谁
3. 构造函数中this指向构造函数的实例



## location对象

> ​	window对象提供了location属性用于获取、设置或解析窗体的URL，返回的是一location个对象



### location 对象的属性

| location对象      | 返回值                          |
| ----------------- | ------------------------------- |
| location.href     | 获取或者设置整个URL             |
| location. host    | 返回主机(域名)                  |
| location.port     | 返回端口号如果未写返回空字符串  |
| location.pathname | 返回路径                        |
| location. search  | 返回参数                        |
| location.hash     | 返回片段#后面内容常见于链接锚点 |

重点是href和search



### location对象的方法

| location对象方法     | 返回值                                                       |
| -------------------- | ------------------------------------------------------------ |
| location.assign(）   | 跟href一样,可以跳转页面(也称为重定向页面)                    |
| location. replace(） | 替换当前页面,因为不记录历史,所以不能后退页面                 |
| location.reload()    | 重新加载页面,相当于刷新按钮或者f5如果参数为true强制刷新ctrl+f5 |



## navigator对象

> ​	window对象的 navigator 对象包含有关浏览器的信息，它有很多属性，我们最常用的是 userAgent，该属性可以返回由客户机发送服务器的 user-agent 头部的值。
>

~~~js
// 下面前端代码可以判断用户那个终端打开页面，实现跳转 
if((navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i))) {  
    window.location.href = "";   //手机 
} else {   
    window.location.href = "";   //电脑 
}
~~~



## history对象

> ​	window对象给我们提供了一个 history对象，与浏览器历史记录进行交互。该对象包含用户（在浏览器窗口中）访问过的URL。
>

| history对象方法   | 作用                                                        |
| ----------------- | ----------------------------------------------------------- |
| history.back()    | 可以后退功能                                                |
| history.forward() | 前进功能                                                    |
| history.go(参数)  | 前进后退功能。<br/>参数如果是1前进1个页面如果是1后退1个页面 |

history对象一般在实际开发中比较少用，但是会在一些 OA 办公系统中见到。



## 本地存储

> ​	随着互联网的快速发展，基于网页的应用越来越普遍，同时也变的越来越复杂，为了满足各种各样的需求，会经常性在本地存储大量的数据，HTML5规范提出了相关解决方案。

1、本地存储的数据存储在用户浏览器中

2、设置、读取方便、甚至页面刷新不丢失数据

3、容量较大，sessionStorage约5M、localStorage约20M

4、只能存储字符串，可以将对象转化成JSON格式编码后存储





# 元素偏移量 offset 系列

> ​	offset 翻译过来就是偏移量， 我们使用 offset系列相关属性可以动态的得到该元素的位置（偏移）、大小等。

1. 获得元素距离带有定位父元素的位置
2. 获得元素自身的大小（宽度高度）
3. 注意：返回的数值都不带单位

| offset系列属性        | 作用                                                         |
| --------------------- | ------------------------------------------------------------ |
| element. offsetParent | 返回该元素带有定位的父级元素<br/>如果父级都没有定位则返回body |
| element.offsetTop     | 返回元素相对带有定位父元素上方的偏移                         |
| element.offsetLeft    | 返回元素相对带有定位父元素左边框的偏移                       |
| element. offsetWidth  | 返回自身包括 padding、边框、内容区的宽度,返回数值不带单位    |
| element. offsetHeight | 返回自身包括 padding、边框、内容区的高度,返回数值不带单位    |

![BOM_02.01](../../../../../%E5%9F%BC%E7%8E%89/Desktop/%E7%AC%94%E8%AE%B0/%E7%9F%A5%E8%AF%86%E7%82%B9/DOM%E3%80%81BOM%E3%80%81JQ/images/BOM_02.01.png)





## offset 与 style 区别

#### offset

- offset 可以得到任意样式表中的样式值
- offset 系列获得的数值是没有单位的
- offsetWidth 包含padding+border+width
- offsetWidth 等属性是只读属性，只能获取不能赋值
- ==所以，我们想要获取元素大小位置，用offset更合适==

#### style

- style 只能得到行内样式表中的样式值，
- style.width 获得的是带有单位的字符串
- style.width 获得不包含padding和border 的值
- style.width 是可读写属性，可以获取也可以赋值
- ==所以，我们想要给元素更改值，则需要用style改变==





# 元素可视区 client 系列

> ​	client 翻译过来就是客户端，我们使用 client 系列的相关属性来获取元素可视区的相关信息。通过 client系列的相关属性可以动态的得到该元素的边框大小、元素大小等。

| client系列属性      | 作用                                                         |
| ------------------- | ------------------------------------------------------------ |
| element.clientTop   | 返回元素上边框的大小                                         |
| element. clientLeft | 返回元素左边框的大小                                         |
| element.clientWidth | 返回自身包括 padding、内容区的宽度,不含边框,返回数值不带单位 |
| element.clientHeigh | 返回自身包括 padding、内容区的高度,不含边框,返回数值不带单位 |

![BOM_02.02](../../../../../%E5%9F%BC%E7%8E%89/Desktop/%E7%AC%94%E8%AE%B0/%E7%9F%A5%E8%AF%86%E7%82%B9/DOM%E3%80%81BOM%E3%80%81JQ/images/BOM_02.02.png)





### 案例：淘宝 flexible.js 源码分析

#### 立即执行函数

 `(function(){})()  或者 (function(){}())`

主要作用： 创建一个独立的作用域。 避免了命名冲突问题



#### load和pageshow

下面三种情况都会刷新页面都会触发 load 事件。

1. a标签的超链接
2. F5或者刷新按钮（强制刷新）
3. 前进后退按钮

load事件有自己的局限性：

1. 火狐浏览器会有“往返缓存”，这个缓存中将整个页面都保存在了内存里。所以此时==火狐浏览器的后退按钮不能刷新页面==
2. 此时可以使用 ==pageshow==事件来触发。==这个事件在页面显示时触发，无论页面是否来自缓存。==在重新加载页面中，pageshow会在load事件触发后触发；
3. 可以根据事件对象中的persisted来判断是否是缓存中的页面触发的pageshow事件
4. 注意 pageshow 事件是 window的事件。



```js
(function flexible(window, document) {   
    // 获取的html 的根元素  
    var docEl = document.documentElement   
    // dpr 物理像素比   
    var dpr = window.devicePixelRatio || 1  
    // adjust body font size  设置我们body 的字体大小 
    function setBodyFontSize() {     
        // 如果页面中有body 这个元素 就设置body的字体大小   
        if (document.body) {  
            document.body.style.fontSize = (12 * dpr) + 'px'  
        } else {      
            // 如果页面中没有body 这个元素，则等着 我们页面主要的DOM元素加载完毕再去设置body的字体大小   
            document.addEventListener('DOMContentLoaded', setBodyFontSize)         }  
    }   
    setBodyFontSize();    
    // set 1rem = viewWidth / 10  
    设置我们html 元素的文字大小   
    function setRemUnit() {     
        var rem = docEl.clientWidth / 10    
        docEl.style.fontSize = rem + 'px'
    }   
    setRemUnit()    
    // reset rem unit on page resize 当我们页面尺寸大小发生变化的时候，要重新设置下rem 的大小  
    window.addEventListener('resize', setRemUnit)   
    // pageshow 是我们重新加载页面触发的事件
    window.addEventListener('pageshow', function(e) {    
        // e.persisted 返回的是true 就是说如果这个页面是从缓存取过来的页面，也需要从新计算一下rem 的大小   
        if (e.persisted) {  
            setRemUnit()     
        }  
    })
    // detect 0.5px supports  有些移动端的浏览器不支持0.5像素的写法 
    if (dpr >= 2) {  
        var fakeBody = document.createElement('body')   
        var testElement = document.createElement('div')    
        testElement.style.border = '.5px solid transparent' 
        fakeBody.appendChild(testElement)         
        docEl.appendChild(fakeBody)     
        if (testElement.offsetHeight === 1) {    
            docEl.classList.add('hairlines') 
        }     
        docEl.removeChild(fakeBody)   
    }
}(window, document))
```





# 元素滚动 scroll 系列

> ​	scroll 翻译过来就是滚动的，我们使用 scroll 系列的相关属性可以动态的得到该元素的大小、滚动距离等。
>
> ​	滚动条在滚动时会触发 scroll事件。
>
> ​	注意，元素被卷去的头部是element.scrollTop  , 如果是**页面被卷去的头部 则是** **window.pageYOffset**

## 元素被卷去的头部

![BOM_02.03](../../../../../%E5%9F%BC%E7%8E%89/Desktop/%E7%AC%94%E8%AE%B0/%E7%9F%A5%E8%AF%86%E7%82%B9/DOM%E3%80%81BOM%E3%80%81JQ/images/BOM_02.03.png)

​	scrollTop的值如下图，并不能表示其在页面中是否被滚走

![BOM_02.04](../../../../../%E5%9F%BC%E7%8E%89/Desktop/%E7%AC%94%E8%AE%B0/%E7%9F%A5%E8%AF%86%E7%82%B9/DOM%E3%80%81BOM%E3%80%81JQ/images/BOM_02.04.png)





## 页面被卷去的头部

> ​	如果浏览器显示窗口的高（或宽）度不足以显示整个页面时，会自动出现滚动条。当滚动条向下滚动时，页面上面被隐藏掉的高度，我们就称为页面被卷去的头部。

​	页面被卷去的头部：可以通过window.pageYOffset 获得  如果是被卷去的左侧window.pageXOffset

​	body跟html的scrollTop是不一样的，body跟在html文件中的盒子一样，并不能代表页面被卷去的头部，而是元素被卷去的头部；html是document在浏览器的显示窗口中，所以==html.scrollTop 等价于 window.pageYOffset==



# 总结

他们主要用法：

- offset系列 经常用于获得元素位置    offsetLeft  offsetTop
- client经常用于获取元素大小（它不包含边框）  clientWidth clientHeight
- scroll 经常用于获取滚动距离 scrollTop  scrollLeft  
- 注意页面滚动的距离通过 window.pageXOffset  window.pageYOffset获得