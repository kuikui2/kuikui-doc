<a name="ZJ5dr"></a>
## 数据类型
<a name="j0kiO"></a>
### 1. JavaScript有哪些数据类型，它们的区别？
**回答：JavaScript有哪些数据类型，它们的区别？**
1. **null、undefined、number、string、Boolean、object、Bigint、Symbol**
2. **Symbol 定义独一无二且值不可变的数据，Bigint 表示比Number更大范围的数字（2^53 - 1）**
3. **分为引用数据类型和基本数据类型**
4. **基本数据类型存储在栈中，引用类型类型储存在堆中**
5. **基本数据类型是值传递，引用数据类型是引用传递**

八种数据类型，Null、Undefined、Boolean、Number、String、Object、Symbol、BigInt。
其中 Symbol 和 BigInt 是ES6 中**新增**的数据类型：
- `Symbol`代表创建后**独一无二**且**值不可变**的数据类型，它主要是为了解决可能出现的**全局变量命名冲突**问题。
- `BigInt` 它提供了一种方法来表示大于 2^53 - 1 的整数。这是用 Number 表示的最大数字。BigInt 可以表示任意大范围的整数。
这些数据可以分为原始数据类型和引用数据类型：
- 引用数据类型object如（对象、数组和函数） ，其它为原始数据类型~~（Undefined、Null、Boolean、Number、String  ~~~~Bigin Symbol~~
两种类型的区别在于存储位置的不同：
- 原始数据类型，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储；
- 引用数据类型，占据空间大、大小不固定。如果存储在栈中，将会**影响程序运行的性能**；所以放入堆中存储，引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。
栈和堆，代码空间堆和栈的概念存在于数据结构和操作系统内存中，在数据结构中：

- 在数据结构中，栈中数据的存取方式为先进后出。
- 堆是一个优先队列，是按优先级来进行排序的，优先级可以按照大小来规定。

在操作系统中，内存被分为栈区和堆区：

- 栈区内存由编译器自动分配释放，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈。
- 堆区内存一般由开发着分配释放，若开发者不释放，程序结束时可能由垃圾回收机制回收。

代码空间：其中的代码空间主要是存储可执行代码的
<a name="REUsv"></a>
### 2. 数据类型检测的方式有哪些
**回答：数据类型检测的方式有哪些**
1. **typeof：不能区分（数组、对象、null）**
2. **instanceof：只能检测对象类型**
3. **constructor：不能检测出null，且 constructor 属性若被修改则判断不准确**
4. **Object.prototype.toString.call()：打印数据的tagName  内置的[[class]]属性**
**typeof： **其中数组、对象、null都会被判断为object，其他判断都正确。```javascript
console.log(typeof 2);               // number
console.log(typeof true);            // boolean
console.log(typeof 'str');           // string
console.log(typeof []);              // object    
console.log(typeof function(){});    // function
console.log(typeof {});              // object
console.log(typeof undefined);       // undefined
console.log(typeof null);            // object
```
**instanceof：instanceof **可以判断对象的类型，但不能判断基本数据类型。其内部运行机制是判断在其原型链中是否存在一个构造函数的原型。
```javascript
console.log(2 instanceof Number);                    // false
console.log(true instanceof Boolean);                // false 
console.log('str' instanceof String);                // false 
 
console.log([] instanceof Array);                    // true
console.log(function(){} instanceof Function);       // true
console.log({} instanceof Object);                   // true
```
**constructor：**访问**constructor **属性获得构造函数从而确定数据类型，但null 会报错， 且对象原型如果被修改则**constructor**判断数据类型将不准确** **```javascript
console.log((2).constructor === Number); // true
console.log((true).constructor === Boolean); // true
console.log(('str').constructor === String); // true
console.log(([]).constructor === Array); // true
console.log((function() {}).constructor === Function); // true
console.log(Symbol("sym").constructor===Symbol);// true
console.log(1n.constructor===BigInt);// true
console.log(({}).constructor === Object); // true
```
改变它的原型
```javascript
function Fn(){};
 
Fn.prototype = new Array();
 
var f = new Fn();
 
console.log(f.constructor===Fn);    // false
console.log(f.constructor===Array); // true
```
**Object.prototype.toString.call()：**使用 Object 对象的原型方法 toString 来判断数据类型：```javascript
Object.prototype.toString({})       // "[object Object]"
Object.prototype.toString.call({})  // 同上结果，加上call也ok
Object.prototype.toString.call(1)    // "[object Number]"
Object.prototype.toString.call('1')  // "[object String]"
Object.prototype.toString.call(true)  // "[object Boolean]"
Object.prototype.toString.call(function(){})  // "[object Function]"
Object.prototype.toString.call(null)   //"[object Null]"
Object.prototype.toString.call(undefined) //"[object Undefined]"
Object.prototype.toString.call(/123/g)    //"[object RegExp]"
Object.prototype.toString.call(new Date()) //"[object Date]"
Object.prototype.toString.call([])       //"[object Array]"
Object.prototype.toString.call(document)  //"[object HTMLDocument]"
Object.prototype.toString.call(window)   //"[object Window]"

// 从上面这段代码可以看出，Object.prototype.toString.call() 
//  可以很好地判断引用类型，甚至可以把 document 和 window 都区分开来。
```
同样是检测对象obj调用toString方法，obj.toString()的结果和Object.prototype.toString.call(obj)的结果不一样，这是为什么？

这是因为toString是Object的原型方法，而Array、function等**类型作为Object的实例，都重写了toString方法**。不同的对象类型调用toString方法时，根据原型链的知识，调用的是对应的重写之后的toString方法（function类型返回内容为函数体的字符串，Array类型返回元素组成的字符串…），而不会去调用Object上原型toString方法（返回对象的具体类型），所以采用obj.toString()不能得到其对象类型，只能将obj转换为字符串类型；因此，在想要得到对象的具体类型时，应该调用Object原型上的toString方法。
**用  typeof  与 Object.prototype.toString.call() 实现一个全局通用的数据类型判断方法 **```javascript
function type(variable) {
  let type = typeof variable;
  if (type !== "object") {
    return type;
  } else {
    type = Object.prototype.toString
      .call(variable)
      .replace(/^\[object (\S+)\]$/, "$1");
    return type[0].toLowerCase() + type.slice(1);
  }
}
```
<a name="Q2PxU"></a>
### 3. JS判断变量是不是数组
**回答：JS判断变量是不是数组**
1. **es6中： Array.isArray(arr)**
2. **instanceof / isPrototypeOf： 判断是否为Array的实例**
3. **通过 constructor 判断**
4. **通过 Object.prototype.toString.call(arr)判断 **
**ES6中 Array.isArray**```css
Array.isArray(arr)
```
**instanceof  判断是否是Array的实例**```css
arr instanceof Array
```
**isPrototypeOf  判断是否是Array的实例**```css
Array.prototype.isPrototypeOf(arr);
//检查调用方法的对象是否存在于参数对象的`原型链`上。
```
**访问constructor 属性获得构造函数从而确定是否为数组类型**```css
arr.constructor.toString().includes("Array")
```
**使用 Object 对象的原型方法 toString 来判断数据类型是否是Array**```css
Object.prototype.toString.call(arr).includes("Array") 
```
<a name="JAAJB"></a>
### 4. typeof null 的结果是什么，为什么？
**回答： typeof null 的结果是什么，为什么？**
1. **输出object ，在 JavaScript 最初的实现中，值以32位存储，前3位表示数据类型标记，其余位表示值**
2. **所有对象前3 位都以000作为标记位，null 代表空、并以32位0表示**
3. **所以用 typeof 检查时会返回 object**
1. **typeof null输出为'object'其实是一个底层的bug，在 JavaScript 最初的实现中，值以**`**32位**`**存储。前3位表示数据类型的标记，其余位则表示值。所有对象，它的前3位都以**`**000**`**作为类型标记位。 **`**null**`** 代表的是空指针，意味着什么都没有并全以**`**0**`**(32个)表示。所以null的前3位也为000，当用typeof检查null时会返回object**
2. 曾有个 ECMAScript 的修复提案使 **typeof null === 'null'**，但没有通过。 
判断null 类型   **===   is   toString  **```javascript
Object.prototype.toString(null)
null === null
Object.is(null, null)
```
更多 
1.  有两种特殊数据类型 
   1.  undefined的值是 (-2)30(一个超出整数范围的数字)； 
   2.  null 的值是机器码 NULL 指针(null 指针的值全是 0) 
```javascript
000: object   - 当前存储的数据指向一个对象。
  1: int      - 当前存储的数据是一个 31 位的有符号整数。
010: double   - 当前存储的数据指向一个双精度的浮点数。
100: string   - 当前存储的数据指向一个字符串。
110: boolean  - 当前存储的数据是布尔值。
```
<a name="RdNcN"></a>
### 5. typeof NaN 的结果是什么？
**回答：typeof NaN 的结果是什么？**
1. **NaN（not a number）指“不是数字”,表示一个“警戒值”，用于指出数字类型中的错误情况**
2. **即“执行数学运算没有成功，这是失败后返回的结果”。**

NaN 指“不是一个数字”（not a number），NaN 是一个“**警戒值**”（有特殊用途的常规值），用于指出数字类型中的错误情况，即“执行数学运算没有成功，这是失败后返回的结果”。
```javascript
typeof NaN; // "number" 且NaN 是一个特殊值，它和自身不相等
```
<a name="GyXv8"></a>
### 6.  null和undefined的区别
**回答：null和undefined的区别**
1. **含义：undefined（no value没有值）它表达了将来的值可能是任意的值，但目前无值，null（no object无对象）表达了将来值可能是对象，但目前无对象**
2. **转为数值：null 转为数值时为0; unaeflned 转为NaN**
3. **保留字：undefined不是保留字（可作变量名），可能会影响对undefined的判断，可通过 void 表达式获得安全的 undefined 值**
4. **类型判断/比较：null 返回object，两者比较===为真，==比较为假**
1. **从含义：**undefined表示**未定义(no value)，**表达了变量将来可能是任意的值，但目前是没有值的，null 代表的含义是**空对象(no object)**，表达的是无对象，。一般变量声明了但还没有定义的时候会返回 undefined，null主要用于赋值给一些可能会返回对象的变量，作为初始化。
2. **保留字：**undefined 在 JavaScript 中不是一个保留字，若使用 undefined 来作为变量名 将非常危险，它会影响对 undefined 值的判断。可以通过 **void 表达式**获得安全的 undefined 值， 如**void 0**。
3. **类型判断/比较：**当对这两种类型使用 typeof 进行判断时，Null 类型化会返回 “object”，这是一个历史遗留的问题。当使用双等号对两种类型的值进行比较时会返回 true，使用三个等号时会返回 false。
4. **转为数值：**null 转为数值时为0; unaeflned 转为数值时为NaN
<a name="C9YIV"></a>
### 7.  如何获取安全的 undefined 值？

- undefined 在 JavaScript 中不是一个保留字，所以可以被当作变量来使用和赋值，但是这样**会影响 undefined 的正常判断**。**表达式 void ___ 没有返回值**，因此返回结果是 undefined。void 并不改变表达式的结果，只是让表达式不返回值。因此可以用 void 0 来获得 undefined。
<a name="Uhbcj"></a>
### 8. isNaN 和 Number.isNaN 函数的区别？
**回答： isNaN 和 Number.isNaN 函数的区别？**
1. **isNaN 会尝试先把参数传为数值，若不能被转换为数值将会返回 true，因此非数字传入也会返回true，影响 NaN 的判断**
2. **Number.isNaN 会先判断是否是数字，再进行判断是否是 NaN**
- 函数 isNaN 接收参数后，会尝试将这个参数转换为数值，isNaN**任何不能被转换为数值的的值都会返回 true**，因此非数字值传入也会返回 true ，会影响 NaN 的判断。
   - isNaN 接收参数-->参数转换为数值-->失败则-->也返回**true（**影响 NaN 的判断**）**
- 函数 Number.isNaN 会首先判断传入参数是否为数字，如果是数字再继续判断是否为 NaN ，不会进行数据类型的转换，这种方法对于 NaN 的判断更为准确。
   - 判断是否为数字-->判断是否为 NaN (不会数据类型转换)-->是则true
<a name="vrLW2"></a>
### 9. intanceof 操作符的实现原理及实现
**instanceof   判断对象的原型链中是否有 待测构造函数的 prototype 属性**
实现```javascript
function myInstanceof(sub, sup) {
    // 判断是不是基本类型
    let type = ['number', 'string', 'boolean', 'bigint', 'symbol']
    if ((type.includes(typeof sub)) || sub === null) {
        return false
    }

    // 获取对象的原型
    let proto = Object.getPrototypeOf(sub)
    // 获取构造函数的 prototype 对象
    const prototype = sup.prototype

    // 判断构造函数的 prototype 对象是否在对象的原型链上
    while (true) {
        if (!proto) return false
        if (proto === prototype) return true
        proto = Object.getPrototypeOf(proto)
    }
}
```
<a name="rmroR"></a>
### 10. JavaScript 中如何进行隐式类型转换？
回答： JavaScript 中如何进行隐式类型转换？
1. 可能转化的类型有 boolean、number、string
2. **~~运行环境对数据类型隐式转换的影响~~**~~，很多内置函数期望传入的参数的数据类型是固定的，但是如果我们传入非期待数据类型会发生数据类型的隐式转换。这就是环境运行环境对数据类型转换的影响。~~
3. **复杂对象如何转换为简单值**
   1. `**ToPrimitive**`方法，这是 JavaScript 中每个值隐含的自带的方法，用来将值转换为基本类型值 ** ToPrimitive(obj,type)**
   2. **（1）当**`type`**为**`number`**时规则如下：**
         - 调用`obj`的`valueOf`方法，如果为原始值，则返回，否则下一步；
         - 调用`obj`的`toString`方法，如果返回的还是复杂值，则抛出异常
         - 抛出`TypeError` 异常。

**（2）当**`type`**为**`string`**时规则如下：**

      - 调用`obj`的`toString`方法，如果为原始值，则返回，否则下一步；
      - 调用`obj`的`valueOf`方法，如果返回的还是复杂值，则抛出异常
      - 抛出`TypeError` 异常。

可以看出两者的主要区别在于调用`toString`和`valueOf`的先后顺序。默认情况下：

      - 如果对象为 Date 对象，则`type`默认为`string`；
      - 其他情况下，`type`默认为`number`。
4. **而JavaScript中的隐式类型转换主要发生在+、-、*、/以及==、>、<这些运算符之间。而这些运算符只能操作基本类型值，所以在进行这些运算前的第一步就是将两边的值用ToPrimitive转换成基本类型，再进行操作。**
   1. **+操作符+操作符的两边有至少一个string类型变量时，两边的变量都会被隐式转换为字符串;其他情况下两边的变量都会被转换为数字。**
   2. **-、*、/  都会被转换为数字。**
   3. **对于==操作符  操作符两边的值都尽量转成number:**
   4. **对于<和>比较符：如果两边都是字符串，则比较字母表顺序，其他情况下，转换为数字再比较**
   5. **而对象会被ToPrimitive转换为基本类型再进行转换**

**js中的数据类型隐式转换的三种情况**

1. 转换为boolean类型  (数据在 **逻辑判断** 和 **逻辑运算** 之中会隐式转换为boolean类型)
2. 转换为number类型
3. 转换为string类型

**运行环境对数据类型隐式转换的影响**<br />很多内置函数期望传入的参数的数据类型是固定的，如:alert(value)方法，它期望传入的value值是一个string类型，但是如果我们传入的是number类型或者object类型等非string类型的数据的时候，就会发生数据类型的隐式转换。这就是环境运行环境对数据类型转换的影响。<br />**复杂对象如何转换为简单值**<br />一个复杂对象在转为基础类型的时候会调用ToPrimitive(hint)方法来指定其目标类型。如果传入的hint值为number,那么就先调用对象的valueOf()方法，调用完valueOf()方法后，如果返回的是原始值，则结束ToPrimitive操作，如果返回的不是原始值，则继续调用对象的toString()方法，调用完toString()方法之后如果返回的是一个原始值，则结束ToPrimitive操作，如果返回的还是复杂值，则抛出异常。如果传入的hint值为string，则先调用toString()方法，再调用valueOf()方法，其余的过程一样。<br />**那么复杂对象是以什么标准来判断 ToPrimitive(hint) 操作传入的hint值到底是number还是string 呢？**<br />如果运行环境非常明确的需要将一个复杂对象转换为数字则传入number如 Number(value) 和 +value 则传入number<br />如果运行环境非常明确的需要将一个复杂对象转换为字符串则传入string如String(value) 和 alert(value) 则传入strin<br />如果是用+号连接两个操作数，操作数在确定其中只要有一个为字符串的时候另外一个操作数会转为字符串，ToPrimitive()会传入string，但是如果两个操作数都不能确定是字符串的时候则默认传入number(Date对象是一个例外，它会默认传入string)进行数据类型转换。<br />**总结**

- 类型相同
   - 基本类型，直接比较值
   - 引用类型比较指针
- 类型不同，尝试转成number类型，
   - 先调用valueOf()转成number
   - 不行就再用toString()方法转成string
- null、NaN、undefined单独一套规则

首先要介绍`**ToPrimitive**`方法，这是 JavaScript 中每个值隐含的自带的方法，用来将值 （无论是基本类型值还是对象）转换为基本类型值。如果值为基本类型，则直接返回值本身；如果值为对象，其看起来大概是这样：
```javascript
/**
* @obj 需要转换的对象
* @type 期望的结果类型
*/
ToPrimitive(obj,type)
```
`type`的值为`number`或者`string`。<br />**（1）当**`type`**为**`number`**时规则如下：**

- 调用`obj`的`valueOf`方法，如果为原始值，则返回，否则下一步；
- 调用`obj`的`toString`方法，如果返回的还是复杂值，则抛出异常
- 抛出`TypeError` 异常。

**（2）当**`type`**为**`string`**时规则如下：**

- 调用`obj`的`toString`方法，如果为原始值，则返回，否则下一步；
- 调用`obj`的`valueOf`方法，如果返回的还是复杂值，则抛出异常
- 抛出`TypeError` 异常。

可以看出两者的主要区别在于调用`toString`和`valueOf`的先后顺序。默认情况下：

- 如果对象为 Date 对象，则`type`默认为`string`；
- 其他情况下，`type`默认为`number`。
总结上面的规则，对于 Date 以外的对象，转换为基本类型的大概规则可以概括为一个函数：```javascript
var objToNumber = value => Number(value.valueOf().toString())
objToNumber([]) === 0
objToNumber({}) === NaN
```

而 JavaScript 中的隐式类型转换主要发生在`+、-、*、/`以及`==、>、<`这些运算符之间。而这些运算符只能操作基本类型值，所以在进行这些运算前的第一步就是将两边的值用`ToPrimitive`转换成基本类型，再进行操作。<br />以下是基本类型的值在不同操作符的情况下隐式转换的规则 （对于对象，其会被`ToPrimitive`转换成基本类型，所以最终还是要应用基本类型转换规则）：
**+操作符 **+ 操作符的两边有至少一个string类型变量时，两边的变量都会被隐式转换为字符串；其他情况下两边的变量都会被转换为数字。```javascript
1 + '23' // '123'
 1 + false // 1 
 1 + Symbol() // Uncaught TypeError: Cannot convert a Symbol value to a number
 '1' + false // '1false'
 false + true // 1
```
**-、 -、/ **操作符 NaN也是一个数字```javascript
1 * '23' // 23
 1 * false // 0
 1 / 'aa' // NaN
```
对于**==**操作符 操作符两边的值都尽量转成  number```javascript
3 == true // false, 3 转为number为3，true转为number为1
'0' == false //true, '0'转为number为0，false转为number为0
'0' == 0 // '0'转为number为0
```
对于**<和>**比较符  如果两边都是字符串，则比较字母表顺序、其他情况下，转换为数字再比较：```javascript
'ca' < 'bd' // false
'a' < 'b' // true
```
```javascript
'12' < 13 // true
false > -1 // true
```
以上说的是基本类型的隐式转换，而对象会被**ToPrimitive**转换为基本类型再进行转换：```javascript
var a = {}
a > 2 // false
```
其对比过程如下：
```javascript
a.valueOf() // {}, 上面提到过，ToPrimitive默认type为number，所以先valueOf，结果还是个对象，下一步
a.toString() // "[object Object]"，现在是一个字符串了
Number(a.toString()) // NaN，根据上面 < 和 > 操作符的规则，要转换成数字
NaN > 2 //false，得出比较结果
```

又比如：
```javascript
var a = {name:'Jack'}
var b = {age: 18}
a + b // "[object Object][object Object]"
```
运算过程如下：
```javascript
a.valueOf() // {}，上面提到过，ToPrimitive默认type为number，所以先valueOf，结果还是个对象，下一步
a.toString() // "[object Object]"
b.valueOf() // 同理
b.toString() // "[object Object]"
a + b // "[object Object][object Object]"
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/28203632/1665291880427-dc1a6705-5f06-49c9-88bc-58c6128ca5a1.png#averageHue=%23f8f7f6&clientId=u8a736936-0f40-4&errorMessage=unknown%20error&from=paste&height=822&id=ufeeff4b7&originHeight=822&originWidth=976&originalType=binary&ratio=1&rotation=0&showTitle=false&size=69565&status=error&style=stroke&taskId=ued304463-c3e7-4ef0-b50d-0955b6c4465&title=&width=976)
<a name="xnUQK"></a>
### 11. == 操作符的隐式类型转换规则？
**回答：== 操作符的隐式类型转换规则？**
1. **判断类型是否相同**
2. **是否两者为：null和undefined**
3. **是否两者为：string 和 number ，是 string->number**
4. **是否其中一方为：Boolean ,boolean->number**
5. **是否其中一方为：objec 且另一方为 **`**string**`**、**`**number**`** 或者 **`**symbol**`**则object->基本**
   1. **对象在转换类型的时候，会调用内置的 [[ToPrimitive]] 函数，对于该函数来说，算法逻辑一般来说如下**
   2. **调用 x.valueOf()，如果转换为基础类型，就返回转换的值，否则执行 x.toString**
   3. **调用 x.toString()，如果转换为基础类型，就返回转换的值**

对于 `==` 来说，如果对比双方的类型**不一样**，就会进行**类型转换**。假如对比 `x` 和 `y` 是否相同，就会进行如下判断流程：

1. 首先会判断两者类型是否**相同，**相同的话就比较两者的大小；类型不相同的话，就会进行类型转换；
2. 会先判断是否在对比 `null` 和 `undefined`，是的话就会返回 `true`  
3. 判断两者类型是否为 `string` 和 `number`，是的话就会将字符串转换为 `number`  toNumber(y)
4. 判断其中一方是否为 `boolean`，是的话就会把 `boolean` 转为 `number` 再进行判断  toNumber(y）
5. 判断其中一方是否为 `object` 且另一方为 `string`、`number` 或者 `symbol`，是的话就会把 `object` 转为原始类型再进行判断

其流程图如下：

![](https://cdn.nlark.com/yuque/0/2021/png/1500604/1615475217180-eabe8060-a66a-425d-ad4c-37c3ca638a68.png#averageHue=%23fdfdfd&id=ZSeX8&originHeight=426&originWidth=1005&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

- 对象在转换类型的时候，会调用内置的 [[ToPrimitive]] 函数，对于该函数来说，算法逻辑一般来说如下
- 如果Symbol.toPrimitive()方法，优先调用再返回；
- 调用 x.valueOf()，如果转换为基础类型，就返回转换的值
- 调用 x.toString()，如果转换为基础类型，就返回转换的值
- 均不返回基本类型值，会产生 TypeError 错误。

当然你也可以重写 Symbol.toPrimitive，该方法在转原始类型时调用优先级最高。toPrimitive  要返回基本类型（除了symbol）symbol 转化为数字会出错 ```javascript
let a = {
    valueOf() {
        return 0
    },
    toString() {
        return '1'
    },
    [Symbol.toPrimitive]() {
        return 2
    }
}
1 + a // => 3

```

- 如果是对象，并且部署了 [Symbol.toPrimitive] ，那么调用此方法，否则调用对象的 valueOf() 方法，然后依据前面的规则转换返回的值；如果转换的结果是 NaN （如果返回的是引用类型），则调用对象的 toString() 方法，再次依照前面的顺序转换返回对应的值。
<a name="iwYfA"></a>
### 12. == 和 === 有什么不同？
**回答：== 和 === 有什么不同？**
1. **== 类型不同会进行类型转化再比较，===类型不同直接返回false，只比较值 **

===它是值之间的比较,而:==不仅仅是值之间的比较,也是类型之间的比较<br />如果一个是对象，另一个是数值或字符串，把对象转换成基础类型的值再比较。对象转换成基础类型，利用它的toString或者valueOf方法。 js核心内置类，会尝试valueOf先于toString;<br />valueOf()方法返回 对象的原始值。

toPrimitive首先在对象中使用valueOf方法，然后使用toString方法来获取该对象的原始值。
<a name="t9eLP"></a>
### 13.  其他值到字符串的转换规则？
**回答：其他值到字符串的转换规则？**
1. **null、undefined、boolean、number（极大和极小会使用指数表示），直接原样转**
2. **Bigint 类型的值直接转换不要 n**
3. **Symbol 类型的值直接转换，只能显示不能隐式转接**
4. **如果对象有自己的 toString() ，会调用 toString 转化，否则调用 Object.prototype.toString()返回内部属性 [[Class]] 的值**
   1. 多数情况下，对象的内部[[class]]属性和创建该对象的内建原生构造函数相对应，不过也不总是这样。虽然Null()和Undefined()这样的原生构造函数并不存在，但是内部[[class]]属性仍然是“Null”和“Undefined”。
- ~~Null 和 Undefined 类型 ，null 转换为 "null"，undefined 转换为 "undefined"，~~
- ~~Boolean 类型，true 转换为 "true"，false 转换为 "false"。~~
- Number 类型的值直接转换，不过那些极小和极大的数字会使用指数形式。
- Bigint 类型的值直接转换不要 n
- Symbol 类型的值直接转换，
   - 但是只允许显式（String(sym)）强制类型转换，
   - 使用隐式（"" + Symbol("syl")）强制类型转换会产生错误。
- 对普通对象来说，除非自行定义 toString() 方法，否则会调用 toString()（Object.prototype.toString()）来返回内部属性 [[Class]] 的值，如"[object Object]"。如果对象有自己的 toString() 方法，字符串化时就会调用该方法并使用其返回值。
   - 定制[[Class]]
定制[[Class]]```javascript
class Person {
    get [Symbol.toStringTag]() {
        return "person"
    }
}

function Person() { }
Person.prototype[Symbol.toStringTag] = 'Person'    

let p = new Person();
console.log(p + "");// [object Person]
```
多数情况下，对象的内部**[[class]]**属性和创建该对象的内建原生构造函数相对应，不过也不总是这样。虽然Null()和Undefined()这样的原生构造函数并不存在，但是内部[[class]]属性仍然是“Null”和“Undefined”。
<a name="kGak6"></a>
### 14. 其他值到数字值的转换规则？
**回答：其他值到数字值的转换规则？**
1. **Undefined 类型的值转换为 NaN。**
2. **Null 类型的值转换为 0。**
3. **Boolean 类型的值，true 转换为 1，false 转换为 0。**
4. **String 若含非数字值则转换为 NaN，空字符串( ' ' '' )为 0。**
5. **Symbol 类型的值不能转换为数字，会报错。**
6. **Bigint  转数字不要 n 、极小/大的数字会使用指数形式**
7. **对象转化为NaN ToPrimitive  valueOf  toString**
- ~~Undefined 类型的值转换为 NaN。~~
- ~~Null 类型的值转换为 0。~~
- ~~Boolean 类型的值，true 转换为 1，false 转换为 0。~~
- String 类型的值转换如同使用 Number() 函数进行转换，如果包含非数字值则转换为 NaN，空字符串( ' ' '' )为 0。
- Symbol 类型的值不能转换为数字，会报错。
- Bigint  转数字不要 n 、极小和极大的数字会使用指数形式
- 对象（包括数组）会首先被转换为相应的基本类型值（ToPrimitive ，valueOf，toString），如果返回的是非数字的基本类型值，则再遵循以下规则将其强制转换为数字。
对象在转换类型规则
- 对象在转换类型的时候，会调用内置的 [[ToPrimitive]] 函数，对于该函数来说，算法逻辑一般来说如下
- 如果已经是原始类型了，那就不需要转换了
- 调用 x.valueOf()，如果转换为基础类型，就返回转换的值
- 调用 x.toString()，如果转换为基础类型，就返回转换的值
- 均不返回基本类型值，会产生 TypeError 错误。
<a name="TrtPw"></a>
### 15. 其他值到布尔类型的值的转换规则？
**回答：其他值到布尔类型的值的转换规则？**
1. **undefined、null、（+0、-0，0n） 、 NaN、""，都将转化为 false**
2. **从逻辑上说，假值列表以外的都应该是真值。**

以下这些是假值：<br />• undefined<br />• null<br />• +0、-0，0n 和 NaN<br />• ""<br />假值的布尔强制类型转换结果为 false。从逻辑上说，假值列表以外的都应该是真值。
<a name="KkDqJ"></a>
### 16. || 和 && 操作符的返回值？

1. **先对第一个操作数进行条件判断**
2. **如果不是布尔类型就先强制转换为布尔类型，再执行条件判断。**
3. **短路判断，如果第一个操作数已经能确定操作符结果，则不会执行第二个操作数**
4. **返回的是它们其中一个操作数的值，而非条件判断的结果**

|| 和 && 首先会对第一个操作数执行条件判断，如果其不是布尔值就先强制转换为布尔类型，然后再执行条件判断。

- 对于 || 来说，如果条件判断结果为 true 就返回第一个操作数的值，如果为 false 就返回第二个操作数的值。
- && 则相反，如果条件判断结果为 true 就返回第二个操作数的值，如果为 false 就返回第一个操作数的值。

|| 和 && 返回它们其中一个操作数的值，而非条件判断的结果
<a name="gQLmt"></a>
### 17. Object.is() 与比较操作符 “==、===” 的区别？

1. **== 两者类型不同会进行强制类型转化，再比较**
2. **=== 不会做强制类型准换，直接比较**
3. **Object.is(+0, -0) false 其他相同都为 true，两个 NaN 是相等**
- 使用双等号（==）进行相等判断时，如果两边的类型不一致，则会进行强制类型转化后再进行比较。
- 使用三等号（===）进行相等判断时，如果两边的类型不一致时，不会做强制类型准换，直接返回 false。
- 使用 Object.is 来进行相等判断时，一般情况下和三等号的判断相同，它处理了一些特殊的情况，
   - 比如 两个 NaN 是相等的。
   - Object.is(+0, -0) false     ( +0 === -0)  true
<a name="ESU7f"></a>
### 18. 什么是 JavaScript 中的包装类型？

1. **基本类型是没有属性和方法的，但是为了便于操作基本类型的值，在调用基本类型的属性或方法时 JavaScript 会在后台隐式地将基本类型的值转换为对象，调用完成后，销毁该对象**
2. **JavaScript也可以使用 Object('abc') 函数显式地将基本类型转换为包装类型**
3. **也可以使用 valueOf 方法将包装类型倒转成基本类型**

在 JavaScript 中，基本类型是没有属性和方法的，但是为了便于操作基本类型的值，在调用基本类型的属性或方法时 JavaScript 会在后台隐式地将基本类型的值转换为对象，<br />过程

- 第一步: 创建Object类实例。注意为什么不是String ？ 由于Symbol和BigInt的出现，对它们调用new都会报错，目前ES6规范也不建议用new来创建基本类型的包装类。
- 第二步: 调用实例方法。
- 第三步: 执行完方法立即销毁这个实例。
在访问'**abc'.length**时，JavaScript 将'abc'在后台转换成**String('abc')**，然后再访问其length属性。```javascript
const abc = "abc";
abc.length; // 3
abc.toUpperCase(); // "ABC"
```
JavaScript也可以使用 **Object** 函数显式地将基本类型转换为包装类型：```javascript
var a = 'abc'
Object(a) // String {"abc"}
Object("sfas") instanceof String//true
```
也可以使用 valueOf 方法将包装类型倒转成基本类型：```javascript
var a = 'abc'
var b = Object(a)
var c = b.valueOf() // 'abc'
```
<a name="gjWhr"></a>
### 19. 什么0.1+0.2 ! == 0.3，如何让其相等

1. **计算机是通过二进制的方式存储数据的，0.1和0.2的二进制都是无限循环的数，计算机计算0.1+0.2的时候，实际上是计算的两个数的二进制和。然后再转化为十进制，就是：**`**0.30000000000000004**`
2.  ** Number.EPSILON设置一个最小误差范围，在该范围内视为相等**
3. **  可以将其转换为整数后在进行运算,运算后再转为对应的小数**
1. 计算机是通过二进制的方式存储数据的，0.1和0.2的二进制都是无限循环的数，计算机计算0.1+0.2的时候，实际上是计算的两个数的二进制和。然后再转化为十进制，就是：`0.30000000000000004`
   1. `Number.EPSILON`设置一个最小误差范围
   2. 可以将其转换为整数后在进行运算,运算后再转为对应的小数
```javascript
const isEqual = (a, b) => Math.abs(a - b) < Number.EPSILON
console.log(isEqual(0.1 + 0.2, 0.3)); // true
```
<a name="orhDA"></a>
### 20. `+` 操作符什么时候用于字符串的拼接？

1. **如果 + 的其中一个操作数是字符串**
2. **或者通过调用 ToPrimitive 最终得到字符串，例如**({} + 1)

根据 ES5 规范，如果某个操作数是字符串或者能够通过以下步骤转换为字符串的话，+ 将进行拼接操作。如果其中一个操作数是对象({} + 1)（包括数组），则首先对其调用 ToPrimitive 抽象操作，该抽象操作再调用 [[DefaultValue]]，以数字作为上下文。如果不能转换为字符串，则会将其转换为数字类型来进行计算。

简单来说就是，如果 + 的其中一个操作数是字符串（或者通过调用 ToPrimitive 最终得到字符串），则执行字符串拼接，否则执行数字加法。

那么对于除了加法的运算符来说，只要其中一方是数字，那么另一方就会被转为数字。
<a name="UaQBN"></a>
### 21. 为什么会有 BigInt 的提案？

1. **Number.MAX_SAFE_INTEGER表示最⼤安全数字（ 2^53 - 1 ）⼀旦超过这个范围则会出现精度丢失**
2. **这在⼤数计算的时候不得不依靠⼀些第三⽅库进⾏解决**
3. **因此官⽅提出了BigInt来解决此问题，bigint 能表示更大范围的数字**

JavaScript中**Number.MAX_SAFE_INTEGER**表示最⼤安全数字，计算结果是9007199254740991（ 2^53 - 1 ），即在这个数范围内不会出现精度丢失（⼩数除外）。但是⼀旦超过这个范围，js就会出现计算不准确的情况，这在⼤数计算的时候不得不依靠⼀些第三⽅库进⾏解决，因此官⽅提出了BigInt来解决此问题。
<a name="HCgOr"></a>
### 22. object.assign和扩展运算符是深拷贝还是浅拷贝，两者区别

1. **两者都是浅拷贝。都会复制ES6的 symbols 属性。且不复制继承的属性**
2. **Object.assign()参数一作为目标对象，后面的所有参数作为源对象。然后把所有的源对象合并到目标对象中。它会修改了一个对象，因此会触发 ES6 setter。**
3. **扩展操作符（…）使用它时，数组或对象中的每一个值都会被拷贝到一个新的数组或对象中。**

两者都是浅拷贝。都会复制ES6的 symbols 属性。**不复制继承的属性   ~~或类的属性~~**~~，~~

- Object.assign()方法接收的第一个参数作为目标对象，后面的所有参数作为源对象。然后把所有的源对象合并到目标对象中。它会修改了一个对象，因此会触发 ES6 setter。
- 扩展操作符（…）使用它时，数组或对象中的每一个值都会被拷贝到一个新的数组或对象中。 
<a name="vmr88"></a>
### 23.JS 整数是怎么表示的

1. **通过 Number 类型来表示整数，通过 64 位来表示一个数字，**
2. **（1 + 11 + 52）（符号位 + 指数位 + 表示尾数（即有效数字）**
3. **最大安全数字是 Math.pow(2, 53) - 1**

通过 Number 类型来表示，遵循 IEEE754 标准，通过 64 位来表示一个数字，（1 + 11 + 52），最大安全数字是 Math.pow(2, 53) - 1，对于 16 位十进制。（符号位 + 指数位 + 小数部分（即有效数字））
<a name="hzH83"></a>
## 对象相关
<a name="b82Am"></a>
### [1. 对象拷贝](https://github.com/yygmind/blog/issues/31)

1. **创建一个新的对象，拷贝源对象属性，**
2. **如果对象属性是基本的数据类型，复制的就是基本类型的值，但如果属性是引用数据类型，复制的就是内存中的地址，这种情况修改该引用属性会互相影响**
3. **实现方法**
   1. **object.assign**
   2. **扩展运算符方式**
   3. **遍历**
      1. **Reflect.ownKeys 加 forof**
      2. **Object.getOwnPropertyNames 加 forEach 加 getOwnPropertyDescriptor**
      3. **forin 原先**

被拷贝对象
```javascript
var obj = {
	name: 'mfk',
	age: 21,
	haha: [123, {
		a: 1,
		b: [3, 4, 5]
	}]
}
```
<a name="sx96G"></a>
#### 浅拷贝
自己创建一个新的对象，来接受需要重新复制或引用的对象值。如果对象属性是基本的数据类型，复制的就是基本类型的值给新对象；但如果属性是引用数据类型，复制的就是内存中的地址，如果其中一个对象改变了这个内存中的地址，肯定会影响到另一个对象
**object.assign**```javascript
Object.assign(target, obj);
//它不会拷贝对象的继承属性；
//它不会拷贝对象的不可枚举的属性；
//可以拷贝 Symbol 类型的属性。
```
**扩展运算符方式**```javascript
let obj2 = {...obj}
//扩展运算符 和 object.assign 有同样的缺陷，也就是实现的浅拷贝的功能差不多
```
**手工实现一个浅拷贝**```javascript
function shallowClone(target) {
    if (target instanceof Object && (typeof target === 'object')) {
        const cloneTarget = Array.isArray(target) ? [] : {}
        // 方式一
        Object.getOwnPropertyNames(target).forEach(key => {
            cloneTarget[key] = target[key]
        })
        // 方式二
        // for (const key in target) {
        //     if (Object.hasOwnProperty.call(target, key)) {
        //         cloneTarget[key] = target[key]
        //     }
        // }
        // 方式三
        // Object.getOwnPropertyNames(target).forEach(key => {
        //     Object.defineProperty(cloneTarget, key, Object.getOwnPropertyDescriptor(target, key))
        // })
        return cloneTarget
    } else {
        throw Error('传入数组或对象')
    }
}
```
```javascript
//测试
console.log(obj);
console.log(newObj);
newObj.haha[1].b[0] = 111
console.log(obj);
```
<a name="PAKjf"></a>
#### [深拷贝](https://interview2.poetries.top/docs/excellent-docs/3-JS%E6%A8%A1%E5%9D%97.html#_15-%E6%B7%B1%E6%B5%85%E6%8B%B7%E8%B4%9D)

1. **创建了一个新的对象**
2. **递归拷贝对象属性，如果是基本类型直接赋值，对于引用数据类型，会在堆内存中开辟了一块新内存空间，并将原有的对象完全复制过来存放。**
3. JSON.stringify
   1. **会忽略 undefined**
   2. 对象中含有 NaN、Infinity 以及 -Infinity，JSON 序列化的结果会变成 null
   3. **不能序列化函数**
   4. **无法拷贝对象的原型链**
   5. 拷贝 RegExp 引用类型会变成空对象、拷贝 Date 引用类型会变成字符串
   6. 无法拷贝不可枚举（Error）和symbol的属性
   7. **不能解决循环引用的对象**，即对象成环 (obj[key] = obj)。(栈溢出)
4.   **递归实现**
   1. **遍历当前对象属性，如果当中属性仍为对象则进行递归，否则直接赋值给新对象的属性**
   2. **Date、RegExp Map Set 类型**
   3. **循环引用**
   4. **拷贝原型链**
   5. **拷贝不可枚举属性和Symbol**
**JSON.stringify**```javascript
var newObj = JSON.parse(JSON.stringify(obj));
```
**局限性的**：

1. 会忽略 undefined      
2. 对象中含有 NaN、Infinity 以及 -Infinity，JSON 序列化的结果会变成 null
3. 不能序列化函数
4. 无法拷贝对象的原型链  
5. 拷贝 RegExp 引用类型会变成空对象、拷贝 Date 引用类型会变成字符串
6. 无法拷贝不可枚举（Error）和symbol的属性
7. 不能解决循环引用的对象，即对象成环 (obj[key] = obj)。(栈溢出)
**基础版（手写递归实现）**```javascript
function deepClone(target) {
    if (typeof obj !== 'object' || target == null) return target
    var newObj = Array.isArray(target) ? [] : {}
    for (const key in target) {
        console.log(key + 1)
        newObj[key] = arguments.callee(target[key])//特点：等待下一次返回值
    }
    return newObj
}
```
```javascript
function deepCopy(obj, resObj) {
	var newObj = resObj || {}
	for (const key in obj) {
		if (typeof obj[key] === 'object' && obj[key] != null) {
			newObj[key] = obj[key] instanceof Array ? [] : {}
			arguments.callee(obj[key], newObj[key])//特点：把索引传下去
		} else {
			newObj[key] = obj[key]
		}
	}
	return newObj
}
```
测试
```javascript
//测试
obj = {
  haha: [0, {
      a: 0,
      b: [1, 2, 3, 4]
  }]
}

