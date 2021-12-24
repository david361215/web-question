# 前端———问题合集

## 目录

1. 什么是闭包？闭包的用途是什么？闭包的缺点是什么？
2. call、apply、bind 的用法分别是什么？
3. 请说出至少 10 个 HTTP 状态码，并描述各状态码的意义。
4. 如何实现数组去重？
5. DOM 事件相关：什么是事件委托、怎么阻止默认动作、怎么阻止事件冒泡？
6. 你如何理解 JS 的继承？
7. 数组排序
8. 你对 Promise 的了解？
9. 说说跨域。
10. 说说你对前端的理解

## 正文

### 1.什么是闭包？闭包的用途是什么？闭包的缺点是什么？

&nbsp;&nbsp;&nbsp;&nbsp;闭包就是一个函数以及它能访问的作用域链。

&nbsp;&nbsp;&nbsp;&nbsp;闭包的用途就是只暴露想要暴露的函数，而隐藏所有不打算让外部能够访问到的数据。比如链式操作，我们不打算暴露获得的元素，只想暴露出一个对象，对象上有一些方法，外部只能通过这些方法来做操作。
```
window.jQuery = function(selector){
    const elements = document.querySelectorAll(selector);
    // api 可以操作elements
    const api = {
        // 闭包：函数访问外部的变量
        addClass(className){
            for(let i = 0;i< elements.length; i++){
                elements[i].classList.add(className);
            }
            return api;
        }
    }
    return api;
}![image](https://user-images.githubusercontent.com/20218586/146121927-f19cee86-904c-485b-bca7-32defdfd066e.png)

```
缺点：如果去恰当的滥用闭包，那么会导致程序运行变慢，而且浪费内存。

### 2.call、apply、bind 的用法分别是什么？

call 可以指定 this，第一个参数就作为函数内的 this，其余参数作为函数参数

apply 也类似 call，可以指定 this，第一个参数就作为函数内的 this，第二个参数是一个数组，数组中的每一项都作为函数参数
```
function Product(name, price) {
  this.name = name;
  this.price = price;
}

function Food(name, price) {
  Product.call(this, name, price);
  this.category = 'food';
}

console.log(new Food('cheese', 5).name);
// expected output: "cheese"
```
bind 基于一个函数创建一个新的函数，当调用时，bind 的参数作为原来函数内部的 this。bind 的其余参数都作为新函数的参数。
```
const module = {
  x: 42,
  getX: function() {
    return this.x;
  }
};

const unboundGetX = module.getX;
console.log(unboundGetX()); // The function gets invoked at the global scope
// expected output: undefined

const boundGetX = unboundGetX.bind(module);
console.log(boundGetX());
// expected output: 42
```

### 3.请说出至少 10 个 HTTP 状态码，并描述各状态码的意义。

&nbsp;&nbsp;&nbsp;&nbsp;状态码是一个三位整数，用于描述请求的结果。

&nbsp;&nbsp;&nbsp;&nbsp;状态码是可扩展的。客户端没必要理解所有状态码，但是至少应该知道状态码属于哪一类，这由第一个数字表示。第一个数字有五个值可选。

* 1xx 表示信息型状态码。表示请求已经收到持续处理中。
* 2xx 表示成功。成功收到并理解请求。
* 3xx 表示重定向。需要采取进一步的动作，来完成请求。
* 4xx 表示客户端错误。请求包含错误的语法或者不能被满足。
* 5xx 表示服务器错误。服务器不能满足一个看起来有效的请求。
     
HTTP 100 Continue 信息型状态响应码表示目前为止一切正常, 客户端应该继续请求, 如果已完成请求则忽略.

HTTP 101 Switching Protocol（协议切换）状态码表示服务器应客户端升级协议的请求（Upgrade (en-US)请求头）正在切换协议。

HTTP 200 OK 表明请求已经成功. 默认情况下状态码为200的响应可以被缓存。

