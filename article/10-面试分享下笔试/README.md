## 前言

越来越多的公司都在面试前加入了笔试环节。

有的甚至会根据你的笔试答题情况来决定是否进入面试环节。

当然，进入面试环节，也会时不时的出几道算法或者其他类型的相关的题目让你写出来。

所以不仅要会说，还要会写。

关于手写面试题的文章也有好多，实现 call,apply,bind 之类的，也都最好要掌握。

总结了一些我在面试中遇到的题，这些题不像是基础部分，会针对特定的公司，也许不一定适合你，`由于面试的级别不高，题目难度属于中等偏下`，所以仅供参考。

若想提高算法思维，建议多刷刷 `leetcode`。

> 一直在纠结这篇文章该不该分享出来，个人感觉此类题目不像是基础知识，作用不大。但是为了兑现上篇文章，所以还是水了这篇。

## 笔试篇
### 1. 替换手机号中间四位
```js
// getPhone('13211223344') => 132****3344
function getPhone(phone) {
    // todo
}
```

方法有很多，循环遍历，或者转换成数组，使用数组的各种 api。

当然还是正则比较简单。
```js
function getPhone(phone) {
    phone = "" + phone;
    const reg = /(\d{3})\d{4}(\d{4})/;
    return phone.replace(reg, "$1****$2")
}
```
如果学习正则的话，

