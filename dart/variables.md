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

## 类

### 使用构造函数

构造函数的名字可以是 `类名` 或 `类名.标识符` !

```dart
var p1 = Point(2, 2);
var p2 = Point.fromJson({'x': 1, 'y': 2});
```

构造2个相同的编译时常量实例，他们是相等的！

```dart
var a = const ImmutablePoint(1, 1);
var b = const ImmutablePoint(1, 1);

assert(identical(a, b)); // They are the same instance!
```

### 实例变量

```dart
class Point {
  num x; // Declare instance variable x, initially null.
  num y; // Declare y, initially null.
  num z = 0; // Declare z, initially 0.
}
```

每个实例会隐式给每个属性创建一个 getter 方法，如果是非 final 属性，还会隐式创建一个 setter 方法。

### 构造函数

```dart
class Point {
  num x, y;

  Point(num x, num y) {
    // There's a better way to do this, stay tuned.
    this.x = x;
    this.y = y;
  }
}
```

由于构造函数初始化赋值的操作太常见，Dart 有一个简化的语法糖！

```dart
class Point {
  num x, y;

  // Syntactic sugar for setting x and y
  // before the constructor body runs.
  Point(this.x, this.y);
}
```

#### 命名构造函数

可以使用命名构造函数来实现一个类的多个构造函数：

```dart
class Point {
  num x, y;

  Point(this.x, this.y);

  // Named constructor
  Point.origin() {
    x = 0;
    y = 0;
  }
}
```

注意，子类的命名构造函数是不会默认继承父类的。如果有这种需要，需要在子类的构造函数中自行实现。

#### 调用父类非默认构造函数

默认情况下，子类的构造函数会调用父类的非命名无参构造函数。父类的构造函数在子类的构造函数体最开始的位置被调用。如果同时使用了初始化列表，它会在父类之前被调用。简单来说，执行顺序如下：

1. 初始化列表
2. 父类无参构造函数
3. 主类无参构造函数

如果父类没有定义非命名无参构造函数，那么你必须手动调用父类的某一个构造函数。在 `:` 后面指定父类的构造函数。

```dart
class Person {
  String firstName;

  Person.fromJson(Map data) {
    print('in Person');
  }
}

class Employee extends Person {
  // Person does not have a default constructor;
  // you must call super.fromJson(data).
  Employee.fromJson(Map data) : super.fromJson(data) {
    print('in Employee');
  }
}

main() {
  var emp = new Employee.fromJson({});

  // Prints:
  // in Person
  // in Employee
  if (emp is Person) {
    // Type check
    emp.firstName = 'Bob';
  }
  (emp as Person).firstName = 'Bob';
}
```

#### 初始化列表

除了调用父类的构造函数，你还可以在构造函数运行之前初始化实例属性。

```dart
// Initializer list sets instance variables before
// the constructor body runs.
Point.fromJson(Map<String, num> json)
    : x = json['x'],
      y = json['y'] {
  print('In Point.fromJson(): ($x, $y)');
}
```

可以利用这个特性来做输入校验

```dart
Point.withAssert(this.x, this.y) : assert(x >= 0) {
  print('In Point.withAssert(): ($x, $y)');
}
```

初始化列表用来做 final 属性的初始化非常方便。

```dart
import 'dart:math';

class Point {
  final num x;
  final num y;
  final num distanceFromOrigin;

  Point(x, y)
      : x = x,
        y = y,
        distanceFromOrigin = sqrt(x * x + y * y);
}

main() {
  var p = new Point(2, 3);
  print(p.distanceFromOrigin);
}
```

#### 重定向构造函数

唯一目的是重定向到同一个类其他构造函数的构造函数。重定向构造函数没有函数体。

```dart
class Point {
  num x, y;

  // The main constructor for this class.
  Point(this.x, this.y);

  // Delegates to the main constructor.
  Point.alongXAxis(num x) : this(x, 0);
}
```

#### 常量构造函数

如果类创造的对象是不会变化的，可以把这些对象置为编译时常量。为此，定义常量构造函数并保证所有的实例属性都是 final 的。

```dart
class ImmutablePoint {
  static final ImmutablePoint origin =
      const ImmutablePoint(0, 0);

  final num x, y;

  const ImmutablePoint(this.x, this.y);
}
```

#### 工厂构造函数

要实现不总是返回一个新的实例的构造函数，使用 `factory` 关键字。工厂构造函数可能返回一个缓存的实例，或者一个子类的实例。

```dart
class Logger {
  final String name;
  bool mute = false;

  // _cache is library-private, thanks to
  // the _ in front of its name.
  static final Map<String, Logger> _cache =
      <String, Logger>{};

  factory Logger(String name) {
    if (_cache.containsKey(name)) {
      return _cache[name];
    } else {
      final logger = Logger._internal(name);
      _cache[name] = logger;
      return logger;
    }
  }

  Logger._internal(this.name);

  void log(String msg) {
    if (!mute) print(msg);
  }
}
```

