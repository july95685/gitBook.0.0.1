# Object



### let const var

#### 变量提升，临时死区

var声明的变量会被当成当前作用域的顶部变量

解析器进入一个块级作用域，发现let关键字，变量只是先完成声明，并没有到初始化那一步，此刻如果在此作用域前访问，则报错，这就是临时死区的由来，当然，如果let为赋值操作，则初始化和赋值同时完成

**let的优点**

1.防止修改window上重要变量 2.仅存在于当前作用域，出了作用域即销毁，不宜造成内存泄漏

**变量的生命周期**

声明，初始化，赋值

var 在var的作用域,声明,初始化会在任何语句执行前完成，但此时未赋值，所以拿到的值为undefined

function 的声明，初始化，赋值在一开始就全部完成了，所以函数的变量提升优先级更高

```javascript
console.log(a)
console.log(b)
var a = function(){
    console.log('a')
}
function b(){
    console.log('b')
}
console.log(a)
console.log(b)
```

**const,let声明的变量在不在window上？**

在ES5中，顶层对象的属性和全局变量是等价的，var 命令和 function 命令声明的全局变量，自然也是顶层对象。

但ES6规定，var 命令和 function 命令声明的全局变量，依旧是顶层对象的属性，但 let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性。

### Object

#### Object.defineProperty

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

#### Object.prototype相关方法

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

**Symbol的属性检索**

Object.keys\(\)方法和Object.getOwnPropertypeName\(\)方法可以检索对象中所有的属性名，前一个方法返回所以可枚举的属性，后一个方法不考虑属性可枚举性一律返回，然而为了保持es5的原有功能，都支持Symbol属性，所以es6提供了getOwnPropertySymbols,hasOwnProperty可以检测Symbol的属性

#### Symbol相关

**创建共享Symbol**

Symbol.for\(\)方法可以在全局注册一个Symbol，后面传入的键已经存在时，则返回已有的Symbol

```javascript
var uid = Symbol('uid')
var uid2 = Symbol.for('uid')
uid === uid2 // false
var uid3 = Symbol.for('uid')
uid2 === uid3 // true
```

Symbol除了逻辑运算符外，无论使用哪一种数学操作符，都会异常

**检测Symbol类型使用typeof**

### ES6 增强对象原型

#### es5下原型保持不变是一个最重要的设定

`Objcect.setPrototypeOf()`来返回任意指定对象的原型

```javascript
let person = {
    getGreeting(){
        return 'hello'
    }
}
let friend = Object.create(person)
// Object.getPrototypeOf(friend) === friend.__proto__ === person
```

#### es6新增setPrototypeOf\(\)方法改变对象的原型

```javascript
let person = {
    getGreeting(){
        return 'hello'
    }
}
let dog = {
    getGreeting(){
        return 'woof'
    }
}
let friend = Object.create(person)
Object.setPrototypeOf(friend, dog)
console.log(friend.getGreeting) // woof
```

**函数表达式和函数声明的区别**

1. 以函数声明的方法定义的函数,函数可以在函数声明之前调用,而函数表达式的函数只能在声明之后调用.
2. 以函数声明的方法定义的函数并不是真正的声明,它们仅仅可以出现在全局中,或者嵌套在其他的函数中,但是它们不能出现在循环,条件或者try/catch/finally中,而函数表达式可以在任何地方声明.
3. 同时存在函数声明跟函数表达式，则函数表达式优先级要高

   ```javascript
   f1() // f1
   f2() // Error
   function f1(){ // 函数声明
    console.log('f1')
   }
   var f2 = function(){ // 函数表达式
    console.log('f2')
   }
   var f1 = function(){ // 函数表达式
    console.log('f3')
   }
   f1() // f3
   ```

#### 解构的赋值

在任何可以使用值的地方你都可以使用解构表达式

```javascript
node = {
    key: 'k',
    value: 2
}
function getNode(value) {
    return null
}
getNode({value, key} = node)
console.log(value) // 2
```

解构赋值表达式（=右边的）如果为null或undefined会导致异常

### 类

#### 通过类语法声明的特点

1. 函数声明可以被提升，类声明与let类似，不能被提升
2. 类声明下的代码在严格模式下
3. 在类中，所有方法不可枚举
4. 用除关键字new 外方式调用构造函数会导致程序异常
5. 在类中修改类名会异常

   \`\`\`javascript

   // 用es5实现一个es6的等价类

   let person = \(function\(\){ 

    const PersonType = function\(name\) {

   ```text
    if(type new.target === 'undefined') {
        throw new Error('必须通过new关键字调用')
    }
    this.name = name
   ```

    }

    Object.defineProperty\(PersonType.prototype, 'sayName', {

   ```text
    value: function() {
        if(type new.target !== 'undefined') {
            throw new Error('不可通过new关键字调用')
        }
    },
    enumerable: false,
    weitable: true,
    configurable: true
   ```

    }\)

    return PersonType

   }\(\)\)

let methodName = 'getName' class PersonClass { constructor\(name\) { this.name = name }

```text
static create(name) { // 静态方法
    return new PrersonClass('lu')
}

*[Symbol.iterator]() {
    yield this.name
}

[getName]() {
    return this.name
}
```

}

```text
### 类的继承与派生
只要表达式能被解析成函数并且具有[[construct]]属性和原型，那就能被extends继承，生成器函数没有没有[[construct]]属性 
```javascript
// es5的继承实现
function Rect(length, width) {
    this.length = length
    this.width = width
}

Rect.prototype.getArea() {
    return super.getArea()
}

function Square(length) {
    Rect.call(this, length, length)
}

Square.prototype = Object.create(Rect.prototype, {
    constructor: {
        value: Square,
        enumerable: true,
        writable: true,
        configurable: true
    }
})

class Square extends Rect{
    constructor(length) {
        super(length, length)
    }

    // 覆盖遮挡后调用Rect.prototype.getArea
    getArea() {
        return super.getArea()
    }
}
```

es5中实现类的继承需要用一个创建自父类.prototype的新对象重写Square.prototype并调用Rectangle.call\(\)方法,如果派生类中不定义constructor，则会有默认的构造函数

Es6通过super实现

#### super关键字

**使用**

1.当做函数使用，派生类初始化

只可在派生类（用extends声明的类）的构造函数中使用super\(\)，因为子类没有自己的 this 对象，而是继承父类的 this 对象，然后对其进行加工,而 super 就代表了父类的构造函数，super\(\)负责初始化this，如果在调用super\(\)之前访问this会报错

2.当做对象使用,屏蔽遮挡

super引用相当于指向对象原型的指针，实际上也就是Object.getPrototypeOf\(this\),想要通过super引用调用对象原型上的方法，必须使用简写方法，super在多重继承下非常有用，这里的super不能用于匿名函数定义的方法

```javascript
let person = {
    getGreeting(){
        return 'hello'
    },
}
let dog = {
    getGreeting(){
        return super.getGreeting()
    }  // 能被super使用
    sayHi: function(){
        return super.getGreeting()
    }  // 不能被super使用

}
Object.setPrototypeOf(dog, person)
dog.getGreeting() // hello
```

#### new.target的作用

new.target等于类的构造函数，可以用它来创建一个抽象基类（即不能被实例化的基类）

```javascript
class Shape{
    constructor() {
        if(new.target === Shape) {
            throw new Error('这个类不能被实例化')
        }
    }
}
class Rectangle extends Shape{
    constructor(length, width) {
        super();
        this.length = length;
        this.width = width;
    }
}

var x = new Shape() // Error
var y = new Rectangle(3,4)
```

