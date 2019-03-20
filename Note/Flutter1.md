[Flutter 实战](https://book.flutterchina.club/chapter3/flutter_widget_intro.html)

# Widget 简介

## Widget与Element

`Widget`的功能是“描述一个UI元素的配置数据”，它就是说，`Widget`其实并不是表示最终绘制在设备屏幕上的显示元素，而只是显示元素的一个配置数据。实际上，`Flutter`中真正代表屏幕上显示元素的类是`Element`，也就是说`Widget`只是描述`Element`的一个配置，有关`Element`的详细介绍我们将在本书后面的高级部分深入介绍，现在只需要知道，`Widget`只是`UI`元素的一个配置数据，并且一个`Widget`可以对应多个`Element`，这是因为同一个`Widget`对象可以被添加到UI树的不同部分，而真正渲染时，UI树的每一个`Widget`节点都会对应一个`Element`对象

- `Widget`实际上就是`Element`的配置数据，`Widget`树实际上是一个配置树，而真正的UI渲染树是由`Element`构成；不过，由于`Element`是通过`Widget`生成，所以它们之间有对应关系，所以在大多数场景，可以宽泛地认为`Widget`树就是指`UI`控件树或`UI`渲染树
- 一个`Widget`对象可以对应多个`Element`对象。这很好理解，根据同一份配置`Widget`，可以创建多个实例`Element`


***

## Stateless Widget

继承自`Widget`，重写了`createElement()`方法

```java
@override
StatelessElement createElement() => new StatelessElement(this);
```

`StatelessWidget`用于不需要维护状态的场景，它通常在`build()`方法中通过嵌套其它`Widget`来构建`UI`，在构建过程中会递归的构建其嵌套的`Widget`

***

## Stateful Widget

`StatefulWidget`也是继承自`widget`类，并重写了`createElement()`方法，不同的是返回的`Element` 对象并不相同；另外`StatefulWidget`类中添加了一个新的接口`createState()`

```java
abstract class StatefulWidget extends Widget {
  const StatefulWidget({ Key key }) : super(key: key);

  @override
  StatefulElement createElement() => new StatefulElement(this);

  @protected
  State createState();
}
```

`StatefulElement` 间接继承自`Element`类，与`StatefulWidget`相对应（作为其配置数据）。`StatefulElement`中可能会多次调用`createState()`来创建状态`State`对象

***

## State 

[Flutter-UI绘制解析](https://juejin.im/post/5c866cf6f265da2de165d89d)

一个`StatefulWidget`对应一个`State`，`State`表示与其对应的`StatefulWidget`要维护的状态，`State`中的保存的状态信息可以：

1. 在`widget build`时可以被同步读取
2. 在`widget`生命周期中可以被改变，当`State`被改变时，可以手动调用其`setState()`方法通知`Flutter framework`状态发生改变，`Flutter framework`在收到消息后，会重新调用其`build()`重新构建`widget`树，更新`UI`

`State`中有两个常用属性：	

1. `Widget`:表示与该`State`实例关联的`widget`实例，由`Flutter framework`动态设置
> 注意，这种关联并非永久的，因为在应用声明周期中，`UI`树上的某一个节点的`widget`实例在重新构建时可能会变化，但`State`实例只会在第一次插入到树中时被创建，当在重新构建时，如果`widget`被修改了，`Flutter framework`会动态设置`State.widget`为新的widget实例

2. `context`:`BuildContext`类的一个实例，表示构建`widget`的上下文，它是操作`widget`在树中位置的一个句柄，它包含了一些查找、遍历当前`Widget`树的一些方法。**每一个`widget`都有一个自己的`context`对象**

***

## 生命周期

### initState()

初始化，`Weidget` 第一次插入 `Weidget` 会调用<br>
只一次，做一些状态初始化、消息订阅<br>
**不能调用`BuildContext.inheritFromWidgetOfExactType`**

***

### didChangeDependencies()

当 `State` 依赖对象发生改变，回调

例如：在之前`build()` 中包含了一个`InheritedWidget`，然后在之后的`build()` 中`InheritedWidget`发生了变化，那么此时`InheritedWidget`的子`widget`的`didChangeDependencies()`回调都会被调用。典型的场景是当系统语言Locale或应用主题改变时，`Flutter framework`会通知`widget`调用此回调

***

### build(BuildContext context)

构建，多次调用

1. 在调用`initState()`之后
2. 在调用`didUpdateWidget()`之后
3. 在调用`setState()`之后
4. 在调用`didChangeDependencies()`之后
5. 在`State对`象从树中一个位置移除后，会调用`deactivate()`又重新插入到树的其它位置之后

***

### didUpdateWidget()

在`widget`重新构建时，`Flutter framework`会调用`Widget.canUpdate`来检测`Widget`树中同一位置的新旧节点，然后决定是否需要更新，如果`Widget.canUpdate`返回`true`，则会调用此回调。`Widget.canUpdate`会在新旧`widget`的`key`和`runtimeType`同时相等时会返回`true`，也就是说在在新旧`widget`的`key`和`runtimeType`同时相等时`didUpdateWidget()`就会被调用

***

### deactivate()

当`State`对象从树中被移除时，会调用此回调。在一些场景下，`Flutter framework`会将`State`对象重新插到树中，如包含此`State`对象的子树在树的一个位置移动到另一个位置时，可以通过`GlobalKey`来实现。如果移除后没有重新插入到树中则紧接着会调用`dispose()`方法

### dispose() 

当`State`对象从树中被永久移除时调用；通常在此回调中释放资源

***

### reassemble()

`debug`下，热重载`hot reload`时会被调用，`release` 不会走

***

## State 管理

管理状态常见方法:

- `Widget`管理自己的`state`
- 父`widget`管理子`widget`状态
- 混合管理，父`widget`和子`widge`t都参与管理状态

**如何决定使用哪种：**

- 如果状态是用户数据，如复选框的选中状态、滑块的位置，则该状态最好由父`widget`管理
- 如果状态是有关界面外观效果的，例如颜色、动画，那么状态最好由`widget`本身来管理
- 如果某一个状态是不同`widget`共享的则最好由它们共同的父`widget`管理

在`widget`内部管理状态封装性会好一些，而在父`widget`中管理会比较灵活。有些时候，如果不确定到底该怎么管理状态，那么推荐的首选是在父`widget`中管理






