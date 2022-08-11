## Object常用方法
### Object.keys
    返回包含对象中所有属性的数组。
```
var cat= { 
        name:'mini', 
        age:2, 
        color:'yellow', 
        desc:'cute' 
    }
    console.log(Object.keys(cat)); // ["name", "age", "color", "desc"]
```
### Object.values
```
var obj = { foo: "bar", baz: 42 };  
Object.values(obj) // ["bar", 42]

Object.values('foo')  
// ['f', 'o', 'o'] 

```
### Object.create()
### Object.hasOwnProperty()
### Object.getOwnPropertyNames()
### Object.assign()

## 对象遍历

## 对象数组比较

## 对象数组去重

## 数组扁平化

## 深拷贝

## 节流防抖

## 对象判空