
>  对于HTML元素本身就带有的固有属性，在处理时，使用prop方法。

>  对于HTML元素我们自己自定义的DOM属性，在处理时，使用attr方法。

 
举个例子：

```
<a href="http://www.baidu.com" target="_self" class="btn">百度</a>
```

> <a>元素的DOM属性有“href、target和class"，这些属性就是<a>
> 素本身就带有的属性，也是W3C标准里就包含有这几个属性，或者说
> 在IDE里能够智能提示出的属性，这些就叫做固有属性。处理这些属性时，建议使用prop方法。


举个例子：
```
<a href="#" id="link1" action="delete">删除</a>
```
> 后面一个“action”属性是我们自己自定义上去的，<a>元素本身是没有这个属性的。这种就是自定义的DOM属性。处理这些属性时，建议使用attr方法。