### 方法

#### 实例方法

实例方法可以访问实例属性和 `this` 。

#### Getters 和 Setters

```dart
class Rectangle {
  num left, top, width, height;

  Rectangle(this.left, this.top, this.width, this.height);

  // Define two calculated properties: right and bottom.
  num get right => left + width;
  set right(num value) => left = value - width;
  num get bottom => top + height;
  set bottom(num value) => top = value - height;
}

void main() {
  var rect = Rectangle(3, 4, 20, 15);
  assert(rect.left == 3);
  rect.right = 12;
  assert(rect.left == -8);
}
```

#### 抽象方法

实例方法、getter 和 setter 可以是抽象的，定义一个接口，等待其他的类去实现它。抽象方法只能定义在抽象类中。

```dart
abstract class Doer {
  // Define instance variables and methods...

  void doSomething(); // Define an abstract method.
}

class EffectiveDoer extends Doer {
  void doSomething() {
    // Provide an implementation, so the method is not abstract here...
  }
}
```

### 抽象类

使用 `abstract` 修饰符定义一个抽象类，抽象类不能被实例化。抽象类常被用于定义接口。如果想要实例化一个抽象类，可以定义工厂构造函数。

### 隐式接口

每个类都隐式的定义了一个接口，接口包含了该类所有的实例成员及其实现的接口。一个类可以实现多个接口。

```dart
// A person. The implicit interface contains greet().
class Person {
  // In the interface, but visible only in this library.
  final _name;

  // Not in the interface, since this is a constructor.
  Person(this._name);

  // In the interface.
  String greet(String who) => 'Hello, $who. I am $_name.';
}

// An implementation of the Person interface.
class Impostor implements Person {
  get _name => '';

  String greet(String who) => 'Hi $who. Do you know who I am?';
}

String greetBob(Person person) => person.greet('Bob');

void main() {
  print(greetBob(Person('Kathy')));
  print(greetBob(Impostor()));
}
```

### 继承类

#### 重载成员

子类可以重载实例方法，getter 和 setter。

```dart
class SmartTelevision extends Television {
  @override
  void turnOn() {...}
  // ···
}
```

#### 运算符重载

```dart
class Vector {
  final int x, y;

  Vector(this.x, this.y);

  Vector operator +(Vector v) => Vector(x + v.x, y + v.y);
  Vector operator -(Vector v) => Vector(x - v.x, y - v.y);

  // Operator == and hashCode not shown. For details, see note below.
  // ···
}

void main() {
  final v = Vector(2, 3);
  final w = Vector(2, 2);

  assert(v + w == Vector(4, 5));
  assert(v - w == Vector(0, 1));
}
```

重载 `==` 运算符时，要同时重载对象的 `hashCode` 的 getter 方法。

#### noSuchMethod\(\)

检测代码使用不存在的方法或属性。

```dart
class A {
  // Unless you override noSuchMethod, using a
  // non-existent member results in a NoSuchMethodError.
  @override
  void noSuchMethod(Invocation invocation) {
    print('You tried to use a non-existent member: ' +
        '${invocation.memberName}');
  }
}
```

只有在以下情况满足一种的情况下才可以调用未定义的方法：

* 接收者是 `dynamic` 静态类型
* 接收者是静态类型且定义了 `noSuchMethod()` 方法且与 `Object` 的定义不同

这块不太理解，后面再补充。。。

### 枚举类型

枚举类用于定义表示固定数量常量的类型。

```dart
enum Color { red, green, blue }
```

枚举类成员的默认 getter，返回从 0 计数的索引值。

```dart
assert(Color.red.index == 0);
assert(Color.green.index == 1);
assert(Color.blue.index == 2);
```

`values` 常量返回了枚举类全部值组成的数组。

```dart
List<Color> colors = Color.values;
assert(colors[2] == Color.blue);
```

在 `switch` 语句中使用枚举时，如果没有覆盖到枚举类的每个值，会有警告。

### Mixins

使用 `with` 关键字来使用一个 mixin。

```dart
class Maestro extends Person with Musical, Aggressive, Demented {
  Maestro(String maestroName) {
    name = maestroName;
    canConduct = true;
  }
}
```

指定可以使用 mixin 的类型：

```dart
mixin MusicalPerformer on Musician {
  // ···
}
```

### 类属性和类方法

#### 静态属性

```dart
class Queue {
  static const initialCapacity = 16;
  // ···
}
```

静态属性在被使用之前不会被初始化。

#### 静态方法

静态方法不能在实例上使用，所以无法访问 `this` 。

