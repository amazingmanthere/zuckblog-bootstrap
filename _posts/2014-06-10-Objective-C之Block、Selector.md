---
layout: post
---

# Objective-C之block、selector

## block

block是代码块的意思，是一种数据类型，使用时跟函数有些类似，可以传入参数和返回值。

#### 基本使用

block的定义格式如下：

	返回值 (^变量名称)(参数1类型, 参数2类型 ...) = ^(参数1类型 参数1名称, 参数2类型 参数2名称 ...){
		// 代码块内容
	}

按照block的定义格式，定义一个用于计算两个整数的和的block：  
	
	int (^sumBlock)(int, int) = ^(int num1, int num2){
		return num1 + num2;
	}

调用定义的block计算：

	int result = sumBlock(5, 7);

#### 访问局部变量和全局变量

block可以直接访问局部变量和全局变量。  
访问全局变量时，可使用和修改全局变量的值，如：

	int num3 = 9;	// 全局变量
	...
	int (^sumBlock)(int, int) = ^(int num1, int num2){
		num3 = 3;
		return num1 + num2 + num3;
	};
	NSLog(@"sum:%d num3:%d", sumBlock(5, 7), num3);

使用局部变量时，只能使用局部变量的值，如果要修改局部变量的值，需要加关键字`__block`，如：  

	__block int num3 = 9;	// 局部变量
	int (^sumBlock)(int, int) = ^(int num1, int num2){
		num3 = 3;
		return num1 + num2 + num3;
	};
	NSLog(@"sum:%d num3:%d", sumBlock(5, 7), num3);

#### 作为函数参数

block还可以作为函数的参数传入。

首先，定义block的名称：  

	typedef int (^calculateBlock)(int, int);

其次，定义用block类型的变量作为参数的函数：  

	- (void)doCalculate:(int)num1 anotherNum:(int)num2 action:(calculateBlock)calculate{
	    calculate(num1, num2);
	}

最后，将block的具体实现作为参数传递给函数：  
	
	// 计算和
	[calculateObj doCalculate:5 anotherNum: 7 ^(int num1, int num2){
        return num1 + num2;
    }];

	// 计算差
	[calculateObj doCalculate:5 anotherNum: 7 ^(int num1, int num2){
        return num1 - num2;
    }];

## selector

selector字面上是选择器的意思，可用于获取函数指针，结果类型为`SEL`。

#### 基本使用

编译时，使用@selector获取：  

	SEL aSelector = @selector(method);

运行时，使用NSSelectorFromString获取：  

	SEL aSelector = NSSelectorFromString(@"methodName");

如果函数需要带参数的，则函数名称后面需要加上分号,如下所示：  
	
	SEL aSelector ＝ @selector(method:);
	SEL aSelector ＝ NSSelectorFromString(@"methodName:");  

一般情况下，如果方法在当前是已知的，直接使用第一种方式即可，当方法未知但知道方法名称时才使用第二种方式。 
接下来，使用函数所在对象的`performSelector`方法就可以调用该函数，SEL结果则作为参数传递进去：  

	[methodObj performSelector:aSelector];  

如果该函数需要传递参数，则使用`performSelector:withObject:`方法带上参数：  

	[methodObj performSelector:aSelector withObject:param1 withObject:param2];

#### 示例

假如在Car类中有一个方法start：  

	- (void)start{

	}

如果要在Car类里面使用start方法，则如下所示：  

	SEL aSelector ＝ @selector(start);

接着调用Car类自身的performSelector方法：  

	[self performSelector:aSelector];

如果在其它类中使用Car类的start方法，则如下所示：  

	SEL aSelector ＝ NSSelectorFromString(@"start"); 

接着调用Car类对象的performSelector方法：  

	[car performSelector:aSelector];

如果start方法有带参数，则start后面需要加上分号,如：  
	
	SEL aSelector ＝ @selector(start:);

## 参考  

1. <https://developer.apple.com/library/ios/documentation/cocoa/Conceptual/Blocks/Articles/bxGettingStarted.html#//apple_ref/doc/uid/TP40007502-CH7-SW1>  
2. <https://developer.apple.com/library/mac/documentation/general/conceptual/devpedia-cocoacore/Selector.html>