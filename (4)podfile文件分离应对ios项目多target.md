
#Podfile 文件分离

##分离Podfile文件

###背景：

<li>在制作客户提供的库XXX的时候，我们发现里面有个JDStatusBarNotification, 然后在另一个客户提供的sdk中也有个JDStatusBarNotification, 并且两者的版本不一，且前者提供的，在该库的源代码上进行了一些修改。如果两者都用pod，那么就会有 冲突存在， 但pod方便开发，减少构建的复杂性。
</li>

那么接下来对前者的库做一些处理

添加不支持支持 arc

asi

tbxml

keychainItemWrapper

zip

simple.m (simple helper类不用)

openuuid

<pre>
pod 可以很容易解决这些问题。

* 在构建的时候，我们仅仅需要该`Target`下的pod库的下载，但是我们如果是独立一个`podfile`文件，它会把全部的`target`的库下载下来，然后根据target来进行刷选编译。

* 如果不同的Target有相同的库，但依赖的版本不一样，我们该怎么办？类似的客户提供的库是在第三方源码修改使用，我们该怎么抉择[我们现在采用的是把客户的删除，保留项目中的，不如zip, 比如]。
</pre>

删除客户sdk中使用的第三方：

mjrefresh

reachability

mbprogress

zip换名 删除

gtmbase64

保留我们项目中使用的

###不同target使用不同的podfile如何实现
<pre>
## 思索：

* 思路1： 

  `podfile`是ruby, 我们在podfile文件内部可不可以获取相关环境来进行控制，比如`Target`.

* 思路2： 

 每个项目单独一个`podfile`文件.

## 过程和结果：

* 思路1： 

 初步了解ruby,在经过一些探研下，如果要实现需要扩展`Pod`相关的方法，可能需要较多的成本和时间投入，结果暂时未实现

* 思路2：

 通过shell脚本根据选项来自动生成`Podfile`文件。结果实现。

## 思路2相关的要点(转移维护点)：

 我们在原来`Podfile`等级目录下，创建了个`podfiles`文件夹.其目录结构如下：
</pre>

<pre>
podfiles

├── podfile-DaTongZhengQuan_Phone

├── podfile-GuoYuan_Phone

├── podfile-HuaLong_Phone

├── podfile-HuaXi_Phone

├── podfile-KDS_Phone

├── podfile-ShiJiZhengQuan_Phone

├── podfile-WanHe_Phone

├── podfile-XiZhangTongXin_Phone

├── podfile-ZhongXinJianTou_Phone

└── podfile-common
</pre>



<pre><code>
**podfile-common** 里面的内容相当于之前`Podfile`的公共`pod`, 内容如下：

