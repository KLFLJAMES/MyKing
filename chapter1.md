## 自定义表情键盘

 [http:\/\/blog.sina.com.cn\/s\/blog\_6317728d0102vwjv.html](http://blog.sina.com.cn/s/blog_6317728d0102vwjv.html)


* 使用系统自带的表情定制表情键盘

```objc
#define EMOJI_CODE_TO_SYMBOL(x) ((((0x808080F0 | (x & 0x3F000) >> 4) | (x & 0xFC0) << 10) | (x & 0x1C0000) << 18) | (x & 0x3F) << 24);

#pragma mark - Life Cycle Methods

- (void)viewDidLoad {
 [super viewDidLoad];
 //获取数组
 NSArray *arrEmotion = [self defaultEmoticons];
 //将表情放到UIButton里
 CGFloat W = 30;
 CGFloat H = 30;
 CGFloat X;
 CGFloat Y;
 for (int i = 0; i < arrEmotion.count; i++) {
 X = 10 +(W+5) * (i%10);
 Y = 260 + (i/10)* (H + 5);
 UIButton *biaoqing =[[UIButton alloc] init];![](/assets/Snip20161228_4.png)
 biaoqing.backgroundColor = [UIColor redColor];![
 biaoqing.frame = CGRectMake(X, Y, W, H);](/assets/Snip20161230_7.png)
 [self.view addSubview:biaoqing];
 NSString *Str = arrEmotion[i];
 [biaoqing setTitle:Str forState:UIControlStateNormal];
 biaoqing.tag = i;
 [biaoqing addTarget:self action:@selector(biaoqingClick:) forControlEvents:UIControlEventTouchUpInside];
 }
}

#pragma mark - Custom Event Methods

- (NSArray *)defaultEmoticons {
 NSMutableArray *array = [NSMutableArray new];
 for (int i=0x1F600; i<=0x1F64F; i++) {
 if (i < 0x1F641 || i > 0x1F644) {
 int sym = EMOJI_CODE_TO_SYMBOL(i);
 NSString *emoT = [[NSString alloc] initWithBytes:&sym length:sizeof(sym) encoding:NSUTF8StringEncoding];
 [array addObject:emoT];
 }
 }
 return array;
}

- (void) biaoqingClick:(UIButton *)biaoqing{
 NSArray *emoji = [self defaultEmoticons];
 NSString *str = emoji[biaoqing.tag];
 self.textView.text = str;
}
```

## git暂存

http://www.tuicool.com/articles/rUBNBvI

* 问题:当出现一个紧急bug需要处理,但是又不适合提交时,就需要将改动过得文件暂存起来,处理完在恢复
* 操作步骤:
  * 在source中进行如下操作起名为`testStash`,这样就暂存成功了
    ![](/assets/Snip20161230_6.png)
  * 恢复,在终端恢复,打开终端
    ![](/assets/Snip20161230_7.png)
  * 操作命令
    ```objc
    // 显示已暂存列表 git stash list
    // 恢复暂存区和工作进度 git stash pop --index stash@{0}
    // 删除所有进度 git stash clear
    ```

## 注意

- 密码
![](/assets/Snip20161226_2.png)
![](/assets/Snip20161226_1.png)

- inset 和 offset
![](/assets/Snip20161220_3.png)
![](/assets/Snip20161220_2.png)

- 


