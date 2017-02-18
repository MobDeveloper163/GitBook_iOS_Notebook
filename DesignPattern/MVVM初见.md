 #MVVM初见
 
 _译自 _：[Introduction to MVVM](https://www.objc.io/issues/13-architecture/mvvm/)_（注：没有完全翻译全文，只是翻译了和MVVM真正相关的部分)_

---

## 什么是MVVM?

![典型的MVC架构.png](http://upload-images.jianshu.io/upload_images/3658743-74e1db033f92f768.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上图演示了典型的MVC架构。Model展示数据，view展示用户界面，ViewController调解两者之间的交互。

思考片刻，尽管view和viewController严格来讲是不同的组件，但是他们总是配对，手牵手的在一起。那么为什么不正式确立两者的关系？

![MVVM架构.png](http://upload-images.jianshu.io/upload_images/3658743-1be6f7235c5af063.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如此一来能更精确地描述你可能已经写过的MVC代码。但是这并不能处理iOS中不断增长的臃肿的视图控制器。通常MVC应用，很多逻辑都放在视图控制器中。其中的一些属于视图控制器，但是其中很多叫做‘展示逻辑（presentation logic）’。在MVVM术语中，类似把模型值转换为视图可展示的东西的逻辑和把NSDate转换成字符串的逻辑都视为“展示逻辑”。

在我们的示意图中少了一些东西。可以放置所有展示逻辑的东西。我们把它叫做‘View Model’ - 它将处于View/Controller和Model之间。

![MVVM_Diagram.png](http://upload-images.jianshu.io/upload_images/3658743-f38d0ce308624fa5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

看起来好多了！这个图表准确的描述了什么是MVVM:一个增强版本的MVC，我们正式连接了视图和控制器，并将展示逻辑从控制器转移到新的对象——View Model对象中。MVVM听起来很复杂，但它本质上是一个你已经熟悉的MVC架构的装饰版本。

目前我们知道了什么是MVVM,为什么要用它呢？总之，对我而言，MVVM在iOS上动机是减少视图控制器的复杂性，并且使展示逻辑更容易测试。我们用一些实例来看看它是怎样完成这些目标的。

我希望你能从这篇文章中学习到非常重要的三点：
*  MVVM兼容你现有的MVC架构
*  MVVM使应用更具可测性
* MVVM和绑定机制一起完美工作

如我们前面所见，MVVM基本上是MVC的装饰版本。因此很容易理解它是怎样和现有的使用MVC架构的应用合并在一起的。一起创建一个Person模型以及相关的视图控制器。

```objective-c
@interface Person : NSObject

 - (instancetype)initwithSalutation:(NSString *)salutation firstName:(NSString *)firstName lastName:(NSString *)lastName birthdate:(NSDate *)birthdate;

	@property (nonatomic, readonly) NSString *salutation; 
	@property (nonatomic, readonly) NSString *firstName; 
	@property (nonatomic, readonly) NSString *lastName;
	@property (nonatomic, readonly) NSDate *birthdate; 

@end

```
现在假设我们有了一个 `PersonViewController` ,在 `viewDidLoad `中，只需要根据模型属性创建一些labels：
	
```
- (void)viewDidLoad { 
	[super viewDidLoad]; 
	if (self.model.salutation.length > 0) { 
		self.nameLabel.text = [NSString stringWithFormat:@"%@ %@ %@", self.model.salutation, self.model.firstName, self.model.lastName]; 
	} else { 
		self.nameLabel.text = [NSString stringWithFormat:@"%@ %@", self.model.firstName, self.model.lastName]; 
	} 
	NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init]; 
	[dateFormatter setDateFormat:@"EEEE MMMM d, yyyy"]; 
	self.birthdateLabel.text = [dateFormatter stringFromDate:model.birthdate]; 
}

```

通常的MVC的实现相当直接。现在来看怎样通过Model View添加这些实现。

```
@interface PersonViewModel : NSObject 
- (instancetype)initWithPerson:(Person *)person; 

	 @property (nonatomic, readonly) Person *person;
	 @property (nonatomic, readonly) NSString *nameText; 
	 @property (nonatomic, readonly) NSString *birthdateText; 

@end

```

View Model的实现如下：

```
@implementation PersonViewModel 

- (instancetype)initWithPerson:(Person *)person { 
	self = [super init]; 
	if (!self) 
		return nil；
	_person = person; 
	if (person.salutation.length > 0) { 
		_nameText = [NSString stringWithFormat:@"%@ %@ %@", self.person.salutation, self.person.firstName, self.person.lastName]; 
	} else { 
		_nameText = [NSString stringWithFormat:@"%@ %@", self.person.firstName, self.person.lastName]; 
	}
	 NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init]; [dateFormatter setDateFormat:@"EEEE MMMM d, yyyy"]; 
	_birthdateText = [dateFormatter stringFromDate:person.birthdate]; 
	return self; 
}

@end

```
我们已经把 viewDidLoad 中展示逻辑移到了View Model中了。现在我们的viewDidLoad方法是非常轻量的：

```
- (void)viewDidLoad { 
	[super viewDidLoad]; 
	self.nameLabel.text = self.viewModel.nameText; 
	self.birthdateLabel.text = self.viewModel.birthdateText; 
}
```

正如你所见，从MVC架构到MVVM并没有太大的改变。相同的代码，只是移来移去。它和MVC兼容，实现[更轻量的视图控制器](https://www.objc.io/issues/1-view-controllers/)，并更具可测性。

啊！可测性?什么鬼？中所周知视图控制器因其做了太多事情而很难测试。在MVVM中我们尽可能多的将代码移动到view models中。测试视图控制变得更加简单，因为他们不再做所有的事情，view models很容易测试。一起瞧一瞧：

```
SpecBegin(Person) 
	NSString *salutation = @"Dr."; 
	NSString *firstName = @"first"; 
	NSString *lastName = @"last"; 
	NSDate *birthdate = [NSDate dateWithTimeIntervalSince1970:0]; 
	
	it (@"should use the salutation available. ", ^{ 
		Person *person = [[Person alloc] initWithSalutation:salutation firstName:firstName lastName:lastName birthdate:birthdate]; 
		PersonViewModel *viewModel = [[PersonViewModel alloc] initWithPerson:person]; expect(viewModel.nameText).to.equal(@"Dr. first last"); 
	});
	
	 it (@"should not use an unavailable salutation. ", ^{ 
		Person *person = [[Person alloc] initWithSalutation:nil firstName:firstName lastName:lastName birthdate:birthdate]; 
		PersonViewModel *viewModel = [[PersonViewModel alloc] initWithPerson:person]; expect(viewModel.nameText).to.equal(@"first last"); 
	}); 
	
	it (@"should use the correct date format. ", ^{ 
		Person *person = [[Person alloc] initWithSalutation:nil firstName:firstName lastName:lastName birthdate:birthdate]; 
		PersonViewModel *viewModel = [[PersonViewModel alloc] initWithPerson:person]; expect(viewModel.birthdateText).to.equal(@"Thursday January 1, 1970"); 
	}); 
	
SpecEnd

```

 如果我们没有把这些逻辑移到View Model中，比较labels中的值，我们不等不创建完整视图控制器和视图。现在我们可以随意的更改我们的视图层级而不用担心破坏我们的单元测试。使用MVVM的测试益处显而易见，即便是从这个简单的例子来看，并且随着展示逻辑越来越复杂，其好处就越明显。


注意在这个简单的示例中，模型是不可变的，所以我们能够在初始化的时候分配View Models的属性。对于可变模型，我们需要使用绑定机制以便View Model在model返回这些属性变化时更新它的属性。而且，一旦View Model上的model改变，视图的属性也要跟着改变。来自model的变化要通过View Model传递到View中。

在OS X上，可以使用Cocoa绑定，但是我们在iOS上没有这个功能。 考虑使用KVO，它做得很好。 但是，对于简单的绑定，它有很多样板，特别是如果有很多属性要绑定。 相反，我喜欢使用ReactiveCocoa，但使用MVVM没有强制要使用ReactiveCocoa。 MVVM是一个很棒的范例，它代表自己，只是跟一个很好的绑定框架一起会更好。

我们已经涵盖很多了：从MVC衍生的MVVM，两者如何兼容的示例，从可测试性查看MVVM,看到了MVVM和绑定机制一起能够很好的工作。假如你有兴趣学习更多关于MVVM的知识，你可以查看[这篇博客](http://www.teehanlax.com/blog/model-view-viewmodel-for-ios/)，非常详细的解释了MVVP的优点，或者[这篇文章](http://www.teehanlax.com/blog/krush-ios-architecture/)，关于我们怎样在现有的工程中使用MVVM取得了很大的成功。我同时有完整的测试，基于MVVM叫做[C-41](https://github.com/AshFurrow/C-41)的开放源。检出它，如果有问题[请让我知道](https://twitter.com/ashfurrow)。

> 相关推荐：[MVC vs MVVM](http://www.jianshu.com/p/d9d39e4798ce