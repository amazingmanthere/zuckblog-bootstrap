---
layout: post
---

# iPhone开发之UINavigationController

## UINavigationController简介
	
UINavigationController继承至UIViewController，常用来管理和切换多个使用UIViewController展示内容的视图，如图所示：  

![A sample navigation interface](https://developer.apple.com/library/ios/documentation/uikit/reference/UINavigationController_Class/Art/navigation_interface_2x.png)

UINavigationController控制的视图由三个部分组成：UINavigationBar、UIToolBar和UIView，UINavigationBar代表导航栏，UIToolBar代表工具栏，UIView代表内容视图，如图所示：

![The views of a navigation controller](https://developer.apple.com/library/ios/documentation/uikit/reference/UINavigationController_Class/Art/NavigationViews_2x.png)

UINavigatinoController通过维护一个导航栈来对多个UIViewController进行管理，第一个UIViewController是根视图控制器，最后一个UIViewController则表示当前正在显示的视图管理器。  
UINavigatinoController通常作为父类使用，其它类通过继承UINavigationController来拥有UINavigationController的特性。  
	
## UINavigationController使用

#### 初始化

调用方法`initWithRootViewController`可创建一个指定根视图控制器的UINavigatinoController实例。

调用方法`initWithNavigationBarClass`可创建一个自定义导航栏和自定义工具栏的UINavigatinoController实例。

#### 添加视图控制器到栈顶

UINavigatinoController调用方法`pushViewController:animated: `将视图控制器添加到栈顶。  
PS:如果传递进来的视图控制器也是UINavigatinoController对象，运行时会报以下错误：   

	*** Terminating app due to uncaught exception 'NSInvalidArgumentException', reason: 'Pushing a navigation controller is not supported'

#### 移除栈顶视图控制器

UINavigatinoController调用方法`popViewControllerAnimated`移除栈顶视图控制器。

#### 显示或隐藏导航栏

设置属性`navigationBarHidden`或者调用方法`setNavigationBarHidden:animated: `可控制导航栏的显示或隐藏，导航栏默认是显示的。

#### 显示或隐藏工具栏

调用方法`setToolbarHidden:animated: `可控制工具栏的现实或隐藏，工具栏默认是隐藏的。

#### 获取栈顶视图控制器

调用属性`topViewController`可获取到栈顶视图控制器。

#### 获取当前显示视图控制器

调用属性`visibleViewController`可获取到当前正在显示的视图控制器。

## 参考

1. <https://developer.apple.com/library/ios/documentation/uikit/reference/UINavigationController_Class/Reference/Reference.html>