
## 前言
在这互联网的寒冬腊月时期，虽说过了金三银四，但依旧在招人不断。更偏向于招聘高级开发工程师。本人在这期间求职，去了几家创业，小厂，大厂厮杀了一番，也得到了自己满意的offer。

整理一下自己还记得的面试题，希望能帮助到还在求职的你。大佬略过，不喜勿喷。

> 记录下我遇到的面试题，都有大佬分享过，附上各个大佬的文章，总结出其中的主要思想即可。

## css篇
### 1. 盒模型
概念就不介绍了，请看一个例题，若你能立马反应过来，就说明掌握的不错了。
```css
/* 红色区域的大小是多少？ */
.box {
    width: 200px;
    height: 200px;
    padding: 20px;
    margin: 20px;
    background: red;
    border: 20px solid black;
    box-sizing: border-box;
}
```
### 2. 水平垂直居中
贴上一位腾讯大佬总结的文章

[16种方法实现水平居中垂直居中](https://juejin.im/post/58f818bbb123db006233ab2a)

按照固定宽高和不固定宽高分类各说几个方法就可以了。
### 3. 页面布局
#### 3.1 高度已知，三栏布局，左右宽度300，中间自适应
```css
/* 浮动布局 */
  .layout.float .left{
    float:left;
    width:300px;
    background: red;
  }
  .layout.float .center{
    background: yellow;
  }
  .layout.float .right{
    float:right;
    width:300px;
    background: blue;
  }
 /* 决定定位布局 */
 .layout.absolute .left-center-right>div{
  position: absolute;
 }
.layout.absolute .left{
  left:0;
  width: 300px;
  background: red;
}
.layout.absolute .center{
  left: 300px;
  right: 300px;
  background: yellow;
}
.layout.absolute .right{
  right:0;
  width: 300px;
  background: blue;
}
 /* flex布局 */
  .layout.flexbox{
      margin-top: 110px;
    }
    .layout.flexbox .left-center-right{
      display: flex;
    }
    .layout.flexbox .left{
      width: 300px;
      background: red;
    }
    .layout.flexbox .center{
      flex:1;
      background: yellow;
    }
    .layout.flexbox .right{
      width: 300px;
      background: blue;
    }
```
当然还有 `table` 布局 和 `grid` 布局。

当你回答出来以后，面试官还可能会追问你，这几种布局有什么优缺点，兼容性？

如果把高度已知换成未知，又该怎么做？

这些都是我们要考虑总结的。

#### 3.2 高度已知，两栏布局，左边固定，右边自适应
基本思路和三栏布局一样
#### 3.3 如何实现左右等高布局
`table` 布局就登场了
```css
section {
    width:100%;
  display: table;
}
article  {
    display: table-cell;
}
.left {
    height: 100px;
    background: red;
}
.right {
    background: black;
}
```
### 4. 如何实现一个最大的正方形
用 `padding-bottom` 撑开边距就可以了。
```css
 section {
    width:100%;
    padding-bottom: 100%;
    background: #333;
}
```
### 5. css 的解析顺序
> `css` 选择器匹配元素是逆向解析

* 因为所有样式规则可能数量很大，而且绝大多数不会匹配到当前的 DOM 元素（因为数量很大所以一般会建立规则索引树），所以有一个快速的方法来判断「这个 selector 不匹配当前元素」就是极其重要的。
* 如果正向解析，例如「div div p em」，我们首先就要检查当前元素到 html 的整条路径，找到最上层的 div，再往下找，如果遇到不匹配就必须回到最上层那个 div，往下再去匹配选择器中的第一个 div，回溯若干次才能确定匹配与否，效率很低。
* 逆向匹配则不同，如果当前的 DOM 元素是 div，而不是 selector 最后的 em，那只要一步就能排除。只有在匹配时，才会不断向上找父节点进行验证。
> 所以为了减少查找时间，尽量不要直接使用标签选择器。

### 6. css 如何开启 gpu 加速
请参考`网易云社区`的文章[CSS动画的性能分析和浏览器GPU加速](https://juejin.im/post/5bd947326fb9a0226924ad77#heading-2)
## js篇
### 1. 不借助第三者交换 a，b两个值。
```javascript
/* 方法一 */
a = a + b;
b = a - b;
a = a - b;

/* 方法二 */
a = a - b;
b = a + b;
a = b - a;

/* 方法三 */
a = {a:b,b:a};
b = a.b;
a = a.a;

/* 方法四 */
a = [a,b];
b = a[0];
a = a[1];

/* 方法五 */
[a,b] = [b,a];
```
### 2. new 的过程和实现
```js
/* 选自 yck 文章 */
function create(Con, ...args) {
  let obj = {}
  Object.setPrototypeOf(obj, Con.prototype)
  let result = Con.apply(obj, args)
  return result instanceof Object ? result : obj
}

```
如果你能清楚的了解上边代码的原理，请忽略。

不然的话建议阅读下边大佬的文章。

推荐 `yck` 的文章 [重学 JS 系列：聊聊 new 操作符
](https://juejin.im/post/5c7b963ae51d453eb173896e)
### 3. Eventloop
无论是笔试还是面试，高频出现，要知其然还要知其所以然。
```js
/* 选自 小美娜娜 文章 */
async function async1() {
    console.log("async1 start");
    await  async2();
    console.log("async1 end");
}

async  function async2() {
    console.log( 'async2');
}

console.log("script start");

setTimeout(function () {
    console.log("settimeout");
},0);

async1();

new Promise(function (resolve) {
    console.log("promise1");
    resolve();
}).then(function () {
    console.log("promise2");
});
console.log('script end'); 

```
如果你能很清楚的知道每一步的输出，可以忽略。

不然的话建议阅读下边大佬的文章，解释的很详细。

推荐 `小美娜娜` 的文章 [Eventloop不可怕，可怕的是遇上Promise
](https://juejin.im/post/5c9a43175188252d876e5903)
### 4. 继承优缺点
```js
 function inherits(childCtor, parentCtor) {
    function tempCtor() {};
    tempCtor.prototype = parentCtor.prototype;
    childCtor.prototype = new tempCtor();
    childCtor.prototype.constructor = childCtor;
};
```
比如说 原型继承，构造函数继承，组合继承等。

向面试官从简到难介绍每个继承的优缺点，一步步的写出最完美的继承。

绝对能加好多分。

推荐一篇 `路易斯` 的文章 [JS原型链与继承别再被问倒了
](https://juejin.im/post/58f94c9bb123db411953691b)
### 5. 实现一个 promise
首先你需要对 promise 有一个清晰的认识，封装过请求，使用过它的 api 等。

请参考 `前端劝退师` 的文章  [「中高级前端面试」JavaScript手写代码无敌秘籍
](https://juejin.im/post/5c9c3989e51d454e3a3902b6#heading-16)
### 6. 实现一个按顺序加载的 promise
当你说出 promise.all 和 promise.race 的区别后，面试官可能就会接着考察此题。
```js
/* 使用 async await */
async function queue(tasks) {
  const res = []
  for (let promise of tasks) {
    res.push(await promise(res));
  }
  return await res
}
queue([a, b, c])
  .then(data => {
    console.log(data)
  })
```
当然也可以使用 `reduce` 方法。
### 7. 对 import 和 module.exports的理解
考察对模块化的理解。
区分 Es6 和 commonJs 规范。

可以参考 `有赞 2dunn` 的 文章 [import、require、export、module.exports 混合使用详解
](https://juejin.im/post/5a2e5f0851882575d42f5609)
### 8. 实现一个 EventEmitter 方法
当你回答出 vue 中用 emit 通信的时候，就要小心了。

EventEmitter 方法主要包含了 on,emit,once,off方法。
```js
  class Event {
    constructor() {
          this.events = Object.create(null);
      }
      on(name, fn) {
        if (!this.events[name]) {
            this.events[name] = []
          }
          this.events[name].push(fn);
          return this;
      }
      emit(name, ...args) {
        if (!this.events[name]) {
            return this;
        }
        const fns = this.events[name]
        fns.forEach(fn => fn.call(this, ...args))
        return this;
      }
      off(name,fn) {
        if (!this.events[name]) {
            return this;
        }
          if (!fn) {
            this.events[name] = null
            return this
          }
          const index = this.events[name].indexOf(fn);
          this.events[name].splice(index, 1);
        return this;
      }
      once(name,fn) {
        const only = () => {
          fn.apply(this, arguments);
          this.off(name, only);
        };
        this.on(name, only);
        return this;
      }
  }
```
可以参考 `尤大大`的 vue 源码 [vue-event](https://github.com/vuejs/vue/blob/dev/src/core/instance/events.js)
### 9. this 指向
如果你能清楚的知道每个this的指向，可以忽略。

比如说 默认绑定，隐式绑定，显示绑定，new 绑定。

```js
/* 节选自 京东小姐姐 文章 */
var obj = {
    hi: function(){
        console.log(this);
        return ()=>{
            console.log(this);
        }
    },
    sayHi: function(){
        return function() {
            console.log(this);
            return ()=>{
                console.log(this);
            }
        }
    },
    see: function() {
        console.log(this);
        (function(){
            console.log(this)
        })()
    },
    say: ()=>{
        console.log(this);
    }
}
let hi = obj.hi(); 
hi();            
let sayHi = obj.sayHi();
let fun1 = sayHi(); 
fun1();             
obj.say();
obj.see();
```
不清楚的话，建议阅读一下

`京东小姐姐`的文章  [嗨，你真的懂this吗？](https://juejin.im/post/5c96d0c751882511c832ff7b)
### 10. 普通函数和箭头函数的区别
* this指向的问题，会捕获其所在上下文的 this 值，作为自己的 this 值。
* 箭头函数不绑定 arguments，取而代之用rest参数解决。
```js
var foo = (...args) => {
  return args[0]
}
console.log(foo(1)) 
```
* 箭头函数不能用作构造器，和 new 一起用就会抛出错误。
* 箭头函数没有原型属性。
```js
var foo = () => {};
console.log(foo.prototype) //undefined
```
### 11. 考察本地存储
```js
/* 以下代码会输出什么 */
localStorage.setItem('show',false);
console.log(localStorage.show || '显示')
```
## vue篇
### 1. vnode的转换过程 和 dom-diff 的过程
有的大厂面试官需要你手写这个过程，很残忍有木有。

请参考 `360-chenhongdong`大佬的文章 
[让虚拟DOM和DOM-diff不再成为你的绊脚石
](https://juejin.im/post/5c8e5e4951882545c109ae9c#heading-5)
### 2. key值的作用
使用 `v-for`更新已渲染的元素列表时,默认用就地复用策略。列表数据修改的时候,他会根据key值去判断某个值是否修改：如果修改,则重新渲染这一项;否则复用之前的dom，仅修改value值。

详细参考 `阿里-funnycoderstar` 文章[为什么使用v-for时必须添加唯一的key?
](https://juejin.im/post/5aae19aa6fb9a028d4445d1a)
### 3. 对 v-show的理解
我猜你可能会回答 `v-show` 通过控制样式的 `display: none` 和 `display: block` 实现显示隐藏的。
你觉得这样合理吗？

比如说：对一个 img 或者 span 使用 v-show，那么当它为 true 的时候不就把内联元素改成块级元素了嘛。

出题目啦：
```html
/* 1.此时span是显示还是隐藏？ */
<span  class="span" v-show="isShow">111</span>
<script>
data() {
    return {
      isShow: false
    }
  },
</script>
<style>
.span {
  display: block;
}
</style>

/* 2.此时span是显示还是隐藏？ */
<span  class="span" v-show="isShow">111</span>
<script>
data() {
    return {
      isShow: true
    }
  },
</script>
<style>
.span {
  display: none;
}
</style>
```

> v-show 控制显隐，是通过 js 代码去修改元素的 element style。
 如果 value 为 false，设置 `display: none`； 
 如果 value 为 true，清除 display 属性。
所以 value 为 true 时，只是将 element style 中的 display 效果清除，并不能覆盖 css 中的 display 样式。

### 4. vue的生命周期
不要直接巴拉巴拉的说出那几个英文单词，建议按照官网图的执行顺序来引出各个生命周期名称，会有加分的。
<div align="center"><img src="https://user-gold-cdn.xitu.io/2019/5/15/16abbd8d0c60f754" style="height:800px"></div>

## react篇
### 1. react的自定义事件和原生事件的区别
React 并不是将 click 事件绑在该 div 的真实 DOM 上，而是在 document 处监听所有支持的事件，当事件发生并冒泡至 document 处时，React 将事件内容封装并交由真正的处理函数运行。

详情请参考文章 `蚂蚁金服-Nekron` 大佬的文章 [React合成事件和DOM原生事件混用须知
](React合成事件和DOM原生事件混用须知
)
### 2. setState是异步还是同步的
不要着急回答是异步的，再上问的基础上 setState 也可以是同步的。

`setState` 只在合成事件和钩子函数中是“异步”的，在原生事件和 `setTimeout` 中都是同步的。

详情请阅读 `饿了么-虹晨`大佬的文章
[你真的理解setState吗？
](https://juejin.im/post/5b45c57c51882519790c7441)
### 3. redux和vuex的区别
* redux 是 flux 的一种实现，redux 不单单可以用在 react 上面。
* vuex 是 redux 的基础上进行改变，对仓库的管理更加明确。
* 数据流向不一样，vuex 的同异步有不同的流向，而 redux 的同异步是一样的。
* redux 使用的是不可变的数据，而 vuex 的数据是可变的，redux 每次修改更新数据，其实就是用新的数据替换旧的数据，而 vuex 是直接修改原数据。

### 4.react 和 vue 的 diff 过程有什么区别

React 是这么干的：你给我一个数据，我根据这个数据生成一个全新的 Virtual DOM，然后跟我上一次生成的 Virtual DOM 去 diff，得到一个 Patch，然后把这个 Patch 打到浏览器的 DOM 上去。完事。并且这里的 Patch 显然不是完整的 Virtual DOM，而是新的 Virtual DOM 和上一次的 Virtual DOM 经过 diff 后的差异化的部分。

Vue 在渲染过程中，会跟踪每一个组件的依赖关系，不需要重新渲染整个组件树。

React 每当应用的状态被改变时，全部子组件都会重新渲染。
这可以通过 shouldComponentUpdate 这个生命周期方法来进行控制。

React diff的是 Dom，而 Vue diff 的是数据。
## webpack篇
### 1. webpack 如何进行打包优化
从提取公共模块，区分开发环境，移除重复不必要的 css 和 js 文件等方面说。

推荐`arlendp2012`的文章 [Webpack打包优化
](https://juejin.im/post/5b1e303b6fb9a01e605fd0b3)
### 2. webpack 打包原理
* 初始化：启动构建，读取与合并配置参数，加载 Plugin，实例化 Compiler。
* 编译：从 Entry 发出，针对每个 Module 串行调用对应的 Loader 去翻译文件内容，再找到该 Module 依赖的 Module，递归地进行编译处理。
* 输出：对编译后的 Module 组合成 Chunk，把 Chunk 转换成文件，输出到文件系统。

推荐`whjin`的文章 [webpack原理](https://segmentfault.com/a/1190000015088834)
## 浏览器篇
#### 1. 如何实现跨域
主要介绍 jsonp 和 cors 即可，其他可以一笔带过。

推荐阅读 `浪里行舟` 大佬的文章
[九种跨域方式实现原理（完整版）
](https://juejin.im/post/5c23993de51d457b8c1f4ee1)
### 2. 输入url后的加载过程
简要回答出 域名解析，返回资源，浏览器渲染，重排重绘概念等即可。

推荐 `浪里行舟` 文章[从URL输入到页面展现到底发生什么？
](https://juejin.im/post/5bf3ad55f265da61682afc9b)

### 3. 缓存
分为 强缓存 和 协商缓存，
各自的标识要记住。

推荐 `黑金团队`的文章 [前端缓存最佳实践
](https://juejin.im/post/5c136bd16fb9a049d37efc47)

 推荐`名扬`的文章 [浅解强缓存和协商缓存
](浅解强缓存和协商缓存
)
### 4. http状态码
重点是 301 302 304 401 等，要给面试官介绍清楚。

304 的话就会涉及到上问的缓存，能详细介绍下会是个加分项。

推荐`骑摩托马斯`文章，有图有文字
[具有代表性的 HTTP 状态码
](https://juejin.im/post/5a276865f265da432c23b8d2)
### 5. 本地存储
要记住 cookie 的几个属性值，分别是做什么的。

比如说 cookie 设置了 express 属性，存储位置有什么区别？

比如说 a.snow.com 和 b.snow.com 两个网页如何共享 cookie ?

推荐 `Damonare`的文章
[Javascript 本地存储小结
](https://juejin.im/post/582c7d330ce463006ce33838)
### 6. http请求头有哪些
介绍几个，然后说清楚各自的作用即可。

又会涉及到 缓存头的知识。

推荐`非常兔`的文章[http请求头与响应头的应用
](https://juejin.im/post/5b854ddef265da43635d9302)
### 7. 路由实现的原理
两种模式 hash 和 history。

推荐`寻找海蓝96`的文章 [面试官: 你了解前端路由吗?
](https://juejin.im/post/5ac61da66fb9a028c71eae1b)
### 8. 常见的内存泄露
全局变量，未清除的定时器，闭包，以及 dom 的引用等。

推荐掘金的`LeviDing`文章
[[译] JavaScript 工作原理：内存管理 + 处理常见的4种内存泄漏
](https://juejin.im/post/5a2559ae6fb9a044fe4634ba#heading-14)
### 9. 前端安全问题
主要从 XSS 和 CSRF 两个方面回答即可。

推荐 `京东小姐姐`的文章 [【面试篇】寒冬求职之你必须要懂的Web安全
](https://juejin.im/post/5cd6ad7a51882568d3670a8e)
### 10. GET 和 POST 的区别 
这题道理很深，也有很多误区。

推荐 `WebTechGarden 微信公众号`的文章
[99%的人都理解错了HTTP中GET与POST的区别
](https://mp.weixin.qq.com/s?__biz=MzI3NzIzMzg3Mw==&mid=100000054&idx=1&sn=71f6c214f3833d9ca20b9f7dcd9d33e4#rd)

### 11. 对 HTTP2 的理解
HTTP2号称可以让我们的应用更快、更简单、更稳定，它完美解决了1.1版本的诸多问题。

推荐`黑金团队`的文章
[面试官问：你了解HTTP2.0吗？
](https://juejin.im/post/5c0ce870f265da61171c8c66)
## 扩展篇
### 1. 请问你怎么看待技术和业务的关系？
欢迎讨论、、、
### 2. 请问你怎么看待996或者说公司加班？
也欢迎讨论，，，
## 总结
写业务的同时，不要忘记提高自己的基础知识，css，js，http 等，才会在面试中脱颖而出。

水平有限，如果有不妥的回答，还望指出，谢谢。

下篇总结下遇到的编码算法相关题目。