```ruby

# Podfile文件说明：

# 0、Podfile 文件只能由master修改，其它同事有需要添加新的库时，请提PR;

# 1、添加的每一个第三方库之前，请在 ReadMe_Venders.md 文件中，添加说明！！# 2、新增第三方库源码文件不要往项目源码中提交commit ！

# 3、为保证第三方库的稳定性，每个第三方库必须注明依赖的版本号！！

# 4、请升级你的 cocoaPods 版本到1.0；

# 5、添加第三方依赖时，注意区分 target添加！

source 'https://github.com/CocoaPods/Specs.git'source 'https://gitlab.inin88.com/xxx/specs.git'

# 以下是所有 target 通用的 pod

platform :ios, '7.0' pod 'IQKeyboardManager', '~> 3.3.7' pod 'MJRefresh', '~> 3.1.0' pod 'MBProgressHUD', '~> 0.9.2' pod 'FMDB', '~> 2.6' pod 'SocketRocket', '~> 0.5.1'

 ### AOP pod 'Aspects', '~> 1.4.1'

 pod 'YTKeyBoardView', :git => 'git@gitlab.inin88.com:xxx/YTKeyBoardView.git', :branch => 'develop'

# TODO: Protobuf 3.0.0 最低支持到 7.1platform :ios, '7.1' pod 'Protobuf', '3.0.0-beta-3'
</code></pre>

而其他的文件名以"podfile-" + Target名字构成.内容是原podfile文件的个个Target所对应的内容。

make-podfile.sh 这个文件是负责自动生成podfile文件的，该脚本是给客户端使用的。

auto-make-podfile.sh这个脚本是给自动化打包使用的。在feature/ZXJTA-podfile分支下的构建过程，有这么段输出：
<pre>
============= 3. 执行 iOS 打包脚本 ========================== 打包 STEP1.更新第三方pod依赖 =============--------copy 文件到podfile文件中--------

-----------自动生成的 Podfile文件 --------------:

 # Podfile文件说明：

 # 0、Podfile 文件只能由master修改，其它同事有需要添加新的库时，请提PR;

 # 1、添加的每一个第三方库之前，请在 ReadMe_Venders.md 文件中，添加说明！！

 # 2、新增第三方库源码文件不要往项目源码中提交commit ！

 # 3、为保证第三方库的稳定性，每个第三方库必须注明依赖的版本号！！

 # 4、请升级你的 cocoaPods 版本到1.0；

 # 5、添加第三方依赖时，注意区分 target添加！

source 'https://github.com/CocoaPods/Specs.git'source 'https://gitlab.inin88.com/xxx/specs.git'

 # 以下是所有 target 通用的 pod

platform :ios, '7.0' pod 'IQKeyboardManager', '~> 3.3.7' pod 'MJRefresh', '~> 3.1.0' pod 'MBProgressHUD', '~> 0.9.2' pod 'FMDB', '~> 2.6' pod 'SocketRocket', '~> 0.5.1'

 ### AOP pod 'Aspects', '~> 1.4.1'

 pod 'YTKeyBoardView', :git => 'git@gitlab.inin88.com:xxx/YTKeyBoardView.git', :branch => 'develop'

 # TODO: Protobuf 3.0.0 最低支持到 7.1platform :ios, '7.1' pod 'Protobuf', '3.0.0-beta-3'target 'ZhongXinJianTou_Phone' do pod 'MJExtension', '~> 3.0.13' pod 'SDWebImage', '~> 3.8.1' pod 'DZNEmptyDataSet', '~> 1.8.1' pod 'KMNavigationBarTransition', '~> 0.0.10' pod 'WebViewJavascriptBridge', '~> 5.0.5' pod 'KRVideoPlayer', '~> 1.0.1' pod 'WMPageController', '~> 1.7.0' pod 'JDStatusBarNotification', '~> 1.5.3' pod 'Masonry', '~> 1.0.1'

 # 以下是私有库 pod 'YTNetworking', :git => 'git@gitlab.inin88.com:xxx/YTNetworking.git', :branch => 'develop' pod 'YT_BonreeAgent', :git => 'git@gitlab.inin88.com:xxx/YT_BonreeAgent.git', :branch => 'master', :tag => '0.1.0' pod 'ZXJTOpenAccount', :git => 'git@gitlab.inin88.com:xxx/ZXJTOpenAccount.git', :branch => 'develop', :tag => '0.1.2'

end-----------自动生成的 Podfile文件完成------------ Preparing

Analyzing dependencies

Inspecting targets to integrate
</pre>

构建结果是成功的

###make-podfile.sh 的使用：

过去：候我们维护Podfile，一般我们直接pod install or pod update就可以把项目中的pod库拉下来了。在每一次修改Podfile文件，我们也是这样执行。

现在：我们维护的是文件夹podfiles下的对于文件， 一般我们的操作：

<pre><code>sh make-file.sh # 是为了生成最新的podfile文件pod install | pod update
</code></pre>
要添加库比如在podfile-xxxApp,

<pre><code>
pod 'MJExtension', '~> 3.0.13' 

pod 'SDWebImage', '~> 3.8.1' 

pod 'DZNEmptyDataSet', '~> 1.8.1' 

pod 'KMNavigationBarTransition', '~> 0.0.10' 

pod 'WebViewJavascriptBridge', '~> 5.0.5' 

pod 'KRVideoPlayer', '~> 1.0.1' 

pod 'WMPageController', '~> 1.7.0' 

pod 'JDStatusBarNotification', '~> 1.5.3' 

pod 'Masonry', '~> 1.0.1'

 #以下是私有库

pod 'YTKeyBoardView', :git => 'git@gitlab.inin88.com:mobile-stock-ios-libs/YTKeyBoardView.git', :branch => 'develop'
        
pod 'GuoYuanOTC', :git=>'git@gitlab.inin88.com:kun.ding/GuoYuanOTC.git', :tag => '0.4.0'
    ###模拟器
    
pod 'GuoYuanYeWuBanLi', :git=>'git@gitlab.inin88.com:kun.ding/GuoYuanYeWuBanLi.git', :branch => 'iPhoneSimulatorYWBLSDK'
</code></pre>


