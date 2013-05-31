---
layout: post
title: 关于对象安全释放
description: Object-C
category: blog
---
###self.xx = nil 或self setXxx:nil]
安全释放
可能有个例子形容是妥当的。

想象我们的对象是一条狗，狗想要跑掉（被释放）。

strong型指针就像是栓住的狗。只要你用牵绳挂住狗，狗就不会跑掉。如果有5个人牵着一条狗（5个strong型指针指向1个对象），除非5个牵绳都脱落 ，否着狗是不会跑掉的。

weak型指针就像是一个小孩指着狗喊到：“看！一只狗在那” 只要狗一直被栓着，小孩就能看到狗，（weak指针）会一直指向它。只要狗的牵绳脱落，狗就会跑掉，不管有多少小孩在看着它。

只要最后一个strong型指针不再指向对象，那么对象就会被释放，同时所有的weak型指针都将会被清除。


###[_xx release];_xx = nil;
安全释放


### [self.xx  setYy] 或[_xx setYy];
这个效果是一样的。

### set method 用synthesize来让系统生成set方法
-(void)setAbc:(id)newAbc
{
	if(abc != newAbc){
		[abc release];
		abc = [newAbc retain];// retain or copy 取决于声明的属性
	}
}
只有当[self setAbc:abc]或self.abc = a;时调用。其他都是等同于_abc;

###@synthesize xx = _xx;
当前最新的xcode版本是默认上面的方法的
在 32-bit 时，如果类的 @interface 部分没有进行 ivar 声明，但有 @property 声明，在类的 @implementation 部分有响应的 @synthesize，则会得到类似下面的编译错误：
Synthesized property 'xX' must either be named the same as a compatible ivar or must explicitly name an ivar
在 64-bit时，运行时系统会自动给类添加 ivar，添加的 ivar 以一个下划线"_"做前缀。
上面声明部分的 @synthesize window=_window; 意思是说，window 属性为 _window 实例变量合成访问器方法。

也就是说，window属性生成存取方法是setWindow，这个setWindow方法就是_window变量的存取方法，它操作的就是_window这个变量。
下面是一个常见的例子
@interface MyClass:NSObject{
　　MyObjecct *_myObject;
}
@property(nonamtic, retain) MyObjecct *myObject;
@end

@implementatin MyClass
@synthesize myObject=_myObject;
这个类中声明了一个变量_myObject，又声明了一个属性叫myObject,然后用@synthesize生成了属性myObject的存取方法，这个存取方法的名字应该是：setmyObject和getmyObject。@synthesize myObject=_myObject的含义就是属性myObject的存取方法是做用于_myObject这个变量的。
这种用法在Apple的Sample Code中很常见，



### const CGFloat kWidth = 1  或 #define  kWidth 1 或 static NSString * kWidth = @"123";
#define 用来定义方法，常数（只替代不检查类型）
#const  用来定义常数据，并检查数据类型
#static 用来定义静态对象

```
加油！
```
