# 不借助中间变量互换2个变量的值

```
let a = 1 , b = 2;
// 1、加减
a = a + b
b = a - b
a = a - b

// 2、位运算
a ^= b;
b ^= a;
a ^= b;
a = (b^=a^=b)^a;



// 3、数组
a = [b,b=a][0]

// 4、对象
a = {a:b,b:a};
b = a.b;
a = a.a;

// 5 数组
a = [a,b];
b = a[0];
a = a[1];

// 6、解构赋值
[a,b] = [b,a]

console.log(`a = ${a} b = ${b}`)
```