console.log(obj);
var newObj = deepCopy(obj)
console.log(newObj);
newObj.haha[1].b[0] = 111
console.log(obj);
```
**改进版（改进后递归实现）**```javascript
function deepClone(obj, hash = new WeakMap()) {
    if ([Date, RegExp, Map, Set].includes(obj.constructor)) 
      return new obj.constructor(obj)

    if (hash.has(obj)) return hash.get(obj)

    const cloneDesc = Object.getOwnPropertyDescriptors(obj)
    // 此方式创建对象,数组情况会导致Array.isArray判断类型失效，但instanceof正常
    const cloneObj = Object.create(Object.getPrototypeOf(obj), cloneDesc)

    hash.set(obj, cloneObj)

    for (const key of Reflect.ownKeys(obj)) {
        cloneObj[key] = (typeof obj[key] === 'object' && obj[key] !== null) ?
          deepClone(obj[key], hash) : obj[key]
    }

    return cloneObj
}
```
测试对象
```javascript
function Obj() {
    this.func = function () { alert(1) };
    this.obj = { a: '对象' };
    this.arr = ['数组', 2, 3];
    this.und = undefined;
    this.NaN = NaN;
    this.infinity = Infinity;
    this[Symbol('Symbol')] = 'symbl'
    this.reg = /123/;
    this.date = new Date(0);
    // this.my = this
    this.error = Error('sds')
}
let obj1 = new Obj();
Object.defineProperty(obj1, 'method', {
    value: {
        a: 1
    },
    enumerable: false
})
```
**改进版（改进后递归实现）关键点**针对上面几个待解决问题，我先通过四点相关的理论告诉你分别应该怎么做。

1. 当参数为 **Date、RegExp** 类型，则直接生成一个新的实例返回；
2. 利用 WeakMap 类型作为 Hash 表，因为 WeakMap 是弱引用类型，可以有效防止内存泄漏，如果存在**循环引用**，则引用直接返回 WeakMap 存储的值
3. 利用 Object 的 getOwnPropertyDescriptors 方法可以获得对象的所有属性，以及对应的特性，顺便结合 Object.create 方法创建一个新对象，并**继承传入原对象的原型链**；
4. **针对能够遍历对象的不可枚举属性以及 Symbol 类型，我们可以使用 Reflect.ownKeys 方法；**

如果你在考虑到循环引用的问题之后，还能用 WeakMap 来很好地解决，并且向面试官解释这样做的目的，那么你所展示的代码，以及你对问题思考的全面性，在面试官眼中应该算是合格的了
<a name="kBLst"></a>
### 2. 如何判断一个对象是空对象

1. **JSON.stringify(可枚举)：**~~**undefined**~~**  **~~**Symbol**~~
2. **Object.keys (可枚举) ：**~~**Symbol**~~
3. **Object.getOwnPropertyNames(可/不可枚举)  **~~**Symbol**~~
4. **Reflect.ownKeys (可/不/Symbol)**
5. **forin (可枚举)  **~~**Symbol**~~
使用JSON自带的.stringify方法来判断：**undefined 不能被转化   **可枚举```javascript
JSON.stringify({}) === "{}"//true
//该方法有个缺点，JSON.stringify()只能序列化对象的可枚举的自有属性，
//即如果有属性是不可枚举或继承属性的话，结果也是true
```
使用ES6新增的方法**Object.keys()**来判断：  可枚举（除symbol）```javascript
(Object.keys({}).length = 0)//true
//包含指定对象自有的可枚举属性
```
**Object.getOwnPropertyNames**  可枚举和不可枚举属性名（除symbol）```javascript
console.log(Object.getOwnPropertyNames({}).length==0);//返回true
//返回该对象所有可枚举和不可枚举属性的属性名组成的数组
```
**for in循环**```javascript
let result=function(obj){
    for(let key in obj){
        return false;//若不为空，可遍历，返回false
    }
    return true;
}
console.log(result(obj));//返回true
//for in 循环遍历一个对象的可枚举属性，包括这个对象继承自原型链上的可枚举属性
//使用for..in循环遍历对象除Symbol以外的所有可枚举属性，
//当对象有属性存在返回false，否则返回true


//在实际开发工作中，有时需要只考虑对象自身的属性，而不是继承来的，这时可以配合Object.getOwnPropertyNames() 或 Object.hasOwnProperty() 来进行过滤。
function isObjectEmpty (obj) {
  for (let key in obj) {
    if(obj.hasOwnProperty(key)) return false
  }
  return true
}
```
<a name="GR0kW"></a>
### 3. 如何判断一个对象是否属于某个类？

1. **instanceof**
2. **isPrototypeOf**
3. **constructor**
4. **Object.prototype.toString.call()**
（1）使用 instanceof 运算符来判断构造函数的 prototype 属性是否出现在对象的原型链中的任何位置。```javascript
f instanceof F
```
（2） isPrototypeOf 来判断构造函数的 prototype 属性是否出现在对象的原型链中的任何位置。```javascript
F.prototype.isPrototypeOf(f)
```
（3）通过对象的 constructor 属性来判断，对象的 constructor 属性指向该对象的构造函数，但是这种方式不是很安全，因为 constructor 属性可以被改写。```javascript
f.constructor.toString().includes("F")
```
（4）如果需要判断的是某个内置的引用类型的话，可以使用 Object.prototype.toString() 方法来打印对象的[[Class]] 属性来进行判断。```javascript
Object.prototype.toString.call(f).includes("Object")
```
<a name="hHmdQ"></a>
### 4. 有四个操作会忽略enumerable为false的属性

1. **忽略 不可枚举属性的操作：forin、JSON.stringify、Object.key、Object.assign**
1. for...in循环：只遍历对象自身的和继承的可枚举的属性。
2. JSON.stringify()：只串行化对象自身的可枚举的属性。
3. Object.keys()：返回对象自身的所有可枚举的属性的键名。
4. Object.assign()： 忽略enumerable为false的属性，只拷贝对象自身的可枚举的属性。
<a name="eIyGy"></a>
### 5. 对象属性的遍历
ES6 一共有 5 种方法可以遍历对象的属性。<br />**（1）for...in：**循环遍历对象自身的和继承的可枚举属性（不含 Symbol 属性）。<br />**（2）Object.keys(obj)：**返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性）的键名。<br />**（3）Object.getOwnPropertyNames(obj)：**返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）的键名。<br />**（4）Object.getOwnPropertySymbols(obj)：**返回一个数组，包含对象自身的所有 Symbol 属性的键名。<br />**（5）Reflect.ownKeys(obj)：**返回一个数组，包含对象自身的（不含继承的）所有键名，不管键名是 Symbol 或字符串，也不管是否可枚举。<br />以上的 5 种方法遍历对象的键名，都遵守同样的属性遍历的次序规则。

- 首先遍历所有数值键，按照数值升序排列。
- 其次遍历所有字符串键，按照加入时间升序排列。
- 最后遍历所有 Symbol 键，按照加入时间升序排列。 
<a name="MWXVE"></a>
### 6. 如何使用for...of遍历对象 

**定义一个迭代器：[Symbol.iterator] 是一个方法，该方法返回一个对象，对象中有一个next方法，该方法返回一个{value,done}形势的对象**<br />for…of是作为ES6新增的遍历方式，允许遍历一个含有iterator接口的数据结构（数组、对象等）并且返回各项的值，普通的对象用for..of遍历是会报错的。
如果需要遍历的对象是类数组对象，用Array.from转成数组即可。```javascript
var obj = {
    0:'one',
    1:'two',
    length: 2
};
obj = Array.from(obj);
for(var k of obj){
    console.log(k)
}
```
如果不是类数组对象，就给对象添加一个[Symbol.iterator]属性，并指向一个迭代器即可。```javascript
//方法一：
var obj = {
    a:1,
    b:2,
    c:3
};

obj[Symbol.iterator] = function(){
	var keys = Object.keys(this);
	var count = 0;
	return {
		next(){
			if(count<keys.length){
				return {value: obj[keys[count++]],done:false};
			}else{
				return {value:undefined,done:true};
			}
		}
	}
};

for(var k of obj){
	console.log(k);
}


// 方法二
var obj = {
    a:1,
    b:2,
    c:3
};
obj[Symbol.iterator] = function*(){
    var keys = Object.keys(obj);
    for(var k of keys){
        yield [k,obj[k]]
    }
};

for(var [k,v] of obj){
    console.log(k,v);
}

```
<a name="r4uBu"></a>
### 7.Js有哪些内置对象
全局的对象（ global objects ）或称标准内置对象，不要和 "全局对象（global object）" 混淆。这里说的全局的对象是说在全局作用域里的对象。全局作用域中的其他对象可以由用户的脚本创建或由宿主程序提供。<br />**标准内置对象的分类：**<br />（1）值属性，这些全局属性返回一个简单值，这些值没有自己的属性和方法。<br />例如 NaN、undefined、null Infinity 字面量<br />（2）函数属性，全局函数可以直接调用，不需要在调用时指定所属对象，执行结束后会将结果直接返回给调用者。<br />例如 eval()、parseFloat()、parseInt()  escape，isNaN等<br />（3）基本对象，基本对象是定义或使用其他对象的基础。基本对象包括一般对象、函数对象和错误对象。<br />例如 Object、Function、Boolean、Symbol、Error 等<br />（4）数字和日期对象，用来表示数字、日期和执行数学计算的对象。<br />例如 Number、Math、Date<br />（5）字符串，用来表示和操作字符串的对象。<br />例如 String、RegExp<br />（6）可索引的集合对象，这些对象表示按照索引值来排序的数据集合，包括数组和类型数组，以及类数组结构的对象。例如 Array<br />（7）使用键的集合对象，这些集合对象在存储数据时会使用到键，支持按照插入顺序来迭代元素。<br />例如 Map、Set、WeakMap、WeakSet<br />（8）矢量集合，SIMD 矢量集合中的数据会被组织为一个数据序列。<br />例如 SIMD 等<br />（9）**结构化数据**，这些对象用来表示和操作结构化的缓冲区数据，或使用 JSON 编码的数据。<br />例如 JSON 等<br />（10）控制抽象对象<br />例如 Promise、Generator 等<br />（11）反射<br />例如 Reflect、Proxy<br />（12）国际化，为了支持多语言处理而加入 ECMAScript 的对象。<br />例如 Intl、Intl.Collator 等<br />（13）WebAssembly<br />（14）其他<br />例如 arguments<br />**总结：**<br />js 中的内置对象主要指的是在程序执行前存在全局作用域里的由 js 定义的一些全局值属性、函数和用来实例化其他对象的构造函数对象。一般经常用到的如全局变量值 NaN、undefined，全局函数如 parseInt()、parseFloat() 用来实例化对象的构造函数如 Date、Object 等，还有提供数学计算的单体内置对象如 Math 对象。
<a name="M9MeC"></a>
### 8. 对象合并
```javascript
const obj1 = { a: 1, b: 2 };
const obj2 = { b: 3, d: 4 };
```
**Object.assign**```javascript
const obj3 = Object.assign(obj1, obj2);
```
**扩展运算符**```javascript
const obj4 = { ...obj1, ...obj2 };
```
**forin遍历**```javascript
function extend(target, source) {
    for (const key in source) {
        if (Object.hasOwnProperty.call(source, key)) {
            target[key] = source[key];
        }
    }
    return target;
}
extend(obj1, obj2)
```
<a name="MFpwX"></a>
## 数组相关
<a name="jszZC"></a>
### 1. JS数组去重

1. NaN的处理
2. 是否修改数组
**2. ES5中最常用：利用for嵌套for，然后splice去重    1.（=== && 注意NaN） 处理NaN  2.（（typeof a + a && 全为对象）|| ===）引用和普通类型分开处理**```javascript
function unique(arr) {
    for (var i = 0; i < arr.length; i++) {
        for (var j = i + 1; j < arr.length; j++) {
            if (arr[i] === arr[j] || 
                (Number.isNaN(arr[i]) && Number.isNaN(arr[j]))) 
            {         
                //第一个等同于第二个，splice方法删除第二个
                arr.splice(j, 1);
                j--;
            }
        }
    }
    return arr;
}
```
es5方式一    **处理NaN**
```javascript
function unique(arr) {
    const ifNaN = (item) => typeof item === "number" && isNaN(item);
    for (var i = 0; i < arr.length; i++) {
        for (var j = i + 1; j < arr.length; j++) {
            if (arr[i] === arr[j] || (ifNaN(arr[i]) && ifNaN(arr[j]))) {
                //第一个等同于第二个，splice方法删除第二个
                arr.splice(j, 1);
                j--;
            }
        }
    }
    return arr;
}
```
es5方式二   **引用和普通类型分开处理 **
```javascript
function unique(arr) {
    // (typeof a + a) === (typeof b + b) 非对象才能判断
    const is = (a, b) => (((typeof a + a) === (typeof b + b)) && (typeof a !== 'object' && typeof b !== 'object')) ? true : a === b
    for (var i = 0; i < arr.length; i++) {
        for (var j = i + 1; j < arr.length; j++) {
            //第一个等同于第二个，splice方法删除第二个
            is(arr[i], arr[j]) && arr.splice(j--, 1);
        }
    }
    return arr;
}
```
**利用filter   indexOf+index   处理NaN  **```javascript
function unique(arr) {
    let nanIndex = false
    const ifNaN = (item) => typeof item === "number" && isNaN(item);
    return arr.filter(function (item, index, arr) {
        //处理NaN
        if (ifNaN(item)) {
            return nanIndex ? false : nanIndex = true
        }
        //当前元素，在原始数组中的第一个索引==当前索引值，否则返回当前元素
        return arr.indexOf(item) == index
    })
}
```
**5 .利用filter  hasOwnProperty(typeof item + item)对象属性去重   indexOf+index处理对象  **```javascript
function unique5(arr) {
    const mapObj = {}
    return arr.filter((item, i) => {
        if (item instanceof Object) return arr.indexOf(item) === i
        return mapObj.hasOwnProperty((typeof item) + item) ? false : mapObj[(typeof item) + item] = true
    })
}
```
**利用递归去重  处理NaN**```javascript
function unique(arr) {
    var len = arr.length;
    arr.sort((a, b) => a - b)
    const ifNaN = (item) => typeof item === "number" && isNaN(item);
    function loop(index) {
        if (index >= 1) {
            if (arr[index] === arr[index - 1] || (ifNaN(arr[index]) && ifNaN(arr[index - 1]))) {
                arr.splice(index, 1);
            }
            loop(index - 1);    //递归loop，然后数组去重
        }
    }
    loop(len - 1);
    return arr;
}
```
**利用sort()**```javascript
function unique(arr) {
    const ifNaN = (item) => typeof item === "number" && isNaN(item);
    arr = arr.sort()
    var arrry = [arr[0]];
    for (var i = 1; i < arr.length; i++) {
        if ((ifNaN(arr[i]) && ifNaN(arr[i - 1]))) {
            continue
        }
        if (arr[i] !== arr[i - 1]) {
            arrry.push(arr[i]);
        }
    }
    return arrry;
}
```
**1. set  集合去重  返回新数组 **```javascript
Array.from(new Set(arr))
[...new Set(arr)]
```
**3. reduce+includes  归并  返回新数组 **```javascript
function unique(arr){
    return arr.reduce((prev,cur) => prev.includes(cur) ? prev : [...prev,cur],[]);
}
```
**4. 利用Map数据结构去重 +push  返回新数组 **```javascript
function unique4(arr) {
    const newArry = []
    const map = new Map()
    for (const val of arr) {
        if (!map.has(val)) {
            newArry.push(val)
            map.set(val, true)
        }
    }
    return newArry
}
```
**6. includes+push  返回新数组  **```javascript
function unique3(arr) {
    let newArr = []
    for (const value of arr) {
        !newArr.includes(value) && newArr.push(value)
    }
    return newArr
}
```
测试```javascript
let obj1 = {}
let obj2 = obj1
var arr = [
            1,
            1,
            "1",
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
            // Infinity,
            // Infinity,
            // -Infinity,
            // -Infinity,
            // obj1,
            // obj2
        ];
console.log(unique(arr));
```
<a name="nJg1l"></a>
### 2.JS中flat---数组扁平化
```javascript
let ary = [1, [2, [3, [4, 5]]], 6];// -> [1, 2, 3, 4, 5, 6]
let str = JSON.stringify(ary);
```
一、**调用ES6中的flat方法**```javascript
ary = ary.flat(Infinity);
```
二、**遍历(for/for...of) + 递归**```javascript
function deepFlat(arr) {
    let newArr = []
    for (const val of arr) {
        newArr = newArr.concat(Array.isArray(val) ? deepFlat(val) : val)
    }
    return newArr
}
```
三、**利用reduce+递归**```javascript
function deepFlat(arr) {
    return arr.reduce((pre, cur) => {
        return pre.concat(Array.isArray(cur) ? deepFlat(cur) : cur)
    }, [])
}
```
四、(**while  + some** )  **+ 扩展运算符/** **Array.prototype.concat.apply([], arr)**```javascript
function deepFlat(arr) {
    while (arr.some(Array.isArray)) {
        arr = [].concat(...arr)
        // arr = Array.prototype.concat.apply([], arr)
    }
    return arr;
}
```
五、**toString和split结合    缺点多(String)**```javascript
function deppFlat(arr) {
  return [...arr.toString().split(',').map(Number)]
}
```
<a name="YhCAe"></a>
### 3.JavaScript 类数组对象的定义？	
一个拥有 length 属性和若干索引属性的对象就可以被称为类数组对象，类数组对象和数组类似，但是不能调用数组的方法。常见的类数组对象有 arguments 和 DOM 方法的返回结果，函数也可以被看作是类数组对象，因为它含有 length 属性值，代表可接收的参数个数。<br />常见的类数组转换为数组的方法有这样几种：
（1）通过 call 调用数组的 slice 方法来实现转换```javascript
Array.prototype.slice.call(arrayLike);
```
（2）通过 call 调用数组的 splice 方法来实现转换```javascript
Array.prototype.splice.call(arrayLike, 0);
//未能从' NodeList '中删除索引属性：不支持索引属性删除器。
```
（3）通过 apply 调用数组的 concat 方法来实现转换```javascript
Array.prototype.concat.apply([], arrayLike);
```
（4）通过 Array.from 方法来实现转换```javascript
Array.from(arrayLike);
```
（5） 使用展开运算符将类数组转化成数组 ：前提要有 **iterable**```
const arrArgs = [...arguments];
```
（6）以下几种方法需要考虑稀疏数组的转化```javascript
Array.prototype.filter.call(divs, x => 1)
Array.prototype.map.call(arrayLike, x => x)
Array.prototype.filter.call(arrayLike, x => 1)
```
<a name="S7EIc"></a>
### 4数组有哪些原生方法？
![image.png](https://cdn.nlark.com/yuque/0/2022/png/28203632/1664245254925-52637213-8c08-4587-bbd7-436b2395323c.png#averageHue=%23eee9e1&clientId=ubc265b9a-134b-4&errorMessage=unknown%20error&from=paste&id=npxzh&originHeight=454&originWidth=1426&originalType=url&ratio=1&rotation=0&showTitle=false&size=187133&status=error&style=stroke&taskId=u0592e94e-ae52-4d5d-96c0-e5a6f6b4d6e&title=)

1. 数组和字符串的转换方法：toString()、~~toLocalString()~~、join() ~~其中 join() 方法可以指定转换为字符串时的分隔符。~~
2. 数组尾部操作的方法 push() 和 pop()，push 方法可以传入多个参数。
3. 数组首部操作的方法 unshift() 和 shift()
4. 重排序的方法 reverse() 和 sort()，sort() 方法可以传入一个函数来进行比较，传入前后两个值，如果返回值为正数，则交换两个参数的位置。
5. 数组连接的方法 concat() ，返回的是拼接好的数组，不影响原数组。
6. 数组截取办法 slice()，用于截取数组中的一部分返回，不影响原数组。
7. 数组插入方法 splice()，影响原数组
8. 查找特定项的索引的方法，indexOf() 和 lastIndexOf()  fondIndex()
9. 迭代方法 every()、some()、filter()、map() 和 forEach() 方法	
10. 数组归并 (并入)方法reduce() 和 reduceRight() 方法
<a name="MUcjn"></a>
### 5. 数组的遍历方法有哪些
| **方法** | **是否改变原数组** | **特点** |
| --- | --- | --- |
| **forEach()** | 否 | 数组方法，不改变原数组，没有返回值 |
| **map()** | 否 | 数组方法，不改变原数组，有返回值，可链式调用 |
| **filter()** | 否 | 数组方法，过滤数组，返回包含符合条件的元素的数组，可链式调用 |
| **every() 和 some()** | 否 | 数组方法，some()只要有一个是true，便返回true；而every()只要有一个是false，便返回false. |
| **find() 和 findIndex()** | 否 | 数组方法，find()返回的是第一个符合条件的值；findIndex()返回的是第一个满足条件的值的索引值 |
| **reduce() 和 reduceRight()** | 否 | 数组方法，reduce()对数组正序操作；reduceRight()对数组逆序操作 |
| **for...of** | 否 | for...of遍历具有 Iterator 迭代器的对象的属性，返回的是数组的元素、对象的属性值，不能遍历普通的obj对象，将异步循环变成同步循环 |

遍历方法的详细解释：[《细数JavaScript中那些遍历和循环》](https://cuggz.blog.csdn.net/article/details/107649549)
<a name="Z09Ue"></a>
### 6. forEach和map方法有什么区别 
这方法都是用来遍历数组的，两者区别如下：

1. forEach()方法会针对每一个元素执行提供的函数，该方法**没有返回值**；
2. map()方法不会改变原数组的值，**返回一个新数组**，新数组中的值为原数组调用函数处理之后的值；
3. map 适合对数组元素操作，forEach 适合遍历数据
4. forEach()允许callback更改原始数组的元素。map()返回新的数组。
```javascript
let arr = [1, 2, 3, 4];
let arr2 = [...arr]

arr2.forEach((item, index, array) => {
  console.log(item, index)
  array.splice(index, 1);
});


console.log(arr)//[1, 2, 3, 4]
console.log(arr2)//[2, 4]
```
<a name="AAWNW"></a>
### 7.forEach 和 for 的区别

1. **for 可以 break 跳出循环**
2. **且 for 可以控制循环起点**
3. **for 在循环过程中可以修改索引，forEach 的索引由底层控制 **
4. **forEach 循环是并行执行的，for 是串行执行**
1. for循环可以使用break跳出循环，但forEach不能。
2. for循环可以控制循环起点（i初始化的数字决定循环的起点），forEach只能默认从索引0开始。
3. for循环过程中支持修改索引（修改 i），但forEach做不到（底层控制index自增，我们无法左右它）
4. foreach 适用于循环次数未知，或者计算循环次数比较麻烦情况下使用效率更高，但是更为复杂的一些循环还是需要用到for循环效率更高。
5. forEach的回调函数是同步调用的
关于循环中包含有异步语句：forEach 和 map, some, every 循环是并行执行的，相当于 Promise.all，其它 for, for in, for of, while, do while 都是串行执行的。```javascript
const arr = [1, 2, 3, 4]
const foo = i => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log(i)
            resolve('')
        }, 1000)
    })
}

e()
a()
console.log(1111111111111)

async function e() {
    arr.forEach(async a => {
        console.log(121)
        await foo(a)
    })
}

async function a() {
    for (var i = 0; i < arr.length;) {
        await foo(arr[i])
        i++
    }
}


console.log('emd')
// 4index.js:17 121
// index.js:13 1111111111111
// index.js:30 emd
// index.js:5 1
// index.js:5 2
// index.js:5 3
// index.js:5 4
// index.js:5 1
// index.js:5 2
// index.js:5 3
// index.js:5 4
```
可以看到本质上 forEach 还是通过 while 循环来实现的，假如我们想要一个异步的 forEach 的话，只需要将 callback 的调用改成 await 即可：<br /> <br />首先查看 forEach 的 [polyfill](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)，简化后可以理解为以下代码：
```javascript
Array.prototype.forEachAsync = async function(callback, thisArg) {
  var len = this.length
  var k = 0
  while (k < len) {
    var kValue
    if (k in this) {
      kValue = this[k]
      await callback.call(thisArg, kValue, k, this)
    }
    k++
  }
}
```
<a name="HnV2o"></a>
### 8. forEach中return有效果吗？如何中断forEach循环？
**在forEach中用return不会返回，函数会继续执行。**
```javascript
let nums = [1, 2, 3];
nums.forEach((item, index) => {
  return;//无效
})
```
**中断方法：**

- 使用try监视代码块，在需要中断的地方使用**throw****抛出异常**。
- 官方推荐方法（替换方法）：用every和some替代forEach函数。
   - every在碰到return false的时候，中止循环。
   - some在碰到return true的时候，中止循环
<a name="Q6QRy"></a>
### 8. 快速的让一个数组乱序
```css
var arr = [1,2,3,4,5,6,7,8,9,10];
arr.sort( Math.random() - 0.5 )
console.log(arr);
```
<a name="VN6FN"></a>
### 9. 请创建一个长度为100，值都为1的数组
```javascript
new Array(100).fill(1)
```
<a name="rw6mN"></a>
## 函数相关
<a name="idTxU"></a>
### 1. new操作符具体做了什么

1. **创**建了一个空的对象
2. 将空对象的**原型**，指向于构造函数的原型 prototype
3. 让函数的 **this **指向这个对象，执行构造函数的代码（为这个新对象添加属性）
4. 判断函数的返回值类型，如果是值类型，返回创建的对象。如果是引用类型，就返回这个引用类型的对象。
**模拟new过程**```javascript
function create(sup, ...args) {
    // 1. 创建了一个空的对象
    let obj = {}
    // 2. 将空对象的原型，指向于构造函数的原型
    Object.setPrototypeOf(obj, sup.prototype)
    // 3. 将空对象作为构造函数的上下文（改变this指向）
    let res = sup.apply(obj, args)
    // 4. 对构造函数返回值的处理判断
    return res instanceof Object ? res : obj
}
```
<a name="VMpdr"></a>
### 2. 谈谈This对象的理解
this 是执行上下文中的一个属性，**它的设计目的就是在函数体内部，this引用的是把函数当成方法调用的上下文对象**

1. **方法调用模式**
   1. this总是指向函数的直接调用者（而非间接调用者）
   2. 在事件中，this指向触发这个事件的对象，特殊的是，~~IE~~~~中的~~~~attachEvent~~~~中的~~~~this~~~~总是指向全局对象~~~~Window~~
2. **函数调用模式**
   1. this总是指向全局对象Window
   2. 严格模式下 this 为 undefined
3. **构造器调用模式**
   1. new关键字，this指向new出来的那个对象
4. **显示绑定：apply/call/bind调用模式**
   1. 显示操纵 this 指针 call apply bind
5. **箭头函数this**，代码书写时就能确定 this 的指向（编译时绑定），指向定义时作用域下的this
6. 优先级：new绑定优先级 > 显示绑定优先级 > 隐式绑定优先级 > 默认绑定优先

![image.png](https://cdn.nlark.com/yuque/0/2022/png/28203632/1663770386362-eb4efb1a-7c00-49ee-8ba3-a5e93db246a0.png#averageHue=%23f5f5f5&clientId=u0d1bb06b-58ac-4&errorMessage=unknown%20error&from=paste&height=487&id=WhqjX&originHeight=487&originWidth=632&originalType=binary&ratio=1&rotation=0&showTitle=false&size=43728&status=error&style=stroke&taskId=uafcc0cd0-a8ac-499c-963d-93f82113b83&title=&width=632)
<a name="sIyK7"></a>
### 3. 为什么函数的 arguments 参数是类数组而不是数组？如何遍历类数组?
`arguments`是一个对象，~~它的属性是从 0 开始依次递增的数字~~，还有`**callee**`**和**`**length**`等属性，与数组相似；**但是它却没有数组常见的方法属性，如**`**forEach**`**, **`**reduce**`**等，所以叫它们类数组**。<br />遍历类数组
（1）将数组的方法（**forEach**）应用到类数组上，这时候就可以使用 **call **和 **apply **方法，如：```javascript
function foo(){ 
  Array.prototype.forEach.call(arguments, a => console.log(a))
}
```
（2）转化成数组再遍历
<a name="L2a2n"></a>
### 4. javascript中callee和caller的作用？
caller是函数对象中的属性：返回一个对该函数的调用者
1.   ○ 这个属性只有当函数在执行时才有用
2.   ○ 如果在javascript程序中，**函数是由顶层调用的，则返回null**
```css
var a = function () {
    alert(a.caller);
}
var b = function () {
    a();
}
b();
// 上面的代码中，b调用了a，那么a.caller返回的是b的引用，结果如下:
var b = function () {
    a();
}
```
callee是返回正在被执行的function函数，它是arguments的一个属性```javascript
var a = function () {
    alert(arguments.callee === a);//true
}
a();
```
<a name="h94yf"></a>
### 5. 箭头函数和普通函数有什么区别？

1. 箭头函数中的this在定义时就决定的，指向它定义时所在作用域下的this，且不可修改（call、apply、bind)
2. 箭头函数不能new(不能当作构造函数) ，否则抛出错误
3. 箭头函数没有prototype，箭头函数无 super。箭头函数无new.target
4. 箭头函数没有arguments，取参可以用Rest参数代替
5. 箭头函数不能用作Generator函数，不能使用yeild关键字
6. ~~箭头函数全都是~~~~匿名~~~~函数：普通函数可以有匿名函数，也可以有具名函数~~
<a name="tJLGI"></a>
### 6. call 、apply、bind  的区别

1. 函数通过call、apply调用返回该函数正常调用时的返回值 。
2. 函数通过 bind 调用返回的是一个 **bound 函数** ，需再调用
**bound 函数内容**![image.png](https://cdn.nlark.com/yuque/0/2022/png/28203632/1662693825827-fc1d5e61-c3c6-401f-a1e6-57f7bdfb9a0e.png#averageHue=%23fefcfb&clientId=u9bb780f4-52b3-4&errorMessage=unknown%20error&from=paste&height=184&id=SVjn7&originHeight=184&originWidth=250&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11258&status=error&style=stroke&taskId=u1084e495-0cd7-4b4f-b76c-3b41a36831c&title=&width=250)

3. 参数不同：apply第二个参数是数组。ca11和bind有多个参数需要挨个写。
**apply 使用场景**```javascript
// 1.用applyl的情况
var arr1 = [1, 2, 45, 7, 3, 321];
console.log(Math.max.apply(null, arr1));
```
<a name="FamXn"></a>
### 7.  实现call、apply 及 bind 函数
**（1）call 函数的实现步骤：**

- 判断调用对象是否为函数，即使是定义在函数的原型上的，但是可能出现使用 call 等方式调用的情况。
- 判断传入上下文对象是否存在，如果不存在，则设置为 window 。
- 处理传入的参数，截取第一个参数后的所有参数。
- **将函数作为上下文对象的一个属性。**
- **使用上下文对象来调用这个方法，并保存返回结果。**
- **删除刚才新增的属性。**
- **返回结果。**
```javascript
Function.prototype.mycall = function (content) {
    // 判断调用对象
    if (typeof this !== 'function') throw new Error("this不是函数")
    // 判断 context 是否传入，如果未传入则设置为 window
    content = content || window
    // 获取参数
    const args = [...arguments].slice(1)
    // 将调用函数设为对象的方法
    content.method = this
    // 调用函数
    let res = content.method(...args)
    // 将属性删除
    delete content.method
    // 返回调用结果
    return res
}
```
**（2）apply 函数的实现步骤：**

- 判断调用对象是否为函数，即使是定义在函数的原型上的，但是可能出现使用 call 等方式调用的情况。
- 判断传入上下文对象是否存在，如果不存在，则设置为 window 。
- **将函数作为上下文对象的一个属性。**
- **判断参数值是否传入**
- **使用上下文对象来调用这个方法，并保存返回结果。**
- **删除刚才新增的属性**
- **返回结果**
```javascript
Function.prototype.myApply = function(context) {
  // 判断调用对象是否为函数
  if (typeof this !== "function") {
    throw new TypeError("Error");
  }
  let result = null;
  // 判断 context 是否存在，如果未传入则为 window
  context = context || window;
  // 将函数设为对象的方法
  context.fn = this;
  // 调用方法
  if (arguments[1]) {
    result = context.fn(...arguments[1]);
  } else {
    result = context.fn();
  }
  // 将属性删除
  delete context.fn;
  return result;
};
```
**（3）bind 函数的实现步骤：**

1. 判断调用对象是否为函数，即使是定义在函数的原型上的，但是可能出现使用 call 等方式调用的情况。
2. **保存当前函数的引用，获取其余传入参数值。**
3. **创建一个函数返回**
4. **函数内部使用 apply 来绑定函数调用，传入组合参数（内外arguments）**
   1. **需要判断函数作为构造函数的情况**，**这个时候需要传入当前函数的 this 给 apply 调用**，
   2. **其余情况都传入指定的上下文对象。**
```javascript
Function.prototype.myBind = function (context) {
    // 判断调用对象
    if (typeof this !== 'function') throw new Error("this不是函数")
    // 获取参数
    let args = [...arguments].slice(1)
    let fn = this;
    function Fn() {
        // 根据调用方式，传入不同绑定值
        return fn.apply(this instanceof Fn ? this : context, args.concat(...arguments));
    };
    // Fn.prototype = Object.create(fn.prototype)
    return Fn
};
```
<a name="pN2Ai"></a>
### 8.函数柯里化
柯里化(Currying)**是一种函数转化方法**，它**将一个接收多参数的函数转化为接 收部分参数的函数。柯里化后的函数可只传 递部分参数来调用，并返回一个新的函数 去处理剩余的参数，是逐步传参的过程。****柯里化作用**

1. **延时计算**，又叫惰性求值。柯里化后的 函数是分步执行的，前几次调用均返回一 个函数，累积传入的参数，最后调用才会 计算，起到延时计算的作用。如bind函 数，改变this指向但并不执行，只返回函数
2. **固定易变因素**。提前将易变因素传参 定下来，生成一个更明确的应用函数。
3. **参数复用**。当多次调用同一个函数，并 且传递的参数绝大多数是相同的时候。
4. **动态创建函数**。先完成部分传参， 创建新的函数返回，在需要的时候传入其余的参数调用执行。
例子：求和```javascript
function num(arr) {
    return arr.reduce((pre, cur) => pre + cur)
}
function curry(...args1) {
    let arr = args1
    return function inner(...args2) {
        if (args2.length > 0) {
            arr.push(...args2)
            return inner
        } else {
            return num(arr)
        }
    }
}

