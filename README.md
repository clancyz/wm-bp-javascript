# Waimai Business Platform JavaScript Style Guide() {

**基于[Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)的javascript代码规范**


本文是[完整代码规范建议](airbnb.md)的**子集**，提及内容为`eslint`可检查部分。

修改的部分会以`[modified]`标注，无此说明的即与原文规约相同。

<a name="table-of-contents"></a>
## 目录

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


<a name="references"></a>
## 引用

- [2.1](#2.1) <a name='2.1'></a> **[强制]** 对所有的引用使用 `const` ；不要使用 `var`。eslint: [`prefer-const`](http://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](http://eslint.org/docs/rules/no-const-assign.html)


  > 为什么？这能确保你无法对引用重新赋值，也不会导致出现 bug 或难以理解。

  ```javascript
  // bad
  var a = 1;
  var b = 2;

  // good
  const a = 1;
  const b = 2;
  ```

- [2.2](#2.2) <a name='2.2'></a> **[强制]** 如果你一定需要可变动的引用，使用 `let` 代替 `var`。 eslint: [`no-var`](http://eslint.org/docs/rules/no-var.html) jscs: [`disallowVar`](http://jscs.info/rule/disallowVar)

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

**[⬆ 返回目录](#table-of-contents)**

<a name="objects"></a>
## 对象

- [3.1](#3.1) <a name='3.1'></a> **[强制]** 使用字面值创建对象。eslint: [`no-new-object`](http://eslint.org/docs/rules/no-new-object.html)

  ```javascript
  // bad
  const item = new Object();

  // good
  const item = {};
  ```

<a name="es6-object-concise"></a>

- [3.4](#3.4) <a name='3.4'></a> **[强制]** 使用对象属性值的简写。eslint: [`object-shorthand`](http://eslint.org/docs/rules/object-shorthand.html) jscs: [`requireEnhancedObjectLiterals`](http://jscs.info/rule/requireEnhancedObjectLiterals)

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


- [3.6](#3.6) <a name="3.6"></a> **[强制]** 仅在对象属性有特殊符号时使用引号包裹。 eslint: [`quote-props`](http://eslint.org/docs/rules/quote-props.html) jscs: [`disallowQuotedKeysInObjects`](http://jscs.info/rule/disallowQuotedKeysInObjects)

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

**[⬆ 返回目录](#table-of-contents)**

<a name="arrays"></a>
## 数组

- [4.1](#4.1) <a name='4.1'></a> **[强制]** 使用字面值创建数组。eslint: [`no-array-constructor`](http://eslint.org/docs/rules/no-array-constructor.html)

  ```javascript
  // bad
  const items = new Array();

  // good
  const items = [];
  ```

<a name="arrays--callback-return"></a><a name="4.5"></a>

- [4.5](#arrays--callback-return) **[强制]** 数组的相关方法使用return语句。如果函数体仅由一个带表达式且无副作用的语句组成，可以忽略return。参考[8.2](#arrows--implicit-return). eslint: [`array-callback-return`](http://eslint.org/docs/rules/array-callback-return)

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


<a name="destructuring"></a>

## 解构


<a name="strings"></a>
## Strings

- [6.1](#6.1) <a name='6.1'></a> **[强制]** 字符串使用单引号 `''` 。eslint: [`quotes`](http://eslint.org/docs/rules/quotes.html) jscs: [`validateQuoteMarks`](http://jscs.info/rule/validateQuoteMarks)

  ```javascript
  // bad
  const name = "Capt. Janeway";

  // good
  const name = 'Capt. Janeway';
  ```

- [6.2](#6.2) <a name='6.2'></a> `[modified]`**[建议]** 字符串超过 100 个字节应该使用字符串连接号换行。
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

- [6.4](#6.4) <a name='6.4'></a>  `[modified]`**[建议]** 程序化生成字符串时，使用模板字符串代替字符串连接。eslint: [`prefer-template`](http://eslint.org/docs/rules/prefer-template.html) [`template-curly-spacing`](http://eslint.org/docs/rules/template-curly-spacing) jscs: [`requireTemplateStrings`](http://jscs.info/rule/requireTemplateStrings)


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

- [6.4](#strings--eval) **[强制]** 不要对字符串使用 `eval()`，会导致一系列问题。 eslint: [`no-eval`](http://eslint.org/docs/rules/no-eval)

<a name="strings--escaping"></a>

- [6.5](#strings--escaping)  `[modified]`**[建议]** 不要对字符串使用不必要的escape操作。 eslint: [`no-useless-escape`](http://eslint.org/docs/rules/no-useless-escape)

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

- [7.1](#7.1) <a name='7.1'></a> `[modified]`**[建议]** 使用具名函数代替函数声明。 eslint: [`func-style`](http://eslint.org/docs/rules/func-style) jscs: [`disallowFunctionDeclarations`](http://jscs.info/rule/disallowFunctionDeclarations)

  > 为什么？因为函数声明会把函数提升(hoisted), 这样易使函数在定义前被引用。这会影响可读性和可维护性。如果一个函数的定义很长或很复杂，会干扰对文件剩余部分的理解，更好的方式是将它抽象在它自己的模块中。别忘了给函数表达式命名 - 匿名函数会使得在错误调用栈中定位问题变得困难。([讨论](https://github.com/airbnb/javascript/issues/794))

  ```javascript
  // bad
  function foo() {
    // ...
  }

  // bad
  const foo = function () {
    // ...
  };

  // good
  const foo = function bar() {
    // ...
  };
  ```

<a name="functions--iife"></a><a name="7.2"></a>

- [7.2](#functions--iife) **[强制]**  对于立即调用(IIFE)的函数表达式，用括号包裹函数体。 eslint: [`wrap-iife`](http://eslint.org/docs/rules/wrap-iife.html) jscs: [`requireParenthesesAroundIIFE`](http://jscs.info/rule/requireParenthesesAroundIIFE)

  > 为什么？立即调用函数是一个独立单元 - 用括号包裹函数体可以清晰地表达这一点。需要注意的是，在一个到处都是「模块」的世界，几乎从不需要IIFE。


  ```javascript
  // 立即调用的函数 (IIFE)
  (function () {
  console.log('Welcome to the Internet. Please follow me.');
  }());
  ```

- [7.3](#7.3) <a name='7.3'></a> **[强制]** 永远不要在一个非函数代码块（`if`、`while` 等）中声明一个函数，把那个函数赋给一个变量。浏览器允许你这么做，但它们的解析表现不一致。eslint: [`no-loop-func`](http://eslint.org/docs/rules/no-loop-func.html)

<a name="es6-rest"></a>

- [7.6](#7.6) <a name='7.6'></a> `[modified]`**[建议]** 不要使用 `arguments`。可以选择 rest 语法 `...` 替代。 eslint: [`prefer-rest-params`](http://eslint.org/docs/rules/prefer-rest-params)

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

<a name="functions--constructor"></a><a name="7.10"></a>

- [7.10](#functions--constructor) **[强制]**  永远不要使用Function构造函数来创建一个新函数。 eslint: [`no-new-func`](http://eslint.org/docs/rules/no-new-func)

  > 为什么？这种方式在分析字符串时与eval()类似，会带来各种问题。

  ```javascript
  // bad
  var add = new Function('a', 'b', 'return a + b');

  // still bad
  var subtract = Function('a', 'b', 'return a - b');
  ```

<a name="functions--signature-spacing"></a><a name="7.11"></a>

- [7.11](#functions--signature-spacing)  **[强制]**  在函数签名中使用空格. eslint: [`space-before-function-paren`](http://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks)

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

- [7.12](#functions--mutate-params) **[强制]** 永远不要改变（mutate）参数。 eslint: [`no-param-reassign`](http://eslint.org/docs/rules/no-param-reassign.html)

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

- [7.13](#functions--reassign-params) **[强制]** 永远不要对参数重新赋值(reassign)。 eslint: [`no-param-reassign`](http://eslint.org/docs/rules/no-param-reassign.html)

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

- [7.14](#functions--spread-vs-apply) `[modified]`**[建议]**  推荐使用展开运算符 `...` 来调用可变参数的函数。 eslint: [`prefer-spread`](http://eslint.org/docs/rules/prefer-spread)

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

- [7.15](#functions--signature-invocation-indentation)  **[强制]**  多行签名或调用的函数， 应该和其他多行列表一样保持缩进：每项都占一行，最后一项也应有逗号。

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

  - [8.1](#8.1) <a name='8.1'></a>   **[强制]**  当你必须使用函数表达式（或传递一个匿名函数）时，使用箭头函数符号。eslint: [`prefer-arrow-callback`](http://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](http://eslint.org/docs/rules/arrow-spacing.html) jscs: [`requireArrowFunctions`](http://jscs.info/rule/requireArrowFunctions)

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

  - [8.2](#8.2) <a name='8.2'></a>   **[强制]**  如果一个函数适合用一行写出并且只有一个参数，那就把花括号、圆括号和 `return` 都省略掉。如果不是，那就不要省略。eslint: [`arrow-parens`](http://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](http://eslint.org/docs/rules/arrow-body-style.html) jscs:  [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam), [`requireShorthandArrowFunctions`](http://jscs.info/rule/requireShorthandArrowFunctions)

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

<a name="arrows--one-arg-parens"></a><a name="8.4"></a>

- [8.4](#arrows--one-arg-parens)   **[强制]**  如果函数是单参数且不需要花括号`{}`，则要省略掉括号。否则，始终用括号包裹参数，这样更加清晰，可读性也更好。 注：始终使用括号包裹参数也可以。 在eslint中使用 [“always” option](http://eslint.org/docs/rules/arrow-parens#always) 或在jscs中不引入 [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam)。 eslint: [`arrow-parens`](http://eslint.org/docs/rules/arrow-parens.html) jscs:  [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam)

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

- [8.5](#arrows--confusing)   `[modified]`**[建议]**  在箭头函数后避免使用 (`<=`, `>=`)这样的比较操作符。 eslint: [`no-confusing-arrow`](http://eslint.org/docs/rules/no-confusing-arrow)

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

- [9.1](#9.1) <a name='9.1'></a>  **[强制]** 总是使用 `class`。避免直接操作 `prototype` 。

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

<a name="constructors--no-useless"></a><a name="9.5"></a>

- [9.5](#constructors--no-useless)  **[强制]** 如果没有显式声明，类都有个默认的构造器(constructor)。一个空或者仅代理了父类的构造函数是不必要的。 eslint: [`no-useless-constructor`](http://eslint.org/docs/rules/no-useless-constructor)

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
  
- [9.6](#classes--no-duplicate-members) **[强制]** 避免重复类成员。 eslint: [`no-dupe-class-members`](http://eslint.org/docs/rules/no-dupe-class-members)

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

<a name="modules--no-duplicate-imports"></a>

- [10.4](#modules--no-duplicate-imports)  **[强制]** 避免从同一个位置重复import.
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

- [10.5](#modules--no-mutable-exports)  **[强制]** 不要将可变量export。eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)

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

- [10.6](#modules--prefer-default-export)  **[强制]** 只有单个export的模块，推荐使用default export而不是具名export。
  eslint: [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)

  ```javascript
  // bad
  export function foo() {}

  // good
  export default function foo() {}
  ```

<a name="modules--imports-first"></a>

- [10.7](#modules--imports-first)  **[强制]** 把所有的`import`放在非import句式前。
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

<a name="modules--no-webpack-loader-syntax"></a>

- [10.9](#modules--no-webpack-loader-syntax)  **[强制]** 在import语句中不允许使用webpack loader语法。
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
## 迭代器和生成器（Iterators and Generators）

- [11.3](#generators--spacing) **[强制]**  如果必须要使用generator, 那么要确保函数签名中使用正确的空格格式。 eslint: [`generator-star-spacing`](http://eslint.org/docs/rules/generator-star-spacing)

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

- [12.1](#12.1) <a name='12.1'></a>  **[强制]** 使用 `.` 来访问对象的属性。eslint: [`dot-notation`](http://eslint.org/docs/rules/dot-notation.html) jscs: [`requireDotNotation`](http://jscs.info/rule/requireDotNotation)

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

**[⬆ 返回目录](#table-of-contents)**

<a name="variables"></a>
## 变量

- [13.1](#13.1) <a name='13.1'></a> **[强制]** 一直使用 `const` 来声明变量，如果不这样做就会产生全局变量。我们需要避免全局命名空间的污染。[地球队长](http://www.wikiwand.com/en/Captain_Planet)已经警告过我们了。（译注：全局，global 亦有全球的意思。地球队长的责任是保卫地球环境，所以他警告我们不要造成「全球」污染。）eslint: [`no-undef`](http://eslint.org/docs/rules/no-undef) [`prefer-const`](http://eslint.org/docs/rules/prefer-const)


  ```javascript
  // bad
  superPower = new SuperPower();

  // good
  const superPower = new SuperPower();
  ```

- [13.2](#13.2) <a name='13.2'></a>  **[强制]** 使用 `const` 声明每一个变量。eslint: [`one-var`](http://eslint.org/docs/rules/one-var.html) jscs: [`disallowMultipleVarDecl`](http://jscs.info/rule/disallowMultipleVarDecl)

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

<a name="variables--no-chain-assignment"></a><a name="13.5"></a>

- [13.5](#variables--no-chain-assignment) **[强制]** 不要使用链式变量赋值.

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

- [13.6](#variables--unary-increment-decrement)  `[modified]`**[建议]** 避免使用一元增减运算符(++, --)。 eslint [`no-plusplus`](http://eslint.org/docs/rules/no-plusplus)

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


- 想了解更多信息，参考 [Ben Cherry](http://www.adequatelygood.com/) 的 [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting)。

**[⬆ 返回目录](#table-of-contents)**

<a name="blocks"></a>
## 代码块

- [16.2](#16.2) <a name='16.2'></a>  **[强制]** 如果通过 `if` 和 `else` 使用多行代码块，把 `else` 放在 `if` 代码块关闭括号的同一行。eslint: [`brace-style`](http://eslint.org/docs/rules/brace-style.html) jscs:  [`disallowNewlineBeforeBlockStatements`](http://jscs.info/rule/disallowNewlineBeforeBlockStatements)

  ```javascript
  // bad
  if (test) {
    thing1();
    thing2();
  }
  else {
    thing3();
  }

  // good
  if (test) {
    thing1();
    thing2();
  } else {
    thing3();
  }
  ```


**[⬆ 返回目录](#table-of-contents)**

## 控制语句

<a name="control-statements"></a>

- [17.1](#control-statements)  `[modified]`**[建议]** 在控制语句中 (`if`, `while` 等)，如果超过了一行的最大长度，应该对每个分组换行。逻辑运算符应该在行首或行尾取决于你自己。

  ```javascript
  // bad
  if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
    thing1();
  }

  // bad
  if (foo === 123 &&
    bar === 'abc') {
    thing1();
  }

  // bad
  if (foo === 123
    && bar === 'abc') {
    thing1();
  }

  // good
  if (
    (foo === 123 || bar === "abc") &&
    doesItLookGoodWhenItBecomesThatLong() &&
    isThisReallyHappening()
  ) {
    thing1();
  }

  // good
  if (foo === 123 && bar === 'abc') {
    thing1();
  }

  // good
  if (
    foo === 123 &&
    bar === 'abc'
  ) {
    thing1();
  }

  // good
  if (
    foo === 123
    && bar === 'abc'
  ) {
    thing1();
  }
  ```


<a name="comments"></a>

## 注释

- [18.3](#comments--spaces)  **[强制]** 所有注释开头加一个空格，增加可读性。eslint: [`spaced-comment`](http://eslint.org/docs/rules/spaced-comment)

  ```javascript
  // bad
  //is current tab
  const active = true;

  // good
  // is current tab
  const active = true;

  // bad
  /**
   *make() returns a new element
   *based on the passed-in tag name
   */
  function make(tag) {

    // ...

    return element;
  }

  // good
  /**
   * make() returns a new element
   * based on the passed-in tag name
   */
  function make(tag) {

    // ...

    return element;
  }
  ```

**[⬆ 返回目录](#table-of-contents)**

<a name="whitespace"></a>

## 空格

- [19.1](#19.1) <a name='19.1'></a>  **[强制]** 使用 2 个空格作为缩进。eslint: [`indent`](http://eslint.org/docs/rules/indent.html) jscs: [`validateIndentation`](http://jscs.info/rule/validateIndentation)

  ```javascript
  // bad
  function() {
  ∙∙∙∙const name;
  }

  // bad
  function() {
  ∙const name;
  }

  // good
  function() {
  ∙∙const name;
  }
  ```

- [19.2](#19.2) <a name='19.2'></a>  **[强制]** 在花括号前放一个空格。eslint: [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks.html) jscs: [`requireSpaceBeforeBlockStatements`](http://jscs.info/rule/requireSpaceBeforeBlockStatements)

  ```javascript
  // bad
  function test(){
    console.log('test');
  }

  // good
  function test() {
    console.log('test');
  }

  // bad
  dog.set('attr',{
    age: '1 year',
    breed: 'Bernese Mountain Dog',
  });

  // good
  dog.set('attr', {
    age: '1 year',
    breed: 'Bernese Mountain Dog',
  });
  ```

- [19.3](#19.3) <a name='19.3'></a>  **[强制]** 在控制语句（`if`、`while` 等）的小括号前放一个空格。在函数调用及声明中，不在函数的参数列表前加空格。 eslint: [`keyword-spacing`](http://eslint.org/docs/rules/keyword-spacing.html) jscs: [`requireSpaceAfterKeywords`](http://jscs.info/rule/requireSpaceAfterKeywords)

  ```javascript
  // bad
  if(isJedi) {
    fight ();
  }

  // good
  if (isJedi) {
    fight();
  }

  // bad
  function fight () {
    console.log ('Swooosh!');
  }

  // good
  function fight() {
    console.log('Swooosh!');
  }
  ```

- [19.4](#19.4) <a name='19.4'></a>  **[强制]** 使用空格把运算符隔开。eslint: [`space-infix-ops`](http://eslint.org/docs/rules/space-infix-ops.html) jscs: [`requireSpaceBeforeBinaryOperators`](http://jscs.info/rule/requireSpaceBeforeBinaryOperators), [`requireSpaceAfterBinaryOperators`](http://jscs.info/rule/requireSpaceAfterBinaryOperators)

  ```javascript
  // bad
  const x=y+5;

  // good
  const x = y + 5;
  ```

- [19.5](#19.5) <a name='19.5'></a>  **[强制]** 在文件末尾插入一个空行。 eslint: [`eol-last`](https://github.com/eslint/eslint/blob/master/docs/rules/eol-last.md)

  ```javascript
  // bad
  (function(global) {
    // ...stuff...
  })(this);
  ```

  ```javascript
  // bad
  (function(global) {
    // ...stuff...
  })(this);↵
  ↵
  ```

  ```javascript
  // good
  (function(global) {
    // ...stuff...
  })(this);↵
  ```

- [19.6](#19.6) <a name='19.6'></a>  `[modified]`**[建议]** 在使用长方法链时进行缩进。使用前面的点 `.` 强调这是方法调用而不是新语句。 eslint: [`newline-per-chained-call`](http://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](http://eslint.org/docs/rules/no-whitespace-before-property)

  ```javascript
  // bad
  $('#items').find('.selected').highlight().end().find('.open').updateCount();

  // bad
  $('#items').
    find('.selected').
      highlight().
      end().
    find('.open').
      updateCount();

  // good
  $('#items')
    .find('.selected')
      .highlight()
      .end()
    .find('.open')
      .updateCount();

  // bad
  const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
      .attr('width', (radius + margin) * 2).append('svg:g')
      .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
      .call(tron.led);

  // good
  const leds = stage.selectAll('.led')
      .data(data)
    .enter().append('svg:svg')
      .classed('led', true)
      .attr('width', (radius + margin) * 2)
    .append('svg:g')
      .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
      .call(tron.led);
  ```

<a name="whitespace--padded-blocks"></a><a name="18.8"></a>

- [19.8](#whitespace--padded-blocks)  **[强制]** 不要用代码块起始/结束位置加入空行。eslint: [`padded-blocks`](http://eslint.org/docs/rules/padded-blocks.html) jscs:  [`disallowPaddingNewlinesInBlocks`](http://jscs.info/rule/disallowPaddingNewlinesInBlocks)

  ```javascript
  // bad
  function bar() {

    console.log(foo);

  }

  // also bad
  if (baz) {

    console.log(qux);
  } else {
    console.log(foo);

  }

  // good
  function bar() {
    console.log(foo);
  }

  // good
  if (baz) {
    console.log(qux);
  } else {
    console.log(foo);
  }
  ```

<a name="whitespace--in-parens"></a><a name="18.9"></a>

- [19.9](#whitespace--in-parens)  **[强制]** 不要在括号前后加入空格。eslint: [`space-in-parens`](http://eslint.org/docs/rules/space-in-parens.html) jscs: [`disallowSpacesInsideParentheses`](http://jscs.info/rule/disallowSpacesInsideParentheses)

  ```javascript
  // bad
  function bar( foo ) {
    return foo;
  }

  // good
  function bar(foo) {
    return foo;
  }

  // bad
  if ( foo ) {
    console.log(foo);
  }

  // good
  if (foo) {
    console.log(foo);
  }
  ```

<a name="whitespace--in-brackets"></a><a name="18.10"></a>

- [19.10](#whitespace--in-brackets)  **[强制]** 不要在数组起始和尾部加入空格。 eslint: [`array-bracket-spacing`](http://eslint.org/docs/rules/array-bracket-spacing.html) jscs: [`disallowSpacesInsideArrayBrackets`](http://jscs.info/rule/disallowSpacesInsideArrayBrackets)

  ```javascript
  // bad
  const foo = [ 1, 2, 3 ];
  console.log(foo[ 0 ]);

  // good
  const foo = [1, 2, 3];
  console.log(foo[0]);
  ```

<a name="whitespace--in-braces"></a><a name="18.11"></a>

- [19.11](#whitespace--in-braces)  **[强制]** 在花括号首尾加入空格。 eslint: [`object-curly-spacing`](http://eslint.org/docs/rules/object-curly-spacing.html) jscs: [`requireSpacesInsideObjectBrackets`](http://jscs.info/rule/requireSpacesInsideObjectBrackets)

  ```javascript
  // bad
  const foo = {clark: 'kent'};

  // good
  const foo = { clark: 'kent' };
  ```

<a name="whitespace--max-len"></a><a name="18.12"></a>

- [19.12](#whitespace--max-len)  **[强制]** 避免一行超过100个字符。备注：每个如上[above](#strings--line-length)的长字符串 可以不遵循这条规则。 eslint: [`max-len`](http://eslint.org/docs/rules/max-len.html) jscs: [`maximumLineLength`](http://jscs.info/rule/maximumLineLength)

  > 为什么？这样做可以增加可读性和可维护性。

  ```javascript
  // bad
  const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

  // bad
  $.ajax({ method: 'POST', url: 'https://airbnb.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

  // good
  const foo = jsonData
    && jsonData.foo
    && jsonData.foo.bar
    && jsonData.foo.bar.baz
    && jsonData.foo.bar.baz.quux
    && jsonData.foo.bar.baz.quux.xyzzy;

  // good
  $.ajax({
    method: 'POST',
    url: 'https://airbnb.com/',
    data: { name: 'John' },
  })
    .done(() => console.log('Congratulations!'))
    .fail(() => console.log('You have failed this city.'));
  ```


**[⬆ 返回目录](#table-of-contents)**

<a name="commas"></a>
## 逗号

- [20.1](#20.1) <a name='20.1'></a>  **[强制]** 行首逗号：**不需要**。eslint: [`comma-style`](http://eslint.org/docs/rules/comma-style.html) jscs: [`requireCommaBeforeLineBreak`](http://jscs.info/rule/requireCommaBeforeLineBreak)

  ```javascript
  // bad
  const story = [
      once
    , upon
    , aTime
  ];

  // good
  const story = [
    once,
    upon,
    aTime,
  ];

  // bad
  const hero = {
      firstName: 'Ada'
    , lastName: 'Lovelace'
    , birthYear: 1815
    , superPower: 'computers'
  };

  // good
  const hero = {
    firstName: 'Ada',
    lastName: 'Lovelace',
    birthYear: 1815,
    superPower: 'computers',
  };
  ```

- [20.2](#20.2) <a name='20.2'></a>  **[强制]** 增加结尾的逗号: **需要**。eslint: [`comma-dangle`](http://eslint.org/docs/rules/comma-dangle.html) jscs: [`requireTrailingComma`](http://jscs.info/rule/requireTrailingComma)

  > 为什么? 这会让 git diffs 更干净。另外，像 babel 这样的转译器会移除结尾多余的逗号，也就是说你不必担心老旧浏览器的[尾逗号问题](es5/README.md#commas)。

  ```javascript
  // bad - git diff without trailing comma
  const hero = {
        firstName: 'Florence',
  -    lastName: 'Nightingale'
  +    lastName: 'Nightingale',
  +    inventorOf: ['coxcomb graph', 'modern nursing']
  }

  // good - git diff with trailing comma
  const hero = {
        firstName: 'Florence',
        lastName: 'Nightingale',
  +    inventorOf: ['coxcomb chart', 'modern nursing'],
  }

  // bad
  const hero = {
    firstName: 'Dana',
    lastName: 'Scully'
  };

  const heroes = [
    'Batman',
    'Superman'
  ];

  // good
  const hero = {
    firstName: 'Dana',
    lastName: 'Scully',
  };

  const heroes = [
    'Batman',
    'Superman',
  ];
  ```

**[⬆ 返回目录](#table-of-contents)**

<a name="semicolons"></a>
## 分号

- [21.1](#21.1) <a name='21.1'></a>  **[强制]** **在语句末使用分号** eslint: [`semi`](http://eslint.org/docs/rules/semi.html) jscs: [`requireSemicolons`](http://jscs.info/rule/requireSemicolons)


  ```javascript
  // bad
  (function() {
    const name = 'Skywalker'
    return name
  })()

  // good
  (() => {
    const name = 'Skywalker';
    return name;
  })();

  // good (防止函数在两个 IIFE 合并时被当成一个参数)
  ;(() => {
    const name = 'Skywalker';
    return name;
  })();
  ```

  [Read more](http://stackoverflow.com/a/7365214/1712802).

**[⬆ 返回目录](#table-of-contents)**

<a name="type-casting--coercion"></a>
## 强制类型转换

- [22.3](#22.3) <a name='22.3'></a>  **[强制]** 对数字使用 `parseInt` 转换，并带上类型转换的基数。eslint: [`radix`](http://eslint.org/docs/rules/radix)


  ```javascript
  const inputValue = '4';

  // bad
  const val = new Number(inputValue);

  // bad
  const val = +inputValue;

  // bad
  const val = inputValue >> 0;

  // bad
  const val = parseInt(inputValue);

  // good
  const val = Number(inputValue);

  // good
  const val = parseInt(inputValue, 10);
  ```


**[⬆ 返回目录](#table-of-contents)**

<a name="naming-conventions"></a>
## 命名规则

- [23.1](#23.1) <a name='23.1'></a>  `[modified]`**[建议]** 避免单字母命名。命名应语义化。eslint: [`id-length`](http://eslint.org/docs/rules/id-length)


  ```javascript
  // bad
  function q() {
    // ...stuff...
  }

  // good
  function query() {
    // ..stuff..
  }
  ```

- [23.2](#23.2) <a name='23.2'></a>  **[强制]** 使用驼峰式命名对象、函数和实例。eslint: [`camelcase`](http://eslint.org/docs/rules/camelcase.html) jscs: [`requireCamelCaseOrUpperCaseIdentifiers`](http://jscs.info/rule/requireCamelCaseOrUpperCaseIdentifiers)

  ```javascript
  // bad
  const OBJEcttsssss = {};
  const this_is_my_object = {};
  function c() {}

  // good
  const thisIsMyObject = {};
  function thisIsMyFunction() {}
  ```

- [23.3](#23.3) <a name='23.3'></a>  **[强制]** 使用帕斯卡式命名构造函数或类。eslint: [`new-cap`](http://eslint.org/docs/rules/new-cap.html) jscs: [`requireCapitalizedConstructors`](http://jscs.info/rule/requireCapitalizedConstructors)


  ```javascript
  // bad
  function user(options) {
    this.name = options.name;
  }

  const bad = new user({
    name: 'nope',
  });

  // good
  class User {
    constructor(options) {
      this.name = options.name;
    }
  }

  const good = new User({
    name: 'yup',
  });
  ```

- [23.4](#23.4) <a name='23.4'></a>  `[modified]`**[建议]** 不要使用下划线 `_` 结尾或开头来命名属性和方法。eslint: [`no-underscore-dangle`](http://eslint.org/docs/rules/no-underscore-dangle.html) jscs: [`disallowDanglingUnderscores`](http://jscs.info/rule/disallowDanglingUnderscores)

  > 为什么？ Javascript对属性或方法而言并没有「私有」的定义。虽然用大多人用下划线开头表示“私有”， 但是实际上这些方法是完全公有的，是公共API的一部分。这种方式会让开发者误认为修改不会影响到它，或者不需要测试。如果你需要一些“私有”定义，那么它们不应该这样显眼。


  ```javascript
  // bad
  this.__firstName__ = 'Panda';
  this.firstName_ = 'Panda';
  this._firstName = 'Panda';

  // good
  this.firstName = 'Panda';
  ```

- [23.5](#23.5) <a name='23.5'></a> **[强制]** 别保存 `this` 的引用。使用箭头函数或 [Function#bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind). jscs: [`disallowNodeTypes`](http://jscs.info/rule/disallowNodeTypes)


  ```javascript
  // bad
  function foo() {
    const self = this;
    return function() {
      console.log(self);
    };
  }

  // bad
  function foo() {
    const that = this;
    return function() {
      console.log(that);
    };
  }

  // good
  function foo() {
    return () => {
      console.log(this);
    };
  }
  ```

**[⬆ 返回目录](#table-of-contents)**

<a name="accessors"></a>
## 存取器


**[⬆ 返回目录](#table-of-contents)**

<a name="events"></a>
## 事件

**[⬆ 返回目录](#table-of-contents)**


## jQuery

**[⬆ 返回目录](#table-of-contents)**

<a name="ecmascript-5-compatibility"></a>
## ECMAScript 5 兼容性

  - [27.1](#27.1) <a name='27.1'></a> 参考 [Kangax](https://twitter.com/kangax/) 的 ES5 [兼容性](http://kangax.github.com/es5-compat-table/).

**[⬆ 返回目录](#table-of-contents)**

<a name="ecmascript-6-styles"></a>
## ECMAScript 6 规范

  - [28.1](#28.1) <a name='28.1'></a> 以下是链接到 ES6 的各个特性的列表。

    1. [Arrow Functions](#arrow-functions)
    1. [Classes](#constructors)
    1. [Object Shorthand](#es6-object-shorthand)
    1. [Object Concise](#es6-object-concise)
    1. [Object Computed Properties](#es6-computed-properties)
    1. [Template Strings](#es6-template-literals)
    1. [Destructuring](#destructuring)
    1. [Default Parameters](#es6-default-parameters)
    1. [Rest](#es6-rest)
    1. [Array Spreads](#es6-array-spreads)
    1. [Let and Const](#references)
    1. [Iterators and Generators](#iterators-and-generators)
    1. [Modules](#modules)


<a name="tc39-proposals"></a>

- [28.2](#tc39-proposals)  `[modified]`**[建议]** 不要使用未到stage 3的 [TC39提案](https://github.com/tc39/proposals) 。

  > 为什么？ [它们还不是终稿](https://tc39.github.io/process-document/), 有可能被改动或废弃。我们使用的是Javascript, 但是提案暂时还不是Javascript。


**[⬆ 返回目录](#table-of-contents)**

<a name="testing"></a>
## 测试

  <a name="testing--yup"></a><a name="28.1"></a>
  - [29.1](#testing--yup) **Yup.**

    ```javascript
    function foo() {
      return true;
    }
    ```

  <a name="testing--for-real"></a><a name="28.2"></a>
  - [29.2](#testing--for-real) **不是强制的，但是建议**:
    - 无论使用哪个测试框架，需要编写测试用例。
    - 尽力去编写小的纯函数，控制可变性。
    - 留意stubs和mocks - 它们会让测试变得脆弱.
    - 在Airbnb我们主要使用 [`mocha`](https://www.npmjs.com/package/mocha) / [`tape`](https://www.npmjs.com/package/tape) 也被用于小的，独立的模块测试。
    - 努力达到100%的测试覆盖率是一个好目标，虽然达到这个目标在某些情况下不太实际。
    - 当你解决完一个bug, 写个回归测试。如果没有回归，这个bug在后面可能引起新的问题。

**[⬆ 返回目录](#table-of-contents)**
