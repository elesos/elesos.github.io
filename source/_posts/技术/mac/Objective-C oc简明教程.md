---
title: Objective-C oc简明教程
tags:
  - Objective-C
categories:
  - Objective-C
date: 2021-07-04 10:50:15
---

https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Introduction/Introduction.html

## 类

```
@property (readonly) NSString *lastName;
```



```
void SomeFunction();
```

The equivalent Objective-C method declaration looks like this:

```
- (void)someMethod;  减号： it is an instance method, which can be called on any instance of the class
void SomeFunction(SomeType value);
- (void)someMethodWithValue:(SomeType)value;
- (void)someMethodWithFirstValue:(SomeType)value1 secondValue:(AnotherType)value2;
```

调用

```
 [someObject doSomething];
```



```
- (void)saySomething:(NSString *)greeting {
    NSLog(@"%@", greeting);  //%@, used to denote an object    NSLog(@"Hello, World!");
}
NSString *testString = @"Hello, world!";  //@符号  将C字符串转换为OC字符串,还有OC中的绝大部份的关键字都是以@符号开头，如@interface
```


"Jack”，这是一个C语言的字符串

@“Jack”，这是一个OC的字符串常量

NSString类型的指针变量，只能存储OC字符串的地址。

语法格式：

NSString *变量名 = @字符串常量;





减号表示

为实例方法，必须使用类的实例才可以调用的。加号表示类方法，这类方法是可以直接用类名来调用的



中括号可以认为是调用你刚才写的这个方法，通常在Objective-C里说“消息”。

比如C语言里你可以这么写：

this.hello(true);

在Objective-C里，就要写成：

[self hello:YES];

## 与c对比

OC中支持C语言中的所有的数据类型。

OC语言完全兼容C语言，在OC中可以写任意的C代码

## c++调用oc

convert char * to NSString

```
char* cstring = "Try harder";
NSString* objcstring = @(cstring);
```

convert map to NSDictionary


void registerPubProperties(std::map<std::string, std::string> pubProperties){

```
   std::cout << "sdk recv map data:"<<std::endl;
   
   NSMutableDictionary *dict = NSMutableDictionary.alloc.init;
   for(auto &item:pubProperties){
       std::cout<<item.first<<"--->"<<item.second<<std::endl;
       //NSString *key =   [NSString stringWithCString:item.first.c_str() encoding:NSUTF8StringEncoding];
       //NSString *value = [NSString stringWithCString:item.second.c_str() encoding:NSUTF8StringEncoding];
       NSString *key = @(item.first.c_str());
       NSString *val = @(item.second.c_str());
       dict[key] = val;
       
   }
   
   NSLog(@"%@", dict);
```

}

## oc调c++

.m => .mm