console.log(curry(1)(2, 10)(3)(4)(7)())
```
柯里化一个函数```javascript
const curry = function (fn) {
    return function nest(...args) {
        // fn.length表示函数的形参个数
        if (args.length === fn.length) {
            // 当参数接收的数量达到了函数fn的形参个数，
          	//即所有参数已经都接收完毕则进行最终的调用
            return fn(...args);
        } else {
            // 参数还未完全接收完毕，递归返回judge，将新的参数传入
            return function (arg) {
                return nest(...args, arg);
            }
        }
    }
}
```
<a name="J4nkn"></a>
## DOM
<a name="DpTp8"></a>
### 1. 什么是 DOM 和 BOM？

- **DOM 指的是文档对象模型**，它指的是把文档当做一个对象，这个对象主要定义了处理网页内容的方法和接口。
- **BOM 指的是浏览器对象模型**，它指的是把浏览器当做一个对象来对待，这个对象主要定义了与浏览器进行交互的法和接口。
- **BOM的核心是 window**，而 window 对象具有双重角色，它既是通过 js 访问浏览器窗口的一个接口，又是一个 Global（全局）对象。这意味着在网页中定义的任何对象，变量和函数，都作为全局对象的一个属性或者方法存在。
- window 对象含有 **location** 对象、**navigator** 对象、**screen** 对象等子对象，并且 DOM 的最根本的对象 **document** 对象也是 BOM 的 window 对象的子对象。
<a name="RB6d5"></a>
### 2. 常见的DOM操作有哪些
<a name="V6l4O"></a>
#### 1）DOM 节点的获取
DOM 节点的获取的API及使用：```javascript
getElementById // 按照 id 查询
getElementsByTagName // 按照标签名查询
getElementsByClassName // 按照类名查询
querySelectorAll // 按照 css 选择器查询

// 按照 id 查询
var imooc = document.getElementById('imooc') // 查询到 id 为 imooc 的元素
// 按照标签名查询
var pList = document.getElementsByTagName('p')  // 查询到标签为 p 的集合
console.log(divList.length)
console.log(divList[0])
// 按照类名查询
var moocList = document.getElementsByClassName('mooc') // 查询到类名为 mooc 的集合
// 按照 css 选择器查询
var pList = document.querySelectorAll('.mooc') // 查询到类名为 mooc 的集合
```
<a name="naTwz"></a>
#### 2）DOM 节点的创建
**创建一个新节点，并把它添加到指定节点的后面。**已知的 HTML 结构如下：```html
<html>
  <head>
    <title>DEMO</title>
  </head>
  <body>
    <div id="container"> 
      <h1 id="title">我是标题</h1>
    </div>   
  </body>
</html>
```
要求添加一个有内容的 span 节点到 id 为 title 的节点后面，做法就是：
```javascript
// 首先获取父节点
var container = document.getElementById('container')
// 创建新节点
var targetSpan = document.createElement('span')
// 设置 span 节点的内容
targetSpan.innerHTML = 'hello world'
// 把新创建的元素塞进父节点里去
container.appendChild(targetSpan)
```
<a name="rGwyE"></a>
#### 3）DOM 节点的删除
**删除指定的 DOM 节点，**已知的 HTML 结构如下：```javascript
<html>
  <head>
    <title>DEMO</title>
  </head>
  <body>
    <div id="container"> 
      <h1 id="title">我是标题</h1>
    </div>   
  </body>
</html>
```
需要删除 id 为 title 的元素，做法是：
```javascript
// 获取目标元素的父元素
var container = document.getElementById('container')
// 获取目标元素
var targetNode = document.getElementById('title')
// 删除目标元素
container.removeChild(targetNode)
targetNode.parentNode.removeChild(targetNode)
```
或者通过子节点数组来完成删除：
```javascript
// 获取目标元素的父元素
var container = document.getElementById('container')
// 获取目标元素
var targetNode = container.childNodes[1]
// 删除目标元素
container.removeChild(targetNode)
```
<a name="h83VY"></a>
#### 4）修改 DOM 元素
修改 DOM 元素这个动作可以分很多维度，比如说移动 DOM 元素的位置，修改 DOM 元素的属性等。**将指定的两个 DOM 元素交换位置，**已知的 HTML 结构如下：
```javascript
<html>
  <head>
    <title>DEMO</title>
  </head>
  <body>
    <div id="container"> 
      <h1 id="title">我是标题</h1>
      <p id="content">我是内容</p>
    </div>   
  </body>
</html>
```
现在需要调换 title 和 content 的位置，可以考虑 insertBefore 或者 appendChild：
```javascript
// 获取父元素
var container = document.getElementById('container')   
 
// 获取两个需要被交换的元素
var title = document.getElementById('title')
var content = document.getElementById('content')
// 交换两个元素，把 content 置于 title 前面
container.insertBefore(content, title)
```
<a name="cHd9z"></a>
### 3. 怎样添加、移除、移动、复制、创建和查找节点
**创建新节点**
```css
createDocumentFragment()    //创建一个DOM片段
createElement()   //创建一个具体的元素
createTextNode()   //创建一个文本节点
```
**添加、移除、替换、插入**
```css
appendChild()      //添加
removeChild()      //移除
replaceChild()      //替换
insertBefore()      //插入
```
**查找**
```css
getElementsByTagName()    //通过标签名称
getElementsByName()     //通过元素的Name属性的值
getElementById()        //通过元素Id，唯一性
```
<a name="LdlYB"></a>
### 4.offsetWidth/offsetHeight,clientWidth/clientHeight与scrollWidth/scrollHeight的区别

- 实际宽高：offsetWidth/offsetHeight返回值包含**content + padding + border**，**不包含滚动条**，效果与e.getBoundingClientRect()相同
- 可视区域宽高：clientWidth/clientHeight返回值只包含**content + padding**，如果有滚动条，也**不包含滚动条**
- 实际宽高：scrollWidth/scrollHeight返回值包含**content + padding + 溢出内容的尺寸（padding+content（内容实际尺寸包括溢出部分））**
> 1. 可以看到scrollHeight和clientHeight输出结果一样，那么它们之间有什么区别呢?
> 2. 其实它们的区别就一个：scrollHeight的高度需要**根据实际内容的实际尺寸**决定

<a name="YKvJC"></a>
### 5. 介绍 DOM 的发展

- DOM：文档对象模型（Document Object Model），定义了访问HTML和XML文档的标准，与编程语言及平台无关
- DOM0：提供了查询和操作Web文档的内容API。未形成标准，实现混乱。如：document.forms['login']
- DOM1：W3C提出标准化的DOM，简化了对文档中任意部分的访问和操作。如：JavaScript中的Document对象
- DOM2：原来DOM基础上扩充了鼠标事件等细分模块，增加了对CSS的支持。如：getComputedStyle(elem, pseudo)
- DOM3：增加了XPath模块和加载与保存（Load and Save）模块。如：XPathEvaluator
<a name="TgAPL"></a>
### 6. 介绍DOM0，DOM2，DOM3事件处理方式区别

- DOM0级事件处理方式：
   - btn.onclick = func;
   - btn.onclick = null;
- DOM2级事件处理方式：
   - btn.addEventListener('click', func, false);
   - btn.removeEventListener('click', func, false);
   - btn.attachEvent("onclick", func);
   - btn.detachEvent("onclick", func);
- DOM3级事件处理方式：
   - eventUtil.addListener(input, "textInput", func);
   - eventUtil 是自定义对象，textInput 是DOM3级事件
<a name="c374f66a837eb1dba8f7510679b1c205"></a>
### 7. 如何判断元素是否到达可视区域 
以图片显示为例：

- `window.innerHeight` **是浏览器可视区的高度**；
- `document.body.scrollTop || document.documentElement.scrollTop` **是浏览器滚动的过的距离**；
- `imgs.offsetTop` 是元素顶部距离文档顶部的高度（包括滚动条的距离）；
- **内容达到显示区域的：**`**img.offsetTop < window.innerHeight + document.body.scrollTop;**`

![](https://cdn.nlark.com/yuque/0/2020/png/1500604/1603966605254-fe880ec0-ebd1-4f94-b662-cdd5e5396c34.png#averageHue=%23848484&height=746&id=iadEG&originHeight=1726&originWidth=1852&originalType=binary&ratio=1&rotation=0&showTitle=false&size=0&status=done&style=shadow&title=&width=800)
<a name="MCo0w"></a>
## 事件
<a name="Odd3K"></a>
### 1. 事件模型
事件流是一个事件沿着特定数据结构传播的过程。冒泡和捕获是事件流在DOM中两种不同的传播方法<br />事件的发生经历三个阶段：捕获阶段（capturing）、目标阶段（targetin）、冒泡阶段（bubbling）

- 冒泡型事件：当你使用事件冒泡时，子级元素先触发，父级元素后触发
- 捕获型事件：当你使用事件捕获时，父级元素先触发，子级元素后触发
- DOM事件流：同时支持两种事件模型：捕获型事件和冒泡型事件

**事件捕获**<br />事件捕获（event capturing）：通俗的理解就是，当鼠标点击或者触发dom事件时，浏览器会从根节点开始由外到内进行事件传播，即点击了子元素，如果父元素通过事件捕获方式注册了对应的事件的话，会先触发父元素绑定的事件<br />**事件冒泡**<br />事件冒泡（dubbed bubbling）：与事件捕获恰恰相反，事件冒泡顺序是由内到外进行事件传播，直到根节点<br />无论是事件捕获还是事件冒泡，它们都有一个共同的行为，就是事件传播<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/28203632/1664269751221-8747cedf-9221-4407-866d-35145b6d9fdf.png#averageHue=%23f7f7f7&clientId=ue68aebd0-043e-4&errorMessage=unknown%20error&from=paste&id=aNeWk&originHeight=357&originWidth=394&originalType=url&ratio=1&rotation=0&showTitle=false&size=47025&status=error&style=stroke&taskId=u7bb0605a-c9b8-438b-ab4f-007db907cea&title=)<br />**阻止事件冒泡**

- event.stopPropagation() 
- 在IE下设置event.cancelBubble = true

**阻止默认行为**

- event.preventDefault()
- 在非IE下则使用 event.preventDefault()进行阻止

**preventDefault与stopPropagation的区别**

- preventDefault告诉浏览器不用执行与事件相关联的默认动作（如表单提交）
- stopPropagation是停止事件继续冒泡，

**W3C事件的 target 与 currentTarget 的区别？**

- target 只会出现在事件流的目标阶段
- currentTarget 可能出现在事件流的任何阶段
- 当事件流处在目标阶段时，二者的指向相同
- 当事件流处于捕获或冒泡阶段时：**currentTarget**** 指向当前事件活动的对象**(一般为父级)
**事件注册****事件注册**

- 通常我们使用 addEventListener 注册事件，该函数的第三个参数可以是布尔值，也可以是对象。对于布尔值 useCapture 参数来说，该参数默认值为 false（冒泡）。useCapture 决定了注册的事件是捕获事件还是冒泡事件
- 一般来说，我们只希望事件只触发在目标上，这时候可以使用 stopPropagation 来阻止事件的进一步传播。通常我们认为 stopPropagation 是用来阻止事件冒泡的，其实该函数也可以阻止捕获事件。stopImmediatePropagation（DOM3级新增事件） 同样也能实现阻止事件冒泡和捕获，但是还能阻止该事件目标执行别的注册事件

**stopImmediatePropagation() 和 stopPropagation()的区别在哪儿呢**？后者只会阻止冒泡或者是捕获。 但是前者除此之外还会阻止该元素的其他事件发生，但是后者就不会阻止其他事件的发生。<br />**事件的代理/委托**

- 事件委托是指将事件绑定目标元素的到父元素上，利用冒泡机制触发该事件
   - 优点：
      - 可以减少事件注册，节省大量内存占用
      - 可以将事件应用于动态添加的子元素上
      - 不需要给子节点注销事件
   - 缺点： 使用不当会造成事件在不应该触发时触发
   - **原理**
      - 实现事件委托是利用了事件的冒泡原理实现的。当我们为最外层的节点添加点击事件，那么里面的ul、li、a的点击事件都会冒泡到最外层节点上，委托它代为执行事件
   - 示例：
```javascript
ulEl.addEventListener('click', function(e){
    var target = event.target || event.srcElement;
    if(!!target && target.nodeName.toUpperCase() === "LI"){
        console.log(target.innerHTML);
    }
}, false);
```


**事件执行次数和顺序**

- 事件执行次数（DOM2-addEventListener）：元素上绑定事件的个数
   - 注意1：前提是事件被确实触发
   - 注意2：事件绑定几次就算几个事件，即使类型和功能完全一样也不会“覆盖”
- 事件执行顺序：判断的关键是否目标元素
   - 非目标元素：根据W3C的标准执行：捕获->目标元素->冒泡（不依据事件绑定顺序）
   - 目标元素：依据事件绑定顺序：先绑定的事件先执行（不依据捕获冒泡标准）
   - 最终顺序：父元素捕获->目标元素事件1->目标元素事件2->子元素捕获->子元素冒泡->父元素冒泡
   - 注意：子元素事件执行前提 事件确实“落”到子元素布局区域上，而不是简单的具有嵌套关系

**在一个DOM上同时绑定两个点击事件：一个用捕获，一个用冒泡。事件会执行几次，先执行冒泡还是捕获？**

- 该DOM上的事件如果被触发，会执行两次（执行次数等于绑定次数）
- **如果该DOM是目标元素，则按事件绑定顺序执行，不区分冒泡/捕获**
- **如果该DOM是处于事件流中的非目标元素，则先执行捕获，后执行冒泡**

**如何派发事件(dispatchEvent)？（如何进行事件广播？）**

- W3C: 使用 dispatchEvent 方法
- IE: 使用 fireEvent 方法
```javascript
var fireEvent = function(element, event){
    if (document.createEventObject){
        var mockEvent = document.createEventObject();
        return element.fireEvent('on' + event, mockEvent)
    }else{
        var mockEvent = document.createEvent('HTMLEvents');
        mockEvent.initEvent(event, true, true);
        return !element.dispatchEvent(mockEvent);
    }
}
```
<a name="xhwb2"></a>
### 2. 请解释什么是事件代理

- 事件代理（Event Delegation），又称之为事件委托。“事件代理”即是**把原本需要绑定的事件委托给父元素，让父元素担当事件监听的职务。事件代理的原理是DOM元素的事件冒泡。**

- 可以大量节省内存占用，减少事件注册，提高性能，比如在table上代理所有td的click事件就非常棒
- 动态绑定，可以实现当新增子对象时无需再次对其绑定

**局限性：**<br />（1）有些事件（没有冒泡特性的）不可以用事件代理进行绑定，比如focus、blur<br />（2）**mousemove 、 mouseout 这样的事件，虽然有事件冒泡，但是需要不断计算位置定位，对性能消耗高，也是不适合用事件代理的**<br />（3）如果把所有事件都⽤事件代理，可能会出现事件误判<br />**主要解决的问题：**<br />解决静态的事件绑定问题；减少事件绑定节约内存，提升整体性能<br />**适合用事件代理的事件：**<br />click、mousedown、mouseup、keydown、keyup、keypress
<a name="il3uI"></a>
### 3.  addEventListener()方法的参数和使用
**EventTarget.addEventListener() **方法添加事件监听，**可添加相同指定事件类型的事件侦听器**<br />它的使用语法如下：
```javascript
target.addEventListener(type, listener, options);
target.addEventListener(type, listener, useCapture);
target.addEventListener(type, listener, useCapture, wantsUntrusted);  
```
其中参数如下：<br />**（1）type**<br />表示监听事件类型的字符串。<br />**（2）listener**<br />当所监听的事件类型触发时，会接收到一个事件通知（实现了 Event 接口的对象）对象。listener 必须是一个实现了 EventListener 接口的对象，或者是一个函数。<br />**（3）useCapture  可选**

   1. true - 事件在捕获阶段执行
   2. false- false- 默认。事件在冒泡阶段执行

**（4）options 可选**<br />一个指定有关 listener 属性的可选参数**对象**。可用的选项如下：

- capture:  Boolean，表示 listener 会在该类型的事件捕获阶段传播到该 EventTarget 时触发。
- once:  Boolean，表示 listener 在添加之后最多只调用一次。如果是 true， listener 会在其被调用之后自动移除。
- passive: Boolean，设置为true时，表示 listener 永远不会调用 preventDefault()。如果 listener 仍然调用了这个函数，客户端将会忽略它并抛出一个控制台警告。
- signal：AbortSignal，该 AbortSignal 的 abort() 方法被调用时，监听器会被移除。
<a name="Uyr5u"></a>
### 4. window.onload和$(document).ready
原生JS的window.onload与Jquery的$(document).ready(function(){})有什么不同？

- window.onload()  必须等到页面内包括图片的所有元素和资源加载完毕后才能执行
- $(document).ready()   **DOMContentLoaded** DOM加载完毕后就执行，不必等到整个网页资源加载完毕
如何用原生JS实现Jq的ready方法？```javascript
function ready(fn){
    if(document.addEventListener) {        //标准浏览器
        document.addEventListener('DOMContentLoaded', function() {
            //注销事件, 避免反复触发
            document.removeEventListener('DOMContentLoaded',arguments.callee, false);
            fn();            //执行函数
        }, false);
    }else if(document.attachEvent) {        //IE
        document.attachEvent('onreadystatechange', function() {
            if(document.readyState == 'complete') {
                document.detachEvent('onreadystatechange', arguments.callee);
                fn();        //函数执行
            }
        });
    }
};
```
<a name="yjJ7c"></a>
### 5. addEventListener()和attachEvent()的区别

- addEventListener()是符合W3C规范的标准方法; attachEvent()是IE低版本的非标准方法
- **addEventListener()支持事件冒泡和事件捕获; - 而attachEvent()只支持事件冒泡**
- addEventListener()的第一个参数中,事件类型不需要添加on; attachEvent()需要添加'on'
- 如果为同一个元素绑定多个事件, addEventListener()会按照事件绑定的顺序依次执行, attachEvent()会按照事件绑定的顺序倒序执行
<a name="eJ421"></a>
### 6. js自定义事件
三要素： document.createEvent() event.initEvent() element.dispatchEvent()
```javascript
// (en:自定义事件名称，fn:事件处理函数，addEvent:为DOM元素添加自定义事件，triggerEvent:触发自定义事件)
window.onload = function(){
    var demo = document.getElementById("demo");
    demo.addEvent("test",function(){console.log("handler1")});
    demo.addEvent("test",function(){console.log("handler2")});
    demo.onclick = function(){
        this.triggerEvent("test");
    }
}
Element.prototype.addEvent = function(en,fn){
    this.pools = this.pools || {};
    if(en in this.pools){
        this.pools[en].push(fn);
    }else{
        this.pools[en] = [];
        this.pools[en].push(fn);
    }
}
Element.prototype.triggerEvent  = function(en){
    if(en in this.pools){
        var fns = this.pools[en];
        for(var i=0,il=fns.length;i<il;i++){
            fns[i]();
        }
    }else{
        return;
    }
}
```
<a name="ACPfj"></a>
## 网络请求
<a name="x0HkK"></a>
### 1. 对AJAX的理解，实现一个AJAX请求
Ajax的原理简单来说是在用户和服务器之间加了—个**中间层****(AJAX引擎)**，通过**XmlHttpRequest**对象来向服务器发异步请求，从服务器获得数据，然后用javascript来操作DOM而更新页面对应部分,而不用刷新整个网页。使用户操作与服务器响应异步化。这其中最关键的一步就是从服务器获得请求数据<br />Ajax的过程只涉及JavaScript、XMLHttpRequest和DOM。XMLHttpRequest是ajax的核心机制<br />**优点：**

- **通过异步模式，提升了用户体验.**
- 优化了浏览器和服务器之间的传输，减少不必要的数据往返，减少了带宽占用.
- Ajax可以实现动态不刷新（**局部刷新**）
- Ajax在客户端运行，承担了一部分本来由服务器承担的工作，减少了大用户量下的服务器负载。

**缺点：**

- 安全问题 AJAX暴露了与服务器交互的细节。
- 对搜索引擎的支持比较弱。
- 不容易调试。

创建AJAX请求的步骤：

- **创建一个 XMLHttpRequest 对象。**
- 在这个对象上**使用 open 方法创建一个 HTTP 请求**，open 方法所需要的参数是请求的方法、请求的地址、是否异步和用户的认证信息。
- 在发起请求前，可以为这个对象**添加一些信息和监听函数**。比如说可以通过 **setRequestHeader **方法来为请求添加头信息。还可以为这个对象添加一个状态监听函数。一个 XMLHttpRequest 对象一共有 5 个状态，当它的状态变化时会触发**onreadystatechange **事件，可以通过设置监听函数，来处理请求成功后的结果。当对象的 **readyState **变为 4 的时候，代表服务器返回的数据接收完成，这个时候可以通过判断请求的状态，如果状态是 2xx 或者 304 的话则代表返回正常。这个时候就可以通过 **response **中的数据来对页面进行更新了。当对象的属性和监听函数设置完成后，最后调**用 sent 方法来向服务器发起请求**，可以传入参数作为发送的数据体。
- open()的第三个参数是表示是否异步发送请求的值，如果不填写。默认为true,表示异步发送，
**例子**```javascript
const SERVER_URL = "/server";
let xhr = new XMLHttpRequest();
// 创建 Http 请求
xhr.open("GET", url, true);
// 设置状态监听函数
xhr.onreadystatechange = function() {
  if (this.readyState !== 4) return;
  // 当请求成功时
  if (this.status === 200) {
    handle(this.response);
  } else {
    console.error(this.statusText);
  }
};
// 设置请求失败时的监听函数
xhr.onerror = function() {
  console.error(this.statusText);
};
// 设置请求头信息
xhr.responseType = "json";
xhr.setRequestHeader("Accept", "application/json");
// 发送 Http 请求
xhr.send(null);
```
**使用Promise封装AJAX：**```javascript
// promise 封装实现：
function getJSON(url) {
  // 创建一个 promise 对象
  let promise = new Promise(function(resolve, reject) {
    let xhr = new XMLHttpRequest();
    // 新建一个 http 请求
    xhr.open("GET", url, true);
    // 设置状态的监听函数
    xhr.onreadystatechange = function() {
      if (this.readyState !== 4) return;
      // 当请求成功或失败时，改变 promise 的状态
      if (this.status === 200) {
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };
    // 设置错误监听函数
    xhr.onerror = function() {
      reject(new Error(this.statusText));
    };
    // 设置响应的数据类型
    xhr.responseType = "json";
    // 设置请求头信息
    xhr.setRequestHeader("Accept", "application/json");
    // 发送 http 请求
    xhr.send(null);
  });
  return promise;
}
```
<a name="vnRhb"></a>
### 2. ajax、axios、fetch的区别
Ajax
1. 全称：“异步的 Javascript 和 XML”，XMLHttpRequest 与 != Ajax，
2. **Ajax 是一个技术统称，是一个概念模型，它囊括了很多技术**，它很重要的特性之一就是，动态不刷新、在不更新整个页面的前提下维护数据，对网页的部分DOM进行更新,AJAX在浏览器与Web服务器之间使用异步数据传输（HTTP请求）。
3. 简单来说，Ajax 是一种思想，XMLHttpRequest 只是实现 Ajax 的一种方式
4. 我们使用原生XMLHttpRequest 这种方式实现网络请求时，如果请求内部又包含请求，以此循环，就会出现回调地狱，这也是一个诟病，后来才催生了更加优雅的请求方式。
```javascript
<body>
  <script>
    function ajax(url) {
      const xhr = new XMLHttpRequest();
      xhr.open("get", url, false);
      xhr.onreadystatechange = function () {
        // 异步回调函数
        if (xhr.readyState === 4) {
          if (xhr.status === 200) {
            console.info("响应结果", xhr.response)
          }
        }
      }
      xhr.send(null);
    }
    ajax('https://smallpig.site/api/category/getCategory')
  </script>
</body>
```
Fetch
1. Fetch 是在 ES6 出现的，它使用了 ES6 提出的 promise 对象。是 XMLHttpRequest 的替代品。
2. Fetch 是一个 API，它是真实存在的，它是基于 promise 的。
3. 特点：
   1. 使用 promise，不使用回调函数，**解决了回调地狱的问题**
   2. **采用模块化设计，比如 rep、res 等对象分散开来，比较友好。**
   3. 通过数据流对象处理数据，可以提高网站性能。
4. 和 Ajax 不同，一个是思想，一个是真实存在的 API，
5. 缺点：
- **fetch只对网络请求报错，对400，500都当做成功的请求**，服务器返回 400，500 错误码时并不会 reject，只有网络错误这些导致请求不能完成时，fetch 才会被 reject。
- **fetch默认不会带cookie，需要添加配置项**： fetch(url, {credentials: 'include'})
- **fetch不支持abort，不支持超时控制**，使用setTimeout及Promise.reject的实现的超时控制并不能阻止请求过程继续在后台运行，造成了流量的浪费
- **fetch没有办法原生监测请求的进度**，而XHR可以
```javascript
<body>
  <script>
    function ajaxFetch(url) {
      fetch(url).then(res => res.json()).then(data => {
        console.info(data)
      })
    }
    ajaxFetch('https://smallpig.site/api/category/getCategory')
  </script>
</body>
```
Axios
1. Axios 是一个基于 promise 封装的网络请求库，它是基于 XMLHttpRequests、http 进行二次封装。
2. 特点
   1. 从浏览器中创建 XMLHttpRequests
   2. 从 node.js 创建 http 请求
   3. 支持 Promise API
   4. 拦截请求和响应
   5. 转换请求数据和响应数据
   6. 取消请求
   7. 自动转换 JSON 数据
   8. 客户端支持防御 XSRF
3. axios基于promise，也没有fetch的各种问题，而且体积也较小，现应用比较广范的请求的方式。

更多**（1）AJAX**<br />Ajax 即“AsynchronousJavascriptAndXML”（异步 JavaScript 和 XML），是指一种创建交互式[网页](https://link.zhihu.com/?target=https%3A//baike.baidu.com/item/%25E7%25BD%2591%25E9%25A1%25B5)应用的网页开发技术。它是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。通过在后台与服务器进行少量数据交换，Ajax 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。传统的网页（不使用 Ajax）如果需要更新内容，必须重载整个网页页面。其缺点如下：

- 本身是针对MVC编程，不符合前端MVVM的浪潮
- 基于原生XHR开发，XHR本身的架构不清晰
- 不符合关注分离（Separation of Concerns）的原则
- 配置和调用方式非常混乱，而且基于事件的异步模型不友好。

**（2）Fetch**<br />fetch号称是AJAX的替代品，是在ES6出现的，使用了ES6中的promise对象。Fetch是基于promise设计的。Fetch的代码结构比起ajax简单多。**fetch不是ajax的进一步封装，而是原生js，没有使用XMLHttpRequest对象**。<br />fetch的优点：

- 语法简洁，更加语义化
- 基于标准 Promise 实现，支持 async/await
- 更加底层，提供的API丰富（request, response）
- 脱离了XHR，是ES规范里新的实现方式

fetch的缺点：

- fetch只对网络请求报错，对400，500都当做成功的请求，服务器返回 400，500 错误码时并不会 reject，只有网络错误这些导致请求不能完成时，fetch 才会被 reject。
- fetch默认不会带cookie，需要添加配置项： fetch(url, {credentials: 'include'})
- fetch不支持abort，不支持超时控制，使用setTimeout及Promise.reject的实现的超时控制并不能阻止请求过程继续在后台运行，造成了流量的浪费
- fetch没有办法原生监测请求的进度，而XHR可以

**（3）Axios**<br />Axios 是一种基于Promise封装的HTTP客户端，其特点如下：

- 浏览器端发起XMLHttpRequests请求
- node端发起http请求
- 支持Promise API
- 监听请求和返回
- 对请求和返回进行转化
- 取消请求
- 自动转换json数据
- 客户端支持抵御XSRF攻击
<a name="GFlev"></a>
### <br />
<a name="mxe1Y"></a>
### 3. XMLHttpRequest的发展历程是怎样的？
它最开始只是微软浏览器提供的一个接口，后来各大浏览器纷纷效仿也提供了这个接口，再后来W3C对它进行了标准化，提出了XMLHttpRequest标准。标准又分为Level 1和Level 2。<br />Level 2相对于Level 1做了很大的改进，具体来说是：

- 可以设置HTTP请求的超时时间。
- **可以使用FormData对象管理表单数据。**
- 可以上传文件。
- 可以请求不同域名下的数据（跨域请求）。
- 可以获取服务器端的二进制数据。
- **可以获得数据传输的进度信息。**

<a name="l3cka"></a>
### 4. 如何解决跨域问题?
首先了解下浏览器的同源策略 同源策略/SOP（Same origin policy）是一种约定，由Netscape公司1995年引入浏览器，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，浏览器很容易受到XSS、CSRF等攻击。所谓同源是指**"协议+域名+端口"**三者相同，即便两个不同的域名指向同一个ip地址，也非同源<br />**那么怎样解决跨域问题的呢？**

- **通过jsonp跨域**
   - 有些标签 script、img、link、iframe ... 这些标签不存在跨域请求的限制，就是利用这个特点解决跨域问题。
   - 只支持 get 请求，不支持 post 请求，导致数据不安全
   - 核心思想：网页通过添加一个 <script> 元素，向服务器请求 JSON 数据，服务器收到请求后，将数据放在一个指定名字的回调函数的参数位置传回来。
- **nginx反向代理**
   - 既然不能跨域请求，那么我们不跨域就可以了，通过在请求到达服务器前部署一个服务，将接口请求进行转发，这就是反向代理。通过一定的转发规则可以将前端的请求转发到其他的服务。
   - 通过反向代理我们将前后端项目统一通过反向代理来提供对外的服务，这样在前端看上去就跟不存在跨域一样。
   - 反向代理麻烦之处就在原对 Nginx 等反向代理服务的配置，在目前前后端分离的项目中很多都是采用这种方式。
- **CORS 跨域资源共享（Cross-Origin Resource Sharing）**
   - 这种方式最主要的特点就是会在**响应的 HTTP 首部增加 Access-Control-Allow-Origin 等信息**，从而判定那些资源站可以进行跨域请求，还有几个其他相关的 HTTP 首部进行更加精细化的控制，最主要的还是 Access-Control-Allow-Origin 。
   - 在真正的发送跨域请求之前会发送一个试探性请求（OPTIONS），服务器接收到 OPTIONS请求之后，做一个处理，返回成功，表示可以发送跨域请求，再发送真正的跨域请求
- **nodejs中间件代理跨域**
- **后端在头部信息里面设置安全域名**

<a name="J0RGp"></a>
### 5. 为什么要有同源限制？

- 同源策略指的是：协议，域名，端口相同，同源策略是一种安全协议
- 设置同源策略的主要目的是为了安全，如果没有同源限制，**在浏览器中的cookie等其他数据可以任意读取，不同域下的DOM任意操作，ajax任意请求其他网站的数据，包括隐私数据**。
- 举例说明：比如一个黑客程序，他利用Iframe把真正的银行登录页面嵌到他的页面上，当你使用真实的用户名，密码登录时，他的页面就可以通过Javascript读取到你的表单中input中的内容，这样用户名，密码就轻松到手了。
<a name="Uum67"></a>
### 6. 对JSON的理解
JSON 是一种基于文本的轻量级的数据交换格式。它可以被任何的编程语言读取和作为数据格式来传递。在项目开发中，**可使用 JSON 作为前后端数据交换的方式**。 

因为 JSON 的语法是基于 js 的，因此很容易将 JSON 和 js 中的对象弄混，但是应该注意的是 JSON 和 js 中的对象不是一回事，JSON 中对象格式更加严格，**比如说在 JSON 中属性值不能为函数，不能出现 NaN 这样的属性值**等，因此大多数的 js 对象是不符合 JSON 对象的格式的。

在 js 中提供了两个函数来实现 js 数据结构和 JSON 格式的转换处理，

- **JSON.stringify 函数**，通过传入一个符合 JSON 格式的数据结构，将其转换为一个 JSON 字符串。如果传入的数据结构不符合 JSON 格式，那么在序列化的时候会对这些值进行对应的特殊处理，使其符合规范。在前端向后端发送数据时，可以调用这个函数将数据对象转化为 JSON 格式的字符串。
- **JSON.parse() 函数**，这个函数用来将 JSON 格式的字符串转换为一个 js 数据结构，如果传入的字符串不是标准的 JSON 格式的字符串的话，将会抛出错误。当从后端接收到 JSON 格式的字符串时，可以通过这个方法来将其解析为一个 js 数据结构，以此来进行数据的访问。
<a name="pdiJO"></a>
## 编码优化
<a name="xf5p6"></a>
###  1. [防抖节流](https://blog.csdn.net/qq_42357338/article/details/107864044?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166538823516782414942631%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166538823516782414942631&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-107864044-null-null.142^v52^new_blog_pos_by_title,201^v3^add_ask&utm_term=%E9%98%B2%E6%8A%96%E8%8A%82%E6%B5%81%E5%8E%9F%E7%90%86&spm=1018.2226.3001.4187)
<a name="UCnIo"></a>
#### 防抖

- 函数防抖 是指在事件被触发 n 秒后再执行回调，如果在这 n 秒内事件又被触发，则重新计时。~~这可以使用在一些点击请求的事件上，避免因为用户的多次点击向后端发送多次请求。~~

`防抖`  ：用户前面一定时间内的连续触发都被取消,最后一次执行在规定的时间之后才会触发,也就是说如果连续快速的触发只会执行一次<br />`特点`：用户操作很频繁,但是只是执行一次 
**设计思路**：：使用定时器做时间防抖。 当触发一个事件时，先用 setTimout 让这个事件延迟一小段时间再执行。 如果在这个时间间隔内又触发了事件，就 clearTimeout 原来的定时器， 再 setTimeout 一个新的定时器重复以上流程。```javascript
function debounce(fn, wait) {
    var timer = null;
    return function () {
        // 如果此时存在定时器的话，则取消之前的定时器重新记时
        if (timer) {
            clearTimeout(timer);
            timeout = null
        }
        args = arguments;
        // 设置定时器，使事件间隔指定事件后执行
        timer = setTimeout(() => {
            //fn.apply(context, args);
            fn.apply(this, args);
        }, wait);
    };
}

//immediate表示是否立即执行
function debounce(func, delay, immediate) {
  // result表示返回值
  let timeout, result;
  return function () {
    let valArr = arguments;
    clearTimeout(timeout);
    // 判断是否立即执行，如果不传默认不立即执行
    if (immediate) {
      // 立即执行
      let callNow = !timeout;
      timeout = setTimeout(() => {
        timeout = null;
      }, delay);
      if (callNow) result = func.apply(this, valArr);
    } else {
      timeout = setTimeout(() => {
        func.apply(this, valArr);
      }, delay);
    }
    return result;
  };
}
```
<a name="BjaX2"></a>
#### 节流

- 函数节流 是指规定一个单位时间，在这个单位时间内，只能有一次触发事件的回调函数执行，如果在同一个单位时间内某事件被触发多次，只有一次能生效。~~节流可以使用在 scroll 函数的事件监听上，通过事件节流来降低事件调用的频率。~~

`节流`：在规定的间隔时间范围内只能有一次触发事件的回调函数执行，不会重复触发回调,只有大于这个时间间隔才会再次触发回调,把频繁触发变为少量触发<br />`特点`：用户操作很频繁,但是把频繁的操作变为少量操作【可以给浏览器有充裕的时间解析代码】
**设计思路（定时器）**：我们可以设计一种类似控制阀门一样定期开放的函数，事件触发时让函数执行一次，然后关闭这个阀门，过了等待时间（定时器回调执行完）后再将这个阀门打开，再次触发事件。```javascript
function throttle(callback, wait) {
    let flag = true;
    return function () {
        if (flag) {
            flag = false;
            let valArr = arg	uments;
            setTimeout(() => {
                callback.apply(this, valArr);
                flag = true;
            }, wait);
        }
    };
}
```
**设计思路（Date.now()）**：定义两次触发的时间和计算两次触发的时间间隔，如果两次触发的时间间隔超过了指定时间，则执行函数。```javascript
function throttle(fn, delay) {
  var preTime = 0
  return function () {
    let nowTime = Date.now();
    // 如果两次时间间隔超过了指定时间，则执行函数。
    if (nowTime - preTime >= delay) {
      preTime = Date.now();
      return fn.apply(this, arguments);
    }
  };
}
```

**防抖和节流的区别**<br />防抖类似于玩一个游戏：如果小冬站着不动，倒计时 10s 就可以吃苹果🍎，但是如果他这 10s 内动了，那么就重新计时，直到啥时候它坚持 10s 内不动才能吃到苹果。防抖类似于规定10s内能且只能吃一个苹果

**应用场景**<br />防抖

1. **按钮提交场景**，比如点赞，表单提交等，防止多次提交
2. **调整浏览器窗口大小时**，resize 次数过于频繁，造成计算过多。
3. 文本编辑器实时保存，当无任何更改操作一秒后进行保存。
4. ~~DOM 元素的拖拽功能实现。~~

节流

1. **scroll 事件**，每隔一秒计算一次位置信息、加载更多等。
2. DOM元素拖拽功能实现
3. **鼠标不断点击触发**，mousedown(单位时间内只触发一次)，抢购和疯狂点击等会用到节流
4. **搜索框，搜索联想功能**
5. 浏览器播放事件，每个一秒计算一次进度信息等
6. 高频率触发的事件,在指定的单位时间内，只响应第一次。
<a name="AbD0k"></a>
### 2. 说几条写JavaScript的基本规范

- 请使用===/!==来比较true/false或者数值
- If语句必须使用大括号
- 语句结束使用分号;
- 变量和函数在使用前进行声明
- 以大写字母开头命名构造函数，全大写命名常量
- Switch语句必须带有default分支
- 代码段使用花括号{}包裹
- 不要在同一行声明多个变量
- 使用对象字面量替代new Array这种形式
- 不要使用全局函数
- for-in循环中的变量 应该使用var关键字明确限定作用域，从而避免作用域污染
- 代码缩进，建议使用“四个空格”缩进
- 规范定义JSON对象，补全双引号
- 用{}和[]声明对象和数组
<a name="p0LBu"></a>
### 3. 如何编写高性能的JavaScript

1. 遵循严格模式："use strict";
2. 尽量减少使用闭包
3. 最小化重绘(repaint)和回流(reflow)
4. 将js脚本放在页面底部，加快渲染页面，使用非阻塞方式下载js脚本
5. 缓存(减少) DOM 节点的访问
6. 尽量使用局部变量来保存全局变量
7. 将js脚本将脚本成组打包，减少请求
8. 使用 window 对象属性方法时，省略 window
9. 尽量减少对象成员嵌套
10. 通过避免使用 eval() 和 Function() 构造器
11. 给 setTimeout() 和 setInterval() 传递函数而不是字符串作为参数
12. 尽量使用直接量创建对象和数组
<a name="dVGCJ"></a>
### 4. 图片的懒加载和预加载

- **预加载：提前加载图片**，当用户需要查看时可直接从本地缓存中渲染。
- **懒加载**：懒加载的主要目的是作为服务器前端的优化，**减少请求数或延迟请求数**

两种技术的本质：两者的行为是相反的，一个是提前加载，一个是迟缓甚至不加载。懒加载对服务器前端有一定的缓解压力作用，预加载则会增加服务器前端压力。
<a name="baNhK"></a>
### 5. 项目做过哪些性能优化？

1. 减少 HTTP 请求数
2. 图片懒加载
3. 减少 DOM 元素数量，减少DOM 操作
4. 使用 CDN
5. 减少 DNS 查询
6. 避免重定向
7. 使用外部 JavaScript 和 CSS
8. 压缩 JavaScript 、 CSS 、字体、图片等
9. 优化 CSS Sprite
10. 使用 iconfont
11. 字体裁剪
12. 多域名分发划分内容到不同域名
13. 尽量减少 iframe 使用
14. 避免图片 src 为空
15. 把样式表放在link 中
16. 把JavaScript放在页面底部
<a name="d2dH3"></a>
## 基础
<a name="FbH0J"></a>
### 1.   延迟加载JS有哪些方式？
```javascript
<script async src="./js/js"></script>
<script defer src="./js/js"></script>
```

1. `**async**`**  **遇到scirpt标签开始通知浏览器异步下载，**下载完毕之后，就可以立即执行**。 **不是顺次执行**脚本（谁先加载完谁先执行) 。
2. `**defer**`**  **遇到scirpt标签时，浏览器开始异步下载，**当 html 加载完毕，开始执行js文件** 。并且js文件是**按顺序执行的。**
3. **把js文件放在最后**: 当外部加载js文件时，应该将js脚本放在最后，当全部的文件都加载完成后，再开始加载执行js脚本。
4. **使用setTimeout**: 在每一个脚本文件最外层设置一个定时器。
5. 动态创建DOM方式
**动态创建DOM方式**```javascript
动态创建script标签，当页面的全部内容加载完毕后，在执行创建挂载。
<script>
function loadJS() {
    let element = document.createElement("script")
    element.src = "download.js"
     document.body.appendChild(element)
}

if(window.addEventListener) {
    window.addEventListener("load", loadJS, false)
}else if(window.attachEvent) {
    window.attachEvent("onload", loadJS)
}else {
    window.onload = loadJS
}
</script>
```
s![image.png](https://cdn.nlark.com/yuque/0/2022/png/28203632/1662362186380-0ee2e6f8-3602-48de-8821-c428e64b492c.png#averageHue=%23edecec&clientId=uc9ad66c5-ad7b-4&errorMessage=unknown%20error&from=paste&height=238&id=kfPHt&originHeight=238&originWidth=272&originalType=binary&ratio=1&rotation=0&showTitle=false&size=36306&status=error&style=none&taskId=uc41a2503-e763-499c-ae82-17c1a8a8dba&title=&width=272)![image.png](https://cdn.nlark.com/yuque/0/2022/png/28203632/1662362206095-ae15b1b6-427e-4270-af13-80cc6e4cf71e.png#averageHue=%23c9c9c6&clientId=uc9ad66c5-ad7b-4&errorMessage=unknown%20error&from=paste&height=415&id=yChNP&originHeight=415&originWidth=956&originalType=binary&ratio=1&rotation=0&showTitle=false&size=30011&status=error&style=none&taskId=u19479b26-e669-4d63-99a5-488d20dabb0&title=&width=956)

**异步加载库 LABjs**<br />**模块加载器 Sea.js**
<a name="f9s2Y"></a>
### 2. sort背后原理是什么？

- sort方法用于对数组的元素进行排序，并返回数组。默认排序顺序是根据字符串Unicode码点。
- 语法：array..sort(sortby);参数sortby可选。规定排序顺序。必须是函数
- 注：如果调用该方法时没有使用参数，将按照字符编码的顺序进行排序。要实现这一点，首先应把数组的元素都转换成字符串(如有必要)，以便进行比较。

如果指明了 compareFunction参数 ，那么数组会按照调用该函数的返回值排序，即 a和 b是两个将要被比较的元素：
```javascript
arr2.sort((a,b)=>a-b);
```

1. 如果 compareFunction（a, b）小于 0，那么 a 会被排列到 b 之前；
2. 如果 compareFunction（a, b）等于 0，a和 b 的相对位置不变；
3. 如果 compareFunction（a, b）大于 0，b会被排列到 a之前。

 实现原理 sort结合了快速排序.堆排序.直接插入排序三种排序方法. 根据不同的数量级别以及不同情况,能自动选用合适的排序方法.当数据量较大时采用快速排序,分段递归.一旦分段后的数据量小于某个阀值,为避免递归调用带来过大的额外负荷,便会改用插入排序.而如果递归层次过深,有出现最坏情况的倾向,还会改用堆排序.
<a name="Be9Zm"></a>
### 3. [localstorage、sessionstorage、cookie的区别](https://blog.csdn.net/qq_42485707/article/details/121564189)
公共点：在客户端存放数据<br />**区别**

1. **数据存放有效期 **
   1. sessionStorage：仅在当前浏览器窗口关闭之前有效。【关闭浏览器就没了】
   2. localStorage ：始终有效，窗口或者浏览器关闭也一直保存，所以叫持久化存储。
   3. cookie : 只在设置的cookie过期时间之前有效，即使窗口或者浏览器关闭也有效。
2. **localStorage、sessionStorage不可以设置过期时间 **
   1. cookie有过期时间，可以设置过期（把时间调整到之前的时间，就过期了)
3. **存储大小的限制 **
   1. cookie存储量不能超过4k
   2. localStorage、sessionStorage不能超过5M
   3. 根据不同的浏览器存储的大小是不同的。
4. **与服务端的通信**
   1. cookie会参与到与服务端的通信中，一般会携带在同源的http请求的头部中，即cookie在浏览器和服务器间来回传递，例如一些关键密匙验证等。
   2. localStorage和sessionStorage是单纯的前端存储，不参与与服务端的通信
5. **作用域不同**
   1. sessionStorage不在不同的浏览器窗口中共享
   2. localStorage和cookie在所有同源窗口中都是共享的
6. **应用场景：**
   1. cookie：不能跨域请求！验证登录、判断是否登陆过网站、**保存上次登录的信息、保存上次查看的页面**、浏览计数器	
   2. localStorage：跨页面传递参数，长期登录，长期保存在本地的数据，数据永久保存除非手动删
   3. sessionStorage：临时保存数据，页面刷新，敏感账号一次性登录
7. **web Storage支持事件通知机制（ storage 事件），可以将数据更新的通知发送给监听者**

![image.png](https://cdn.nlark.com/yuque/0/2022/png/28203632/1665324508131-1d1ddd02-3685-42e8-ab73-c1f4d01a5655.png#averageHue=%23faf9f8&clientId=uceb09f59-a883-4&errorMessage=unknown%20error&from=paste&height=330&id=klIEA&originHeight=330&originWidth=830&originalType=binary&ratio=1&rotation=0&showTitle=false&size=34831&status=error&style=stroke&taskId=uc26ad6d2-bade-4abc-b87d-6f179772fbc&title=&width=830)

8. **cookie数据还有路径的概念，可以限制cookie只属于某个路径下**

**sessionStorage与页面 js 数据对象的区别**<br />页面中一般的 js 对象或数据的生存期是仅在当前页面有效，因此刷新页面或转到另一页面这样的重新加载页面的情况，数据就不存在了。<br />而 sessionStorage 只要同源的同窗口（或tab）中，刷新页面或进入同源的不同页面，数据始终存在。也就是说只要这个浏览器窗口没有关闭，加载新页面或重新加载，数据仍然存在。
<a name="EXZbs"></a>
### 4. 常用的正则表达式有哪些？
```javascript
// （1）匹配 16 进制颜色值
var regex = /#([0-9a-fA-F]{6}|[0-9a-fA-F]{3})/g;

// （2）匹配日期，如 yyyy-mm-dd 格式
var regex = /^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[12][0-9]|3[01])$/;

