####  数据结构之---队列
#####  1.队列的定义
> 队列是一种特殊的线性表，特殊之处在于它只允许在表的前端（front）进行删除操作，而在表的后端（end）进行插入操作，和栈一样，队列是一种操作受限制的线性表。进行插入操作的端称为队尾，进行删除操作的端称为队首。

> 队列的数据元素又称为队列元素。在队列中插入一个队列元素称为入队，从队列中删除一个队列元素称为出队。因为队列只允许在一端插入，在另一端删除，所以只有最早进入队列的元素才能最先从队列中删除，故队列的特性为  ***先进先出*** (First-In-First-Out，FIFO)

请看下面的图解
![](https://user-gold-cdn.xitu.io/2018/12/27/167eeca806e77636?w=540&h=162&f=png&s=16067)
队列添加新的元素，左侧是队列的头部，右侧是队列的尾部，新的元素如果想进入队列，只能从尾部进入。
![](https://user-gold-cdn.xitu.io/2018/12/27/167eeca806deec9c?w=540&h=593&f=png&s=57955)
队列移除元素，左侧是队列的头部，右侧是队列的尾部， 如果想要出队列，只能从队列的头部出去
![](https://user-gold-cdn.xitu.io/2018/12/27/167eeca806c410c0?w=540&h=541&f=png&s=47220)
日常生活中，排队就是典型的队列结构，先到的先被服务，后来的在队尾等着，直到轮到他为止（当然，特殊情况除外）。比如说其他场景 提交操作系统执行的一系列进程、打印任务池等，一些仿真系统用队列来模拟银行或杂货 店里排队的顾客。
![](https://user-gold-cdn.xitu.io/2018/12/27/167eeca806f5e49d?w=540&h=336&f=png&s=346440)

#####  2.队列的实现
> 从数据存储的角度看，实现队列有两种方式，一种是以数组做基础，一种是以链表做基础，数组是最简单的实现方式，本文以基础的数组来实现队列。

> 队列的操作包括创建队列、销毁队列、入队、出队、清空队列、获取队头元素、获取队列的长度。

> 我们定义以下几个队列的方法：
>+ enqueue 从队尾添加一个元素（新来一个办业务的人，排在了队尾）
>+ dequeue 从队首删除一个元素（队伍最前面的人，办完了业务，离开了）
>+ head 返回队首的元素（后边的人好奇看一下，队伍最前面的人是谁）
>+ tail 返回队尾的元素（前边的人好奇看一下，队伍最后面的人是谁）
>+ size 返回队列的大小（营业员数一下队伍有多少人）
>+ isEmpty 返回队列是否为空（营业员查看当前是不是有人在排队） 
>+ clear 清空队列（此窗口暂停营业，大家撤了吧） 

然后我们利用es6的class的实现以上的方法
新建一个queue.js文件
```
class Queue {
  constructor() {
    this.items = []; // 存储数据
  }
  enqueue(item) { // 向队尾添加一个元素
    this.items.push(item);
  }
  dequeue() { // 删除队首的一个元素
    return this.items.shift();
  }
  head() { // 返回队首的元素
    return this.items[0];
  }
  tail() { // 返回队尾的元素
    return this.items[this.items.length - 1];
  }
  size() { // 返回队列的元素
    return this.items.length;
  }
  isEmpty() { // 返回队列是否为空
    return this.items.length === 0;
  }
  clear() { // 清空队列
    this.items = [];
  }
}

```
#####  3.队列的应用
记住两点：
>+ 栈的特性是先进后出(联想：羽毛球桶)
>+ 队列的特性是先进先出(联想：排队)

###### 3.1  约瑟夫环问题
有一个数组存放了100个数据0-99，要求每隔两个数删除一个数，到末尾时再循环至开头继续进行，求最后一个被删除的数字。

> 比如说：有十个数字：0，1，2，3，4，5，6，8，9，每隔两个数删除一个数，就是2 5 8 删除，如果只是从0到99每两个数删除一个数，其实挺简单的，但是我们还得考虑到末尾的时候还有再重头开始，还得考虑删除掉的元素从数组中删除。那我们如果队列的话，就比较简单了   

3.1.2 思路分析
>+  先将这100个数据放入队列，用while循环，终止的条件是队列里只有一个元素。
>+ 定义index变量从0开始计数，从队列头部删除一个元素，index + 1
>+ 如果index%3 === 0 ,说明这个元素需要被移除队列，否则的话就把它添加到队列的尾部

经过while循环后，不断的有元素出队列，最后队伍中只会剩下一个被删除的元素

3.1.3 看代码实现
```
// 每隔两个数删除一个数
    {
      var arr = []; // 准备0-99  100个数据
      for (var i = 0; i < 100; i++) {
        arr.push(i);
      }
      function delRang(arr) {
        var queue = new Queue(); // 调用之前实现Queue类
        var len = arr.length;
        for (var i = 0; i < len; i++) {
          queue.enqueue(i); // 将数据存入队列
        }
        var index = 0;
        while (queue.size() !== 1) { // 循环判断队列里大小否为还剩下1个
          var item = queue.dequeue(); // 出队一个元素，根据当前的index来判断是否需要移除
          index += 1;
          if (index % 3 !== 0) {
            queue.enqueue(item); // 不是的话，则添加到队尾，继续循环
          }
        }
        console.log(queue.head()); // 90
        return queue.head(); // 返回最后一个元素
      }
      delRang(arr);
    }
```
是不是感觉使用队列很简单呢，接下来再看几个小练习
###### 3.2  斐波那契数列
3.2.1 题目介绍
> 什么是斐波那契数列： 斐波那契数列（Fibonacci sequence），又称黄金分割数列、因数学家列昂纳多·斐波那契（Leonardoda Fibonacci）以兔子繁殖为例子而引入，故又称为“兔子数列”，指的是这样一个数列：1、1、2、3、5、8、13、21、34、……这个数列从第3项开始，每一项都等于前两项之和。在数学上，斐波纳契数列以如下被以递归的方法定义：F(1)=1，F(2)=1, F(n)=F(n-1)+F(n-2)（n>=3，n∈N*）[来源：斐波那契数列——百度百科](https://baike.baidu.com/item/%E6%96%90%E6%B3%A2%E9%82%A3%E5%A5%91%E6%95%B0%E5%88%97/99145?fr=aladdin)

3.2.2 我们先考虑使用普通的方法实现 -- 递归
 递归版 代码实现
```
function Fibonacci (n) {
  if ( n <= 2 ) {return 1};
  return Fibonacci(n - 1) + Fibonacci(n - 2);
}
Fibonacci(10) // 55
Fibonacci(100) // 堆栈溢出
Fibonacci(500) // 堆栈溢出
```
由上可见，递归非常消耗内存，因为需要同时保存成千上百个调用帧，很容易发生“栈溢出”错误。但是也有解决的办法，采用尾递归优化。
> 函数调用自身，称为递归；如果尾调用自身，就称为尾递归。
对于尾递归来说，由于只存在一个调用栈，所以永远不会发生“栈溢出”错误。

尾递归版 代码实现
```
function Fibonacci2 (n , ac1 = 1 , ac2 = 1) {
  if( n <= 2 ) {return ac2};
  return Fibonacci2 (n - 1, ac2, ac1 + ac2);
}
Fibonacci2(100) // 354224848179262000000
Fibonacci2(1000) // 4.346655768693743e+208
```
上面代码虽然简洁，但是不易想到

3.2.3  那接下来我们用队列实现一遍
 思路分析
>+ 需要先将两个1 添加到队列中
>+ 定义index来计数，采用while循环，终止条件是 index < n - 2(因为每次遍历我们只保留2个元素在队列中)
>+  使用dequeue方法移除队列头部的元素，标记为 numDel;
>+ 使用head方法获取此时头部的元素，标记为numHead;
>+ 使用enqueue方法将前两者的和从尾部放入队列中
>+ index + 1

当循环结束后，队列里面只有两个元素，用dequeue方法移除头部元素后，再用head方法获取的头部元素就是最终的结果，而且此方法不会产生“栈溢出”错误。

队列版 代码实现
```
  {
      function fibonacci(n) {
        if (n <= 2) return 1;
        var queue = new Queue();
        // 先存入序列的前两个值
        queue.enqueue(1);
        queue.enqueue(1);
        var index = 0;
        while (index < n - 2) {
          var delItem = queue.dequeue(); // 移除队列的头部元素
          var headItem = queue.head(); // 获取队列头部元素(因为上一步已经将头部元素移除)
          var resNum = delItem + headItem;
          queue.enqueue(resNum); // 将两者之和存入队列
          index += 1;
        }
        queue.dequeue();
        return queue.head();
      }
      console.log("fibonacci", fibonacci(10)); // 55
      console.log("fibonacci", fibonacci(100)); // 354224848179262000000
    }
```
###### 3.3  打印杨辉三角
3.3.1 题目分析
所谓杨辉三角，大家肯定都不会陌生，如下图所示
[杨辉三角——百度百科介绍](https://baike.baidu.com/item/%E6%9D%A8%E8%BE%89%E4%B8%89%E8%A7%92/215098?fr=aladdin)
计算的方式：f[i][j] = f[i-1][j-1] + f[i-1][j], i 代表行数，j代表一行的第几个数，如果j= 0 或者 j = i ,则 f[i][j] = 1。

![](https://user-gold-cdn.xitu.io/2018/12/27/167eeca807072ee3?w=340&h=229&f=png&s=7430)
3.3.2 思路分析
>+ 杨辉三角中的每一行，都依赖于上一行，假设现在队列里已经存储了第n-1行的数据，那么输出第n行时，只需要将队列里的数据依次出队列，进行计算得到下一行的数值并讲计算所得存储到队列中
>+ 然后我们需要两层for循环，将n-1行和n行的数据分开打印；有上图可以得出规律，n行只有n个数，所以我们就可以使用for循环控制enqueue的次数，n次结束后，队列里存储的就是计算好的第n+1行的数据

3.3.3 代码实现
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>打印杨辉三角</title>
  </head>
  <body>
    <script src="./queue.js"></script>
    <script>
      // 杨辉三角
      {
        function yangHui(n) {
          var queue = new Queue();
          queue.enqueue(1); // 先在队列中存储第一行的数据
          for (var i = 1; i <= n; i++) { // 第一层循环控制层数
            var line = "";
            var pre = 0;
            for (var j = n; j > i; j--) { // 打印空格
              document.write("&nbsp;");
            }
            for (var j = 0; j < i; j++) { // 第二层控制当前层的数据
              var item = queue.dequeue();
              var value = item + pre; // 计算下一行的值
              pre = item;
              line += item + " ";
              queue.enqueue(value);
            }
            queue.enqueue(1); // 将每层的最后一个数值 1 存入队列中
            document.write(line + "<br />");
          }
        }
        yangHui(10);
      }
    </script>
  </body>
</html>
```

#####  4.队列总结
> 使用队列的例子还有很多，比如逐层打印一颗树上的节点，还有消息通讯使用的socket，当大量客户端向服务端发起连接，而服务端拥挤时，就会形成队列，先来的先处理，后来的后处理，当队列满时，新来的请求直接抛弃掉。
数据结构在系统设计中的应用非常广泛，只是我们水平达不到那个级别，知道的太少，但如果能理解并掌握这些数据结构，那么就有机会在工作中使用它们并解决一些具体的问题，当我们手里除了锤子还有电锯时，那么我们的眼里就不只是钉子，解决问题的思路也会更加开阔。

#####  5.参考
[阮一峰-函数的扩展](http://es6.ruanyifeng.com/#docs/function#%E5%B0%BE%E8%B0%83%E7%94%A8%E4%BC%98%E5%8C%96)

