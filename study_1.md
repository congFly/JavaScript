# JS按值传递和按引用传递

### 1.   按值传递

将参数值传递给过程的方式，使过程访问到变量的复本。结果，过程不可改变变量的真正值，传递的是变量的内容。

### 2.   按引用传递

将参数地址传递给过程的方式，使过程访问到实际的变量。结果，过程可以改变变量的真正值，传递的是变量在内存中地址的指针或引用。

先举个两个例子：

```
var a = 10;
var b = a;
a = 20;
console.log(b)   //30


var a = [1,2]

var b = a;

a[0] = 3;

console.log(b)  //[3,2]
```

**************

>有没有觉得很神奇，number,string都是基本数据类型，而基本数据类型存放在栈区，是直接按值存放的，可以直接访问；对象和数组是存放在堆中的，通过引用来赋值，实际是一个存放在栈内存的指针，这个指针指向堆内存中的地址。
现在我们来看一下ECMAScript中的数据类型。主要分为两大类：

#### 基本数据类型 （`undefined，boolean，string，number，null`）

`var a = 3;`

`var b = 3;`

`console.log(a === b)   //true`

所以记住基本数据类型的比较是值的大小。

此处如果用__==__呢？是会进行类型转换的，举个简单点的例子：

`var a = 10;`

`var b = '10';`

`console.log(a == b);   //true`

#### 引用数据类型（`object（数组和函数）`）

引用数据类型在内存中是这样存的，每个空间大小不一样，要根据情况进行特定分配，如下所示：

`var  personA = {name:'fcc'}`

`var personB = {name:'fcc1'}`

`var personC = {name:'fcc2'}`

![堆内存](http://upload-images.jianshu.io/upload_images/4506573-0d051e07fcf922c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

举个例子：

`var a =[1,2,3,4];`

`var b =[1,2,3,4];`

`console.log(a === b)  //false`

虽然变量a，b内容是一样的，但是在内存中却存在不同的位置，他们指的不是同一个对象。

### 传值与传址有和不同？

基本数据类型的__=__是在内存中新开辟一段栈内存，然后把值赋值到新的栈中。

`var a = 1;`

`var b = a;   //只传递值`

`a++;`

`console.log(a);   //2`

`console.log(b);   //1`

![基本数据类型的赋值过程](http://upload-images.jianshu.io/upload_images/4506573-33bd6620c78de7d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
由图可以看出，基本数据类型是相互不会影响的。

引用数据类型的赋值是传址，改变指针的方向。就是说引用类型的赋值是对象保存在栈中的地址，两个变量之间就指向同一个对象，所以操作的时候互相之间会有影响。

`var a = {}; // a保存了一个空对象的实例`

`var b = a;  // a和b都指向了这个空对象`

`a.name = 'fcc';`

`console.log(a.name);    // 'fcc'`

`console.log(b.name);   // 'fcc'`

`b.age = 23;`

`console.log(b.age);   // 23`

`console.log(a.age);   // 23`

`console.log(a == b);   // true`

![引用类型赋值地址](http://upload-images.jianshu.io/upload_images/4506573-ff4f38bc5226ce4c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
由图可以看出，引用数据类型之间是会相互影响的。