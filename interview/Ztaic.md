# 云轴面试

# 1、面试心得

云轴是一家云计算公司，在其他地方也有分公司，公司人数也很多，这次面试时我第二次来面试了，哈哈。

大概面了一半小时左右，面试我的小哥年纪和我差不多大，整个面试也十分融洽，在上海的朋友可以去面一面，薪资待遇什么都很棒。

我对这次面试的心得总结如下：

1、技术栈广度：你掌握的越多固然越好，但前提你需要精通。

2、技术栈深度：我认为深度的优先级要高于广度，不然就会和我一样，都会都不会。

3、基础知识：基础知识要扎实，每个人都知道，能做的却不多，基础不会会在面试大打折扣。

4、算法与数据结构：可以算是锦上添花，但是面试都会问，体现你的深度。

5、我有段时间没有面试了，面试就像照妖镜，你是什么妖怪，一照便知；面试也像磨刀石，多经历几次，善于总结，离offer也就不远了。太多的人不愿面试的根本原因还是不自信，或者心理因素。成为一个强大的人这是必不可少的。我的建议是多面试，每个月面两到三次。

# 2、面试题分享

## 1、JavaScript实现sleep（等待一定时间）

```
1、利用时间等待
function sleep(d){
    for(var t = Date.now();Date.now() - t <= d;);
}
sleep(5000)

2、利用promise
function sleep(ms) {
  return new Promise(resolve => 
      setTimeout(resolve, ms)
  )
}
sleep(3000).then(()=>{
   //code
})

3、使用node-sleep
let sleep = require('sleep');
let n=10;

sleep.sleep(n)

我认为阻塞程序，还是需要像while()循环或for循环来阻塞（注：for循环要比while的效率要高）
如果利用promise来阻塞的话和普通的setTimeout，没有区别
```



## 2、数组去重

```
let obj = {}
let res1 = []
let res2 = []

a.concat(b).forEach(a =>{
    if (!obj[a]){
        obj[a] = 1
        res1.push(a)  
    }else{
        obj[a]++
        res2.push(a)  
    }
})
console.log(obj)
console.log(res1)
console.log(res2)

obj : { '1': 1, '2': 2, '3': 2, '4': 1 }
res1 : [ 1, 2, 3, 4 ]   去重
res2 : [ 2, 3 ]			交集
```



## 3、算法（请移步算法那篇博客）

## 4、http的三次握手与四次挥手

### 建立链接（三次握手）

1、client发起连接请求，发送SYN标志位携带client数据包号（随机）

2、server接收到包，返回SYN标志位+server数据包号（随机）；ACK应答+（client数据包号+1）

3、client收到包，返回ACK应答+（server数据包号+1）

### 断开链接（四次握手）

1、client发起连接请求，发送FIN标志位携带client数据包号（随机）

2、server接收到包，发送ACK应答+（client数据包号+1）

3、server发送ACK应答+（server数据包号+1）

4、client收到包，返回ACK应答+（server数据包号+1）



## 5、Vue与React的diff算法有什么区别

diff算法的目的是比较新旧虚拟dom，传统的diff算法的时间复杂度为O(n^3),可以想象的到O(n^3)的是时间复杂度还是很高的，所以React与Vue的diff算法都改进了很多，使得时间复杂度降为O(n),但略有不同。

### React：

**树的diff：**跨层级的dom操作很少，可以忽略不计

1、通过uodateDepth对虚拟数进行层级控制

2、对两棵树同一层级进行比较，节点不存在就直接删掉，遍历一次就能完成比较

3、如果出现跨层操作，比如`a`从原位置移动到`b`位置，就删除原位置的`a`，在`b`新建`a`，所以a以下的树会被重建，所以官方不建议跨层操作，建议通过隐藏显示来操作，比如**visibility:hidden**

**组件diff：**相同类型组件生成相同树结构；不同类型组件生成不同树结构

1、同一类型的组件，按原策略（层级比较）

2、同一类型的组件，`a`变化时，虚拟DOM没有变，可以在这里进行`shouldComponentUpdate`操作

3、如果被判定为不同类型的组件，删除原组件，构建新组件

**元素diff：**同一层级的一组子节点，通过唯一id进行区分

1、插入：如果元素不在原集合，就插入

2、删除：`d`在集合中，但是`d`节点被更改，不能更新复用，就删除重建；或者d没有了，直接删除

3、移动：`d`在集合中，并且没有变化，只是换了位置，通过key来区分并移动。**所以通过map出来的元素，如果不加key，就会报错**

:warning:比较元素的新旧index：lastIndex，index；只有index<lastIndex才会移动，也就是往右移动，往左不移动。所以！！如果在一长串集合中，如果最后一个元素移动到第一个，前面的所有元素都会移动，性能不佳要尽量避免

**所以不难理解为什么不要把index作为key值（当数组变化了，index也会变化，从而lastIndex和index不再能正确代表新旧index），应该用元素的唯一标识id作为key值**