HTTP 201 Created 是一个代表成功的应答状态码，表示请求已经被成功处理，并且创建了新的资源。新的资源在应答返回之前已经被创建。同时新增的资源会在应答消息体中返回，其地址或者是原始请求的路径，或者是 Location 首部的值。

HTTP 202 Accepted 表示服务器端已经收到请求消息，但是尚未进行处理。但是对于请求的处理确实无保证的，即稍后无法通过 HTTP 协议给客户端发送一个异步请求来告知其请求的处理结果。这个状态码被设计用来将请求交由另外一个进程或者服务器来进行处理，或者是对请求进行批处理的情形。

HTTP 203 Non-Authoritative Information 表示请求已经成功被响应，但是获得的负载与源头服务器的状态码为 200 (OK)的响应相比，经过了拥有转换功能的 proxy （代理服务器）的修改。

HTTP 204 No Content 成功状态响应码，表示该请求已经成功了，但是客户端客户不需要离开当前页面。默认情况下 204 响应是可缓存的。一个 ETag 标头包含在此类响应中。 

HTTP 205 Reset Content 用来通知客户端重置文档视图，比如清空表单内容、重置 canvas 状态或者刷新用户界面。

HTTP 206 Partial Content 成功状态响应代码表示请求已成功，并且主体包含所请求的数据区间，该数据区间是在请求的 Range 首部指定的。

HTTP 300 Multiple Choices 是一个用来表示重定向的响应状态码，表示该请求拥有多种可能的响应。用户代理或者用户自身应该从中选择一个。由于没有如何进行选择的标准方法，这个状态码极少使用。

HTTP 302 Found 重定向状态码，表明请求的资源被暂时的移动到了由该 HTTP 响应的响应头Location 指定的 URL 上。浏览器会重定向到这个URL，但是搜索引擎不会对该资源的链接进行更新 (In SEO-speak, it is said that the link-juice is not sent to the new URL)。

HTTP 400 Bad Request 响应状态码表示由于语法无效，服务器无法理解该请求。 客户端不应该在未经修改的情况下重复此请求。

HTTP 401 Unauthorized 代表客户端错误，指的是由于缺乏目标资源要求的身份验证凭证，发送的请求未得到满足。

HTTP 500 Internal Server Error 是表示服务器端错误的响应状态码，意味着所请求的服务器遇到意外的情况并阻止其执行请求。

### 4.著名面试题：如何实现数组去重？

* 假设有数组 array = [1,5,2,3,4,2,3,1,3,4]
* 你要写一个函数 unique，使得
* unique(array) 的值为 [1,5,2,3,4]
* 也就是把重复的值都去掉，只保留不重复的值。

* 要求写出两个答案：

* 一个答案不使用 Set 实现（6分）
* 另一个答案使用 Set （4分）
* （附加分）使用了 Map / WeakMap 以支持对象去重的，额外加 5 分。
* 说出每个方案缺点的，再额外每个方案加 2 分。

不使用 Set 实现
```
        const array1 = [1, 5, 2, 3, 4, 2, 3, 1, 3, 4];
        const array2 = {};
        function unique(array) {
          let uniqueArray = [];
          let length = array.length;
          if (array instanceof Array) {
            if (length >= 2) {
              uniqueArray.push(array[0]);
              for (let i = 1; i < length; i++) {
                let sign = true;
                for (let j = 0; j < length; j++) {
                  if (!uniqueArray[j]) {
                    break;
                  } else if (uniqueArray[j] === array[i]) {
                    sign = false;
                    break;
                  }
                }
                if (sign) {
                  uniqueArray.push(array[i]);
                }
              }
              return uniqueArray;
            } else {
              return array;
            }
          } else {
            return "please supply an array";
          }
        }
        console.log(unique(array1)); //[1, 5, 2, 3, 4]
        console.log(unique(array2));
```
使用 Set 实现
```
      function unique(arr) {
        console.log(arr);
        return Array.from(new Set(arr));
      }
      let arr = [
        1,
        1,
        "true",
        "true",
        true,
        true,
        15,
        15,
        false,
        false,
        undefined,
        undefined,
        null,
        null,
        NaN,
        NaN,
        "NaN",
        0,
        0,
        "a",
        "a",
        {},
        {},
      ];
      console.log(unique(arr));
      //[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {}, {}]
```

