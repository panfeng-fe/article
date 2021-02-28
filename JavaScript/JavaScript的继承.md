### ES5继承与原型链

```
function Person(name,age){
        this.name = name;
        this.age = age;
        Person.prototype.say = function(){
            console.log(`my name is ${this.name} and ${this.age} years`)
        }
    }

    let p1 = new Person('pfzz',25) 
    // prototype是函数对象的属性,其本质是 一个对象,包含了__proto__与constructor,
    // 只看普通对象p1，p1只有__proto__,并且指向创建它的函数对象的prototype
    console.info(p1)
    console.info(p1.__proto__ === Person.prototype)
    console.info(p1.__proto__.constructor === Person)
    console.info(p1.__proto__.__proto__ === Object.prototype)
    console.info(p1.__proto__.__proto__.constructor === Object)
    console.info(p1.__proto__.__proto__.__proto__ === null)



    // 函数对象Person，有__proto__、prototype属性
    console.info(Person.__proto__ === Function.prototype)
    console.info(Person.__proto__.constructor === Function)
    console.info(Person.__proto__.__proto__ === Object.prototype)
    console.info(Person.__proto__.__proto__.constructor === Object)
    console.info(Person.__proto__.__proto__.__proto__ === null)

    console.info(Person.prototype.constructor === Person)
    console.info(Person.prototype.__proto__ === Object.prototype)
    console.info(Person.prototype.__proto__.constructor === Object)
    console.info(Person.prototype.__proto__.__proto__ === null)
    
    // 综上具有在指向的只有__proto__与constructor属性，
    // 而constructor属性总是指向prototype所在的函数对象，形成闭环。
    // __proto__最终总是指向null

    // 类式继承
    function Child1(name,age,sex){
        this.sex = sex;
        const prototype = new Person(name,age);
        prototype.play = function(){
            console.log(`my name is ${this.name} and ${this.age} years and ${this.sex}`)
        };
        this.__proto__ = prototype
    }
    const kind1 = new Child1('pfzzz',25,'man')
    // kind1.say()
    // console.info(kind1)
    // console.info(kind1.__proto__ !== Child1.prototype)
    // 缺点：继承属性被放在原型链上，导致kind1.__proto__ !== Child1.prototype

    // 2、构造函数式继承
    function Child2(name,age,sex){
        this.sex = sex;
        Person.call(this,name,age)
    }

    const kind2 = new Child2('pfzzz',25,'man')
    // console.info(kind2)
    // kind2.say()
    // 缺点：原型链上的方法违背继承

    // 组合式继承
    function Child3(name,age,sex){
        this.sex = sex;
        Person.call(this,name,age)  // 第一次
    };
    Child3.prototype = new Person() // 第二次
     
    const kind3 = new Child3('pfzzz',25,'man')
    // console.info(kind3)
    // kind3.say()
    // 缺点：子类无法传递动态参数给父类，且父类构造函数被调用两次,问题不大。

    // 原型继承
    // function Child4(o){
    //      function f(){}
    //      f.prototype = o
    //      return new f()
    // }
    // kind4.prototype = Child4(Person)

    function inherit(child, parent) {
        // 将父类原型和子类原型合并，并赋值给子类的原型
        child.prototype = Object.assign(Object.create(parent.prototype), child.prototype)
        // 重写被污染的子类的constructor
        p.constructor = child
    }

    function fancyShadowMerge(target, source) {
        for (const key of Reflect.ownKeys(source)) {
            Reflect.defineProperty(target, key, Reflect.getOwnPropertyDescriptor(source, key))
        }
        return target
    }

// Core
function inherit(child, parent) {
    const objectPrototype = Object.prototype
    // 继承父类的原型
    const parentPrototype = Object.create(parent.prototype)
    let childPrototype = child.prototype;
    // 若子类没有继承任何类，直接合并子类原型和父类原型上的所有方法
    // 包含可枚举/不可枚举的方法
    if (Reflect.getPrototypeOf(childPrototype) === objectPrototype) {
        	child.prototype = fancyShadowMerge(parentPrototype, childPrototype)
        } else {
            // 若子类已经继承子某个类
            // 父类的原型将在子类原型链的尽头补全
            while (Reflect.getPrototypeOf(childPrototype) !== objectPrototype) {
            childPrototype = Reflect.getPrototypeOf(childPrototype)
        }
        Reflect.setPrototypeOf(childPrototype, parent.prototype)
    }
    // 重写被污染的子类的constructor
    parentPrototype.constructor = child
}
```

### ES6继承与原型链

```
class A{}
class B extends A{}


```

