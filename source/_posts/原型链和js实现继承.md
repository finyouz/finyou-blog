---
title: 原型链和js实现继承
date: 2025-02-24 13:58:02
tags: [继承,js]
---

## 原型链

> js中对象之间通过原型(prototype或者__proto__)连接起来的机制,用户实现继承和查找

### 核心概念

### 1. 原型对象（prototype）
- 每个js对象（除null外）都有一个原型对象，可以通过__proto__属性访问或Object.getPrototypeOf()方法获取
- 构造函数通过prototype属性来访问原型对象

### 2. 原型链
- 当访问一个对象的属性时，如果该对象没有这个属性，js会沿着原型链向上查找，直到找到该属性或者到达原型链的顶端（null）
- 原型链是由原型对象组成的层次结构，每个对象都有一个原型对象，通过__proto__属性连接起来
- 原型链的顶端是Object.prototype，它没有原型对象

![原型链](image.png)

## js实现继承

### 1. 原型链继承

> 原型链继承通过修改子类的原型为父类的实例，从而实现子类可以访问到父类构造函数以及原型上的属性或者方法。


```js
function Person() {
  this.name = 'finyou'
}

Person.prototype.getName = function() {
  return this.name;
}

function Child() {}

Child.prototype = new Person()

var child = new Child()
child.getName() // finyou

```

#### 问题
> 父类构造函数中的引用类型（比如对象/数组），会被所有子类实例共享。其中一个子类实例进行修改，会导致所有其他子类实例的这个值都会改变

```js
        // 父类构造函数
function Animal() {
    this.name = 'Animal';
    this.colors = ['red', 'blue', 'green'];
}

// 父类方法
Animal.prototype.sayName = function() {
    console.log(`My name is ${this.name}`);
};

// 子类构造函数
function Dog(name, breed) {
    this.breed = breed;
}

// 原型链继承：将 Dog 的原型指向 Animal 的实例
Dog.prototype = new Animal();

// 创建子类实例
const dog1 = new Dog('Buddy', 'Golden Retriever');
const dog2 = new Dog('Max', 'Labrador');


dog1.colors.push('black');

```

### 2.构造函数继承
> 构造函数继承其实就是通过修改父类构造函数this实现的继承。我们在子类构造函数中执行父类构造函数，同时修改父类构造函数的this为子类的this。

```js
function Person(){
    this.name = 'finyou';
}

function Student(){
    this.age = 18;
    Person.call(this);
}

var student = new Student();
console.log(student);

```

#### 问题

1.优点
> 解决了原型链继承引用类型共享的问题，同时可以向构造函数传参。

2.缺点
 - 无法继承父类原型上的属性和方法

```js
function Person(){
    this.name = 'finyou';
}

Person.prototype.getName = function(){
    return this.name;
}

function Student(){
    this.age = 18;
    Person.call(this);
}

var student = new Student();
console.log(student)
```
- 方法定义在构造函数中导致内存浪费

> 由于在构造函数继承中，每个子类实例都会在其自身的内存空间中拥有一份从父类继承来的属性和方法的副本。如果父类的构造函数中定义了较多的方法，那么每个子类实例都会重复存储这些方法，造成内存的浪费，尤其是在创建大量子类实例的情况下，这种内存开销会更加明显。

```js
function Parent() {
   
  this.parentProperty = 'I am from parent';
  this.someMethod = function() {
   
    console.log('This is a method in Parent');
  };
}

function Child() {
   
  Parent.call(this);
  this.childProperty = 'I am from child';
}

var child1 = new Child();
var child2 = new Child();

console.log(child1.someMethod === child2.someMethod); //false
```

> 在这个示例中，Parent 类的构造函数中定义了 someMethod 方法，通过构造函数继承创建的 child1 和 child2 两个实例都各自拥有一份 someMethod 方法的副本，导致内存中存在重复的函数定义，输出结果为 false。

### 3.组合继承
> 结合原型链继承和构造函数继承的优点，通过原型链继承父类的原型，通过构造函数继承父类的实例属性。

```js
function Person(){
    this.name = 'finyou';
}

Person.prototype.getName = function(){
    return this.name;
}

function Student(){
    Person.call(this);
    this.age = 18; 
}

Student.prototype = new Person();
// 需要重新设置子类的constructor，Child.prototype = new Parent()相当于子类的原型对象完全被覆盖了
Student.prototype.constructor = Student
let student = new Student();


console.log(student);

```

#### 缺点

> 父类构造函数被调用了两次。同时子类实例以及子类原型对象上都会存在name属性。虽然根据原型链机制，并不会访问到原型对象上的同名属性，但总归是不美。

![组合继承](image2.png)

### 4.寄生组合继承

> 寄生组合继承其实就是在组合继承的基础上，解决了父类构造函数调用两次的问题。

```js
function Animal(name) {
    this.name = name;
    this.colors = ['red', 'blue', 'green'];
}

Animal.prototype.sayName = function () {
    console.log(`My name is ${this.name}`);
};

function Dog(name, breed) {
    // 构造函数继承
    Animal.call(this, name);
    this.breed = breed;
}


//寄生组合继承的核心：使用Object.create()方法来创建一个原型对象
Dog.prototype = Object.create(Animal.prototype);

//修复构造函数指向
Dog.prototype.constructor = Dog;

let dog = new Dog('旺财', '哈士奇');

console.log(dog);

```

### 5.ES6继承
> ES6继承是通过extends关键字实现的，它是一种更简洁、更直观的继承方式。
```js
class Animal {
    constructor(name) {
        this.name = name;
    }
    sayName() {
        console.log(`My name is ${this.name}`);
    }
}
class Dog extends Animal {
    constructor(name, breed) {
        super(name);
        this.breed = breed;
    }
}

```