// （3）匹配 qq 号
var regex = /^[1-9][0-9]{4,10}$/g;

// （4）手机号码正则
var regex = /^1[34578]\d{9}$/g;

// （5）用户名正则
var regex = /^[a-zA-Z\$][a-zA-Z0-9_\$]{4,16}$/;
```
<a name="QFhS3"></a>
### 5. JavaScript为什么要进行变量提升，它导致了什么问题？

1.  js 引擎在代码执行前，JS会检查语法，并对函数进行预编译，解析时会创建了执行上下文，初始化变量对象，把代码中即将执行的变量、函数声明都拿出来，变量先赋值为undefined，函数先声明好可使用，
2. **在执行阶段**当访问一个变量时，会到当前执行上下文中的作用域链中去查找，而作用域链的首端指向的是当前执行上下文的变量对象，

首先要知道，JS在拿到一个变量或者一个函数的时候，会有两步操作，即解析和执行。

-  **在解析阶段**，JS会检查语法，并对函数进行预编译。解析的时候会先创建一个全局执行上下文环境，先把代码中即将执行的变量、函数声明都拿出来，变量先赋值为undefined，函数先声明好可使用。在一个函数执行之前，也会创建一个函数执行上下文环境，跟全局执行上下文类似，不过函数执行上下文会多出this、arguments和函数的参数。 
   - 全局上下文：变量定义，函数声明
   - 函数上下文：变量定义，函数声明，this，arguments
-  **在执行阶段**，就是按照代码的顺序依次执行。 

那为什么会进行变量提升呢？主要有以下两个原因：

- 提高性能
- 容错性更好

**（1）提高性能**<br />在JS代码执行之前，会进行语法检查和预编译，在预编译时，会统计声明了哪些变量、创建了哪些函数，并对函数的代码进行压缩，去除注释、~~不必要的空白~~等（并且这一操作只进行一次）。好处就是每次执行函数时都可以直接为该函数分配栈空间，。~~这么做就是为~~提高了性能，如果没有这一步，那么每次执行代码前都必须重新解析一遍该变量（函数），而这是没有必要的，因为变量（函数）的代码并不会改变，解析一遍就够了。<br />在~~解析的过程中，还会为函数生成预编译代码。~~在预编译时，会统计声明了哪些变量、创建了哪些函数，并对函数的代码进行压缩，去除注释、不必要的空白等。这样做的好处就是每次执行函数时都可以直接为该函数分配栈空间（不需要再解析一遍去获取代码中声明了哪些变量，创建了哪些函数），并且因为代码压缩的原因，代码执行也更快了。<br />**（2）容错性更好**<br />变量提升可以在一定程度上提高JS的容错性，看下面的代码：
```javascript
a = 1;
var a;
console.log(a);
```
如果没有变量提升，这两行代码就会报错，但是因为有了变量提升，这段代码就可以正常执行。<br />**总结：**

- 解析和预编译过程中的声明提升可以提高性能，让函数可以在执行时预先为变量分配栈空间
- 声明提升还可以提高JS代码的容错性，使一些不规范的代码也可以正常执行

变量提升虽然有一些优点，但是他也会造成一定的问题，**在ES6中提出了let、const来定义变量，它们就没有变量.提升的机制。**
**下面看一下变量提升可能会导致的问题：**在这个函数中，原本是要打印出外层的tmp变量，但是因为变量提升的问题，内层定义的tmp被提到函数内部的最顶部，相当于覆盖了外层的tmp，所以打印结果为undefined。
```javascript
var tmp = new Date();

function fn(){
	console.log(tmp);
	if(false){
		var tmp = 'hello world';
	}
}

fn();  // undefined
```

由于遍历时定义的i会变量提升成为一个全局变量，在函数结束之后不会被销毁，所以打印出来11。
```javascript
var tmp = 'hello world';

for (var i = 0; i < tmp.length; i++) {
	console.log(tmp[i]);
}

console.log(i); // 11
```
<a name="PADYA"></a>
### 6. 严格模式
use strict是一种ECMAscript 5 添加的（严格）运行模式,这种模式使得 Javascript 在更严格的条件下运行,使JS编码更加规范化的模式,消除Javascript语法的一些不合理、不严谨之处~~，减少一些怪异行为~~

1. 全局的严格模式
2. 局部的严格模式
3. `"use strict"` 写在js代码题上面
4. 局部的严格模式要执行函数才报错

**严格模式的规则：**<br />变量

1. 无法意外创建全局变量
2. 禁止删除声明变量
3. 在严格模式中一部分字符变成了保留的关键字。如, eval、arguments、interface, let...

对象

1. 出现与对象属性描述符不符合的操作的时候报错,（静默失败）
   1. delete不可删除属性(isSealed或isFrozen)时报错
   2. 给不可扩展对象的新属性赋值
2. 无法给原始值(NaN)设置属性

函数

1. 函数的参数名唯一
2. 函数参数与arguments间不存在映射关系
3. arguments.callee   函数名.caller
4. 函数的默认this指向undefined
5. 使用call, apply,bind来指定的this不会被强制转换为对象（传入null和undefined，this将是Window）
6. eval 不再为上层范围引入新变量

其它

1. 禁止0前缀表示八进制数字（0o）
2. 禁用 with(改变上下文)，

严格模式对正常的 JavaScript语义做了一些更改。

1. 通过**抛出错误**来消除了一些原有`静默错误`。
2. 修复了一些导致 JavaScript引擎难以执行`优化的缺陷`
3. **禁用了**ECMAScript的`未来版本`中可能会定义的一些语法。为未来新版本的Javascript做好铺垫

[MDN例子](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode#%E4%B8%A5%E6%A0%BC%E6%A8%A1%E5%BC%8F%E4%B8%AD%E7%9A%84%E5%8F%98%E5%8C%96)<br />[bili](https://www.bilibili.com/video/BV1154y157UW?p=3&spm_id_from=pageDriver)<br />**静态绑定**

- 严格模式下，禁止使用with语句，使用with语句将报错。因为with语句无法在编译时就确定，某个属性到底归属哪个对象，从而影响了编译效果
- 严格模式创设了第三种作用域：eval作用域。eval语句本身就是一个作用域，不再能够在其所运行的作用域创设新的变量了，也就是说，eval所生成的变量只能用于eval内部。
<a name="p5yuU"></a>
### 7. 强类型语言和弱类型语言的区别

- **强类型语言**：强类型语言也称为强类型定义语言，是一种总是强制类型定义的语言，要求变量的使用要严格符合定义，所有变量都必须先定义后使用。Java和C++等语言都是强制类型定义的，也就是说，一旦一个变量被指定了某个数据类型，如果不经过强制转换，那么它就永远是这个数据类型了。例如你有一个整数，如果不显式地进行转换，你不能将其视为一个字符串。
- **弱类型语言**：弱类型语言也称为弱类型定义语言，与强类型定义相反。JavaScript语言就属于弱类型语言。简单理解就是一种变量类型可以被忽略的语言。比如JavaScript是弱类型定义的，在JavaScript中就可以将字符串'12'和整数3进行连接得到字符串'123'，在相加的时候会自动类型转换。

两者对比：强类型语言在速度上可能略逊色于弱类型语言，但是强类型语言带来的严谨性可以有效地帮助避免许多错误。
<a name="n7JI2"></a>
### 8. 解释性语言和编译型语言的区别
（1）解释型语言<br />使用专门的解释器对源程序逐行解释成特定平台的机器码并立即执行。是代码在执行时才被解释器一行行动态翻译和执行，而不是在执行之前就完成翻译。解释型语言不需要事先编译，其直接将源代码解释成机器码并立即执行，所以只要某一平台提供了相应的解释器即可运行该程序。其特点总结如下

- 解释型语言每次运行都需要将源代码解释称机器码并执行，效率较低；
- 只要平台提供相应的解释器，就可以运行源代码，所以可以方便源程序移植；
- JavaScript、Python等属于解释型语言。

（2）编译型语言

1. 编译型程序是面向特定平台的因而是平台依赖的。

使用专门的编译器，针对特定的平台，将高级语言源代码一次性的编译成可被该平台硬件执行的机器码，并包装成该平台所能识别的可执行性程序的格式。在编译型语言写的程序执行之前，需要一个专门的编译过程，把源代码编译成机器语言的文件，如exe格式的文件，以后要再运行时，直接使用编译结果即可，如直接运行exe文件。因为只需编译一次，以后运行时不需要编译，所以编译型语言执行效率高。其特点总结如下：

- 一次性的编译成平台相关的机器语言文件，运行时脱离开发环境，运行效率高；
- 与特定平台相关，一般无法移植到其他平台；
- C、C++等属于编译型语言。

**两者主要区别在于：**前者源程序编译后即可在该平台运行，后者是在运行期间才编译。所以前者运行速度快，后者跨平台性好。<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/28203632/1665374595773-667f29c8-2aee-442d-a6ca-eeb58b16a513.png#averageHue=%23f7f7f7&clientId=ua2e65bab-5242-4&errorMessage=unknown%20error&from=paste&id=L8Qf0&originHeight=752&originWidth=1620&originalType=url&ratio=1&rotation=0&showTitle=false&size=195354&status=error&style=stroke&taskId=ufc5eb067-71e1-4712-ba71-e0042051c8c&title=)

<a name="mXr4a"></a>
### 9. ？？？模块化开发怎么做？
 模块化就是讲js文件按照功能分离，根据需求引入不同的文件中

- 立即执行函数,不暴露私有成员
- 块化开发在现代开发中已是必不可少的一部分，它大大提高了项目的可维护、可拓展和可协作性。通常，我们 在浏览器中使用 ES6 的模块化支持，在 Node 中使用 commonjs 的模块化支持。

**分类:**

- es6: import / export
- commonjs: require / module.exports / exports
- amd: require / defined

**require与import的区别**

- require支持 动态导入，import不支持，正在提案 (babel 下可支持)
- require是 同步 导入，import属于 异步 导入
- require是 值拷贝，导出值变化不会影响导入值；import指向 内存地址，导入值会随导出值而变化
```javascript
var module1 = (function(){
    var _count = 0;
    var m1 = function(){
        //...
    };
    var m2 = function(){
        //...
    };
    return {
        m1 : m1,
        m2 : m2
    };
})();

```
<a name="HIsp6"></a>
### 10. eval是做什么的

- 把[字符串](https://so.csdn.net/so/search?q=%E5%AD%97%E7%AC%A6%E4%B8%B2&spm=1001.2101.3001.7020)参数解析成JS代码并运行，并返回执行的结果；
- 应该避免使用eval，不安全，非常耗性能（2次，一次解析成js语句，一次执行）
- 由JSON字符串转换为JSON对象的时候可以用eval，var obj =eval('('+ str +')')
<a name="LHDJ5"></a>
### 12. ["1", "2", "3"].map(parseInt) 答案是多少
如果 radix 是 undefined、0 或未指定的，JavaScript 会假定为 10 或 16
1. 如果输入的 string 以 0x 或 0X（一个 0，后面是小写或大写的 X）开头，那么 radix 被假定为 16，字符串的其余部分被当做十六进制数去解析。
2. 如果输入的 string 以 "0"（0）开头，radix 被假定为 8（八进制）或 10（十进制）。具体选择哪一个 radix 取决于实现。ECMAScript 5 澄清了应该使用 10 (十进制)，但不是所有的浏览器都支持。因此，在使用 parseInt 时，一定要指定一个 radix。
3. 如果输入的 string 以任何其他值开头，radix 是 10 (十进制)。
- [1, NaN, NaN]因为 parseInt 需要两个参数 (val, radix)，其中radix 表示解析时用的基数（radix 是 2-36 之间的整数）。
- map传了 3个(element, index, array)，对应的 radix 不合法导致解析失败。
<a name="o0l7w"></a>
### 13. 同步和异步的区别

- 同步：浏览器访问服务器请求，用户看得到页面刷新，重新发请求,等请求完，页面刷新，新内容出现，用户看到新内容,进行下一步操作
- 异步：浏览器访问服务器请求，用户正常操作，浏览器后端进行请求。等请求完，页面不刷新，新内容也会出现，用户看到新内容
<a name="MULeV"></a>
### 14. 渐进增强和优雅降级

- **渐进增强 ：针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。**
- **优雅降级 ：针对一个高版本的浏览器构建页面，一开始就构建完整的功能，然后再针对低版本浏览器进行兼容，保证低级浏览器也有基本功能 就好**
css3中-moz、-ms、-webkit分别代表的意思**-moz**代表firefox浏览器私有属性<br />**-ms**代表ie浏览器私有属性（360浏览器是ie内核）<br />**-webkit**代表safari、chrome私有属性<br />**-o**代表opera私有属性<br />这三个是厂商前缀,不同浏览的厂商,因为不同浏览器有不同的标准，所以为了兼容性，需要把常用的浏览器对应的厂商前缀加上。所以四个属性代表的是一个意思
```javascript
//通常最好的做法是先指定供应商前缀的版本，然后再指定非前缀版本，以使非前缀属性在实现后将覆盖卖方前缀的属性设置
.transition { /*渐进增强写法*/
  -webkit-border-radius:30px 10px;
  -moz-border-radius:30px 10px;
  border-radius:30px 10px;
}

.transition { /*优雅降级写法*/
  border-radius:30px 10px;
  -moz-border-radius:30px 10px;
  -webkit-border-radius:30px 10px;
}
```
<a name="QjjwY"></a>
###  15. [attribute和property的区别是什么](https://zhuanlan.zhihu.com/p/25913115)
含义区别

- attribute是dom元素在文档中**作为html标签拥有的特性**；attribute特性的类型总是字符串类型
- property就是dom元素在js中**作为对象拥有的属性**。property属性可以是任意类型。

取值和赋值区别

- getAttribute/setAttribute
- .  /  []
- 除了value属性之外，非自定义的attribute特性与property有1:1的映射关系，attribute和property是同步的，是会自动更新的
- 但是对于自定义的属性来说，他们是不同步的
<a name="bsoYl"></a>
### 16. 谈一谈你理解的函数式编程
函数式编程的本质是：把函数看作是数据。

- 简单说，"函数式编程"是一种"编程范式"（programming paradigm），也就是如何编写程序的方法论
- 它具有以下特性：闭包和高阶函数、惰性计算、递归、函数是"第一等公民"、只用"表达式"
<a name="KoNTA"></a>
### 17. 对原生Javascript了解程度

- 数据类型、运算、对象、Function、继承、闭包、作用域、原型链、事件、RegExp、JSON、Ajax、DOM、BOM、内存泄漏、模块化、跨域、异步装载、模板引擎、前端MVC、路由、Canvas、ECMAScript
<a name="FtVAS"></a>
### 18. Js动画与CSS动画区别及相应实现
CSS 动画
- 优点
   - 在性能上会稍微好一些，浏览器会对CSS3的动画做一些优化对于帧速不好的浏览器，css3可以做到自动降级，js则需要添加额外的代码。
   - 代码相对简单
- 缺点
   - 在动画控制上不够灵活
   - 兼容性不好
javaScrip 
- 优点
   - 控制能力很强，可以帧数的控制、变换，兼容强。js 能够让动画，暂停，取消，终止，css动画不能添加事件,对于一些复杂控制的动画，使用javascript会比较好
- 缺点
   - **javascript在浏览器的主线程中运行，而主线程中还存在其他需要运行的javascript脚本，样式计算、布局、绘制任务等,对其干扰导致线程可能出现阻塞，从而造成丢帧的情况。优化 requestAnimationFrame**
   - 在实现一些小的交互动效的时候，考虑CSS
1. 代码复杂度方面：
   1. 简单动画，css 代码实现会简单一些，js 复杂一些。
   2. 复杂动画的话，css 代码就会变得冗长，js实现起来更优。
2. 动画运行时：
   1. css动画不能添加事件，只能设置固定节点进行什么样的过渡动画。
   2. 对动画的控制程度上，js 比较灵活，能控制动画暂停，取消，终止等；
3. 兼容方面：
   1. css 有浏览器兼容问题，js 大多情况下是没有的。
4. 性能方面
   1. css动画相对于优一些，css 动画通过GUI解析；
   2. js 动画需要经过j s 引擎代码解析，然后再进行 GUI 解析渲染。
   3. js是逐帧动画，每一帧都是由代码控制，操作不当，极易引发回流 ，浏览器可以对css动画进行优化，其优化原理类似于requestAnimationFrame，会把每一帧的DOM操作都集中起来，在一次重绘和回流中去完成。一般来说频率为每秒60帧。

<a name="OtjIc"></a>
### 19. Js全局函数和全局变量
**全局变量**

- Infinity 代表正的无穷大的数值。
- NaN 指示某个值是不是数字值。
- undefined 指示未定义的值。

**全局函数**

- decodeURI() 解码某个编码的 URI。
- decodeURIComponent() 解码一个编码的 URI 组件。
- encodeURI() 把字符串编码为 URI。
- encodeURIComponent() 把字符串编码为 URI 组件。
- escape() 对字符串进行编码。
- eval() 计算 JavaScript 字符串，并把它作为脚本代码来执行。
- isFinite() 检查某个值是否为有穷大的数。
- isNaN() 检查某个值是否是数字。
- Number() 把对象的值转换为数字。
- parseFloat() 解析一个字符串并返回一个浮点数。
- parseInt() 解析一个字符串并返回一个整数。
- String() 把对象的值转换为字符串。
- unescape() 对由escape() 编码的字符串进行解码

<a name="qSfBE"></a>
### ？？？20. 浏览器缓存
浏览器缓存分为强缓存和协商缓存。当客户端请求某个资源时，获取缓存的流程如下

- 先根据这个资源的一些 http header 判断它是否命中强缓存，如果命中，则直接从本地获取缓存资源，不会发请求到服务器；
- 当强缓存没有命中时，客户端会发送请求到服务器，服务器通过另一些request header验证这个资源是否命中协商缓存，称为http再验证，如果命中，服务器将请求返回，但不返回资源，而是告诉客户端直接从缓存中获取，客户端收到返回后就会从缓存中获取资源；
- 强缓存和协商缓存共同之处在于，如果命中缓存，服务器都不会返回资源； 区别是，强缓存不对发送请求到服务器，但协商缓存会。
- 当协商缓存也没命中时，服务器就会将资源发送回客户端。
- 当 ctrl+f5 强制刷新网页时，直接从服务器加载，跳过强缓存和协商缓存；
- 当 f5刷新网页时，跳过强缓存，但是会检查协商缓存；

**强缓存**

- Expires（该字段是 http1.0 时的规范，值为一个绝对时间的 GMT 格式的时间字符串，代表缓存资源的过期时间）
- Cache-Control:max-age（该字段是 http1.1的规范，强缓存利用其 max-age 值来判断缓存资源的最大生命周期，它的值单位为秒）

**协商缓存**

- Last-Modified（值为资源最后更新时间，随服务器response返回）
- If-Modified-Since（通过比较两个时间来判断资源在两次请求期间是否有过修改，如果没有修改，则命中协商缓存）
- ETag（表示资源内容的唯一标识，随服务器response返回）
- If-None-Match（服务器通过比较请求头部的If-None-Match与当前资源的ETag是否一致来判断资源是否在两次请求之间有过修改，如果没有修改，则命中协商缓存）
<a name="yPzDx"></a>
### 21. 什么是单线程，和异步的关系

- 单线程 - 只有一个线程，只能做一件事
- 原因是 - 避免 DOM 渲染的冲突
   - 浏览器需要渲染 DOM，JS 可以修改 DOM 结构，JS 执行的时候，浏览器 DOM 渲染会暂停两段 JS 也不能同时执行（都修改 DOM 就冲突了），webworker 支持多线程，但是不能访问 DOM
- 解决方案 - 异步
<a name="iTjcI"></a>
### 22. JavaScript的组成

- JavaScript 由以下三部分组成：
   - ECMAScript（核心）：JavaScript` 语言基础
   - DOM（文档对象模型）：规定了访问HTML和XML的接口
   - BOM（浏览器对象模型）：提供了浏览器窗口之间进行交互的对象和方法
<a name="mCANV"></a>
### 23.  检测浏览器版本版本有哪些方式？

- 根据 navigator.userAgent UA.toLowerCase().indexOf('chrome')
- 根据 window 对象的成员 'ActiveXObject' in window
<a name="EOfWN"></a>
### 24. 页面编码和被请求的资源编码如果不一致如何处理

- 后端响应头设置 charset
- 前端页面<meta>设置 charset
- 对于get请求的参数需要使用 encodeURIComponent函数对参数进行编码处理
<a name="kUIYN"></a>
### 25. 把<script>放在</body>之前和之后有什么区别？浏览器会如何解析它们？

1. **如果把script标签放在</body>之前，则浏览器加载时会阻塞其他资源，如果放到</body>标签之后，则不会阻塞资源。**
2. 按照HTML标准，在</body>结束后出现<script>或任何元素的开始标签，都是解析错误，虽然不符合HTML标准，但浏览器会自动容错，使实际效果与写在</body>之前没有区别
3. **浏览器的容错机制会忽略****<script>****之前的****</body>****，视作****<script>****仍在 body 体内**。省略</body>和</html>闭合标签符合HTML标准，服务器可以利用这一标准尽可能少输出内容
<a name="PMFey"></a>
### 26. RAF 和 RIC 是什么

- requestAnimationFrame： 告诉浏览器在下次重绘之前执行传入的回调函数(通常是操纵 dom，更新动画的函数)；由于是每帧执行一次，那结果就是每秒的执行次数与浏览器屏幕刷新次数一样，通常是每秒 60 次。
- requestIdleCallback：: 会在浏览器空闲时间执行回调，也就是允许开发人员在主事件循环中执行低优先级任务，而不影响一些延迟关键事件。如果有多个回调，会按照先进先出原则执行，但是当传入了 timeout，为了避免超时，有可能会打乱这个顺序
   - requestIdleCallback是在绘制之后执行，假如某一帧里面要执行的任务不多，在不到16ms（1000/60)的时间内就完成了上述任务的话，那么这一帧就会有一定的空闲时间，这段时间就恰好可以用来执行requestIdleCallback的回调，由于requestIdleCallback利用的是帧的空闲时间，所以就有可能出现浏览器一直处于繁忙状态，导致回调一直无法执行，这其实也并不是我们期望的结果，那么这种情况我们就需要在调用requestIdleCallback的时候传入第二个配置参数timeout了？
<a name="qmcdN"></a>
### 27. 前后端路由差别

1. 后端每次路由请求都是重新访问服务器
2. 前端路由实际上只是JS根据URL来操作DOM元素，根据每个页面需要的数据去服务端请求数据，返回数据后和模板进行组合
3. .前端路由的优缺点

优点：<br />1.用户体验好，页面初始化后，只需要根据路由变换页面内容，不需要再向服务器发送请求，内容变换速度快。<br />2.可以在浏览器中输入指定想要访问的url<br />3.实现了前后端分离，方便开发。<br />缺点：<br />1.使用浏览器的前进、后退键的时候会重新发送请求，没有合理的利用缓存<br />2.单页面无法记住之前滚动的位置，无法在前进、后退的时候记住滚动的位置
<a name="CJgRJ"></a>
### 28.escape、encodeURI、encodeURIComponent Base64  的区别

- encodeURI （decodeURI）是对整个 URI 进行转义，将 URI 中的非法字符转换为合法字符，所以对于一些在 URI 中有特殊意义的字符不会进行转义。适合编码整个url
- encodeURIComponent （decodeURIComponent）是对 URI 的组成部分进行转义，所以一些特殊字符也会得到转义。适合编码请求参数
- escape（unescap） 和 encodeURI 的作用相同，不过它们对于 unicode 编码为 0xff 之外字符的时候会有区别，escape 是直接在字符的 unicode 编码前加上 %u，而 encodeURI 首先会将字符转换为 UTF-8 的格式，再在每个字节前加上 %。 适合编码字符串
- Base64  是一种用64个字符来表示任意二进制数据的方法。它是一种编码方式，而非加密方式。它通过将二进制数据转变为64个可打印字符”，因为网络传输只能传输可打印字符完成了数据在HTTP协议上的传输。
```javascript
//btoa
//atob
var str = "Lucson";
var res = window.btoa(str);
//然后使用 atob() 方法来对数据进行解码
console.log(window.atob(res));
```
**什么场合用什么方法**<br />（1）**如果只是编码字符串，不和URL有半毛钱关系，那么用escape。**<br />（2）**如果你需要编码整个URL，然后需要使用这个URL，那么用encodeURI。**<br />（3）**当你需要编码URL中的参数的时候，那么encodeURIComponent是最好方法。**

