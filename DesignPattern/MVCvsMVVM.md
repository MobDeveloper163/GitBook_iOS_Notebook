# MVC vs MVVM

  文章译自：[Model-View-ViewModel for iOS](http://www.teehanlax.com/blog/model-view-viewmodel-for-ios/)

---

如果你已经做过iOS开发，不管多久，你可能都听说过MVVM或者MVC。它是构建iOS应用的标准方式。然而最近，我越来越厌倦MVC的一些缺点。在这篇文章中，我将讨论什么是MVC,详述它的缺点，告诉你一种新的方式以组织你的应用：Model-View-ModelView。Get out your buzzword bingo cards, because we’re about to have a paradigm shift。（哈哈哈哈哈哈哈哈，我不会翻译，英文扔这儿了，我打赌这句没什么用）。

### Model-View-Controller

模型 - 视图 - 控制器是在其中构建代码的最终范例。 即使苹果也这样认为。 在MVC下，所有对象都被分类为模型，视图或控制器。 模型持有数据，视图向用户呈现交互界面，视图控制器调解模型和视图之间的交互。

![MVC_Diagram.png](http://upload-images.jianshu.io/upload_images/3658743-619cce3d4e3a2afd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在我们的示意图中，view通知controller所有的交互。controller更新model以响应状态的变化。然后model通知（通常通过KVO）任何控制器的视图执行所需要操作的更新。这种调解需要在iOS应用中写大量的应用代码。

模型对象通常非常简单。它们通常是Core Data Managed Objects,或者更喜欢其他的[流行模型层](https://developer.apple.com/library/content/documentation/DataManagement/Devpedia-CoreData/managedObject.html#//apple_ref/doc/uid/TP40010398-CH23-SW1)而不是Core Data。据Apple所言，models包含数据和逻辑以操作数据。实践中，models总是非常小，或好或坏，model逻辑被拉进了controller中。

视图通常是UIKit组件或者用户自定义的UIKit组件的集合。这些是.xib和storyboard中的部分：应用可见、可交互的组件。按钮、标签，你懂的。views绝不该直接引用models，只通过IBAction事件引用控制器。业务逻辑和视图无关，也不能有业务逻辑在里面。


剩下的就是Controllers了。Controller是应用“胶水代码”的所在地：这些代码调节Views和Models之间所有的交互。Controllers负责管理它们拥有的View的视图层级，以及它们相应视图的加载，显示，消失等等。它们还倾向于装载我们保留在模型中的模型逻辑以及保留在视图中的业务逻辑。这就导致了使用MVC是的第一个问题……

### 臃肿的(Massive)ViewController

因为有非常多的代码放在ViewControllers中，它们倾向于变的非常臃肿。也不是没有听过，iOS的ViewControllers扩充到成千上万行代码。你的应用中激增的部分权衡下来：臃肿的ViewControllers维护非常困难，包含大量的属性以至于其状态很难管理，遵从了许多协议把控制器逻辑和协议相应代码混合在了一起。


臃肿的控制器难以测试，不管是人工还是单元测试，因为他们有太多可能发生的状态。将你的代码分解成更小，更小的块通常是一件很好的事情。 想起了[最近的一个故事](http://mikehadlow.blogspot.co.uk/2013/12/are-your-programmers-working-hard-or.html)。

### 网络逻辑缺失

MVC的定义 — 苹果使用的其中之一 — 表示所有的对象分类为Model、View或者Controller。那么网络代码应该放在哪里？那么和API通信的代码放在哪里？

你可能想把它放在models对象中，但是会很棘手，因为应该异步调用，那么如果网络请求放置在拥有它的model之外，会变得复杂。绝对不能把网络代码放在View中，那么就只有……Controllers了。这个是个坏主意，因为这样引发臃肿的控制器问题。

那么，放哪儿呢？不适合在其三个组件中的代码，MVC没有为它们提供位置。

### 槽糕的可测性

MVC的另一个大问题是不鼓励开发者写单元测试代码。因为ViewController把视图操作逻辑和业务逻辑混合在了一起，把这些组件分成单元测试变成了艰巨的任务。许多人忽略的任务有利于...只是不测试任何东西。

### 模糊的“管理”定义

前面我提到过ViewController管理视图层级；ViewController有一个view属性，能够通过IBOutLets访问其所有的子视图。当有很多outlets时不能很好的伸缩，并且在某点上，你可能最好使用子控制器帮助管理所有的子视图。

这点在哪儿？什么时候分解事务更好？验证用户输入的业务逻辑属于Controller还是Model?

这里有多条模糊的界线，没有人完全同意。仿佛不管你把界线画在哪里，View和对应的Controller都紧紧的连接着，反正你可能把他们看做同一个组件。

嘿！现在有了一个想法……

### Model-View-ViewModel

在理想的国度里，MVC也许适用。然而我们活在现实世界中，MVC不适用。目前我们已经详述了通常使用MVC时的分解方式，一起来看看它的替代品：Model-View-ViewModel。

MVVM[来自微软](https://msdn.microsoft.com/en-us/library/hh848246.aspx)，但是别抵触它。MVVM和MVC相当相似。它正式确立了视图和控制器的紧密耦合性质，并引入了一个新的组件。


![MVVM_Diagram2.png](http://upload-images.jianshu.io/upload_images/3658743-79a50a522c247b76.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

MVVM中，View和Controller正式连接在一起；我们认为它们是相同的。View和Controller都不引用Model。它们引用ViewModel。

ViewModel是个很棒的位置以存放用户输入的验证逻辑，View的展示逻辑，启动网络请求，以及其它各式各样的代码。对视图本身的任何引用都不属于ModelView.ViewModel中的逻辑应该在iOS和OS X中都适用。(话句话说，在ViewModel不 #import UIKit也是可行的)。

因为展示逻辑 - 比如映射model值到格式化的字符串 - 属于ViewModel，ViewController自身变得不那么臃肿。最棒的部分是在开始适用MVVC时，可以在ViewModel中只放置较少的逻辑，随着更加适应这种范式（架构），将更多的逻辑迁移到ModelView中。

以我的经验，适用MVVM的结果是在代码的总数量上添加了很少的代码，但是大量的减少了代码的复杂性。很值的交易！

如果你再看看MVVM图示，我用的是模糊动词“Update”和“Notify”,但是并没有指出怎样做。你可以适用KVO,就像MVC中一样，但是很快会变的不可管理。实践中,适用ReactiveCocoa是一种很棒的方式，把移动部分粘在一起。

查看更多关于怎样结合MVVM和ReactiveCocoa,阅读Colin Wheeler的[优秀文章](http://cocoasamurai.blogspot.ca/2013/03/basic-mvvm-with-reactivecocoa.html)，或者检出我写的开源应用。你也可以读我的[关于ReactiveCocoa和MVVM的书](https://leanpub.com/iosfrp)。



### The End