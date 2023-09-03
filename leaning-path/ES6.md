ES6

## 1、let和const

ES6 新增了`let`命令，用来声明变量。它的用法类似于`var`，但是所声明的变量，只在`let`命令所在的代码块内有效。

`for`循环还有一个特别之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域。

```javascript
for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}
// abc
// abc
// abc
```

`let`命令改变了语法行为，它所声明的变量一定要在声明后使用，否则报错。

```javascript
// var 的情况
console.log(foo); // 输出undefined
var foo = 2;

// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;
```

`let`不允许在相同作用域内，重复声明同一个变量。

----

`const`声明的变量不得改变值，这意味着，`const`一旦声明变量，就必须立即初始化，不能留到以后赋值。

> ```javascript
> 
> // const只能保证这个指针是固定的（即总是指向另一个固定的地址），至于它指向的数据结构是不是可变的，就完全不能控制了。
> 
> const a = [];
> a.push('Hello'); // 可执行
> a.length = 0;    // 可执行
> a = ['Dave'];    // 报错
> ```

## 2、变量的解构赋值

### 2.1、数组的解构赋值

ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，称为解构。

```javascript
let [foo, [[bar], baz]] = [1, [[2], 3]];
let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []
```

不完全解构：

```javascript
let [x, y] = [1, 2, 3];
x // 1
y // 2

let [a, [b], d] = [1, [2, 3], 4];
a // 1
b // 2
d // 4
```

### 2.2、对象解构

```javascript
let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz // "aaa"

let obj = { first: 'hello', last: 'world' };
let { first: f, last: l } = obj;
f // 'hello'
l // 'world'
```

嵌套赋值：

```javascript
let obj = {};
let arr = [];

({ foo: obj.prop, bar: arr[0] } = { foo: 123, bar: true });

obj // {prop:123}
arr // [true]
```

### 2.3、字符串解构

```javascript
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"
```



#### 用途

```javascript
// 返回一个数组

function example() {
  return [1, 2, 3];
}
let [a, b, c] = example();

// 返回一个对象

function example() {
  return {
    foo: 1,
    bar: 2
  };
}
let { foo, bar } = example();

// 函数参数定义
// 参数是一组有次序的值
function f([x, y, z]) { ... }
f([1, 2, 3]);

// 参数是一组无次序的值
function f({x, y, z}) { ... }
f({z: 3, y: 2, x: 1});
                       
// 提取json数据
let jsonData = {
  id: 42,
  status: "OK",
  data: [867, 5309]
};

let { id, status, data: number } = jsonData;

console.log(id, status, number);
// 42, "OK", [867, 5309]
                       
// 遍历map                   
const map = new Map();
map.set('first', 'hello');
map.set('second', 'world');

for (let [key, value] of map) {
  console.log(key + " is " + value);
}
// first is hello
// second is world
```

## 3、字符串扩展

### 3.1、模板字符串

```javascript
模板字符串（template string）是增强版的字符串，用反引号（`）标识。它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。
模板字符串中嵌入变量，需要将变量名写在${}之中。


// 普通字符串
`In JavaScript '\n' is a line-feed.`

// 多行字符串
`In JavaScript this is
 not legal.`

console.log(`string text line 1
string text line 2`);

// 字符串中嵌入变量
let name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`
```

如果需要引用模板字符串本身，在需要时执行，可以写成函数。

```javascript
let func = (name) => `Hello ${name}!`;
func('Jack') // "Hello Jack!"
```

### 3.2、实例方法

- **includes()**：返回布尔值，表示是否找到了参数字符串。
- **startsWith()**：返回布尔值，表示参数字符串是否在原字符串的头部。
- **endsWith()**：返回布尔值，表示参数字符串是否在原字符串的尾部。
- `repeat`方法返回一个新字符串，表示将原字符串重复`n`次。
- ES2017 引入了字符串补全长度的功能。如果某个字符串不够指定长度，会在头部或尾部补全。`padStart()`用于头部补全，`padEnd()`用于尾部补全。
- [ES2019](https://github.com/tc39/proposal-string-left-right-trim) 对字符串实例新增了`trimStart()`和`trimEnd()`这两个方法。它们的行为与`trim()`一致，`trimStart()`消除字符串头部的空格，`trimEnd()`消除尾部的空格。它们返回的都是新字符串，不会修改原始字符串。

```javascript
let s = 'Hello world!';

s.startsWith('Hello') // true
s.endsWith('!') // true
s.includes('o') // true


'x'.repeat(3) // "xxx"
'hello'.repeat(2) // "hellohello"
'na'.repeat(0) // ""

'x'.padStart(5, 'ab') // 'ababx'
'x'.padStart(4, 'ab') // 'abax'

'x'.padEnd(5, 'ab') // 'xabab'
'x'.padEnd(4, 'ab') // 'xaba'

```

## 4、正则

```javascript

- `^` ： 表示开头

- `\$` ： 表示结尾

- `\` ： 转译符号，把有意义的 **符号** 转换成没有意义的 **字符**，把没有意义的 **字符** 转换成有意义的 **符号**


- `\s` ： 匹配空白字符（空格/制表符/...）

- `\S` ： 匹配非空白字符

- `\d` ： 匹配数字

- `\D` ： 匹配非数字

- `\w` ： 匹配数字字母下划线

- `\W` ： 匹配非数字字母下划线


- `*` ： 前一个内容重复至少 0 次，也就是可以出现 **0 ～ 正无穷** 次

- `+` ： 前一个内容重复至少 1 次，也就是可以出现 **1 ～ 正无穷** 次

- `?` ： 前一个内容重复 0 或者 1 次，也就是可以出现 **0 ～ 1** 次

- `{n}` ： 前一个内容重复 n 次，也就是必须出现 **n** 次

- `{n,}` ： 前一个内容至少出现 n 次，也就是出现 **n ～ 正无穷** 次

- `{n,m}` ： 前一个内容至少出现 n 次至多出现 m 次，也就是出现 **n ～ m** 次



// 下面是一个简单的邮箱验证

// 非_$开头，任意字符出现至少6次，一个@符号，(163|126|qq|sina)中的任意一个，一个点，(com|cn|net)中的任意一个

var reg = /^[^_$].{6,}@(163|126|qq|sina)\.(com|cn|net)$/
```

### 4.1、标识符

```javascript
- `i` ： 表示忽略大小写
- 这个 i 是写在正则的最后面的
- `/\w/i`
- 就是在正则匹配的时候不去区分大小写
- `g` ： 表示全局匹配
- 这个 g 是写在正则的最后面的
```





