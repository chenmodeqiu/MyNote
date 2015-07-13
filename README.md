# MyNote
个人笔记，ios学习相关。
## 添加一个控件，淡入淡出效果
###控件的透明度alpha属性
+ 该属性设置透明度，对整个控件有效
```objc
// 8.控制提示框是否显示
    if (self.addBtn.enabled == NO) {
        self.hudLabel.text = @"商品柜已经满了, 不要再买买买了...";
        // 显示提示框
        self.hudLabel.hidden = NO;
        // 注意: 如果是通过控件的alpha控制背景颜色的透明度, 那么整个控件都会变为透明
//        self.hudLabel.alpha = 0.5;
    }
```

###颜色有关
+ 1.常用颜色组成 RGB

    R：red

    G：green

    B：blue
+ 2. ARGB
    A:alpha只颜色的透明度

###透明度设置过程——》动画
####动画分类
+ 头尾式动画
 写在开始和结束之间的代码，都会被执行动画
 开始动画 beginAnimations:nil context:nil

 结束动画 commitAnimations

```objc
// 动画开始
    [UIView beginAnimations:nil context:nil];
    // 设置动画时间
    [UIView setAnimationDuration:3];

    [UIView setAnimationDelegate:self];

    // 只要写在开始和结束之间的代码, 就会被执行动画
    // 但是: 并不是所有的代码都能够执行动画
    // 只有属性声明中说明了是animatable的属性,才可以执行UIView动画
//    CGRect tempFrame2 = self.hudLabel.frame;
//    tempFrame2.origin.y -= 100;
//    self.hudLabel.frame = tempFrame2;

    self.hudLabel.alpha = 1.0;
    //隐藏属性放入动画中无效，该属性无animatable
//    self.hudLabel.hidden = YES;

    // 动画结束
    [UIView commitAnimations];
```
+ block动画

将执行的代码放入block中，继承与uiview的控件都可以执行uiview中都block动画方法。

```objc
// Duration: 动画执行的时长
// animations: 在block中写需要执行动画的代码
// completion: 动画结束后系统会自动调用该block
// delay: 延迟多少秒
[UIView animateWithDuration:1.0 animations:^{
        self.hudLabel.alpha = 1.0;
    } completion:^(BOOL finished) {
        NSLog(@"动画结束了");
        // 动画结束后1秒控件透明度变为0
        [UIView animateWithDuration:1.0 delay:1.0 options:kNilOptions animations:^{
            self.hudLabel.alpha = 0.0;
        } completion:nil];

    }];
```

如何监听动画都结束？
+ 实现uiviewdelegate，成为其代理。
+ 只要成为UIView动画的代理, 然后实现
\- (void)animationDidStop:(nonnull CAAnimation *)anim finished:(BOOL)flag方法, 就可以监听动画什么时候结束

```objc
//该方法在CAAnimation.h中
- (void)animationDidStop:(nonnull CAAnimation *)anim finished:(BOOL)flag
{
    NSLog(@"%s", __func__);
    [UIView beginAnimations:nil context:nil];
    // 设置动画时间
    [UIView setAnimationDuration:3];
    self.hudLabel.alpha = 0.0;
    [UIView commitAnimations];
}
```
