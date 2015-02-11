---
layout: post
---

# Objective-C之protocol学习

## protocol简介

protocol是Objective-C的一种数据类型，属于Objective-C的特性之一。  
protocol从字面上看是协议的意思，类似于C#、Java等语言的接口，多用于Objective-C委托模式的实现。  
protocol的简单定义如下：  

	@protocol ProtocolName <InheritProtocolName>

	@end

其中ProtocolName是该protocol的名称，InheritProtocolName是继承来的protocol的名称，一般可继承根protocol：NSObject，这样便可以使用NSObject协议的一系列方法了。  

开发者在protocol中定义一系列方法接口和属性，供声明了该protocol的类去实现：  

	@protocol ProtocolName <InheritProtocolName>
	
	method1
	method2
	...

	@end

另外，protocol提供了`@optional`和`@required`关键词来标识方法接口是否必须实现，默认是@required，标识该方法接口必须实现，@optional则标识该方法可以不实现：  

	@protocol ProtocolName <InheritProtocolName>

	@required:
	method1
	
	@optional:
	method2

	@end

实现protocol的类只需要在类的定义处用尖括号将protocol包含进来，实现多个protocol时，各个protocol用分号隔开：  

	@interface TestClass : NSObject<Protocol1, Protocol2, ...>

	@end

然后在具体实现的.m文件中实现各个方法：  

	@implementation TestClass
	
	method1{
	}

	method2{
	}

	...

	@end

## protocol示例

#### 声明protocol：  

	@protocol CarDelegate <NSObject>
	
	- (void) beforeStart;
	- (void) beforeStop;

	@end

#### 中间层：  

	// 车的基类
	@interface CarBase : <NSObject>
	- (void) start;
	- (void) stop;
	@property (nonatomic,assign) id <CarDelegate> delegate;
	@end

	// 车的基类具体实现
	@implementation CarBase
	- (void) start{
		[_delegate beforeStop];
	}
	- (void) stop{
		[_delegate beforeStop];
	}
	@end
	
	// 宝马车
	@interface BMWCar : <CarBase>
	@end
	
	// 宝马车的具体实现
	@implementation BMWCar
	
	- (void) start{
		[super start];
		NSLog(@"BMWCar is starting");
	}
	- (void) stop{
		[super stop];
		NSLog(@"BMWCar is stopping");
	}
	@end		
	
	// 奔驰车
	@interface BenzCar : <CarBase>
	@end
	
	// 奔驰车的具体实现
	@implementation BenzCar
	- (void) start{
		[super start];
		NSLog(@"BenzCar is starting");
	}
	- (void) stop{
		[super stop];
		NSLog(@"BenzCar is stopping");
	}		
	@end

#### 实现protocol：

	@interface CarController : NSObject<CarDelegate>{
		CarBase* bmwCar;
		CarBase* benzCar
	}
	- (void) startBMW;
	- (void) startBenz;	
	- (void) stopBMW;
	- (void) stopBenz;
	@end

	@implementation CarController
	- (id) init{
		if(self = [super init]){
			bmwCar = [[BMWCar alloc]init];
			benzCar = [[BenzCar alloc]init];

			[bmwCar setDelegate:self];	// 指定委托对象
			[benzCar setDelegate:self];	// 指定委托对象
		}
		return self;
	}	
	- (void) startBMW{
		[bmwCar start];
	}		
	- (void) startBenz{
		[benzCar start];
	}	
	- (void) stopBMW{
		[bmwCar stop];
	}		
	- (void) stopBenz{
		[benzCar stop];
	}	
	- (void) beforeStart{
		NSLog(@"car doesn't start");
	}
	- (void) beforeStop{
		NSLog(@"car doesn't stop");
	}
	@end

## 参考

1. <http://www.cnblogs.com/chijianqiang/archive/2012/06/22/objc-category-protocol.html>