### iOS 使用fastlane 自动打包步骤



！[参考](http://www.cocoachina.com/ios/20190109/26077.html)



#### 1 查看ruby版本信息 本机是否安装ruby

> ruby -v 

#### 2 安装xcode命令行工具 点击同意即可

>  xcode-select --install 



#### 3 安装fastlane

键入如下命令

> ```shell
> sudo gem install fastlane -NV
> ```



####  4 使用

* 1 打开终端  cd 进入到要打包的项目下
* 2 执行fastlane init 命令

会进行一个友情的提示，what would you like to use fastlane for ?

1 自动截屏操作，帮我们自动截取App中的截图

2 自动发布beta版本用于TestFlight

3 自动发布到AppStore

4 手动设置



* 3 手动输入4 然后一路敲击回车键就可以了

![](https://github.com/AlexanderYeah/SK_AutoPackage/blob/master/fastlane.png)



#### 5 必要的配置

项目的fastlane 文件夹面有两个文件  Appfile 和 Fastfile

* 1   Appfile文件描述

 APP_IDENTIFIER 为bundleID

APPLE_ID 就是AppleID

````swift
# app_identifier("[[APP_IDENTIFIER]]") # The bundle identifier of your app
# apple_id("[[APPLE_ID]]") # Your Apple email address


# For more information about the Appfile, see:
#     https://docs.fastlane.tools/advanced/#appfile

````



* 2 Fastfile 文件描述

其实这个文件就是要编辑脚本的文件

custom_lane 相当于方法名  就是执行打包的指令名，编写指令完成 进入到项目下面

> fastlane  custom_lane



```python
# This file contains the fastlane.tools configuration
# You can find the documentation at https://docs.fastlane.tools
#
# For a list of all available actions, check out
#
#     https://docs.fastlane.tools/actions
#
# For a list of all available plugins, check out
#
#     https://docs.fastlane.tools/plugins/available-plugins
#

# Uncomment the line if you want fastlane to automatically update itself
# update_fastlane

default_platform(:ios)

platform :ios do
  desc "Description of what the lane does"
  lane :custom_lane do
  
  	time = Time.new.strftime("%Y%m%d") #获取时间格式
    version = get_version_number#获取版本号
    ipaName = "Release_#{version}_#{time}.ipa"# 输出后的ipa的名字
    
  	gym(
       scheme:"YourAppName", #项目名称
       export_method:"ad-hoc",#打包的类型
       configuration:"Release",#模式，默认Release，还有Debug
       output_name:"#{ipaName}",#输出的包名
       output_directory:"/desktop"#输出的位置
	
     )
    api_key = "aaaaaaa"
 	user_key = "bbbbbb"
 	pgyer(api_key:"#{api_key}",user_key:"#{user_key}")
    
    # add actions here: https://docs.fastlane.tools/actions
  end
end

```



#### 6 上传到蒲公英上面

* 1 cd 到项目下面 安装蒲公英的插件 安装成功会进行提示 Successfully installed plugins

  > fastlane add_plugin pgyer

* 2 在Fastfile 文件中重新编辑。添加你的api_key 和 user_key，这两个key 登录自己的蒲公英官网账户，点击账户设置可以找到相关信息


* 3 cd 到项目下面 执行命令 打包上传成功

       api_key = "aaaaaaa"
       user_key = "bbbbbb"
       pgyer(api_key:"#{api_key}",user_key:"#{user_key}")

![](https://github.com/AlexanderYeah/SK_AutoPackage/blob/master/success.png)  

![](https://github.com/AlexanderYeah/SK_AutoPackage/blob/master/pgyfloder.png)
