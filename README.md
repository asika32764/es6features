# <img src="http://i.imgur.com/yy1sACZ.png" width="100px"/> ECMAScript 6 Features 中文版

如詞不達意，歡迎提 PR & issue

採用中英混排的方式進行譯制，如不解請查看對應原文

**本文件將與原作者的 [文件](https://github.com/lukehoban/es6features) 保持同步更新，歡迎關注**

## Contributors 翻譯貢獻者
- [Lenville](https://github.com/lenville)
- [CloudiDust](https://github.com/CloudiDust)

## Introduction 簡介
ECMAScript 6, also known as ECMAScript 2015, is the upcoming version of the ECMAScript standard. This standard is targeting ratification in June 2015. ES6 is a significant update to the language, and the first update to the language since ES5 was standardized in 2009. Implementation of these features in major JavaScript engines is [underway now](http://kangax.github.io/es5-compat-table/es6/).

ECMAScript 6(標準官方名稱是 ECMAScript 2015) 是 ECMAScript 的下一代標準，預計將在 2015年6月 正式發佈。ES6 的發布將是這門語言自2009年 ES5 正式發佈以來的首次更新，是一次富有意義的更新。主流Javascript引擎中的這些新特性[正在](http://kangax.github.io/es5-compat-table/es6/)開發中。

See the [draft ES6 standard](https://people.mozilla.org/~jorendorff/es6-draft.html) for full specification of the ECMAScript 6 language.

若希望閱讀 ECMAScript 6 語言的完整規範，請參見[ES6標準草案](https://people.mozilla.org/~jorendorff/es6-draft.html)。

ES6 includes the following new features:

ES6 包含了以下這些新特性：

- [Arrows 箭頭函數](#arrows-箭頭函數)
- [classes 類](#classes-類)
- [enhanced object literals 增強的物件字面量](#enhanced-object-literals-增強的object字面量)
- [template strings 模板字串](#template-strings-模板字串)
- [destructuring 解構](#destructuring-解構)
- [default + rest + spread 預設參數 不定參數 參數展開](#default--rest--spread--預設參數不定參數參數展開)
- [let + const let + const 操作符](#let--const-操作符)
- [iterators + for..of 迭代器 + for...of](#iterators--forof-迭代器--forof-循環)
- [generators 產生器](#generators-產生器)
- [unicode 萬國碼](#unicode-萬國碼)
- [modules 模組](#modules-模組)
- [module loaders 模組加載器](#module-loaders-模組加載器)
- [map + set + weakmap + weakset 資料結構](#map--set--weakmap--weakset-資料結構)
- [proxies 代理](#proxies-代理)
- [symbols 符號](#symbols-符號)
- [subclassable built-ins 可子類化內建物件](#subclassable-built-ins-可子類化的內建物件)
- [promises 物件](#promises-物件)
- [math + number + string + object APIs](#math--number--string--object-apis-擴展)
- [binary and octal literals 二進制和八進制字面量](#binary-and-octal-literals-二進制和八進制字面量)
- [reflect api 反射API](#reflect-api-反射api)
- [tail calls 尾調用](#tail-calls-尾調用)

## ECMAScript 6 Features 特性

### Arrows 箭頭函數
Arrows are a function shorthand using the `=>` syntax. They are syntactically similar to the related feature in C#, Java 8 and CoffeeScript. They support both expression and statement bodies. Unlike functions, arrows share the same lexical this as their surrounding code.

箭頭函數是使用`=>`語法的函數簡寫形式。這在語法上與 C#、Java 8 和 CoffeeScript 的相關特性非常相似。它們同時支持表達式體和語句體。與（普通的）函數所不同的是，箭頭函數和其上下文中的代碼共享同一個具有詞法作用域的`this`。

```JavaScript
// Expression bodies
// 表達式體
var odds = evens.map(v => v + 1);
var nums = evens.map((v, i) => v + i);
var pairs = evens.map(v => ({even: v, odd: v + 1}));

// Statement bodies
// 語句體
nums.forEach(v => {
  if (v % 5 === 0)
    fives.push(v);
});

// Lexical this
// 具有詞法作用域的 this
var bob = {
  _name: "Bob",
  _friends: ["Amy", "Bob", "Cinne", "Dylan", "Ellen"],
  printFriends() {
    this._friends.forEach(f =>
      console.log(this._name + " knows " + f));
  }
}
```

### Classes 類
ES6 classes are a simple sugar over the prototype-based OO pattern. Having a single convenient declarative form makes class patterns easier to use, and encourages interoperability. Classes support prototype-based inheritance, super calls, instance and static methods and constructors.

ES6 的類是在基於原型的面向物件模式之上的簡單語法糖，它有唯一的、便捷的聲明形式，這使得類模式更容易使用，並且鼓勵了互操作性。class定義的類支持基於原型的繼承、[super 調用](http://en.wikipedia.org/wiki/Call_super)、實例和靜態方法以及構造函數。

```JavaScript
class SkinnedMesh extends THREE.Mesh {
  constructor(geometry, materials) {
    super(geometry, materials);

    this.idMatrix = SkinnedMesh.defaultMatrix();
    this.bones = [];
    this.boneMatrices = [];
    //...
  }
  update(camera) {
    //...
    super.update();
  }
  get boneCount() {
    return this.bones.length;
  }
  set matrixType(matrixType) {
    this.idMatrix = SkinnedMesh[matrixType]();
  }
  static defaultMatrix() {
    return new THREE.Matrix4();
  }
}
```

### Enhanced Object Literals 增強的Object字面量
Object literals are extended to support setting the prototype at construction, shorthand for foo: foo assignments, defining methods, making super calls, and computing property names with expressions. Together, these also bring object literals and class declarations closer together, and let object-based design benefit from some of the same conveniences.

物件字面量被擴展以支持以下特性：在建構的時候設置原型、`foo: foo`賦值的簡寫形式、定義方法、進行[super 調用](http://en.wikipedia.org/wiki/Call_super)以及使用表達式計算屬性名稱等。這樣就使得物件字面量和類的聲明的聯繫更加緊密，使得基於物件的設計更加便利。

```JavaScript
var obj = {
    // __proto__
    __proto__: theProtoObj,
    // Shorthand for ‘handler: handler’
    // ‘handler: handler’ 的簡寫形式
    handler,
    // Methods
    toString() {
      // Super calls
      return "d " + super.toString();
    },
    // Computed (dynamic) property names
    // 計算所得的（動態的）屬性名稱
    [ 'prop_' + (() => 42)() ]: 42
};
```

### Template Strings 模板字串
Template strings provide syntactic sugar for constructing strings. This is similar to string interpolation features in Perl, Python and more. Optionally, a tag can be added to allow the string construction to be customized, avoiding injection attacks or constructing higher level data structures from string contents.

模板字串提供構造字串的語法糖，這與Perl、Python等許多語言中的字串插值功能非常相似，你也可以通過添加標籤(tag)來自定義構造字串，避免注入攻擊，或者基於字串建構更高層次的資料結構。

```JavaScript
// Basic literal string creation
// 基礎字串字面量的創建
`In JavaScript '\n' is a line-feed.`

// Multiline strings
// 多行字串
`In JavaScript this is
 not legal.`

 // String interpolation
// 字串插值
var name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`

// Construct an HTTP request prefix is used to interpret the replacements and construction
// 構造一個HTTP請求前綴用來解釋替換和構造，大意就是可以構造一個通用的HTTP prefix並通過賦值生成最終的HTTP請求
GET`http://foo.org/bar?a=${a}&b=${b}
    Content-Type: application/json
    X-Credentials: ${credentials}
    { "foo": ${foo},
      "bar": ${bar}}`(myOnReadyStateChangeHandler);
```

### Destructuring 解構
Destructuring allows binding using pattern matching, with support for matching arrays and objects.  Destructuring is fail-soft, similar to standard object lookup `foo["bar"]`, producing `undefined` values when not found.

解構允許在（變量-值）綁定時使用模式匹配，支持匹配數組和物件，解構支持[故障弱化](http://www.computerhope.com/jargon/f/failsoft.htm)，與標準的物件查詢`foo["bar"]`相似，當查詢無結果時生成`undefined`值。

```JavaScript
// list matching
// 列表匹配
var [a, , b] = [1,2,3];

// object matching
// 物件匹配
var { op: a, lhs: { op: b }, rhs: c }
       = getASTNode()

// object matching shorthand
// binds `op`, `lhs` and `rhs` in scope
// 物件匹配簡寫形式
var {op, lhs, rhs} = getASTNode()

// 上面作者給的示例看得雲裡霧裡的，這裡我再給出一個
function today() { return { d: 2, m: 3, y: 2015 }; }
var { m: month, y: year } = today(); // month = 3, year = 2015

// Can be used in parameter position
// 也可以作為參數使用
function g({name: x}) {
  console.log(x);
}
g({name: 5})

// Fail-soft destructuring
// 故障弱化解構，結果查詢不到時定義為 undefined
var [a] = [];
a === undefined;

// Fail-soft destructuring with defaults
// 具備預設值的故障弱化解構
var [a = 1] = [];
a === 1;
```

### Default + Rest + Spread  預設參數+不定參數+參數展開
Callee-evaluated default parameter values.  Turn an array into consecutive arguments in a function call.  Bind trailing parameters to an array.  Rest replaces the need for `arguments` and addresses common cases more directly.

支持由被調用函數進行求值的參數預設值。
在函數調用時使用`...`運算符，可以將作為參數的數組拆解為連續的多個參數。
在函數定義時使用`...`運算符，則可以將函數尾部的多個參數綁定到一個數組中。
不定參數取代了`arguments`，並可更直接地應用於通常的用例中。

```JavaScript
function f(x, y=12) {
  // y is 12 if not passed (or passed as undefined)
  return x + y;
}
f(3) == 15
```
```JavaScript
function f(x, ...y) {
  // y is an Array
  return x * y.length;
}
f(3, "hello", true) == 6
```
```JavaScript
function f(x, y, z) {
  return x + y + z;
}
// Pass each elem of array as argument
f(...[1,2,3]) == 6
```

### Let + Const 操作符
Block-scoped binding constructs.  `let` is the new `var`.  `const` is single-assignment.  Static restrictions prevent use before assignment.

let 和 const 是具有塊級作用域的綁定用構造，`let` 是新的 `var`，只在塊級作用域內有效，`const` 是[單賦值](http://zh.wikipedia.org/zh-cn/%E9%9D%99%E6%80%81%E5%8D%95%E8%B5%8B%E5%80%BC%E5%BD%A2%E5%BC%8F)，聲明的是塊級作用域的常量。此兩種操作符具有靜態限制，可以防止出現“在賦值之前使用”的錯誤。


```JavaScript
function f() {
  {
    let x;
    {
      // okay, block scoped name
      const x = "sneaky";
      // error, const
      x = "foo";
    }
    // error, already declared in block
    let x = "inner";
  }
}
```

### Iterators + For..Of 迭代器 + For..of 循環
Iterator objects enable custom iteration like CLR IEnumerable or Java Iterable.  Generalize `for..in` to custom iterator-based iteration with `for..of`.  Don’t require realizing an array, enabling lazy design patterns like LINQ.

迭代器物件允許像 [CLI IEnumerable](https://msdn.microsoft.com/zh-cn/library/system.collections.ienumerable(v=vs.110).aspx) 或者 [Java Iterable](http://docs.oracle.com/javase/7/docs/api/java/lang/Iterable.html) 一樣自定義迭代器。將`for..in`轉換為自定義的基於迭代器的形如`for..of`的迭代，不需要實現一個數組，支持像 [LINQ](https://msdn.microsoft.com/zh-cn/library/bb397926.aspx) 一樣的惰性設計模式
```JavaScript
let fibonacci = {
  [Symbol.iterator]() {
    let pre = 0, cur = 1;
    return {
      next() {
        [pre, cur] = [cur, pre + cur];
        return { done: false, value: cur }
      }
    }
  }
}

for (var n of fibonacci) {
  // truncate the sequence at 1000
  if (n > 1000)
    break;
  console.log(n);
}
```

Iteration is based on these duck-typed interfaces (using [TypeScript](http://typescriptlang.org) type syntax for exposition only):

迭代器基於這些[鴨子類型的接口](http://zh.wikipedia.org/zh/%E9%B8%AD%E5%AD%90%E7%B1%BB%E5%9E%8B) (此處使用[TypeScript](http://typescriptlang.org) 的類型語法，僅用於闡述問題)：
```TypeScript
interface IteratorResult {
  done: boolean;
  value: any;
}
interface Iterator {
  next(): IteratorResult;
}
interface Iterable {
  [Symbol.iterator](): Iterator
}
```

### Generators 產生器
Generators simplify iterator-authoring using `function*` and `yield`.  A function declared as function* returns a Generator instance.  Generators are subtypes of iterators which include additional  `next` and `throw`.  These enable values to flow back into the generator, so `yield` is an expression form which returns a value (or throws).

產生器通過使用`function*`和`yield`簡化迭代器的編寫， 形如function*的函數聲明返回一個產生器實例，產生器是迭代器的子類型，迭代器包括附加的`next`和`throw`，這使得值可以回流到產生器中，所以，`yield`是一個返回或拋出值的表達式形式。

Note: Can also be used to enable ‘await’-like async programming, see also ES7 `await` proposal.
注意：也可以被用作類似‘await’一樣的異步編程中，具體細節查看[ES7的`await`提案](http://wiki.ecmascript.org/doku.php?id=strawman:async_functions)

```JavaScript
var fibonacci = {
  [Symbol.iterator]: function*() {
    var pre = 0, cur = 1;
    for (;;) {
      var temp = pre;
      pre = cur;
      cur += temp;
      yield cur;
    }
  }
}

for (var n of fibonacci) {
  // truncate the sequence at 1000
  if (n > 1000)
    break;
  console.log(n);
}
```

The generator interface is (using [TypeScript](http://typescriptlang.org) type syntax for exposition only):
產生器接口如下(此處使用[TypeScript](http://typescriptlang.org) 的類型語法，僅用於闡述問題)：

```TypeScript
interface Generator extends Iterator {
    next(value?: any): IteratorResult;
    throw(exception: any);
}
```

### Unicode 萬國碼
Non-breaking additions to support full Unicode, including new Unicode literal form in strings and new RegExp `u` mode to handle code points, as well as new APIs to process strings at the 21bit code points level.  These additions support building global apps in JavaScript.

> Non-breaking additions to support full Unicode

~~這句看了半天不知道作者想要表達什麼，我就查了下資料，有一種可能是： 增加[不換行空格](http://zh.wikipedia.org/wiki/%E4%B8%8D%E6%8D%A2%E8%A1%8C%E7%A9%BA%E6%A0%BC)的特性以全面支持Unicode，還有一種可能是：~~漸進增強地、非破壞性地全面支持Unicode，也就是說，新加入的特性並不影響老的代碼的使用。我個人比較傾向於第二種解讀。[@sumhat](https://github.com/sumhat)提示說第二種解讀是正確的

（續）字串支持新的Unicode文本形式，也增加了新的正規表示式修飾符`u`來處理碼位，同時，新的API可以在[21bit碼位級別](http://zh.wikipedia.org/wiki/Unicode#.E7.BC.96.E7.A0.81.E6.96.B9.E5.BC.8F)上處理字串，增加這些支持後可以使用 Javascript 建構全球化的應用。
註：關於Unicode推薦閱讀[複雜的Unicode，疑惑的Python](http://www.blogjava.net/pts/archive/2009/07/20/287506.html)

```JavaScript
// same as ES5.1
// 與 ES5.1 相同
"𠮷".length == 2

// new RegExp behaviour, opt-in ‘u’
// 新的正規表示式行為，使用可選的‘u’修飾符
"𠮷".match(/./u)[0].length == 2

// new form
// ES5.1的寫法是`反斜槓+u+碼點`，新的形式可以通過添加一組大括號`{}`來表示超過四字節的碼點
"\u{20BB7}"=="𠮷"=="\uD842\uDFB7"

// new String ops
// 新的字串處理方法
"𠮷".codePointAt(0) == 0x20BB7

// for-of iterates code points
// foo-of 以碼位為單位進行迭代
for(var c of "𠮷") {
  console.log(c);
}
```

### Modules 模組
Language-level support for modules for component definition.  Codifies patterns from popular JavaScript module loaders (AMD, CommonJS). Runtime behaviour defined by a host-defined default loader.  Implicitly async model – no code executes until requested modules are available and processed.

ES6 在語言層面上支持使用模組來進行元件定義，將流行的JavaScript模組加載器（AMD、CommonJS）中的模式固化到了語言中。運行時行為由宿主定義的預設加載器定義，隱式異步模型 - 直到（全部）請求的模組均可用且經處理後，才會執行（當前模組內的）代碼。

```JavaScript
// lib/math.js
export function sum(x, y) {
  return x + y;
}
export var pi = 3.141593;
```
```JavaScript
// app.js
import * as math from "lib/math";
alert("2π = " + math.sum(math.pi, math.pi));
```
```JavaScript
// otherApp.js
import {sum, pi} from "lib/math";
alert("2π = " + sum(pi, pi));
```

Some additional features include `export default` and `export *`:

額外的新特性，包括`export default`以及`export *`：

```JavaScript
// lib/mathplusplus.js
export * from "lib/math";
export var e = 2.71828182846;
export default function(x) {
    return Math.log(x);
}
```
```JavaScript
// app.js
import ln, {pi, e} from "lib/mathplusplus";
alert("2π = " + ln(e)*pi*2);
```

### Module Loaders 模組加載器
Module loaders support:
- Dynamic loading
- State isolation
- Global namespace isolation
- Compilation hooks
- Nested virtualization

模組加載器支持:
- 動態加載
- 狀態隔離
- 全域命名空間隔離
- 編譯鉤子
- [嵌套虛擬化(註: 在模組內調用模組)](http://en.wikipedia.org/wiki/Virtualization#Nested_virtualization)

The default module loader can be configured, and new loaders can be constructed to evaluate and load code in isolated or constrained contexts.

預設的模組加載器是可配置的，也可以建構新的加載器，對在隔離和受限上下文中的代碼進行求值和加載。

```JavaScript
// Dynamic loading – ‘System’ is default loader
// 動態加載 - ‘System’ 是預設的加載器
System.import('lib/math').then(function(m) {
  alert("2π = " + m.sum(m.pi, m.pi));
});

// Create execution sandboxes – new Loaders
// 創建一個執行沙箱- 新的加載器
var loader = new Loader({
  global: fixup(window) // replace ‘console.log’
});
loader.eval("console.log('hello world!');");

// Directly manipulate module cache
// 直接操作模組緩存
System.get('jquery');
System.set('jquery', Module({$: $})); // WARNING: not yet finalized 警告：此部分的設計尚未最終定稿
```

### Map + Set + WeakMap + WeakSet 資料結構
Efficient data structures for common algorithms.  WeakMaps provides leak-free object-key’d side tables.
用於實現常見算法的高效資料結構，WeakMaps提供不會洩露的物件鍵(物件作為鍵名，而且鍵名指向物件)索引表
註：所謂的不會洩露，指的是對應的物件可能會被自動回收，回收後WeakMaps自動移除對應的鍵值對，有助於防止內存洩露

```JavaScript
// Sets
var s = new Set();
s.add("hello").add("goodbye").add("hello");
s.size === 2;
s.has("hello") === true;

// Maps
var m = new Map();
m.set("hello", 42);
m.set(s, 34);
m.get(s) == 34;

// Weak Maps
var wm = new WeakMap();
wm.set(s, { extra: 42 });
wm.size === undefined

// Weak Sets
var ws = new WeakSet();
ws.add({ data: 42 });
// Because the added object has no other references, it will not be held in the set
// 由於所加入的物件沒有其他引用，故在此集合內不會保留之。
```

### Proxies 代理
Proxies enable creation of objects with the full range of behaviors available to host objects.  Can be used for interception, object virtualization, logging/profiling, etc.

代理可以創造一個具備宿主物件全部可用行為的物件。可用於攔截、物件虛擬化、日誌/分析等。

```JavaScript
// Proxying a normal object
// 代理一個普通物件
var target = {};
var handler = {
  get: function (receiver, name) {
    return `Hello, ${name}!`;
  }
};

var p = new Proxy(target, handler);
p.world === 'Hello, world!';
```

```JavaScript
// Proxying a function object
// 代理一個函數物件
var target = function () { return 'I am the target'; };
var handler = {
  apply: function (receiver, ...args) {
    return 'I am the proxy';
  }
};

var p = new Proxy(target, handler);
p() === 'I am the proxy';
```

There are traps available for all of the runtime-level meta-operations:

所有運行時級別的元操作都有對應的陷阱（使得這些操作都可以被代理）：

```JavaScript
var handler =
{
  get:...,
  set:...,
  has:...,
  deleteProperty:...,
  apply:...,
  construct:...,
  getOwnPropertyDescriptor:...,
  defineProperty:...,
  getPrototypeOf:...,
  setPrototypeOf:...,
  enumerate:...,
  ownKeys:...,
  preventExtensions:...,
  isExtensible:...
}
```

### Symbols 符號
Symbols enable access control for object state.  Symbols allow properties to be keyed by either `string` (as in ES5) or `symbol`.  Symbols are a new primitive type. Optional `name` parameter used in debugging - but is not part of identity.  Symbols are unique (like gensym), but not private since they are exposed via reflection features like `Object.getOwnPropertySymbols`.

符號(Symbol) 能夠實現針對物件狀態的訪問控制，允許使用`string`(與ES5相同)或`symbol`作為鍵來訪問屬性。符號是一個新的原語類型，可選的`name`參數可以用於調試——但並不是符號身份的一部分。符號是獨一無二的(如同gensym（所產生的符號）)，但不是私有的，因為它們可以通過類似`Object.getOwnPropertySymbols`的反射特性暴露出來。

```JavaScript
var MyClass = (function() {

  // module scoped symbol
  // 具有模組作用域的符號
  var key = Symbol("key");

  function MyClass(privateData) {
    this[key] = privateData;
  }

  MyClass.prototype = {
    doStuff: function() {
      ... this[key] ...
    }
  };

  return MyClass;
})();

var c = new MyClass("hello")
c["key"] === undefined
```

### [Subclassable Built-ins](http://wiki.ecmascript.org/doku.php?id=strawman:subclassable-builtins) 可子類化的內建物件
In ES6, built-ins like `Array`, `Date` and DOM `Element`s can be subclassed.

在 ES6 中，內建物件，如`Array`、`Date`以及DOM`元素`可以被子類化。

Object construction for a function named `Ctor` now uses two-phases (both virtually dispatched):
- Call `Ctor[@@create]` to allocate the object, installing any special behavior
- Invoke constructor on new instance to initialize

針對名為`Ctor`的函數，其對應的物件的構造現在分為兩個階段（這兩個階段都使用虛分派）：
- 調用`Ctor[@@create]`為物件分配空間，並插入特殊的行為
- 在新實例上調用構造函數來進行初始化

The known `@@create` symbol is available via `Symbol.create`.  Built-ins now expose their `@@create` explicitly.

已知的`@@create`符號可以通過`Symbol.create`來使用，內建物件現在顯式暴露它們的`@@create`。

```JavaScript
// Pseudo-code of Array
// Array偽代碼
class Array {
    constructor(...args) { /* ... */ }
    static [Symbol.create]() {
        // Install special [[DefineOwnProperty]]
        // to magically update 'length'
    }
}

// User code of Array subclass
// Array子類的用戶代碼
class MyArray extends Array {
    constructor(...args) { super(...args); }
}

// Two-phase 'new':
// 1) Call @@create to allocate object
// 2) Invoke constructor on new instance

// 兩階段的'new':
// 1) 調用@@create來為物件分配空間
// 2) 在新實例上調用構造函數
var arr = new MyArray();
arr[1] = 12;
arr.length == 2
```

### Math + Number + String + Object APIs 擴展
Many new library additions, including core Math libraries, Array conversion helpers,  String helpers, and Object.assign for copying.

新加入了許多庫，包括核心數學庫，進行數組轉換的協助函數，字串 helper，以及用來進行拷貝的Object.assign。

```JavaScript
Number.EPSILON
Number.isInteger(Infinity) // false
Number.isNaN("NaN") // false

Math.acosh(3) // 1.762747174039086
Math.hypot(3, 4) // 5
Math.imul(Math.pow(2, 32) - 1, Math.pow(2, 32) - 2) // 2

"abcde".includes("cd") // true
"abc".repeat(3) // "abcabcabc"

Array.from(document.querySelectorAll('*')) // Returns a real Array 返回一個真正的Array
Array.of(1, 2, 3) // Similar to new Array(...), but without special one-arg behavior 與Array(...)類似，但只有一個參數時，並不會有特殊行為。
[0, 0, 0].fill(7, 1) // [0,7,7]
[1, 2, 3].find(x => x == 3) // 3
[1, 2, 3].findIndex(x => x == 2) // 1
[1, 2, 3, 4, 5].copyWithin(3, 0) // [1, 2, 3, 1, 2]
["a", "b", "c"].entries() // iterator [0, "a"], [1,"b"], [2,"c"]
["a", "b", "c"].keys() // iterator 0, 1, 2
["a", "b", "c"].values() // iterator "a", "b", "c"

Object.assign(Point, { origin: new Point(0,0) })
```

### Binary and Octal Literals 二進制和八進制字面量
Two new numeric literal forms are added for binary (`b`) and octal (`o`).

加入對二進制(`b`)和八進制(`o`)字面量的支持。

```JavaScript
0b111110111 === 503 // true
0o767 === 503 // true
```

### Promises 物件
Promises are a library for asynchronous programming.  Promises are a first class representation of a value that may be made available in the future.  Promises are used in many existing JavaScript libraries.

Promise是用來進行異步編程的庫。Promise是對一個“將來可能會變得可用”的值的第一類表示，Promise被使用在現有的許多JavaScript庫中。

```JavaScript
function timeout(duration = 0) {
    return new Promise((resolve, reject) => {
        setTimeout(resolve, duration);
    })
}

var p = timeout(1000).then(() => {
    return timeout(2000);
}).then(() => {
    throw new Error("hmm");
}).catch(err => {
    return Promise.all([timeout(100), timeout(200)]);
})
```

### Reflect API 反射API
Full reflection API exposing the runtime-level meta-operations on objects.  This is effectively the inverse of the Proxy API, and allows making calls corresponding to the same meta-operations as the proxy traps.  Especially useful for implementing proxies.

完整的反射API。此API在物件上暴露了運行時級別的元操作，從效果上來說，這是一個反代理API，並允許調用與代理陷阱中相同的元操作。實現代理非常有用。

```JavaScript
// No sample yet
```

### Tail Calls 尾調用
Calls in tail-position are guaranteed to not grow the stack unboundedly.  Makes recursive algorithms safe in the face of unbounded inputs.

（ES6）保證尾部調用時棧不會無限增長，這使得遞歸算法在面對未作限制的輸入時，能夠安全地執行。

```JavaScript
function factorial(n, acc = 1) {
    'use strict';
    if (n <= 1) return acc;
    return factorial(n - 1, n * acc);
}

// Stack overflow in most implementations today,
// but safe on arbitrary inputs in ES6
// 棧溢出存在於現在絕大多數的實現中，
// 但是在 ES6 中，針對任意的輸入都很安全
factorial(100000)
```

編程語言進化到現階段沉澱了許多成熟方案，例如接口，duck-typed，映射等等，還有許多不明覺厲的概念，每個語言都爭相支持這些語言設計的新方案，所以 ES6 的一部分特性看起來很像 Go
