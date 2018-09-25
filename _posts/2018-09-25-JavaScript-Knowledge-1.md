---
layout: post
img: https://images.pexels.com/photos/802959/pexels-photo-802959.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260
categories: JavaScript
title: JavaScript 知识点梳理 (一)
description: |
date: 2018-09-25 18:00
permalink: archivers/blog3
---

梳理 JavaScript 基础知识：变量定义及声明，变量的解析等，数据类型的种类，数值和数值操作，以及字符串操作相关的知识点。

# JavaScript 知识梳理
## 基础篇

### 变量
* 变量严格区分大小写；
* 变量的声明和赋值，是分开的两个步骤，先声明，再赋值；
* JavaScript 引擎的工作方式是，先解析代码，获取所有被声明的变量，然后再一行一行地运行。这造成的结果，就是所有的变量的声明语句，都会被提升到代码的头部，这就叫做变量提升 (Hoisting) ；
* 常见 JavaScript 保留标识符
	* 函数参数 arguments, void
	* 异常捕捉 try, catch, throw, finally
	* 变量声明 var, const, let, enum
	* 函数修饰符 private, public, protected, static, function, return
	* 条件控制 with, for, in, continue, do, while, if, else, switch, case, default, break
	* 运算符 yield, typeof, instanceof, delete, eval, new
	* 类操作 import, export, extends, implements
	* 类声明 package, interface, class, super, this
	* 数据类型 false, null, true
	* 函数调试 debugger
* `<!--` 和 `-->` 的 `HTML` 组合注释符，在 `JavaScript` 中也被视为合法的单行注释；
```javascript
x = 1; <!-- x = 2;
--> x = 3;
```
> 需要注意的是，`-->` 只有在行首，才会被当成单行注释，否则会当作正常的运算；
* JavaScript 使用大括号，将多个相关的语句组合在一起，称为“区块”(block) ;
	* 对于 var 命令来说，JavaScript 的区块不构成单独的作用域（scope）。
	* 在 JavaScript 语言中，单独使用区块并不常见，区块往往用来构成其他更复杂的语法结构，比如for、if、while、function等。
* 区别赋值表达式 `=`，严格相等运算符 `===` 和相等运算符 `==`，赋值表达式不具有比较作用；
	* 我们为避免出现赋值的问题，常常将比较的常量写在左边，将变量写在右边；
	```javascript
if (x = 2) { // 不报错
if (2 = x) { // 报错
```
* `break` 和 `continue` 都是针对当前最内层的循环，进行**跳出循环**和**立即终止本轮循环**；
* JavaScript 语言允许，语句的前面有标签（label），相当于定位符，用于跳转到程序的任意位置。
	* 标签通常与 `break` 语句和 `continue` 语句配合使用，跳出特定的循环。
	```javascript
foo: {
  console.log(1);
  break foo;
  console.log('本行不会输出');
}
console.log(2);
// 1
// 2
```

### 数据类型

* JavaScript 的数据类型，共有六种，ES6 又新增了第七种 Symbol 类型的值
	* 数值（number）：整数和小数
	* 字符串（string）：文本
	* 布尔值（boolean）：表示真伪的两个特殊值，即true（真）和false（假）
	* undefined：表示“未定义”或不存在，即由于目前没有定义，所以此处暂时没有任何值
	* null：表示空值，即此处的值为空。
	* 对象（object）：各种值组成的集合
	
* 如何判断一个变量是属于什么类型？共有三种方法：
	* `typeof` 运算符
	* `instanceof` 运算符
	* `Object.prototype.toString` 方法

> `null` 的类型是 `object`，这是由于历史原因造成的。1995年的 JavaScript 语言第一版，只设计了五种数据类型（对象、整数、浮点数、字符串和布尔值），没考虑null，只把它当作object的一种特殊值。后来null独立出来，作为一种单独的数据类型，为了兼容以前的代码，typeof null返回object就没法改变了 
```javascript
typeof null // "object"
```

