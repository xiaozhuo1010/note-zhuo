## 大纲

- 数据类型判断
- 继承
- 数组去重
- 数组扁平化
- 深浅拷贝
- 发布订阅模式
  - 该类实现 on 订阅方法，off 取消订阅，emit 触发事件并执行订阅方法，支持传参。
- 解析 url 为对象



















## 数据类型判断

概要：使用

~~~js
function typeof (obj){
    // 通过 Object.toString实现
    return Object.prototype.toString.call(obj).slice(8, -1).toLowerCase()
}
~~~



## 几种继承方式

组合式继承

~~~js
function SonExtendFather(Father){
    function Son (){
    	Father.call(this);
	}
	Son.prototype = new Father();
	Son.prototype.constructor = Son;
    return Son;
}
~~~

寄生式继承：子实例的原型是  父实例 变为 寄生了父构造函数的一个空对象

```js
function SonExtendFather(Father){
    function Son (){
    	Father.call(this);
	}
	Son.prototype =  Object.create(Father.prototype);
	Son.prototype.constructor = Son;
    return Son;
}
```



## 数组去重

~~~js
// 1.  遍历去重(还可以使用 include 等 api ，其实相当于双重循环)
function unique(arr) {
    return arr.filter((ele, ind, arr) => (arr.indexOf(ele) === ind))
}
// 2. 使用 set
const unique = arr => Array.from(new Set(arr))
~~~



## 数组扁平化

将多层数组拍平成一层

~~~js
// 1. es6
function flatten(arr){
    while(arr.some(item => Array.isArray(item))){
        // concat 里使用展开符，相当于将 arr 的每一项都作为单独的参数传入
        // 如 [1, 1, [2]] 
        // [].concat(...arr) => [].concat(1, 1, [2]);
        arr = [].concat(...arr)
    }
    return arr;
}

// 2. 递归
function flatten(arr) {
    var result = [];
    if(!Array.isArray(arr)){
        result.push(arr);
    }else{
        for (var i = 0; i < arr.length; i++) {
            if (Array.isArray(arr[i])) {
                result = result.push(...flatten(arr[i]))
            } else {
                result.push(arr[i])
            }
    	}
    }
    return result;
}
~~~





## 深浅拷贝

浅拷贝：在 js 中有很多 API 实现了浅拷贝

1. 可以通过 `Object.assign` 方法进行浅拷贝。`Object.assign（target,source)`方法会拷贝source对象所有的属性值到target对象中，如果属性值是复杂类型的话，拷贝的是地址。然后返回合并后的target对象

```js
let a = {
  age: 1
}
let b = Object.assign({}, a)
a.age = 2
console.log(b.age) // 1
```

2. 可以通过展开运算符实现浅拷贝

```js
let a = {
  age: 1
}
let b = { ...a }
a.age = 2
console.log(b.age) // 1
```

3. 利用for-in循环遍历来手动浅拷贝。注意for-in循环会遍历对象原型上的自定义属性和方法，因此使用 hasOwnProperty 配合

```js
function shallowCopy(source) {
    if (!source || typeof source !== 'object') {
        return;
    }
    // 考虑复制数组还是对象
    let clone = Array.isArray(source) ? [] : {};
    for (var key in source) {
        if (source.hasOwnProperty(key)) {
            clone[key] = source[key];
        }
    }
    return clone
```





深拷贝：**浅拷贝只解决了第一层的问题**，如果第一层对象的属性值中还有对象的话，也只会复制指针。

1. 通过 `JSON.parse(JSON.stringify(object))` 来解决。但这种方法有局限性：
   - 会忽略 undefined。
   - 会忽略 symbol。
   - 无法拷贝函数
   - 无法拷贝一个引用自身的对象， 比如 `let shit = { name: 'xxx' }; shit.myself = shit; ` 如果 JSON 写的就会无限调用自身最后执行栈溢出  

```js
let a = {
  age: 1,
  jobs: {
    first: 'FE'
  }
}
let b = JSON.parse(JSON.stringify(a))
a.jobs.first = 'native'
console.log(b.jobs.first) // FE
```

2. 利用for-in循环遍历来手动深拷贝，此外加一层判断是否是引用类型，如果是就进行递归。

