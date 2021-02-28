1、Promise

```
1、由来：Promise是解决异步编程的一种方案，比回调函数要合理的多，最早在社区提出和实现，在ES6被写进标准语言。提供原生Promise对象。

2、Promise的三种状态：
pending		进行中
fulfilled	成功
rejected	失败
其状态的改变只由异步操作的结果可以改变，不受其他外界影响。
状态一但改变就不可逆，一点发生就不可停止，如果不设置回调函数，Promise会内部抛出错误，有些反复不断的stream要比Promise要好。
stream：
    fs.createWriteStream()
    fs.createReadStream()
    net.Socket

3、ES6规定Promise是一个构造函数，参数是接受一个函数参数分别是resolve与reject两个js引擎提供的函数。

4、Promise.prototype.then()、Promise.prototype.catch()、Promise.prototype.finally()
Promises实例有then，就说明then是定义在Promise原型上的，then最后会返回一个新的Promise实例，所以可以链式调用，第一次回调的结果会作为第二个回调的参数传入。
catch则是发生错误触发的，什么时候触发，当resolve(null)或resolve（uundefined）时触发
finally则是队后状态，无论成功失败都会触发，与状态无关

5、Promise API
Promise.all([p1,p2,p3])当p1,p2,p3都执行完毕后才会向后执行，都fulfilled则resolve，有一个rejected则reject（catch）

Promise.race([p1,p2,p3])只有有一个ok，就ok

Promise.allSetld([p1,p2,p3])es2020
不管其中失败还是成功，全部等待结束后状态变为fulfilled不会变成rejected弥补Peromis.all的不足

Promise.any()还在提案

Promise.resolve与Promise.reject直接返回Promise实例，状态是resolve或者是reject


```

2、Vue传值

3、权限管理

4、ES6

5、盒子居中

6、Vuex

```

```



7、手写intanceof

8、手写栈

9、反转链表

10、链表和数组区别

11、css扇形

12、手写v-model

13、webpack优化打包速度

14、Vue3 Proxy

15、手写es5继承，及缺点

16、csrf攻击

17、tcp握手

18、单例模式实现（闭包）