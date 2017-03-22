# **制作CocoaPods组件以及遇到的一些坑**

## 制作篇

由于公司最近要将项目朝向组件化方向重构,所以需要将每个模块都制作成依赖库.制作完成之后,就可以多人合作开发,开发完成之后只需将依赖库引入到项目中就可以快速拼装成一个功能齐全的app.下面我们就着手做一个测试依赖库`TestCocoapod`

#### 创建自己的gitHub仓库

cocoaPods都托管在gitHub上,所以我们首先在gitHub上创建一个自己的仓库.

* 1、Repository name 仓库名称，必填的；
* 2、Description 仓库的描述信息，可选的；
* 3、仓库的公开性 这里只能选Public，因为Private是要money的；
* 4、是否创建一个默认的README文件 一个完整地仓库，都需要README说明文档，建议选上。当然不嫌麻烦的话你也可以后面再手动创建一个；
* 5、是否添加.gitignore文件 .gitignore文件里面记录了若干中文件类型，凡是该文件包含的文件类型，git都不会将其纳入到版本管理中。是否选择看个人需要；
* 6、license类型 正规的仓库都应该有一个license文件，Pods依赖库对这个文件的要求更严，是必须要有的。因此最好在这里让github创建一个，也可以自己后续再创建。我使用的license类型是MIT。

  ![](/assets/Snip20170321_21.png)


#### clone仓库到本地

仓库制作完成,我们需要向里面添加一些必要的文件.所以需要将它克隆到本地,方式有很多,这里我用终端进行操作.

* `git clone https://github.com/KLFLJAMES/TestCocoapod.git`

* 克隆到本地后文件夹中只包含下图箭头所指的四个文件

  ![](/assets/Snip20170322_23.png)


#### 向本地仓库添加制作依赖可的必要文件

以下几个都是必须的,否则到后面验证.podspec文件会报错

* 创建后缀名为.podspec的文件,该文件是依赖库的描述文件,文件名称要和我们想创建的依赖库名称保持一致,这里我们的名字是TestCocoapod.podspec

  * 创建方式`pod spec create TestCocoapod`

    * 内容 新创建的.podspec文件包含很多东西,我们只需要一些必要的描述
    * 若是依赖某些framework如Foundation,UIKit 需要填写`s.frameworks = 'Foundation', 'UIKit'`
    * 若是依赖其它的依赖库如SD,AF需要填写
      `s.dependency "SDWebImage", "~> 3.7.1"` 
      `s.dependency "AFNetworking", "~> 3.1.0"`

      ```objc
          Pod::Spec.new do |s|
           s.name = 'TestCocoapod'
           s.version = '1.0.1'
           s.summary = 'a extension of color on iOS' 
           s.homepage = 'https://github.com/KLFLJAMES/TestCocoapod'
           s.description = <<-Desc
           It is a extension for ios color,written by Objective-C. 
           DESC
           s.license = 'MIT'
           s.authors = {'KLFLJAMES' => 'fanzhou_James@163.com'}
           s.platform = :ios, '8.0'
           s.source = {:git => 'https://github.com/KLFLJAMES/TestCocoapod.git', :tag => s.version}
           s.source_files = 'TestCocoapod/**/*.{h,m}'
           s.requires_arc = true
          end
      ```




* 创建主类文件,就是我们想让其他人用的类.比如我共享的颜色分类放在了TestCocoapod文件夹里.对应我们上面填写的**s.source\_files**

  * 写法

    ![](/assets/Snip20170322_32.png)
    ![](/assets/Snip20170322_24.png)



* 创建demo工程 为了快速地教会别人使用我们的Pods依赖库，通常需要提供一个demo工程.
  ![](/assets/Snip20170322_25.png)

#### 提交添加的文件到gitHub

提交之前,为了保证.podspec文件合法,我们先要对其进行验证.

* 首先打上tag\(给源代码打版本标签\) 与podspec文件中version一致即可 `git tag 1.0.1   git push --tag`

* 验证合法性  `pod lib lint`成功之后,就会出现下图提示
  ![](/assets/Snip20170322_26.png)

* 更新gitHub仓库

  ```objc
  $ git add .
  $ git status
  $ git commit -m ‘first commit’
  $ git push origin master
  ```

* 发布

  ```objc
  $ pod trunk push TestCocoapod.podspec
  ```

  * 若是第一次发布得需要注册,注册邮箱必须是gitHub的邮箱,名称随意但是无法修改。命令执行完后邮箱会收到带有验证链接的邮件，打开链接就能完成trunk注册流程.然后再执行发布命令
    ![](/assets/Snip20170322_30.png)

  * 成功之后会出现下图提示
    ![](/assets/Snip20170322_27.png)



#### 搜索我们发布的依赖库

`pod search TestCocoapodFanzhou`

![](/assets/Snip20170322_31.png)

## 遇到的问题

##### 新建的GitHub仓库没有选择许可证.没法,只能删了重新建.

##### **没有建demo工程并上传到**`GitHub`**。只传了主类文件，这个时候验证**`podspec`**是通不过的。**

会报如下图错误

![](/assets/Snip20170322_28.png)

##### **swift版本不对**

* 解决 终端执行命令 `echo "3.0" > .swift-version`

![](/assets/Snip20170322_29.png)

##### 执行`pod trunk push TestCocoaPods.podspec` 时，报下图错误

![](/assets/Snip20170322_34.png)

