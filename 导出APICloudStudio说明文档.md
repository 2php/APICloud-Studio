#导出APICloudStudio说明文档

##环境：

* Eclipse3.7.2(汉化版)
* jdk1.6

##RCP基础简介
	
Eclipse RCP是一项位于Eclipse平台核心的功能,Eclipse为创建可扩展的集成开发环境提供了一个开放源码平台，允许开发人员构建与环境和其他工具无缝连接的工具。

工具是由各个plugin与Eclipse相互关联起来的，workbench和workspace是Eclipse平台的两个必备的插件，插件在运行时需要借助扩展点才可以插入。

APICloud Studio是基于Eclipse和Aptana Studio的标准的Eclipse RCP产品，架构如下：

![图片说明](http://docs.apicloud.com/img/export-apistudio-doc/apicloud-studio-architecture.png)

##一个Eclipse RCP可分为如下五个部分

（1）Wrokbench工作台

为Eclipse提供用户界面。它是使用SWT（Standard Widget Toolkit）和一个更高级的API（JFace）来构建的；SWT是Java的Swing/AWT GUI API的非标准替代者，JFace则建立在SWT基础上，提供用户界面组件。

（2）Workspace工作区

工作区是负责管理用户资源的插件。它包括用户创建的项目、项目中的文件，以及文件变更和其它资源。工作区还负责通知其它插件关于资源变更的信息，比如文件创建、删除或者变更。

（3）Help帮助系统

帮助组建具有与Eclipse平台本身相当的可扩展能力。与插件向Eclipse添加功能相同，帮助提供一个附加的导航结构，允许工具以HTML文件的形式添加文档。

（4）Team团队支持系统

团队支持组件负责提供版本控制和配置管理支持。它根据需要添加视图，以允许用户与所有使用的任何版本控制系统交互。大多数插件都不需要与团队支持组件交互，除非它们提供版本控制服务。

（5）Platform Runtime运行平台

平台运行库是整个Eclipse的内核，它在启动时检查已安装了哪些插件，并创建关于它们的注册表信息。为降低启动时间和资源使用，它在实际需要某个插件的时候才加载该插件。除了内核外，其它每样东西都是作为插件来实现的。

##1、创建.product配置文件

在主插件(com.apicloud.navigator)下新建产品配置(.product)文件，如图1-1

![图片说明](http://docs.apicloud.com/img/export-apistudio-doc/图片1.png)

*图1-1*

在overview中配置相关的Definition，
产品定义包括Product Name，Product ID 和与Product ID相关联的Application。并指定产品配置单元是基于插件的。如图1-2

![图片说明](http://docs.apicloud.com/img/export-apistudio-doc/图片2.png)

*图1-2*

##2、添加导出的插件

如图1-3，在Dependencies中添加导出产品所需要的依赖的插件。
首先添加主插件com.apicloud.navigator，然后点击 “添加必需的插件”。

![图片说明](http://docs.apicloud.com/img/export-apistudio-doc/图片3.png)

*图1-3*

![图片说明](http://docs.apicloud.com/img/export-apistudio-doc/图片4.png)

*图1-4*

##3、配置config.ini

选择生成缺省的config.ini文件，然后替换为下列文本信息。

```java
1 org.eclipse.update.reconcile=false
2  eclipse.p2.profile=profile
3  osgi.framework=file\:plugins/org.eclipse.osgi_3.7.2.v20120110-1415.jar
4  equinox.use.ds=true
osgi.bundles=reference\:file\:org.eclipse.equinox.simpleconfigurator_1.0.200.v20110815-1438.jar@1\:start
org.eclipse.equinox.simpleconfigurator.configUrl=file\:org.eclipse.equinox.simpleconfigurator/bundles.info
5 eclipse.product=com.apicloud.navigator.APICloud
6 osgi.splashPath=platform\:/base/dropins/com.apicloud.navigator
osgi.framework.extensions=reference\:file\:org.eclipse.osgi.nl_zh_3.7.0.v20131123061707.jar
osgi.bundles.defaultStartLevel=4
eclipse.p2.data.area=@config.dir/../p2
eclipse.application=org.eclipse.ui.ide.workbench
```

其中 标号为5的为需要启动的product名称，标号为6的为 启动的splash Screen的位置。

##4、配置启动选项卡

配置启动文件的图标，.ico 文件是一个容器，为其主应用程序包括不同大小和颜色模式的必需的图像文件。启动参数提供产品启动时的默认程序参数和 VM 参数。在这一过程中，在导出文件夹的根目录下将生成一个与启动程序同名的 .ini 文件记录这些参数。可以参照下列信息。

```
-startup
plugins/org.eclipse.equinox.launcher_1.2.0.v20110502.jar
--launcher.library
plugins/org.eclipse.equinox.launcher.win32.win32.x86_1.1.100.v20110502
-vmargs
-clearPersistedState

-Xverify:none
-Xmx512m
-Xms512m
-Xmn128m
-XX:PermSize=128m
-XX:MaxPermSize=256m
-XX:+DisableExplicitGC
-XX:+UseConcMarkSweepGC
-XX:+UseParNewGC
-XX:CMSInitiatingOccupancyFraction=85
-Xnoclassgc
-xss2m
-Dfile.encoding=utf-8
```

##5、在branding中配置窗口图像

![图片说明](http://docs.apicloud.com/img/export-apistudio-doc/图片5.png)

##6、About对话框配置

image与Text
image选择需要的GIF文件，
Text 引用配置文件里的文本信息，如plugin.properties文件中定义键值，或是直接写入文本信息。

##7、发布RCP

overview中在测试部分中点击“同步”( Synchronize )，链接向 plugin.xml 反映您的变更以确保插件 manifest 保持最新。在导出前单击 Launch the product 测试产品。

找到Exporting部分，在弹出的导出对话框中选择要导出的文件目录，选择同步按钮确保产品与编译环境下保持一致。

![图片说明](http://docs.apicloud.com/img/export-apistudio-doc/图片6.png)

![图片说明](http://docs.apicloud.com/img/export-apistudio-doc/图片7.png)




