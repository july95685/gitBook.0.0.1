# Object



#### Object

**Object.defineProperty**

语法 `Object.defineProperty(obj, prop, descripter)`

参数

`obj: 必需，目标对象` `prop: 需定义的对象` `descripter: 目标属性所拥有的特性`

```javascript
Object.defineProperty(obj, "newKey", {
    configurable:true | false,
    enumerable:true | false,
    value:任意类型的值,
    writable:true | false
});
//writable 属性的值是否可以被重写。设置为true可以被重写；设置为false，不能被重写。默认为false
//enumerable 此属性是否可以被枚举（使用for...in或Object.keys()）。设置为true可以被枚举；设置为false，不能被枚举。默认为false。
//configurable 是否可以删除目标属性或是否可以再次修改属性的特性（writable, configurable, enumerable）
```

**Object.getOwnPropertyNames**

Object.getOwnPropertyNames\(\) 返回一个数组，该数组对元素是 obj自身拥有的枚举或不可枚举属性名称字符串。获取到的属性不包括Symbol属性

```javascript
var arr = ["a", "b", "c"];
console.log(Object.getOwnPropertyNames(arr));
// ["0", "1", "2", "length"]
```

**Object.prototype相关方法**

**Object.prototype.hasOwnProperty**

hasOwnProperty\(\) 方法会返回一个布尔值，指示对象自身属性中是否具有指定的属性（也就是是否有指定的键,包括enumerable定义的不可枚举属性和Symbol）

```javascript
o.hasOwnProperty('prop');
```

**Object.getOwnPropertySymbols**

Object.getOwnPropertySymbols\(\) 方法返回一个给定对象自身的所有 Symbol 属性的数组。

```javascript
var obj = {};
var a = Symbol("a");
obj[a] = "localSymbol";
var objectSymbols = Object.getOwnPropertySymbols(obj);
console.log(objectSymbols)         // [Symbol(a)]
```

