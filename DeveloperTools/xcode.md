Xcode是一个IDE（Integrated Development Environment，集成开发环境)

**编译器**：将人类能够理解的源代码转换为随处理器而异的可执行代码，以便在计算机中执行。
**调试器**： 在程序不能正确运行时帮助你找出代码中的问题。
**编辑器**： 让你能够编写源代码。
**项目管理器**： 帮助你更加有序的组织软件项目的文件。
**测试子系统**：帮助测试，而测试在任何情况下都是软件开发的重要方面。
**剖析和分析工具**：帮助你研究代码的运行情况，确定其性能如何、资源使用情况如何等。

### Developer Documentation 

Xcode下载的文档安装在

```
~/Library/Developer/Shared/Documentation/DocSets

```

####在Xcode中添加HeaderDoc,并且通过headerdoc2html生成Apple-Style的HTML文档 

在安装Xcode的时候，已经安装了`headerdoc2html`工具，所以只要在终端输入以下命令，就可以把指定的文件夹下的所有类中的HeaderDoc注释转换成html帮助文档：

```
headerdoc2html -o ~/Desktop/AppleStyleDoc 10-XcodeDocCreator/ 

```

`~/Desktop/AppleStyleDoc`表示生成的html文档存放的位置，代表把哪个文件夹下的文件的HeaderDoc转换成HTML帮助文档。

HeaderDoc的格式如下：

```
/*!

 @discussion 方法的详细描述
 @abstract 方法概述

 @param obj 参数说明

 @param something dkasjd

 @return <#方法返回值说明#>

 */

-(void)someone:(NSObject *)obj do:(NSString *)something;
```