### Vue：

1、虚拟DOM是将真实的DOM的数据抽取出来，以对象的形式模拟树结构。（虚拟DOM和oldVNode都是对象）

2、比较新旧节点的时候，比较只会在同层级进行，不会跨层级比较。

3、当数据改变，`set`方法会通知所有订阅者`watcher`，订阅者会调用`patch(oldVnode, Vnode)`给真实的DOM打补丁，更新相应的视图。是否是同一个VNode？不是就替换，是就继续进行patch

4、patch接收`oldVnode和Vnode`来代表新的节点和之前的旧节点

4.1、判断两节点是否值得比较，值得就继续比较；不值得直接替换

4.2、当确定值得比较后，会对两个节点指定patchVnode方法

4.3、找到对应的真实DOM，称`el`，判断Vnode和oldVnode是否指向同一对象，如果是直接return

4.4、都有文本对象且不相等，就将el的文本节点设置为Vnode的文本节点

4.5、如果oldVnode没有子节点，而Vnode有，将Vnode子节点真实化后添加到el

4.6、如果两个都有子节点，执行`updateChildren`函数比较子节点

4.7、oldCh旧的子节点，newCh新的子节点提取出来，oldCh和newCh各有两个头尾变量，startIdx和EndIdx；2个变量互相比较。涉及到4种比较方式，如果4种都不成功，**就用key**来比较，比较过程中，变量会向中间靠拢，一旦startIdx>endIdx，表明oldCh和newCh至少有一个已经遍历结束。**如果old先结束，那么newCh中的节点按照其index插入到DOM中去；如果newCh先遍历完，就将真实DOM中多余的节点删掉**



## 6、对HTTP2的了解

1.解析数据的开销很低 - 这是HTTP / 2与HTTP1的关键价值主张。

2.不容易出错。

3.更轻的网络足迹。

4.有效的网络资源利用率

5.消除与HTTP1.x的文本性质相关的安全问题，例如响应分裂攻击。

6.启用HTTP / 2的其他功能，包括压缩，多路复用，优先级排序，流量控制和TLS的有效处理。

7.紧凑的命令表示，便于处理和实现。

8.在客户端和服务器之间处理数据方面高效且稳健。

9.减少网络延迟并提高吞吐量。



## 7、连续发送2次http请求，经历几次握手与挥手，能不能不挥手

http被成为无状态协议，指的是该协议对于交互性场景没有记忆能力。所以在没有开启Keep-Alive模式时，浏览器还是会请求两次。

### Keep-Alive模式：

我们知道Http协议采用“请求-应答”模式，当使用普通模式，即非Keep-Alive模式时，每个请求/应答，客户端和服务器都要新建一个连接，完成之后立即断开连接；当使用Keep-Alive模式时，Keep-Alive功能使客户端到服务器端的连接持续有效，当出现对服务器的后继请求时，Keep-Alive功能避免了建立或者重新建立连接。
http1.0中默认是关闭的，需要在http头加入”Connection: Keep-Alive”，才能启用Keep-Alive；
http 1.1中默认启用Keep-Alive，如果加入”Connection: close “才关闭。目前大部分浏览器都是用http1.1协议，也就是说默认都会发起Keep-Alive的连接请求了，所以是否能完成一个完整的Keep- Alive连接就看服务器设置情况。



## 8、数据库索引有什么用，怎么创建索引

数据库索引的作用当然是快速查询，但更深层次的问题就是关于算法了，

非聚集索引：日常用的索引

聚集索引：主键

我们先引入几个问题：

### 1、为什么要给表加上主键？

主流索引的原理是平衡树（B+ 树），也有用哈希桶作为索引的数据结构

一张表没有主键，数据库会拒绝执行建表语句，没加主键的表，数据会无序且整齐的排列在磁盘上

当给表加了主键后，数据就会像树状结构（B+树）。这是表就成了一个聚集索引，这也是为什么一个表只能有一个主键，一个表只能有一个聚集索引。

### 2、为什么加索引后会使查询变快？

不加索引在最坏的情况下时间复杂度是O(n),在海量数据下这是非常恐怖的。

加了索引情况下的时间复杂度是 O(logn)

### 3、为什么加索引后会使写入、修改、删除变慢？

加了索引的表就是平衡树，为了维持在一个正确的状态，增删改数据都会改变平衡树节点的索引类容，破坏了树结构，所以DBMS必须重新梳理索引确保平衡树的状态，这会带来不小的性能开销



## 9、BFC是什么，怎么清除

BFC(Block formatting context)直译为"块级格式化上下文"。

它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的元素如何布局，并且与这个区域外部毫不相干。

###  BFC的布局规则

1、内部的Box会在垂直方向，一个接一个地放置。

2、Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠。

3、每个盒子（块盒与行盒）的margin box的左边，与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。

4、BFC的区域不会与float box重叠。

5、BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。

