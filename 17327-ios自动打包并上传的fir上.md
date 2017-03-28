## iOS自动打包\(**使用fastlane gym编写ipa打包脚本**\)

### 目的

* 编写一个脚本实现自动打包ipa文件.并上传到**fir**上.但对于要上传到**AppStore**的应用,目前还是采取手动上传.

### 具体实现步骤

* 通过fastlane和fir配合自己写的脚本实现.

#### 安装[**fastlane**](https://github.com/fastlane/fastlane)环境

fastlane is a tool for iOS, Mac, and Android developers to automate tedious tasks like generating screenshots, dealing with provisioning profiles, and releasing your application.

* 前提

  * Ruby 2.0 以上
  * Xcode
  * 拥有一个付费的苹果开发者账号


* 安装方式

  * 方式有三种,我们选择最后一种.


![](/assets/Snip20170328_3.png)

* 安装

  * 首先确定安装的Ruby版本是否是2.0以上
    `ruby -v`
  * 然后检查 Xcode 命令行工具\(CLT\)是否安装.在终端窗口中输入命令.若已安装则会报如下错误,若未安装,则点击安装即可.
    `xcode-select --install`

  * 直接安装的话可能会出现下图错误,这是因为苹果在10.11后加上了Rootless机制.

    ![](/assets/Snip20170328_4.png)

    * 解决办法.指定路径,期间会输入AppId等
      `sudo gem install -n /usr/local/bin fastlane`

  * 安装结束后,在终端进行验证
    `fastlane -v`
    ![](/assets/Snip20170328_5.png)



#### 安装安装fir-cli

* 安装

  `gem install fir-cli`


* fir和fastlane一样,由于10.11引入了 rootless, 无法直接安装 fir-cli,所以官方给出了三种安装方式.并且推荐第一种.但是按照第一种方式安装完成之后虽然将fir安装成功了,但是之前安装的fastlane和pod都不见了.还得重新安装,所以直接选择第二种方式进行安装.

* 安装结束后查看
  `echo $PATH`


#### 编写脚本

* 脚本

  * fir的token从官网获取
    ![](/assets/Snip20170328_11.png)


* ```obje
  #!/bin/bash
  #计时SECONDS=0
  #假设脚本放置在与项目相同的路径下
  project_path=$(pwd)
  #取当前时间字符串添加到文件结尾
  now=$(date +"%Y_%m_%d_%H_%M_%S")
  #指定项目的scheme名称
  scheme="JiaeD2C"
  #指定要打包的配置名
  configuration="Debug"
  #指定打包所使用的输出方式，目前支持app-store, package, ad-hoc, enterprise, development, 和developer-id，即xcodebuild的method参数
  export_method='ad-hoc'
  #指定项目地址
  workspace_path="/Users/fanzhou/Desktop/JIAE3/JiaeD2C/JiaeD2C.xcworkspace"
  #指定输出路径
  output_path="/Users/fanzhou/Desktop/JiaeD2Cbulid/"
  #指定输出归档文件地址
  archive_path="$output_path/Demo_${now}.xcarchive"
  #指定输出ipa地址
  ipa_path="/Users/fanzhou/Desktop/JiaeD2Cbulid/JiaeD2C.ipa"
  #指定输出ipa名称
  ipa_name="JiaeD2C.ipa"
  #获取执行命令时的commit 
  messagecommit_msg="
                     1.测试自动打包并上传到fir 
                     2.项目使用cocoapod管理三方库
                     "
  #输出设定的变量值
  echo "===workspace path: ${workspace_path}==="
  echo "===archive path: ${archive_path}==="
  echo "===ipa path: ${ipa_path}==="
  echo "===export method: ${export_method}==="
  echo "===commit msg: $1==="
  #先清空前一次build
  fastlane gym --workspace ${workspace_path} --scheme ${scheme} --clean --configuration ${configuration} --archive_path ${archive_path} --export_method ${export_method} --output_directory ${output_path} --output_name ${ipa_name}
  #上传到firfir 
  publish ${ipa_path} -T 398942b933e10f315789459cb62e0e7d -c "${commit_msg}"
  #输出总用时echo "===Finished. Total time: ${SECONDS}s==="
  ```

* 将编写好的脚本放到工程目录下

  ![](/assets/Snip20170328_9.png)

* 终端执行,总用时203秒
  `sh /Users/fanzhou/Desktop/JIAE3/JiaeD2C/build_using_gym.sh`

  ![](/assets/Snip20170328_10.png)

  ![](/assets/Snip20170328_12.png)



### 注意

* 打测试包和AppStore时需要将参数修改下

  ![](/assets/Snip20170328_13.png)

### 参考文档

#### 安装fastlane

* [gitHub](https://github.com/fastlane/fastlane)

* [iOS-Fastlane\(安装篇\)](http://www.jianshu.com/p/abc2063f0981)


#### 安装fir

* [gitHub](https://github.com/FIRHQ/fir-cli/blob/master/doc/install.md)

* [使用 fir CLI插件上传 Swift 应用至fir.im](http://www.jianshu.com/p/f17caec72f0b)


#### 编写脚本

* [使用fastlane gym\/xctool编写ipa打包脚本](http://www.jianshu.com/p/54ab07f2e63b)

