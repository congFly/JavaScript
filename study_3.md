# JS中变量提升和函数提升

##  1、变量提升
> __在ES6之前，声明变量都是用var，会出现变量提升的过程。在某些情况下，这些可能会产生一些小bug，首先我们要知道什么是变量提升，它是怎么样提升的？__

__变量的生命周期__

> 变量的生命周期有三部分，变量声明（Declaration Phase）、变量初始化（Initialization Phase）及变量赋值（Assignment Phase）

其中变量声明时会在作用域中注册变量，初始化时是为变量分配内存并且创建作用域，而此时变量会被初始化为 undefined，最后的赋值则会将指定的值分配给该变量。

来看一个例子：
```
(function () {

    console.log(foo);   //   undefined

    var foo = "fcc";

})();
```
__为什么会打印出undefined？而不是报错或者fcc呢?__

> 此处应该扯到浏览器是如何解析 JavaScript 代码的。
有三个参与者，分别是编译器、作用域、浏览器引擎

1、编译器是对源代码进行解析，以便浏览器能够识别

2、在解析完成之后，浏览器引擎会执行解析后的代码

3、作用域则参与其中，在编译过程中对于需要进行提升的变量，会将这些变量存储在作用域中，然后在引擎执行的过程中对其进行赋值操作

举个例子，来看看变量提升的过程：
```
console.log(a);     // undefined

var a = 1;
```
这段代码的执行过程应该是：
```
var a;

console.log(a);

a = 1;
```
> 在执行的过程中，遇到var，先将var 提到代码头部，进行变量提升。
所以在console之前只是对a进行了声明，以至于打印出undefined。

##  2、函数提升
> 函数提升不同于变量提升，他是将整个函数提升至代码头部，然后执行代码
```
console.log(fun1());      //    1

function fun1 () {

    return 1;
};
```

这段代码的执行过程应该是：
```
function fun1 () {

    return 1;
};

console.log(fun1 ()); 
```

再看一个例子：
```
console.log(fun2);    //    undefined

var fun2 = function () {

    return 1;

};
```
这里又涉及到函数声明和函数表达式。。。。。。它的执行过程应该是什么呢？
```
var fun2;

console.log(fun2);

fun2 = function () {

     return 1;

}
```
__函数声明和函数表达式：__
> 函数声明即是以 function 关键字开始，跟随者函数名与函数体。而函数表达式则是先声明函数名，然后赋值匿名函数给它。

从上面的例子中可以看到：
> 函数表达式遵循变量提升的规则，函数体不会被提升至代码头部：

__变量提升和函数提升的顺序__

> 此处给个例子就很明白了：
```
console.log(fun1);       

function fun1() {

    return 1;

};

var fun1= function () {

    return 2;

};
```
看看执行过程：
```
function fun1() {

    return 1;

};

var fun1;

console.log(fun1);  

fun1= function () {

    return 2;

};
```
是不是一下子就明白了。

好了，睡觉！