<a name="Y2QfA"></a>
### 29.（设计题）想实现一个对页面某个节点的拖曳？如何做？（使用原生JS）

1. 给需要拖拽的节点绑定mousedown, mousemove, mouseup事件
2. mousedown事件触发后，开始拖拽
3. mousemove时，需要通过event.clientX和clientY获取拖拽位置，并实时更新位置
4. mouseup时，拖拽结束
5. 需要注意浏览器边界的情况
<a name="mCQHD"></a>
### 30.封装一个函数，参数是定时器的时间，.then执行回调函数
```javascript
function sleep (time) {
    return new Promise((resolve) => setTimeout(resolve, time));
}
```
<a name="W7TRc"></a>
### 31.在输入框中如何判断输入的是一个正确的网址
```javascript
function isUrl(url) {
    try {
        new URL(url);
        return true;
    } catch (err) {
        return false;
    }
}
```
<a name="ooB3A"></a>
### 32. 全局作用域中，用 const 和 let 声明的变量不在 window 上，那到底在哪里？如何去获取？
在全局作用域中，用 let 和 const 声明的全局变量并没有在全局对象中，**只是一个单独的块级作用域（Script）中**。那要怎么获取呢？在定义变量的块级作用域中就能获取啊，既然不属于顶层对象Window，那就不加 window（global），直接访问即可。
<a name="vbz7Y"></a>
## ES6
<a name="fcJu0"></a>
### 1. Proxy 可以实现什么功能？
在 Vue3.0 中通过 `Proxy` 来替换原本的 `Object.defineProperty` 来实现数据响应式。<br />Proxy 是 ES6 中新增的功能，它可以用来自定义对象中的操作。
```javascript
let p = new Proxy(target, handler)
```
`target` 代表需要添加代理的对象，`handler` 用来自定义对象中的操作，比如可以用来自定义 `set` 或者 `get` 函数。<br />下面来通过 `Proxy` 来实现一个数据响应式：
```javascript
let onWatch = (obj, setBind, getLogger) => {
  let handler = {
    get(target, property, receiver) {
      getLogger(target, property)
      return Reflect.get(target, property, receiver)
    },
    set(target, property, value, receiver) {
      setBind(value, property)
      return Reflect.set(target, property, value)
    }
  }
  return new Proxy(obj, handler)
}
let obj = { a: 1 }
let p = onWatch(
  obj,
  (v, property) => {
    console.log(`监听到属性${property}改变为${v}`)
  },
  (target, property) => {
    console.log(`'${property}' = ${target[property]}`)
  }
)
p.a = 2 // 监听到属性a改变
p.a // 'a' = 2
```
在上述代码中，通过自定义 `set` 和 `get` 函数的方式，在原本的逻辑中插入了我们的函数逻辑，实现了在对对象任何属性进行读写时发出通知。

当然这是简单版的响应式实现，如果需要实现一个 Vue 中的响应式，需要在 `get` 中收集依赖，在 `set` 派发更新，之所以 Vue3.0 要使用 `Proxy` 替换原本的 API 原因在于 `Proxy` 无需一层层递归为每个属性添加代理，一次即可完成以上操作，性能上更好，并且原本的实现有一些数据更新不能监听到，但是 `Proxy` 可以完美监听到任何方式的数据改变，唯一缺陷就是浏览器的兼容性不好。
<a name="f9GGv"></a>
### 2. var、let、const 区别
var、let、const共同点都是可以声明变量的

1. **变量提升、重复声明：** let和const没有变量提升的机制，且不可重复声明，var具有，var可以
2. **初始值设置** ：const声明变量必须设置初始值且不可再次赋值，var、let可以
3. **块级作用域** ：let和const具有块级作用域，var无
   1. 块级作用域解决了ES5中的两个问题： 
      - 内层变量可能覆盖外层变量
      - 用来计数的循环变量泄露为全局变量的问题
5. **给全局添加属性** 
   1. 浏览器的全局对象是window，Node的全局对象是global。var声明的变量为全局变量，并且会将该变量添加为全局对象的属性，但是let和const不会。variables_:VariableMap
6. **暂时性死区** 
   1. 在使用let、const命令声明变量之前，该变量都是不可用的。这在语法上，称为**暂时性死区**。使用var声明的变量不存在暂时性死区。
- 该变量处于从块开始到初始化处理的“暂存死区”
7. **let-const和window的关系**
   1. 每一个执行上下文会关联到一个变量环境(VariableEnvironment)中，在执行代码中变量和函数的声明会作为环境记录(Environment Record)添加到变量环境中。对于函数来说，参数也会被作为环境记录添加到变量环境中。
   2. variables_:VariableMap

<a name="C2pdz"></a>
### 3. 扩展运算符的作用及使用场景
**对象扩展运算符**用于取出参数对象中可枚举属性，拷贝到当前对象之中。**两种方法后面的同名属性都会覆盖前面的属性**
```javascript
let bar = { a: 1, b: 2 };
let baz = { ...bar }; // { a: 1, b: 2 }
//上述方法实际上等价于:
let baz = Object.assign({}, bar); // { a: 1, b: 2 }
```
**数组扩展运算符****可将一个数组转为用逗号分隔的参数序列**，且每次只能展开一层数组。
```javascript
console.log(...[1, [2, 3, 4], 5])
// 1 [2, 3, 4] 5
```
**应用：**

- **将数组转换为参数序列**
```javascript
function add(x, y) {
  return x + y;
} 
add(...[1, 2]) // 3
```

- **复制数组（浅拷贝）**
```javascript
const arr1 = [1, 2];
const arr2 = [...arr1];
```

- **合并数组**
```javascript
const arr1 = ['two', 'three'];
const arr2 = ['one', ...arr1, 'four', 'five']; // ["one", "two", "three", "four", "five"]
```

- **扩展运算符与解构赋值结合起来，用于生成数组**
```javascript
const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest  // [2, 3, 4, 5]
```

- **将字符串转为真正的数组**
```javascript
[...'hello']    // [ "h", "e", "l", "l", "o" ]
```

- **任何 Iterator 接口的对象，都可以用扩展运算符转为真正的数组**
```javascript
// arguments对象
function foo() {
  const args = [...arguments];//可替换es5中的Array.prototype.slice.call(arguments)写法。
}
```
<a name="KFNpk"></a>
### 4. 对对象与数组的解构的理解
解构是 ES6 提供的一种提取数据的模式，这种模式能够从对象或数组里有针对性地拿到想要的数值。
**数组的解构：以元素的位置为匹配条件来提取想要的数据**```javascript
const [a, b, c] = [1, 2, 3]
```
还可以通过给左侧变量数组设置**空占位的方式**，实现对数组中某几个元素的精准提取：
```javascript
const [a,,c] = [1,2,3]
```
**对象的解构：以属性的名称为匹配条件，来提取想要的数据的**```javascript
const num = { a: 'a',  b: b }
const { name, age } = num  //a 和 b 两个和 num 平级的变量
```
**如何提取高度嵌套的对象里的指定属性？**<br />可以在解构出来的变量名右侧，通过冒号+{目标属性名}这种形式，进一步解构它，一直解构到拿到目标数据为止。
```javascript
const school = {
   classes: {
      stu: {
         name: 'Bob',
         age: 24,
      }
   }
}

const { classes: { stu: { age } } } = school

console.log(age)//24
```
<a name="y12m3"></a>
### 5. 对 rest 参数的理解

1. 扩展运算符被用在函数形参上时，**它还可以把一个分离的参数序列整合成一个数组**：
2. **它可以把函数的多佘的参收敛进一个数组里。这一点经常用于获取函数的参数个数不确定的情况。**
```javascript
function mutiple(...args) {
    console.log(args)// [1, 2, 3, 4]
}
mutiple(1, 2, 3, 4) 
```

<a name="z4nSw"></a>
### 6.ES6对String类型做的常用升级优化?
**优化部分**<br />ES6新增了字符串模板，可插入变量，能保留所有空格和换行，使得字符串拼接看起来更加直观，更加优雅<br />**升级部分**

1. ES6在String原型上新增了includes()方法，用于取代传统的只能用indexOf查找包含字符的方法(indexOf返回-1表示没查到不如includes方法返回false更明确，语义更清晰), 
2. 此外还新增了startsWith(), endsWith(), 用来判断当前字符串是否是以另一个子串“开始/结尾"，
3. padStart(),padEnd()开始填充字符串,
4. repeat()字符串复制指定次数，等方法
<a name="cTGFl"></a>
### 7.举一些ES6对Array数组类型做的常用升级优化
**优化部分**

- **数组解构赋值**。es6提取数据的模式，这种模式能够从对象或数组里有针对性地拿到想要的数值，且支持赋默认值
- **扩展运算符**。可将一个数组转为用逗号分隔的参数序列，轻松获取未知参数个数情况下的参数集合。可以取代arguments对象和apply方法将类数组转化为真正的数组。可方便的实现数组的复制和解构赋值 

**升级部分**

1. ES6在Array原型上新增了find()/findIndex方法，用于取代传统的只能用indexOf查找包含数组项目的方法,且修复了indexOf查找不到NaN的bug([NaN].indexOf(NaN) === -1).
2. Array.from
3. 此外还新增了~~copyWithin~~(),includes(), fill(),flat(),keys(),kvalues(),entries()等方法，更方便对数组操作
<a name="kTaTw"></a>
### 8.举一些ES6对Number数字类型做的常用升级优化
**优化部分**<br />ES6在Number原型上新增了isFinite(有穷数?), isNaN()方法，用来取代传统的全局isFinite(), isNaN()方法检测数值是否有限、是否是NaN。ES5的isFinite(), isNaN()方法都会先将非数值类型的参数转化为Number类型再做判断，这其实是不合理的，最造成isNaN('NaN') === true的奇怪行为--'NaN'是一个字符串，但是isNaN却说这就是NaN。而Number.isFinite()和Number.isNaN()则不会有此类问题(Number.isNaN('NaN') === false)。（isFinite()同上）<br />**升级部分**

1. ES6在Math对象上新增了Math.cbrt()（返回数字的立方根），trunc()（方法会将数字的小数部分去掉，只保留整数部分。），等数学计算
2. 引入bigint类型，处理大整数
<a name="zhbqJ"></a>
### 9.举一些ES6对Object类型做的常用升级优化
**优化部分**

1. 对象的扩展运算符
2. 对象解构赋值
对象属性变量式声明。ES6可以直接以变量形式声明对象属性或者方法，~~比传统的键值对形式声明更加简洁，更加方便，语义更加清晰~~```javascript
let {keys, values, entries} = Object;
let MyOwnMethods = {keys, values, entries};
// let MyOwnMethods = {keys: keys, values: values, entries: entries}

let es5Fun = {
    method: function(){}
}; 
let es6Fun = {
    method(){}
}
```

3. super 关键字。ES6在Class类里新增了类似this的关键字super。同this总是指向当前函数所在的对象不同，super关键字总是指向当前函数所在对象的原型对象

**升级部分**

1. ES6在Object原型上新增了is()方法，做两个目标对象的相等比较，用来完善'==='方法。'==='方法中NaN === NaN //false其实是不合理的，Object.is修复了这个小bug。(Object.is(NaN, NaN) // true)
2. ES6在Object原型上新增了assign()方法，用于对象新增属性或者多个对象合并, assign方法且无法正确复制get和set属性（会直接执行get/set函数，取return的值）
3. ES6在Object原型上新增了getOwnPropertyDescriptors()方法，此方法增强了ES5中getOwnPropertyDescriptor()方法，可以**获取指定对象所有自身属性的描述对象**。结合defineProperties()方法，**可以完美复制对象，包括复制get和set属性**
4. ES6在Object原型上新增了getPrototypeOf()和setPrototypeOf()方法，用来获取或设置当前对象的prototype对象。这个方法存在的意义在于，ES5中获取设置prototype对像是通过__proto__属性来实现的，然而__proto__属性并不是ES规范中的明文规定的属性，~~只是浏览器各大产商“私自”加上去的属性，只不过因为适用范围广而被默认使用了，再非浏览器环境中并不一定就可以使用，所以为了稳妥起见，获取或设置当前对象的~~~~prototype~~~~对象时，都应该采用ES6新增的标准用法~~
5. ES6在Object原型上还新增了Object.keys()，Object.values()，Object.entries()方法，用来获取对象的所有键、所有值和所有键值对数组
<a name="Js0su"></a>
### 10.举一些ES6对Function函数类型做的常用升级优化?
**优化部分**

1. **箭头函数(核心)**。箭头函数是ES6核心的升级项之一，箭头函数里没有自己的this,这改变了以往JS函数中最让人难以理解的this运行机制。主要优化点
2. **函数默认赋值**。ES6之前，函数的形参是无法给默认值得，只能在函数内部通过变通方法实现。ES6以更简洁更明确的方式进行函数默认赋值
3. **reset 参数**
```javascript
function es6Fuc (x, y = 'default') {
    console.log(x, y);
}
es6Fuc(4) // 4, default
```
**升级部分**<br />ES6新增了双冒号运算符，用来取代以往的bind，call,和apply。(浏览器暂不支持，Babel已经支持转码)
```javascript
foo::bar;
// 等同于
bar.bind(foo);

foo::bar(...arguments);
// 等同于
bar.apply(foo, arguments);
```
<a name="zpqPB"></a>
### 11.Iterator是什么，有什么作用？

1. Iterator（迭代器）是一种接口，也可以说是一种标准/一种规范。为各种不同的数据结构提供统一的遍历方式。
2. Iterator标准规定，所有部署了key值为[Symbol.iterator]，且[Symbol.iterator]的value是标准的Iterator接口函数的对象，都称之为可遍历对象，next()后返回的Iterator对象也就是Iterator遍历器
3. 标准的Iterator接口函数: 该函数必须返回一个对象，且对象中包含next方法，且执行next()能返回包含value/done属性的Iterator对象
详细
- terator是ES6中一个很重要概念，它并不是对象，也不是任何一种数据类型。因为ES6新增了Set、Map类型，他们和Array、Object类型很像，Array、Object都是可以遍历的，但是Set、Map都不能用for循环遍历，解决这个问题有两种方案，一种是为Set、Map单独新增一个用来遍历的API，另一种是为Set、Map、Array、Object新增一个统一的遍历API，显然，第二种更好，ES6也就顺其自然的需要一种设计标准，来统一所有可遍历类型的遍历方式。Iterator正是这样一种标准。或者说是一种规范理念
- 就好像JavaScript是ECMAScript标准的一种具体实现一样，Iterator标准的具体实现是Iterator遍历器。Iterator标准规定，所有部署了key值为[Symbol.iterator]，且[Symbol.iterator]的value是标准的Iterator接口函数的对象，都称之为可遍历对象，next()后返回的Iterator对象也就是Iterator遍历器
- 标准的Iterator接口函数: 该函数必须返回一个对象，且对象中包含next方法，且执行next()能返回包含value/done属性的Iterator对象
**Iterator 接口（遍历器）实现过程：**
1. 创建一个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是一个指针对象。
2. 第一次调用指针对象的next方法，可以将指针指向数据结构的第一个成员。每一次的 next 都会返回一个对象且指针下移，该对象有两个属性
   1. value 代表想要获取的数据
   2. done 布尔值，false表示当前指针指向的数据有值，true表示遍历已经结束
3. 不断调用指针对象的next方法，直到它指向数据结构的结束位置。
```javascript
//obj就是可遍历的，因为它遵循了Iterator标准，且包含[Symbol.iterator]方法，方法函数也符合标准的Iterator接口规范。
//obj.[Symbol.iterator]() 就是Iterator遍历器
let obj = {
  data: [ 'hello', 'world' ],
  [Symbol.iterator]() {
    const self = this;
    let index = 0;
    return {
      next() {
        if (index < self.data.length) {
          return {
            value: self.data[index++],
            done: false
          };
        } else {
          return { value: undefined, done: true };
        }
      }
    };
  }
};
```
ES6给Set、Map、Array、String，类数组对象都加上了[Symbol.iterator]方法，且[Symbol.iterator]方法函数也符合标准的Iterator接口规范，所以它们默认都是可以遍历的<br />for of   遍历的目标需要具备Iterator接口<br />**为什么对象（Object）没有部署Iterator接口呢？**

- 一是因为对象的哪个属性先遍历，哪个属性后遍历是不确定的，需要开发者手动指定。然而遍历遍历器是一种线性处理，对于非线性的数据结构，部署遍历器接口，就等于要部署一种线性转换
- 对对象部署Iterator接口并不是很必要，因为Map弥补了它的缺陷，又正好有Iteraotr接口
<a name="JaQJV"></a>
### 12.Generator函数是什么，有什么作用？

1. Generator 函数是 ES6 提供的一种异步编程解决方案
2. Generator 函数有多种理解角度。语法上，首先可以把它理解成，Generator 函数是一个状态机，封装了多个内部状态。
3. 执行 Generator 函数会返回一个遍历器对象，也就是说，Generator 函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。
4. 形式上，Generator 函数是一个普通函数，但是有两个特征。一是，function关键字与函数名之间有一个星号；二是，函数体内部使用yield表达式，定义不同的内部状态，函数遇到 yield 的时候会暂停，并返回yield表达式后面的结果（yield在英语里的意思就是“产出”）。
5. **Generator 可以控制函数的分段执行**
6. next作用是将代码的控制权交还给生成器函数，作用（指针移向下一个状态）

**总结**，Generator函数是ES6提供的一种异步编程解决方案。通过yield标识位和next()方法调用，实现函数的分段执行，**遍历器对象生成函数，最大的特点是可以交出函数的执行权**
更多**Generator.prototype.throw()：**Generator 函数返回的遍历器对象，都有一个throw方法，可以在函数体外抛出错误，然后在 Generator 函数体内捕获。throw方法可以接受一个参数，该参数会被catch语句接收，如果 Generator 函数内部没有部署try...catch代码块，那么throw方法抛出的错误，将被外部try...catch代码块捕获。如果 Generator 函数内部和外部，都没有部署try...catch代码块，那么程序将报错，直接中断执行。throw方法被捕获以后，会附带执行下一条yield表达式。也就是说，会附带执行一次next方法。一旦 Generator 执行过程中抛出错误，且没有被内部捕获，就不会再执行下去了。如果此后还调用next方法，将返回一个value属性等于undefined、done属性等于true的对象，即 JavaScript 引擎认为这个 Generator 已经运行结束了。<br />**Generator.prototype.return()**：Generator 函数返回的遍历器对象，还有一个return("sdsad")方法，可以返回给定的值，并且终结遍历 Generator 函数。如果 Generator 函数内部有try...finally代码块，且正在执行try代码块，那么return()方法会导致立刻进入finally代码块，执行完以后，整个函数才会结束。<br />**next()、throw()、return() 的共同点：**可以放在一起理解。它们的作用都是让 Generator 函数恢复执行，并且使用不同的语句替换yield表达式。next()是将yield表达式替换成一个值。
**分析其执行过程**```javascript
// 使用 * 表示这是一个 Generator 函数
// 内部可以通过 yield 暂停代码
// 通过调用 next 恢复执行
function *foo(x) {
  let y = 2 * (yield (x + 1))
  let z = yield (y / 3)
  return (x + y + z)
}
let it = foo(5)
console.log(it.next())   // => {value: 6, done: false}
console.log(it.next(12)) // => {value: 8, done: false}
console.log(it.next(13)) // => {value: 42, done: true}
```
分析其执行过程：

- 首先 Generator 函数调用时它会返回一个迭代器
- 当执行第一次 next 时，传参会被忽略，并且函数暂停在 yield (x + 1) 处，所以返回 5 + 1 = 6
- 当执行第二次 next 时，传入的参数等于上一个 yield 的返回值，如果你不传参，yield 永远返回 undefined。此时 let y = 2 * 12，所以第二个 yield 等于 2 * 12 / 3 = 8
- 当执行第三次 next 时，传入的参数会传递给 z，所以 z = 13, x = 5, y = 24，相加等于 42

yield实际就是暂缓执行的标示，每执行一次next()，相当于指针移动到下一个yield位置
**Generator 实现**```javascript
// 使用 * 表示这是一个 Generator 函数
// 内部可以通过 yield 暂停代码
// 通过调用 next 恢复执行
function* test() {
  let a = 1 + 2;
  yield 2;
  yield 3;
}
let b = test();
console.log(b.next()); // >  { value: 2, done: false }
console.log(b.next()); // >  { value: 3, done: false }
console.log(b.next()); // >  { value: undefined, done: true }
```
从以上代码可以发现，加上 * 的函数执行后拥有了 next函数，也就是说函数执行后返回了一个对象。每次调用 next函数可以继续执行被暂停的代码。以下是 Generator 函数的简单实现
```javascript
// cb 也就是编译过的 test 函数
function generator(cb) {
    return (function () {
        var object = {
            next: 0,
            stop: function () { }
        };

        return {
            next: function () {
                var ret = cb(object);
                if (ret === undefined) return { value: undefined, done: true };
                return {
                    value: ret,
                    done: false
                };
            }
        };
    })();
}
// 如果你使用 babel 编译后可以发现 test 函数变成了这样
function test() {
    var a;
    return generator(function (_context) {
        while (1) {
            switch ((_context.prev = _context.next)) {
                // 可以发现通过 yield 将代码分割成几块
                // 每次执行 next 函数就执行一块代码
                // 并且表明下次需要执行哪块代码
                case 0:
                    a = 1 + 2;
                    _context.next = 4;
                    return 2;
                case 4:
                    _context.next = 6;
                    return 3;
                // 执行完毕
                case 6:
                case "end":
                    return _context.stop();
            }
        }
    });
}
```
**传统的异步编程。ES6 之前，异步编程大致如下**这里你可以说说 Generator的异步编程，以及它的语法糖 async 和 awiat，传统的异步编程。ES6 之前，异步编程大致如下

- 回调函数
- 事件监听
- 发布/订阅

<a name="Hk7vw"></a>
### 13.async函数是什么，有什么作用？
async函数可以理解为内置自动执行器的Generator函数语法糖，它配合ES6的Promise近乎完美的实现了异步编程解决方案<br />**async、await 优缺点**

1. **优点：**async 和 await 相比直接使用 Promise 来说，优势在于处理 then 的调用链，能够更清晰准确的书写代码。
2. **缺点：**在于滥用 await 可能会导致性能问题，因为 await 会阻塞代码，也许之后的异步代码并不依赖于前者，但仍然需要等待前者完成，导致代码失去了并发性
await 会阻塞代码```javascript
var a = 0
var b = async () => {
  a = a + await 10
  console.log('2', a) // -> '2' 10
  a = (await 10) + a
  console.log('3', a) // -> '3' 20
}
b()
a++
console.log('1', a) // -> '1' 1
```

1. 首先函数b 先执行，在执行到 await 10 之前变量 a 还是 0，因为在 await 内部实现了 generators ，generators 会保留堆栈中东西，所以这时候 a = 0 被保存了下来
2. 因为 await 是异步操作，遇到await就会立即返回一个pending状态的Promise对象，暂时返回执行代码的控制权，使得函数外的代码得以继续执行，所以会先执行 console.log('1', a)
3. 这时候同步代码执行完毕，开始执行异步代码，将保存下来的值拿出来使用，这时候 a = 10 然后后面就是常规执行代码了
<a name="Xu4l8"></a>
### 14.Class、extends是什么，有什么作用？
**ES6 的class可以看作只是一个ES5生成实例对象的构造函数的语法糖。**它参考了java语言，定义了一个类的概念，让对象原型写法更加清晰，对象实例化更像是一种面向对象编程。**Class类可以通过extends实现继承**。它和ES5构造函数的不同点<br />ES5 / ES6 的继承除了写法以外还有什么区别

1. ES6的class类必须用new命令操作，而ES5的构造函数不用new也可以执行。
2. ES6的class类不存在变量提升，必须先定义class之后才能实例化，不像ES5中可以将构造函数的写在实例化之后。
3. ES5 的继承，实质是先创造子类的实例对象this，然后再将父类的方法添加到this上面。ES6 的继承机制完全不同，~~实质是先将父类实例对象的属性和方法，加到~~~~this~~~~上面~~   实质上是先创建父类的实例对象（ this）（所以必须先调用super方法），然后再用子类的构造函数修改this。
4. ES5 的继承时通过原型或构造函数机制来实现。
5. ES6 通过 class 关键字定义类，里面有构造方法，类之间通过 extends 关键字实现继承。
6. 子类必须在 constructor 方法中调用 super 方法，否则新建实例报错。**因为子类没有自己的 this 对象，而是继承了父类的 this 对象，然后对其进行加工**。如果不调用 super 方法，子类得不到 this 对象。
7. 注意 super 关键字指代父类的实例，即父类的 this 对象。
8. 注意：在子类构造函数中，调用 super 后，才可使用 this 关键字，否则报错。
**类的内部（原型上）定义的所有方法，都是不可枚举的**```javascript
///ES5
function ES5Fun (x, y) {
	this.x = x;
	this.y = y;
}
ES5Fun.prototype.toString = function () {
	 return '(' + this.x + ', ' + this.y + ')';
}
var p = new ES5Fun(1, 3);
p.toString();
Object.keys(ES5Fun.prototype); //['toString']

//ES6
class ES6Fun {
	constructor (x, y) {
		this.x = x;
		this.y = y;
	}
	toString () {
		return '(' + this.x + ', ' + this.y + ')';
	}
}

Object.keys(ES6Fun.prototype); //[]
```
<a name="nUpxC"></a>
### 15.module、export、import是什么，有什么作用？

- module、export、import是ES6用来统一前端模块化方案的设计思路和实现方案。export、import的出现统一了前端模块化的实现方案，整合规范了浏览器/服务端的模块化方法，用来取代传统的AMD/CMD、requireJS、seaJS、commondJS等等一系列前端模块不同的实现方案，使前端模块化更加统一规范，JS也能更加能实现大型的应用程序开发。
- **import引入的模块是静态加载（编译阶段加载）而不是动态加载（运行时加载**）。
- **import引入export导出的接口值是动态绑定关系，即通过该接口，可以取到模块内部实时的值**
<a name="N8nQo"></a>
### 16 日常前端代码开发中，有哪些值得用ES6去改进的编程优化或者规范？

- 常用箭头函数来取代var self = this;的做法。
- 常用let取代var命令。
- 常用数组/对象的解构赋值来命名变量，结构更清晰，语义更明确，可读性更好。
- 在长字符串多变量组合场合，用模板字符串来取代字符串累加，能取得更好地效果和阅读体验。
- 用Class类取代传统的构造函数，来生成实例化对象。
- 在大型应用开发中，要保持module模块化开发思维，分清模块之间的关系，常用import、export方法。
<a name="VJ4Zg"></a>
### 17 ES6的了解

1. 新增模板字符串（为JavaScript提供了简单的字符串插值功能）
2. 箭头函数
3. for-of（用来遍历数据—例如数组中的值。）
4. arguments对象可被不定参数和默认参数完美代替。
5. ES6将promise对象纳入规范，提供了原生的Promise对象。
6. 增加了let和const命令，用来声明变量。增加了块级作用域，ES6规定，var命令和function命令声明的全局变量，属于全局对象的属性；let命令、const命令、class命令声明的全局变量，不属于全局对象的属性。。
7. 还有就是引入module模块的概念
<a name="c3UMc"></a>
### 18 谈一谈你了解ECMAScript6的新特性？

- 块级作用区域 let a = 1;，可定义常量 const PI = 3.141592654;，增加了块级作用域
- 变量解构赋值 var [a, b, c] = [1, 2, 3];
- 字符串的扩展(模板字符串) var sum =${a + b};
- 数组的扩展(转换数组类型) Array.from($('li'));
- 函数的扩展(扩展运算符) [1, 2].push(...[3, 4, 5]);
- 对象的扩展(同值相等算法) Object.is(NaN, NaN);
- 新增数据类型(Symbol) let uid = Symbol('uid');
- 新增数据结构(Map) let set = new Set([1, 2, 2, 3]);
- for...of循环 for(let val of arr){};
- Promise对象 var promise = new Promise(func);
- Generator函数 function* foo(x){yield x; return x*x;}
- 引入Class(类) class Foo {}
- 引入模块体系 export default func;
- 引入async函数[ES7]
<a name="iFive"></a>
### 19  symbol 有什么用处

1. 可以用来表示一个独一无二的变量防止**命名冲突**。
2. 利用 symbol 不会被常规的方法（除了 Object.getOwnPropertySymbols 外）遍历到，所以可以用来**模拟私有属性**。
3. 用来**重置对象的属性，**比如 Symbol.toStringTag，symbol.iterator（ES6提供了11个内置的Symbol值，可以**更改原生js的行为**）
4. 在 ES5 使用字符串表示常量，但是用字符串不能保证常量是独特的，但是使用 **Symbol 定义常量**，这样就可以保证这一组常量的值都不相等。 
消除魔术字符串魔术字符串指的是，代码中多次出现、与代码形成强耦合的某一个具体的字符串或者数值。
```css
function getArea(shape, options) {
  let area = 0;

  switch (shape) {
    case 'Triangle': // 魔术字符串
      area = .5 * options.width * options.height;
      break;
    /* ... more code ... */
  }

  return area;
}

getArea('Triangle', { width: 100, height: 100 }); // 魔术字符串
```
字符串Triangle就是一个魔术字符串。它多次出现，与代码形成“强耦合”，不利于将来的修改和维护。<br />写成一个变量
```css
const shapeType = {
  triangle: 'Triangle'
};

function getArea(shape, options) {
  let area = 0;
  switch (shape) {
    case shapeType.triangle:
      area = .5 * options.width * options.height;
      break;
  }
  return area;
}

getArea(shapeType.triangle, { width: 100, height: 100 });
```
把Triangle写成shapeType对象的triangle属性，这样就消除了强耦合。<br />如果仔细分析，可以发现shapeType.triangle等于哪个值并不重要，只要确保不会跟其他shapeType属性的值冲突即可。因此，这里就很适合改用 Symbol 值。

**总结 **

1. 标识符  （**命名冲突**）
2. 对象的属性名 （**模拟私有属性**）
3. 做独一无二的常量 （**消除魔术字符串**）
4. 内置的Symbol值，它们代表了内部语言行为（**更改原生js的行为**）
<a name="sSNdD"></a>
### 20 模块化
js 中现在比较成熟的有四种模块加载方案：

- 第一种是 CommonJS 方案，它通过 require 来引入模块，通过 module.exports 定义模块的输出接口。这种模块加载方案是服务器端的解决方案，它是以同步的方式来引入模块的，因为在服务端文件都存储在本地磁盘，所以读取非常快，所以以同步的方式加载没有问题。但如果是在浏览器端，由于模块的加载是使用网络请求，因此使用异步加载的方式更加合适。
- 第二种是 AMD 方案，这种方案采用异步加载的方式来加载模块，模块的加载不影响后面语句的执行，所有依赖这个模块的语句都定义在一个回调函数里，等到加载完成后再执行回调函数。require.js 实现了 AMD 规范
- 第三种是 CMD 方案，这种方案和 AMD 方案都是为了解决异步模块加载的问题，sea.js 实现了 CMD 规范。它和require.js的区别在于模块定义时对依赖的处理不同和对依赖模块的执行时机的处理不同。
- 第四种方案是 ES6 提出的方案，使用 import 和 export 的形式来导入导出模块

在有 Babel 的情况下，我们可以直接使用 ES6的模块化
```javascript
// file a.js
export function a() {}
export function b() {}
// file b.js
export default function() {}

import {a, b} from './a.js'
import XXX from './b.js'
```
**CommonJS**<br />CommonJs 是 Node 独有的规范，浏览器中使用就需要用到 Browserify解析了。
```javascript
// a.js
module.exports = {
    a: 1
}
// or
exports.a = 1

// b.js
var module = require('./a.js')
module.a // -> log 1l
```
在上述代码中，module.exports 和 exports 很容易混淆，让我们来看看大致内部实现
```javascript
var module = require('./a.js')
module.a
// 这里其实就是包装了一层立即执行函数，这样就不会污染全局变量了，
// 重要的是 module 这里，module 是 Node 独有的一个变量
module.exports = {
    a: 1
}
// 基本实现
var module = {
  exports: {} // exports 就是个空对象
}
// 这个是为什么 exports 和 module.exports 用法相似的原因
var exports = module.exports
var load = function (module) {
    // 导出的东西
    var a = 1
    module.exports = a
    return module.exports
};
```
再来说说 module.exports 和exports，用法其实是相似的，但是不能对 exports 直接赋值，不会有任何效果。<br />对于 CommonJS 和 ES6 中的模块化的两者区别是：

- 前者支持动态导入，也就是 require(${path}/xx.js)，后者目前不支持，但是已有提案,前者是同步导入，因为用于服务端，文件都在本地，同步导入即使卡住主线程影响也不大。
- 而后者是异步导入，因为用于浏览器，需要下载文件，如果也采用同步导入会对渲染有很大影响
- 前者在导出时都是值拷贝，就算导出的值变了，导入的值也不会改变，所以如果想更新值，必须重新导入一次。
- 但是后者采用实时绑定的方式，导入导出的值都指向同一个内存地址，所以导入值会跟随导出值变化
- 后者会编译成 require/exports 来执行的

**AMD**<br />AMD 是由 RequireJS 提出的<br />**AMD 和 CMD 规范的区别？**

- 第一个方面是在模块定义时对依赖的处理不同。AMD推崇依赖前置，在定义模块的时候就要声明其依赖的模块。而 CMD 推崇就近依赖，只有在用到某个模块的时候再去 require。
- 第二个方面是对依赖模块的执行时机处理不同。首先 AMD 和 CMD 对于模块的加载方式都是异步加载，不过它们的区别在于模块的执行时机，AMD 在依赖模块加载完成后就直接执行依赖模块，依赖模块的执行顺序和我们书写的顺序不一定一致。而 CMD在依赖模块加载完成后并不执行，只是下载而已，等到所有的依赖模块都加载好后，进入回调函数逻辑，遇到 require 语句的时候才执行对应的模块，这样模块的执行顺序就和我们书写的顺序保持一致了。
```javascript
// CMD
define(function(require, exports, module) {
  var a = require("./a");
  a.doSomething();
  // 此处略去 100 行
  var b = require("./b"); // 依赖可以就近书写
  b.doSomething();
  // ...
});

// AMD 默认推荐
define(["./a", "./b"], function(a, b) {
  // 依赖必须一开始就写好
  a.doSomething();
  // 此处略去 100 行
  b.doSomething();
  // ...
})
```

- **AMD**：requirejs 在推广过程中对模块定义的规范化产出，提前执行，推崇依赖前置
- **CMD**：seajs 在推广过程中对模块定义的规范化产出，延迟执行，推崇依赖就近
- **CommonJs**：模块输出的是一个值的 copy，运行时加载，加载的是一个对象（module.exports 属性），该对象只有在脚本运行完才会生成
- **ES6 Module**：模块输出的是一个值的引用，编译时输出接口，ES6模块不是对象，它对外接口只是一种静态定义，在代码静态解析阶段就会生成。

**谈谈对模块化开发的理解**

- 我对模块的理解是，一个模块是实现一个特定功能的一组方法。在最开始的时候，js 只实现一些简单的功能，所以并没有模块的概念，但随着程序越来越复杂，代码的模块化开发变得越来越重要。
- 由于函数具有独立作用域的特点，最原始的写法是使用函数来作为模块，几个函数作为一个模块，但是这种方式容易造成全局变量的污染，并且模块间没有联系。
- 后面提出了对象写法，通过将函数作为一个对象的方法来实现，这样解决了直接使用函数作为模块的一些缺点，但是这种办法会暴露所有的所有的模块成员，外部代码可以修改内部属性的值。
- 现在最常用的是立即执行函数的写法，通过利用闭包来实现模块私有作用域的建立，同时不会对全局作用域造成污染。
<a name="GRGkw"></a>
### 21  map和Object的区别
**Object**类型是可以通过JSON.stringify()进行序列化操作的，**Map**结构可以转化为JSON,但是无法通过parse解析回去

