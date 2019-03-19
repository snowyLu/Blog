
今天稍微停下前进的脚步，来看下队栈的左右互搏术。
前两天学习了队列和栈以后，今天就可以试着来用两个栈实现队列的功能或者用两个队列来实现栈的功能。




## 1. 用两个栈实现一个队列



### 1.1 题目分析


  
  栈是先进后出，队列是先进先出，但可以用两个栈来模拟一个队列的功能，来实现队列中主要的 enqueue，dequeue， head 方法。


### 1.2 思路分析
 我们所学的每一种数据结构，本质上都是对数据如何存储和使用的研究，这就必然涉及到增删改查，那么考虑实现这些方法时，我们优先考虑如何实现数据的增加，只有存在了数据，才能够后续的操作。
所以如何实现队列中的增加数据方法 enqueue 呢？
- 给两个栈分别命名为 stack1，stack2。那有了这两个栈以后，可以选取其中一个来存储数据，比如说 stack1，那么队列的 enqueue 方法就很容易了，直接利用栈的 push 方法就能够添加数据。

接下来考虑队列中的删除 dequeue 方法

- 首先要注意下 dequeue 方法是删除队列中的头部元素，而此时队首是在 stack1 栈底的，目前来说还取不到。
- 这个时候 stack2 该上场了，可以把 stack1 中的元素都依次移除并压入 stack2 中，这样的话，stack2 的栈顶就变成了队首，不就可以利用 stack2 的 pop 方法来移除元素了嘛。

那队列的 head 方法呢

- 执行完 stack2 的 pop 方法后，还需要把数据再移回 stack1 里吗？
  其实不需要了，因为此时队首正好是 stack2 的栈顶，而队列的 head 方法就可以利用栈的 top 方法来实现了。
- 如果 stack2 是空的怎么办？那 stack1 的元素都移除到 stack2 就可以了。
- 如果 stack1 也是空的呢，那就说明队列中没有元素了，此时返回 null 就可以了。

注意到了吗，这里又用到了 ***分而治之*** 的思想，还记得之前在哪里用过吗？
对，就是在给栈添加获取最小值方法的时候用过，当时也是用了两个栈来实现。
这里的话 enqueue 始终都操作 stack1，dequeue 和 head 方法始终都操作 stack2。

### 1.3 代码实现




```
{
        class StackQueue {
          constructor() {
            this.stack1 = new Stack();
            this.stack2 = new Stack();
          }
          // 初始化stack，伪造私有方法
          _initStack() {
            if (this.stack1.isEmpty() && this.stack2.isEmpty()) {
              return null; // 如果两个栈都是空的，那么队列中就没有元素
            }
            if (this.stack2.isEmpty()) {
              // 如果stack2是空的，那么此时stack1一定不为空
              while (!this.stack1.isEmpty()) {
                this.stack2.push(this.stack1.pop()); // 把stack1的元素移除到stack2中
              }
            }
          }
          // 向队尾添加一个元素
          enqueue(item) {
            this.stack1.push(item); // 把数据存入到stack1中
          }
          // 删除队首的一个元素
          dequeue() {
            this._initStack();
            return this.stack2.pop();
          }
          // 返回队首的元素
          head() {
            this._initStack();
            return this.stack2.top();
          }
        }
        var stackQueue = new StackQueue();
        stackQueue.enqueue(1);
        stackQueue.enqueue(4);
        stackQueue.enqueue(8);
        console.log(stackQueue.head()); // 1
        stackQueue.dequeue();
        stackQueue.enqueue(9);
        console.log(stackQueue.head()); // 4
        stackQueue.dequeue();
        console.log(stackQueue.head()); // 8
        console.log(stackQueue.dequeue()); // 8
        console.log(stackQueue.dequeue()); // 9
      }
```
是不是觉得很简单呢，梳理清楚队列和栈的特性就OK啦。
接下来让我们继续修炼，用队列实现栈吧！


## 2. 用两个队列实现一个栈

 
### 2.1 题目分析
 
队列是先进先出，栈是先进后出，(不断重复这两个知识点) 但可以用两个队列来模拟一个栈的功能，来实现栈中主要的 push，pop， top 方法。

