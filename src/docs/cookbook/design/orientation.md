---
title: Updating the UI based on orientation
title: 根据屏幕方向更新界面
prev:
  title: Exporting fonts from a package
  title: 以 package 的方式使用字体
  path: /docs/cookbook/design/package-fonts
next:
  title: Using Themes to share colors and font styles
  title: 使用 Themes 统一颜色和字体风格
  path: /docs/cookbook/design/themes
---

In certain cases, it can be handy to update the design of an app when the user
rotates their screen from portrait mode to landscape mode. For example, we may
want to show one item after the next in portrait mode, yet put those same items
side-by-side in landscape mode.

在有些情况下，用户把屏幕从纵向转到横向模式时，更新应用程序的设计可以很方便。比如，我们想在纵向视图里一个接一个展示对象，而在横向视图里并列展示。



In Flutter, we can build different layouts depending on a given
[`Orientation`]({{site.api}}/flutter/widgets/Orientation-class.html).
In this example, we'll build a list that displays 2 columns in portrait mode and
3 columns in landscape mode.

在flutter中，我们可以根据给定的 [`Orientation`]({{site.api}}/flutter/widgets/Orientation-class.html) 构建不同的布局。

在下面这个例子里，我们会构建一个列表，在纵向显示 2 列，横向显示 3 列。



## Directions

## 方向

  1. Build a `GridView` with 2 columns

     构建一个 2 列的 `GridView` 

  2. Use an `OrientationBuilder` to change the number of columns

     用一个 `OrientationBuilder` 来改变列的数量

## 1. Build a `GridView` with 2 columns

## 1. 构建一个 2 列的 `GridView` 

First, we'll need a list of items to work with. Rather than using a normal list,
we'll want a list that displays items in a Grid. For now, we'll create a grid
with 2 columns.

首先，我们需要一个可以用的项目列表。我们不使用普通列表，而是要把列表中的内容展示在网格里。现在，我们要创建一个包含2列的网格。



<!-- skip -->

```dart
GridView.count(
  // A list with 2 columns
  crossAxisCount: 2,
  // ...
);
```

To learn more about working with `GridViews`, please see the
[Creating a grid list](/docs/cookbook/lists/grid-lists/) recipe.

要学习更多关于使用 `GridViews` 的知识，请查看 [Creating a grid list](/docs/cookbook/lists/grid-lists/) 。

## 2. Use an `OrientationBuilder` to change the number of columns

## 2. 用 `OrientationBuilder` 来改变列数

In order to determine the current `Orientation`, we can use the
[`OrientationBuilder`]({{site.api}}/flutter/widgets/OrientationBuilder-class.html)
Widget. The `OrientationBuilder` calculates the current `Orientation` by
comparing the width and height available to the parent widget, and rebuilds
when the size of the parent changes.

为了确定当前的 `Orientation`，我们可以使用 [`OrientationBuilder`]({{site.api}}/flutter/widgets/OrientationBuilder-class.html) Widget。 `OrientationBuilder` 通过比较父 widget 可用的宽度和高度来计算当前的 `Orientation`，这样就可以在父 widget 大小改变时重建。



Using the `Orientation`, we can build a list that displays 2 columns in portrait
mode, or 3 columns in landscape mode.

使用 `Orientation`，我们可以构建一个在纵向显示 2 列，而横向显示 3 列的列表。

<!-- skip -->
```dart
OrientationBuilder(
  builder: (context, orientation) {
    return GridView.count(
      // Create a grid with 2 columns in portrait mode, or 3 columns in
      // landscape mode.
      crossAxisCount: orientation == Orientation.portrait ? 2 : 3,
    );
  },
);
```

Note: If you're interested in the orientation of the screen, rather than
the amount of space available to the parent, please use
`MediaQuery.of(context).orientation` instead of an `OrientationBuilder` Widget.

注意：如果你更关心屏幕的方向，而不是父组件可用的空间大小，请不要用 `OrientationBuilder`，用 `MediaQuery.of(context).orientation`。

## Complete example

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final appTitle = 'Orientation Demo';

    return MaterialApp(
      title: appTitle,
      home: OrientationList(
        title: appTitle,
      ),
    );
  }
}

class OrientationList extends StatelessWidget {
  final String title;

  OrientationList({Key key, this.title}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(title)),
      body: OrientationBuilder(
        builder: (context, orientation) {
          return GridView.count(
            // Create a grid with 2 columns in portrait mode, or 3 columns in
            // 创建一个在纵向展示两列，横向展示三列的网格。
            // landscape mode.
            crossAxisCount: orientation == Orientation.portrait ? 2 : 3,
            // Generate 100 Widgets that display their index in the List
            // 生成 100 个在列表中展示其索引的 Widget
            children: List.generate(100, (index) {
              return Center(
                child: Text(
                  'Item $index',
                  style: Theme.of(context).textTheme.headline,
                ),
              );
            }),
          );
        },
      ),
    );
  }
}
```

![Orientation Demo](/images/cookbook/orientation.gif){:.site-mobile-screenshot}