```js
function deepCopy(source) {
    if (!source || typeof source !== 'object') {
        return;
    }
    // 考虑复制数组还是对象
    let clone = Array.isArray(source) ? [] : {};
    for (const key in source) {
        if (source.hasOwnProperty(key)) {
            if (typeof source[key] === 'object') {
                clone[key] = deepCopy(source[key]);
            } else {
                clone[key] = source[key];
            }
        }
    }
    return clone
}

// 升级版本： 考虑了内置对象如 Date， RegExp， 函数。并且解决了循环引用的问题
function deepClone(target, map = new WeakMap()) {
    // 解决循环引用问题和多次引用同一个对象时需要复制指针的问题，如果遍历到已经遍历过的对象上，就直接返回指针。
    if (map.has(target)) return map.get(target);
    // 获取当前值的构造函数：获取它的类型
    let constructor = target.constructor;
    // 检测当前对象target是否与正则、日期格式对象匹配
    if (constructor && /^(RegExp|Date)$/i.test(constructor.name)) {
        // 创建一个新的特殊对象(正则类/日期类)的实例
        return new constructor(target);  
    }
    if (isObject(target)) {
        const cloneTarget = Array.isArray(target) ? [] : {};
        for (let prop in target) {
            if (target.hasOwnProperty(prop)) {
                cloneTarget[prop] = deepClone(target[prop], map);
            }
        }
        map.set(target, cloneTarget);  // 为循环引用的对象做标记
        return cloneTarget;
    } else {
        return target;
    }
}

function isObject (target) {
    return (typeof target === "object" || typeof target === "function") && target !== null;
}
```



## 手写发布订阅模式

~~~js
class EventEmitter {
    constructor() {
        this.cache = {}
    }
    // 添加订阅事件
    on(name, fn) {
        if (this.cache[name]) {
            this.cache[name].push(fn)
        } else {
            this.cache[name] = [fn]
        }
    }
    // 取消指定事件
    off(name, fn) {
        let tasks = this.cache[name]
        if (tasks) {
            const index = tasks.findIndex(f => f === fn || f.callback === fn)
            if (index >= 0) {
                tasks.splice(index, 1)
            }
        }
    }
    // 触发事件
    emit(name, once = false, ...args) {
        if (this.cache[name]) {
            // 创建数组副本，因为如果回调函数内继续注册相同事件，会造成死循环
            let tasks = this.cache[name].slice()
            for (let fn of tasks) {
                fn(...args)
            }
            if (once) {
                delete this.cache[name]
            }
        }
    }
}

// 测试
let eventBus = new EventEmitter()
let fn1 = function(name, age) {
	console.log(`${name} ${age}`)
}
let fn2 = function(name, age) {
	console.log(`hello, ${name} ${age}`)
}
eventBus.on('aaa', fn1)
eventBus.on('aaa', fn2)
eventBus.emit('aaa', false, '布兰', 12)
// '布兰 12'
// 'hello, 布兰 12'
~~~



## 解析url 为对象

~~~js
function parseParams(url){
    // .表示匹配除换行符 \n 之外的任何单字符
    // + 表示 1 个或更多
    // 括号标记一个子表达式的开始和结束位置，供以后使用
    // $ 表示以其结束
    // 将 ? 后面的字符串取出来（正则中的子表达式）
    const paramsStr = /.+\?(.+)$/.exec(url)[1];
    const paramsArr = paramsStr.split('&');
    const paramsObj = {};
    paramsArr.forEach(param => {
        if(/=/.test(param)){
            //  表示有 key 和 value 的参数，他们以 = 分割
            let [key, val] = param.split('=');
            // 解码api
            val = decodeURIComponent(val);
            // \d 表示数字
            val = /^\d+$/.test(val) ? parseFloat(val) : val;
            if(paramsObj.hasOwnProperty(key)) {
                paramsObj[key] = paramsObj[key].concat(val);
            }else{
                paramsObj[key] = [val];
            }
        }else{
            paramsObj[params] = true;
        }
    })
    return paramsObj;
}
~~~



 

## 字符串模板

~~~js
function render(template, data){
    // /w 表示匹配任意一个字母，下划线，或数字
    const reg = /\{\{(\w+)\}\}/;
    if(reg.test(tempalte)) {
        const name = reg.exec(template)[1];
        tempalte = template.replace(reg, data[name]);
        // 通过递归解决多个模板语法
        return render(tempalte,data);
    }
    return template;
}
~~~



