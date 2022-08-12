---
theme: fancy
highlight: xcode
---
## Object常用方法
### 1. Object.keys
 **返回包含对象所有属性的数组**
```
    var cat= { 
        name:'mini', 
        age:2, 
        color:'yellow', 
        desc:'cute' 
    }
    console.log(Object.keys(cat)); // ["name", "age", "color", "desc"]
```
### 2. object.values
**返回包含对象中所有键值的数组**
```
var obj = { foo: "bar", baz: 42 }; 
Object.values(obj) // ["bar", 42]
```
如果对象中的属性为数字，返回的数组会按属性值从小到大排列：
```
var obj = { 100: 'a', 2: 'b', 7: 'c' }; 
Object.values(obj) // ["b", "c", "a"]
```
对象为字符串时，会返回所有的字符：
```
Object.values('foo') // ['f', 'o', 'o']
```
### 3. Object.create
**使用指定的原型对象及其属性去创建一个新的对象**
```
var parent = {
    x: 1,
    y: 1
}
var child = Object.create(parent, {
    z: {                           // z会成为创建对象的属性
        writable: true,
        configurable: true,
        value: "newAdd"
    }
});
console.log(child)
```

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a934222ccbb34984b48c684fd4c9bc33~tplv-k3u1fbpfcp-watermark.image?)

### 4. Object.hasOwnProperty()
**判断对象自身属性中是否具有指定的属性**
```
var obj = { name:'fei' } 
console.log(obj.hasOwnProperty('name'))//true
console.log(obj.hasOwnProperty('toString'))//false
```

### 5. Object.assign

**用于对象拷贝**
    Object.assign方法实行的是**浅拷贝**，而不是**深拷贝**。
```
var target = { a: 1 }; 
var source1 = { b: 2 }; 
var source2 = { c: 3 }; 
Object.assign(target, source1, source2); 
target // {a:1, b:2, c:3}
```
## 对象遍历
### 1. for in
`for in` 循环是最基础的遍历对象的方式，它还会得到**对象原型链**上的属性。
```
const obj = Object.create({ bar: 'bar' })
obj.foo = 'foo' 

for (let key in obj) 
{ 
    console.log(obj[key]) // foo, bar 
}

```
上面的例子中，对象原型的属性也被遍历出来了, 这个时候可以使用上面提到的`hasOwnProperty`来过滤。
```
for (let key in obj) 
{ 
    if (obj.hasOwnProperty(key))
        { 
            console.log(obj[key]) // foo 
        } 
}
```
### 2. Object.keys (见Object常用方法)