推荐 `老姚` [JS正则表达式完整教程](https://juejin.im/post/5965943ff265da6c30653879)

这篇文章很长，需细嚼慢咽。
### 2. 拆分数组
```js
/* 将一维数组切分成二维数组： 按照第二个参数（数字）的值，来决定二维数组长度 */
 let a = ['a', 'b', 'c', 'd'];
 let result = chunk(a, 3);
 console.log(result) => [['a', 'b', 'c'], ['d']];
 let result = chunk(a, 2);
 console.log(result) => [['a', 'b'], ['c','d']];
 let result = chunk(a, 1);
 console.log(result) => [['a'],['b'],['c'],['d']];
```
 提供其中一种解题方法，用数组的 slice 方法。
```js
function chunk(arr, ind) {
    const len = arr.length;
    if(ind > len || ind <= 0) return [];
    const newArr = [];
    for(let i = 0;i < len;i += ind) {
        let j = i;
        newArr.push(arr.slice(j, j += ind));
    }
    return newArr;
}
```
如果学习数组相关 api 的话，

推荐 `OBKoro1` 的文章 [js 数组详细操作方法及解析合集](https://juejin.im/post/5b0903b26fb9a07a9d70c7e0)
### 3. 获取对象中指定 path 的值
```js
// let obj = {foo: {bar: {name:'biz'}}};
// get(obj,'foo.bar.name'); // biz
// obj = {};
// get(obj,'foo.bar.name'); // underfind
// get(obj,'foo.bar.name','biz'); // biz
function get(obj, path, defalutValue) {
    // todo
}
```
我的解法是利用了递归。
```js
function get(obj, path, defalutValue) {
    if (!path || JSON.stringify(obj) === '{}') {
        return defalutValue;
    }
    for(let key in obj) {
        if(!path.split('.').includes(key)) {
            return null
        }
        if(!path.endsWith(key)) {
            return typeof obj[key] === 'object' ? get(obj[key], path, defalutValue) : defalutValue;
        } 
        return obj[key]
    }
}
```
关于递归用法的话，如果不清楚:

推荐 [JavaScript算法之递归
](https://juejin.im/post/5c3e9fdbf265da61285a596d)
### 4. 扁平化期望数组
```js
const data = {
   rows: [
           ["Lisa", 16, "Female", "2000-12-01"], 
           ["Bob", 22, "Male", "1996-01-21"]
        ], 
  metaData: [ 
          { name: "name", note: '' }, 
          { name: "age", note: '' }, 
          { name: "gender", note: '' },
          { name: "birthday", note: '' } 
     ]
}
/*
rows 是数据， metaData 是对数据的说明。
现写⼀个函数 parseData，将上⾯面的对象转化为期望的数组
*/
outputData = [
 { name: "Lisa", age: 16, gender: "Female", birthday: "2000-12-01" },
 { name: "Bob", age: 22, gender: "Male", birthday: "1996-01-21" }, 
]
```
使用了一种比较笨拙的方法，循环遍历
```js
const parseData = (data) => {
    const newData = [];
    const { metaData, rows } = data;
    const mapArr = metaData.map(item => item.name)
   rows.forEach((item,ind) => {
       const obj = {};
       item.forEach((item,ind) => {
            obj[[mapArr[ind]]]=item
       })
       newData.push(obj)
   });
   return newData;
} 
```
### 5. 实现一个红绿灯
结合生活编码

利用了 promise 加 递归实现。
```js
  function delay(timer) {
      return () => (
           new Promise(resolve => {
              setTimeout(resolve, timer);
          })
      )
  }
 const red = delay(10000);
 const green = delay(5000);
 const yellow = delay(2000);
 const traffic = document.getElementById('traffic');
 (function resStart(){
    traffic.innerText = '红';
     red().then(()=> {
        traffic.innerText = '绿';
        return green();
     })
     .then(()=> {
        traffic.innerText = '黄';
        return yellow();
     })
     .then(()=> {
        return resStart();
     })
 })()
```
其他解法可以参考这篇文章：

推荐 `FreePotato` 的文章 [【javascript】一个可以配置的红绿灯
](https://juejin.im/post/5c76357f51882540713f5717)

### 6. 实现可以链式调用的延时
```js
/* 按照调用实例，实现下面的Person方法 */
Person("Li");
// 输出： Hi! This is Li!
Person("Dan").sleep(10).eat("dinner");
// 输出：
// Hi! This is Dan!
// 等待10秒
// Wake up after 10
// Eat dinner~
Person("Jerry").eat("dinner").eat("supper");
// 输出：
// Hi This is Jerry!
// Eat dinner~
// Eat supper~
Person("Smith").sleepFirst(5).eat("supper");
// 输出：
// 等待5秒
// Wake up after 5
// Hi This is Smith!
// Eat supper
```
链式调用可以阅读下 jquery 的源码，在每个方法的最后 return this 就好了。

1.链接所有 this;

2.把所有任务放到任务队列里面;

3.通过一个方法有序执行队列里面的任务;

```js
function Person(name) {
    this.task = [];
    const fn = () => {
        console.log(`Hi! This is ${name}!`);
        this.then();
    }
    this.task.push(fn);
    setTimeout(()=>{
        this.then();
    })
    return this;
}
Person.prototype = {
    sleepFirst(timeout) {
        const fn = () => {
            console.log(`Wake up after ${timeout}`)
            setTimeout(() => {
                this.then();
            }, timeout)
        }
        this.task.unshift(fn);
        return this;
    },
    sleep(timeout) {
        const fn = () => {
            console.log(`Wake up after ${timeout}`)
            setTimeout(() => {
                this.then();
            }, timeout)
        }
        this.task.push(fn);
        return this;
    },
    eat(meal) {
        const fn = () => {
            console.log(`Eat ${meal}`)
            this.then();
        }
        this.task.push(fn);
        return this;
    },
    then() {
        const fn = this.task.shift();
        fn && fn();
    }
}
```
### 7. 增加随机值
```js
/* 
下面的数据结构中，不同层级的key可能会相同。
实现一个方法，调用时更新数组中的key值，
使所用的key对应的值更新为新的随机数，
并且保证更新前相同的key值更新后也相同。
*/
let arr = [
    {
        key: 123123,
        child: {
            key: 3213313,
            child: {
                key: 123123,
                child: {
                    key: 3213313
                }
            }
        }
    },
    {
        key: 789789,
        child: {
            key: 312308,
            child: {
                key: 123123,
                child: {
                    key: 3213313,
                    child: {
                        key: 873432
                    }
                }
            }
        }
    }
]

function update(arr) {
    // todo
}
```
基本思路是：
1. 每个key是随机值，那么都加上同一个随机值以后，还是个随机值。
2. 递归遍历。

```js
function update(arr) {
    const num = Math.ceil(Math.random()*1000000)/1000000;
    for(let item of arr) {
        item.key += num;
        (function updateKey(obj){
            if(!obj.hasOwnProperty('child')) {
              return obj.key += num;
            }
            obj.child.key += num;
            if(obj.hasOwnProperty('child')) {
              return  updateKey(obj.child);
            }
        })(item)
    }
    return arr;
}
```
> 面试官提供的思路是转换字符串，然后利用正则匹配。如果有大佬写出来了，欢迎告知。

## 数据结构篇
### 1. 说出你所写代码的复杂度
首先需要先了解什么是代码的复杂度，

复杂度是怎么计算的。

可以参考以下两篇文章，也可自行搜索，相关文章有很多：

[JavaScript 算法之复杂度分析
](https://juejin.im/post/5c2a1d9d6fb9a04a0f654581)

[JavaScript 算法之最好、最坏时间复杂度分析
](https://juejin.im/post/5c309aef6fb9a049c84f9adb)

> 从网上截取了一张关于排序的复杂度。如若侵权，告知删除。

![](https://user-gold-cdn.xitu.io/2019/5/27/16af834bc1bdf612?w=2002&h=2018&f=png&s=429914)
### 2. 用堆实现队列
在了解了基本的堆和队列以后，

这两种数据结构相互转换也是需要了解到的。

可以参考下面的文章，也可自行搜索，相关文章有很多。

[JavaScript 数据结构之队栈互搏
](https://juejin.im/post/5c25e3566fb9a04a0f653f8e)
### 3. 实现链表的添加和查找方法
首先得需要了解什么是链表，

链表和数组有什么区别。

可以参考下面的文章，也可自行搜索，相关文章有很多：

[JavaScript数据结构之链表--介绍
](https://juejin.im/post/5c8a8289f265da2dda69927d)

[JavaScript数据结构之链表--设计
](https://juejin.im/post/5c8e5a086fb9a070bc3f02f9)

### 4. 反转单链表
```js
/*
输入：1 -> 2  ->  3 ->  4
输出：4 -> 3  ->  2 ->  1
*/
```

设置两个指针，
cur 为当前节点，
pre 为当前节点的前一个节点，

利用 pre 让节点反转所指方向。

```js
  reverseList(){
      if(this.head == null) return null;
      let cur = this.head;
      let prev = null;
      while( cur!==null ){
        let nextNode = cur.next
        cur.next = prev
        prev = cur
        cur = nextNode
      }
     this.head = prev
    }
```
### 4. 判断链表是否有环

![](https://user-gold-cdn.xitu.io/2019/5/29/16b017b624e5883a?w=256&h=183&f=png&s=7174)
这题方法也有很多，比如说 硬循环，一直 next 下去，直到为 null 。设置一个时间段，在这个时间内看是否为空。虽说不可取，但也是种方法。

还有一种常用的办法就是龟兔赛跑。利用两个快慢指针，慢指针每次走一格，快指针每次走两格，当快慢指针相遇的时候，就可以说明此单链表有环。
```js
 hasCircle() {
        let fast = this.head;
        let slow = this.head;
        while (fast !== null && fast.next !== null) {
            fast = fast.next.next
            slow = slow.next
            if (slow === fast) return true
        }
        return false
    }
```
### 5. 单链表排序
```js
/*
输入：4 -> 1  ->  3 ->  5
输出：1 -> 3  ->  4 ->  5
*/
```

这里使用了是快排
```js
// 创建节点
class Node {
    constructor(value) {
        this.val = value;
        this.next = null;
    }
}
// 创建链表
class NodeList {
    constructor(arr) {
        let head = new Node(arr.shift());
        let next = head;
        arr.forEach(item => {
            next.next = new Node(item);
            next = next.next;
        })
        return head;
    }
}
// 交换两个链表的值
function swap(p,q) {
     let val = p.val;
     p.val = q.val;
     q.val = val;
}
// 找到基准值
function partion(begin,end) {
    let val = begin.val;
    let p = begin;
    let q = begin.next;
    while(q !== end) {
        if(q.val < val) {
            p = p.next;
            swap(p, q) // 交换小的值
        }
        q = q.next; // 指针后移
    }
    swap(p, begin);// 交换基准值
    return p;
}
// 利用快排递归
function sort(begin,end = null) {
    if(begin !== end) {
        let part = partion(begin,end);
        // 两路快排
        sort(begin,part);
        sort(part.next,end);
    }
    return begin
}
```

### 6. 整合两个单链表
```js
/*
输入：1 -> 2 -> 3 -> 4, 11 -> 21 -> 31 -> 41
输出：1 -> 2 -> 3 -> 4 -> 11 -> 21 -> 31 -> 41
*/
```
用递归的方法将两个单链表整合在一起
```js
   function merge(list1, list2) {
        if (list1 == null) return list2;
        else if (list2 == null) return list1;
        let mergehead = null;
        if (list1.val <= list2.val) {
          mergehead = list1;
          mergehead.next = merge(list1.next, list2);
        } else {
          mergehead = list2;
          mergehead.next = this.merge(list1, list2.next);
        }
        return mergehead;
      }
```

更多关于链表的题目，

推荐 `尾尾部落` 的文章 [[算法总结] 17 题搞定 BAT 面试——链表题
](https://www.weiweiblog.cn/linkedlist_summary/)

## 算法篇   
### 1. 数组去重
老生常谈的问题，习惯了使用 api 去重的童鞋要小心了，

有的面试官可能会刁难你，比如说，不使用 api，不能创建新的变量。

推荐 `冴羽`的文章 - [JavaScript专题之数组去重
](https://juejin.im/post/5949d85f61ff4b006c0de98b)
### 2. 查找两数之和
```js
// 给定 nums = [2, 7, 11, 15], target = 9
// 因为 nums[0] + nums[1] = 2 + 7 = 9
// 所以返回 [0, 1]
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
};
```
此题解法也有很多种，

比如说 暴力解法，两层循环，时间复杂度较高。

可以把代码在 leetcode 运行一次，看看运行效率，然后寻找更优的解法。

下面的解法只是一种思路，不是最优的。

```js
var twoSum = function(nums, target) {
     const obj = {};
     for(let i=0;i<nums.length;i++) {
         const num = nums[i];
         if(obj[num] === undefined) {
             const mins = target - num;
             obj[mins] = i;
         } else {
             return [obj[num], i]
         }
     }
     return [];
};
```
### 3. 最大子序和
```js
// 输入: [-2,1,-3,4,-1,2,1,-5,4],
// 输出: 6
// 解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function(nums) {
  }
```
提供一种解题思路
```js
var maxSubArray = function(nums) {
    let current =  nums[0];
    let  sum =  nums[0];
    for (let i = 1; i < nums.length; i++) {
      if (current > 0) {
        current += nums[i];
      } else {
        current = nums[i];
      }
      if (current > sum) {
        sum = current;
      }
    }
    return sum;
  }
```

### 4. 得到两个列表的交集
```js
// 输入: nums1 = [1,2,2,1], nums2 = [2,2]
// 输出: [2]

/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */

  var intersection = function(nums1, nums2) {
  }

```
两个数组中查找重复出现的值，且只能出现一次。
```js
  var intersection = function(nums1, nums2) {
  const map = {};
  const res = [];
  nums1.forEach((item) => {
    map[item] = 1;
  });

  nums2.forEach((item) => {
    if (map[item] === 1) {
        res.push(item);
        map[item]++;
    }
  });
  return res;
}
```
### 5. 快速排序
这也是常考的一道题。

如果你挥毫泼墨写出了[阮老师版本的快排](http://www.ruanyifeng.com/blog/2011/04/quicksort_in_javascript.html)后就要小心了。

此写法虽然简单易理解，但是时间复杂度和空间复杂度还是比较高的，

不适合大数据排序。

优化版本的三路快排，时间复杂度还是没有降低。

从性能来说，还是推荐 `分治法`。

推荐 `百度-舞动乾坤`的文章[JS快速排序&三路快排
](https://juejin.im/post/5c662e496fb9a049b82afb71)

或者 `张晨辉Allen` 的 java 版本文章 [快速排序算法原理及实现](https://www.jianshu.com/p/2ae6ba100d3a)
### 6. 更多解法
推荐 我自建群里的一位小哥的 github 
[leet-code-solutions](https://github.com/ZodiacSyndicate/leet-code-solutions)

python,go,js 多语言满足你。

## 提示

水平有限，如果有不妥的回答，还望指出，谢谢。

[【前端面试分享】- 寒冬求职上篇](https://juejin.im/post/5cdb7bc26fb9a0321557044d)

## 有你才完美
 自认很菜，创建了一个数据结构和算法的交流群，不限开发语言，前端后端，欢迎各位同学入驻。

群以超过扫码限制人数，可以加我好友，邀请你入群。

请备注：`掘金加群` 哦
<div align="center"><img src="https://user-gold-cdn.xitu.io/2019/5/16/16ac1346ca68c86c" style="height:300px"></div>