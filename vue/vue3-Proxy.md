# Vue3数据绑定原理

上篇我们说到vue2X数据绑定原理及其缺陷后，尤大在vue3使用数据绑定的方式换了。

vue3中使用Proxy对数据进行代理，Proxy的优点是更高效，可以对数组有原生支持。

话不说，看代码：

```
function reactive(target = {}){
    if (typeof target !== 'object' || target == null){
        return target
    }

    const proxyConf = {
        get(target,key,receiver){
            // 只处理原型属性
            const ownKeys = Reflect.ownKeys(target)
    
            if (ownKeys.includes(key)) {
                console.log(`get--->${key}`)
            }
            const result = Reflect.get(target,key,receiver)
            return reactive(result)
        },
        set(target,key,val,receiver){
            // 判断前后数据不重复修改
            if (target[key] === val) {
                return true
            }

            const ownKeys = Reflect.ownKeys(target)
            if (ownKeys.includes(key)){
                console.log(`已有的key：${key}`)
            } else {
                console.log(`新增的key：${key}`)
            }
            
            const result = Reflect.set(target,key,val,receiver)
            console.log(`set:${key}----->${val}`)
            console.log(`result--->${result}`)
            return result
    
        },
        deleteProperty(target,key){
            const result = Reflect.deleteProperty(target,key)
            console.log(`deleteProperty--->${val}`)
            console.log(`result--->${result}`)
            return result
        },
    }

    const oberserved = new Proxy(target,proxyConf)

    return oberserved
}

const data = {
    name:'pf',
    age:20,
    info:{
        city:'上海'
    }
}

const proxyData = reactive(data)
console.log(proxyData.name)
console.log(proxyData.info.city)
```

上述代码是否要比*Object*.defineProperty更优雅一点，

Proxy和*Object*.defineProperty都是大家不常用的JavaScript高级特性，理由这些搞基特性加上设计模式可以玩出很多意想不到的新花样。