### 5.DOM 事件相关

什么是事件委托？

&nbsp;&nbsp;&nbsp;&nbsp;要说清楚事件委托，得先说清楚事件模型。HTML 元素是层层包含的，当一个事件触发时，是先从 window 对象开始，检查他上面是否定义了事件处理程序，然后到 document 对象，到 html 元素，到 body 元素，最后到最内层的元素，比如一个 td 元素。之后再从内向往把这个过程重复一遍。前一个过程叫捕获，后一个过程叫冒泡。

&nbsp;&nbsp;&nbsp;&nbsp;事件委托，就是在外层元素添加事件处理程序，监听内部元素发生的事件。这样可以大大节省内存。

怎么阻止默认动作？

&nbsp;&nbsp;&nbsp;&nbsp;在事件对象上，使用 preventDefault 方法

怎么阻止事件冒泡？

&nbsp;&nbsp;&nbsp;&nbsp;在事件对象上，使用 stopPropagation 方法


### 6.你如何理解 JS 的继承？

JavaScript 的对象有继承机制。

每个对象都有一个内部属性，叫 [[Prototype]] 或者 __proto__，这个属性保存着另一个对象的地址，这个对象就叫做 prototype。

这个 prototype 对象自己也有叫做 [[Prototype]] 或者 __proto__ 内部属性，同样保存着另一个对象的地址，这样构成了一条 prototype chain。prototype chain 的终点是 null，也就是 Object.prototype 的 __proto__ 属性。

如下代码就展示了基于 prototype 的继承。
```
// 基于函数 F，创建一个对象 o，o 自身有 a b 两个属性。
let F = function () {
   this.a = 1;
   this.b = 2;
}
let o = new F(); // {a: 1, b: 2}

// 在函数 F 的 prototype 上添加属性。
F.prototype.b = 3;
F.prototype.c = 4;

// 不要这样设置 prototype：F.prototype = {b:3,c:4}; 这会破坏原型链 prototype chain
// o.[[Prototype]] 有属性 b 和 c.
// o.[[Prototype]].[[Prototype]] 是 Object.prototype.
// 最终, o.[[Prototype]].[[Prototype]].[[Prototype]] is null.
// 这是原型链的终点，也就是 null，根据定义，他是没有 [[Prototype]] 属性的
// 因此，整个原型链看起来就是这样：
// {a: 1, b: 2} ---> {b: 3, c: 4} ---> Object.prototype ---> null

console.log(o.a); // 1
// o 有一个 a 这样的自有属性码？答案是有，而且它的值是 1.

console.log(o.b); // 2
// o 有一个 a 这样的自有属性码？答案是有，而且它的值是 2.
// prototype 对象也有一个 'b' 属性, 但是没有被访问到
// 这叫 Property Shadowing

console.log(o.c); // 4
// o 有一个 c 这样的自有属性码？答案是没有，于是查看它的 prototype.
// o.[[Prototype]] 有一个 c 这样的自有属性码？答案是有，它的值是 4.

console.log(o.d); // undefined
// o 有一个 d 这样的自有属性码？答案是没有，于是查看它的 prototype.
// o.[[Prototype]] 有一个 d 这样的自有属性码？答案是没有，于是查看它的 prototype.
// o.[[Prototype]].[[Prototype]] 就是 Object.prototype，默认它没有 d 属性，查看它的 prototype
// o.[[Prototype]].[[Prototype]].[[Prototype]] 是 null, 于是停止搜索,
// 找不到属性d，于是返回 undefined
```

ECMAScript 2015 引进了一系列新的关键字来表示类。这些关键字包括：class, constructor, static, extends, 以及 super.

