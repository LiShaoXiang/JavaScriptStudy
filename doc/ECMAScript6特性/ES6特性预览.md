# ES6特性概览
> ES6是新版本的JavaScript语言标准，本文主要介绍一些ES6中的新特性

## 箭头操作符
类似于java中lambda表达式，ES6中箭头操作符`=>`便有异曲同工之妙。它简化了函数的书写。
操作符左边为输入参数，而右边则是进行的操作以及返回的值`Inputs=>Outouts`。
在JS中回调是经常发生的事，回调通常以匿名函数的形式出现，每次都要写一个function。当引入箭头操作符以后，大大简化了操作，下面举个栗子：
```javascript
var someArray = [1,2,3];
//传统写法
someArray.forEach(function(v,i,a){
    console.log(v);
});
//ES6写法
someArray.forEach(v => console.log(v));
```

## 类的支持
ES6加入了对类的支持，引入了`class`关键字。JS本身就是面向对象的，ES6提供的类实际上只是JS原型模式的包装。现在提供原生的class支持后，对象的创建，继承更加直观了，并且父类方法的调用，实例化，静态方法和构造函数等概念都更加形象化。下面举个栗子
```javascript
//类的定义
class Animal{
    constructor(name){//ES6构造器
        this.name = name;
    }
    showName(){       //实例方法
        console.log("The animal's name is "+this.name);
    }
}

//类的继承
class Dog extends Animal{
    constructor(name){
        super(name);//直接调用父类构造器进行初始化
    }
    bark(){
        console.log("A "+this.name+ " is barking..")
    }
}

//测试上面的类
var animal = new Animal("汤姆猫"),
whiteDog = new Dog("哈士奇");
animal.showName();// The animal's name is 汤姆猫
whiteDog.showName();//The animal's name is 哈士奇
whiteDog.bark();//A 哈士奇 is barking...
```

## 增强的对象字面量
对象字面量增强了，写法更加简洁灵活，同时在定义对象的时候能做更多的事情了，具体表现在：
- 可以在对象字面量里定义原型
- 定义方法可以不用function关键字
- 直接调用父类方法
这样一来，对象字面量与前面提到的类的概念更加吻合，在编写JavaScript对象的时候更加轻松方便了。下面举个栗子
```javascript
//通过对象字面量创建对象
var person = {
    breathe(){//定义方法属性，可以不用function关键字了
        console.log("We can breathe because we are still alive.");
    }
};
var programmer = {
    __proto__:person,//设置此对象的原型为person，相当于继承person
    company:"Alimama",
    work(){
        console.log("Coding,Coding,Coding.Programming to death!");
    }
};
person.breathe();//We can breathe because we are still alive.
programmer.breathe();//We can breathe because we are still alive.
programmer.work();//Coding,Coding,Coding.Programming to deatth!
```

## 字符串模板
ES6中允许使用反引号来创建字符串，这种方法创建的字符串里面可以包含由美元符号`$`加括号包裹的变量`${variable}`。
```javascript
var num = Math.random();
console.log(`you get a num ${num}`);
```

## 解构
自动解析数组或对象中的值。比如一个函数要返回多个值，常规做法是返回一个对象，将每个值作为这个对象的属性返回。但在ES6中，利用解构这一特性，可以直接返回一个数组，然后数组中的值会自动被解构到对应接收该值的变量中。下面举个栗子
```javascript
var [x,y] = getVal(),//函数返回值得解构
    [name,,age] = ["Dylan","male","secrect"];//数组解构
function getVal(){
    return [1,2];
}
console.log("x:"+x+",y:"+y);//x:1,y:2
console.log("name:"+name+",age:"+age);//name:Dylan,age:secrect
```

## 默认参数、不定参数、拓展参数
默认参数：可以在定义函数的时候指定默认参数值了，而不用像以前一样通过逻辑操作符`||`来达到目的
```javascript
function sayHi(name){
    //传统方式
    var name = name || "Dylan";
    console.log("Hi "+name);
}
//ES6默认参数
function sayHi2(name="Dylan"){
    console.log(`Hi ${name}`);
}
sayHi();
sayHi("Nick");
sayHi2();
sayHi2("Nick");
```

