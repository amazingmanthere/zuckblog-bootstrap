---
layout: post
---

# Objective-C之异常处理
 
## 异常捕获  

Objective-C是支持异常处理的，格式如图所示：  

 ![](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Exceptions/Tasks/Art/flow_control_directive.gif)

使用也是很简单，跟其它高级编程语言差不多，三个部分组成：  
1. 在`@try`中包含有可能抛出异常的代码块  
2. 在`@catch`中捕获并处理异常  
3. 在`@finally`中做释放等操作

捕获到的异常是`NSException`类型。  
**只有NSException类型的异常才能被捕获到，如果是其它类型的异常，程序会直接crash掉。**  

另外，捕获的异常是从子类到父类的顺序一层一层捕获下来的，处理流程如下：  

	@try {
	    // code that throws an exception
	    ...
	}
	@catch (CustomException *ce) { // most specific type
	    // handle exception ce
	    ...
	}
	@catch (NSException *ne) { // less specific type
	    // do whatever recovery is necessary at his level
	    ...
	    // rethrow the exception so it's handled at a higher level
	    @throw;
	}
	@catch (id ue) { // least specific type
	    // code that handles this exception
	    ...
	}
	@finally {
	    // perform tasks necessary whether exception occurred or not
	    ...
	}

## 抛出异常  

抛出异常有两种方式：  
1. 使用关键字`@throw`
2. 向NSException对象发送`raise`消息

**这两者重要的区别是：@throw带的参数可以是其它类型的，比如NSString，而raise只能向NSException类型的对象发送消息。**  

两者的使用例子如下：  

	NSException* myException = [NSException
	        exceptionWithName:@"FileNotFoundException"
	        reason:@"File Not Found on System"
	        userInfo:nil];
	@throw myException;
	// [myException raise]; /* equivalent to above directive */

在@catch中使用@throws重新抛出异常时，可以把  

	@throw e;
简写成
	
	@throw;

## 参考

1. <https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Exceptions/Tasks/HandlingExceptions.html#//apple_ref/doc/uid/20000059-SW1>  
2. <https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Exceptions/Tasks/RaisingExceptions.html#//apple_ref/doc/uid/20000058-BBCCFIBF>