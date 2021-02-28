# JavaScript的函数声明的9种方法

## 1、正常的函数声明

```
function say(){}
function eat(){}
```

如果你以这种方式定义函数的话是不怎么明智的方法，因为随着你没定义一个方法就增加一个变量；如果是多人协作开发，那么遇到相似的功能实现很可能发生函数覆盖，造成程序运行错误。



## 2、用对象收录方法

```
let pain = {
	say:function(){},
	eat:funtction(){}
}
```

一个对象可以有自己的属性和方法，这样可以定义自己独特的方法签名，几乎不会发生函数覆盖的概率。



## 3、函数也是对象哦

```
let pain = function(){}
pain.say = function(){}
pain.eat = function(){}
```

函数function的本事也是对象，所以可以用上面的方法定义；但是它的缺点是不能复用。



## 4、返回带有方法的对象实现复用

```
let pain = function(){
	return {
		say: function(){},
		eat:funtction(){}
	}
}

let a = pain()
a.say()
a.eat()
```

返回带有方法的对象，实现复用。



## 5、通过类的方式实现复用

```
let pain = function(){
	this.eat = function(){}
	this.eat = function(){}
}

let a = new pain()
a.say()
a.eat()
```

这样定义方法，每new一次都要从原来的对象（pain）上复制方法，会有一定的内存消耗，对于一个讲究的人来说，这无疑是十分奢侈的，恰好我就是一个十分不讲究的人。哈哈，逗你的，看下面的方式。



## 6、类方式的优化

```
let pain = function(){}
pain.prototype.say = function(){}
or
pain.prototype = {
	say: function(){},
	eat:funtction(){}
}

注意上面两种方法不要用混淆，请注意JavaScript的继承。
let a = new pain()
a.say()
a.eat()
调用还是一样
```



## 7、对象收录方法升级，链式调用

```
let pain = {
	eat: function(){
		// TODO ...
		return this
	},
	say: function(){
		// TODO ...
		return this
	}
}

pain.eat().say()
```

根据上面方法的升级，此时你会不会感叹，小东西，花样还真多。



## 8、对类原型链的升级，链式调用

```
let pain = function(){}
pain.prototype = {
	eat: function(){
		// TODO ...
		return this
	},
	say: function(){
		// TODO ...
		return this
	}
}

let a = new pain()
a.eat().say()
```



## 9、在Function的原型链添加

```
Function.prototype.addMethodPrototype = function(name,fn){
    this.prototype[name] = fn;
    return this;
}

let method = function(){}
method.addMethodPrototype('checkName',function(){
	// TODO ...
	return this
}).addMethodPrototype('checkPhone',function(){
	// TODO ...
	return this
})

let methodFunc = new method()
methodFunc.checkName().checkPhone()
```



个人学习，如有错误，望指正。