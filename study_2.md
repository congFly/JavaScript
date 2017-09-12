# JS中的作用域
> 作用域是一个基础但又很重要的概念，对于初学者来说，深入了解作用域是前端进阶的基础。
### 1. 什么是作用域

> 简单的来说，作用域就是程序源代码中定义变量的区域，也是你代码当前的上下文环境，更好的解释就时内存中开辟出来的一段空间，JavaScript采用词法作用域，同时也被称作静态作用域。

__静态作用域、动态作用域：__

```
var a= 1;

function fun1() {

    console.log(a);
}

function fun2() {

    var a= 2;

    fun1();
}

fun1();   // 1
```
__由于JavaScript采用的是静态作用域，所以他的执行过程如下：__

> 执行 fun1函数，在从 fun1函数内部查找是否有局部变量 a。如果没有，查找上面一层的代码，也就是 a等于 1，所以最终结果为 1。

__那么JavaScript采用的是动态作用域呢，他的执行是怎么的？__

> 执行 fun1函数，从 fun1函数内部查找是否有局部变量 a。如果没有，就从调用fun1的函数中查找是否有a，即fun2 函数中找a 变量，这样执行的结果为2。

### 2. 作用域之间的差异

```
function a() {

    function b() {

       c = 1;
   }
}
```
提出问题，c的作用域应该是？b的？a的？哦，原来是全局的，因为前面没有加var。

__局部作用域：__
> 一般只在固定的代码片段内可访问到，如函数内部

直接上代码：
```
function fun1() {

      var a = 1;

      console.log(a);
}

fun1();   //   1



function fun2() {

      var b = 2;    //   b只能在函数内部使用
}

fun2();

console.log(b);   //   报错:b b is not defined
```

__全局作用域：__

```
var c = 3;

function fun3() {

     console.log(c);
}

fun3();   //   3


function fun4() {

      d = 4;    //
}

fun4();

      console.log(d);   //   4
```
> 从上面两个例子中可以看到：__函数内部可以访问外部变量，函数外部不能访问函数内部变量。__
__此处还应该注意：__
如果是在过程外部，用var和不用var定义的变量都是全局变量，但是在过程内部，用了var定义的变量是局部变量，没有用var定义的变量就是全局变量。
### 3. ES6中新增的let作用域

在ES6未出现之前没有块级作用域的概念，这就会导致在一些情况下，局部变量常常会污染全局变，举个例子：
```
var a = 1;

function fun1() {

      var a = 2;

      console.log(a);
}

fun1();   //   2    局部变量即fun1中的变量a，替换了全局变量中的a，污染了全局变量。
```

ES6中的块级作用域，在{}、for、if中都可以声明。

__ES6有几个特点：__
>  1、let是块级变量，不存在于window下，window.变量名是找不到的
2、 在没有声明的情况下使用会抛出异常
3、也不允许重复声明变量，即变量名唯一

```
let a = 1;

window.a;   //   undefined



function fun1(){

        console.log(b);

        let b =2;
    }

    fun1();  //   报错    fun1 is not defined



function fun2() {

     let c =2;

     var c = 3;

     console.log(c);
}

fun2();   //   报错    Identifier 'b' has already been declared
```

let在for循环中的使用：
(先看一下var)
```
for(var i=0; i<2; i++){

    console.log('outer i: ' + i);

    for(var i=0; i<2; i++){

	console.log('inner i: '+i);   //   外层循环被打断了，因为ｉ为全局变量所以 i 的值被内层循环修改了

    }
}
```
结果是：
>outer i: 0
 inner i: 0
 inner i: 1


把var换成let：
```
for(var i=0; i<2; i++){

    console.log('outer i: ' + i);

    for(let i=0; i<2; i++){

	console.log('inner i: '+i);   //   每一次循环都重新绑定一次作用域

    }
}
```

结果是：
> outer i: 0
outer i: 0
inner i: 0
inner i: 1
outer i: 1
inner i: 0
inner i: 1