## 图片懒加载

~~~js
// 浅复制
const imgList = [...document.querySelectorAll('img')];
const len = imgList.length;

// 利用闭包函数存储 count 变量
const lazyLoad = (function  () {
    let count = 0;
    return function() {
        let deleteIndexList = [];
        imgList.forEach((img, index) => {
            // 该 api 返回元素的大小和相对于 视口 的位置
            let rect = img.getBoundingClientRect();
            // innerHeight 表示浏览器窗口的视口高度
            if(rect.top < window.innerHeight){
                // 在该 img 标签中的 dataset 自定义属性存放真正的图片地址
                img.src = img.dataset.src;
                deleteIndexList.push(index);
                if(++count === len) document.removeEventListener('scroll', imgLazyLoad);
            }
        })
        imgList = imgList.filter((_, index) => !deleteIndexList.includes(index));
    }
}) ();
document.addEventListener('scroll', imgLazyLoad);
~~~





## 节流

如：发送短信验证码

核心思想是一个闭包函数，记录执行的时间戳。

这个闭包函数内保存一个上一次执行的时间。

值得注意的是，这类闭包函数（防抖节流ke'li），返回的函数==一定是非箭头函数==，这样在 apply 调用时原函数才能有正常的 this 指向。

```js
const throttle = (func, wait = 50) => {
  // 上一次执行该函数的时间
  let lastTime = 0
  return function() {
    // 当前时间
    let now = +new Date()
    // 将当前时间和上一次执行函数时间对比
    if (now - lastTime > wait) {
      lastTime = now
      func.apply(this, arguments)
    }
  }
}
```



## 防抖

高频事件限制触发次数，n 秒后只会执行一次，如果再次触发则重新计时。

核心思想是一个闭包函数，存储一个计时器。

```js
const debounce = (func, wait = 50) => {
  let timer = null;
  return function(...args) {
    if (timer) clearTimeout(timer)
    timer = setTimeout(() => {
      func.apply(this, args)
    }, wait)
  }
}
```



## 函数柯里化

实现函数从一次调用传入多个参数编程多次调用每次传入一个参数，使用了闭包记录参数；

~~~js
function curry(func) {
  const curried = function (...args)  {
    if (args.length >= func.length) return func.apply(this, args);
	else return (...args2) => curried(...args, ...args2);
  };
  return curried;
}
~~~



## jsonp

写函数实现 jsonp：

```js
function jsonp({ url, query = {}, callbackName }) {
    let script = document.createElement("script");
    let promise = new Promise((resolve, reject) => {
        // 注意这个 jsonp 函数发送给服务器的回调函数是一样的，因为只是通过拿数据。
        window[callbackName] = data => {
            // 移除script元素
                document.body.removeChild(script)
            try {
                resolve(data)
            } catch (e) {
                reject(e)
            }
        };
    });
    // 拼接查询字符串，并将回调函数拼接进去。
    query = {...query, callbackName };
    let arrs = [];
    for (let key in query) {
        arrs.push(`${key}=${query[key]}`);
    }
    script.src = `${url}?${arrs.join("&")}`;
    // 将这个 script 标签加入到 body 中
    document.body.appendChild(script);
    return promise;
}

jsonp({
        url: "http://localhost:3000/index",
        query: { name: "zs" },
        // 这里的 callback 存储的只是一个用于避免重名的函数名
        callback: "getdata",
    })
    // 我们通过 then 方法自定义处理函数。
    .then((data) => {
        console.log(data);
    });
```





##  call、bind、apply

三者都是函数（Function的实例）的方法，即箭头函数无法修改 this 指向。

```js
Function.prototype.myCall = function(context， ...args) {
  context = context || window;
  context.fn = this
  let result = context.fn(...args)
  delete context.fn
  return result
}
// apply 方法只有参数的区别

// bind 方法返回的是一个闭包函数而非一个函数调用
Function.prototype.myBind = function (context) {
  const originFn = this;
  // 返回一个函数，该函数返回一个 apply 调用。
  return function F(...args) {
    return originFn.apply(context, args);
  }
}
```



###  new

```js
function myNew(Fn, ...args) {
  var obj = Object.create(Fn.prototype);
  var result = constr.apply(obj, args);
    // 构造函数如果没有显示的返回一个对象，则将创建的obj返回，否则返回返回的对象。
  return result instanceof Object? result : obj;
}
```



