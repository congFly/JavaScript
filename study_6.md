# javascript new 的过程
```
var obj = new Person();
```
上面的代码是这样解析的：
```
var obj = {};
obj.__orpto__ = Person.prototype;
Person.caLL(obj);
```
__在上面 new的过程中，其实做了四件事件：__

1. 创建一个空对象
2. 将空对象的__proto__成员指向了Person对象的prototype对象
3. 将Person对象的this指针指向obj，并传入所需参数
4. 执行构造函数，并返回创建的对象