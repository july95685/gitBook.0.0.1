## Array的构造函数
### Array.of
Array.of基本上可以用来替代Array()或newArray()，并且不存在由于参数不同而导致的重载，而且他们的行为非常统一。
#### 解决问题
如果给Array传入一个数值型的值，那么数组的length会为该值，但如果是一个非数字型的类型，则为数组的第一项，我们不可能总能注意到数据的类型，所以存在一定的风险
Array.of()总能创建一个包含所有参数的数组
```javascript
let item = Array.of(2) // [2]
let item2 = Array(2) // [empty * 2]

// 生成一个1~100的数组
let item3 = [...Array(101).keys()].splice(1,101);
// splice方法通过删除或替换现有元素或者原地添加新的元素来修改数组,并以数组形式返回被修改的内容。此方法会改变原数组。
var months = ['Jan', 'March', 'April', 'June'];
months.splice(0, 2, 'a', 'b') // ["Jan", "March"]
months // ["a", "b", "April", "June"]
months.splice(0, 2, 'c') // ["a", "b"]
months // ["c", "April", "June"]
// slice 不改变
// shift() 方法从数组中删除第一个元素，并返回该元素的值。此方法更改数组的长度。
```

### Array.from
将类数组和可迭代对象转换为数组，
典型的“类似数组的对象”是函数的**arguments对象，以及大多数 DOM 元素集，还有字符串**。
```javascript
function getArgument() {
    console.log(arguments)
    let arr1 = Array.prototype.slice.call(arguments)
    console.log(arr1)
    
    let arr2 = [...arguments]
    console.log(Array.isArray(arr2))

    let arr3 = Array.from(arguments)
    console.log(Array.isArray(arr3))
}
getArgument(1,2,3,4,5)
```

### 新方法
find/findIndex()
find() 方法返回数组中满足提供的测试函数的第一个元素的值。否则返回 undefined。findIndex为key
fill
```javascript
let number = [1,2,3,4]
number.fill(0, 1, 3) // [1, 0, 0, 4]
```
copyWithin
```javascript
let number = [1,2,3,4]
number.copyWithin(2, 1, 3) // 1, 2, 2, 3]
// 粘贴位置 复制开始位置 复制结束位置

```

### 数组遍历
常见遍历方法：forEach、map、filter、find、every、some、reduce
这些方法都不会改变原始数组

#### 1.forEach: 变量数组

#### 2.map：将数组映射成另一个数组
forEach和map的区别在于,forEach没有返回值,map需要返回值,如果不给return,默认返回undefined

#### 3.filter：从数组中找出所有符合指定条件的元素

#### 4.find：返回通过测试（函数内判断）的数组的第一个元素的值

#### 5.every：数组中是否每个元素都满足指定的条件;some:数组中是否有元素满足指定的条件(返回的是boolean)

#### 6.reduce：将数组合成一个值
reduce() 方法接收一个方法作为累加器，数组中的每个值(从左至右) 开始合并，最终为一个值。


## Set 和 Map
Set常用于检测给定值是否存在，而Map集合常用于获取以存的值

### es5对于Map的实现及存在的问题
```javascript
var map = Object.create(null)
map.foo = 'bar'
var value = foo.bar

let key1 = {}
    key2 = {}
    key3 = { a : 2}
map[key1] = 'b'
map[key2] === map[key3] // 2
```
问题：
1.所有对象的属性名都必须是字符串，键名存在数据类型转换
2.判断一个值有无其实判断的是他的Boolean()

**在Javascript中有一个in运算符，是不需要读取对象的值就可以判断属性在对象中
是否存在值，但他会检查对象的原型，所以需要Object.create(null)创建的对象才准确

### set
Set集合内部不存在数据类型转换，判断是否独立元素使用Object.is()(但set认为+0和-0
是相等的)，如果向set中添加多个对象，则他们保持独立
```javascript
let set = new Set()
    key1 = {},key2 = {}
set.add(key1)
set.add(key2)
set.size // 2

set.has(key1)
set.delete(key1)
// clear清除所有元素 forEach循环
```
### WeakSet
WeakSet只存储对象的弱引用，并且不可以存储原始值（即非对象，数组也是对象值）,如果集合中的弱引用是对象唯一的
引用，则会被回收机制释放相应的内存

#### 与Set的区别
1. WeakSet向add中传入非对象参数会报错，has，delete会返回false(has，delete与set表现一致)
2. WeakSet不可迭代，无迭代方法values(),kets(),entries(),for..in,没有forEach方法
3. 不支持size

### Map
对象保存键值对。任何值(对象或者原始值) 都可以作为一个键或一个值。

```javascript
var map = new Map([['name', 'Ni', 'wo'], ['age', 15]])
// Map(2) {"name" => "Ni", "age" => 15}
map.set('ser': 'son')
map.set('ser': 'son1')
map.has('name') // true
map.get('name') // Ni
```

### WeakMap
WeakMap中的键名必须是一个对象，键名对应的值可以是任意类型,具有set,get,delete,has方法，
不支持clear,size,forEach

如果键名对应的值是一个对象，则保存的是对象的强引用，不会触发垃圾回收机制

#### WeakMap最大的作用是保存Web页面中的DOM

```javascript
let map = new WeakMap()
    element = document.querySelect('.class');
map.set(element,'origin')
element.parentNode.removeChild(element)
element = null // 此时WeakMap清空
```

#### 储存对象实例的私有数据
```javascript
/// es5
var Person = (function(){
    var privateData = {}
        priveteId = 0;
    function Person(name) {
        Object.definePrototype(this, '_id', {value: priveteId++ } )

        privateData[this._id] = {
            name: name
        }
    }

    Person.prototype.getName = function() {
        return privateData[this._id].name
    }

    return Person
}())

/// weakMap
let Person = (function() {
    let pervateData = new WeakSet()

    function Person(name) {
        privateData.set(this, {name: name})
    }

    Person.prototype.getName = function() {
        return privateData.get(this).name
    }

    return Person
}())
```