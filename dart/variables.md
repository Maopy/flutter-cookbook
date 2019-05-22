# Tour

## 定义变量

定义一个变量，并赋初值：

```dart
var name = 'Bob';
```

定义了一个`name` 变量保存了`String` 类型对象的引用，值为`Bob` 。

{% hint style="info" %}
与 JavaScript 中的 var 不同，Dart 中的 var 一旦赋值，在编译阶段会根据第一次赋值数据的类型来为变量推测出其类型，编译结束后其类型就已经被确定。不能再改变其类型。
{% endhint %}

使用类名定义明确类型的变量：

```dart
String name = 'Bob';
```

使用 `dynamic` 和 `Object` 可以定义一个动态类型的变量。由于 `Object` 是所有类型的基类，所以任何类型的数据都可以赋值给`Object` 声明的变量。

```dart
dynamic name = 'Bob';
Object foo = 'bar';
// 下面代码不会报错
name = 100;
foo = 200;
```

`dynamic`与`Object`不同的是，`dynamic`声明的对象编译器会提供所有可能的组合，而`Object`声明的对象只能使用 Object 的属性与方法，否则编译器会报错。如：

```dart
dynamic a;
Object b;

main () {
    a = "";
    b = "";
    printLength();
}

printLength () {
    print(a.length); // no warning
    print(b.length); // The getter 'length' is not defined for the class 'Object'
}
```

{% hint style="info" %}
官方推荐使用`var`关键字来定义本地变量
{% endhint %}

### 默认值

```text
int count;
assert(count == null);
```

未设初值的变量会被设为`null` 哪怕是数字类型的变量，因为 _numbers-like_ 的值在 Dart 中都是对象。

### Final 和 const

使用 `final` 或 `const` 定义一个不会改变的值，好于用 `var` 或者固定的类型。两者的区别是：`const` 定义的是编译时常量，`final` 定义的顶层变量或类属性在第一次使用时被初始化。

{% hint style="info" %}
实例变量需定义为 `final` 而非 `const`，且必须在构造器运行之前被初始化——变量通过构造器参数传参或构造器初始化列表声明的时候
{% endhint %}

## 内置类型

内置类型列表：

* numbers
* strings
* booleans
* lists
* sets
* maps
* runes
* symbols

### Numbers

#### int

整型占用 64 bits 空间。根据平台差异，在 Dart VM 取值范围是 -2^63 到 2^63 - 1，当 Dart 编译为 JavaScript 时，取值范围变成 -2^53 到 2^53 - 1。

#### double

双精度浮点型是标准 IEEE 754。

`int` 和 `double` 都是 `num` 的子类型。如果 `num` 及其子类中没有找到想使用的方法，可以到`dart:math` 库找找。

### Strings

Dart 的字符串采用 UTF-16 编码。可以使用 `${表达式}` 语法。如果表达式是一个变量，可以省略 {}，Dart 会调用对象的 `toString` 方法。

```dart
var s = 'string interpolation';

assert('Dart has $s, which is very handy.' == 'Dart has string interpolation, which is very handy.');
assert('That deserves all caps. ${s.toUpperCase()} is very handy!' == 'That deserves all caps. STRING INTERPOLATION is very handy!');
```

多行字符串语法：使用三连单引号或双引号。

```dart
var s1 = '''
You can create
multi-line strings like this one.
''';
var s2 = """This is also a
multi-line string.""";
```

### Booleans

Dart 是类型安全语言，所以不能使用 `if (非布尔值)` 这种代码。

### Lists

Dart 的 `List` 相当于其他语言的 array。

{% hint style="info" %}
Dart 的 List 会根据初始值做类型推断，添加其他类型元素会导致异常。
{% endhint %}

如果在数组中使用展开运算符的右值有可能是 null，可以使用 _空值敏感展开运算符_\(`...?`\)

```dart
var list;
var list2 = [0, ...?list];
assert(list2.length == 1);
```

collection if:

```dart
var nav = [
  'Home',
  'Furniture',
  'Plants',
  if (promoActive) 'Outlet'
];
```

collection for:

```dart
var listOfInts = [1, 2, 3];
var listOfStrings = [
  '#0',
  for (var i in listOfInts) '#$i'
];
assert(listOfStrings[1] == '#1');
```

### Sets

Dart 中的 Set 是无序无相同项的集合。

```dart
var halogens = {'fluorine', 'chlorine', 'bromine', 'iodine', 'astatine'};
```

{% hint style="info" %}
Dart 的 Set 会根据初始值做类型推断，添加其他类型元素会导致异常。
{% endhint %}

添加元素的方法：

```dart
var elements = <String>{};
elements.add('fluorine');
elements.addAll(halogens);
```

### Maps

map 的 key 和 value 可以是任意类型，但 key 不可以重复。

```dart
var gifts = new Map();
var nobleGases = Map(); // 在 Dart 2，new 关键字可以省略
```

在 map 中检索一个不存在的 key 会返回 null。

map 中也可以使用 `...` `...?` `collection if` `collection for` 。

### Runes

Dart 的 runes 是 UTF-32 编码的字符串。

```dart
var clapping = '\u{1f44f}';
```

### Symbols

还不太懂。。

```dart
#bar
```

## 函数

Dart 的函数参数分为必传参数和可选参数。

### 可选参数

可选参数分为 位置可选参数和命名可选参数：

Flutter 中大量使用了命名可选参数，官方说法是这样的语法更加清晰。

```dart
/// Sets the [bold] and [hidden] flags ...
void enableFlags({bool bold, bool hidden}) {...}
// when calling a function
enableFlags(bold: true, hidden: false);
```

### 主函数

每个 APP 必须有一个顶层的主函数，作为 APP 的入口函数。

### 匿名函数

```dart
/* 格式
  ([[Type] param1[, …]]) { 
    codeBlock; 
  }; 
*/

var list = ['apples', 'bananas', 'oranges'];
list.forEach((item) {
  print('${list.indexOf(item)}: $item');
});
```

### 返回值

如果函数没有指定返回值，会返回 null。

## 运算符

JavaScript 中没有的运算符列表：

| 描述 | 运算符 |
| :--- | :--- |
| 一元后缀 | `?.`  |
| 乘法类 | `~/`  |
| 按位 | `&` `|`  |
| 类型检测 | `as` `is` `is!`  |
| if null | `??`  |
| 层叠 | `..`  |

### 类型检测运算符

```dart
// 1
if (emp is Person) {
  // Type check
  emp.firstName = 'Bob';
}
// 2
(emp as Person).firstName = 'Bob';
```

{% hint style="info" %}
代码段 1 和 2 并不等价。如果 emp 不是 Person 类型或者是 null，1 不会做任何事，2 会抛出异常。
{% endhint %}