{% hint style="info" %}
官方建议使用顶层函数作为工具方法，而不是一个静态方法。（值得商榷）
{% endhint %}

## 泛型

如果你看基础的数组类型 `List` ，你会发现实际的类型是 `List<E>` 。&lt;...&gt; 标记 List 为一个泛型——一个有形参的类型。按照惯例，大多数类型变量都有一个单字母名称，比如 E, T, S, K, V。

### 为什么用泛型?

泛型常被用于保证类型安全，不过相比于允许你的代码运行之外，其实有更多优势：

* 正确的指定泛型可以提高代码质量；
* 可以利用泛型来减少代码重复；

假如你定义了 `List<String>` \(读作 "list of string"\)：

```dart
var names = List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
names.add(42); // Error
```

泛型的另一个好处是可以避免代码重复：

比如，你要定义一个用于缓存对象的接口：

```dart
abstract class ObjectCache {
  Object getByKey(String key);
  void setByKey(String key, Object value);
}
```

后来发现你想要一个字符串类型的版本，于是你写了另一个接口：

```dart
abstract class StringCache {
  String getByKey(String key);
  void setByKey(String key, String value);
}
```

后来，你觉得你需要一个数字类型版本的。。。

泛型可以避免这些麻烦：

```dart
abstract class Cache<T> {
  T getByKey(String key);
  void setByKey(String key, T value);
}
```

这段代码中，T 是备用类型。开发者调用的时候会指定这个类型。

### 使用集合字面量

List, set 和 map 是可以参数化的。

```dart
var names = <String>['Seth', 'Kathy', 'Lars'];
var uniqueNames = <String>{'Seth', 'Kathy', 'Lars'};
var pages = <String, String>{
  'index.html': 'Homepage',
  'robots.txt': 'Hints for web robots',
  'humans.txt': 'We are people, not machines'
};
```

### 使用泛型类型的构造函数

```dart
var nameSet = Set<String>.from(names);
var views = Map<int, View>();
```

### 泛型集合持有的类型

Dart 的泛型类型是固定的，意味着在运行时持有着类型信息。

```dart
var names = List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
print(names is List<String>); // true
```

### 限制泛型类型

```dart
class Foo<T extends SomeBaseClass> {
  // Implementation goes here...
  String toString() => "Instance of 'Foo<$T>'";
}
class Extender extends SomeBaseClass {...}
var someBaseClassFoo = Foo<SomeBaseClass>();
var extenderFoo = Foo<Extender>();
var foo = Foo();
print(foo); // Instance of 'Foo<SomeBaseClass>'
```

可以指定想要限制的参数类型。可以使用参数类型及其子类作为泛型参数。也可以不指定泛型参数。

### 使用泛型函数

起初，Dart 的泛型只能在类中使用。新的语法，_泛型函数_，允许在函数上定义类型参数。

```dart
T first<T>(List<T> ts) {
  // Do some initial work or error checking, then...
  T tmp = ts[0];
  // Do some additional checking or processing...
  return tmp;
}
```

## 类和可见性

`import` 和 `library` 指令可以用来创建模块化和可共享的代码库。库不仅提供 API，而且提供了封装性。以下划线开头的标识符仅在库的内部可见。_每个 Dart 应用程序都是一个库_，即便没有使用 `library` 指令。

### 指定库前缀

```dart
import 'package:lib1/lib1.dart';
import 'package:lib2/lib2.dart' as lib2;

// Uses Element from lib1.
Element element1 = Element();

// Uses Element from lib2.
lib2.Element element2 = lib2.Element();
```

### 引用库的一部分

```dart
// Import only foo.
import 'package:lib1/lib1.dart' show foo;

// Import all names EXCEPT foo.
import 'package:lib2/lib2.dart' hide foo;
```

### 延迟引入

延迟引入是当使用到一个库的时候再引入，一些使用场景：

* 减少 App 启动时间；
* A/B 测试；
* 加载很少使用的功能；

```dart
import 'package:greetings/hello.dart' deferred as hello;
Future greet() async {
  await hello.loadLibrary();
  hello.printGreeting();
}
```

可以多次调用 `loadLibrary` ，库仅会被引用一次。

Dart 会隐式的插入 `loadLibrary()` 到延迟引入的命名空间。函数返回一个 Futrue。

## 异步支持

Dart 的库包含很多的 Future 和 Stream 对象。

### 声明 async 函数

```dart
Future<String> lookUpVersion() async => '1.0.0';
```

函数体可以不使用 Future API。Dart 会在需要时自动创建 Future 对象。

如果函数没有返回一个有用的值，则将其返回类型设为 Futrue&lt;void&gt;。

### 处理 Streams

两种获取 Stream 中值得方式：

* 异步循环\(`await for`\)；
* 使用 Stream API；

