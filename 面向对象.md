## 原型链
JavaScript 中的每个对象都有一个内部私有的链接指向另一个对象，这个对象就是原对象的原型。这个原型对象也有自己的原型，直到对象的原型为 null 为止（也就是没有原型）。这种一级一级的链结构就称为原型链。

## 构造函数
### 构造函数解决的问题是“模板化的创建对象”
````javascript
function Person (name,age){
    this.name = name;
    this.age  = age;
}
var a = new Person('pin', 20);
var b = new Person('tang',20);
````
### 如果不使用构造函数，这个创建就略显得复杂
````javascript
var a = {};
a.name = 'pin';
a.age  = 20;

var b = {};
b.name = 'tang';
b.age = 20;
````
### 用new运算符实例化的时候，相当于做了以下三个步骤。
````javascript
// 1. 创建一个空对象
var obj = {};
// 2. 让构造函数中的this指向这个空对象
Person.apply(obj,arguments);
// 3. 返回这个空对象
return obj;
````

### 继承
以上的分析并没有涉及到原型链，实际上new操作符还做了一件事：`让创建的这个空对象继承于构造函数的prototype`，上面代码中的第一步后面还要添加一步
````javascript
obj.__proto__ = Person.prototype;
````
正因为这个继承，new运算符实例化出来的对象才可以调用到Person的prototype上的属性和方法。

## 继承
那么，当我们在说继承的时候，我们在说什么呢。

### __proto__属性
每个对象都有一个内部私有属性__proto__，这个属性指向该对象的原型。将对象obj的内部属性__proto__指向一个对象o，对象obj就继承了o的方法和属性。
当我们调用obj.method1()时，js引擎会先在obj自己的属性里查找，如果没有找到method1方法，就会去他的__proto__属性所指向的对象里查找。

### 创建原型链的三种方式
#### 构造函数
new 操作是最常用的创建对象的方式之一，new 操作返回的对象的__proto__指向构造函数对象的prototype属性。
new操作的作用实际上就是：
1. 以创建的对象为context执行构造函数
2. 将创建的对象obj的__proto__属性指向构造函数的prototype
````javascript
var Person = function(){
    this.type = 'person';
}
Person.prototype.sayname = function(){
    console.log(this.name);
};
var pin = new Person();
// 创建的对象pin的原型为Person.prototype，
pin.sayname;
````

#### Object.create()
ES5中提供了一个方法创建一个指定原型的对象`Object.create()`。在上面构造函数的例子中，假设我们要让Person类的实例都还有一些动物都有的方法，我们会扩展Person.prototype
````javascript
var Animal = function(){}
Animal.prototype.move = function(){}
var Person = function(){}
// 这里得到的Person.prototype的__proto__属性指向Animal.prototype
Person.prototype = Object.create(Animal.prototype)
// 定义Person.prototype自己的属性和方法
Person.prototype.sayname =function(){}
var pin = new Person()
// 1. pin对象本身并没有move方法
// 2. 去pin对象的原型(__proto__属性所指向的对象)上寻找，也没有
// 3. 去pin.__proto__.__proto__对象上找，get
pin.move()
````

#### 普通语法
看代码
````javascript
// obj这个对象继承了Object.prototype上面的所有属性(same as 'new Object()')
// 所以可以这样使用 obj.hasOwnProperty('a').
// hasOwnProperty 是Object.prototype的自身属性。
// Object.prototype的原型为null。
// 原型链如下:
// obj ---> Object.prototype ---> null
var obj = {name:'pin'}

// 数组都继承于Array.prototype (indexOf, forEach等方法都是从它继承而来).
// 原型链如下:
// a ---> Array.prototype ---> Object.prototype ---> null
var a = ["yo", "whadup", "?"];

// 函数都继承于Function.prototype(call, bind等方法都是从它继承而来):
// f ---> Function.prototype ---> Object.prototype ---> null
function f(){
  return 2;
}
````
实际上，这些常规语法与构造函数是相同的，只是简便方法。

