# **轮播组件：JYCarousel**

**开源库名称：** **JYCarousel**

**开源库当前版本：** **0.0.1**

**开源库简介：** 这是一个使用起来非常简单的开源轮播库，自带下载和缓存，不会造成循环引用，不用考虑定时器不会销毁（我都已经帮你做好了）。

---


## **1. 轮播基本原理**

使用三个imageView添加到ScrollView，始终保持中间的imageView在可视界面里。当前的imageView滚动到下一个imageView，然后把下一个imageView滚动到三个imageView的中心位置，在这过程中赋值的时候是三个imageView同时赋值，滚动的时候找到最中间的imageView，把这个imageView的tag值设置为当前的索引，滚动完成后把这个imageView设置为中心滚动位置。

比如三张图A、B、C。要做的scrollview实际上应该是五张的大小顺序是C、A、B、C、A。初始偏移量设置到第二张，监听scrollview滑动事件。判断偏移量。当偏移量在第一张时将偏移量修改到第四张，当偏移量在第五张时将偏移量调整到第二章。这样在循环时比较流畅，才能无缝无限循环滚动


## **2. 代码文件结构和功能**

**JYCarousel**

- **JYCarousel**
	- 作用：轮播组件的创建和开始
- **JYConfiguration** 
	- 作用：轮播组件的自定义配置，配置你想要的效果
- **JYPageControl** 
	- 作用：轮播组件的指示器样式
- **JYWeakTimer**
	- 作用：轮播组件的弱引用定时器，解决NSTimer不能销毁的问题
- **UIImageView+JYImageViewManager** 
	- 作用：imageView请求网络图片的分类
- **JYImageDownloader**
	-  作用：请求网络图片
- **JYImageCache**
	- 作用：网络图片的缓存
- **JYCarouselAnimation**
	- 作用：轮播翻页的效果动画


## **3. 轮播组件的使用**

**提供两个初始化方法：**
<pre>
/**
 block方式回调初始化
 @param frame       frame
 @param configBlock 轮播属性配置
 @param clickBlock  点击回调
 @return carousel
 */
- (instancetype)initWithFrame:(CGRect)frame
                  configBlock:(CarouselConfigurationBlock)configBlock
                   clickBlock:(CarouselClickBlock)clickBlock;


/**
 delegate方式回调初始化
 @param frame       frame
 @param configBlock 轮播属性配置
 @param target      delegate
 @return carousel
 */
- (instancetype)initWithFrame:(CGRect)frame
                  configBlock:(CarouselConfigurationBlock)configBlock
                       target:(id<JYCarouselDelegate>)target;
</pre>

**使用举例：**
<pre>
- (void)addCarouselView1{
    __weak typeof(self) weakSelf = self;
    //图片数组（或者图片URL，图片URL字符串，图片UIImage对象）
    NSMutableArray *imageArray = [[NSMutableArray alloc] initWithArray: @[@"1.jpg",@"2.jpg",@"3.jpg",@"4.jpg"]];
   JYCarousel *carouselView = [[JYCarousel alloc] initWithFrame:CGRectMake(0, 64, ViewWidth(self.view), 100) configBlock:^JYConfiguration *(JYConfiguration *carouselConfig) {			
   		 //配置指示器类型
        carouselConfig.pageContollType = LabelPageControl;
        //配置轮播时间间隔
        carouselConfig.interValTime = 3;
        //配置轮播翻页动画
        carouselConfig.pushAnimationType = PushCube;
        //配置动画方向
        carouselConfig.animationSubtype = kCATransitionFromRight;
        return carouselConfig;
    } clickBlock:^(NSInteger index) {
    	  //点击imageView回调方法
        [weakSelf clickIndex:index];
    }];
    //开始轮播
    [carouselView startCarouselWithArray:imageArray];
    [self.view addSubview:carouselView];
}
</pre>

如果使用Delegate可以遵循协议，实现代理方法也可以实现回调

## **4. 注意事项：**

内存得不到释放造成内存泄露，使用循环引用了。请注意使用。看下面在block回调处，对Self是使用弱引用的，不然内存是得不到释放的。
<pre>
// 请使用weakSelf，不然内存得不到释放
  __weak typeof(self) weakSelf = self;
  //图片数组（或者图片URL，图片URL字符串，图片UIImage对象）
  NSMutableArray *imageArray = [[NSMutableArray alloc] initWithArray: @[@1.jpg,@2.jpg,@3.jpg,@4.jpg]];
  JYCarousel *carouselView = [[JYCarousel alloc] initWithFrame:CGRectMake(0, 64, ViewWidth(self.view), 100) configBlock:nil clickBlock:NSInteger index {
    //点击imageView回调方法
    [weakSelf clickIndex:index];
  }];
  //开始轮播
 [carouselView startCarouselWithArray:imageArray];
 [self.view addSubview:carouselView];
</pre>


## **喜欢就给个Star吧！**