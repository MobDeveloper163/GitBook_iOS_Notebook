### Audio Session默认行为

* 默认允许播放，不允许录制；
* 当用户讲响铃/静音开关移动到静音的位置时，你的音频也会静音。
* 当你的音频开始播放时，设备上的其他音频，比如正在播放的音乐应用的音频就会被静音


当你开始播放和录制音频的时候Audio Session自动开始；然而对于应用来说。然而依赖默认的状态，对应用来说有一定的风险。比如，如果iPhone响铃，用户忽略了来电，使你的应用继续运行，基于你所使用的播放技术，你的音频可能不再播放。


只有在以下情况，一个shipping app才可以安全地忽略Audio Session:
* 当你在使用`System Sound Service` 或者 UIKit 的 `playInputClick`方法而不使用其他Audio API处理音频的时候，
* 你的应用不适用音频

其他的任何情况，不要使用默认的Audio Session ship一个应用。



### 为什么默认的Audio Session通常不是你需要的？

如果不初始化、配置和明确的使用Audio Session,应用就不会响应中断或者音频硬件设备的变化。此外，你的应用不能决定操作系统在应用间如何混合音频。

### 初始化Audio Session
在应用启动时，系统会提供一个Audio Session对象。然而，你必须初始化Audio Session来处理中断。
```
// 隐式初始化Audio Session
AVAudioSession *session = [AVAudioSession sharedInstance];

```
现在session代表已经初始化了的Audio Session,可以立即使用。当你使用AVAudioSession类的中断通或者使用AVAudioPlayer和AVAudioRecorder的代理协议知处理Audio中断时，Apple建议隐式初始化。

### 添加音量和连接控制
使用MPVolumeView类为你的应用展示声音和Route Control。


### 响应Route Controller事件
Route Control事件让用户控制多媒体。iOS通过UIEvent将时间传递给应用，应用将事件发送给第一响应者，如果第一响应者不处理，事件沿着响应链走。

你的应用必须是当前正在播放的应用，并且在响应事件之前，正在播放音频。

### 激活和取消激活Audio Session
在程序启动时，系统自动激活Audio Session。尽管如此，Apple建议显式的激活Session,通常作为viewDidLoad方法的一部分，并且在激活Audio Session前设置优先选取的硬件值。这里你有机会测试是否成功激活。当更改Audio Session激活状态时，检查确保成功调用。


在有闹钟、日历提醒或者来电时，系统取消激活应用的Audio Session。当用户解除闹铃或者忽略来电时，允许应用重新激活。是否重新在中断后激活取决于应用类型。应用类型参照
`Audio Guidelines By App Type`。


使用AVFoundation激活Audio Session

```
NSError *activationError = nil;

BOOL success = [[AVAudioSession sharedInstance] setActive: YES error: &activationError];

if (!success) { /* handle the error in activationError */ }

```

Apple建议注册通知消息，显式的激活和取消激活Audio Session。

大多数应用并不需要显式的取消激活Audio Session。重要的例外有VoIP（网络协议声音）应用，turn-by-turn导航应用，以及某些情况下的录制应用。

* 确保通常运行在后台的VoIP应用，只在通话的时候激活。当等待呼叫的时候，不应该激活。
* 确保使用录制类型的应用只在录制的时候激活Audio Session。在录制前后录制结束后，保证session未激活，允许播放其他的声音，比如收到信息的通知。

### 检测应用启动时，其他音频正在播放

### Working with Inter-App Audio

> In its most basic form, inter-app audio allows one app, called the node app, to send its audio output to another app, called the host app. However, it is also possible for the host app to send its output to the node app, have the node app process the audio, and send the result back to the host app. The host app needs to have an active audio session; however, the node app only needs an active audio session if it is receiving input from the host app or the system. Use the following guidelines when setting up inter-app audio:

>Set the “inter-app-audio” entitlement for both the host and node app.

>Set the UIBackgroundModes audio flag for the host app.
Set the UIBackgroundModes audio flag for node apps that use audio input or output routes while simultaneously connected to an inter-app audio host.

>Set the AVAudioSessionCategoryOptionMixWithOthers for both the host and node app.

>Make sure the node app’s audio session is active if it receives audio input from the system (or sends audio output) while simultaneously connected to an inter-app host.




### Working with Categories

##### 录制权限：
在iOS中开始，应用在录制音频前，必须请求和接收用户的许可。如果用户没有给你录制的权限，就录不到声音。当你使用用的Category支持录制以及应用尝试使用input route时，系统自动地通知用户请求权限。你也可以使用requestRecordingPermission:方法向用户请求许可，替代等待系统请求。体验更好。










