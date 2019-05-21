# Variables

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



