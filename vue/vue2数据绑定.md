# vue2x 数据绑定原理

## *Object*.defineProperty的简介

vue2x数据绑定的原理基于*Object*.defineProperty来实现的，类似在Object上的方法还有很多，只是我们日常用的很少，这些应该可以归纳到JavaScript高级特性里，下面简单用代码说明

```
function updateView(){
    console.log('视图跟新')
}
function defineReactive(target,key,value){
    observer(value)
    Object.defineProperty(target,key,{
        get(){
            return value
        },
        set(newValue){
            if(newValue !== value){
                observer(newValue)
                value = newValue
                updateView()
            }
        }
    })
}

function observer(target){
    if(typeof target !== "object" || target === null){
        return target
    }

    for(let key in target){
        defineReactive(target,key,target[key])
    }
}

const data = {
    name:'pf',
    age:20,
    info:{
        sex:'man'
    },
}

observer(data)

data.name = 'lisi'
data.age = 21
data.info.sex = 'aaa'

```

上述代码可以放到node执行，也可以当做script脚本在html引用执行，应为没有涉及dom，bom操作，只是JavaScript代码片段，所以在哪执行都可以。


看上面代码可以看出一些简单的问题，将一个对象用for key in 循环遍历，将属性传入Object.defineProperty
实现数据绑定，当对象的层级很深时（就是对象里面有对象...）,代码逻辑是无限递归，虽然js运行速度比较快，但是当项目庞大后，其效率还是堪忧的，另外可以看到data数据里并没有数组，应为Object.defineProperty不可以代理数组，所以需要重写代理数组的方法，综上：
1、有一定的效率问题，当代码层级很深时，递归是十分奢侈的；
2、Object.defineProperty并不能对数组进行数据代理，所以要重写数组相关逻辑代码。

```
const arrayProperty = Array.prototype
const arryProto = Object.create(arrayProperty);

function updateView(){
    console.log('视图跟新')
}

function defineReactive(target,key,value){
    observer(value)
    Object.defineProperty(target,key,{
        get(){
            return value
        },
        set(newValue){
            if(newValue !== value){
                observer(newValue)
                value = newValue
                updateView()
            }
        }
    })
}


['push','pop','shift','unshift','splice'].forEach(methodName => {
    arryProto[methodName] = function(){
        updateView()
        arrayProperty[methodName].call(this,...arguments)
    }
})

function observer(target){
    if(typeof target !== "object" || target === null){
        return target
    }

    if(Array.isArray(target)){
        target.__proto__ = arryProto
    }
    
    for(let key in target){
        defineReactive(target,key,target[key])
    }

}

const data = {
    name:'pf',
    age:20,
    info:{
        sex:'man'
    },
    nums:[1,2,3,4]
}

observer(data)

data.name = 'lisi'
data.age = 21
data.info.sex = 'aaa'
data.nums.push(1)
```

以上代码对数组代理进行了一定的封装，声明一个新的变量接收数组原型然后在操作。其原因是如果直接用数组原型会污染原数组的方法，后果是十分严重的。

vue的核心便是数据驱动视图，我们不在关心dom操作和怎么优化dom操作，而是只关心数据书如何变幻，数据变化后vue会把数据整合后，利用diff算法生成虚拟dom，在将虚拟dom渲染到页面上，所以在数据变化到视图渲染这一过程是一个异步过程，只用这样才能减少dom操作，节约浏览器性能。
vue的数据绑定的小demo就是上面一段代码了，真是的vue代码要远远复杂的多，考虑的也更多。