不定参数：不定参数是在函数中使用命名参数同时接收不定数量的未命名参数。以前的JavaScript我们可以通过arguments对象达到这一目的。不定参数的格式是三个句点后跟代表所有不定参数的变量名`...variable`。
```javascript
//定义一个函数将所有参数相加
function add(...x){
    return x.reduce((m,n) => m+n);//reduce方法是数组类型的缩小方法
}
//传递任意个参数
console.log(add(1,2,3));//6
console.log(add(1,2,3,4,5));//15
```

拓展参数：它允许传递数组或者类数组直接作为函数的参数而不用通过`apply()`。
```javascript
var people = ["Dylan","Nick","Greg"];
function sayHello(people1,people2,people3){
    console.log(`Hello ${people1},${people2},${people3}`);
}
//sayHello函数原本要接收三个参数，但我们可以将数组以一个拓展参数的形式传递
sayHello(...people);
//以前传递数组作为参数需要借助apply
sayHello.apply(null,people);
```

## let与const关键字
可以把let看成var，只是它定义的变量被限定在特定的范围内才能使用，const用来定义常量。
```javascript
for(let i=0;i<2;i++){
    console.log(i)//输出0，1
}
console.log(i);//输出undefined，严格模式下回报错
```

## for of 值遍历
我们都知道for in循环用于遍历数组，类数组或对象。ES6中`for-of`循环功能类似，不同的是每次循环它提供的不是序号而是值。
```javascript
var array = ["a","b","c"];
for(v of array){
    console.log(v);//输出a,b,c
}
for(i in array){
    console.log(i);//0,1,2
}
```

## iterator,generator
//TODO

## 模块
ES6标准中，JavaScript原生支持module了。
将不同功能的代码分别写在不同的文件中（模块），各模块只需导出公共接口部分，然后通过模块的导入可以在其他地方方便的使用。
```javascript
//====point.js=====//
module "point"{
    export class Point{
        constructor(x,y){
            public x=x;
            public y=y;
        }
    }
}
//=====myapp.js=====//
module point from "/point.js";//声明引用的模块
import Point from "point";//尽管声明了引用模块，还是可以通过指定需要的部分进行导入

var origin = new Point(0,0);
console.log(origin);
```

## Map,Set和WeakMap，WeakSet
这些是新加的集合类型，提供了更加方便的获取属性值得仿佛，不用像以前一样用`hasOwnProperty()`来检查某个属性是属于原型链还是当前对象，同时属性值得添加与获取时有专门的get、set方法。
```javascript
//Sets
var s = new Set();
s.add("hello").add("goodbye").add("hello");
console.log(s.size) //=== 2;
console.log(s.has("hello"))// === true;
//Maps
var m = new Map();
m.set("hello",42);
m.set(s,34);
console.log(m.get(s) == 34);//true
```
有时候我们会把对象作为一个对象的键用来存放属性值，一般集合类型会阻止垃圾回收对这些作为属性键存在的对象的回收，有造成内存泄漏的危险。而`WeakMap`,`WeakSet`则更安全些，这些作为属性键的对象如果没有别的变量在引用它们，则会被回收释放掉。
```javascript
//WeakMap
var wm = new WeakMap();
wm.set(s,{extra:42});
wm.size === undefined;

//WeakSet
var ws = new WeakSet();
ws.add({data:47});//因为添加到ws的这个临时对象没有其他变量引用它，所以ws不会保存它的值
```

