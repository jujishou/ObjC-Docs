##Objective-C的类库管理 - CocoaPods

在开发iOS APP或者Mac应用时，我们使用Apple本身提供的开发Framework库的API外，往往会发现很多功能不是很容易开发，这时候不可避免的会引用第三方OpenSource或公司其他团队已开发的成熟类库来辅助开发。而使用第三方OpenSource或者公司其他团队的类库我们经常会碰到版本演进、依赖、License规范等问题。当我们确定要引用某个OpenSource或者其他团队的类库时，我们之前有两种方式：

	1. 下载。下载源码，直接拷贝源码放到自己的工程中，然后配置依赖的framework、ARCorMRC等。这种方式缺点很多，第一、脱离了原来的版本控制，未来要升级版本很不方便，需要重复之前的下载拷贝行为。第二、因为直接拷贝源码，必然会导致项目代码会变大，增加维护成本。
	
	2. 使用Git Submodule进行管理。Git有个功能是Submodule，它可以让你的项目和其他使用Git的类库之间建立关联。通过Git可以随时Fetch、更新类库。这种方式，我们同样还是要管理它的依赖问题。
	
一种语言发展到一个阶段，必定会有相应的依赖管理工具, 或者是中央代码仓库。比如Java的maven，Ruby的gems，Python的pip、easy_install，Nodejs的npm等等。同样，Objective-C也不例外，今天我们要分享的CocoaPods工具，就是我们Objective-C程序的依赖管理工具。

##CocoaPods简介

CocoaPods是一个管理Objective-C项目中各种类库的工具。CocoaPods项目的源码在GitHub上管理。CocoaPods工具的出现使得我们可以节省大量配置和更新类库的时间。我们可以把手动管理依赖包更新、配置类库依赖、配置编译参数、下载拷贝源码等毫无技术含量的事情交给Cocoapods来完成。我们只需要配置好一个Podfile文件，告诉CocoaPods我们需要哪些类库即可，CocoaPods会自动将类库源码及一些依赖都下载下来，并为我们的工程配置好相应的系统依赖和编译参数。

##CocoaPods的安装和使介绍用

###安装
CocoaPods是一个Ruby的Gem，而且Mac下都自带Ruby，所以CocoaPods的安装很简单，输入以下gem命令即可:
	
	$ gem install cocoapods
	$ pod setup
	

###使用

安装完以后我们可以使用CocoaPods的 `search` 命令来搜索第三方OpenSource类库。例如输入:

	$ pod search nimbus

会得到以下结果:
	
	-> Nimbus (1.0.0)
   		An iOS framework whose growth is bounded by O(documentation).
   		- Homepage: http://docs.nimbuskit.info/index.html
   		- Source:   https://github.com/jverkoey/nimbus.git
   		- Versions: 1.0.0, 0.9.3, 0.9.2, 0.9.1, 0.9.0 [master repo]
   		- Sub specs:
     		- Nimbus/Core (1.0.0)
     		- Nimbus/Badge (1.0.0)
     		- Nimbus/CSS (1.0.0)
     		- Nimbus/AttributedLabel (1.0.0)
     		- Nimbus/Interapp (1.0.0)
     		- Nimbus/Launcher (1.0.0)
     		- Nimbus/Models (1.0.0)
     		- Nimbus/NetworkControllers (1.0.0)
     		- Nimbus/NetworkImage (1.0.0)
     		- Nimbus/Overview (1.0.0)
     		- Nimbus/PagingScrollView (1.0.0)
     		- Nimbus/Photos (1.0.0)
     		- Nimbus/Operations (1.0.0)
     		- Nimbus/Operations/JSON (1.0.0)
     		- Nimbus/WebController (1.0.0)

接下来我们在项目的根目录新建一个名为Podfile的文件，内容如下:

	platform :ios, '5.0'
	pod 'JSONKit','~> 1.4' #会安装 >= 1.4到1.x, 也就是 < 2.0以下的
	pod 'BlocksKit','~>1.8' #同上理
	pod 'Reachability',  '~> 3.0.0' #同上理 
	
配置文件指定了iOS平台，并且是需要Deployment Target为5.0，需要JSONKit、BlocksKit、Reachability类库以及各自指定的版本号。

编辑好之后，我们使用以下的CocoaPods命令:

	$ pod install
	
执行成功后，我们所有指定的类库都已下载完成并设置好了依赖和编译参数。然后使用CocoaPods生成.xcworkspace文件来打开工程。

	$ open xxx.xcworkspace

之后要记得都用这种方式来打开项目来开发。如果你修改了Podfile文件，你需要重新执行下install命令。

##原理

CocoaPods会生成一个xcworkspace项目文件，在workspace里面包含两个项目，一个是我们的主项目，一个是pods项目。CocoaPods将所有的依赖库都放到另一个名为Pods项目中，然后让主项目依赖Pods项目，源码管理工作都从主项目移到了Pods项目中。Pods项目最终会编译成一个名为libPods.a的文件，主项目只需要依赖这个.a文件即可。对于资源文件，CocoaPods提供了一个名为Pods-resources.sh脚本，该脚本在每次项目编译的时候都会执行，将第三方库的各种资源文件复制到目标目录中。CocoaPod还通过一个名为Pods.xcconfig的文件来在编译时设置所有的依赖和参数。


