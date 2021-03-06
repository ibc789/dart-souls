# 变量与基本类型

先来认识Dart的变量和基本数据类型。

## 变量

变量用于存储数据，Dart的变量存储的是对象的引用，即它们都指向某个对象。

### 变量声明

有两种基本的变量声明方式，一种是不指定类型，即使用`var`（变量此时的类型为`dynamic`）

```dart
// name变量指向一个内容为'bob'的字符串对象
var name = 'bob';
```

另一种是指定类型

```dart
// 显式指明name变量为字符串类型
String name = 'bob';
```

指定或不指定类型，由开发者自己决定；指定类型将获得更好的开发体验，比如：更多更全面的代码提示。

变量存储的是对象的引用，即变量都指向某个对象（没有初始化的变量指向`null`对象）。

### 默认值

Dart的变量有一个显著区别于其他语言的特点，即无论什么类型的变量，在没有赋初始值的情况下，默认为`null`

```dart
// 未指定类型的变量
var name;
// 指定为数字类型的变量
int age;

// 使用print函数打印，两个变量都为null
print(name); // null
print(age); // null
```

### 作用域

Dart使用词法作用域（也称为静态作用域），即变量的作用域在其定义时就已经确定。

作用域由变量所处代码块（大括号）的层级决定，层次越深，作用域越小。

不被任何代码块包含的变量通常称为顶层变量，它们的作用域范围最大

```dart
var top = true; // 顶层变量

main() {
  var inMain = 'main';
  print(inMain); // OK
  print(top); // OK
  print(inFn); // 错误，无法访问
  print(inIf); // 错误，无法访问

  fn() {
    var inFn = 'fn';
    print(inFn); // OK
    print(inMain); // OK
    print(top); // OK
    print(inIf); // 错误，无法访问

    if (top) {
      var inIf = 'if';
      print(inIf); // OK
      print(inFn); // OK
      print(inMain); // OK
      print(top); // OK
    }
  }
}
```

### final与const

如果变量内容不会改变，建议使用`final`或`const`替代`var`，或者搭配类型使用。

`final`表示变量只能被赋值一次；`const`表示变量是编译时常量，`const`变量默认为`final`变量。

需要注意的是，顶层`final`变量与[类变量](/language/class_i.md)在首次访问时才执行初始化

```dart
// 定义final变量并使用函数进行初始化
final name = initName();
// name = 'john'; // 错误，final变量不能再次赋值!

// 定义用于初始化name的函数（下一节将对函数进行深入讲解）
String initName() {
  print('init name');
  return 'bob';
}

// 常量
const pi = 3.14;
// 由常量值计算得到的也是常量
const area = pi * 2 * 2;

main() {
  print(pi); // 3.14
  print(area); // 12.56
  print(name); // 先打印 init name，再打印 bob，即直到访问name时initName函数才执行
}
```

## 基本数据类型

Dart的基本数据类型有三种：数字、字符串与布尔。基本数据类型一般通过字面量进行创建，如：1，'bob'，true 等。

### 数字

常用的数字类型有两种，一种是整数`int`，另一种是双精度浮点数`double`。

`int`与`double`都继承自数字类型`num`，在需要同时支持整数与浮点数时可以使用`num`

```dart
int age = 28;
double percent = 0.23;
num price = 2.3;
```

数字对象具备多种数学计算相关的属性与方法

```dart
int size = 9;
// 是否偶数
print(size.isEven); // false
// 是否奇数
print(size.isOdd); // true
// 转换为6进制字符串
print(size.toRadixString(6)); // 13
......

double pi = 3.1415926;
// 向上取整
print(pi.ceil()); // 4
// 转成整形
print(pi.toInt()); // 3
// 转换为固定精度字符串
print(pi.toStringAsPrecision(2)); // 3.1
......
```

### 字符串

字符串是由单引号或双引号包裹的字符序列，它支持使用`${ 表达式 }`进行插值，插值表达式为标识符时可以省略`{}`

```dart
String name = 'bob';
String message = 'Hi ${name.toUpperCase()}'; // Hi BOB
String text = 'Welcome $name'; // Welcome bob
```

连接多个字符串可以使用`+`，而多个相邻的字符串也会自动连接在一起，多行字符串则使用`'''`或`"""`创建

```dart
// 连接字符串
String message1 = 'Hello' + ' World'; // Hello World

// 相邻字符串会自动连接
String message2 = 'Hello'
                 ' World'; // Hello World

// 使用'''创建多行字符串
String multiMessage = '''
Hello
again!
'''; // Hello
     // again!
```

字符串对象自带多种实用属性与方法

```dart
String name = 'bob';
// 长度
print(name.length); // 3
// 是否为空
print(name.isEmpty); // false
// 子字符串
print(name.substring(1)); // ob
// 是否以某字符开头
print(name.startsWith('b')); // true
// 替换所有
print(name.replaceAll('b', 'w')); // wow
......
```

### 布尔

布尔类型只有两个对象，`true`和`false`。

有别于很多动态语言，Dart中的`true`是唯一“正确”的值，所有非`true`的值都被视为`false`

```dart
// 以下Dart代码也是合法的JavaScript代码，但是执行结果完全不同
var name = 'bob';
if (name) {
  // 在JavaScript中，name被视为true，此处代码会执行
  // 在Dart中，name被视为false，此处代码不会执行
  var message = 'hi ' + name;
}
```



