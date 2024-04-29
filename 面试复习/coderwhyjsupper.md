# This问题
1. 函数调用时，`javascript`默认给`this`绑定一个值
2. `this`的绑定与 定义的位置（编写位置）**无关**
3. `this`的绑定与 **调用方式以及调用位置** **有关**
4. `this`是在**运行时**被绑定的

## 绑定规则
### 默认绑定
```javascript
function fn() {
	console.log(this) 
	// node: Object [global]
	// browser: Object [window]
	// 严格模式下： 
	// node: {}
	// browser: Object [window]
}

fn()


const fn = () => {
  console.log(this)
  // node: {}
  // browser: Object [window]
  // 严格模式下：
  // node: {}
  // browser: Object [window]

}
fn()



var obj = {
	name: 'John',
	hello: function () {
		console.log(this)
	},
}

obj.hello() 
// node: Object [obj]
// browser: Object [obj]
// 严格模式下： 
// node: Object [obj]
// browser: Object [obj]

var hello = obj.hello
hello()  // 独立调用，在严格模式下指向undefined
// node: Object [global] 
// browser: Object [window]
// 严格模式下
// node: undefined
// browser: undefined

// 下述情况同属上列独立调用
const test = (fn) => {
	fn()
}
function fn() {
	console.log(this)
}

test(fn)


```

### 隐式绑定
前提条件：
- 必须在调用的对象内部有一个对函数的引用
- 如果没有这样的引用，在进行调用时，会找不到该函数而报错
- 正式通过这个引用，间接的将`this`绑定到了这个对象之上
```javascript
function foo() {
	console.log(this)
}

var obj = {
	fn: foo,
}

foo() // Object [global]
obj.fn() // Object [obj]
	
```
### 显示绑定
如果我们不希望在对象内部包含这个函数的引用，同时又希望在这个对象上强制调用某函数
可以使用`call`和`apply`方法
- 第一个参数是相同的，要求传入一个对象
	- 这个对象是给`this`准备的
	- 在调用这个函数时，会将`this`绑定到这个传入的对象上
- 后面的参数，`apply`为数组，`call`为参数列表
```javascript
func.apply(thisArg, [argsArray])
func.call(thisArg, arg1, arg2, arg3, ...)
```

通过`call`和`apply`绑定`this`对象
- 显示绑定过后，`this`就会明确的指向绑定的对象
```javascript
function foo() {
	console.log(this)
}
var efo = foo.call(window) // 这就是怪异函数对象
foo.call({name: 'apries'})
foo.call(123) // [Number: 123]
```

- 使用`bind`方法，`bind()`创建一个新的**绑定函数**（`bound function, BF`)
- 绑定函数是一个**怪异函数对象**(`exotic function object`) $ESMAScript\space 2015$术语
### `new`绑定
`JavaScript`中的函数可以当做一个类的构造函数来使用，也就是`new`关键字
使用`new`关键字来调用函数时，会执行如下的操作：
1. 创建一个新对象
2. 对这个新对象执行`prototype`连接原型链
3. 这个新对象被绑定到函数调用的`this`上
4. 如果函数没有返回其他对象，则返回这个隐式的`this`
```javascript
function Person(name, age) {
	this.name = name
	this.age = age
	console.log(this)
}

var p1 = new Person('John', 25) // Person { name: 'John', age: 25}
```

## 意料之外
### `null`和`undefined`
如果在显示绑定中，传入一个`null`或者`undefined`，那么这个显示绑定会被忽略，使用默认绑定（非严格模式）
```javascript
function foo() {
	console.log(this)
}

foo.apply(null) // S -> null    Object [global]
foo.apply(undefined) // S -> undefined  Object [global]
foo.apply('123') // S -> 123   [String: '123']

```

### 间接引用
```javascript
var obj1 = {
	name: 'obj1',
	foo: function () {
		console.log(this)
	},
}

var obj2 = {
	name: 'obj2',
}

obj1.foo() // Object [obj1]
;(obj2.foo = obj1.foo)() // Object [global]  默认绑定

obj2.foo() // Object [obj2]
```

## 箭头函数
- 箭头函数不会绑定`this`，`arguments`属性
- 剪头函数不能作为构造函数来使用