### CocoaPods创建自己的Pod

#### 本地创建自己的Pod

1. 在GitHub上为自己的Pod创建一个新的仓库：[https://github.com/MobDeveloper163/MyCocoaPods.git](https://github.com/MobDeveloper163/MyPodTest.git)
	 

2. 打开终端cd到桌面，执行`pod lib create MyPodTest`命令，终端会提示
   >Cloning `https://github.com/CocoaPods/pod-template.git` into `pod`.

   ** 注意：我已经创建和上传了MyPodTest,所以这里不要再用这个名字作为自己的Pod名 **
  
3. 等待克隆完毕后，回答一些问题，比如`What language do you want to use?? [ Swift / ObjC ]`是问你想用什么语言。我这里写ObjC,回车。

4. 回到问题后，在桌面的MyPodTest文件夹下克隆了一下文件：
   ![](/assets/lib created directories.png)。

5. 其中Example文件夹下是自动创建的实例项目，此时`pod lib create MyPodTest`可能会自动集成自己的Pod类库到Example中，那么，第3步中回答完问题后，会自动打开Example的MyCocoaPods.xcworkspace。也可能不会自动集成而会提示找不到`MyPodTest.xcworkspace`文件，但是不用担心，自己cd到Example文件夹，并在终端执行`pod install`命令，手动生成并打开。

6. 此时我们就可以把自己的类库放在`MyPodTest/MyPodTest/Classes/`文件夹中；

7. 重新`cd` 到 `Example`文件夹，执行`pod update`命令，让示例项目重新加载类库。此时在例项目中就可以使用自己的类库了。

##### 使用GitHub和CocoaPods管理自己的类库

   此时创建的MyPodTest类库只能在本地使用，接下来，我要做的事情是让自己的类库  能想AFNetwroking一样，只要在podfile中配置`pod MyPodTest`,任何人都能使用MyPodTest。


1. 配置外面的MyPodTest文件夹下的MyPodTest.spec（summary、homepage、source等）。 
   
   ```
   # pod库名
   s.name = 'MyPodTest' 

   # pod版本
   s.version = '0.1.0'

   # pod概述
   s.summary = 'The most powerful pod in the world.'

   # pod的详细描述，可选，也可以在前面加#号，注释掉
   # s.description = <<-DESC
   # # 在这里写pod的详细描述
   # DESC

   # pod的主页
   s.homepage = 'https://github.com/MobDeveloper163/MyPodTest'

   # 许可证书
   s.license = { :type => 'MIT', :file => 'LICENSE' }

   # 作者 名字和邮箱地址
   s.author = { 'Mob_Developer' => 'mob_developer@163.com' }

   # pod源代码在GitHub的仓库地址，以及pod版本
   s.source = { :git => 'https://github.com/MobDeveloper163/MyPodTest.git', :tag => s.version.to_s }

   # pod支持的最低版本
   s.ios.deployment_target = '8.0'

   # pod类库的源文件位置
   s.source_files = 'MyPodTest/Classes/**/*'
 ```

2. 验证MyPodTest是否有效，打开终端，cd到外面的MyPodTest文件夹，输入
   ```
   pod lib lint MyPodTest.podspec
   ```
   如果终端提示`MyPodTest passed validation.`就代表配置无误，否则，就要根据错误提示，检查修改配置信息。

3. cd到外面的MyPodTest文件夹，执行以下命令，将MyCocoaPods推送到前面在GitHub中为这个pod创建的仓库中。
   ```
   git init 
   git add .
   git commit -a -m "初始化MyPodTest"
   git tag "0.1.0"
   git remote add origin https://github.com/MobDeveloper163/MyPodTest.git

   git push origin master --tags	
   ```
  然后到GitHub检查推送的文件是否存在。

4. 在终端执行
   ```
   pod trunk register mob_developer@163.com "Mob Developer" --verbose # 邮箱 用户名
   ```
  回车，终端提示
  > Please verify the session by clicking the link in the verification email that has been sent to mob_developer@163.com
  
  此时等待注册的邮箱收到CocoaPods发来的确认邮件。然后点击邮件中的验证链接，跳转网页，提示Ace, You're set up.

5. cd到外面的MyPodTest文件夹，执行
   ```
   pod trunk push MyPodTest.podspec --verbose
   ```
   等待上传成功,如果443，就再运行一次。
   
   成功后终端会显示以下信息
	
  ```
  🎉 Congrats
  🚀 MyPodTest (0.1.1) successfully published
  📅 November 16th, 04:52
  🌎 https://cocoapods.org/pods/MyPodTest
  👍 Tell your friends!

  ```
  在浏览器中输入[https://cocoapods.org/pods/MyPodTest](https://cocoapods.org/pods/MyPodTest)就可以查看MyPodTest在CocoaPods的主页了。
  
6. 创建一个新工程
  ```
  pod init # 生成Podfile,在Podfile中配置pod 'MyPodTest'
  pod install # 运行MyPodTest.xcworkspace，检查MyPodTest是否被加入工程的依赖库
  
  ```






