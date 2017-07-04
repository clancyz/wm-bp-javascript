# Waimai Enterprise Platform JavaScript Style Guide() {

**基于[Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)的javascript代码规范**

本规范适用于ESNext. 修改的部分会以`[modified]`标注，无此说明的即与Airbnb的规约相同。

<a name="table-of-contents"></a>
## 目录

  1. [类型](#types)
  1. [引用](#references)
  1. [对象](#objects)
  1. [数组](#arrays)
  1. [解构](#destructuring)
  1. [字符串](#strings)
  1. [函数](#functions)
  1. [箭头函数](#arrow-functions)
  1. [构造函数](#constructors)
  1. [模块](#modules)
  1. [Iterators & Generators ](#iterators-and-generators)
  1. [属性](#properties)
  1. [变量](#variables)
  1. [提升](#hoisting)
  1. [比较运算符 & 等号](#comparison-operators--equality)
  1. [代码块](#blocks)
  1. [注释](#comments)
  1. [空白](#whitespace)
  1. [逗号](#commas)
  1. [分号](#semicolons)
  1. [类型转换](#type-casting--coercion)
  1. [命名规则](#naming-conventions)
  1. [存取器](#accessors)
  1. [事件](#events)
  1. [jQuery](#jquery)
  1. [ECMAScript 5 兼容性](#ecmascript-5-compatibility)
  1. [ECMAScript 6 编码规范](#ecmascript-6-styles)
  1. [测试](#testing)
  1. [性能](#performance)
  1. [资源](#resources)
  1. [使用人群](#in-the-wild)
  1. [翻译](#translation)
  1. [JavaScript 编码规范说明](#the-javascript-style-guide-guide)
  1. [一起来讨论 JavaScript](#chat-with-us-about-javascript)
  1. [Contributors](#contributors)
  1. [License](#license)

<a name="types"></a>
## 类型

  - [1.1](#1.1) <a name='1.1'></a> **基本类型**: 直接存取基本类型。

    + `字符串`
    + `数值`
    + `布尔类型`
    + `null`
    + `undefined`

    ```javascript
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```

  - [1.2](#1.2) <a name='1.2'></a> **复杂类型**: 通过引用的方式存取复杂类型。

    + `对象`
    + `数组`
    + `函数`

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="references"></a>
## 引用

- [2.1](#2.1) <a name='2.1'></a> 对所有的引用使用 `const` ；不要使用 `var`。eslint: [`prefer-const`](http://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](http://eslint.org/docs/rules/no-const-assign.html)


> 为什么？这能确保你无法对引用重新赋值，也不会导致出现 bug 或难以理解。

```javascript
// bad
var a = 1;
var b = 2;

// good
const a = 1;
const b = 2;
```

- [2.2](#2.2) <a name='2.2'></a> 如果你一定需要可变动的引用，使用 `let` 代替 `var`。 eslint: [`no-var`](http://eslint.org/docs/rules/no-var.html) jscs: [`disallowVar`](http://jscs.info/rule/disallowVar)

> 为什么？因为  `let` 是块级作用域，而 `var` 是函数作用域。

```javascript
// bad
var count = 1;
if (true) {
    count += 1;
}

// good, use the let.
let count = 1;
if (true) {
    count += 1;
}
```

- [2.3](#2.3) <a name='2.3'></a> 注意 `let` 和 `const` 都是块级作用域。

```javascript
// const 和 let 只存在于它们被定义的区块内。
{
    let a = 1;
    const b = 1;
}
console.log(a); // ReferenceError
console.log(b); // ReferenceError
```

**[⬆ 返回目录](#table-of-contents)**

<a name="objects"></a>
## 对象

- [3.1](#3.1) <a name='3.1'></a> 使用字面值创建对象。eslint: [`no-new-object`](http://eslint.org/docs/rules/no-new-object.html)

```javascript
// bad
const item = new Object();

// good
const item = {};
```


<a name="es6-computed-properties"></a>
- [3.2](#3.2) <a name='3.2'></a> 创建有动态属性名的对象时，使用可被计算的属性名称。

> 为什么？因为这样可以让你在一个地方定义所有的对象属性。

```javascript
function getKey(k) {
    return `a key named ${k}`;
}

// bad
const obj = {
    id: 5,
    name: 'San Francisco',
};
obj[getKey('enabled')] = true;

// good
const obj = {
    id: 5,
    name: 'San Francisco',
    [getKey('enabled')]: true,
};
```

<a name="es6-object-shorthand"></a>
- [3.3](#3.3) <a name='3.5'></a> 使用对象方法的简写。

```javascript
// bad
const atom = {
    value: 1,

    addValue: function (value) {
    return atom.value + value;
    },
};

// good
const atom = {
    value: 1,

    addValue(value) {
    return atom.value + value;
    },
};
```

<a name="es6-object-concise"></a>
- [3.4](#3.4) <a name='3.6'></a> 使用对象属性值的简写。eslint: [`object-shorthand`](http://eslint.org/docs/rules/object-shorthand.html) jscs: [`requireEnhancedObjectLiterals`](http://jscs.info/rule/requireEnhancedObjectLiterals)

> 为什么？因为这样更短更有描述性。

```javascript
const lukeSkywalker = 'Luke Skywalker';

// bad
const obj = {
    lukeSkywalker: lukeSkywalker,
};

// good
const obj = {
    lukeSkywalker,
};
```

- [3.5](#3.5) <a name='3.7'></a> 在对象属性声明前把简写的属性分组。

> 为什么？因为这样能清楚地看出哪些属性使用了简写。

```javascript
const anakinSkywalker = 'Anakin Skywalker';
const lukeSkywalker = 'Luke Skywalker';

// bad
const obj = {
    episodeOne: 1,
    twoJedisWalkIntoACantina: 2,
    lukeSkywalker,
    episodeThree: 3,
    mayTheFourth: 4,
    anakinSkywalker,
};

// good
const obj = {
    lukeSkywalker,
    anakinSkywalker,
    episodeOne: 1,
    twoJedisWalkIntoACantina: 2,
    episodeThree: 3,
    mayTheFourth: 4,
};
```


- [3.6](#3.6) <a name="3.6"></a> 仅在对象属性有特殊符号时使用引号包裹。 eslint: [`quote-props`](http://eslint.org/docs/rules/quote-props.html) jscs: [`disallowQuotedKeysInObjects`](http://jscs.info/rule/disallowQuotedKeysInObjects)

> 为什么？ 这样看上去更加易读，另外还能有相关的代码高亮，也容易被许多JS引擎优化。

```javascript
// bad
const bad = {
'foo': 3,
'bar': 4,
'data-blah': 5,
};

// good
const good = {
foo: 3,
bar: 4,
'data-blah': 5,
};
```

<a name="objects--prototype-builtins"></a>
- [3.7](#objects--prototype-builtins) 不要直接使用`Object.prototype`相关语法，如 `hasOwnProperty`, `propertyIsEnumerable`, 和 `isPrototypeOf`.

> 为什么？ 这些方法有可能被属性覆盖 - 如 `{ hasOwnProperty: false }` - 或者，对象可能是个空对象(`Object.create(null)`).

```javascript
// bad
console.log(object.hasOwnProperty(key));

// good
console.log(Object.prototype.hasOwnProperty.call(object, key));

// best
const has = Object.prototype.hasOwnProperty; // cache the lookup once, in module scope.
/* or */
import has from 'has';
// ...
console.log(has.call(object, key));
```

<a name="objects--rest-spread"></a>
- [3.8](#objects--rest-spread) 在浅拷贝时，推荐使用对象展开运算符而不是[`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)。在对象解构时，使用对象的剩余运算符来获得一个新对象。

```javascript
// very bad
const original = { a: 1, b: 2 };
const copy = Object.assign(original, { c: 3 }); // this mutates `original` ಠ_ಠ
delete copy.a; // so does this

// bad
const original = { a: 1, b: 2 };
const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

// good
const original = { a: 1, b: 2 };
const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

const { a, ...noA } = copy; // noA => { b: 2, c: 3 }

```

**[⬆ 返回目录](#table-of-contents)**

<a name="arrays"></a>
## 数组

- [4.1](#4.1) <a name='4.1'></a> 使用字面值创建数组。eslint: [`no-array-constructor`](http://eslint.org/docs/rules/no-array-constructor.html)

```javascript
// bad
const items = new Array();

// good
const items = [];
```

- [4.2](#4.2) <a name='4.2'></a> 向数组添加元素时使用 Arrary#push 替代直接赋值。

```javascript
const someStack = [];

// bad
someStack[someStack.length] = 'abracadabra';

// good
someStack.push('abracadabra');
```

<a name="es6-array-spreads"></a>
- [4.3](#4.3) <a name='4.3'></a> 使用拓展运算符 `...` 复制数组。

```javascript
// bad
const len = items.length;
const itemsCopy = [];
let i;

for (i = 0; i < len; i++) {
    itemsCopy[i] = items[i];
}

// good
const itemsCopy = [...items];

```

- [4.4](#4.4) <a name='4.4'></a> 使用 Array.from 把一个类数组对象转换成数组。

```javascript
const foo = document.querySelectorAll('.foo');
const nodes = Array.from(foo);
```

<a name="arrays--callback-return"></a><a name="4.5"></a>

- [4.5](#arrays--callback-return) 数组的相关方法使用return语句。如果函数体仅由一个带表达式且无副作用的语句组成，可以忽略return。参考[8.2](#arrows--implicit-return). eslint: [`array-callback-return`](http://eslint.org/docs/rules/array-callback-return)

```javascript
// good
[1, 2, 3].map((x) => {
    const y = x + 1;
    return x * y;
});

// good
[1, 2, 3].map(x => x + 1);

// bad
const flat = {};
[[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
    const flatten = memo.concat(item);
    flat[index] = flatten;
});

// good
const flat = {};
[[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
    const flatten = memo.concat(item);
    flat[index] = flatten;
    return flatten;
});

// bad
inbox.filter((msg) => {
    const { subject, author } = msg;
    if (subject === 'Mockingbird') {
    return author === 'Harper Lee';
    } else {
    return false;
    }
});

// good
inbox.filter((msg) => {
    const { subject, author } = msg;
    if (subject === 'Mockingbird') {
    return author === 'Harper Lee';
    }

    return false;
});
```

**[⬆ 返回目录](#table-of-contents)**

<a name="arrays--bracket-newline"></a>
- [4.6](#arrays--bracket-newline) 如果数组有多行，在数组括号起始和结束位置使用换行。

  ```javascript
  // bad
  const arr = [
    [0, 1], [2, 3], [4, 5],
  ];

  const objectInArray = [{
    id: 1,
  }, {
    id: 2,
  }];

  const numberInArray = [
    1, 2,
  ];

  // good
  const arr = [[0, 1], [2, 3], [4, 5]];

  const objectInArray = [
    {
      id: 1,
    },
    {
      id: 2,
    },
  ];

  const numberInArray = [
    1,
    2,
  ];
  ```

**[⬆ back to top](#table-of-contents)**


<a name="destructuring"></a>

## 解构

- [5.1](#5.1) <a name='5.1'></a> 使用解构存取和使用多属性对象。jscs: [`requireObjectDestructuring`](http://jscs.info/rule/requireObjectDestructuring)

> 为什么？因为解构能减少临时引用属性。

```javascript
// bad
function getFullName(user) {
    const firstName = user.firstName;
    const lastName = user.lastName;

    return `${firstName} ${lastName}`;
}

// good
function getFullName(obj) {
    const { firstName, lastName } = obj;
    return `${firstName} ${lastName}`;
}

// best
function getFullName({ firstName, lastName }) {
    return `${firstName} ${lastName}`;
}
```

- [5.2](#5.2) <a name='5.2'></a> 对数组使用解构赋值。

```javascript
const arr = [1, 2, 3, 4];

// bad
const first = arr[0];
const second = arr[1];

// good
const [first, second] = arr;
```

- [5.3](#5.3) <a name='5.3'></a> 需要回传多个值时，使用对象解构，而不是数组解构。 jscs: [`disallowArrayDestructuringReturn`](http://jscs.info/rule/disallowArrayDestructuringReturn)

> 为什么？增加属性或者改变排序不会改变调用时的位置。

```javascript
// bad
function processInput(input) {
    // then a miracle occurs
    return [left, right, top, bottom];
}

// 调用时需要考虑回调数据的顺序。
const [left, __, top] = processInput(input);

// good
function processInput(input) {
    // then a miracle occurs
    return { left, right, top, bottom };
}

// 调用时只选择需要的数据
const { left, right } = processInput(input);
```


**[⬆ 返回目录](#table-of-contents)**

<a name="strings"></a>
## Strings

- [6.1](#6.1) <a name='6.1'></a> 字符串使用单引号 `''` 。eslint: [`quotes`](http://eslint.org/docs/rules/quotes.html) jscs: [`validateQuoteMarks`](http://jscs.info/rule/validateQuoteMarks)

```javascript
// bad
const name = "Capt. Janeway";

// good
const name = 'Capt. Janeway';
```

- [6.2](#6.2) <a name='6.2'></a> 字符串超过 100 个字节应该使用字符串连接号换行。
- [6.3](#6.3) <a name='6.3'></a> 注：过度使用字串连接符号可能会对性能造成影响。[jsPerf](http://jsperf.com/ya-string-concat) 和 [讨论](https://github.com/airbnb/javascript/issues/40).

```javascript
// bad
const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

// bad
const errorMessage = 'This is a super long error that was thrown because \
of Batman. When you stop to think about how Batman had anything to do \
with this, you would get nowhere \
fast.';

// good
const errorMessage = 'This is a super long error that was thrown because ' +
    'of Batman. When you stop to think about how Batman had anything to do ' +
    'with this, you would get nowhere fast.';
```

<a name="es6-template-literals"></a>
- [6.4](#6.4) <a name='6.4'></a> 程序化生成字符串时，使用模板字符串代替字符串连接。eslint: [`prefer-template`](http://eslint.org/docs/rules/prefer-template.html) [`template-curly-spacing`](http://eslint.org/docs/rules/template-curly-spacing) jscs: [`requireTemplateStrings`](http://jscs.info/rule/requireTemplateStrings)


> 为什么？模板字符串更为简洁，更具可读性。

```javascript
// bad
function sayHi(name) {
    return 'How are you, ' + name + '?';
}

// bad
function sayHi(name) {
    return ['How are you, ', name, '?'].join();
}

// good
function sayHi(name) {
    return `How are you, ${name}?`;
}
```

<a name="strings--eval"></a><a name="6.5"></a>

- [6.4](#strings--eval) 不要对字符串使用 `eval()`，会导致一系列问题。 eslint: [`no-eval`](http://eslint.org/docs/rules/no-eval)

<a name="strings--escaping"></a>
- [6.5](#strings--escaping) 不要对字符串使用不必要的escape操作。 eslint: [`no-useless-escape`](http://eslint.org/docs/rules/no-useless-escape)

> 为什么？ 反斜杠会影响可读性，仅在必须的时候使用它。

```javascript
// bad
const foo = '\'this\' \i\s \"quoted\"';

// good
const foo = '\'this\' is "quoted"';
const foo = `my name is '${name}'`;
```

**[⬆ 返回目录](#table-of-contents)**

<a name="functions"></a>
## 函数

- [7.1](#7.1) <a name='7.1'></a> 使用具名函数代替函数声明。 eslint: [`func-style`](http://eslint.org/docs/rules/func-style) jscs: [`disallowFunctionDeclarations`](http://jscs.info/rule/disallowFunctionDeclarations)

> 为什么？因为函数声明会把函数提升(hoisted), 这样易使函数在定义前被引用。这会影响可读性和可维护性。如果一个函数的定义很长或很复杂，会干扰对文件剩余部分的理解，更好的方式是将它抽象在它自己的模块中。别忘了给函数表达式命名 - 匿名函数会使得在错误调用栈中定位问题变得困难。([讨论](https://github.com/airbnb/javascript/issues/794))

```javascript
// bad
const foo = function () {
};

// good
function foo() {
}
```

<a name="functions--iife"></a><a name="7.2"></a>

- [7.2](#functions--iife) 对于立即调用(IIFE)的函数表达式，用括号包裹函数体。 eslint: [`wrap-iife`](http://eslint.org/docs/rules/wrap-iife.html) jscs: [`requireParenthesesAroundIIFE`](http://jscs.info/rule/requireParenthesesAroundIIFE)

> 为什么？立即调用函数是一个独立单元 - 用括号包裹函数体可以清晰地表达这一点。需要注意的是，在一个到处都是「模块」的世界，几乎从不需要IIFE。


```javascript
// 立即调用的函数 (IIFE)
(function () {
console.log('Welcome to the Internet. Please follow me.');
}());
```

- [7.3](#7.3) <a name='7.3'></a> 永远不要在一个非函数代码块（`if`、`while` 等）中声明一个函数，把那个函数赋给一个变量。浏览器允许你这么做，但它们的解析表现不一致。

- [7.4](#7.4) <a name='7.4'></a> **注意:** ECMA-262 把 `block` 定义为一组语句。函数声明不是语句。[阅读 ECMA-262 关于这个问题的说明](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97)。

```javascript
// bad
if (currentUser) {
    function test() {
    console.log('Nope.');
    }
}

// good
let test;
if (currentUser) {
    test = () => {
    console.log('Yup.');
    };
}
```

- [7.5](#7.5) <a name='7.5'></a> 永远不要把参数命名为 `arguments`。这将取代原来函数作用域内的 `arguments` 对象。

```javascript
// bad
function nope(name, options, arguments) {
    // ...stuff...
}

// good
function yup(name, options, args) {
    // ...stuff...
}
```

<a name="es6-rest"></a>
- [7.6](#7.6) <a name='7.6'></a> 不要使用 `arguments`。可以选择 rest 语法 `...` 替代。

> 为什么？使用 `...` 能明确你要传入的参数。另外 rest 参数是一个真正的数组，而 `arguments` 是一个类数组。

```javascript
// bad
function concatenateAll() {
    const args = Array.prototype.slice.call(arguments);
    return args.join('');
}

// good
function concatenateAll(...args) {
    return args.join('');
}
```

  <a name="es6-default-parameters"></a>
  - [7.7](#7.7) <a name='7.7'></a> 直接给函数的参数指定默认值，不要使用一个变化的函数参数。

    ```javascript
    // really bad
    function handleThings(opts) {
      // 不！我们不应该改变函数参数。
      // 更加糟糕: 如果参数 opts 是 false 的话，它就会被设定为一个对象。
      // 但这样的写法会造成一些 Bugs。
      //（译注：例如当 opts 被赋值为空字符串，opts 仍然会被下一行代码设定为一个空对象。）
      opts = opts || {};
      // ...
    }

    // still bad
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {};
      }
      // ...
    }

    // good
    function handleThings(opts = {}) {
      // ...
    }
    ```

  - [7.8](#7.8) <a name='7.8'></a> 直接给函数参数赋值时需要避免副作用。

  > 为什么？因为这样的写法让人感到很困惑。

  ```javascript
  var b = 1;
  // bad
  function count(a = b++) {
    console.log(a);
  }
  count();  // 1
  count();  // 2
  count(3); // 3
  count();  // 3
  ```

<a name="functions--defaults-last"></a><a name="7.9"></a>
- [7.9](#functions--defaults-last) Always put default parameters last.

```javascript
// bad
function handleThings(opts = {}, name) {
    // ...
}

// good
function handleThings(name, opts = {}) {
    // ...
}
```

<a name="functions--constructor"></a><a name="7.10"></a>
- [7.10](#functions--constructor) 永远不要使用Function构造函数来创建一个新函数。 eslint: [`no-new-func`](http://eslint.org/docs/rules/no-new-func)

> 为什么？这种方式在分析字符串时与eval()类似，会带来各种问题。

```javascript
// bad
var add = new Function('a', 'b', 'return a + b');

// still bad
var subtract = Function('a', 'b', 'return a - b');
```

<a name="functions--signature-spacing"></a><a name="7.11"></a>

- [7.11](#functions--signature-spacing) 在函数签名中使用空格. eslint: [`space-before-function-paren`](http://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks)

> 为什么？保持一致性是最佳实践， 另外如果在添加或删除名称时也不应增加/删除空格。

```javascript
// bad
const f = function(){};
const g = function (){};
const h = function() {};

// good
const x = function () {};
const y = function a() {};
```

<a name="functions--mutate-params"></a><a name="7.12"></a>
- [7.12](#functions--mutate-params) 永远不要改变（mutate）参数。 eslint: [`no-param-reassign`](http://eslint.org/docs/rules/no-param-reassign.html)

> 为什么？对传入的参数进行操作会在原始调用带来不想要的变量副作用。

```javascript
// bad
function f1(obj) {
    obj.key = 1;
}

// good
function f2(obj) {
    const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
}
```

<a name="functions--reassign-params"></a><a name="7.13"></a>
- [7.13](#functions--reassign-params) 永远不要对参数重新赋值(reassign)。 eslint: [`no-param-reassign`](http://eslint.org/docs/rules/no-param-reassign.html)

> 为什么？对参数重新赋值会引起意料之外的行为，特别是对于`arguments`的访问。同时它会引起优化问题，特别在V8引擎中。


```javascript
// bad
function f1(a) {
    a = 1;
    // ...
}

function f2(a) {
    if (!a) { a = 1; }
    // ...
}

// good
function f3(a) {
    const b = a || 1;
    // ...
}

function f4(a = 1) {
    // ...
}
```

<a name="functions--spread-vs-apply"></a><a name="7.14"></a>
- [7.14](#functions--spread-vs-apply) 推荐使用展开运算符 `...` 来调用可变参数的函数。 eslint: [`prefer-spread`](http://eslint.org/docs/rules/prefer-spread)

> 为什么？这样更加清晰，也不用提供上下文，而且把`new`和 `apply` 组合的方式也比较蛋疼。


```javascript
// bad
const x = [1, 2, 3, 4, 5];
console.log.apply(console, x);

// good
const x = [1, 2, 3, 4, 5];
console.log(...x);

// bad
new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]));

// good
new Date(...[2016, 8, 5]);
```

<a name="functions--signature-invocation-indentation"></a>

- [7.15](#functions--signature-invocation-indentation) 多行签名或调用的函数， 应该和其他多行列表一样保持缩进：每项都占一行，最后一项也应有逗号。

```javascript
// bad
function foo(bar,
                baz,
                quux) {
    // ...
}

// good
function foo(
    bar,
    baz,
    quux,
) {
    // ...
}

// bad
console.log(foo,
    bar,
    baz);

// good
console.log(
    foo,
    bar,
    baz,
);
```

**[⬆ 返回目录](#table-of-contents)**

<a name="arrow-functions"></a>
## 箭头函数

  - [8.1](#8.1) <a name='8.1'></a> 当你必须使用函数表达式（或传递一个匿名函数）时，使用箭头函数符号。eslint: [`prefer-arrow-callback`](http://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](http://eslint.org/docs/rules/arrow-spacing.html) jscs: [`requireArrowFunctions`](http://jscs.info/rule/requireArrowFunctions)

  > 为什么?因为箭头函数创造了新的一个 `this` 执行环境（译注：参考 [Arrow functions - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) 和 [ES6 arrow functions, syntax and lexical scoping](http://toddmotto.com/es6-arrow-functions-syntaxes-and-lexical-scoping/)），通常情况下都能满足你的需求，而且这样的写法更为简洁。

  > 为什么不？如果你有一个相当复杂的函数，你或许可以把逻辑部分转移到一个函数声明上。

    ```javascript
    // bad
    [1, 2, 3].map(function (x) {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  - [8.2](#8.2) <a name='8.2'></a> 如果一个函数适合用一行写出并且只有一个参数，那就把花括号、圆括号和 `return` 都省略掉。如果不是，那就不要省略。eslint: [`arrow-parens`](http://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](http://eslint.org/docs/rules/arrow-body-style.html) jscs:  [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam), [`requireShorthandArrowFunctions`](http://jscs.info/rule/requireShorthandArrowFunctions)

  > 为什么？语法糖。在链式调用中可读性很高。


    ```javascript
    // bad
    [1, 2, 3].map(number => {
      const nextNumber = number + 1;
      `A string containing the ${nextNumber}.`;
    });

    // good
    [1, 2, 3].map(number => `A string containing the ${number}.`);

    // good
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      return `A string containing the ${nextNumber}.`;
    });

    // good
    [1, 2, 3].map((number, index) => ({
      [index]: number,
    }));

    // 当有副作用时，不要隐式return
    function foo(callback) {
      const val = callback();
      if (val === true) {
        // Do something if callback returns true
      }
    }

    let bool = false;

    // bad
    foo(() => bool = true);

    // good
    foo(() => {
      bool = true;
    });
    ```

<a name="arrows--paren-wrap"></a><a name="8.3"></a>

- [8.3](#arrows--paren-wrap) 如果表达式是多行的， 用括号包裹来获得更好的可读性。

> 为什么？这样能更清晰显示出函数的起始和结束。

      ```javascript
      // bad
      ['get', 'post', 'put'].map(httpMethod => Object.prototype.hasOwnProperty.call(
          httpMagicObjectWithAVeryLongName,
          httpMethod,
        )
      );

      // good
      ['get', 'post', 'put'].map(httpMethod => (
        Object.prototype.hasOwnProperty.call(
          httpMagicObjectWithAVeryLongName,
          httpMethod,
        )
      ));
      ```

<a name="arrows--one-arg-parens"></a><a name="8.4"></a>

- [8.4](#arrows--one-arg-parens) 如果函数是单参数且不需要花括号`{}`，则要省略掉括号。否则，始终用括号包裹参数，这样更加清晰，可读性也更好。 注：始终使用括号包裹参数也可以。 在eslint中使用 [“always” option](http://eslint.org/docs/rules/arrow-parens#always) 或在jscs中不引入 [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam)。 eslint: [`arrow-parens`](http://eslint.org/docs/rules/arrow-parens.html) jscs:  [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam)

> 为什么？ 可以避免视觉上的混乱。

    ```javascript
      // bad
      [1, 2, 3].map((x) => x * x);

      // good
      [1, 2, 3].map(x => x * x);

      // good
      [1, 2, 3].map(number => (
        `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
      ));

      // bad
      [1, 2, 3].map(x => {
        const y = x + 1;
        return x * y;
      });

      // good
      [1, 2, 3].map((x) => {
        const y = x + 1;
        return x * y;
      });
    ```

<a name="arrows--confusing"></a><a name="8.5"></a>

- [8.5](#arrows--confusing) Avoid confusing arrow function syntax (`=>`) with comparison operators (`<=`, `>=`). eslint: [`no-confusing-arrow`](http://eslint.org/docs/rules/no-confusing-arrow)

```javascript
// bad
const itemHeight = item => item.height > 256 ? item.largeSize : item.smallSize;

// bad
const itemHeight = (item) => item.height > 256 ? item.largeSize : item.smallSize;

// good
const itemHeight = item => (item.height > 256 ? item.largeSize : item.smallSize);

// good
const itemHeight = (item) => {
  const { height, largeSize, smallSize } = item;
  return height > 256 ? largeSize : smallSize;
};

```

**[⬆ 返回目录](#table-of-contents)**



<a name="constructors"></a>
## 类和构造器

  - [9.1](#9.1) <a name='9.1'></a> 总是使用 `class`。避免直接操作 `prototype` 。

  > 为什么? 因为 `class` 语法更为简洁易读。


    ```javascript

    // bad
    function Queue(contents = []) {
        this._queue = [...contents];
    }
    Queue.prototype.pop = function() {
        const value = this._queue[0];
        this._queue.splice(0, 1);
        return value;
    }


    // good
    class Queue {
        constructor(contents = []) {
        this._queue = [...contents];
        }
        pop() {
        const value = this._queue[0];
        this._queue.splice(0, 1);
        return value;
        }
    }

    ```
    

  - [9.2](#9.2) <a name='9.2'></a> 使用 `extends` 继承。

  > 为什么？因为 `extends` 是一个内建的原型继承方法并且不会破坏 `instanceof`。

    ```javascript
    // bad
    const inherits = require('inherits');
    function PeekableQueue(contents) {
      Queue.apply(this, contents);
    }
    inherits(PeekableQueue, Queue);
    PeekableQueue.prototype.peek = function() {
      return this._queue[0];
    }

    // good
    class PeekableQueue extends Queue {
      peek() {
        return this._queue[0];
      }
    }
    ```

  - [9.3](#9.3) <a name='9.3'></a> 方法可以返回 `this` 来帮助链式调用。

  ```javascript
  // bad
  Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
  };

  Jedi.prototype.setHeight = function(height) {
      this.height = height;
  };

  const luke = new Jedi();
  luke.jump(); // => true
  luke.setHeight(20); // => undefined

  // good
  class Jedi {
      jump() {
      this.jumping = true;
      return this;
      }

      setHeight(height) {
      this.height = height;
      return this;
      }
  }

  const luke = new Jedi();

  luke.jump()
      .setHeight(20);
  ```


  - [9.4](#9.4) <a name='9.4'></a> 可以写一个自定义的 `toString()` 方法，但要确保它能正常运行并且不会引起副作用。

  ```javascript
  class Jedi {
    constructor(options = {}) {
      this.name = options.name || 'no name';
    }

    getName() {
      return this.name;
    }

    toString() {
      return `Jedi - ${this.getName()}`;
    }
  }
  ```

  <a name="constructors--no-useless"></a><a name="9.5"></a>
  - [9.5](#constructors--no-useless) 如果没有显式声明，类都有个默认的构造器(constructor)。一个空或者仅代理了父类的构造函数是不必要的。 eslint: [`no-useless-constructor`](http://eslint.org/docs/rules/no-useless-constructor)

    ```javascript
    // bad
    class Jedi {
      constructor() {}

      getName() {
        return this.name;
      }
    }

    // bad
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
      }
    }

    // good
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
        this.name = 'Rey';
      }
    }
    ```

  <a name="classes--no-duplicate-members"></a>
  - [9.6](#classes--no-duplicate-members) 避免重复类成员。 eslint: [`no-dupe-class-members`](http://eslint.org/docs/rules/no-dupe-class-members)

  > 为什么？ 重复声明类成员，默认最后一个优先级最高 - 几乎肯定是个bug。

    ```javascript
    // bad
    class Foo {
      bar() { return 1; }
      bar() { return 2; }
    }

    // good
    class Foo {
      bar() { return 1; }
    }

    // good
    class Foo {
      bar() { return 2; }
    }
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="modules"></a>
## 模块

  - [10.1](#10.1) <a name='10.1'></a> 总是使用模组 (`import`/`export`) 而不是其他非标准模块系统。你可以编译为你喜欢的模块系统。

  > 为什么？模块就是未来，让我们开始迈向未来吧。

    ```javascript
    // bad
    const AirbnbStyleGuide = require('./AirbnbStyleGuide');
    module.exports = AirbnbStyleGuide.es6;

    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    export default AirbnbStyleGuide.es6;

    // best
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  - [10.2](#10.2) <a name='10.2'></a> 不要使用通配符 import。

  > 为什么？这样能确保你只有一个默认 export。

    ```javascript
    // bad
    import * as AirbnbStyleGuide from './AirbnbStyleGuide';

    // good
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    ```

  - [10.3](#10.3) <a name='10.3'></a>不要从 import 中直接 export。

  > 为什么？虽然一行代码简洁明了，但让 import 和 export 各司其职让事情能保持一致。

    ```javascript
    // bad
    // filename es6.js
    export { es6 as default } from './airbnbStyleGuide';

    // good
    // filename es6.js
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

   <a name="modules--no-duplicate-imports"></a>
   - [10.4](#modules--no-duplicate-imports) 避免从同一个位置重复import.
  eslint: [`no-duplicate-imports`](http://eslint.org/docs/rules/no-duplicate-imports)

   > 为什么？ 这样会降低可维护性。

     ```javascript
     // bad
     import foo from 'foo';
     // … some other imports … //
     import { named1, named2 } from 'foo';

     // good
     import foo, { named1, named2 } from 'foo';

     // good
     import foo, {
       named1,
       named2,
     } from 'foo';
     ```

   <a name="modules--no-mutable-exports"></a>
   - [10.5](#modules--no-mutable-exports) 不要将可变量export。
  eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)

   > 为什么？ 一般来说需要避免可变，特别是在export可变量时。虽然在某些特殊情况下需要这么做，但是一般来说只对常量的引用作export。

     ```javascript
     // bad
     let foo = 3;
     export { foo };

     // good
     const foo = 3;
     export { foo };
     ```

   <a name="modules--prefer-default-export"></a>
   - [10.6](#modules--prefer-default-export) 只有单个export的模块，推荐使用default export而不是具名export。
  eslint: [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)

     ```javascript
     // bad
     export function foo() {}

     // good
     export default function foo() {}
     ```

  <a name="modules--imports-first"></a>
   - [10.7](#modules--imports-first) 把所有的`import`放在非import句式前。
  eslint: [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)

  > 为什么？ `import`存在提升(hoisted)，把它们都置于文件顶部可以避免一些奇怪的行为。

     ```javascript
     // bad
     import foo from 'foo';
     foo.init();

     import bar from 'bar';

     // good
     import foo from 'foo';
     import bar from 'bar';

     foo.init();
     ```

   <a name="modules--multiline-imports-over-newlines"></a>

   - [10.8](#modules--multiline-imports-over-newlines) 多行的import应该像多行Array/object一样保持缩进。

     > 为什么？ 这条规则在style guide里面的其他缩进规则保持一致，包括尾部的逗号。

     ```javascript
     // bad
     import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

     // good
     import {
       longNameA,
       longNameB,
       longNameC,
       longNameD,
       longNameE,
     } from 'path';
     ```

   <a name="modules--no-webpack-loader-syntax"></a>
   - [10.9](#modules--no-webpack-loader-syntax) 在import语句中不允许使用webpack loader语法。
  eslint: [`import/no-webpack-loader-syntax`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)
     
  > 为什么？ 使用这种语法的代码对打包工具有强依赖。推荐在`webpack.config.js`中使用这类语法。

     ```javascript
     // bad
     import fooSass from 'css!sass!foo.scss';
     import barCss from 'style!css!bar.css';

     // good
     import fooSass from 'foo.scss';
     import barCss from 'bar.css';
     ```

**[⬆ 返回目录](#table-of-contents)**

<a name="iterators-and-generators"></a>
## Iterators and Generators

  - [11.1](#11.1) <a name='11.1'></a> 不要使用 iterators。使用高阶函数例如 `map()` 和 `reduce()` 替代 `for-of`。

  > 为什么？这加强了我们不变的规则。处理纯函数的回调值更易读，这比它带来的副作用更重要。

    ```javascript
    const numbers = [1, 2, 3, 4, 5];

    // bad
    let sum = 0;
    for (let num of numbers) {
      sum += num;
    }

    sum === 15;

    // good
    let sum = 0;
    numbers.forEach((num) => sum += num);
    sum === 15;

    // best (use the functional force)
    const sum = numbers.reduce((total, num) => total + num, 0);
    sum === 15;
    ```

  - [11.2](#11.2) <a name='11.2'></a> 现在还不要使用 generators。

  > 为什么？因为它们现在还没法很好地编译到 ES5。 

  <a name="generators--spacing"></a>
  - [11.3](#generators--spacing) 如果必须要使用generator, 那么要确保函数签名中使用正确的空格格式。 eslint: [`generator-star-spacing`](http://eslint.org/docs/rules/generator-star-spacing)

  > 为什么？ `function` 和 `*` 都是概念上的关键字 - `*` 不是 `function` 的修饰符, `function*` 是一个统一结构体, 与`function`不同。

    ```javascript
    // bad
    function * foo() {
      // ...
    }

    // bad
    const bar = function * () {
      // ...
    };

    // bad
    const baz = function *() {
      // ...
    };

    // bad
    const quux = function*() {
      // ...
    };

    // bad
    function*foo() {
      // ...
    }

    // bad
    function *foo() {
      // ...
    }

    // very bad
    function
    *
    foo() {
      // ...
    }

    // very bad
    const wat = function
    *
    () {
      // ...
    };

    // good
    function* foo() {
      // ...
    }

    // good
    const foo = function* () {
      // ...
    };
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="properties"></a>
## 属性

  - [12.1](#12.1) <a name='12.1'></a> 使用 `.` 来访问对象的属性。eslint: [`dot-notation`](http://eslint.org/docs/rules/dot-notation.html) jscs: [`requireDotNotation`](http://jscs.info/rule/requireDotNotation)

  ```javascript
  const luke = {
    jedi: true,
    age: 28,
  };

  // bad
  const isJedi = luke['jedi'];

  // good
  const isJedi = luke.jedi;
  ```

  - [12.2](#12.2) <a name='12.2'></a> 当通过变量访问属性时使用中括号 `[]`。

  ```javascript
  const luke = {
    jedi: true,
    age: 28,
  };

  function getProp(prop) {
    return luke[prop];
  }

  const isJedi = getProp('jedi');
  ```

**[⬆ 返回目录](#table-of-contents)**

<a name="variables"></a>
## 变量

  - [13.1](#13.1) <a name='13.1'></a> 一直使用 `const` 来声明变量，如果不这样做就会产生全局变量。我们需要避免全局命名空间的污染。[地球队长](http://www.wikiwand.com/en/Captain_Planet)已经警告过我们了。（译注：全局，global 亦有全球的意思。地球队长的责任是保卫地球环境，所以他警告我们不要造成「全球」污染。）eslint: [`no-undef`](http://eslint.org/docs/rules/no-undef) [`prefer-const`](http://eslint.org/docs/rules/prefer-const)


  ```javascript
  // bad
  superPower = new SuperPower();

  // good
  const superPower = new SuperPower();
  ```

  - [13.2](#13.2) <a name='13.2'></a> 使用 `const` 声明每一个变量。eslint: [`one-var`](http://eslint.org/docs/rules/one-var.html) jscs: [`disallowMultipleVarDecl`](http://jscs.info/rule/disallowMultipleVarDecl)

  > 为什么？增加新变量将变的更加容易，而且你永远不用再担心调换错 `;` 跟 `,`。

  ```javascript
  // bad
  const items = getItems(),
      goSportsTeam = true,
      dragonball = 'z';

  // bad
  // (compare to above, and try to spot the mistake)
  const items = getItems(),
      goSportsTeam = true;
      dragonball = 'z';

  // good
  const items = getItems();
  const goSportsTeam = true;
  const dragonball = 'z';
  ```

  - [13.3](#13.3) <a name='13.3'></a> 将所有的 `const` 和 `let` 分组

  > 为什么？当你需要把已赋值变量赋值给未赋值变量时非常有用。

    ```javascript
    // bad
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // good
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
    ```

  - [13.4](#13.4) <a name='13.4'></a> 在你需要的地方给变量赋值，但请把它们放在一个合理的位置。

  > 为什么？`let` 和 `const` 是块级作用域而不是函数作用域。

    ```javascript
    // good
    function() {
      test();
      console.log('doing stuff..');

      //..other stuff..

      const name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // bad - unnecessary function call
    function(hasName) {
      const name = getName();

      if (!hasName) {
        return false;
      }

      this.setFirstName(name);

      return true;
    }

    // good
    function(hasName) {
      if (!hasName) {
        return false;
      }

      const name = getName();
      this.setFirstName(name);

      return true;
    }
    ```

    <a name="variables--no-chain-assignment"></a><a name="13.5"></a>
    - [13.5](#variables--no-chain-assignment) 不要使用链式变量赋值.

      > 为什么？链式变量赋值会创建隐式全局变量。

      ```javascript
      // bad
      (function example() {
        // JavaScript 解释器将这个语句解释为
        // let a = ( b = ( c = 1 ) );
        // let关键字仅对a生效，b和c变成了全局变量。
        let a = b = c = 1;
      }());

      console.log(a); // throws ReferenceError
      console.log(b); // 1
      console.log(c); // 1

      // good
      (function example() {
        let a = 1;
        let b = a;
        let c = a;
      }());

      console.log(a); // throws ReferenceError
      console.log(b); // throws ReferenceError
      console.log(c); // throws ReferenceError

      // 对于 `const`也是一样的。
      ```

  <a name="variables--unary-increment-decrement"></a><a name="13.6"></a>

  - [13.6](#variables--unary-increment-decrement) 避免使用一元增减运算符(++, --)。 eslint [`no-plusplus`](http://eslint.org/docs/rules/no-plusplus)

      > 为什么？ 根据eslint文档， 一元增减运算符会自动加入分号，在应用中会引起静默错误。使用`num += 1` 代替 `num++` 或 `num ++`来变更值会显得更加易读。 不使用一元增减运算符也可以避免在不注意的情况下值做了预增减（pre-incrementing/pre-decrementing），也会导致预期外的错误。

      ```javascript
      // bad

      const array = [1, 2, 3];
      let num = 1;
      num++;
      --num;

      let sum = 0;
      let truthyCount = 0;
      for (let i = 0; i < array.length; i++) {
        let value = array[i];
        sum += value;
        if (value) {
          truthyCount++;
        }
      }

      // good

      const array = [1, 2, 3];
      let num = 1;
      num += 1;
      num -= 1;

      const sum = array.reduce((a, b) => a + b, 0);
      const truthyCount = array.filter(Boolean).length;
      ```

**[⬆ 返回目录](#table-of-contents)**

<a name="hoisting"></a>
## Hoisting

  - [14.1](#14.1) <a name='14.1'></a> `var` 声明会被提升至该作用域的顶部，但它们赋值不会提升。`let` 和 `const` 被赋予了一种称为「[暂时性死区（Temporal Dead Zones, TDZ）](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone_and_errors_with_let)」的概念。这对于了解为什么 [type of 不再安全](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15)相当重要。

  ```javascript
  // 我们知道这样运行不了
  // （假设 notDefined 不是全局变量）
  function example() {
    console.log(notDefined); // => throws a ReferenceError
  }

  // 由于变量提升的原因，
  // 在引用变量后再声明变量是可以运行的。
  // 注：变量的赋值 `true` 不会被提升。
  function example() {
    console.log(declaredButNotAssigned); // => undefined
    var declaredButNotAssigned = true;
  }

  // 编译器会把函数声明提升到作用域的顶层，
  // 这意味着我们的例子可以改写成这样：
  function example() {
    let declaredButNotAssigned;
    console.log(declaredButNotAssigned); // => undefined
    declaredButNotAssigned = true;
  }

  // 使用 const 和 let
  function example() {
    console.log(declaredButNotAssigned); // => throws a ReferenceError
    console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
    const declaredButNotAssigned = true;
  }
  ```

  - [14.2](#14.2) <a name='14.2'></a> 匿名函数表达式的变量名会被提升，但函数内容并不会。

  ```javascript
  function example() {
    console.log(anonymous); // => undefined

    anonymous(); // => TypeError anonymous is not a function

    var anonymous = function() {
      console.log('anonymous function expression');
    };
  }
  ```

  - [14.3](#14.3) <a name='14.3'></a> 命名的函数表达式的变量名会被提升，但函数名和函数函数内容并不会。

  ```javascript
  function example() {
    console.log(named); // => undefined

    named(); // => TypeError named is not a function

    superPower(); // => ReferenceError superPower is not defined

    var named = function superPower() {
      console.log('Flying');
    };
  }

  // the same is true when the function name
  // is the same as the variable name.
  function example() {
    console.log(named); // => undefined

    named(); // => TypeError named is not a function

    var named = function named() {
      console.log('named');
    }
  }
  ```

  - [14.4](#14.4) <a name='14.4'></a> 函数声明的名称和函数体都会被提升。

  ```javascript
  function example() {
    superPower(); // => Flying

    function superPower() {
      console.log('Flying');
    }
  }
  ```

  - 想了解更多信息，参考 [Ben Cherry](http://www.adequatelygood.com/) 的 [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting)。

**[⬆ 返回目录](#table-of-contents)**