* `null` 是一个表示“空”的对象，转为数值时为0；
* `undefined` 是一个表示"此处无定义"的原始值，转为数值时为NaN
	```javascript
Number(null) // 0
5 + null // 5
Number(undefined) // NaN
5 + undefined // NaN
```

* 如果 JavaScript 预期某个位置应该是布尔值，会将该位置上现有的值自动转为布尔值。转换规则是除了下面六个值被转为 `false`，其他值都视为 `true` ：
	* undefined
	* null
	* false
	* **0**
	* NaN
	* `""` 或 `''` (空字符串)

#### 数值 `Number`

* JavaScript 内部，所有数字都是以64位浮点数形式储存，即使整数也是如此，JavaScript 语言的底层根本没有整数，所有数字都是小数（64位浮点数）
* 精度最多只能到53个二进制位，这意味着，绝对值小于2的53次方的整数，即 `-2`<sup>53</sup> 到 `2`<sup>53</sup> 都可以精确表示
* 大于 2 的 53次方以后，多出来的有效数字（最后三位的111）都会无法保存，变成0
	```javascript
Math.pow(2, 53)
// 9007199254740992
// 多出的三个有效数字，将无法保存
9007199254740992111
// 9007199254740992000
```
*  JavaScript 能够表示的数值范围为21024到2-1023（开区间），超出这个范围的数无法表示
```javascript
Math.pow(2, 1024) // Infinity
Math.pow(2, -1075) // 0
```
* JavaScript 提供 Number 对象的 `MAX_VALUE` 和 `MIN_VALUE` 属性，返回可以表示的具体的最大值和最小值
```javascript
Number.MAX_VALUE // 1.7976931348623157e+308
Number.MIN_VALUE // 5e-324
```
* 数值也可以采用科学计数法表示，下面是几个科学计数法的例子
```javascript
123e3 // 123000
123e-3 // 0.123
-3.1E+12 // -3100000000000
.1e-23 // 1e-24
```
* 以下两种情况，JavaScript 会自动将数值转为科学计数法表示，其他情况都采用字面形式直接表示：
	* 小数点前的数字多于21位
	* 小数点后的零多于5个
* 使用字面量（literal）直接表示一个数值时，JavaScript 对整数提供四种进制的表示方法：十进制、十六进制、八进制、二进制：
	* 十进制：没有前导 0 的数值
	* 八进制：有前缀 `0o` 或 `0O` 的数值，或者有前导0、且只用到0-7的八个阿拉伯数字的数值
	* 十六进制：有前缀 `0x` 或 `0X` 的数值
	* 二进制：有前缀 `0b` 或 `0B` 的数值

* 以下两种情况，JavaScript 会自动将数值转为科学计数法表示，其他情况都采用字面形式直接表示：
	* 小数点前的数字多于21位
	* 小数点后的零多于5个
* 使用字面量（literal）直接表示一个数值时，JavaScript 对整数提供四种进制的表示方法：十进制、十六进制、八进制、二进制：
	* 十进制：没有前导 0 的数值
	* 八进制：有前缀 `0o` 或 `0O` 的数值，或者有前导0、且只用到0-7的八个阿拉伯数字的数值
	* 十六进制：有前缀 `0x` 或 `0X` 的数值
	* 二进制：有前缀 `0b` 或 `0B` 的数值

> 通常来说，有前导0的数值会被视为八进制，但是如果前导0后面有数字8和9，则该数值被视为十进制
> ```javascript
0888 // 888
0777 // 511
```
> 前导0表示八进制，处理时很容易造成混乱。ES5 的严格模式和 ES6，已经废除了这种表示法，但是浏览器为了兼容以前的代码，目前还继续支持这种表示法。

* JavaScript 内部实际上存在2个 0：一个是 `+0`，一个是 `-0`，它们大部分情况下是等价的，区别就是64位浮点数表示法的符号位不同。
	```javascript
+0 // 0
-0 // 0
(-0).toString() // '0'
(+0).toString() // '0'
```
> 唯一有区别的场合是，`+0` 或 `-0` 当作分母：
> ````javascript
(1 / +0) === (1 / -0) // false
````
> 上面的代码之所以出现这样结果，是因为除以正零得到+Infinity，除以负零得到-Infinity，这两者是不相等的

