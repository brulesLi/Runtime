# Runtime

runtime  
//导入运行框架
//Build  Setting  ->搜索msg  -->  设置为NO
import "Person.h"
import "ViewController.h"

@interface ViewController ()

@end

@implementation ViewController
pragma mark--发送消息

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
import <Foundation/Foundation.h>

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




#方法交换
//
import "UIImage+image.h"
import "VCViewController.h"

@interface VCViewController ()

@end

@implementation VCViewController
#pragma mark--交换方法
#pragma mark 在原有方法添加更多东西
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    
   UIImage *image= [UIImage imageNamed:@"123"];
    //imageNamed加载图片，并不知道图片是否加载成功
    //以后调用imageNamed的时候 ，就知道图片是否加载
    
    //1.每次使用 都需要导入文件
    //2.当一个项目开发太久 使用这种方式不靠谱
    UIImage *iamg=[UIImage ZT_ImageNamed:@"123"];
    
    
    //imageNamed  底层调用 ZT_ImageNamed
    //本质 交换两个方法实现
    
    
    
}


#import "UIImage+image.h"
#import <objc/message.h>

@implementation UIImage (image)
//运行时
//先写其他方法 实现这个功能
+(void)load{
    //交换方法的实现
    
    
    //IMP: 方法实现
    //方法都是定义类里面
//   class_getMethodImplementation(<#__unsafe_unretained Class cls#>, <#SEL name#>)//获取类方法实现
    
    //Class获取那个类的方法
    
    //SEL 获取方法的编号 根据SEL就能去对应的类找方法
 Method    imageNamedMethod = class_getClassMethod([UIImage class], @selector(imageNamed:));//获取类方法
    
    
     Method    ZTImageNamedMethod = class_getClassMethod([UIImage class], @selector(ZT_ImageNamed:));//获取类方法
    
//    class_getInstanceMethod(<#__unsafe_unretained Class cls#>, <#SEL name#>)//获取对象方法
    
    method_exchangeImplementations(imageNamedMethod, ZTImageNamedMethod);
    
    
    
}

////在分类里面不能调用 Super  分类没有父类
//+(UIImage *)imageNamed:(NSString *)name{
//    
//}
+(UIImage *)ZT_ImageNamed:(NSString *)name{
 UIImage  *image =  [UIImage ZT_ImageNamed:name];
    
    if (image==nil) {
        NSLog(@"加载image为空");
        
    }
    
    return image;
    
}
/**
+(UIImage *)ZT_ImageNamed:(NSString *)name{
    UIImage  *image =  [UIImage imageNamed:name];
    
    if (image==nil) {
        NSLog(@"加载image为空");
        
    }
    
    return image;
    
}
 */
@end