### Instanceof

```js
function myInstanceof(left, right) {
  let prototype = right.prototype;
  let left = left.__proto__;
  while (true) {
    if (left === null || left === undefined)
      return false
    if (prototype === left)
      return true
    left = left.__proto__
  }
}
```



## Promise A+（自己不必死记）

```js
class MyPromise {
    // 传递的是一个回调函数
    constructor(executor) {
        // 初始化Promise对象的状态、兑现的值、拒绝的理由、执行函数队列、resolve 和 reject
        this.state = 'pending';
        this.value = undefined;
        this.reason = undefined;
        this.resolvedFn = [];
        this.rejectedFn = [];
        let resolve = value => {
            if (this.state === 'pending') {
                this.state = 'fulfilled';
                this.value = value;
                this.resolvedFn.forEach(fn => fn());
            }
        };
        let reject = reason => {
            if (this.state === 'pending') {
                this.state = 'rejected';
                this.reason = reason;
                this.rejectedFn.forEach(fn => fn());
            }
        };
        // 执行函数
        try {
            executor(resolve, reject);
        } catch (e) {
            reject(e);
        }
    }
	// 
    then(onFulfilled, onRejected) {
        // 4.1 判断传入的参数是否是函数
        onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : v => v;
        onRejected = typeof onRejected === 'function' ? onRejected : err => { throw err };

        // 4.3 声明resolvePromise 函数
        function resolvePromise(promise2, x, resolve, reject) {
            // 循环引用报错
            if (x === promoise2) {
                return reject(new TypeError('Chaining cycle detected for promise'))
            }
            // 保证成功和失败的回调函数两者只被调用一次，这是为了防止一些不规范的Promise 执行报错。
            let called;
            // x不是null 且是对象或函数,这样才有判断是否有then 接口的条件
            if (x !== null && (typeof x === 'object' || typeof x === 'function')) {
                try {
                    let then = x.then;
                    // 不一定是Promise对象，只要实现了then 接口就触发的情况。
                    if (typeof then === 'function') {
                        // 以最保险的方式调用x上的then方法
                        then.call(x, v => {
                            if (called) return;
                            called = true;
                            // then方法拿到数据，递归调用自己，
                            resolvePromise(promise2, v, resolve, reject);
                        }, err => {
                            if (called) return;
                            called = true;
                            reject(err);
                        })
                    }
                    // 没有then 接口，说明是一个值
                    else {
                        resolve(x)
                    }
                } catch (e) {
                    // 执行失败也属于失败】
                    if (called) return;
                    called = true;
                    reject(e);
                }
            } else {
                resolve(x)
            }
        }

        // 4.2 声明需要返回的promise2
        let promise2 = new MyPromise((resolve, reject) => {
            // 判断状态，执行响应的函数
            if (this.state === 'fulfilled') {
                let x = onFulfilled(this.value);
                // resolvePromise 函数处理自己在then方法中传入的函数参数return的值x，使其成为Promise2包含的值
                resolvePromise(promise2, x, resolve, reject);
            } else if (this.state === 'rejected') {
                let x = onRejected(this.reason);
                resolvePromise(promise2, x, resolve, reject);
            } else {
                this.resolvedFn.push(
                    () => {
                        let x = onFulfilled(this.value);
                        resolvePromise(promise2, x, resolve, reject);
                    }
                );
                this.rejectedFn.push(
                    () => {
                        let x = onFulfilled(this.value);
                        resolvePromise(promise2, x, resolve, reject);
                    }
                );
            }
        });
        // 返回promise2
        return promise2;

    }
}
```



## 一些Promise的API

Promise.all() / Promise.race()

```js
//race方法 
static race (promises){
  return new Promise((resolve,reject)=>{
    for(val of promises){
      val.then(resolve,reject)
    };
  })
}
//all方法(获取所有的promise，都执行then，把结果放到数组，一起返回)
static all(promises){
  let arr = [];
  let count = 0;
  return new Promise((resolve,reject)=>{
    for(let i = 0; i < promises.length; i++){
      promises[i].then(data => {
        arr[i] = data;
        count++;
        if(count === promises.length) {
            resolve(arr)
        }
      }, error => reject(error) );
     };
  });
}
```