{% hint style="info" %}
在使用 `await for` 之前，要明确这确实使代码更清晰，但你确实要等待 stream 的全部结果。例如，通常**不要**在 UI 事件监听中使用 await for，因为 UI 框架会不断的发送事件流。
{% endhint %}

```dart
await for (varOrType identifier in expression) {
  // Executes each time the stream emits a value.
}
```

`expression` 的值必须是 Stream 类型。执行过程如下：

* 等待，直到 stream 发送了一个值；
* 执行 for 循环体，并将变量的值设为发送的值；
* 重复 1 和 2 直到 stream 关闭；

可以使用 `break` 或 `return` 语句，跳出 for 循环并取消订阅流信息。

## 生成器

使用 `generator` 延迟生成一系列值。Dart 内建了 2 种生成器函数：

* 同步生成器：返回迭代器对象；
* 异步生成器：返回流对象；

同步生成器：

```dart
Iterable<int> naturalsTo(int n) sync* {
  int k = 0;
  while (k < n) yield k++;
}
```

异步生成器：

```dart
Stream<int> asynchronousNaturalsTo(int n) async* {
  int k = 0;
  while (k < n) yield k++;
}
```

如果你的生成器是递归的，可以使用 `yield*` 提升性能：

```dart
Iterable<int> naturalsDownFrom(int n) sync* {
  if (n > 0) {
    yield n;
    yield* naturalsDownFrom(n - 1);
  }
}
```

## 可调用的类

实现了 `call()` 方法的类的实例可以像函数一样被调用。

```dart
class WannabeFunction {
  call(String a, String b, String c) => '$a $b $c!';
}

main() {
  var wf = new WannabeFunction();
  var out = wf("Hi","there,","gang");
  print('$out');
}
```

## Isolates

大多数的电脑，甚至移动终端都是多核 CPU。要充分利用这些 CPU 核心，开发者一般使用共享内存来保证多线程程序正确执行。然而，共享内存容易导致潜在的问题，并导致代码出错。

Dart 代码运行在 isolates 中而非线程。每个 isolate 都有自己的堆内存，来确保每个 isolate 的状态不会被其他 isolate 访问。

## Typedefs

Dart 中，函数是对象，就像字符串和数字是对象一样。`typedef` 或者 `function-type alias` 可以给函数类型命名，当声明字段或者返回值时使用。当函数类型赋值给变量时，typedef 保留了类型信息。

下面的代码没有使用 typedef：

```dart
class SortedCollection {
  Function compare;

  SortedCollection(int f(Object a, Object b)) {
    compare = f;
  }
}

// Initial, broken implementation.
int sort(Object a, Object b) => 0;

void main() {
  SortedCollection coll = SortedCollection(sort);

  // All we know is that compare is a function,
  // but what type of function?
  assert(coll.compare is Function);
}
```

将 `f` 赋值给 `compare` 时，类型信息丢失了。`f` 的类型是 `(Object, Object) -> int` 。如果我们使用显式的名称，就可以保留类型信息，开发者和开发工具都可以使用这个信息。

```dart
typedef Compare = int Function(Object a, Object b);

class SortedCollection {
  Compare compare;

  SortedCollection(this.compare);
}

// Initial, broken implementation.
int sort(Object a, Object b) => 0;

void main() {
  SortedCollection coll = SortedCollection(sort);
  assert(coll.compare is Function);
  assert(coll.compare is Compare);
}
```

{% hint style="info" %}
目前，typedefs 只能在函数类型上使用。
{% endhint %}

因为 typedefs 仅是别名，他们还提供了一种检查任意函数类型的方法。

```dart
typedef Compare<T> = int Function(T a, T b);

int sort(int a, int b) => a - b;

void main() {
  assert(sort is Compare<int>); // True!
}
```

## 元数据

使用元数据给你的代码添加其他额外信息。元数据注解是以 `@` 字符开头，后面是一个编译时 常量\(例如 `deprecated`\)或者 调用一个常量构造函数。

有三个注解所有的 Dart 代码都可以使用： `@deprecated`、 `@override`、 和 `@proxy`。

```dart
class Television {
  /// _Deprecated: Use [turnOn] instead._
  @deprecated
  void activate() {
    turnOn();
  }

  /// Turns the TV's power on.
  void turnOn() {...}
}
```

你还可以定义自己的元数据注解。 下面的示例定义了一个带有两个参数的 @todo 注解：

```dart
library todo;

class Todo {
  final String who;
  final String what;

  const Todo(this.who, this.what);
}
```

```dart
import 'todo.dart';

@Todo('seth', 'make this do something')
void doSomething() {
  print('do something');
}
```

元数据可以在 library、class、typedef、type parameter、constructor、factory、function、field、parameter 或者 variable 声明之前使用，也可以在 import 或者 export 指令之前使用。 使用反射可以在运行时获取元数据 信息。