## Proxy
Proxy可以监听对象身上发生了什么事情，并在这些事情发生后执行一些相应的操作。让我们对对象有了很强的追踪能力，同时在数据绑定方面也很有用处。
```javascript
//定义一个目标对象
var engineer = {
    name:"Dylan",
    salary:50
};
//定义处理程序
var interceptor = {
    set:function(receiver,property,value){
        console.log(property,"is changed to",value);
        receiver[property] = value;
    }
};
//创建代理以监听对象
engineer = new Proxy(engineer,interceptor);
engineer.salary = 60;//控制台输出salary is changed to 60
```
对于上面的例子，处理程序是在被监听的对象身上发生了相应的事件之后，处理程序里面的方法就会被调用，上面的例子中，我们设置了`set`处理函数，表明如果监听到对象的属性发生改变，也就是被set了，那么这个处理程序就会被调用，同时通过参数能够得知是哪个属性被更改，更改了什么值。

## Symbol
我们知道JavaScript对象实际上是键值对的集合，而键通常来说是字符串。现在除了字符串外，我们还可以用symbol值来作为键。
Symbol是一种基本类型，像Number，String，Boolean一样，它不是一个对象。Symbol通过调用symbol函数产生，它接收一个可选的名字参数，该函数返回的symbol是唯一的。之后就可以用这个返回值作为对象的键了。Symbol还可以用来创建私有属性，外部无法直接访问由symbol作为键的属性值。
```javascript
// 没有参数的情况
var s1 = Symbol();
var s2 = Symbol();
s1 === s2 // false
// 有参数的情况
var s1 = Symbol("foo");
var s2 = Symbol("foo");
s1 === s2 // false

//作为属性名的Symbol
var mySymbol = Symbol();
// 第一种写法
var a = {};
a[mySymbol] = 'Hello!';
// 第二种写法
var a = {
  [mySymbol]: 'Hello!'
};
// 第三种写法
var a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });
// 以上写法都得到同样结果
a[mySymbol] // "Hello!"

//注意，Symbol值作为对象属性名时，不能用点运算符。
var a = {};
var name = Symbol();
a.name = 'Lily';
a[name] = 'Lucy';
console.log(a.name,a[name]); //Lily,Lucy
```
Symbol值作为属性名时，该属性还是公开属性，不是私有属性。

这个有点类似于java中的protected属性（protected和private的区别：在类的外部都是不可以访问的，在类内的子类可以继承protected不可以继承private）

但是这里的Symbol在类外部也是可以访问的，只是不会出现在for...in、for...of循环中，也不会被Object.keys()、Object.getOwnPropertyNames()返回。但有一个Object.getOwnPropertySymbols方法，可以获取指定对象的所有Symbol属性名

## Math,Number,String,Object的新API
```javascript
Number.EPSILON
Number.isInteger(Infinity) // false
Number.isNaN("NaN") // false

Math.acosh(3) // 1.762747174039086
Math.hypot(3, 4) // 5
Math.imul(Math.pow(2, 32) - 1, Math.pow(2, 32) - 2) // 2

"abcde".contains("cd") // true
"abc".repeat(3) // "abcabcabc"

Array.from(document.querySelectorAll('*')) // Returns a real Array
Array.of(1, 2, 3) // Similar to new Array(...), but without special one-arg behavior
[0, 0, 0].fill(7, 1) // [0,7,7]
[1,2,3].findIndex(x => x == 2) // 1
["a", "b", "c"].entries() // iterator [0, "a"], [1,"b"], [2,"c"]
["a", "b", "c"].keys() // iterator 0, 1, 2
["a", "b", "c"].values() // iterator "a", "b", "c"

Object.assign(Point, { origin: new Point(0,0) })
```

## Promise
Promise是处理异步操作的一种模式，之前在很多三方库中有实现，比如jQuery的deferred 对象。当你发起一个异步请求，并绑定了.when(), .done()等事件处理程序时，其实就是在应用promise模式。
```javascript
//创建promise
var promise = new Promise(function(resolve, reject) {
    // 进行一些异步或耗时操作
    if ( /*如果成功 */ ) {
        resolve("Stuff worked!");
    } else {
        reject(Error("It broke"));
    }
});
//绑定处理程序
promise.then(function(result) {
    //promise成功的话会执行这里
    console.log(result); // "Stuff worked!"
}, function(err) {
    //promise失败会执行这里
    console.log(err); // Error: "It broke"
});
```