|  | Map | Object |
| --- | --- | --- |
| 意外的键 | Map没有默认的key值 | Object 有一个原型, 原型链上的键名有可能和自己在对象上的设置的键名产生冲突。（在ES5中可以通过Object.create(null)来设置去掉默认的key值，但这种解决方法并不常用） |
| 键的类型 | Map的键可以是任意值，包括函数、对象或任意基本类型。 | Object 的键必须是 String 或是Symbol。 |
| 键的顺序 | Map 中的 key 是有序的。因此，当迭代的时候， Map 对象以插入的顺序返回键值。 | Object 的键是无序的 |
| Size | Map 的键值对个数可以轻易地通过size 属性获取 | Object 的键值对个数只能手动计算 |
| 迭代 | Map 是 iterable 的，所以可以直接被迭代。 | 迭代Object需要以某种方式获取它的键然后才能迭代。 |
| 性能 | 在频繁增删键值对的场景下表现更好。 | 在频繁添加和删除键值对的场景下未作出优化。 |

**8.使用场景**

- 如果只需要简单的存储key-value的数据，并且key不需要存储复杂类型的，直接用对象
- 如果该对象必须通过JSON转换的，则只能用对象，目前暂不支持Map
<a name="PIr6X"></a>
### 22. Map和WeakMap的区别
**（1）Map**<br />map本质上就是键值对的集合，但是普通的Object中的键值对中的键只能是字符串。而ES6提供的Map数据结构类似于对象，但是它的键不限制范围，可以是任意类型，是一种更加完善的Hash结构。如果Map的键是一个原始数据类型，**只要两个键严格相同，就视为是同一个键**。
实际上Map是一个数组，它的每一个数据也都是一个数组，其形式如下：```javascript
const map = [
     ["name","张三"],
     ["age",18],
]
```
**Map 操作方法、Map遍历器生成函数 和 遍历方法**Map数据结构有以下操作方法：

1. **set(key,value)**：设置键名key对应的键值value，然后返回整个Map结构，如果key已经有值，则键值会被更新，否则就新生成该键。（因为返回的是当前Map对象，所以可以链式调用）
2. **get(key)**：该方法读取key对应的键值，如果找不到key，返回undefined。
3. **delete(key)**：该方法删除某个键，返回true，如果删除失败，返回false。
4. **has(key)**：该方法返回一个布尔值，表示某个键是否在当前Map对象中。
5. **size**： `map.size` 返回Map结构的成员总数。
6. **clear()**：map.clear()清除所有成员，没有返回值。

Map结构原生提供是三个遍历器生成函数和一个遍历方法

- keys()：返回键名的遍历器。
- values()：返回键值的遍历器。
- entries()：返回所有成员的遍历器。
- forEach()：遍历Map的所有成员。
```javascript
const map = new Map([
     ["foo",1],
     ["bar",2],
])
for(let key of map.keys()){
    console.log(key);  // foo bar
}
for(let value of map.values()){
     console.log(value); // 1 2
}
for(let items of map.entries()){
    console.log(items);  // ["foo",1]  ["bar",2]
}
map.forEach( (value,key,map) => {
     console.log(key,value); // foo 1    bar 2
})
```
**（2）WeakMap**<br />WeakMap 对象也是一组键值对的集合，其中的**键是弱引用的**。**其键必须是对象** <br />WeakMap 的设计目的在于，有时我们想在某个对象上面存放一些数据，但这会形成对于这个对象的引用。一旦不再需要这个对象，我们就必须手动删除这个引用，否则垃圾回收机制就不会释放它占用的内存。<br />WeakMap 就是为了解决这个问题而诞生的，它的键名所引用的对象都是弱引用，即垃圾回收机制不将该引用考虑在内。因此，只要所引用的对象的其他引用都被清除，垃圾回收机制就会释放该对象所占用的内存。也就是说，一旦不再需要，WeakMap 里面的键名对象和所对应的键值对会自动消失，不用手动删除引用。
**WeakMap 操作方法**该对象也有以下几种方法：

- **set(key,value)**：设置键名key对应的键值value，然后返回整个Map结构，如果key已经有值，则键值会被更新，否则就新生成该键。（因为返回的是当前Map对象，所以可以链式调用）
- **get(key)**：该方法读取key对应的键值，如果找不到key，返回undefined。
- **delete(key)**：该方法删除某个键，返回true，如果删除失败，返回false。
- **has(key)**：该方法返回一个布尔值，表示某个键是否在当前Map对象中。

其clear()方法已经被弃用，所以可以通过创建一个空的WeakMap并替换原对象来实现清除。<br />**总结：**

- Map 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。WeakMap 结构与 Map 结构类似，也是用于生成键值对的集合。但是 WeakMap 只接受对象作为键名（ null 除外），不接受其他类型的值作为键名。
- WeakMap 的**键是弱引用的**， 即垃圾回收机制不将该引用考虑在内。因此，只要所引用的对象的其他引用都被清除，垃圾回收机制就会释放该对象所占用的内存。WeakMap 里面的键名对象和所对应的键值对会自动消失，不用手动删除引用。
- WeakMap 与 Map 在 API 上的区别主要是两个，一是没有遍历操作（即没有keys()、values()、forEach()和entries()方法），也没有size属性。因为没有办法列出所有键名，某个键名是否存在完全不可预测，跟垃圾回收机制是否运行相关。这一刻可以取到键名，下一刻垃圾回收机制突然运行了，这个键名就没了，为了防止出现不确定性，就统一规定不能取到键名。二是无法清空，即不支持clear方法。因此，WeakMap只有四个方法可用：get()、set()、has()、delete()。
- 注意，WeakMap 弱引用的只是键名，而不是键值。键值依然是正常引用。
<a name="LAP6V"></a>
### 23. Set 和 WeakSet 的区别
**（1）Set集合**<br /> ES6 提供了新的数据结构 Set（集合）。它类似于数组，但成员的值都是唯一的，集合实现了 iterator接口，所以可以使用『扩展运算符』和『for…of…』进行遍历，集合的属性和方法：
**Set操作方法、Set遍历器生成函数 和 遍历方法**操作方法：

1.  **add     **增加一个新元素，返回当前集合； 
2.  **delete  **删除元素，返回 boolean 值； 
3.  **has      **检测集合中是否包含某个元素，返回 boolean 值； 
4.  **size     **返回集合的元素个数； 
5.  **clear    **清空集合，返回 undefined；
```javascript
//声明一个 set
let s = new Set();
let s2 = new Set(['大事儿', '小事儿', '好事儿', '坏事儿', '小事儿']);

//元素个数
console.log(s2.size);

//添加新的元素
s2.add('喜事儿');

//删除元素
s2.delete('坏事儿');

//检测
console.log(s2.has('糟心事'));

//清空
s2.clear();
console.log(s2);

for (let v of s2) {
    console.log(v);
}
```
遍历器生成函数 和 遍历方法

- **keys()**：返回键名的遍历器。
- **values()**：返回键值的遍历器。
- **entries()**：返回所有成员的遍历器。
- **forEach()**：遍历Set的所有成员。
```javascript
let set = new Set([11, 22, 33, 44])

console.log(set.keys())  //
console.log(set.values())
console.log(set.entries())
set.forEach(console.log)

index.js:6 11 11 Set(4) {11, 22, 33, 44}
index.js:6 22 22 Set(4) {11, 22, 33, 44}
index.js:6 33 33 Set(4) {11, 22, 33, 44}
index.js:6 44 44 Set(4) {11, 22, 33, 44}
```
**（2）WeskSet集合**<br />和Set类似的另外一个数据结构称之为WeakSet,也是内部元素不能重复的数据结构。<br />那么和Set有什么区别呢？

1. 区别一：WeakSet中只能存放对象类型，不能存放基本数据类型；
2. 区别二：WeakSet对元素的引用是弱引用，如果没有其他引用对某个对象进行引用，那么GC可以对该对象进行回收
**WeakSet操作方法：**
1. **add       **添加某个元素，返回WeakSet)对象本身；
2. **delete  **从WeakSetr中删除和这个值相等的元素，返回boolean类型；
3. **has       **判断WeakSet中是否存在某个元素，返回boolean类型；

WeakSet不能遍历

1. 因为WeakSet只是对对象的弱引用，如果我们遍历获取到其中的元素，那么有可能造成对象不能正常的销毁。所以存储到WeakSet中的对象是没办法获取的；
<a name="BtPW2"></a>
### 24. set 和 Map 的区别/应用场景
**区别**

1. Map和Set查找速度都非常快，时间复杂度为O(1)，而数组查找的时间复杂度为O(n)。
2. Map对象初始化的值为一个二维数组，Set对象初始化的值为一维数组。
3. Map对象和Set对象都不允许键重复（可以将Set对象的键想象成值）。
4. Map对象的键是不能改的，但是值能改，Set对象只能通过迭代器来更改值。

**应用场景**

1. Set和Map主要的应用场景在于数据重组和数据存储，检测给定的值在某个集合中是否存在，数据不重复
2. [value, value]的形式储存用map 不确定键值对里面的键需要用Map
3. Set 是一种叫做集合的数据结构，Map 是一种叫做字典的数据结构。

**集合 与 字典 的区别：**

1. 共同点：集合、字典 可以储存不重复的值
2. 不同点：集合 是以 [value, value]的形式储存元素，字典 是以 [key, value] 的形式储存

<a name="VNdEu"></a>
### 25. for...in和for...of的区别 
for...in语句~~以任意顺序~~迭代对象的除Symbol以外的可枚举属性，包括（原型上）<br />for…of 是ES6新增的遍历方式，允许遍历一个含有iterator接口的数据结构（数组/set/map、对象等）并且返回各项的值，可以由 `break`, `throw` 或 `return` 终止。在这些情况下，迭代器关闭。<br />**for...of与for...in的区别**

1. 迭代方式不同
   1. for...in 迭代对象的`除Symbol以外的可枚举属性，包括（原型上）`的键名；。
   2. for...of 迭代含有iterator接口的数据结构的值。
2. for… in 会遍历对象的整个原型链，性能非常差不推荐使用，而 for … of 只遍历当前对象不会遍历原型链；
3. `for...in`缺点：数组的键名是数字，但是`for...in`循环是以字符串作为键名“0”、“1”、“2”等等。 且遍历顺序有可能不是按照实际数组的内部顺序
4. for...in和for...of :可响应break、continue   throw;  return(非法的返回语句)，forEach 响应 return跳过当前项，不可响应break、continue

**总结：**for...in 循环主要是为了遍历对象而生，不适用于遍历数组；for...of 循环可以用来遍历数组、类数组对象，字符串、Set、Map 以及 Generator 等拥有迭代器对象的集合。

for ... of 循环可以通过 await 关键字在每次迭代中等待异步任务完成：
```abap
for await (... of ...) {
// ...
}
```
<a name="TAVUW"></a>
## 原型与原型链

- 每个函数都有 prototype 属性，除了 Function.prototype.bind()，该属性指向原型。
<a name="31e2756dc0fffdd60bbafbcf21cebbc8"></a>
### 1. 原型

1. 每个函数function都有一个 `**prototype**`，即显式原型属性，在**定义函数时**自动添加的, 默认值是一个空 **Object** 类的实例对象     (但Object不满足) **Object.prototype instanceof Object  ==  **`**false**`
2. 每个实例对象都有一个`**__proto__**`，可称为隐式原型属性，**创建对象时**自动添加的, 对象的隐式原型的值为其对应构造函数的显式原型属性prototype的值
3. 原型对象中有一个属性 `**constructor**`, 它**指向函数对象本身**  Date.prototype.constructor === Date
4. **所有函数都是Function的实例**(包含Function)  Function instanceof Function
5. 构造函数** Function 是自身的实例**, Function.**proto** === Function.prototype
<a name="M5HsB"></a>
### 2. 原型链

1. **实例对象有原型对象（__proto__），而原型对象也有原型对象（__proto__），沿着 __proto__访问到顶级父类（Object）的原型 Prototype，再继续访问将返回 null，这就是整个原型链**
2. **Object的原型对象是原型链尽头** Object.prototype.__proto__  ==  null
3. **访问一个对象的属性时，先在自身属性中查找，找到则返回，如果没有, 再沿着  __**_**proto__**_**  这条链向上查找, 找到则返回，如果最终没找到, 返回 undefined**
4. **表达式: A **`**instanceof**`** B , 如果 B 函数的显式原型对象在 A 对象的原型链上, 返回 true, 否则返回 false**
5. ~~读取对象的属性值时: 会自动到原型链中查找, 设置对象的属性值时: 不会查找原型链, 如果当前对象中没有此属性, 直接添加此属性并设置其值~~
6. 在构造函数显式原型上添加共用的属性或方法（主要是方法），则创建出的实例对象均可通过原型链访问到

