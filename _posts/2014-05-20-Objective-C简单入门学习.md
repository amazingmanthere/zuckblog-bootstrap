---
layout: post
---

# Objective-C简单入门学习

## 简单介绍

Objective-C是一门面向对象的高级变成语言，由Stepstone公司的Brad Cox与Tom Love在1980年代初发明。Objective-C吸收了Smalltalk的特性，扩展了标准的ANSI C语言，最初应用在NeXTSTEP操作系统上，后来在OS X和iOS上继承了下来。  
Objective-C应用于iPhone、iPad等使用iOS系统的产品的程序开发，使用Xcode可以很方便的编写Objective-C代码。Xcode是Apple公司免费提供的强大的IDE(集成开发环境)，并运行在Apple公司的Mac操作系统下。  
Windows下，可以使用GNUstep框架编译Objective-C代码。当然，最好还是使用Mac系统下的Xcode编写Objective-C代码。

## 基本语法

#### 命名

Objective-C的变量名称和方法名称都是区分大小写的。名称以字母或者下划线开头，不能以数字开头。  

#### 结构

Objective-C编写的代码一般由一个.h文件和一个.m文件组成，.h文件用于定义，比如定义类、协议等等，.m文件用于具体实现。

#### 注释

单号注释时，Objective-C使用`\\`；  
多行注释时，Objective-C使用`/**/`。

#### 引用

Objective-C使用`import`或者`include`来引用其它头文件，如：  

	import <MyTest.h>
	import "MyTest.h"
	include <MyTest.h>
	include "MyTest.h"

`import`与`include`的区别在于，import只会引用一次头文件，不会重复引用。而include是C语言支持的引用标识，不能保证只引用一次头文件，需要自己处理重复引用的问题。  
尖括号与双引号的区别在于，尖括号只会在include文件夹中查找文件，找不到就会出错，而双引号则会先在文件所在目录中查找，找不到再去include文件夹中查找。因此，引用Objective-C本身提供的头文件可以使用尖括号，引用自己的头文件可以使用双引号。

#### 类

Objective-C是一门面向对象的高级编程语言，因此操纵的单位是类。  
类的定义放在.h文件里，类的实现则放在.m文件里。  
.h文件：

	#import <Foundation/Foundation.h>
	@interface className : NSObject
	@end

.m 文件：  

	#import "className.h"

	@implementation className
	@end

#### 属性

Objective-C中，使用`@property`声明属性，如：  

	@interface Student : NSObject
	@property NSString* name;
	@property int age;
	@end

声明后，属性就具有读写的功能。如果是在类内部使用，可以直接通过`_name`、`_age`这样变量名称前面加下划线的方式使用。 

如果属性是只读的，可以在`@property`后面使用`readonly`来标识，如：

	@ property (readonly) NSString* name;

#### 方法

Objective-C中，方法分为实例方法和静态方法，分别用`-`和`+`标识，在.h文件中定义，在.m文件中实现，如：  

	@interface Student : NSObject
	- (void) updateInfo:(NSString*)name hisAge:(int)age;
	+ (id) createStudent:(NSString*)name hisAge:(int)age;
	@end

示例中`hisAge`是第二个参数的说明，以此类推来定义后面的参数：  

	- (void) updateInfo:(NSString*)name hisAge:(int)age hisAddress:(NSString*)address;

#### 私有变量

Objective-C中，类的私有变量定义放在类名下面，用大括号括起来，如：

	@interface Student : NSObject{
		NSString* tmpName;
		int tmpAge;
	}
	@end

定义私有变量后，在.m文件中直接使用即可。

#### 静态变量

可以在.m文件中直接定义静态变量，如：  

	static int count = 0; 
但是外部不能直接访问这个静态变量，这时可以通过静态方法访问到这个静态变量，如：

	+(int) getCount{
		return count;
	}
	
## 参考

1. <http://zh.wikipedia.org/zh-cn/Objective-C>
2. <https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/ProgrammingWithObjectiveC.pdf>