* NaN 是 JavaScript 的特殊值，表示“非数字”（`Not a Number`）
	* NaN 不是独立的数据类型，而是一个特殊数值，它的数据类型依然属于Number
	* **NaN 不等于任何值，包括它本身**
	```javascript
NaN === NaN // false
```
	* NaN 与任何数（包括它自己）的运算，得到的都是 NaN
	* NaN 在布尔运算时被当作 false

* Infinity 用来表示两种场景：一种是一个正的数值太大或一个负的数值太小；另一种是非 0 数值除以0，得到 Infinity
	* Infinity 有正负之分，`Infinity` 表示正的无穷，`-Infinity` 表示负的无穷。
	* `Infinity` 大于一切数值（除了NaN），`-Infinity` 小于一切数值（除了NaN）
	* Infinity 与 null 计算时，null 会转成 0，等同于与 0 的计算
	```javascript
null * Infinity // NaN
null / Infinity // 0
Infinity / null // Infinity
0 * Infinity // NaN
0 / Infinity // 0
Infinity / 0 // Infinity
```


* 以下两种情况，JavaScript 会自动将数值转为科学计数法表示，其他情况都采用字面形式直接表示：
	* 小数点前的数字多于21位
	* 小数点后的零多于5个
* 使用字面量（literal）直接表示一个数值时，JavaScript 对整数提供四种进制的表示方法：十进制、十六进制、八进制、二进制：
	* 十进制：没有前导 0 的数值
	* 八进制：有前缀 `0o` 或 `0O` 的数值，或者有前导0、且只用到0-7的八个阿拉伯数字的数值
	* 十六进制：有前缀 `0x` 或 `0X` 的数值
	* 二进制：有前缀 `0b` 或 `0B` 的数值

> 通常来说，有前导0的数值会被视为八进制，但是如果前导0后面有数字8和9，则该数值被视为十进制
> ```javascript
0888 // 888
0777 // 511
```
> 前导0表示八进制，处理时很容易造成混乱。ES5 的严格模式和 ES6，已经废除了这种表示法，但是浏览器为了兼容以前的代码，目前还继续支持这种表示法。

* JavaScript 内部实际上存在2个 0：一个是 `+0`，一个是 `-0`，它们大部分情况下是等价的，区别就是64位浮点数表示法的符号位不同。
	```javascript
+0 // 0
-0 // 0
(-0).toString() // '0'
(+0).toString() // '0'
```
> 唯一有区别的场合是，`+0` 或 `-0` 当作分母：
> ````javascript
(1 / +0) === (1 / -0) // false
````
> 上面的代码之所以出现这样结果，是因为除以正零得到+Infinity，除以负零得到-Infinity，这两者是不相等的

* NaN 是 JavaScript 的特殊值，表示“非数字”（`Not a Number`）
	* NaN 不是独立的数据类型，而是一个特殊数值，它的数据类型依然属于Number
	* **NaN 不等于任何值，包括它本身**
	```javascript
NaN === NaN // false
```
	* NaN 与任何数（包括它自己）的运算，得到的都是 NaN
	* NaN 在布尔运算时被当作 false

* Infinity 用来表示两种场景：一种是一个正的数值太大或一个负的数值太小；另一种是非 0 数值除以0，得到 Infinity
	* Infinity 有正负之分，`Infinity` 表示正的无穷，`-Infinity` 表示负的无穷。
	* `Infinity` 大于一切数值（除了NaN），`-Infinity` 小于一切数值（除了NaN）
	* Infinity 与 null 计算时，null 会转成 0，等同于与 0 的计算
	```javascript
null * Infinity // NaN
null / Infinity // 0
Infinity / null // Infinity
0 * Infinity // NaN
0 / Infinity // 0
Infinity / 0 // Infinity
```

