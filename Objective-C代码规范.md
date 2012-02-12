#<center>Objective-C Coding Style Guidelines</center>

<center>Henson整理</center>

##参考资料:

*    Apple: [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html "Coding Guidelines for Cocoa")
*    Google: [Objective-C Style Guide](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml "Objective-C Style Guide")
*    Three20: [Source code style guidelines](http://three20.info/article/2010-10-24-Three20-Style-Guide "Source code style guidelines")

##背景介绍

Objective-C是一种动态的面向对象的语言，它是C的扩展。它被设计成具有易读易用的,支持复杂的面向对象设计的编程语言。它是Mac OS X以及iPhone、iPad的主要开发语言。

Cocoa是Mac OS X的主要的应用程序框架。它由一组支持Mac OS X全部特性的,并可用于快速开发的Objective-C类构成。

本文档的目的在于为所有的Mac OS X及iOS的代码提供编程规范指南及最佳实践。

请注意,本指南不是Objective-C的教程。我们假定读者已经对Objective-C非常熟悉。如果你刚刚接 触Objective-C或者需要提高,请阅读[The Objective-C Programming Language](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html "The Objective-C Programming Language")。

##空格与格式

###1. 空格与制表符

使用空格进行缩进。不要在代码中使用制表符。你应该将你的文本编辑器设置成自动将制表符替换成空格。

###2. 行宽

* 每行最多不得超过100个字符。* 通过 “Xcode => Preferences => TextEditing => 勾选Show Page Guide / 输入100 => OK” 来设置提醒。

###3. 方法声明与定义

* 在 - OR + 和返回值之间留1个空格。

		- (void)doSomethingWithString:(NSString *)theString {
    		//TODO 
		}

* 参数列表中，只有参数之间有空格, 方法名和第一个参数间不留空格。如：
	
		- (void)doSomething:(NSString *)theString (NSArray *)theArray {
    		//TODO 
		}

* 当参数过长时，每个参数占用一行，以冒号对齐。如：
 
		- (void)doSomethingWith:(GTMFoo *)theFoo                           rect:(NSRect)theRect                       interval:(float)theInterval {			//TODO
		}

* 如果方法名比参数名短，每个参数占用一行，至少缩进4个字符，且为垂直对齐（而非使用冒号对齐）。如：

 		- (void)short:(GTMFoo *)theFoo          	longKeyword:(NSRect)theRect        	evenLongerKeyword:(float)theInterval {        	//TODO		}

###4. 方法的调用

* 调用方法沿用声明方法的习惯。

* 所有参数应在同一行中，或者每个参数占用一行且使用冒号对齐。如：

		[myObject doFooWith:arg1 name:arg2 error:arg3];
    	//或者
		[myObject doFooWith:arg1                       name:arg2                      error:arg3];


* 和方法的声明一样，如果无法使用冒号对齐时，每个参数一行、缩进4个字符、垂直对其(而非使用冒号对齐)。如:
		[myObj short:arg1        	longKeyword:arg2        	evenLongerKeyword:arg3];

###5. @public和@private

@public以及@private访问标识符应该以一个空格缩进。

	@interface MyClass : NSObject {
	 @public		//TODO 
     @private		//TODO 
    }	@end

###6. Protocols
* 类型标示符、代理名称、尖括号间不留空格。
* 该规则同样适用于：类声明、实例变量和方法声明。如:
	
		@interface MyProtocoledClass : NSObject<NSWindowDelegate> {         @private        	id<MyFancyDelegate> _delegate;
		}
		- (void)setDelegate:(id<MyFancyDelegate>)aDelegate;		@end
* 如果类声明中包含多个protocol，每个protocal占用一行，缩进2个字符。如: 

		@interface CustomViewController : ViewController<
			AbcDelegate, 
			DefDelegate> {			//TODO
		}

##命名

对于可维护的代码,命名规则非常重要。 Objective-C的方法名往往十分长,但代码块读起来就像 散文一样,不需要太多的注释修饰。

###1. 文件名

文件的扩展名应该如下：

* \.h, C/C++/Objective-C的头文件
* \.m, Ojbective-C实现文件* \.mm, Ojbective-C++的实现文件* \.cc, 纯C++的实现文件* \.c, 纯C的实现文件

分类的文件名应该包含被扩展的类的名字，如：GTMNSString+Utils.h或GTMNSTextView+Autocomplete.h。

###2. 类名

* 类名(及其category name和protocol name)的首字母大写，使用首字母大写的形式分割单词。* 在面向特定应用的代码中，类名应尽量避免使用前缀，每个类都使用相同的前缀影响可读性。* 在面向多应用的代码中，推荐使用前缀。如:GTMSendMessage。

###3. 方法名
* 方法名的首字母小写,且使用首字母大写的形式分割单词。方法的参数使用相同的规则。
* 方法名+参数应尽量读起来像一句话(如:)。在这里查看苹果对方法命名的规范。* getter的方法名和变量名应相同。不允许使用“get”前缀。如:		- (id)getDelegate; // 禁止		- (id)delegate;     // 正确* 本规则仅针对Objective-C代码,C++代码使用C++的习惯。

###4. 变量名

* 变量名应使用容易意会的应用全称，且首字母小写,、，且使用首字母大写的形式分割单词。* 成员变量使用“m_”作为前缀(如:“NSString *m_varName;”)。* 常量(#define, enums, const等)使用小写“k”作为前缀，首字母大写来分割单词。如:kInvalidHandle。

##Cocoa和Objective-C特有的规则

###1. 成员变量应该为@private

	@interface MyClass : NSObject { 
	 @private		id m_myInstanceVariable; 
	}
	// public accessors, setter takes ownership 
	- (id)myInstanceVariable;	- (void)setMyInstanceVariable:(id)theVar; 
	@end

###2. 初始化* 在初始化方法中，不要将变量初始化为“0”或“nil”，那是多余的。* 内存中所有的新创建的对象(isa除外)都是0，所以不需要重复初始化为“0”或“nil”。

###3. 避免显式的调用+new方法禁止直接调用NSObject的类方法+new，也不要在子类中重载它。使用alloc和init方法。

###4. \#import VS \#include使用#import引入Ojbective-C和Ojbective-C++头文件，使用#include引入C和C++头文件。

###5. 使用根框架

当你试图从框架(如Cocoa或者Foundation)中包含单独的系统头文件时,实际上包含顶级根框架 编译器要作更少的工作。根框架通常被预编译,并且加载得更快。另外记得使用#import而不是 #include来包含Objective-C的框架。###6. 创建对象时尽量使用autorelease

创建临时对象时，尽量同时在同一行中autorelease掉，而非使用单独的release语句。虽然这样会稍微有点慢，但这样可以阻止因为提前return或其他意外情况导致的内存泄露。通盘来看这是值得的。

###7. 先autorelease，再retain
* 在为对象赋值时，遵从“先autorelease，再retain”。* 在将一个新创建的对象赋给变量时，要先将旧对象release掉，否则会内存泄露。注意:在循环中这种方法会“填满”autorelease pool，稍稍影响效率,但是这个代价是可以接受的。如:
		- (void)setFoo:(GMFoo *)aFoo {			[foo_ autorelease]; // Won't dealloc if |foo_| == |aFoo|
			foo_ = [aFoo retain];    	}

###8. dealloc的顺序要与变量声明的顺序相同* 这有利于review代码。* 如果dealloc调用了其它方法释放成员变量，注释这个方法处理了哪些成员变量的释放。

###9. NSString的属性的setter使用“copy”禁止使用retain，以防止意外的修改了NSString变量的值。如:    
	- (void)setFoo:(NSString *)aFoo {    	[foo_ autorelease];    	foo_ = [aFoo copy];	}	//或    @property (nonatomic, copy) NSString *aString;

###10. 对nil的检查* 仅在有业务逻辑需求时检查nil，而非为了防止崩溃。

* 向nil发送消息不会导致系统崩溃，Objective-C运行时负责处理。

###11. BOOL陷阱Ojbective-C中定义BOOL为无符号字符型，这意味着BOOL类型可以有不同于YES(1)或者NO(0)的值。不要直接把整形转换成BOOL。常见的错误包括将数组的大小、指针值及位运算的结果直接转 换成BOOL，这取决于整型结果的最后一个字节，可能产生一个NO的值。当转换整形至BOOL时，使用三目操作符来返回YES或者NO。
* 将int值转换为BOOL时应特别小心。避免直接和YES比较。* Objective-C中,BOOL被定义为unsigned char,这意味着除了 YES(1) 和NO(0)外它还可以是其他值。禁止将int直接转换(cast or convert)为BOOL。
* 常见的错误包括:将数组的大小、指针值或位运算符的结果转换(cast or convert)为BOOL,因为该BOOL值的结果取决于整型值的最后一位。
* 将整型值转换为BOOL的方法：使用三元运算符返回YES/NO，或使用位运算符(&&,||,!)BOOL、_Bool和bool之间的转换是安全的,但是BOOL和Boolean间的转换不是安全的,所以将Boolean看成整型值。* 禁止直接将BOOL和YES/NO比较，如: 
	
		// 禁止    	BOOL great = [foo isGreat];    	if (great == YES)			//TODO...	
		// 对头		BOOL great = [foo isGreat]; 
		if (great) 			//TODO…

###12. 属性

* 命名：与去掉“m_”前缀的成员变量相同，使用@synthesize将二者联系起来。如:

		// test.h    	@interface MyClass : NSObject {
		 @private
			NSString *m_name;
		}
		
		@property (nonatomic, copy) NSString *name;    	@end    
		// test.m    	@implementation MyClass    	@synthesize name = m_name;    	@end

* 位置：属性的声明紧随成员变量块之后，中间空一行，无缩进。如上例所示。

* 严把权限：对不需要外部修改的属性使用readonly。

* NSString使用copy而非retain。

* CFType使用@dynamic，禁止使用@synthesize。

* 属性声明为nonatomic，除非你需要原子操作。

##Cocoa模式

###1. Delegate Pattern(委托)* delegate对象使用assign，禁止使用retain。因为retain会导致循环索引导致内存泄露，并且此类型的内存泄露无法被Instrument发现，极难调试。
* 成员变量命名为m_delegate，属性名为delegate。
###2. Model/View/Controller模式

* 模型与视图分离:不要假设模型或者数据源的表示方法。保持数据源与表示层之间的接口抽 象。视图不需要了解模型的逻辑(主要的规则是问问你自己,对于数据源的一个实例,有没 有可能有多种不同状态的表示方法)。 

* 控制器与模型、视图分离:不要把所有的“领域逻辑”放进跟视图有关的类中。这命名得代码非常难以重用。使用控制器来写这些代码，但保证控制器不需要了解太多表示层的逻辑。 

* 使用@protocol来定义回调API，如果不是所有的方法都必须实现，使用@optional。
