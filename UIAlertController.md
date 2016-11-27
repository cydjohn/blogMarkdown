title: UIAlertController
date: 2015-08-10 12:48:21
tags: IOS
---
ios8新推出了一个东西叫`UIAlertController`，用来代替以前的`UIAlertView`和`UIActionSheet`。 

 以前UIAlertView和UIActionSheet是[这样用的](http://caoyudong.com/2015/07/28/UIAlertView%E5%92%8CUIActionSheet/)
 
但是现在不管是UIAlertView还是UIActionSheet，都要通过UIAlertController来实现。*（当然以前的UIAlerView和UIActionSheet都还可以用，如果只需要支持IOS8以上点机型可以放心大胆的用UIAlertController。）*

<!--more-->
 
 
首先要声明一个`UIAlertController`：

~~~objectivec
//OC
UIAlertController *alertController = nil；
~~~
 
然后初始化，如果需要`UIAlertView`的效果就初始化为：`UIAlertControllerStyleAlert`；如果需要`UIActionSheet`的效果就初始化为：`UIAlertControllerStyleActionSheet`

~~~objectivec
//OC

//UIAlertView效果初始化
alertController = [UIAlertController alertControllerWithTitle:@"Title" message:@"message" preferredStyle:UIAlertControllerStyleAlert];

//UIActionSheet效果初始化
alertController = [UIAlertController alertControllerWithTitle:@"Title" message:@"message" preferredStyle:UIAlertControllerStyleActionSheet];
~~~

然后以前添加button的各种函数全都没有了，改成了添加action，每一个action对应一个button。

每个UIAlertAction都带有一个handler，可以通过block处理点击事件。以前是用代理，现在不用了，感觉更科学了。

~~~objectivec
//OC
 /**
     *  UIAlertAction 表示一个按钮，同时，这个按钮带有处理事件的block
     *
     */
    UIAlertAction *action = [UIAlertAction actionWithTitle:@"取消" style:UIAlertActionStyleCancel handler:^(UIAlertAction *action) {
    NSLog(@"点击取消");
    }];
    [alertController addAction:action];
    
    /**
     *  添加需要谨慎操作的按钮，文字默认是红色的
     */
    [alertController addAction:({
        UIAlertAction *action = [UIAlertAction actionWithTitle:@"谨慎操作的按钮" style:UIAlertActionStyleDestructive handler:^(UIAlertAction *action) {
            NSLog(@"谨慎操作的按钮");
        }];
        action;
    })];
~~~

如果要添加输入框要先做个判断，actionSheet是没有办法添加的，强行添加会崩溃

~~~objectivec
//OC
 /**
     *  添加输入框到alertView中，注意，actionSheet是没有办法添加textField的，强行添加会Crash
     */
    if (alertController.preferredStyle == UIAlertControllerStyleAlert) {
        // 添加用户名输入框
        [alertController addTextFieldWithConfigurationHandler:^(UITextField *textField) {
            // 给输入框设置一些信息
            textField.placeholder = @"请输入用户名";
            textField.textAlignment = NSTextAlignmentCenter;
        }];
        // 添加密码输入框
        [alertController addTextFieldWithConfigurationHandler:^(UITextField *textField) {
            textField.placeholder = @"请输入密码";
            textField.secureTextEntry = YES;
            textField.textAlignment = NSTextAlignmentCenter;
        }];
    }
~~~

最后显示

~~~objectivec
//OC
[self presentViewController:alertController animated:YES completion:nil];
~~~


## actionSheet在iPad上崩溃问题
昨天试了下，在iPhone上actionSheet可以很流畅的运行，但是在iPad上运行就会崩溃，报这个错误：

* Terminating app due to uncaught exception 'NSGenericException', reason: 'Your application has presented a UIAlertController (<UIAlertController: 0x7f9831f2a690>) of style UIAlertControllerStyleActionSheet. The modalPresentationStyle of a UIAlertController with this style is UIModalPresentationPopover. You must provide location information for this popover through the alert controller's popoverPresentationController. You must provide either a sourceView and sourceRect or a barButtonItem.  If this information is not known when you present the alert controller, you may provide it in the UIPopoverPresentationControllerDelegate method -prepareForPopoverPresentation.'

看得我云里雾里，里面这些controller好像闻所未闻……后来在stackoverflow上找到了解决方案，只需要加一段代码，申明一个`UIPopoverPresentationController`就可以了：

~~~objectivec
//OC
 	UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"Title" message:@"message" preferredStyle:UIAlertControllerStyleActionSheet];
    UIPopoverPresentationController *popoverController = alertController.popoverPresentationController;
    popoverController.sourceView = sender;
    popoverController.sourceRect = [sender bounds];
    [self presentViewController:alertController animated:YES completion:nil];
~~~

如果你的按钮是个barButton要用下面这段代码👇

~~~objectivec
//OC
UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"Title" message:@"message" preferredStyle:UIAlertControllerStyleActionSheet];
UIPopoverPresentationController *popoverController = alertController.popoverPresentationController;
popoverController.barButtonItem = sender
[self presentViewController:alertController animated:YES completion:nil];
~~~

效果图：

![actionSheet on iPad](http://7xkfbb.com1.z0.glb.clouddn.com/15-8-10/47061500.jpg)


## 如何在actionSheet上加一个UIPickerView
我在某论坛看到过苹果似乎不建议这么做，但是我还是本着*生命在于折腾*人生信条折腾了一个：

~~~objectivec
//OC
	UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"\n\n\n\n\n\n\n\n\n\n\n" message:nil preferredStyle:UIAlertControllerStyleActionSheet];
    UIDatePicker *picker = [[UIDatePicker alloc] init];
    [picker setDatePickerMode:UIDatePickerModeDate];
    [alertController.view addSubview:picker];
    [alertController addAction:({
        UIAlertAction *action = [UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleDefault handler:^(UIAlertAction *action) {
            NSLog(@"OK");
            NSLog(@"%@",picker.date);
        }];
        action;
    })];
    UIPopoverPresentationController *popoverController = alertController.popoverPresentationController;
    popoverController.sourceView = sender;
    popoverController.sourceRect = [sender bounds];
    [self presentViewController:alertController  animated:YES completion:nil];
~~~

苹果并不建议在ActionSheet里面添加UIPickerView，所以才会写出如此诡异的代码……这么写是为了流出足够的面积来放pickerView。

~~~objectivec
//OC
UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"\n\n\n\n\n\n\n\n\n\n\n" 
message:nil preferredStyle:UIAlertControllerStyleActionSheet];
~~~


效果图：
![](http://7xkfbb.com1.z0.glb.clouddn.com/15-8-10/57494645.jpg)