虽然 ES6 引进了 class，但是 JavaScript 依然是基于原型完成继承的，class 只是语法糖。 

```
class Polygon {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}

// Square 继承了 Polygon
class Square extends Polygon {
  constructor(sideLength) {
    super(sideLength, sideLength);
  }
  get area() {
    return this.height * this.width;
  }
  set sideLength(newLength) {
    this.height = newLength;
    this.width = newLength;
  }
}

var square = new Square(2);
```

### 7.数组排序

给出正整数数组 array = [2,1,5,3,8,4,9,5]
请写出一个函数 sort，使得 sort(array) 得到从小到大排好序的数组 [1,2,3,4,5,5,8,9]
新的数组可以是在 array 自身上改的，也可以是完全新开辟的内存。

不得使用 JS 内置的 sort API

提示：排序算法，任选一个
```
function defaultCompare(a, b) {
    if (a === b) {
        return 0;
    }
    return a < b ? -1 : 1;
}
function swap(array, a, b) {
    [array[a], array[b]] = [array[b], array[a]];
}
function bubbleSort(array, compareFn = defaultCompare) {
    const { length } = array;
    for (let i = 0; i < length; i++) {
        for (let j = 0; j < length - 1; j++) {
        if (compareFn(array[j], array[j + 1]) === 1) {
            swap(array, j, j + 1);
        }
        }
    }
    return array;
}
let array = [2,1,5,3,8,4,9,5]
console.log(bubbleSort(array))
```

### 8.你对 Promise 的了解？

1.Promise 的用途

promise 主要有两大用途。

&nbsp;&nbsp;&nbsp;&nbsp;首先是抽象的表示一个异步操作，promise 的状态代表 promise 是否完成，“待定”表示尚未开始或者正在执行中，“成功”表示已经成功完成，“拒绝”表示则没有成功完成。很多时候，这个状态机就是 promise 可以提供的最有用的信息知道一段异步代码已经完成，对于其他代码而言已经足够。比如，鸡舍 promise 要向服务器发送一个 HTTP 请求，请求返回 200~299 范围内的状态码就足以让 promise 的状态变为“成功”，类似的，如果请求返回的状态码不在 200~299 这个范围内，那么就会把状态码切换为“拒绝”。

&nbsp;&nbsp;&nbsp;&nbsp;另一个用途，promise 封装的异步操作会实际生成某个值，而程序期待 promise 状态改变时可以访问这个值，相应的，如果 promise 被拒绝，程序就会期待 promise 状态改变时可以拿到拒绝的理由。比如，假设promise 向服务器发送一个 HTTP 请求并预定会返回一个JSON，如果请求返回范围在 200~299 的状态码则足以让 promise 的状态变为成功，此时 promise 内部就可以收到一个 JSON 字符串。类似的，如果请求返回的状态码不在 200~299 的范围内，那么就会把 promise 的状态切换为拒绝，此时拒绝的理由可能是一个 error 对象，包含着 HTTP 状态码和相关错误信息。

2.如何创建一个 new Promise
```
return new Promise((resolve,reject)=>{})
```
3.如何使用 Promise.prototype.then

&nbsp;&nbsp;&nbsp;&nbsp;then 方法是为 promise 实例添加处理程序的主要方法。这个 then 方法接收最多两个参数：onResolved 处理程序和 onRejected 处理程序，这两个参数都是可选的，如果提供的话，则会在 promise 分别进入“成功”和“拒绝”状态时执行。

```
      function onResolved(id) {
        setTimeout(console.log, 0, id, "resolved");
      }
      function onRejected(id) {
        setTimeout(console.log, 0, id, "rejected");
      }

      let p1 = new Promise((resolve, reject) => {
        setTimeout(resolve, 3000);
      });
      let p2 = new Promise((resolve, reject) => {
        setTimeout(reject, 3000);
      });

      p1.then(
        () => onResolved("p1"),
        () => onRejected("p1")
      );
      p2.then(
        () => onResolved("p2"),
        () => onRejected("p2")
      );
```