你可能会想到利用上边的套路来实现这个需求，但是最后的结果你会发现是不正确的。因为 把 stack1 的元素移除到 stack2 中，此时的两个栈中的数据就首尾交换了，而如果此处换成队列 this.queue2. enqueue(this.queue1. dequeue())，
你会发现由于队列的特性，此时的两个队列还是一样的，首尾并没有交换。

so 我们来换个思路


 ### 2.2 思路分析
和上边一样，我们先考虑如何实现栈的存储数据 push 方法:
-   给两个队列分别命名为 queue1，queue2。实现 push 方法时，利用队列的 enqueue 方法，如果两个队列都为空，那么默认向 queue1 里添加数据；如果有一个不为空，那么就向这个不为空的队列里添加数据。

top 方法就简单了:
-  利用队列的 tail 方法，两个队列要么都为空，要么有一个不为空，那么返回不为空队列的尾部元素就是栈顶元素了.

接下来思考比较复杂的 pop 方法:

-  pop 方法删除的是栈顶，但此时栈顶元素是队列的尾部元素，而队尾元素是不能删除的。
-  但每次执行 pop 时，可以将不为空的 a 队列里的元素循环删除并放入到另一个 b 队列中，直到 a 队列中只剩下一个元素，此时 a 队列的这个元素就是队列的尾部元素，也就是栈顶元素了，那 pop 方法就简单了，利用 a 队列的 dequeue 方法就可以了。

在具体的实现中，需要额外定义两个变量，dataQueue 和 emptyQueue:

-  dataQueue 始终指向那个不为空的队列
-  emptyQueue 始终指向那个为空的队列
 
 
 ### 2.3 代码实现
```
  {
      class QueueStack {
        constructor() {
          this.queue1 = new Queue();
          this.queue2 = new Queue();
          this.dataQueue = null; // 存放数据的队列
          this.emptyQueue = null; // 存放备份数据的队列
        }
        // 初始化队列数据，模拟私有方法 确认哪个队列存放数据，哪个队列做备份 
        _initQueue() {
          if (this.queue1.isEmpty()) {
            this.dataQueue = this.queue2;
            this.emptyQueue = this.queue1;
          } else {
            // 都为空的话 默认是 队列1
            this.dataQueue = this.queue1;
            this.emptyQueue = this.queue2;
          }
        }
        // 往栈里压入一个元素
        push(item) {
          this._initQueue();
          this.dataQueue.enqueue(item);
        }
        // 返回栈顶的元素
        top() {
          this._initQueue();
          return this.dataQueue.tail();
        }
        // 把栈顶的元素移除
        pop() {
          this._initQueue();
          while (this.dataQueue.size() > 1) {
            // 利用备份队列转移数据，
            this.emptyQueue.enqueue(this.dataQueue.dequeue()); // 数据队列和备份队列交换了身份
          }
          return this.dataQueue.dequeue(); // 移除数据队列的头部元素
        }
      }
      var queueStack = new QueueStack();
      queueStack.push(1);
      queueStack.push(2);
      queueStack.push(4);
      console.log(queueStack.top()); // 栈顶是 4
      console.log(queueStack.pop()); // 移除 4
      queueStack.push(5);
      console.log(queueStack.top()); // 栈顶变成 5
      queueStack.push(6);
      console.log(queueStack.pop()); // 移除 6
      console.log(queueStack.pop()); // 移除5
      console.log(queueStack.top()); // 栈顶是 2
    }
```
如果你有其他好的方法，欢迎留言

## 总结
 我们利用基础的数组 api 实现了队列和栈的功能，再来回顾下（敲黑板，划重点了）

-  队列最大的特点就是先进先出，主要的两个操作是入队和出队，和栈一样，是个受限制的数据结构。
-  队列既可以用数组实现，也可以用链表实现，用数组实现的叫顺序队列，用链表实现的叫链式队列(后续更新)
-  栈最大的特点就是先进后出，主要的两个操作是出栈和入栈，也是一个受限制的数据结构。
-  和队列一样，既可以用数组实现，也可以用链表实现。不管基于数组还是链表，入栈、出栈的时间复杂度都为 O(1)，队列也是如此。

当然还有其他的队列和栈，我这里只介绍到了基础的实现。这个硬骨头，还需慢慢啃。

## 重点

如果有错误或者错别字，还请给我留言指出，谢谢。


