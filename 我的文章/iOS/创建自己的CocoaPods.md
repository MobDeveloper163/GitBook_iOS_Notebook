### CocoaPods创建自己的Pod

#### 本地创建自己的Pod

1. 在GitHub上为自己的Pod创建一个新的仓库：[https://github.com/MobDeveloper163/MyCocoaPods.git](https://github.com/MobDeveloper163/MyCocoaPods.git)
	 

2. 打开终端cd到桌面，执行`pod lib create MyCocoaPods`命令，终端会提示
   >Cloning `https://github.com/CocoaPods/pod-template.git` into `pod`.
  
3. 等待克隆完毕后，回答一些问题，比如`What language do you want to use?? [ Swift / ObjC ]`是问你想用什么语言。我这里写ObjC,回车。

4. 回到问题后，在桌面的MyCocoaPods文件夹下克隆了一下文件：
   ![](/assets/lib created directories.png)。

5. 其中Example文件夹下是自动创建的实例项目，此时`pod lib create MyCocoaPods`可能会自动集成自己的MyCocoaPods类库到Example中，那么，第3步中回答完问题后，会自动打开Example的MyCocoaPods.xcworkspace。也可能不会自动集成而会提示找不到`MyCocoaPods.xcworkspace`文件，但是不用担心，自己cd到Example文件夹，并在终端执行`pod install`命令，手动生成并打开。

6. 此时我们就可以把自己的类库放在`MyCocoaPods/Classes/`文件夹中；

7. 重新`cd` 到 `Example`文件夹，执行`pod update`命令，让示例项目重新加载类库。此时在例项目中就可以使用自己的类库了。

##### 使用GitHub和CocoaPods管理自己的类库

   此时创建的MyCocoaPods类库只能在本地使用，接下来，我要做的事情是让自己的类库  能想AFNetwroking一样，只要在podfile中配置`pod MyCocoaPods`,任何人都能使用MyCocoaPods。


1. 配置MyCocoaPods文件夹下的MyCocoaPods.spec。

2.