4.如何使用 Promise.all

&nbsp;&nbsp;&nbsp;&nbsp;Promise.all() 静态方法创建的 promise  会在一组 promise 全部成功之后再成功。这个静态方法街火速一个可迭代对象，返回一个新 promise。
```
      let p = Promise.all([
        Promise.resolve(),
        new Promise((resolve, reject) => setTimeout(resolve, 1000)),
      ]);
      p.then(() => setTimeout(console.log, 0, "all() resolved!"));
```

5、如何使用 Promise.race

Promise.race() 静态方法返回一个包装好的 Promise 对象，是一组集合中最先成功或拒绝的 Promise 的镜像。这个方法接受一个可迭代对象，返回一个新的 Promise 对象。

```
      let p = Promise.race([
        Promise.resolve(3),
        new Promise((resolve, reject) => setTimeout(reject, 1000)),
      ]);
      setTimeout(console.log, 0, p); // Promise <resolved>:3
```

### 9.说说跨域。

1.什么是源，什么是同源？

源= 协议 + 域名 + 端口号

输入 window.origin 或 location.origin 可以得到当前网页的源

如果两个 url 的协议、域名、端口号完全一致，那么这两个 url 就是同源的

同源策略是浏览器的规定，浏览器故意要设计成这样。

这个规定的内容：如果 JS 运行在源 A 里，那么只能获取源 A 的数据，不能获取源 B 的数据，即不允许跨域访问。不同源的页面之间，不准互相访问数据。

2.什么是跨域：

&nbsp;&nbsp;&nbsp;&nbsp;浏览器默认不同源之间不能互相访问数据，但是 qq.com 和 frank.com 其实都是方方的网站，方方就是想要两个网站互相访问，浏览器能不能提供一个方法呢

3.跨域方法一：CORS
  用 CORS，即提前声明，告诉浏览器，共享给谁。如果想要哪个资源被某个网站访问，就在那个资源的响应头里写这个网站的网址。
  
  具体语法就是 Access-Control-Allow-Origin:https://developer.mozilla.org。 

  具体的设置要在服务器代码中设置 header。见下方代码：

  ```
  else if(path === '/friends.json'){
    response.statusCode = 200
    response.setHeader('Content-Type', 'text/json;charset=utf-8')
    response.setHeader('Access-Control-Allow-Origin','http://frank.com:9999')
    response.write(fs.readFileSync('./public/friends.json'))
    response.end()
  }
  ```

4.跨域方法二：JSONP

  因为 IE 6 7 8 9 都不支持 CORS，怎么在 IE 中跨域呢？即怎么让 frank.com 访问 qq.com 的数据

  现在，假设 qq.com 和 frank.com 都是我们的网站，在不能使用 CORS 的情况下，我们让 qq.com 中的 js 文件包含数据。

思路

1、虽然不能访问别的文件，但是可以引用别的文件。

2、引用一个 JS，让这个 JS 文件包含我们想要的数据。

### 10.说说我对前端的理解

&nbsp;&nbsp;&nbsp;&nbsp;从工作流程上看，前端是和设计、产品经理、后端等，合作完成 Web 产品的一个岗位。因此，在熟悉前端范畴内的知识后，应适当了解设计师和后端的知识，这样可以有助于更加高效的沟通和配合，使工作更加流畅。

&nbsp;&nbsp;&nbsp;&nbsp;从技术要求上看，前端需要熟练掌握 HTTP、Html、Css、Javascript、Vue、React，并对后端知识有一定了解，主要是 Node.js。除此之外还需要掌握相关工具，例如 Git、GitHub、Webpack。

&nbsp;&nbsp;&nbsp;&nbsp;从职业发展的角度，考虑到前端程序员普遍较为薄弱的计算机基础，前端应该在完成好本职工作的同时，积极学习计算机科班知识，从而能够站在一个较高的角度来理解和看待前端。