* parseInt 方法用于将字符串转为整数，如果parseInt的参数不是字符串，则会先转为字符串再转换
	* 如果字符串的第一个字符不能转化为数字（后面跟着数字的正负号除外），返回 NaN
	* 如果字符串以 `0x` 或 `0X` 开头，parseInt 会将其按照十六进制数解析
	* parseInt 会将科学计数法的表示方法视为字符串
	* parseInt 方法还可以接受第二个参数（2到36之间），表示**被解析的值的进制**，返回**该值对应的十进制数**
	```javascript
parseInt('1000', 2) // 8
parseInt('1000', 6) // 216
parseInt('1000', 8) // 512
parseInt('1000') // 1000
// 等同于
parseInt('1000', 10) // 1000
parseInt('10', 37) // NaN
parseInt('10', 1) // NaN
parseInt('10', 0) // 10
parseInt('10', null) // 10
parseInt('10', undefined) // 10
```
* **如果 parseInt 的第一个参数不是字符串，会被先转为字符串，一定要记住这个规则**
	```javascript
parseInt(0x11, 36) // 43
parseInt(0x11, 2) // 1
// 等同于
parseInt(String(0x11), 36)
parseInt(String(0x11), 2)
// 等同于
parseInt('17', 36)
parseInt('17', 2)
```
> 上面代码中，十六进制的 `0x11` 会被先转为十进制的 17，再转为字符串。然后，再用36进制 或 二进制 解读字符串17，最后返回结果 43 和1。

* parseFloat方法用于将一个字符串转为浮点数
	* 如果参数不是字符串，或者字符串的第一个字符不能转化为浮点数，则返回 NaN
	```javascript
parseFloat('3.14') // 3.14
parseFloat('314e-2') // 3.14
parseFloat('0.0314E+2') // 3.14
parseFloat('3.14more non-digit characters') // 3.14
parseFloat('\t\v\r12.34\n ') // 12.34
parseFloat([]) // NaN
parseFloat('FF2') // NaN
parseFloat('') // NaN
```
	* parseFloat会将空字符串转为 NaN。这些特点使得parseFloat的转换结果不同于 Number 函数
	```javascript
parseFloat(true)  // NaN
Number(true) // 1
parseFloat(null) // NaN
Number(null) // 0
parseFloat('') // NaN
Number('') // 0
parseFloat('123.45#') // 123.45
Number('123.45#') // NaN
```

* isNaN 为 true 的值，有可能不是 NaN，而是一个字符串，出于同样的原因，对于对象和数组，isNaN也返回 true
	```javascript
isNaN('Hello') // true
isNaN({}) // true
// 等同于
isNaN(Number({})) // true
isNaN(['xzy']) // true
// 等同于
isNaN(Number(['xzy'])) // true
```

* 但是，对于空数组和只有一个数值成员的数组，isNaN 返回 false。
```javascript
isNaN([]) // false
isNaN([123]) // false
isNaN(['123']) // false
```
因此，使用isNaN之前，最好判断一下数据类型
```javascript
function myIsNaN(value) {
  return typeof value === 'number' && isNaN(value);
}
```
判断NaN更可靠的方法是，利用NaN为唯一不等于自身的值的这个特点，进行判断
```javascript
function myIsNaN(value) {
  return value !== value;
}
```

* isFinite 方法返回一个布尔值，表示某个值是否为正常的数值
```javascript
isFinite(Infinity) // false
isFinite(-Infinity) // false
isFinite(NaN) // false
isFinite(undefined) // false
isFinite(null) // true
isFinite(-1) // true
```

#### 字符串 `String`

* 由于 HTML 语言的属性值使用双引号，所以很多项目约定 JavaScript 语言的字符串只使用单引号
* 字符串默认只能写在一行内，分成多行将会报错，如果长字符串必须分成多行，可以在每一行的尾部使用反斜杠
	```javascript
	var longString = 'Long \
	long \
	long \
	string';
	longString
	// "Long long long string"
	```
> 注意，反斜杠的后面必须是换行符，而不能有其他字符（比如空格），否则会报错。

*  连接运算符（+）可以连接多个单行字符串，将长字符串拆成多行书写，输出的时候也是单行

	```javascript
	var longString = 'Long '
	  + 'long '
	  + 'long '
	  + 'string';
	```