6、计算BFC的高度时，浮动元素也参与计算。

### 如何创建BFC

1、float的值不是none。
2、position的值不是static或者relative。
3、display的值是inline-block、table-cell、flex、table-caption或者inline-flex
4、overflow的值不是visible

### 如何清除浮动

**1、使用伪元素来清除浮动(:after,注意：作用于浮动元素的父亲）**

```css
.clearfix:after{
    content:"";  /*设置内容为空*/
    height:0;  /*高度为0*/
    line-height:0;  /*行高为0*/
    display:block;  /*将文本转为块级元素*/
    visibility:hidden;  /*将元素隐藏*/
    clear:both;  /*清除浮动*/
}
.clearfix{
    zoom:1;  /*为了兼容IE*/
}
```

**2、添加新的元素对其应用 clear:both**

```jsx
//对添加的元素使用 clear:both
.clear{clear:both;}
<div class="box">
    <div class="red" style="float:left;">1</div>
    <div class="sienna" style="float:left;">2</div>
    <div class="blue" style="float:left;">3</div>
    //添加一个新元素
    <div class="clear"></div>
</div>
```

**3、父级div定义overflow:hidden或auto**

```jsx
.over-flow{ overflow:hidden; zoom: 1;}/*zoom1; 是在处理兼容性问题*/
.over-flow{ overflow:auto; zoom: 1;}
<div class="box over-flow">
    <div class="red">1</div>
    <div class="sienna">2</div>
    <div class="blue">3</div>
</div>
```

**4、父级div使用双伪元素清除浮动**

```jsx
.clearfix:before,.clearfix:after {
        display: table;
        content: "";    /*不用有内容也可以*/
    }

.clearfix:after {
    clear: both;
}

.clearfix {
    *zoom: 1;
}
```

## 10、Vue搜索框怎么避免连续点击

自定义指令

```
export default {
    install (Vue) {
      // 防重复点击(指令实现)
      Vue.directive('preventReClick', {
        inserted (el, binding) {
          el.addEventListener('click', () => {
            if (!el.disabled) {
              el.disabled = true
              setTimeout(() => {
                el.disabled = false
              }, binding.value || 3000)
            }
          })
        }
      })
    }
  }
  
在main.js中引入
import preventReClick from '../plugins/plugins.js'

在需要的页面中引入
import preventReClick from '../../plugins/plugins'

在按钮上加上 v-preventReClick 就好了
<el-button @click="func()" v-preventReClick></el-button>
```

## 11、有没有上传docker image，有没有写过dockerfile，怎么把类容复制到docker里面

```
 # push 镜像到 docker hub
docker push myImage
```

dockerfile很繁琐pass

至于把类容复制到docker里，我的方法是启动docker是用命令把需要的文件映射到docker容器里。如果是数据库镜像我们也需要把docker 镜像里的mysql实例保存的数据映射出来，方便备份等操作



## 12、js的原型链指向与继承

在目前上手的集中语言里就TM的JavaScript的继承最搞心态，

原型链构造器原型指向一通乱指。

组合继承：

```
function Person(){}
Person.prototype.say = function (){}

function Kind(){}
Kind.prototype = new Person();  
//此时 Student.prototype 中的 constructor 被重写了，会导致 stu1.constructor === Person

Kind.prototype.constructor = Student;  
//将 Student 原型对象的 constructor 指针重新指向 Student 本身
别问为什么，记住就行了。
```

es6继承

```
class Person {
    constructor(a){
        this.a = a;
    }
    say(){}
}
a
class Kind extends Person{
    constructor(a,b){
        super(a);
        this.b = b;
    }
    say(){
    	// 可以重写
    }
}

/*
* ES5的继承实质是先创建子类的实例对象this，然后再将父类的方法添加到this上面(Parent.apply(this))
* ES6的继承实质是先将父类实例对象的属性和方法加到this上面(所以需先调用super方法)，然后再用子类构造函数修改this
*/
```



## 13、对React Hooks的了解

不是几句话可以说玩的，具体参见：

[]: https://segmentfault.com/a/1190000019513907



## 14、对函数式编程的了解

我个人感觉React编程就是函数式编程最好的范本，每个组件既是一个class或一个function，相比于Vue，这也是React上手要难一点，门槛高一点。

## 16、Vue2X与Vue3的数据绑定怎么实现的，有何优缺点

详见Vue模块



## 17、对Vuex的了解



## 18、Vue生命周期

#### 19、Vue组件传值，方法调用，自定义组件

#### 20、HTML页面的重绘与重排

#### 21、Http响应码

#### 22、Webpack了解

#### 23、Vue性能优化

#### 24、Promise、Generate 函数、Async 函数相关问题，和执行顺序

#### 25、算机网络原理（四层模型及其作用）

#### 26、TypeScript的了解与看法

#### 27、对跨域的了解与解决办法

#### 28、服务端并发、epoll的了解

大概就是上面这些吧，目前能想到的就这些。