### 3. Object.getOwnPropertyNames
该方法返回对象自身属性名组成的数组，包括不可枚举的属性。
```
// 创建一个对象并指定其原型，bar 为原型上的属性
// baz 为对象自身的属性并且不可枚举
const obj = Object.create({
 bar: 'bar'
}, {
 baz: {
  value: 'baz',
  enumerable: false
 }
})
obj.foo = 'foo'
```
```
Object.getOwnPropertyNames(obj).forEach((key) => {  
    console.log(obj[key]) // baz, foo ,包含了不可枚举的属性
})
```
用forEach可以遍历不包含不可枚举属性。
```
Object.keys(obj).forEach((key) => {
 console.log(obj[key]) // foo
})
```
### 4. Reflect.ownKeys
返回一个数组,包含对象自身的所有属性,不管属性名是Symbol或字符串,也不管是否可枚举。
```
Reflect.ownKeys(obj).forEach((key) => {
 console.log(obj[key]) // baz, foo, Symbol baz, Symbol foo
})
```
## 对象数组比较
```
let arr1 = 
[
  { id:'1',name:'zhangsan' }, 
  { id:'2', name:'lisi' }
]
let arr2 = 
[
  { id:'1',name:'zhangsan', age:'15'}, 
  { id:'2', name:'lisi', age:'16' },
  { id:'3', name:'ani', age:'17'}
]
//取不同的元素（ES6的方法）
let res = arr2.filter(item => !arr1.some(ele=> ele.id === item.id))
console.log('res', res)

//取相同的元素（ES6的方法）
let res2 = arr2.filter(item => arr1.some(ele=> ele.id === item.id))
console.log('res2', res2)
```
## 对象数组去重
```
/**
arr:对象;flag:根据哪个属性值，
*/
function uniqueFunc(arr, flag){
  const res = new Map();
  return arr.filter((item) => !res.has(item[flag]) && res.set(item[flag], 1));
}
```
## 数组扁平化
### 1. 递归
```
const flatten = (arr) => {
  let result = [];
  arr.forEach((item, i, arr) => {
    if (Array.isArray(item)) {
      result = result.concat(flatten(item));
    } else {
      result.push(arr[i])
    }
  })
  return result;
};
const arr = [1, [2, [3, 4]]];
console.log(flatten(arr));
```
### 2. toString()
```
const flatten = (arr) => arr.toString().split(',').map((item) => +item);
const arr = [1, [2, [3, 4]]];
console.log(flatten(arr));
```
### 3. [].concat.apply + some
```
const flatten = (arr) => {
  while (arr.some(item => Array.isArray(item))){
    arr = [].concat.apply([], arr);
  }
  return arr;
}
const arr = [1, [2, [3, 4]]];
console.log(flatten(arr));
```
### 4. reduce()
```
function flatten(arr){
  return arr.reduce(function(prev, cur){
    return prev.concat(Array.isArray(cur) ? flatten(cur) : cur)
  }, [])
}
const arr = [1, [2, [3, 4]]];
console.log(flatten(arr));
```
### 5. ES6结构运算符
```
function flatten(arr){
  while(arr.some(item => Array.isArray(item))){
    arr = [].concat(...arr);
  }
  return arr;
}
const arr = [1, [2, [3, 4]]];
console.log(flatten(arr));
```
## 深拷贝
### 1.通过JSON.stringify和JSON.parse
无法实现对对象中方法的深拷贝。
```
var brr=JSON.parse(JSON.stringify(arr))
```
### 2. 递归
```
  //使用递归实现深拷贝
     function deepClone(obj) {
         //判断拷贝的obj是对象还是数组
         var objClone = Array.isArray(obj) ? [] : {};
         if (obj && typeof obj === "object") {
         //obj不能为空，并且是对象或者是数组 因为null也是object
             for (key in obj) {
                 if (obj.hasOwnProperty(key)) {
                     if (obj[key] && typeof obj[key] === "object") { 
                     //obj里面属性值不为空并且还是对象，进行深度拷贝
                         objClone[key] = deepClone(obj[key]); //递归进行深度的拷贝
                     } else {
                         objClone[key] = obj[key]; //直接拷贝
                     }
                 }
             }
         }
         return objClone;
     }
```

## 节流与防抖
### 函数防抖
将多次操作合并为一次操作进行。原理是维护一个计时器，规定在delay时间后触发函数，但是在delay时间内再次触发的话，就会取消之前的计时器而重新设置。这样一来，只有最后一次操作能被触发。
```
function debounce(fn, delay) {
    let timer = null;
    return function (e) {
        // 取消之前的延时调用，每当用户输入的时候把前一个setTimeout清除
        clearTimeout(timer);
        timer = setTimeout(() => {
            fn(e);
        }, delay); // 直至delay时间后，监听的事件没有改变后，才会执行fn()
    }
}
// 测试代码
function sayHi(e){
    console.log('防抖：', e.target.innerWidth, e.target.innerHeight);
}
window.addEventListener('scroll', debounce(sayHi, 500));
```
### 函数节流
使得一定时间内只触发一次函数。原理是通过判断是否有延迟调用函数未执行。
```
function throttle(fn,delay) {
    let canRun = true; // 通过闭包保存一个标记
    return function () {
         // 在函数开头判断标记是否为true，不为true则return
        if (!canRun) return;
         // 立即设置为false
        canRun = false;
        // 将外部传入的函数的执行放在setTimeout中
        setTimeout(() => { 
        // 最后在setTimeout执行完毕后再把标记设置为true(关键)表示可以执行下一次循环了。
        // 当定时器没有执行的时候标记永远是false，在开头被return掉
            fn.apply(this, arguments);
            canRun = true;
        }, delay);
    };
}
 // 测试代码
function sayHi(e) {
    console.log('节流：', e.target.innerWidth, e.target.innerHeight);
}
window.addEventListener('resize', throttle(sayHi,500));
```

## 对象判空
-   通过JSON自带的stringify()方法来判断
-   for in 循环判断
-   使用ES6的Object.keys()方法
-   Object.getOwnPropertyNames()方法
-   将json对象转化为json字符串，再判断该字符串是否为"{}"


持续更新...