* 如果想输出多行字符串，有一种利用多行注释的变通方法
	```javascript
	(function () { /*
	line 1
	line 2
	line 3
	*/}).toString().split('\n').slice(1, -1).join('\n')
	// "line 1
	// line 2
	// line 3"
	```
* 反斜杠还有三种特殊用法：

	1 `\HHH`

		\HHH 反斜杠后面紧跟三个八进制数（000到377），代表一个字符。HHH对应该字符的 Unicode 码点，比如\251表示版权符号。显然，这种方法只能输出256种字符。

	2 `\xHH`

		\x 后面紧跟两个十六进制数（00到FF），代表一个字符。HH 对应该字符的 Unicode 码点，比如\xA9表示版权符号。这种方法也只能输出256种字符

	3 `\uXXXX`

		\u 后面紧跟四个十六进制数（0000到FFFF），代表一个字符。XXXX 对应该字符的 Unicode 码点，比如\u00A9表示版权符号

	```javascript
	'\251' // "©"
	'\xA9' // "©"
	'\u00A9' // "©"
	```

* 字符串可以被视为字符数组，因此可以使用数组的方括号运算符，用来返回某个位置的字符（位置编号从0开始），如果方括号中的数字超过字符串的长度，或者方括号中根本不是数字，则返回 undefined，只适用读取，不能更改里面的内容
```javascript
var s = 'hello';
delete s[0];
s // "hello"
s[1] = 'a';
s // "hello"
s[5] = '!';
s // "hello"
s.length = 7;
s.length // 5
```
* JavaScript 使用 Unicode 字符集。JavaScript 引擎内部，所有字符都用 Unicode 表示
```javascript
var f\u006F\u006F = '\u00A9';
foo // "©"
```
* JavaScript 不仅以 Unicode 储存字符，还允许直接在程序中使用 Unicode 码点表示字符，即将字符写成\uxxxx的形式，其中xxxx代表该字符的 Unicode 码点
```javascript
var s = '\u00A9';
s // "©"
var f\u006F\u006F = 'abc';
foo // "abc"
```
* 每个字符在 JavaScript 内部都是以16位（即2个字节）的 UTF-16 格式储存。也就是说，JavaScript 的单位字符长度固定为16位长度，即2个字节，码点在 `U+0000` 到 `U+FFFF` 之间的字符
> JavaScript 对 UTF-16 的支持是不完整的，由于历史原因，只支持两字节的字符，不支持四字节的字符。对于码点在 `U+10000` 到 `U+10FFFF` 之间的字符，JavaScript 总是认为它们是两个字符（length属性为2）。所以处理的时候，必须把这一点考虑在内，也就是说，JavaScript 返回的字符串长度可能是不正确的。

* 所谓 Base64 就是一种编码方法，可以将任意值转成 0～9、A～Z、a-z、+和/这64个字符组成的可打印字符。使用它的主要目的，不是为了加密，而是为了不出现特殊字符，简化程序的处理

* JavaScript 原生提供两个 Base64 相关的方法：
	* btoa()：任意值转为 Base64 编码 (base-64 encoded ASCII string)
	* atob()：Base64 编码转为原来的值
	```javascript
	var string = 'Hello World!';
	btoa(string) // "SGVsbG8gV29ybGQh"
	atob('SGVsbG8gV29ybGQh') // "Hello World!"
	```
	> 注意，这两个方法不适合非 ASCII 码的字符，会报错。
	> ```javascript
	btoa('你好') // 报错
	```
	* 要将非 ASCII 码字符转为 Base64 编码，必须中间插入一个转码环节，再使用这两个方法。
	```javascript
	function b64Encode(str) {
	  return btoa(encodeURIComponent(str));
	}
	function b64Decode(str) {
	  return decodeURIComponent(atob(str));
	}
	b64Encode('你好') // "JUU0JUJEJUEwJUU1JUE1JUJE"
	b64Decode('JUU0JUJEJUEwJUU1JUE1JUJE') // "你好"
	```
