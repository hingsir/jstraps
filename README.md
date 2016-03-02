# jstraps
受[jstips](https://github.com/loverajoel/jstips)启发，咱也来个中文版的JavaScript那些坑，当然不仅限于JavaScript，也可以是HTML，CSSS等一切前端的坑，欢迎大家一起填坑。[格式请戳](/CONTRIBUTING.md)

## #03 如何用正确的姿势使用touchstart
>2016-03-02 by [@jiasm](https://github.com/jiasm)

为避免某些奇葩浏览器以及桌面浏览器不支持touch系列的事件，所以需要进行特性检测
```js
var eventName = "ontouchstart" in document ? "touchstart" : "click";
document.addEventListener(eventName, e => {
    // do xxx
})
```

## #02 检测jquery对象是否存在
>2016-01-21 by [@hingsir](https://github.com/hingsir)

检测`$(selector)`是否存在，相信不少人写过类似下面的代码：
```js
if($(selector)){
    // do something
}else{
    //do other things
}
```
然而这样写是有问题的，因为`$(selector`返回的是一个object，即便未匹配到任何元素，也会返回空数组`[]`。`ToBoolean([])`为true，那么else代码块永远也不会执行到，因此我们一般利用`$(selector).length>0`来检测。

## #01 this指向疑云
> 2016-01-12 by [@hingsir](https://github.com/hingsir)

关于this的指向问题，往往困扰着许多新人。this指的调用函数的那个对象，也即函数运行时所处的环境。函数有以下四种调用方式
* **普通函数调用** this指向全局环境global，浏览器环境为window。严格模式下指向undefined
* **对象方法调用** this指向该对象
* **构造函数调用** this指向由构造函数实例化后的对象
* **apply/call调用** 指定this指向，func.apply(obj)，指向obj

往往记住这几条规律，就不大容易弄错this指向。但下面例子却往往令不少新人弄错
```js
function A(){
    this.prop = 'a';
    function func(){
        console.log(this.prop)；
    }
    func();
}
new A()；
```
上述代码执行结果会是什么呢？有不少面试者认为会打印出'a'，而实际上是undefined，为什么呢？

其实非常简单，这就是普通函数调用方式，this指向的就是全局对象，而全局对象并没有prop属性，结果当然为undefined。既然如此简单，为何那么多人弄错，这是因为func函数定义在A内，this.prop混淆视听，影响了他们的判断。

所以只要记住下面一句话，就不会搞错：

**普通函数调用，不管该函数定义得多深，如果没显式指定其this指向，那么它就指向全局对象或者undefined**

如果想调整上述例子使其打印出'a'，就可以利用`func.apply(this)`来实现。另外值得一提的是，A()和new A()的区别往往也会令人疏忽，有兴趣的可以看看A()执行结果。
