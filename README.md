# Runtime

runtime  
//导入运行框架
//Build  Setting  ->搜索msg  -->  设置为NO
#import "Person.h"
#import "ViewController.h"

@interface ViewController ()

@end

@implementation ViewController
#pragma mark--发送消息

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    
    Person  *p=[[Person alloc] init];
    [p eat];
    
    //运行机制  最主要消息机制
    //什么是消息机制 任何方法调用 都是发送消息
    
//    SEL 方法编号 根据方法标号找到对应 方法实现
//    p  performSelector:<#(SEL)#> withObject:<#(id)#>
     [p eat];
    [p performSelector:@selector(eat)];
    //运行时 发送消息  谁做事情就那谁
    
    //Xcode5之后不建议使用底层方法
    
    //Xcode5之后使用运行时
    //让P发送消息
    objc_msgSend(p, @selector(eat));
    
   objc_msgSend(p, @selector(run:),10);
    
    //类方法  类名调用类方法，本质类名转换成类对象
    [Person  eat];
    
    //获取类对象
     Class  personclass  =  [Person class];
    
    [personclass  performSelector:@selector(eat)];
    
    
    objc_msgSend(personclass, @selector(eat));
    
}



#import <AVFoundation/AVFoundation.h>
#import <Foundation/Foundation.h>

@interface Person : NSObject
+(void)eat;
-(void)eat;

-(void)run:(CGFloat)mi;
@end



#import "Person.h"

@implementation Person
-(void)eat{
    
}
-(void)run:(CGFloat)mi{
    
}
+(void)eat{
    
}