* 解决办法：执行 `pod search TestCocoaPods` ，会发现有一个重名的私有库存在，重新创建一个私有库，CocoaPods不允许有重名的私有库存在。所以我们将TestCocoapod.podspec文件改成**TestCocoapodFanzhou.podspec,描述文件中的s.name也得改**

##### **最后所有的验证都通过了也上传成功了，结果使用pod search仍然搜索不到**

![](/assets/Snip20170322_33.png)

* 解决办法执行 `pod setup`如果最底下会输出setup completed。说明执行pod setup成功.

##### **如果pod search操作还是搜索失败，删除~\/Library\/Caches\/CocoaPods目录下的search\_index.json文件。**

* 执行`rm ~/Library/Caches/CocoaPods/search_index.json`

## 参考博客

### [**发布CocoaPods组件碰到的坑与心得体会**](http://www.jianshu.com/p/e5209ac6ce6b)

### [CocoaPods详解之----制作篇](http://blog.csdn.net/wzzvictory/article/details/20067595)

### [**创建CocoaPods的制作过程**](http://www.jianshu.com/p/98407f0c175b)

### [iOS之创建CocoaPods私有库采坑记](http://www.tuicool.com/articles/Jnau6zR)

# 制作.framework和.bundle**以及遇到的一些坑**

## 制作

## .framework

* 新建工程 创建完成之后将自动生成的.h文件删掉，因为我们既需要头文件也需要实现文件，所以我们自己新建个类文件

  ![](/assets/Snip20170322_35.png)

* 工程的配置

  * TARGETS - &gt;Build Settings -&gt; Architectures:添加 **_armv7s_**；
  * Build Active Architecture Only" 设置为 "NO"

  * Mach-O Type" 设置为 "Static Library"

  * iOS Deployment Target 这是支持最低运行iOS系统版本

* 暴露头文件 实现文件隐藏起来

  ![](/assets/Snip20170322_40.png)

* 生成framework 来到工程目录树,分别选择真机和模拟器进行编译 Products下的文件会由之前的红色变成灰色的
  ![](/assets/Snip20170322_41.png)
  * Show in Finder 会出现真机和模拟器两个文件
    ![](/assets/Snip20170322_38.png)
    ![](/assets/Snip20170322_39.png)


* 合成通用framework  我们需要将上图内的两个文件（FrameworkTest）合并成一个新的文件，并放在上述.framework文件中（真机or模拟器），使之可以在模拟器和真机上均能运行

  * 命令行语句：sudo lipo -create \(此处请填写真机FrameworkTest文件路径 上述的FrameworkTest文件\) \(此处填写模拟器FrameworkTest文件路径\) -output 自定义合成文件存储路径（合成文件的名字FrameworkTest）

  * 替换 将合成的FrameworkTest文件复制到上述真机or模拟机器编译执行的.framework文件夹内，将原来的FrameworkTest文件替换，至此我们就制作完成了Framework框架了，也就是当前的.framework文件

    * 替换原因 未合成的FrameworkTest文件要么只能在真机上运行，要么只能在模拟器上运行。合成文件为的就是让.framework文件既能在真机上运行也能在模拟器上运行



* 使用 将.framework文件添加到工程中,调用.framework内部头文件声明的方法.

## .bundle

* 新建工程 在setting设置BaseSDK 为iOS版本

  ![](/assets/Snip20170322_43.png)
  ![](/assets/Snip20170322_44.png)

* 工程配置

  * 在TARGTS-&gt;Build Settings-&gt;Deployment-&gt;iOS Deployment Target-&gt;选择自己需要支持的最低系统。
  * build后会生成一个bundle包，但在包中的图片由以前的png格式全部变成tiff格式。为了防止这种格式转变。需要在Build Settings-&gt;Architectures-&gt;Base SDK-&gt;选择iOS的SDK要支持的版本。这时TARGETS中Build Setting-&gt;User-Defined中会出现一个新的Key：COMBINE\_HIDPI\_DEBUG\_INFO,把它设置为NO。

* 使用 拖进项目


## 遇到的问题

**framework**

* 如果你用了Category, 别人在用你的framework时会发生崩溃。这时别人在引用时需要在工程中other linker flags中添加-ObjC如果依然有问题，再添加-all\_load。

* 需要支持bitcode, 在TAGETS的Build setting中搜索Other C Flags，添加命令“-fembed-bitcode”。同样的设置在PROJECT中。如果不进行以上操作。别人在集成你的framework时可以编译，亦可以真机测试。唯独在打包时会发出警告并打包失败。警告为framework不支持bitcode！

* 若framework中包含sb\/xib等文件,需要将.framework加入到Copy Bundle Resources 
  ![](/assets/Snip20170322_45.png)


**bundle**

* 若bundle中包含sb\/xib等文件,也需要将.bundle加入到Copy Bundle Resources
  ![](/assets/Snip20170322_46.png)

### 参考博客

### **[xcode7制作framework,结合xib,storyboard,资源文件等](http://www.jianshu.com/p/038dab7accbc)**

### **[iOS开发Xcode7 Framework制作流程简介](http://www.jianshu.com/p/bc89f3e5b58c)**

### [iOS从静态库加载Storyboard并创建ViewController教程](http://www.jianshu.com/p/f2ffe8325519)

### [Loading nib from my own bundle?](http://stackoverflow.com/questions/13825531/loading-nib-from-my-own-bundle)