**特点：JavaScript 对象是通过引用来传递的，创建的每个新对象实体中并没有一份属于自己的原型副本。当修改原型时，与之相关的对象也会继承这一改变。**<br />![](https://cdn.nlark.com/yuque/0/2021/png/1500604/1615475711487-c474af95-b5e0-4778-a90b-9484208d724d.png#averageHue=%23f8f7f3&height=781&id=vIqx4&originHeight=781&originWidth=618&originalType=binary&ratio=1&rotation=0&showTitle=false&size=0&status=done&style=shadow&title=&width=618)
**原型修改、重写**```javascript
function Person(name) {
    this.name = name
}
// 修改原型
Person.prototype.getName = function() {}
var p = new Person('hello')
console.log(p.__proto__ === Person.prototype) // true
console.log(p.__proto__ === p.constructor.prototype) // true
// 重写原型
Person.prototype = {
    getName: function() {}
}
var p = new Person('hello')
console.log(p.__proto__ === Person.prototype)        // true
console.log(p.__proto__ === p.constructor.prototype) // false
```
**可以看到修改原型的时候p的构造函数不是指向Person了**，因为直接给Person的原型对象直接用对象赋值时，它的构造函数指向的了根构造函数Object，所以这时候`p.constructor === Object` ，而不是`p.constructor === Person`。要想成立，就要用constructor指回来：
```javascript
Person.prototype = {
    getName: function() {}
}
var p = new Person('hello')
p.constructor = Person
console.log(p.__proto__ === Person.prototype)        // true
console.log(p.__proto__ === p.constructor.prototype) // true
```
**原型链指向**```javascript
p.__proto__  // Person.prototype
Person.prototype.__proto__  // Object.prototype
p.__proto__.__proto__ //Object.prototype
p.__proto__.constructor.prototype.__proto__ // Object.prototype
Person.prototype.constructor.prototype.__proto__ // Object.prototype
p1.__proto__.constructor // Person
Person.prototype.constructor  // Person
```
**原型链的终点是什么？如何打印出原型链的终点？**由于`Object`是构造函数，**原型链终点是**`**Object.prototype.__proto__**`，而`Object.prototype.__proto__=== null // true`，所以，**原型链的终点是**`**null**`。原型链上的所有原型都是对象，所有的对象最终都是由`Object`构造的，而`Object.prototype`的下一级是`Object.prototype.__proto__`。<br />![](https://cdn.nlark.com/yuque/0/2020/jpeg/1500604/1605247722640-5bcb9156-a8b4-4d7c-83d7-9ff80930e1de.jpeg#averageHue=%232b2a25&height=118&id=as90T&originHeight=146&originWidth=490&originalType=binary&ratio=1&rotation=0&showTitle=false&size=0&status=done&style=none&title=&width=396)
**如何获得对象非原型链上的属性？**使用后`hasOwnProperty()`方法来判断指定属性是否属于对象自身：
```javascript
function iterate(obj){
   var res=[];
   for(var key in obj){
        if(obj.hasOwnProperty(key))
           res.push(key+': '+obj[key]);
   }
   return res;
} 
```
<a name="ozs8z"></a>
## 面向对象
一般面向对象包含：继承，封装，多态，抽象
<a name="qZS7s"></a>
### 1. 对象创建的方式有哪些？
<a name="iJmwA"></a>
#### 1. 字面量的形式   Object构造函数模式    Object.create
字面量的形式   Object构造函数模式    Object.create缺点：

1. 产生大量的重复代码，**不可复用**
2. 字面量的形式，Object构造函数创建类型单一
<a name="FHugl"></a>
#### 2. 工厂模式
工厂模式**用函数来封装创建对象的细节，达到复用的目的**<br />缺点：

1. 只是简单的封装了复用代码，而没有建立起对象和类型间的关系。
```javascript
function createPerson(name, age) { 
  return {
    name: name,
    age: age,
    setName: function (name) {
      this.name = name
    }
  }
}
```
<a name="sbrtJ"></a>
#### 3. 自定义构造函数模式
自定义构造函数模式复用代码  +  创建的对象和构造函数建立起了联系<br />缺点：

1. 如果对象属性中如果包含函数的话，那么每次都会新建一个函数对象，浪费了不必要的内存空间 
```javascript
function Person(name, age) {
  this.name = name
  this.age = age
  this.setName = function (name) {
    this.name = name
  }
}
```
<a name="qFlv3"></a>
#### 4. 原型模式  /  （构造函数 + 原型模式）
原型模式  /  （构造函数 + 原型模式）使用原型对象来添加公用属性和方法，实现代码的复用，减少不必要的内存消耗<br />缺点：<br />如果原型对象上存在一个引用类型如 Array 这样的值，那么所有的实例将共享一个对象，一个实例对引用类型值的改变会影响所有的实例。
```javascript
function Person(name, age) { //在构造函数中只初始化一般函数
  this.name = name
  this.age = age
}
Person.prototype.setName = function (name) {
  this.name = name
}
```
<a name="ffde5"></a>
#### 5. 动态原型模式
动态原型模式将原型方法赋值的创建过程移动到了构造函数的内部，通过对属性是否存在的判断，可以实现仅在第一次调用函数时对原型对象赋值一次的效果
```javascript
function Perosn（姓名，年龄，工作）{
  //属性
  this.name = name;
  this.age =年龄;
  this.job =工作;
  //方法
  if（typeof this.sayName！=“function”）{
      Perosn.prototype.sayName = fucntion（）{
      	console（this.name）;
      }
  }
}
var friend = new Perosn（“csy”，18岁，“CEO”）;
friend.sayName（）;
```
<a name="do48D"></a>
#### 6. 寄生构造函数模式
寄生构造函数模式和工厂模式的实现基本相同，我对这个模式的理解是，它主要是基于一个已有的类型，在实例化时对实例化的对象进行扩展。这样既不用修改原来的构造函数，也达到了扩展对象的目的。<br />缺点：

1. 和工厂模式一样，无法实现对象的识别。
```javascript
function SpecialArray(){
 
    //创建数组
    var values = new Array();
 
    //添加值
    values.push.apply(values, arguments);
 
    //添加方法
    values.toPipedString = function(){
        return this.join("|");
    };
 
    //返回数组
    return values;
}
 
var colors = new SpecialArray("red", "blue", "green");
alert(colors.toPipedString()); //"red|blue|green"
```
<a name="pRCjj"></a>
### 2. 对象继承的方式有哪些？
<a name="VwXFv"></a>
#### （1）原型链继承
**原型链继承**缺点：

1. 在包含有引用类型的数据时，会被所有的实例对象所共享，容易造成修改的混乱。
2. **子类型在实例化时不能给父类型的构造函数传参。**
```javascript
function Supper() { //1.定义父类型构造函数
  this.superProp = 'The super prop'
}

Supper.prototype.showSupperProp = function () {//2.给父类型的原型添加方法
  console.log(this.superProp)
}

function Sub() { // 3.定义子类型的构造函数
  this.subProp = 'The sub prop'
}

Sub.prototype = new Supper();//4.创建父类型的对象赋值给子类型的原型

Sub.prototype.constructor = Sub//5.修正Sub.prototype.constructor为Sub本身

Sub.prototype.showSubProp = function () {//6.给子类型原型添加方法
  console.log(this.subProp)
}

var sub = new Sub()//7.创建子类型的对象: 可以调用父类型的方法

// 调用父类型的方法
sub.showSubProp()
// 调用子类型的方法
sub.showSupperProp()
```
<a name="nwTzz"></a>
#### （2）借用构造函数继承(假的)
借用构造函数继承(假的)这种方式是通过在子类型的函数中调用超类型的构造函数来实现的。<br />这一种方法解决了不能向父类型传递参数的缺点。<br />缺点：

1. **无法实现函数方法的复用**
2. **子类型没办法访问到，父类型原型定义的方法。**
```javascript
function Person(name, age) {
  this.name = name
  this.age = age
}

function Student(name, age, price) {
  Person.call(this, name, age)   // this.Person(name, age)
  this.price = price
}

var s = new Student('Tom', 20, 12000)
console.log(s.name, s.age, s.price)
```
<a name="Aqspj"></a>
#### （3）组合继承
组合继承组合继承是将原型链和借用构造函数组合起来使用的一种方式。通过借用构造函数的方式来实现类型的属性的继承，通过将子类型的原型设置为超类型的实例来实现方法的继承。<br />种方式解决了上面的两种模式单独使用时的问题<br />缺点：<br />由于我们是以超类型的实例来作为子类型的原型，所以**调用了两次超类的构造函数，造成了子类型的原型中多了很多不必要的属性。**
```javascript
function Person(name, age) {
  this.name = name
  this.age = age
}

Person.prototype.setName = function (name) {
  this.name = name
}

function Student(name, age, price) {
  Person.call(this, name, age) //得到父类型的属性
  this.price = price
}

Student.prototype = new Person()  //得到父类型的方法
Student.prototype.constructor = Student

Student.prototype.setPrice = function (price) {
  this.price = price
}

var s = new Student('Tom', 12, 10000)
console.log(s.__proto__.__proto__.__proto__.__proto__);//null

s.setPrice(11000)
s.setName('Bob')
console.log(s)
console.log(s.constructor)
```
<a name="cJDwy"></a>
#### （4）原型式继承
原型式继承**原型式继承的主要思路就是基于已有的对象来创建新的对象**，实现的原理是，向函数中传入一个对象，然后返回一个以这个对象为原型的对象。<br />这种继承的出发点是即使不自定义类型也可以通过原型实现对象之间的信息共享。ES5 中定义的 Object.create() 方法就是原型式继承的实现。<br />缺点：<br />**缺点与原型链方式相同。**
```javascript
// 中转函数
function obj(o) {
	function F() { }
	F.prototype = o;
	return new F();
}

// 对象创建 new xxx();
var person = {
	name: "lee",
	age: 21,
	family: ["弟弟", "姐姐", "妹妹"],
	run: function () {
		alert(0)
	}
}

var person1 = obj(person);
console.log(person1);
```
<a name="GRNTX"></a>
#### （5）寄生式继承
寄生式继承**寄生式继承的思路是创建一个用于封装继承过程的函数**，通过传入一个对象，然后复制一个对象的副本，然后对象进行扩展，最后返回这个对象。<br />寄生式继承同样适合主要关注对象，对一个简单对象实现继承，而不在乎类型和构造函数的场景<br />缺点：<br />通过寄生式继承给对象添加函数会导致函数难以重用，**与构造函数模式类似**。
```javascript
//中转函数
function object(o) {
    function F() { }
    F.prototype = o;
    return new F();
}
//寄生函数
function createAnother(original) {
    let clone = object(original); // 通过调用函数创建一个新对象  
    clone.sayHi = function () { // 以某种方式增强这个对象  
        console.log("hi");
    };
    return clone; // 返回这个对象 
}

let person = {
    name: "Nicholas",
    friends: ["Shelby", "Court", "Van"]
};
let anotherPerson = createAnother(person);
anotherPerson.sayHi(); // "hi" 
console.log(anotherPerson);
```
<a name="hA2LR"></a>
#### （6）寄生式组合继承
寄生式组合继承组合继承的缺点就是使用父类型的实例做为子类型的原型，导致添加了不必要的原型属性。<br />寄生式组合继承的方式是使用超类型的原型的副本来作为子类型的原型，这样就避免了创建不必要的属性。<br />说到底就是使用寄生式继承来继承父类原型，然后将返回的新对象赋值给子类原型。
```javascript
function object(o) {//中转函数
    function F() { }
    F.prototype = o;
    return new F();//返回包含父类原型的对象
}

function inheritPrototype(subType, superType) {
    let prototype = object(superType.prototype); // 创建对象（做子类的原型）
    prototype.constructor = subType; // 增强对象（增加constructor指向）
    subType.prototype = prototype; // 赋值对象 （给子类构造函数原型赋值）
}

function SuperType(name) {//被初始化到子类中
    this.name = name;
    this.colors = ["red", "blue", "green"];
}

SuperType.prototype.sayName = function () {//往父类原型上添加属性
    console.log(this.name);
};

function SubType(name, age) {
    SuperType.call(this, name);//初始化父类属性到子类中
    this.age = age;
}

//子类原型中包含引用值的时候，原型中包含的引用值会在所有实例间共享
inheritPrototype(SubType, SuperType);
SubType.prototype.sayAge = function () {//往子类原型上添加属性
    console.log(this.age);
}; 
```

**简化**
```javascript
function Parent(value) {
  this.val = value
}
Parent.prototype.getValue = function() {
  console.log(this.val)
}

function Child(value) {
  Parent.call(this, value)
}
Child.prototype = Object.create(Parent.prototype, {
  constructor: {
    value: Child,
    enumerable: false,
    writable: true,
    configurable: true
  }
})

const child = new Child(1)

child.getValue() // 1
child instanceof Parent // true

```
<a name="ig3cW"></a>
### 3. ES6中的 class 类

1. ES6 提供了更接近传统语言的写法，引入了 Class（类）这个概念，作为对象的模板。通过 class 关键字，可以定义类。基本上，ES6 的 class 可以看作只是一个语法糖，它的绝大部分功能，ES5 都可以做到，新的 class 写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已；
2. `class` 声明类；
3. `**constructor**`** 定义构造函数初始化；**
4. `extends` 继承父类；
5. `[super](https://blog.csdn.net/qwerty053/article/details/121310300)` 调用父级构造方法；**只能在派生类构造函数和静态方法中使用**。在静态方法中可以通过super调用继承的类上定义的静态方法：
   1. super既可以当函数使用，也可以当对象使用
      1. super作为函数，只能用在子类的构造函数中。super作为函数在子类的constructor中调用时，代表的是父类的构造函数。但是，虽然super代表的是父类的构造函数，但它内部的this指向的是当前子类的构造函数
      2. super用在普通方法中
         1. super指向父类的原型对象    super.cc
         2. 通过super调用父类方法时（super.print()），super内部的this指向子类的实例
         3. 当通过super为子类属性赋值时，super就是this
      3. super用在静态方法中
         1. super指向父类（不是父类的原型对象）
         2. 在子类的静态方法中通过super调用父类方法时，super内部的this指向子类（不是子类的实例）
6. `static` 定义静态方法和属性；
7. `get`和`set`获取和设置访问器
8. 父类方法可以重写
9. 函数受函数作用域限制，而类受块作用域限制
10. 类构造函数与构造函数的主要区别是，调用类构造函数必须使用new操作符。而普通构造函数如果不使用new调用，那么就会以全局的this（通常是window）作为内部对象。调用类构造函数时如果忘了使用new则会抛出错误：
11. 通过typeof操作符检测类标识符，表明它是一个函数
12. ~~类中定义的constructor方法不会被当成构造函数，在对它使用instanceof操作符时会返回false。但是，如果在创建实例时直接将类构造函数当成普通构造函数来使用，那么instanceof操作符的返回值会反转~~
<a name="TEFW8"></a>
#### 类声明
```javascript
class Shouji {
    //构造方法 名字不能修改 new Shouji时自动执行
    constructor(brand, price) {
        this.brand = brand;
        this.price = price;
    }
    
    //方法必须使用该语法, 不能使用 ES5 的对象完整形式
    call() {
        console.log("我可以打电话!!");
    }
}

let onePlus = new Shouji("1+", 1999);
console.log(onePlus);
```
<a name="Nwqru"></a>
#### class静态成员

1. 注意：实例对象和函数对象的属性是不相通的
```javascript
class Phone {
    //静态属性
    static name = '手机';
    static change() {
        console.log("我可以改变世界");
    }
}

let nokia = new Phone();
console.log(nokia.name);
console.log(Phone.name);
```
<a name="mheQl"></a>
#### 构造函数实现继承 / 重写
```javascript
class Phone {
    constructor(brand, price) {
        this.brand = brand;
        this.price = price;
    }
    call() {
        console.log("我可以打电话！");
    }
}

class SmartPhone extends Phone {
    // 构造函数
    constructor(brand, price, color, size) {
        super(brand, price); // 调用父类构造函数
        this.color = color;
        this.size = size;
    }
    photo() {
        console.log("我可以拍照！");
    }
    game() {
        console.log("我可以玩游戏！");
    }
	// 子类对父类方法重写(定义在子类原型上)
	// 注意：子类无法直接调用父类同名方法
    call(){
        console.log('我可以进行视频通话');
    }
}
const chuizi = new SmartPhone("小米", 1999, "黑色", "5.15inch");
console.log(chuizi);
chuizi.call();
chuizi.photo();
chuizi.game();
```
<a name="GAXW8"></a>
#### getter 和 setter

- class中的getter和setter设置
```javascript
class Example{
    constructor(a, b) {
        this.a = a; // 实例化时调用 set 方法
        this.b = b;
    }
    get a(){
        console.log('getter');
        return this.a;
    }
    set a(newVal){
        console.log('setter');
        this.a = newVal; // 自身递归调用
    }
}
let exam = new Example(1,2); // 不断输出 setter ，最终导致 RangeError

class Example1{
    constructor(a, b) {
        this.a = a;
        this.b = b;
    }
    get a(){
        console.log('getter');
        return this._a;
    }
    set a(a){
        console.log('setter');
        this._a = a;
    }
}
let exam1 = new Example1(1,2); // 只输出 setter , 不会调用 getter 方法
console.log(exam1.a); // 1, 可以直接访问
```
```javascript
class Supper {
    static my = 'my'

    constructor(name, age) {
        this.name = name;
        this.age = age
    }

    show() {
        console.log('this.name', this.name)
        console.log('this.age', this.age)
    }

}


class Sub extends Supper {
    
    constructor(name, age, subProp) {
        super(name, age)
        this.subProp = subProp
    }
    
    subMethod() {
        console.log('sunMethod')
    }
    
    show() {
        console.log(this.name, this.age)
    }
    
    get subProp() {
        console.log('get')
        return this._subProp
    }
    
    set subProp(val) {
        console.log('set')
        this._subProp = val
    }
    
}
const sub = new Sub('mfk', 21, 'sub')
```
<a name="vuhAO"></a>
#### 抽象基类

- 有时候可能需要定义这样一个类，它可供其他类继承，但本身不会被实例化。虽然ECMAScript没有专门支持这种类的语法 ，但通过`new.target`也很容易实现。new.target保存通过new关键字调用的类或函数。通过在实例化时检测new.target是不是抽象基类，可以阻止对抽象基类的实例化：
```javascript
class Vehicle {
    constructor() {
        console.log(new.target);
        if (new.target === Vehicle) {
            throw new Error('Vehicle cannot be directly instantiated');
        }
    }
}
// 派生类
class Bus extends Vehicle { }
new Bus(); //  class Bus {}
new Vehicle(); // class Vehicle {}
// Error: Vehicle cannot be directly instantiated
```

- 另外，通过在抽象基类构造函数中进行检查，可以要求派生类必须定义某个方法。因为原型方法在调用类构造函数之前就已经存在了，所以可以通过`this`关键字来检查相应的方法：
```javascript
// 抽象基类 
class Vehicle {
    constructor() {
        if (new.target === Vehicle) {
            throw new Error('Vehicle cannot be directly instantiated');
        }
        if (!this.foo) {
            throw new Error('Inheriting class must define foo()');
        }
        console.log('success!');
    }
}
// 派生类 
class Bus extends Vehicle { foo() { } }
// 派生类 
class Van extends Vehicle { }
new Bus(); // success! 
new Van(); // Error: Inheriting class must define foo()
```
<a name="Nd4yk"></a>
### 4. 什么是面向对象编程及面向过程编程，它们的异同和优缺点

- **面向过程就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候一个一个依次调用就可以了**
- **面向对象是把构成问题事务分解成各个对象，建立对象的目的不是为了完成一个步骤，而是为了描叙某个事物在整个解决问题的步骤中的行为**
- **面向对象是以功能来划分问题，而不是步骤**
<a name="hxkt4"></a>
### 5.  面向对象编程思想

- **基本思想是使用对象，类，继承，封装等基本概念来进行程序设计**
- 优点
   - 易维护
      - 采用面向对象思想设计的结构，可读性高，由于继承的存在，即使改变需求，那么维护也只是在局部模块，所以维护起来是非常方便和较低成本的
   - 易扩展
   - 开发工作的重用性、继承性高，降低重复操作。
   - 缩短了开发周期

<a name="c3919e1de0c89feca0203f788eb6d030"></a>
## 执行上下文/作用域链/闭包
<a name="NNzpq"></a>
### 1. 闭包
<a name="h92y1"></a>
#### 闭包的定义

1. 一个函数和对其`词法环境`的引用捆绑在一起，这样的组合就是"闭包"（closure）。也就是说，闭包让你可以在一个内层函数中访问到其外层函数的作用域。在 JavaScript 中，每当创建一个函数，闭包就会在函数创建的同时被创建出来
<a name="pqvbb"></a>
#### 产生闭包的条件

1. **函数嵌套,内部函数引用了外部函数内的(变量/函数)**
<a name="VRSaE"></a>
#### 产生和销毁时期

1. **函数被创建（定义）后就形成**
2. **在嵌套的内部函数成为垃圾对象时销毁**
<a name="AeH8l"></a>
#### 闭包到底是什么

1. 理解一:** 闭包是能够读取嵌套外部函数中变量的内部函数，他保存了上层上下文的作用域**
2. 理解二: 
   1. **闭包是包含被引用变量(函数)的对象**，**存在于嵌套的内部函数中，**
   2. **函数创建时，扫描函数内的标识符引用，把用到父函数中作用域的变量打成 Closure （闭包）**，**放到 函数[[Scopes]]（作用域链）属性中**，~~[[Scopes]]里 的Closure+全局对象Global~~**组成新的作用域链**，
   3. 自此函数保存着父函数的（上下文变量对象的引用）（作用域的引用），即使父函数销毁了也能访问外部引用。
<a name="cNY3t"></a>
#### 闭包的优缺点

1. 优点
   1. 可以将一个变量**驻留到内存中 **不会被垃圾回收机制回收
   2. **让函数外部可以操作(读写)到函数内部的数据(变量/函数)**
   3. **（模拟一个私有作用域）**加强封装性，实现了对变量的隐藏和封装，可以避免全局变量的污染
2. 缺点
   1. 因为函数执行完后，执行上下文变量对象不被释放，所以会导致**内存消耗很大**，增加了内存消耗量，影响网页性能
   2. 过度的使用闭包可能会导致**内存泄露**，或程序加载运行过慢卡顿等问题出现。
- **好处**：能够实现封装和缓存等；
- **坏处**：就是消耗内存、不正当使用会造成内存溢出的问题
<a name="PDRpC"></a>
#### 闭包经典使用场景
使用闭包主要是为了设计私有的方法和变量。
**封装私有变量 **```javascript
//使用闭包模拟私有方法,在JavaScript中，没有支持声明私有变量，但我们可以使用闭包来模拟私有方法

var Counter = (function() {
  var privateCounter = 0;
  function changeBy(val) {
    privateCounter += val;
  }
  return {
    increment: function() {
      changeBy(1);
    },
    decrement: function() {
      changeBy(-1);
    },
    value: function() {
      return privateCounter;
    }
  }
})();
 
var Counter1 = makeCounter();
var Counter2 = makeCounter();
console.log(Counter1.value()); /* logs 0 */
Counter1.increment();
Counter1.increment();
console.log(Counter1.value()); /* logs 2 */
Counter1.decrement();
console.log(Counter1.value()); /* logs 1 */
console.log(Counter2.value()); /* logs 0 */
//上述通过使用闭包来定义公共函数，并令其可以访问
```
**缓存——防抖、节流**```javascript
// 防抖
function debounce(fn, delay = 300) {
  let timer; //闭包引用的外界变量
  return function () {
    const args = arguments;
    if (timer) {
      clearTimeout(timer);
    }
    timer = setTimeout(() => {
      fn.apply(this, args);
    }, delay);
  };
}

```
**模块化  IIFE（自执行函数）**```javascript
(function () {
    var a = 10;
    var b = 20;

    function add(num1, num2) {
        var num1 = !!num1 ? num1 : a;
        var num2 = !!num2 ? num2 : b;

        return num1 + num2;
    }

    window.add = add;
})();

add(10, 20);Copy to clipboardErrorCopied
```
**缓存**```javascript
//比如求和操作，如果没有缓存，每次调用都要重复计算，采用缓存已经执行过的去查找，查找到了就直接返回，不需要重新计算

var fn = (function () {
    var cache = {};//缓存对象
    var calc = function (arr) {//计算函数
        var sum = 0;
        //求和
        for (var i = 0; i < arr.length; i++) {
            sum += arr[i];
        }
        return sum;
    }

    return function () {
        var args = Array.prototype.slice.call(arguments, 0);//arguments转换成数组
        var key = args.join(",");//将args用逗号连接成字符串
        var result, tSum = cache[key];
        if (tSum) {//如果缓存有   
            console.log('从缓存中取：', cache)//打印方便查看
            result = tSum;
        } else {
            //重新计算，并存入缓存同时赋值给result
            result = cache[key] = calc(args);
            console.log('存入缓存：', cache)//打印方便查看
        }
        return result;
    }
})();
fn(1, 2, 3, 4, 5);
fn(1, 2, 3, 4, 5);
fn(1, 2, 3, 4, 5, 6);
fn(1, 2, 3, 4, 5, 8);
fn(1, 2, 3, 4, 5, 6);
```
**柯里化函数**```javascript
//柯里化的目的在于避免频繁调用具有相同参数函数的同时，又能够轻松的重用
// 假设我们有一个求长方形面积的函数
function getArea(width, height) {
    return width * height
}
// 如果我们碰到的长方形的宽老是10
const area1 = getArea(10, 20)
const area2 = getArea(10, 30)
const area3 = getArea(10, 40)
 
// 我们可以使用闭包柯里化这个计算面积的函数
function getArea(width) {
    return height => {
        return width * height
    }
}
 
const getTenWidthArea = getArea(10)
// 之后碰到宽度为10的长方形就可以这样计算面积
const area1 = getTenWidthArea(20)
 
// 而且如果遇到宽度偶尔变化也可以轻松复用
const getTwentyWidthArea = getArea(20)
```
词法作用域：在JavaScirpt在解析时 会根据我们的原始的代码去确定某一个变量所生效的范围
<a name="23c9abe7635a9a1228d9001d34e8da61"></a>
### 2. 对执行上下文的理解

1. 创建执行上下文有两个阶段：**创建阶段**和**执行阶段**
2. **执行上下文是 JavaScript 代码被解析和执行时所在环境的抽象概念。**
<a name="HXsKP"></a>
#### 执行上下文的类型

- **全局执行上下文**——一个程序中只有一个全局上下文window（执行全局代码前创建），它在整个 javascript 脚本的生命周期内都会存在于执行堆栈的最底部不会被栈弹出销毁。
- **函数执行上下文**——每当一个函数被调用时，都会创建一个新的函数执行上下文
- **Eval 函数执行上下文**—— 执行在 eval 函数内部的代码也会有它属于自己的执行上下文
<a name="MEPqb"></a>
#### 执行上下文的内容
执行上下文是一个抽象的概念，我们可以将它理解为一个对象 ，一个执行上下文里包括以下内容：

1. **调用者信息(this)**
2. **作用域链（scope chain**） 
   1. 由多个执行上下文的**变量对象(VO)**构成的链表就叫做 **作用域链**。
3. **变量对象或活动对象 **
   1. 每个上下文都有一个关联的变量对象（variable object 简称 VO），而这个上下文中定义的所有变量和函数都存在于这个对象上。全局上下文中的叫变量对象，它会在代码执行期间始终存在。
   2. 而函数局部上下文中的叫活动对象（activation object 简称 AO），只在函数执行期间存在。函数进入执行阶段时，原本不能访问的变量对象被激活成为一个可访问其中的各种属性的活动对象，其实变量对象和活动对象是一个东西，只不过处于不同的状态和阶段而已。 
      1. 变量对象是在函数被调用，但是函数尚未执行的时刻被创建的
      2. 进入执行阶段之后，变量对象转变为了活动对象
<a name="Bxgll"></a>
#### 执行上下文的生命周期
<a name="pLvSy"></a>
##### 函数执行上下文创建阶段
发生在函数调用时且在执行函数体内的具体代码之前

1. **对上下文中数据预处理，作为变量对象的属性**
   1. 在函数被调用时且在具体的**函数代码运行之前**，会为其创建一个**Arguments对象**，JS 引擎会用当前函数的**参数列表**（`arguments`）初始化一个 “变量对象” 并将当前执行上下文与之**关联** ，函数代码块中用var声明的 **变量** (值为undefined)和 (函数声明形势)声明的**函数** 将作为属性添加到这个变量对象上（变量提升）。
2. **构建作用域链** 
   1. **当函数创建时，会有一个名为 **`**[[scope]]**`** 的内部属性保存所有父级变量对象(VO)到其中。在调用这个函数时，会创建相应的执行上下文，然后通过复制函数的**`**[[Scope]]**`**来创建其作用域链，随后将上下文的**`**活动对象**`**推入作用域链的前端，完整的作用域链创建完成。**
   2. 作用域链其实是一个包含指针的列表，每个指针分别指向一个变量对象，但物理上并不会包含相应的对象。
3.  **确定 this 的值 **
   1. 若当前函数被作为对象方法调用或使用 `bind` `call` `apply` 等 `API` 进行委托调用，则将当前代码块的调用者信息（`this value`）存入当前执行上下文，否则默认为全局对象调用。
   2. `this` 的值取决于函数的调用方式。具体有：默认绑定、隐式绑定、显式绑定（硬绑定）、`new`绑定、箭头函数，具体内容会在【this全面解析】部分详解。
<a name="TKgc7"></a>
##### 全局执行上下文创建阶段
全局代码执行之前

1. **对上下文中数据预处理，作为变量对象的属性 **
   1. 将var声明的变量(值为undefined)和(函数声明形势)声明的函数作为属性添加到全局上下文中的变量对象上（变量提升），全局上下文就是我们常说的window对象，因此所有通过var定义的全局变量和函数都会成为window对象的属性和方法。
2. **构建作用域链：全局上下文作用域为自身**
3. **确定 **`**this**`** 的值：全局上下文this为window**
<a name="sqiuW"></a>
##### 执行阶段
执行阶段中，JS 代码开始逐条执行，在这个阶段，JS  引擎开始对定义的变量赋值、开始顺着作用域链访问变量、如果内部有函数调用就创建一个新的执行上下文压入执行栈并把控制权交出……
<a name="nOpFs"></a>
##### 销毁阶段
一般来讲当函数执行完成后，当前执行上下文（局部环境）会被弹出执行上下文栈并且销毁，控制权被重新交给执行栈上一层的执行上下文。
<a name="mbIHD"></a>
#### ES5 中的执行上下文
`ES5` 规范又对 `ES3` 中执行上下文的部分概念做了调整，最主要的调整，就是**去除了 **`**ES3**`** 中变量对象和活动对象，以 **`**词法环境**`**和 **`**变量环境**`**替代**

- 词法环境和变量环境的一个不同就是前者被用来存储`函数声明`和（`let` 和 `const`）声明变量，而后者只用来存储 `var` 定义的变量
- 创建阶段，会在代码中扫描变量和函数声明，然后将函数声明存储在环境中，但变量会被初始化为`undefined`(`var`声明的情况下)和保持`uninitialized`(未初始化状态)(使用`let`和`const`声明的情况下)
- **词法环境和变量环境的内部有两个组件**
- `**外部环境的引用**`：它指向父级词法环境（作用域）（全局执行上下文时为null）
- `**环境记录器**`是存储变量和函数声明的实际位置。 
   - `**声明式环境记录器**`存储变量、函数和参数，arguments（函数执行上下文）
   - `**对象环境记录器**`用来定义出现在全局上下文中的变量和函数的关系。（全局执行上下文）
<a name="FtQR8"></a>
#### ES5 中的执行上下文
**词法环境和 变量环境：**定义标识符和具体变量和函数的关联，前者被用来存储`函数声明`和（`let` 和 `const`）声明变量，而后者只用来存储 `var` 定义的变量

1. **外部环境的引用**：它指向父级词法环境（作用域）（全局执行上下文时为null）
2. **环境记录器**：存储变量和函数声明的实际位置。 
   1. **声明式环境记录器：**存储变量、函数和参数，arguments（函数执行上下文）
   2. **对象环境记录器：**用来定义出现在全局上下文中的变量和函数的关系。（全局执行上下文）
3. **通俗点解释就是，在词法环境中，环境记录器记录保存了执行上下文中的变量和函数的值，外部环境的引用使得该执行上下文可以沿着作用域链访问父级的作用域。**

1. 程序启动，全局上下文被创建 
   1. 创建全局上下文的 **词法环境**
      1. 创建 **对象环境记录器** ，它用来定义出现在 **全局上下文** 中的变量和函数的关系（负责处理 let 和 const 定义的变量）
      2. 创建 **外部环境引用**，值为 **null**
   2. 创建全局上下文的 **变量环境**
      1. 创建 **对象环境记录器**，它持有 **变量声明语句** 在执行上下文中创建的绑定关系（负责处理 var 定义的变量，初始值为 undefined 造成声明提升）
      2. 创建 **外部环境引用**，值为 **null**
   3. 确定 this 值为全局对象（以浏览器为例，就是 window ）
2. 函数被调用，函数上下文被创建 
   1. 创建函数上下文的 **词法环境**
      1. 创建 **声明式环境记录器** ，存储变量、函数和参数，它包含了一个传递给函数的 **arguments** 对象（此对象存储索引和参数的映射）和传递给函数的参数的 **length**。（负责处理 let 和 const 定义的变量）
      2. 创建 **外部环境引用**，值为全局对象，或者为父级词法环境（作用域）
   2. 创建函数上下文的 **变量环境**
      1. 创建 **声明式环境记录器** ，存储变量、函数和参数，它包含了一个传递给函数的 **arguments** 对象（此对象存储索引和参数的映射）和传递给函数的参数的 **length**。（负责处理 var 定义的变量，初始值为 undefined 造成声明提升）
      2. 创建 **外部环境引用**，值为全局对象，或者为父级词法环境（作用域）
   3. 确定 this 值
3. 进入函数执行上下文的执行阶段： 
   1. 在上下文中运行/解释函数代码，并在代码逐行执行时分配变量值。

关于 ES5 中执行上下文的变更，个人感觉就是变了个概念，本质和 ES3 差别并不大。至于变更的目的，**应该是为了 ES6 中的 let 和 const 服务的。**

<a name="Ki5TY"></a>
#### 执行上下文栈
执行栈，也叫调用栈，具有 LIFO（**后进先出**）结构，用于存储在代码执行期间创建的所有执行上下文

1. 在全局代码执行前, JS引擎就会创建一个栈来存储管理所有的执行上下文对象
2. 在全局执行上下文(window)确定后, 将其添加到栈中(压栈)
3. 在函数执行上下文创建后, 将其添加到栈中(压栈)
4. 在当前函数执行完后,将栈顶的对象移除(出栈)
5. 当所有的函数代码执行完后, 栈中只剩下window

![image.png](https://cdn.nlark.com/yuque/0/2022/png/28203632/1662381228720-b47dd5ee-5375-4ea4-af9a-91e84358350f.png#averageHue=%23dfd5cd&clientId=u50684678-1bf9-4&errorMessage=unknown%20error&from=paste&height=176&id=u8bb2779b&originHeight=176&originWidth=940&originalType=binary&ratio=1&rotation=0&showTitle=false&size=28684&status=error&style=stroke&taskId=u12550a16-de23-4acb-b247-b683b7981ea&title=&width=940)
<a name="MewNF"></a>
### 3. 对作用域、作用域链的理解

1. **变量起作用的范围和区域，目的是隔离变量。分类：全局、函数、块级作用域。**
2. 它是静态的(相对于上下文对象), 在编写代码时就确定了
3. 执行上下文(对象)是从属于所在的作用域
<a name="ucidJ"></a>
##### 1）全局作用域和函数作用域
**（1）全局作用域**

   1. 最外层函数和最外层函数外面定义的变量拥有全局作用域
   2. **所有未定义直接赋值的变量自动声明为全局作用域**
   3. 所有window对象的属性拥有全局作用域
   4. 全局作用域有很大的弊端，过多的全局作用域变量会污染全局命名空间，容易引起命名冲突。

**（2）函数作用域**

   1. 函数作用域声明在函数内部的变量，一般只有固定的代码片段可以访问到
   2. 作用域是分层的，内层作用域可以访问外层作用域，反之不行
<a name="UFuAr"></a>
##### 2）块级作用域

- 使用ES6中新增的let和const class指令声明的变量拥有块级作用域，块级作用域可以在函数中创建也可以在一个代码块中的创建（由`{ }`包裹的代码片段）
- let和const声明的变量不会有变量提升，也不可以重复声明
- 在循环中比较适合绑定块级作用域，这样就可以把声明的计数器变量限制在循环内部。

<a name="gFTXr"></a>
##### 3）作用域链：
> 1. **作用域链是指由当前上下文和上层上下文的一系列变量对象（VO）组成的层级链。**
> 2. **当函数创建时，会有一个名为 **`**[[scope]]**`** 的内部属性保存所有父级变量对象(VO)到其中。在调用这个函数时，会创建相应的执行上下文，然后通过复制函数的**`**[[Scope]]**`**来创建其作用域链，然后会创建函数的活动对象（用作变量对象）并将其推入作用域链的前端，完整作用域链创建完成。**
> 3. **作用域链的作用是保证执行环境里有权访问的变量和函数是有序的，作用域链的变量只能向上访问，变量访问到window对象即被终止，作用域链向下访问变量是不被允许的**

更多在当前作用域中查找所需变量，但是该作用域没有这个变量，那这个变量就是自由变量。如果在自己作用域找不到该变量就去父级作用域查找，依次向上级作用域查找，直到访问到window对象就被终止，这一层层的关系就是作用域链。<br />作用域链的作用是**保证对执行环境有权访问的所有变量和函数的有序访问，通过作用域链，可以访问到外层环境的变量和函数。**作用域链的变量只能向上访问，变量访问到window对象即被终止，作用域链向下访问变量是不被允许的，简单的说，作用域就是变量与函数的可访问范围，即作用域控制着变量与函数的可见性和生命周期<br />作用域链的本质上是一个指向变量对象的指针列表。变量对象是一个包含了执行环境中所有变量和函数的对象。作用域链的前端始终都是当前执行上下文的变量对象。全局执行上下文的变量对象（也就是全局对象）始终是作用域链的最后一个对象。
<a name="J97KQ"></a>
### 4.  变量提升和函数提升
**函数提升优先级高于变量提升，且不会被同名变量声明时覆盖，但是会被变量赋值后覆盖**

1. 变量声明提升 
   1. 通过var定义(声明)的变量, 在定义语句之前就可以访问到
   2. 值: undefined
2. 函数声明提升 
   1. 通过function声明的函数, 在之前就可以直接调用
   2. 值: 函数定义(对象)
3. 如何产生的? 
   1. 在代码运行之前，会初始化一个 “变量对象” 并将当前执行上下文与之关联 ，函数代码块中用var声明的 **变量** (值为undefined)和 (函数声明形势)声明的**函数** 将作为属性添加到这个变量对象上
4. `本层作用域有没有此变量【注意变量提升】`
5. 优先级：声明变量>声明普通函数>参数>变量提升

<a name="US1b1"></a>
## 异步编程
<a name="fqSpl"></a>
### 1. 并发与并行的区别？

- 并发是宏观概念，我分别有任务 A 和任务 B，在一段时间内通过任务间的切换完成了这两个任务，这种情况就可以称之为并发。
- 并行是微观概念，假设 CPU 中存在两个核心，那么我就可以同时完成任务 A、B。同时完成多个任务的情况就可以称之为并行。
<a name="646941bb49ab3bd2c744ebedb8f6d896"></a>
### 2. 什么是回调函数？回调函数有什么缺点？如何解决回调地狱问题？
以下代码就是一个回调函数的例子：
```javascript
ajax(url, () => {
    // 处理逻辑
})
```
回调函数有一个致命的弱点，就是容易写出回调地狱（Callback hell）。假设多个请求存在依赖性，可能会有如下代码：
```javascript
ajax(url, () => {
    // 处理逻辑
    ajax(url1, () => {
        // 处理逻辑
        ajax(url2, () => {
            // 处理逻辑
        })
    })
})
```
以上代码看起来不利于阅读和维护，当然，也可以把函数分开来写：
```javascript
function firstAjax() {
  ajax(url1, () => {
    // 处理逻辑
    secondAjax()
  })
}
function secondAjax() {
  ajax(url2, () => {
    // 处理逻辑
  })
}
ajax(url, () => {
  // 处理逻辑
  firstAjax()
})
```
以上的代码虽然看上去利于阅读了，但是还是没有解决根本问题。回调地狱的根本问题就是：

1. 嵌套函数存在耦合性，一旦有所改动，就会牵一发而动全身
2. 可阅读性差
3. 嵌套函数一多，就很难处理错误

当然，回调函数还存在着别的几个缺点，比如不能使用 `try catch` 捕获错误，不能直接 `return`。
<a name="K39kj"></a>
### 3. 异步编程的实现方式？
JavaScript中的异步机制可以分为以下几种：

- **回调函数 **的方式，使用回调函数的方式有一个缺点是，多个回调函数嵌套的时候会造成回调函数地狱，上下两层的回调函数间的代码耦合度太高，不利于代码的可维护。
- **Promise** 的方式，使用 Promise 的方式可以将嵌套的回调函数作为链式调用。但是使用这种方法，**有时会造成多个 then 的链式调用，可能会造成代码的语义不够明确**。
- **generator **的方式，**它可以在函数的执行过程中，将函数的执行权转移出去，在函数外部还可以将执行权转移回来。当遇到异步函数执行的时候，将函数执行权转移出去，当异步函数执行完毕时再将执行权给转移回来**。**因此在 generator 内部对于异步操作的方式，可以以同步的顺序来书写。**使用这种方式需要考虑的问题是何时将函数的控制权转移回来，因此需要有一个自动执行 generator 的机制，比如说 co 模块等方式来实现 generator 的自动执行。
- **async 函数 **的方式，**async 函数是 generator 和 promise 实现的一个自动执行的语法糖**，它内部自带执行器，当函数内部执行到一个 await 语句的时候，**如果语句返回一个 promise 对象，那么函数将会等待 promise 对象的状态变为 resolve 后再继续向下执行。因此可以将异步逻辑，转化为同步的顺序来书写，**并且这个函数可以自动执行。
- **事件监听**(采用时间驱动模式，取决于某个事件是否发生)：
   - 优点：容易理解，可以绑定多个事件，每个事件可以指定多个回调函数
   - 缺点：事件驱动型，流程不够清晰
- **发布/订阅(观察者模式)**
   - 类似于事件监听，但是可以通过‘消息中心’，了解现在有多少发布者，多少订阅

**异步回调**```javascript
// ES5 -> 异步回调 -> 回调函数
function Fn(asyncCallback) {
	var i = 0;
	// 异步操作
	setTimeout(function () {
		i = 100;
		asyncCallback(i);
	})
}

Fn(function (result) {
	console.log(result);
});
```
<a name="vHWEs"></a>
### 4. 定时器![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/270f.svg#height=18&id=vMJC4&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)
<a name="JoDVi"></a>
#### setTimeout()---clearTimeout()  / setInterval()---clearInterval()
setTimeout()---clearTimeout()
1. 延时调用一个函数不马上执行，而是隔一段时间以后在执行，而且只会执行一次
2. 延时调用和定时调用的区别，定时调用会执行多次，而延时调用只会执行一次
3. 延时调用和定时调用实际上是可以互相代替的，在开发中可以根据自己需要去选择
```javascript
var timer = setTimeout(function () {
	console.log("单次定时器");
}, 3000);
//使用clearTimeout()来关闭一个延时调用
clearTimeout(timer);
```
setInterval()---clearInterval()
1. 可以将一个函数，每隔一段时间执行一次
2. 参数：
   1. 回调函数，该函数会每隔一段时间被调用一次
   2. 每次调用间隔的时间，单位是毫秒
3. 返回值：
   1. 返回一个Number类型的数据,这个数字用来作为定时器的唯一标识
```javascript
var timer = setinterval(function () {
	console.log("单多次执行的定时器次定时器");
}, 3000);
//使用 clearInterval()来关闭一个定时调用
clearInterval(timer)
```
<a name="GZfYb"></a>
#### 使用 setTimeOut 模拟 setInterval![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/270f.svg#height=18&id=lBvSZ&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)
JavaScript中使用 setInterval 开启轮询。定时器代码可能在代码再次被添加到队列之前还没有完成执行，结果导致定时器代码连续运行好几次，而之间没有任何停顿。而javascript引擎对这个问题的解决是：当使用setInterval()时，仅当没有该定时器的任何其他代码实例时，才将定时器代码添加到队列中。这确保了定时器代码加入到队列中的最小时间间隔为指定间隔。<br />但是，这样会导致两个问题：

- **某些间隔被跳过；**
- **多个定时器的代码执行之间的间隔可能比预期的小**

setTimeout 模拟实现 seTinterval js单线程，在线程占用时间较长的情况下，seTinterval可能会向任务队列里添加很多宏任务 这些宏任务在线程空下来的时候，会依次执行，而不会间隔执行，导致失效 所以使用setTimeout+递归来模拟，只有前一次任务执行了之后，才添加下一次任务
```javascript
var i = 0;
function run(delay) {
	console.log(++i);
	if (i <= 5) {
		setTimeout(arguments.callee.bind(null, delay), delay);
	}
}

setTimeout(run.bind(null, 1000), 1000);
console.log(1);
```
<a name="W6ZZl"></a>
#### 使用 setInterval 模拟 setTimeOut ![](https://gw.alipayobjects.com/os/lib/twemoji/11.2.0/2/svg/270f.svg#height=18&id=CMfGG&originHeight=150&originWidth=150&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=18)
```javascript
function mySetTimeout(fn, time) {
    let timer = setInterval(() => {
        clearInterval(timer)
        fn()
    }, time);
}

// mySetTimeout(() => {
//     console.log(1);
// }, 1000)
```
<a name="wmEoc"></a>
#### reuqestAnimationFrame
简介：<br />显示器都有自己固有的刷新频率(60HZ或者75HZ)，也就是说每秒最多重绘60次或者75次。而requestAnimationFrame的基本思想就是与这个刷新频率保持同步，利用这个刷新频率进行重绘。<br />特点：

- **使用这个API时，一旦页面不处于浏览器的当前标签，就会自动停止刷新**，这样就节省了CPU、GPU、电力。
- 由于它时在主线程上完成的，所以若是主线程非常忙时它的动画也会收到影响
- **它使用一个回调函数作为参数，这个回调函数会在浏览器重绘之前调用。**
requestAnimationFrame对比setTimeout
- **屏幕刷新频率** 屏幕每秒出现图像的次数。普通笔记本为60Hz
- **动画原理** 计算机每16.7ms刷新一次，由于人眼的视觉停留，所以看起来是流畅的移动。
- **setTimeout** 通过设定间隔时间来不断改变图像位置，达到动画效果。但是容易出现卡顿抖动的现象；原因是：
   - settimeout 任务被放入异步队列，只有当主线程任务执行完后才会执行队列中的任务，因此实际执行时间总是比设定时间要晚；
   - settimeout 的**固定时间间隔不一定与屏幕刷新时间相同**，**会引起丢帧**。

requestAnimationFrame 优势：由系统决定回调函数的执行时机。60Hz的刷新频率，那么每次刷新的间隔中会执行一次回调函数，不会引起丢帧，不会卡顿。且由于一旦页面不处于浏览器的当前标签，就**会自动停止刷新**，这样就节省了CPU、GPU、电力。
```javascript
function myRequestAnimationFrame(fn) {
    let stopAnimationFrame = false;

    window.requestAnimationFrame(function () {
        fn()
        if (!stopAnimationFrame) {
            window.requestAnimationFrame(arguments.callee);
        }
    });

    myRequestAnimationFrame.stop = () => {
        stopAnimationFrame = true;
    }
}
```
```javascript
// 让我们在HTML网页中去使用JS编写动画使用的
// 编写JS的动画库
// Canvas 动画
var i = 1;
var stopAnimationFrame = null;

stopAnimationFrame = window.requestAnimationFrame(function () {
	i++;
    //stopAnimationFrame 每一次都不一样
	stopAnimationFrame = window.requestAnimationFrame(arguments.callee);
	if (i > 200) {
		window.cancelAnimationFrame(stopAnimationFrame); 
		return;
	}
	console.log(i);
});
```
<a name="kIz6v"></a>
#### setInterval 的问题
**无视代码报错   （代码报错）会继续执行**```javascript
setInterval(function () {
	console.log(1)
	a();
	console.log(2);
})
//使用setTimeout报错停止运行
setTimeout(function () {
	console.log(1)
	setTimeout(arguments.callee);
});
//使用requestAnimationFrame报错停止运行
requestAnimationFrame(function () {
	console.log(1)
	a();
	requestAnimationFram(arguments.callee)
});
```
**无视网络延迟**```javascript
// 实现网页聊天   长轮询
setInterval(function () {
	console.log("setInterval");
	// 请求堆积
	// 一次请求有了结果
	setTimeout(function () {
		console.log("setTimeout");
	}, 2000)
}, 1000);

//等上一次有张结果了再发送请求
setTimeout(function () {
	var fn = arguments.callee;
	console.log("外层TimeOut");
	// 请求
	setTimeout(function () {
		console.log("请求有结果了");
		setTimeout(fn, 1000);
	}, 2000);
}, 1000);
```
**执行时间错乱**```javascript
//执行时间错乱
setInterval(function () {
	for (var i = 0; i < 500000; i++) {
		var d = document.createElement("div");
		d.innerHTML = i;
		document.body.appendChild(d);
	}
	var date = new Date();
	console.log(
		date.getHours() +
		":" +
		date.getMinutes() +
		":" +
		date.getSeconds() +
		":" +
		date.getMilliseconds()
	);
}, 2000);
```
**性能问题**
1. **setInterval 他的执行频率太高了  就算你手动设置一秒钟执行60个帧有时候也会卡顿 因为你不确定当前浏览器的一个刷新频率是多少,window.requestAnimationFrame 会根据浏览器的刷新频率进行自动的调整**
2. **而且在网页标签页不在当前窗口中 requestAnimationFrame会暂停执行 回到当前窗口中会继续执行**
3. **而且使用requestAnimationFrame做出来的动画更加流畅 丝滑！**
<a name="rUw8V"></a>
#### 定时器的第三个参数（闭包）
```javascript
//闭包
for (var i = 0; i < 5; i++) {
	setTimeout((res) => {
		console.log(res);
	}, 1000, i);//setTimeout的第三个参数
}
```
<a name="KetyK"></a>
### 5. Promise

1. 什么是回调地狱?
   1. 回调函数嵌套调用, **外部回调函数异步执行的结果是嵌套的回调执行的条件**
2. 回调地狱的缺点?
   1. 不便于阅读
   2. 不便于异常处理
<a name="j8Z1A"></a>
#### Promise 是什么?

1. 抽象表达: 
   1. Promise 是一门新的技术(ES6 规范)
   2. **Promise 是 JS 中进行异步编程的新解决方案，可解决回调地狱问题**<br />备注：旧方案是单纯使用回调函数
2. 具体表达: 
   1. 从语法上来说: Promise 是一个构造函数
   2. 从功能上来说: promise 对象用来封装一个异步操作并可以获取其成功/失败的结果值
3. 优点
   1. **指定回调函数的方式更加灵活**
      1. 旧的: 必须在启动异步任务前指定
      2. promise: 启动异步任务 => 返回promie对象 => 给promise对象绑定回调函数(甚至可以在异步任务结束后指定/多个)
   2. **支持链式调用, 可以解决回调地狱问题**
4.  Promise 优缺点
   1. 无法取消 Promise，一旦新建它就会立即执行，无法中途取消。
   2. 当处于 Pending 状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。
   3. 如果不设置回调函数，Promise 内部抛出的错误，不会反应到外部。
5. 终极解决方案?：async/await
<a name="ntUSE"></a>
#### 改变 promise 的状态
**改变 promise 的状态**
1. 解决：**resolve**(value): 如果当前是 pending 就会变为 resolved
2. 拒绝：**reject**(reason): 如果当前是 pending 就会变为 rejected
3. **抛出异常**: 如果当前是 pending 就会变为 rejected
4. **『PromiseState』**保存着promise 的状态 
   1. **pending ** 未决定的
   2. **resolved** / fullfilled  成功
   3. **rejected ** 失败
5. **『PromiseResult』**,保存着异步任务『成功/失败』的结果
   1. 可被 resolve 和 reject 修改
```javascript
const p = new Promise((resolve, reject) => {
    setTimeout(() => {
        let n = rand(1, 100);
        if (n <= 30) {
            resolve(n); // 将 promise 对象的状态设置为 『成功』
        } else {
            reject(n); // 将 promise 对象的状态设置为 『失败』
        }
    }, 1000);
});
```
<a name="A2v1c"></a>
#### then  /  返回新promise状态
**p.then(onResolved, onRejected) => {} **返回的新 promise 的结果状态由什么决定?then()指定的回调函数执行的结果决定

1. **抛出异常,** 新 promise 变为 rejected, reason 为抛出的异常
2. 返回的是**非promise**的任意值, 新promise变为resolved, value为返回的值
3. 返回的是另一个**新promise**, 此promise的结果就会成为新promise的结果
```javascript
let p = new Promise((resolve, reject) => {
    resolve('ok');
});

//执行 then 方法
let result = p.then(value => {
    return "成功";
}, reason => {
    console.warn(reason);
});
console.log(result);
```
<a name="PxIgT"></a>
#### catch
**p.catch (onRejected) => {}  **处理拒绝的情况
1. 事实上，calling obj.catch(onRejected) 内部 calls obj.then(undefined, onRejected))
2. 如果 onRejected 抛出一个错误或返回一个本身失败的 Promise，通过 catch() 返回的 Promise 被 rejected；否则，它将显示为成功（resolved）。
```javascript
p.catch(function(reason) {
   // 拒绝
});
```
<a name="S570T"></a>
#### resolve 
**Promise.resolve(value) 方法 **返回一个成功/失败的promise对象value：将被 Promise 对象解析的参数

1. **如果参数本身就是一个 Promise 对象，则直接返回这个 Promise 对象。**
2. **非Promise类型**的对象, 则返回的结果为**成功promise对象**
```javascript
let p1 = Promise.resolve(521);
console.log(p1); // Promise {<fulfilled>: 521}
```
<a name="UMRHe"></a>
#### reject 
**Promise.resolve(reason)  方法 **返回一个失败的 promise 对象**reason**：失败的原因
```javascript
let p = Promise.reject(521);
```
<a name="PJFG4"></a>
#### all
**Promise.all (promises) => {}  **返回一个新的 promise, 只有所有的 promise 都成功才成功，只要有一个失败了就直接失败promises：包含 n 个 promise 的可迭代对象（数组） <br />返回的promise包含：成功：所有的值的数组；失败：失败的结果
```javascript
const p1 = Promise.resolve(1)
const p2 = Promise.resolve(2)
const p3 = Promise.reject(3)

const pAll = Promise.all([p1, p2, p3])
const pAll2 = Promise.all([p1, p2])

console.log(pAll)
console.log(pAll2)
```
<a name="HV4oa"></a>
#### race 
**Promise.race (promises) => {}  **返回一个新的 promise, 第一个完成的 promise 的结果状态就是最终的结果状态promises：包含 n 个 promise 的可迭代对象 
```javascript
const p1 = new Promise((resolve, reject) => {
    resolve(1)
})
const p2 = Promise.resolve(2)
const p3 = Promise.reject(3)
const pRace = Promise.race([p1, p2, p3])

console.log(pRace)//p1
```
<a name="QrvV8"></a>
#### any 
**Promise.any (promises) => {} **返回一个新的 promise, 当其中的第一个 promise 成功，就返回那个成功的promise的值。promises：包含 n 个 promise 的可迭代对象 
```javascript
const p1 = new Promise((resolve, reject) => {
    reject(1)
})
const p2 = Promise.resolve(2)
const p2_1 = Promise.resolve(2.1)
const p3 = Promise.reject(3)
const pAny = Promise.any([p1, p2, p2_1, p3])

console.log(pAny)//p2

```
<a name="lkpx4"></a>
#### allSettled 
**Promise.allSettled(promises) => {}  **返回一个(fulfilled)成功 promise，该 promise 在等所有 promise 完成后完成。并带有一个**对象数组**，每个对象对应每个promise 的结果。promises：包含 n 个 promise 的可迭代对象 <br />返回：promise  值里是一个对象数组
```javascript
const p1 = new Promise((resolve, reject) => {
    reject(1)
})
const p2 = Promise.resolve(2)
const p2_1 = Promise.resolve(2.1)
const p3 = Promise.reject(3)
const pAllSettled = Promise.allSettled([p1, p2, p2_1, p3])

console.log(pAllSettled)//p1, p2, p2_1, p3
```
<a name="UA3Bn"></a>
#### finally 
**p.finally () => {}   **是在 Promise 执行的最后一定会执行的序列```javascript
Promise.reject(2).finally(() => {
    console.log('emd')//emd
})
```
<a name="tcadx"></a>
#### 一个 promise 指定多个成功/失败回调函数（then）, 都会调用吗?
当 promise 改变为对应状态时都会调```javascript
let p = new Promise((resolve, reject) => {
    resolve('OK');//ok1,ok2
    // reject('jsf')//err1 err2
});

///指定回调 - 1
p.then(value => {
    console.log("ok1");
}, reason => {
    console.log("err1");
});

//指定回调 - 2
p.then(value => {
    console.log("ok2");
}, reason => {
    console.log("err2");
});
```
<a name="MJUAJ"></a>
#### 改变 promise 状态和指定回调函数谁先谁后执行?
 都有可能, 正常情况下是先指定回调再改变状态, 但也可以先改状态再指定回调
1. 如何先改状态再指定回调?
   1. 在执行器中直接调用 resolve()/reject()
   2. 延迟更长时间才调用 then()
2. 什么时候才能得到数据?
   1. 如果先指定的回调, 那当状态发生改变时, 回调函数就会调用, 得到数据
   2. 如果先改变的状态, 那当指定回调时, 回调函数就会调用, 得到数据
```javascript
let p = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('OK');
    }, 1000);
});

p.then(value => {
    console.log(value);
}, reason => {
    console.log(reason);
})
```
<a name="h2x6i"></a>
#### promise 如何串连多个操作任务?
promise 的 then() 返回一个新的 promise，可以并成 then() 的链式调用，通过 then 的链式调用串联多个同步/异步任务```javascript
#(1) promise 的 then()返回一个新的 promise, 可以开成 then()的链式调用
#(2) 通过 then 的链式调用串连多个同步/异步任务

let p = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('OK');
    }, 1000);
});

p.then(value => {
    return new Promise((resolve, reject) => {
        resolve("success");
    });
}).then(value => {
    console.log(value);//success
}).then(value => {
    console.log(value);//undefined
})
```
<a name="yKxMh"></a>
#### promise 异常传透?
当使用 promise 的 then 链式调用时, 可以在最后指定失败的回调，前面任何操作出了异常, 都会传到最后失败的回调中处理```javascript
let p = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('OK');
        // reject('Err');
    }, 1000);
});

p.then(value => {
    // console.log(111);
    throw '失败啦!';
}).then(value => {
    console.log(222);
}).then(value => {
    console.log(333);
}).catch(reason => {
    console.warn(reason);
});
```
<a name="PQMfm"></a>
#### 中断 promise 链?
当使用 promise 的 then 链式调用时, 在中间中断, 不再调用后面的回调函数，**办法: 在回调函数中返回一个 pendding 状态的 promise 对象**```javascript
let p = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('OK');
    }, 1000);
});

p.then(value => {
    console.log(111);
    //有且只有一个方式
    return new Promise(() => { });
}).then(value => {
    console.log(222);
}).then(value => {
    console.log(333);
}).catch(reason => {
    console.warn(reason);
});
```
<a name="qD9iT"></a>
#### 注意点

1. then、catch 和 finally 序列能否顺序颠倒？
   1. 可以，效果完全一样。但不建议这样做，最好按 then -catch-finally 的顺序编写程序。
2. 除了 then 块以外，其它两种块能否多次使用？
   1. **可以，finally 与 then 一样会按顺序执行，但是 catch 块只会执行第一个，除非 catch 块里有异常。所以最好只安排一个 catch 和 finally 块。**
3. 什么时候适合用 Promise 而不是传统回调函数？
   1. **当需要多次顺序执行异步操作的时候，例如，如果想通过异步方法先后检测用户名和密码，需要先异步检测用户名，然后再异步检测密码的情况下就很适合 Promise。**
4. Promise 是一种将异步转换为同步的方法吗？
   1. 完全不是。Promise 只不过是一种更良好的编程风格。
5. 什么时候我们需要再写一个 then 而不是在当前的 then 接着编程？
   1. 当你又需要调用一个异步任务的时候。
<a name="o2huo"></a>
#### Promise.all和Promise.race的区别的使用场景
**（1）Promise.all**<br />`Promise.all`可以将多个`Promise`实例包装成一个新的Promise实例。同时，成功和失败的返回值是不同的，成功的时候返回的是**一个结果数组**，而失败的时候则返回**最先被reject失败状态的值**。

Promise.all中传入的是数组，返回的也是是数组，并且会将进行映射，传入的promise对象返回的值是按照顺序在数组中排列的，但是注意的是他们执行的顺序并不是按照顺序的，除非可迭代对象为空。

需要注意，Promise.all获得的成功结果的数组里面的数据顺序和Promise.all接收到的数组顺序是一致的，这样当遇到发送多个请求并根据请求顺序获取和使用数据的场景，就可以使用Promise.all来解决。<br />**（2）Promise.race**<br />顾名思义，Promse.race就是赛跑的意思，意思就是说，Promise.race([p1, p2, p3])里面哪个结果获得的快，就返回那个结果，不管结果本身是成功状态还是失败状态。当要做一件事，超过多长时间就不做了，可以用这个方法来解决：
```javascript
Promise.race([promise1,timeOutPromise(5000)]).then(res=>{})
```
<a name="HPO2A"></a>
### <br />

<a name="IaMXT"></a>
### 手写实现 promise
[https://blog.csdn.net/weixin_44972008/article/details/114134995](https://blog.csdn.net/weixin_44972008/article/details/114134995)
<a name="safHs"></a>
### async/await 
<a name="eU50b"></a>
#### 1.  对async/await 的理解
async/await其实是`Generator` 的语法糖，它能实现的效果都能用then链来实现，它是为优化then链而开发出来的。从字面上来看，async是“异步”的简写，await则为等待，所以很好理解async 用于申明一个 function 是异步的，而 await 用于等待一个异步方法执行完成。当然语法上强制规定await只能出现在asnyc函数中，先来看看async函数返回了什么： 
```javascript
async function testAsy(){
   return 'hello world';
}
let result = testAsy(); 
console.log(result)
```
![](https://cdn.nlark.com/yuque/0/2020/png/1500604/1605099411873-d2eac25a-5d8c-4586-bc36-769bce79010e.png#averageHue=%23fcfcfc&height=158&id=sXDga&originHeight=158&originWidth=442&originalType=binary&ratio=1&rotation=0&showTitle=false&size=0&status=done&style=stroke&title=&width=442)<br />所以，async 函数返回的是一个 Promise 对象。async 函数（包含函数语句、函数表达式、Lambda表达式）会返回一个 Promise 对象，如果在函数中 `return` 一个直接量，async 会把这个直接量通过 `Promise.resolve()` 封装成 Promise 对象。

async 函数返回的是一个 Promise 对象，所以在最外层不能用 await 获取其返回值的情况下，当然应该用原来的方式：`then()` 链来处理这个 Promise 对象，就像这样：
```javascript
async function testAsy(){
   return 'hello world'
}
let result = testAsy() 
console.log(result)
result.then(v=>{
    console.log(v)   // hello world
})
```
那如果 async 函数没有返回值，又该如何？很容易想到，它会返回 `Promise.resolve(undefined)`。

**联想一下 Promise 的特点——无等待，所以在没有 **`**await**`** 的情况下执行 async 函数，它会立即执行，返回一个 Promise 对象，并且，绝不会阻塞后面的语句。这和普通返回 Promise 对象的函数并无二致。**

**注意：**`Promise.resolve(x)` 可以看作是 `new Promise(resolve => resolve(x))` 的简写，可以用于快速封装字面量对象或其他对象，将其封装成 Promise 实例。
<a name="As2xL"></a>
#### 2. await 到底在等啥？
**await 在等待什么呢？**一般来说，都认为 await 是在等待一个 async 函数完成。不过按语法说明，**await 等待的是一个表达式**，这个表达式的计算结果是 Promise 对象或者其它值（换句话说，就是没有特殊限定）。

因为 async 函数返回一个 Promise 对象，所以 await 可以用于等待一个 async 函数的返回值——这也可以说是 await 在等 async 函数，但要清楚，它等的实际是一个返回值。**注意到 await 不仅仅用于等 Promise 对象，它可以等任意表达式的结果，所以，await 后面实际是可以接普通函数调用或者直接量的**。所以下面这个示例完全可以正确运行：
```javascript
function getSomething() {
    return "something";
}
async function testAsync() {
    return Promise.resolve("hello async");
}
async function test() {
    const v1 = await getSomething();
    const v2 = await testAsync();
    console.log(v1, v2);
}
test();
```
await 表达式的运算结果取决于它等的是什么。

- **如果它等到的不是一个 Promise 对象，那 await 表达式的运算结果就是它等到的东西。**
- **如果它等到的是一个 Promise 对象，await 就忙起来了，它会阻塞后面的代码，等着 Promise 对象 resolve，然后得到 resolve 的值，作为 await 表达式的运算结果。**

来看一个例子：
```javascript
function testAsy(x){
   return new Promise(resolve=>{setTimeout(() => {
       resolve(x);
     }, 3000)
    }
   )
}
async function testAwt(){    
  let result =  await testAsy('hello world');
  console.log(result);    // 3秒钟之后出现hello world
  console.log('cuger')   // 3秒钟之后出现cug
}
testAwt();
console.log('cug')  //立即输出cug
```
**这就是 await 必须用在 async 函数中的原因。async 函数调用不会造成阻塞，它内部所有的阻塞都被封装在一个 Promise 对象中异步执行。await暂停当前async的执行，所以'cug''最先输出，hello world'和‘cuger’是3秒钟后同时出现的。**
<a name="hA8m7"></a>
#### 3.  async/await的优势
单一的 Promise 链并不能发现 async/await 的优势，但是，如果需要处理由多个 Promise 组成的 then 链的时候，优势就能体现出来了（很有意思，**Promise 通过 then 链来解决多层回调的问题，现在又用 async/await 来进一步优化它）。**

假设一个业务，分多个步骤完成，每个步骤都是异步的，而且依赖于上一个步骤的结果。仍然用 `setTimeout` 来模拟异步操作：
```javascript
/**
 * 传入参数 n，表示这个函数执行的时间（毫秒）
 * 执行的结果是 n + 200，这个值将用于下一步骤
 */
function takeLongTime(n) {
    return new Promise(resolve => {
        setTimeout(() => resolve(n + 200), n);
    });
}
function step1(n) {
    console.log(`step1 with ${n}`);
    return takeLongTime(n);
}
function step2(n) {
    console.log(`step2 with ${n}`);
    return takeLongTime(n);
}
function step3(n) {
    console.log(`step3 with ${n}`);
    return takeLongTime(n);
}
```
现在用 Promise 方式来实现这三个步骤的处理：
```javascript
function doIt() {
    console.time("doIt");
    const time1 = 300;
    step1(time1)
        .then(time2 => step2(time2))
        .then(time3 => step3(time3))
        .then(result => {
            console.log(`result is ${result}`);
            console.timeEnd("doIt");
        });
}
doIt();
// c:\var\test>node --harmony_async_await .
// step1 with 300
// step2 with 500
// step3 with 700
// result is 900
// doIt: 1507.251ms
```
输出结果 `result` 是 `step3()` 的参数 `700 + 200` = `900`。`doIt()` 顺序执行了三个步骤，一共用了 `300 + 500 + 700 = 1500` 毫秒，和 `console.time()/console.timeEnd()` 计算的结果一致。

如果用 async/await 来实现呢，会是这样：
```javascript
async function doIt() {
    console.time("doIt");
    const time1 = 300;
    const time2 = await step1(time1);
    const time3 = await step2(time2);
    const result = await step3(time3);
    console.log(`result is ${result}`);
    console.timeEnd("doIt");
}
doIt();
```
结果和之前的 Promise 实现是一样的，但是这个代码看起来是不是清晰得多，几乎跟同步代码一样
<a name="dyG2g"></a>
#### 4. async/await对比Promise的优势

1. **代码读起来更加同步，Promise虽然摆脱了回调地狱，但是then的链式调⽤也会带来额外的阅读负担 **
2. **Promise传递中间值⾮常麻烦，⽽async/await⼏乎是同步的写法，⾮常优雅 **
3. **错误处理友好，async/await可以⽤成熟的try/catch，Promise的错误捕获⾮常冗余 **
4. 调试友好，Promise的调试很差，由于没有代码块，你不能在⼀个返回表达式的箭头函数中设置断点，如果你在⼀个.then代码块中使⽤调试器的步进(step-over)功能，调试器并不会进⼊后续的.then代码块，因为调试器只能跟踪同步代码的每⼀步。 
<a name="Jl8jm"></a>
#### 5. async/await 如何捕获异常
```javascript
async function fn(){
    try{
        let a = await Promise.reject('error')
    }catch(error){
        console.log(error)
    }
}
```

<a name="9fb41347ef1b2456c5da51e20c970af9"></a>
### 14. setTimeout、setInterval、requestAnimationFrame 各有什么特点？
[https://interview2.poetries.top/docs/excellent-docs/3-JS%E6%A8%A1%E5%9D%97.html#_20-%E5%AE%9A%E6%97%B6%E5%99%A8](https://interview2.poetries.top/docs/excellent-docs/3-JS%E6%A8%A1%E5%9D%97.html#_20-%E5%AE%9A%E6%97%B6%E5%99%A8)<br />异步编程当然少不了定时器了，常见的定时器函数有 `setTimeout`、`setInterval`、`requestAnimationFrame`。最常用的是`setTimeout`，很多人认为 `setTimeout` 是延时多久，那就应该是多久后执行。

其实这个观点是错误的，因为 JS 是单线程执行的，如果前面的代码影响了性能，就会导致 `setTimeout` 不会按期执行。当然了，可以通过代码去修正 `setTimeout`，从而使定时器相对准确：
```javascript
let period = 60 * 1000 * 60 * 2
let startTime = new Date().getTime()
let count = 0
let end = new Date().getTime() + period
let interval = 1000
let currentInterval = interval
function loop() {
  count++
  // 代码执行所消耗的时间
  let offset = new Date().getTime() - (startTime + count * interval);
  let diff = end - new Date().getTime()
  let h = Math.floor(diff / (60 * 1000 * 60))
  let hdiff = diff % (60 * 1000 * 60)
  let m = Math.floor(hdiff / (60 * 1000))
  let mdiff = hdiff % (60 * 1000)
  let s = mdiff / (1000)
  let sCeil = Math.ceil(s)
  let sFloor = Math.floor(s)
  // 得到下一次循环所消耗的时间
  currentInterval = interval - offset 
  console.log('时：'+h, '分：'+m, '毫秒：'+s, '秒向上取整：'+sCeil, '代码执行时间：'+offset, '下次循环间隔'+currentInterval) 
  setTimeout(loop, currentInterval)
}
setTimeout(loop, currentInterval)
```
接下来看 `setInterval`，其实这个函数作用和 `setTimeout` 基本一致，只是该函数是每隔一段时间执行一次回调函数。

通常来说不建议使用 `setInterval`。第一，它和 `setTimeout` 一样，不能保证在预期的时间执行任务。第二，它存在执行累积的问题，请看以下伪代码
```javascript
function demo() {
  setInterval(function(){
    console.log(2)
  },1000)
  sleep(2000)
}
demo()
```
以上代码在浏览器环境中，如果定时器执行过程中出现了耗时操作，多个回调函数会在耗时操作结束以后同时执行，这样可能就会带来性能上的问题。

如果有循环定时器的需求，其实完全可以通过 `requestAnimationFrame` 来实现：
```javascript
function setInterval(callback, interval) {
  let timer
  const now = Date.now
  let startTime = now()
  let endTime = startTime
  const loop = () => {
    timer = window.requestAnimationFrame(loop)
    endTime = now()
    if (endTime - startTime >= interval) {
      startTime = endTime = now()
      callback(timer)
    }
  }
  timer = window.requestAnimationFrame(loop)
  return timer
}
let a = 0
setInterval(timer => {
  console.log(1)
  a++
  if (a === 3) cancelAnimationFrame(timer)
}, 1000)
```
首先 `requestAnimationFrame` 自带函数节流功能，基本可以保证在 16.6 毫秒内只执行一次（不掉帧的情况下），并且该函数的延时效果是精确的，没有其他定时器时间不准的问题，当然你也可以通过该函数来实现 `setTimeout`。
<a name="E8Ndb"></a>
### 2. setTimeout、Promise、Async/Await 的区别
<a name="jeKov"></a>
#### （1）setTimeout
```javascript
console.log('script start')	//1. 打印 script start
setTimeout(function(){
    console.log('settimeout')	// 4. 打印 settimeout
})	// 2. 调用 setTimeout 函数，并定义其完成后执行的回调函数
console.log('script end')	//3. 打印 script start
// 输出顺序：script start->script end->settimeout
```
<a name="YiDG9"></a>
#### （2）Promise
Promise本身是**同步的立即执行函数**， 当在executor中执行resolve或者reject的时候, 此时是异步操作， 会先执行then/catch等，当主栈完成后，才会去调用resolve/reject中存放的方法执行，打印p的时候，是打印的返回结果，一个Promise实例。
```javascript
console.log('script start')
let promise1 = new Promise(function (resolve) {
    console.log('promise1')
    resolve()
    console.log('promise1 end')
}).then(function () {
    console.log('promise2')
})
setTimeout(function(){
    console.log('settimeout')
})
console.log('script end')
// 输出顺序: script start->promise1->promise1 end->script end->promise2->settimeout
```
当JS主线程执行到Promise对象时：

- promise1.then() 的回调就是一个 task
- promise1 是 resolved或rejected: 那这个 task 就会放入当前事件循环回合的 microtask queue
- promise1 是 pending: 这个 task 就会放入 事件循环的未来的某个(可能下一个)回合的 microtask queue 中
- setTimeout 的回调也是个 task ，它会被放入 macrotask queue 即使是 0ms 的情况
<a name="RX1lE"></a>
#### （3）async/await
```javascript
async function async1(){
   console.log('async1 start');
    await async2();
    console.log('async1 end')
}
async function async2(){
    console.log('async2')
}
console.log('script start');
async1();
console.log('script end')
// 输出顺序：script start->async1 start->async2->script end->async1 end
```
async 函数返回一个 Promise 对象，当函数执行的时候，一旦遇到 await 就会先返回，等到触发的异步操作完成，再执行函数体内后面的语句。可以理解为，是让出了线程，跳出了 async 函数体。

例如：
```javascript
async function func1() {
    return 1
}
console.log(func1())
```
![](https://cdn.nlark.com/yuque/0/2020/png/1500604/1604021075237-8249a8df-3a28-4bca-9f22-02923aba8618.png#averageHue=%23fbfafa&height=156&id=UoVYQ&originHeight=156&originWidth=1066&originalType=binary&ratio=1&rotation=0&showTitle=false&size=0&status=done&style=shadow&title=&width=1066)<br />func1的运行结果其实就是一个Promise对象。因此也可以使用then来处理后续逻辑。
```javascript
func1().then(res => {
    console.log(res);  // 30
})
```
await的含义为等待，也就是 async 函数需要等待await后的函数执行完成并且有了返回结果（Promise对象）之后，才能继续执行下面的代码。await通过返回一个Promise对象来实现同步的效果。

<a name="968c05f4053a48772386de85db930270"></a>
## [事件循环（EventLoop）](https://www.bilibili.com/video/BV1bE411B7ez/?spm_id_from=333.999.0.0&vd_source=cd9504ca6ce661eba399a1d45b3241bb)
> js是单线程，代码顺序执行，（很多任务需要排队，容易出现代码阻塞，优先等级高的任务先执行）

JS脚本语言的初衷所是为了实现处理页面中的用户交互，以及操作DOM。在DOM操作中充分展示了单线程特征：创建一个元素，创建成功之后才可以将它添加到某个节点中
> 同步任务和异步任务（定时器，事件，网络请求）

1. **js代码从上往下顺序执行，遇到异步任务将其挂起，执行栈中同步任务执行完毕了 再去执行异步任务**
2. **本质区别：在js中中同步任务和异步任 务的执行顺序不同**
> <a name="HwoyZ"></a>
#### EventLoop（是什么）


![src=http___pic2.zhimg.com_v2-0680532c6e5d1f0455335509109babbe_1440w.jpg_source=172ae18b&refer=http___pic2.zhimg.webp](https://cdn.nlark.com/yuque/0/2022/webp/28203632/1662379351848-93f503a2-34e4-4975-838c-17daba935472.webp#averageHue=%23f5f0e1&clientId=u50684678-1bf9-4&errorMessage=unknown%20error&from=ui&id=u18bb9ada&originHeight=850&originWidth=1440&originalType=binary&ratio=1&rotation=0&showTitle=false&size=29858&status=error&style=stroke&taskId=ud6934adf-1413-4c49-a4d0-f58610345f8&title=)

- **三大内容**
   - stack 栈（Call Stack）
   - 浏览器模块：定时器模块，DOM事件响应模块，网络请求模块
   - 回调队列（Callback Queue）
-  **事件循环**
   1. 任务队列一开始不是空的，整体代码会作为第一个宏任务在任务队列中被循环
   2. 主线程先执行同步任务，遇到异步任务，被相应的浏览器模块处理（定时、网络请求等)，**待符合触发条件，就移动到回调队列（Callback Queue）中。**
   3. **当执行栈中的所有同步任务执行完毕**，Event Loop开始工作，轮询查找回调队列是否有任务 ，如有则（结束等待过程）移动到执行栈中执行（周而复始）
- **异步任务与回调队列**
   1. 异步任务分：**微任务MacroTask， 宏任务MicroTask**  requestAnimationFrame 
      1. 微任务 : **Promsie.then  async/await**   Object.observe    MutationObserver 
         1. 每一个宏任务执行前，~~程序会先检测中是否有当次事件循环未执行的微任务~~，会优先清空本次事件循环的微任务后，再执行，每一个宏任务内部可注册当次任务的微任务队列，在下一个宏任务执行前运行，**微任务也是按照进入队列的顺序执行的**
      2. 宏任务 :   AJAX 网络请求  定时器   DOM 事件 script
      3. **这就是浏览器里的 Event Loop 的设计**：**设计 Loop 机制和 Task 队列是为了支持异步，解决逻辑执行阻塞主线程的问题，设计 MicroTask 队列的插队机制是为了解决高优任务尽早执行的问题**
   2. 微任务队列
   3. 宏任务队列
   4. RAF回调队列（ requestAnimationFrame）
- **事件循环过程中不同任务执行时机**
   1. **上一个宏任务结束 -> 微任务->requestAnimationFrame -> 浏览器渲染页面 -> 下一个宏任务开始**
   2. 每次栈中同步代码执行完（即每次轮询结束），都是重新渲染页的机会，浏览器渲染页面，然后再去触发下一次Event Loop
   3. 但不一定每一轮 event loop 都会对应一次浏览器渲染，要根据屏幕刷新率、页面性能、页面是否在后台运行来共同决定，通常来说这个渲染间隔是固定的
- **GUI渲染线程与JS引擎线程互斥**
   1. 当JS引擎执行时GUI线程会被挂起， GUI更新则会被保存在一个队列中等到JS引擎线程空闲时立即被执行。

**所以正确的一次 Event loop 顺序是这样的**

- 执行同步代码，这属于宏任务
- 执行栈为空，查询是否有微任务需要执行
- 执行所有微任务
- 必要的话渲染 UI
- 然后开始下一轮 Event loop，执行宏任务中的异步代码

通过 Event loop 顺序可知，如果宏任务中的异步代码有大量的计算并且需要操作 DOM 的话，为了更快的响应界面响应，我们可以把操作 DOM 放入微任务中<br />**怎么理解宏微任务的划分呢？**

- 定时器、网络请求这种都是在别的线程跑完之后通知主线程的普通异步逻辑，所以都是宏任务
- 高优任务的这三种也很好理解，MutationObserver 和 Object.observe 都是监听某个对象的变化的，变化是很瞬时的事情，肯定要马上响应，不然可能又变了，Promise 是组织异步流程的，异步结束调用 then 也是很高优的

**这就是浏览器里的 Event Loop 的设计**：**设计 Loop 机制和 Task 队列是为了支持异步，解决逻辑执行阻塞主线程的问题，设计 MicroTask 队列的插队机制是为了解决高优任务尽早执行的问题**

- 注意： setTimeOut并不是直接的把你的回掉函数放进上述的异步队列中去，而是在定时器的时间到了之后，把回掉函数放到执行异步队列中去。如果此时这个队列已经有很多任务了，那就排在他们的后面。这也就解释了为什么setTimeOut为什么不能精准的执行的问题了。
- 首先，new Promise是同步的任务，会被放到主进程中去立即执行。而.then()函数是异步任务会放到异步队列中去，那什么时候放到异步队列中去呢？当你的promise状态结束的时候，就会立即放进异步队列中去了。
<a name="mpq0P"></a>
## 垃圾回收与内存泄漏
<a name="edbc6ae5d0930d1b2047b644faf33356"></a>
### 1. 浏览器的垃圾回收机制
<a name="977f451c8a145133c5f12e362a8b7399"></a>
#### （1）垃圾回收的概念
**垃圾回收**：JavaScript代码运行时，需要分配内存空间来储存变量和值。当变量不在参与运行时，就需要系统收回被占用的内存空间，这就是垃圾回收。<br />当JavaScript的解释器消耗完系统中所有可用的内存时，就会造成系统崩溃。<br />针对JavaScript的来及回收机制有以下两种方法（常用）：**标记清除，引用计数**

**回收机制**：

- Javascript 具有自动垃圾回收机制，会定期对那些不再使用的变量、对象所占用的内存进行释放，原理就是找到不再使用的变量，然后释放掉其占用的内存。
- JavaScript中存在两种变量：局部变量和全局变量。全局变量的生命周期会持续要页面卸载；而局部变量声明在函数中，它的生命周期从函数执行开始，直到函数执行结束，在这个过程中，局部变量会在堆或栈中存储它们的值，当函数执行结束后，这些局部变量不再被使用，它们所占有的空间就会被释放。
- 不过，当局部变量被外部函数使用时，其中一种情况就是闭包，在函数执行结束后，函数外部的变量依然指向函数内部的局部变量，此时局部变量依然在被使用，所以不会回收。
<a name="1fc43a0681131ffebbb5c5379b3dde3d"></a>
#### （2）垃圾回收的方式
浏览器通常使用的垃圾回收方法有两种：标记清除，引用计数。<br />**1）标记清除**

1. 当变量进入执行环境时，就标记这个变量“进入环境”，被标记为“进入环境”的变量是不能被回收的，因为他们正在被使用。当变量离开环境时，就会被标记为“离开环境”，被标记为“离开环境”的变量会被内存释放。
2. 垃圾收集器在运行的时候会给存储在内存中的所有变量都加上标记。然后，它会去掉环境中的变量以及被环境中的变量引用的标记。而在此之后再被加上标记的变量将被视为准备删除的变量，原因是环境中的变量已经无法访问到这些变量了。最后。垃圾收集器完成内存清除工作，销毁那些带标记的值，并回收他们所占用的内存空间。
3. 标记阶段即为所有活动对象做上标记，清除阶段则把没有标记（也就是非活动对象）销毁。

**2）引用计数**

1. 另外一种垃圾回收机制就是引用计数，这个用的相对较少。引用计数就是跟踪记录每个值被引用的次数。当声明了一个变量并将一个引用类型赋值给该变量时，则这个值的引用次数就是1。相反，如果包含对这个值引用的变量又取得了另外一个值，则这个值的引用次数就减1。当这个引用次数变为0时，说明这个变量已经没有价值，因此，在在机回收期下次再运行时，这个变量所占有的内存空间就会被释放出来。
2. 这种方法会引起**循环引用**的问题：例如：` obj1`和`obj2`通过属性进行相互引用，两个对象的引用次数都是2。当使用循环计数时，由于函数执行完后，两个对象都离开作用域，函数执行结束，`obj1`和`obj2`还将会继续存在，因此它们的引用次数永远不会是0，就会引起循环引用。
```javascript
function fun() {
    let obj1 = {};
    let obj2 = {};
    obj1.a = obj2; // obj1 引用 obj2
    obj2.a = obj1; // obj2 引用 obj1
}
```
这种情况下，就要手动释放变量占用的内存：
```javascript
obj1.a =  null
 obj2.a =  null
```
<a name="6431f010ca889398f03a1c5680ef20e4"></a>
#### （3）减少垃圾回收
虽然浏览器可以进行垃圾自动回收，但是当代码比较复杂时，垃圾回收所带来的代价比较大，所以应该尽量减少垃圾回收。

- **对数组进行优化：**在清空一个数组时，最简单的方法就是给其赋值为[ ]，但是与此同时会创建一个新的空对象，可以将数组的长度设置为0，以此来达到清空数组的目的。
- **对**`**object**`**进行优化：**对象尽量复用，对于不再使用的对象，就将其设置为null，尽快被回收。
- **对函数进行优化：**在循环中的函数表达式，如果可以复用，尽量放在函数的外面。
<a name="ec8b12aa8ca42824437791379131e837"></a>
### 2. 哪些情况会导致内存泄漏
以下四种情况会造成内存的泄漏：

1. **意外的全局变量：**由于使用未声明的变量，而意外的创建了一个全局变量，而使这个变量一直留在内存中无法被回收。
2. **被遗忘的定时器或回调函数：**设置了 setInterval 定时器，而忘记取消它，如果循环函数有对外部变量的引用的话，那么这个变量会被一直留在内存中，而无法被回收。
3. **闭包：**不合理的使用闭包，从而导致某些变量一直被留在内存当中。
4. **循环引用(两个对象相互引用)**
5. **控制台日志(console.log)**
6. **脱离 DOM 的引用：**获取一个 DOM 元素的引用，而后面这个元素被删除，由于一直保留了对这个元素的引用，所以它也无法被回收。
7. **移除存在绑定事件的DOM元素(IE)**
<a name="rhrtr"></a>
### 3. JavaScript 对象生命周期的理解

- 当创建一个对象时，JavaScript 会自动为该对象分配适当的内存
- 垃圾回收器定期扫描对象，并计算引用了该对象的其他对象的数量
- 如果被引用数量为 0，或惟一引用是循环的，那么该对象的内存即可回收
<a name="G7YB8"></a>
## 回流和重绘
<a name="aQ7fG"></a>
### 了解浏览器的渲染机制

1. 浏览器采用流式布局模型。
2. 首先浏览器会将HTML解析成DOM，把CSS解析成CSSOM，把CSSOM.与DOM结合产生render tree。
3. 有render tree.之后，我们知道了节点样式，然后浏览器会计算节点的大小和位置，然后把节点绘制到页面上。
<a name="nBqK4"></a>
### 回流 (reflow)

1. **当 render tree 中的一些元素的结构或尺寸，布局，隐藏等改变，浏览器会使渲染树中受到影响的部分失效，并重新构造这部分渲染树的过程称为回流（重排）**
2. **发生了回流则一定发生了重绘，发生了重绘不一定会发生回流**
3. `发生时机` 
   1.  页面初始加载 
   2.  窗口大小发生变化 
   3.  改变字体，元素尺寸(width,height,border,position),改变元素内容 
   4.  添加或删除元素，如果元素本身被display:none会回流，visibility:hidden.则会发生重绘 
   5.  定位为fixed的元素，滚动条拖动时会回流 
   6.  激活CSS伪类 
```
offsetTop、
offsetLeft、
offsetWidth、
offsetHeight、
scrollTop、
scrollLeft、
scrollWidth、
scrollHeight、
clientTop、
clientLeft、
clientWidth、
clientHeight

这些属性有一个共性，就是需要通过即时计算得到。因此浏览器为了获取这些值，也会进行回流
```
<a name="skbCi"></a>
### 重绘 (repaint)

1. **当页面中元素样式的改变不影响它在文档流中的位置，浏览器会将新样式赋值给元素，这个过程叫做重绘，**
2. `发生时机` 
   1. **text-decoration**
   2. **backgtound-image**
   3. **background-repeat**
   4. **box-shadow**
   5. **background-size**
   6. **visibility:hidden**
<a name="tevSx"></a>
### 性能影响

- 总结：**回流的性能消耗要比重绘大**。耗时，导致浏览器卡慢
<a name="lsjhG"></a>
### 减少回流重绘
`css：`

1. 尽量减少`table`布局使用，table属性变化使用会直接导致布局重排或者重绘
2. 避免设置多层内联样式
3. 避免使用`CSS`表达式（例如：`calc()`）。
4. 将动画效果应用到`position`属性为`absolute`或`fixed`的元素上。
5. 尽可能在DOM树的最末端改变class。
6. 使用css3硬件加速，可以让transform、opacity、filters等动画不会引起重绘 

`javascript`

1.  减少直接操作dom元素，改用className用于控制 
2.  **对于复杂动画效果，使用绝对定位让其脱离文档流，否则会引起父元素及后续元素大量回流 **
3. ** 可以先为元素设置**`**display: none**`**，操作结束后再把它显示出来**。因为在`display`属性为`none`的元素上进行的`DOM`操作不会引发回流和重绘。 
4.  **避免频繁操作**`**DOM**`**，创建一个**`**documentFragment**`**，在它上面应用所有**`**DOM操作**`**，最后再把它添加到文档中。 **
5.  避免频繁操作样式，最好一次性重写`style`属性，或者将样式列表定义为`class`并一次性更改`class`属性。 
```javascript
var list = document.querySelector("#list");
var fragment = document.createDocumentFragment(); 
for(var i = 0 ; i < 5 ; i++){
   var li = document.createElement("li");
   li.innerHTML = i;
   fragment.appendChild(li);
}
list.appendChild(fragment);
```
<a name="cgoon"></a>
### 浏览器自身的优化

1. 浏览器会维护1个队列，把所有会引起回流、重绘的操作放入这个队列，等队列中的操作到了一定的数量或者到了一定的时间间隔，浏览器就会flush队列，进行一个批处理。这样就会让多次的回流、重绘变成一次回流重绘


<a name="nkvG6"></a>
## 设计模式
<a name="BPtQj"></a>
### JS 发布订阅模式

- `观察者模式/发布订阅者模式`/消息系统/消息机制/自定义事件
- 为了让发布者和订阅者间解耦
- 先把未来将要做的某些事情注册，在特定某一时刻统一执行

**概念**

- 发布-订阅者模式其实是一种对象间一对多的依赖关系(利用消息队列)，当一个对象的状态(state)发生改变时,所有依赖于它的对象都将得到状态改变的通知，
- 订阅者 把自己想订阅的事件`注册`到`调度中心`
- 当发布者`发布`该（订阅）事件到调度中心，也就是该事件`触发`时，
- 由调度中心`统一调度`订阅者注册到调度中心的处理代码。

**例一**

1. 调度中心(缓存列表)
2. on 订阅者注册事件到调度中心 
   1. type：未订阅过：
   2. type：已订阅过
3. off 根据订阅类型取消订阅 
   1. type：未订阅过：return
   2. type：已订阅过 
      1. fn 未定义，清空队列
      2. fn 已定义，将 fn 移出队列
4. emit 根据订阅类型，发布者发布（订阅）事件到调度中心,调度中心处理代码 
   1. type：未订阅过：return
   2. type：已订阅过，遍历队列，发布消息
```javascript
class Observer {

  constructor() {
    // {
    //     'click':[fn1,fn2,fn3]
    // }
    // 调度中心(消息队列)
    this.eventChannel = {}
  }

  //type订阅主题
  //fn 加入队列的事件
  $on(type, fn) {
    //调度中无type主题:初始化
    if (!this.eventChannel[type]) {
      this.eventChannel[type] = []
    }
    //调度中心已有type主题
    this.eventChannel[type].push(fn)
  }

  //type订阅主题
  //fn 移出队列的事件(可选)
  $off(type, fn) {
    //没订阅
    if (!this.eventChannel[type]) return
    //fn 为 undefined 删除整个队列 
    if (!fn) return delete this.eventChannel[type]
    //fu 不为 undefined 可将 fn 移出队列
    this.eventChannel[type] = this.eventChannel[type].filter(item => item !== fn)
  }

  $emit(type, data) {
    if (!this.eventChannel[type]) return

    this.eventChannel[type].forEach(callback => callback(type + '发布消息了'))
  }
}

const observer = new Observer()

var f1 = (data) => console.log(1, data)
var f2 = (data) => console.log(2, data)
var f3 = (data) => console.log(1, data)
var f4 = (data) => console.log(2, data)
observer.$on('click', f1)
observer.$on('click', f2)
observer.$on('live', f3)
observer.$on('live', f4)

observer.$emit('click', "hello")
observer.$emit('live', "word")

observer.$off('click', f1)
console.log(observer);
```
**例二**
```javascript
// 发布订阅模式   一对多 
// 发布者 订阅者 频道/主题 消息   对象(存储对应的订阅者)
// 一个发布者是可以对应多个订阅者
const callbacks = {
    '频道1': {
        '订阅者1': () => { },
        '订阅者2': () => { },
    },
    '频道2': {
        '订阅者1': () => { },
        '订阅者2': () => { },
    }
}

function Pubsub() {
    // 主要存储 对应的频道->订阅者->回调函数
    this.callbacks = {}
    // 订阅
    //type：频道
    //callback：订阅回调
    //subId：订阅者自定义id标识
    this.sub = function (type, callback, subId) {
        if (arguments.length < 1) throw new Error("必须传入订阅的频道")
        //生成订阅者的id标识
        var id = "token_" + (parseInt(Math.random() * 0xFFFFFF).toString(16))
        // 如果频道已存在
        if (this.callbacks[type]) {
            this.callbacks[type][(subId ? subId : id)] = callback
        } else {//如果频道不存在
            this.callbacks[type] = {
                [(subId ? subId : id)]: callback
            }
        }
        return (subId ? subId : id);
    }
    // 发布
    //type：频道
    //data：发布的消息
    this.publish = function (type, data) {
        if (this.callbacks[type]) {
            // type 频道下有订阅者的回调
            const callbacksArr = Pubsub.getValues(this.callbacks[type]);
            // 调用所有订阅者的回调给订阅者发布消息
            callbacksArr.forEach(function (callback) {
                callback(data);
            });
        } else {
            alert(`${type}频道不存在还没有人订阅`)
        }
    }
    //type可能的值：undefined 频道的名称  订阅者id
    // undefined 频道的名称  对应的标识(对应的某一个订阅者)
    this.cannle = function (type) {
        // undefined情况：取消所有频道所有订阅
        if (typeof type === 'undefined') return this.callbacks = {};

        //判断传入的type是否是字符串,不是则退出并提醒
        if (typeof type !== "string") return alert("参数必须是字符串")

        // type为订阅频道的情况：删除这个频道
        if (this.callbacks.hasOwnProperty(type.toLowerCase())) {
            delete this.callbacks[type]
        } else {// 订阅者的ID情况：
            // 获取所有频道下的所有订阅者和其回调组成的对象数组
                var alltheSubscriberObj = Pubsub.getValues(this.callbacks);
            //遍历出每个频道下所有订阅者和其回调组成的对象 alltheSubscriber
            alltheSubscriberObj.forEach(alltheSubscriber => {
                // 判断该频道是有名为type的订阅者，有则删除该订阅者
                if (alltheSubscriber.hasOwnProperty(type)) {
                    delete alltheSubscriber[type];
                }
            })
        }
        return this;
    }
    //获取对象所有的value组成的数组
    Pubsub.getValues = function (obj) {
        var objs = [];
        for (var key in obj) {
            objs.push(obj[key]);
        }
        return objs;
    }
}
var pubsub = new Pubsub();
// 一个订阅者 订阅了 channl1 频道
var subId = pubsub.sub("频1", function (data) {
    console.log(data + "频道1");
}, "订阅者1");
var subId2 = pubsub.sub("频1", function (data) {
    console.log(data + "频道1");
});
// var subId2 = pubsub.sub("channl1", function (data) {
//     console.log(data + "频道1");
// });
var subId3 = pubsub.sub("频2", function (data) {
    console.log(data + "频道2");
}, "订阅者1");
var subId4 = pubsub.sub("频2", function (data) {
    console.log(data + "频道2");
});
// pubsub.cannle('频1')
// pubsub.cannle(subId).cannle(subId2);
pubsub.publish("频1", "频1你们好！");
pubsub.publish("频2", "频2你们好！！");

console.log(pubsub);
```
[https://www.bilibili.com/video/BV1LL41177AV?p=56](https://www.bilibili.com/video/BV1LL41177AV?p=56)<br />[https://www.bilibili.com/video/BV1q64y1s7eT?p=1](https://www.bilibili.com/video/BV1q64y1s7eT?p=1)<br />[https://www.bilibili.com/video/BV18C4y147t9?p=1](https://www.bilibili.com/video/BV18C4y147t9?p=1)

<a name="doI3i"></a>
## 









