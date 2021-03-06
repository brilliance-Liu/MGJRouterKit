# MGJRouterKit

[![CI Status](https://img.shields.io/travis/907689522@qq.com/MGJRouterKit.svg?style=flat)](https://travis-ci.org/907689522@qq.com/MGJRouterKit)
[![Version](https://img.shields.io/cocoapods/v/MGJRouterKit.svg?style=flat)](https://cocoapods.org/pods/MGJRouterKit)
[![License](https://img.shields.io/cocoapods/l/MGJRouterKit.svg?style=flat)](https://cocoapods.org/pods/MGJRouterKit)
[![Platform](https://img.shields.io/cocoapods/p/MGJRouterKit.svg?style=flat)](https://cocoapods.org/pods/MGJRouterKit)

## 说明

蘑菇街路由器套件,在该基础之上进行了扩展,使其更加简单快捷,主要解决了参数传递问题.利用“runtime”传递参数,同时可以传递多个参数

## 代码片段

* MGJRouter+TXCreateObject.h文件简介

```objc

#import "MGJRouter.h"

NS_ASSUME_NONNULL_BEGIN

/** 传递参数方法键 */
FOUNDATION_EXPORT NSString *const transferParametersMethodKey;

/**
 *  蘑菇街路由器套件,在该基础之上进行了扩展,使其更加简单快捷.
 */
@interface MGJRouter (TXCreateObject)

/**
 *  创建对象
 *
 *  @param className 类名称
 *
 *  @return 创建的对象
 */
+ (id)createObjectWithClassName:(NSString *)className;

/**
 *  创建对象并传递参数
 *
 *  注意:使用该方法创建对象,必须在该对象中实现"-(void)routeWithParameters:(NSDictionary*)parameters;"方法,否则参数将传递失败
 *
 *  @param className 类名字
 *
 *  @param parameters 参数
 *
 *  @return 创建的对象
 */
+ (id)createObjectWithClassName:(NSString *)className parameters:(NSDictionary*)parameters;

@end

```

* NSObject+TXTransfer.h文件简介

```objc
#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN

/**
 *  利用“runtime”传递参数,同时可以传递多个参数
 */
@interface NSObject (TXTransfer)

/**
 *  传递参数
 *
 *  @param method 方法 (例如:"routeWithParameters:")
 *
 *  @param parameterss 参数集合 (例如:"@[parameters]")
 *
 */
- (void)transferParametersWithMethod:(NSString*)method parameterss:(NSArray*)parameterss;

@end

```

## 使用方法

* 导入头文件MGJRouterKit.h即可使用

```objc
/** 自动注册 */
+ (void)load {
    // 模块1
    [MGJRouter registerURLPattern:@"tx://get/module1" toObjectHandler:^id(NSDictionary *routerParameters) {
        UIViewController *module1ViewController=[MGJRouter createObjectWithClassName:@"TXModule1ViewController"];
        module1ViewController.title=@"模块1";
        UINavigationController *module1NavigationViewController=[[UINavigationController alloc]initWithRootViewController:module1ViewController];
        return module1NavigationViewController;
    }];
    
    // 模块2
    [MGJRouter registerURLPattern:@"tx://get/module2" toObjectHandler:^id(NSDictionary *routerParameters) {
        UIViewController *module2ViewController=[MGJRouter createObjectWithClassName:@"TXModule2ViewController"];
        module2ViewController.title=@"模块2";
        UINavigationController *module2NavigationViewController=[[UINavigationController alloc]initWithRootViewController:module2ViewController];
        return module2NavigationViewController;
    }];
    
    // 模块3
    [MGJRouter registerURLPattern:@"tx://get/module3" toObjectHandler:^id(NSDictionary *routerParameters) {
        UIViewController *module3ViewController=[MGJRouter createObjectWithClassName:@"TXModule3ViewController"];
        module3ViewController.title=@"模块3";
        UINavigationController *module3NavigationViewController=[[UINavigationController alloc]initWithRootViewController:module3ViewController];
        return module3NavigationViewController;
    }];
    
    // 模块4
    [MGJRouter registerURLPattern:@"tx://get/module4" toObjectHandler:^id(NSDictionary *routerParameters) {
        UIViewController *module4ViewController=[MGJRouter createObjectWithClassName:@"TXModule4ViewController"];
        module4ViewController.title=@"模块4";
        UINavigationController *module4NavigationViewController=[[UINavigationController alloc]initWithRootViewController:module4ViewController];
        return module4NavigationViewController;
    }];
    
    // 测试1
    [MGJRouter registerURLPattern:@"tx://push/test1ViewController" toHandler:^(NSDictionary *routerParameters) {
        UIViewController *viewController=[MGJRouter createObjectWithClassName:@"TXTest1ViewController" parameters:routerParameters];
        [[self currentViewController].navigationController pushViewController:viewController animated:YES];
    }];
    
    // 测试2
    [MGJRouter registerURLPattern:@"tx://push/test2ViewController" toHandler:^(NSDictionary *routerParameters) {
        UIViewController *viewController=[MGJRouter createObjectWithClassName:@"TXTest2ViewController" parameters:routerParameters];
        [[self currentViewController].navigationController pushViewController:viewController animated:YES];
    }];
    
    // Web模块1
    [MGJRouter registerURLPattern:@"tx://get/webModule" toObjectHandler:^id(NSDictionary *routerParameters) {
        UIViewController *viewController=[MGJRouter createObjectWithClassName:@"TXWebViewController" parameters:routerParameters];
        return viewController;
    }];
    
    // Web模块2
    [MGJRouter registerURLPattern:@"tx://push/web" toHandler:^(NSDictionary *routerParameters) {
        UIViewController *viewController=[MGJRouter createObjectWithClassName:@"TXWebViewController" parameters:routerParameters];
        [[self currentViewController].navigationController pushViewController:viewController animated:YES];
    }];
}

/** 按钮事件 */
- (void)button2Event:(id)sender{
    NSMutableDictionary *parameters=[NSMutableDictionary dictionary];
    parameters[@"url"]=@"https://github.com/xtzPioneer/MGJRouterKit.git";
    [MGJRouter openURL:@"tx://push/web"
          withUserInfo:parameters
            completion:nil];
}

```

## Installation

MGJRouterKit is available through [CocoaPods](https://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod 'MGJRouterKit'
```

## Author

xtzPioneer, xtz_pioneer@163.com

## License

MGJRouterKit is available under the MIT license. See the LICENSE file for more info.
