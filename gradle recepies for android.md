Gradle_Recipes_For_Android	作者：Ken Kousen		翻译：jones；


早在几年前，android就采用gradle作为首选的自动化构建工具，但是很多安卓开发者仍然对gradle这个开源工具不太熟悉。这个亲自实践的指南提供了一系列的gradle方法，从而帮助你快速并且容易的掌握最基本的android app的构建任务。你会学到如何自定义工程布局、添加依赖、创建不同版本的app。
Gradle基于grooy，当然在开始之前你也需要理解一点JVM语言。示例代码使用的Android SDK 23版本，模拟器是android6.0（Marshmallow）或者5.0（Lollipop）。假如你已经熟悉了java和android，那么你已经可以开始了。

内容提要：
1.理解Gradle为Android Apps创建的构建文件
2.通过系统的命令行或者Android Studio内置的命令行运行Gradle
3.为Android App多添加一个Java工程
4.导入导出Eclipse ADT工程
5.数字签名一个要发布到应用市场的正式版本的APK
6.使用product flavors 为同一个App创建多个版本
7.为Gradle 构建程序添加自定义的task
8.测试你的Android 组件 和 非Android组件
9.改善你的Gradle构建性能

























Gradle Recipes for Android
Master the New Build System foe Android
掌握Android新的构建工具







作者：Ken Kousen











目录











































前言

这就是我们所需要的那本书。当Google更换Android开发工具IDE时，正好大概是我们写Head First Android Development 这本书一半的时候。在这时，相当大一部分Android开发者仍然使用的开发工具是Eclipse。但是这时。Google正要推广自己的基于Idea的Android开发工具。
我们常常做大多数科技创始人也喜欢做的事情。一些制造商开始从做一些有意义的事情转向做另一些新的，可能更有意义的事情上面。这经常发生，你可能经常重写所有的示例代码，更新所有的图片，删除现在已经没有用的功能，引进一些新的有用的更具有科技含量的东西。但是为什么要求开发者从Eclipse转变到Android Studio，因为Android Studio作为新的IDE是一个功能非常强大的工具。
我们替换了所有的视图，并且更新了文本，这在第七章会详细介绍，所以再让我们介绍一下这本书还讲了什么。Gradle程序构建Application是非常巧妙的，但是更重要的构建方式相当不同，待会儿我们就明白了。我们可以通过命令行做相当多的此前只能通过IDE做的事情，这就意味着我们可以实现完全自动化的构建。因此我们就可以点一下鼠标，就可以依赖不同的lib，或者创建不同的flavor的app。我们可以编写构建代码实现不同的代码部分，然后就可以编写其余相同的代码部分。
现在对于每一个Android开发者，学习Gradle都是一个非常重要的任务。学习Gradle应该和学习java或者理解Activity的生命周期应该齐头并进。但是通过实验和出错来学习Gralde有时候是一个非常痛苦的过程，这就是这本书出现的原因。通过这本书，你会发现很多有用的方法，并且会帮助你避免最常见的构建错误。无论是你配置测试系统，或者自动化签名APK，更或者试图加速你的构建方式，这本书你都值得拥有。Ken的生动写法和逼真的示例会让你回味无穷。这本书不仅仅适合Grooy的学习者，更适合Gradle的学习者。
—Dawn and David Griffiths
 												Authors, Head First Android 													Development 
April 20th, 2016







前言

关于这本书
这本书包含使用Gradle构建Android工程的方法。Gradle在java世界是最流行的构建工具之一，以至于现在都延伸到了其他语言，例如C++。Google的Android团队把Gradle最为首选的构建工具在2013的春天，从那时候起，他的使用正在稳定的增长。
从Gradle来到Grooy的生态系统，很多Android开发者可能对它还不是特别熟悉。然后，Groooy对于Java开发者是非常容易学的。这本书的目的就是提供示例代码来帮助你学习最基本的Android应用的最常见的构建任务。
所有的示例代码都是基于Android SDK 23、Android 6（Marshmallow）或者Android 5.*（Lollipop）。Android Studio 的2.0或者2.1（beta）是最主要的IDE，它所用的Gradle版本是2.10或者更高的构建工具。

预备知识
Android的Gradle插件至少要了解java知识，Groory知识，Gradle知识和Android知识。
引语

关于这本书
因此整本书是对以上的知识是都是可用的，但是又不能包含这里里面所有的东西。
这本书索要讲解的的主要目标就是针对Android开发者。虽然关于Android方面的介绍的不多，但是所列出的所有示例代码都可以通过这本书的github仓库获取到。理解Android你意味着理解java，所以要学习这本书的要求你的基础是好的。
然而，学习这本书是必须掌握少量的Gradle或者Grooy知识。附录A包含了Grooy语法的快速入门简要。Grooy的概念会在很多不同的地方都重新提到它。附录B介绍了基础的Gradle信息，但是他们所讲述的是贯穿整本书的更详细的东西。
超越这些限制，这本书尽可能的被设计城独立的，与外部引用（特别是文档）提供最合适的地方。
这本书还做了Android Studio的拓展应用，因为它现在是Google官方唯一所推广的你IDE。Android Sdudio为Gradle提供了视图和工具，这些视图和工具在很多块都会有图解。当然这本书并不是Android Studio的教程，但是他会尽可能展示Android Studio的相关的功能，如果这本书帮助你了解到了更多的关于IDE的知识，这当时会锦上添花。


这本书中用到的使用规则：
这本书所用到的印刷规则：
斜体字
表示新的术语，URLs，邮箱地址，文件名称，和文件扩展名。
等宽字体
程序列表，也就是涉及到程序代码的元素，例如变量、方法名称、数据库、数据类型、环境变量、声明、和关键词。
等宽斜体字体
展示命令或者被使用者所输入的文本
等款斜体
被用户所替换的文本或者被指定的值




使用代码用例：
补充的内容（例如代码，练习等等）可以通过https://github.com/kousen/GradleRecipesForAndroid 这个连接来下载。
这本书所讲的内容会对你的工作很有用。通常，在这本书上所用到的代码，你都很可能会在你的项目中用到。你不用经过我们的允许就可以直接使用我们所提供的代码除非你要重构重要部分的代码。例如，你可以直接使用本书中的大块代码而不用经过我们的允许、出售或者散布 O’Reilly 这本书的CD也不用经过我们的允许、回答问题而引用我们的代码、引入自己的项目而合并这本书中很多的示例代码等等都不用通知我们得到许可。
我们很感激，但是不强求，归因。通常归因包含标题，作者，出版社和ISBN。例如：
“Gradle Recipes for Android by Ken Kousen (O’Reilly). Copyright 2016 Gradleware, Inc., 978-1-4919-4702-9.” 
如果您觉得您对代码示例的使用不属于合理使用或上述许可的范围，请随时与permissions@oreilly.com联系。

Safari® Books Online（safari在线书城）
Safari Books Online 是一个对大家来说是一个必需品，因为这上面有好多专家发售的书或者专家所讲授的视频，尤其是在科技和商业这两个行业。
科技专家、软件开发者、网站设计者、以及商人和创造性的专家都把Safari Books Online作为他们研究、解决问题学习、资格认证培训的主要渠道。
Safari Books Online对与企业、政府、教育以及个人都有一系列的计划和定价。






















第一章
Gradle的Android基础


Android应用使用开源项目Gradle作为自己的构建系统。Gradle提供了很完美的API，支持高度定制，并且在JAVA世界广泛使用。Android的Gradle插件为Android提供了很多的特定的功能，包括“build types”、“（flavors）风味”、“（signing configurations）签名配置”、“（library projects）依赖工程”等等。
这本书中所涉及到的方法讲述了Gradle应用到Android工程所能发挥的作用。自从Android Studio开始使用Gradle作为构建工程以后，我们也开始努力更好的支持Studio。

1.1 Android中的Gradle构建文件
问题：
在Android工程中，如何更好的理解生成的Gradle构建文件
解决方案：
使用Android Studio新建一个Android工程，查看这几个文件，setting.gradle,build.gradle，以及app目录下的build.gradle。

讨论：
Android Studio是Google官方所推的唯一的Android的IDE。使用Android Studio创建一个新的工程，点击向导界面中的“Start a new Android Studio project”按钮。
如下图（Figure1-1)

Figure1-1. Android Studio 快速开始





这个向导程序会引导你命名包名和工程名字。你也可以使用快速开始向导迅速的创建一个工程，名称是My Android App 、包名为“oreilly.com”,如图Figure 1-2。
到这个页面，只选择“Phone and Tablet”选项，同时添加一个空的使用默认名字的Activity（MainActivity）。
特别说明：activity的名字和类型并没有和Gradle的构建文件相关联
在android模式下的工程视图如图Figure 1-3,相关的Gradle文件是带箭头的所指示的。

Figure 1-2.创建新的工程向导

Figure 1-3.工程结构（Android 视图）


工程的默认布局视图如图Figure 1-4.


Figure 1-4.工程结构（Project 模式）

Android工程是由多工程组成的，由Gradle构建将他们协调到一块的。setting.gradle文件展示了子目录下所有拥有的子工程。默认的文件目录如图Example 1-1。
Example 1-1. Setting.gradle
include ‘:app’
“include”语句代表的意思是子目录app是唯一被添加的子Project。如果你想添加一个子Android工程project，也需要在这个文件中添加一句话。
根目录下的Gradle构建文件如Example 1-2 所示：
Example 1-2. Top-level Build.gradle 文件

//top-llevel build file where you can add configuration options
//common to all subprojects/moudules
Buildscript{
Repositories{
jcenter()
}
Dependencies{
classpath ‘com.android.tools.build:gradle:2.0.0’
//note:Do not place your application dependencies here;they belong in the 	//individual module build.gradle file
}
allprojects{
repositories{
Jecenter()
}

}

Task clean(type : Delete){
Delete rootProject.buildDir
}
}

默认的Gradle是不支持Android函数的，但是不要担心Android官方提供了Gradle的Android插件，很容易的支持我们配置Android工程。根目录下的”buildscipt”代码块告诉Gradle去哪里下载这个Android插件。
正如你看到的一样，默认的Android插件是从jecenter下载下来的，jecenter是一个Bintray JCenter Artifactory 仓库。Gradle也是支持其他仓库的（尤其是mavenCentral（）仓库，maven默认的仓库），但是目前是Jcenter是Gradle默认的仓库。从JCenter下载的所有内容都是使用了Https安全验证，并且都是经过CDN加速的。
“allproject”代码块意思是一级工程和所有的子工程都默认使用JCenter（）作为仓库，所有的Java 依赖库都会去JCenter仓库中去寻找。
Gradle也允许你自定义的自己的任务，而且你可以直接将这个任务插入到有向非循环图中（DAG），有向非循环图是用来解析任务依赖用的。在这里，clean这个任务被添加到了一级构建中。”Type:Delete”部分表示这个自定义的任务的类型是Delete，这样写是和你新建一个Gradle中的Delete任务是等价的。在这种情况下，这个任务所做的就是删除默认一级目录下的构建文件夹。
子工程app的Gradle的构建文件如Example1-3所示：
Example 1-3. 
Apply plugin:’com.android.application’
Android{
compileSdkVersion 23
buildToolsVersion “23.0.3”
defaultConnfig{
applicationId “com.kousenit.myandroidapp”
minSdkVersion 19
targetSdkVersion 23
versionCode 1
versionName “1.0”
}
buildType{
release{
minifuEnable false
proguardFiles getDefaultProguardFile(‘proguard-android.txt’),
‘proguard-rules.pro’
}
}
}
dependencies{
compile fileTree(dir: ‘libs’,include:[‘*.jar’])
testCompile ‘junit:junit:4.12’
Complile ‘com.android.support:appcompat-v7:23.3.0’
}
Gradle的apply函数的作用是为构建系统添加Android插件，只有有了Gradle的Android插件，’android’代码块才会生效。这部分我们会在Recipe1.2进行讲解。
‘dependencies’代码块又三行代码组成。第一行，文件数依赖，表示所有的在libs文件夹下的以’.jar’结尾的文件都会被加到编译中来。
第二行表示告诉Gradle去下载4.12版本的JUnit，并且再下载完成之后加入到工程的测试编译阶段，有了这句话就表示在src/androidTest/java目录下的文件是可用的，还有src/test/java这个目录也是可用的，这个目录代表纯粹的Junit测试。
第三行大代码告诉Gradle添加Android支持包中‘appcompar-v7 jar’23.3.0 这个版本的依赖包。


另见参阅：
相关的文档的链接地址会在Recipe6.2中介绍。所依赖的内容会在Recipe1.5中介绍，仓库也会在Recipe1.7中进行讨论。




1.2 配置SDK版本和其他的默认配置
问题：
你可能想要定制最小的和目标的Android SDK版本，以及其他的默认的属性。
解决方法：
一级目录下的Android build.gradle通过”buildscrpt”代码块为你的工程添加了Gradle的安卓插件。模块的build.gralde通过”apply”关键字应用这个Gradle插件，具体的使用都是通过在android代码块的配置来定义的。

在这个android块中，你可以指定好些个工程属性，如下面Example 1-4所示：

Example 1-4. build.gradle的android代码块

apply plugin : ’com.android.application’
android{
compileSdkVersion 23
buildToolsVersion “23.0.3”
defaultConfig{
applicationId “com.kousenit.myandroidapp”
minSdkVersion 19
targetSkdVersion 23
versionCode 1
versionName “1.0”
}
compileOptions{
sourceCompatibility JavaVersion.VERSION_1_7
targetCompatibility JavaVersion.VERSION_1_7
}
}

常规的Java工程使用“java plug-in”,android工程使用”com.android.application” 插件所代替。

注意应用Java插件，如果应用了会造成构建错误。要使用android插件。
android 代码块是Android DSL语言的入口。在这你必须要通过”compileSdkVersion”关键字来指定工程的编译版本和”buildToolsVersion”关键字来指定构建工具的版本。所指定的版本号最好是当下最流行的版本，因为它们是向后兼容的、而且修复了当下出现的bug。

在android 代码块中的defaultConfig 代码块中可以配置如下属性：
applicationId
应用的包名，applicationId必须是唯一的在Google Play store中。这个值在应用的整个生命周期中都是不可变更的；改变它代表又会出现一个新的app，已经安装该应用的用户不会知道你的app更新。优先把他放到Gradle文件中来配置，在gradle中配置等价于android manifest文件中的package标签中的值。现在这两种声明方式已经解耦了。
minSdkVersion
应用所支持的最低版本的Android SDK。更早版本的设备在Google商店里面是看不到这个应用的。
versionCode
应用版本号（是数字类型的），代表着你应用的当前版本号。Apps通常在做程序更新的会用到。
versionName
一个字符串代表着你已经发版的应用的名称（这个名称一般是展示给用户看的）。

“minSdkVersion”和“buildToolsVersion”这两个属性原来是在Android的清单文件中”<use-sdk>”这个标签中来配置，但是现在建议优先使用gradle的配置来代替。原因是清单文件中的这个值会在Gradle构建的过程中将它替换掉（前提是你在Gralde文件中做过配置）。
“compileOption”部分含义就是配置app使用的JDK版本是1.7.



在Android Studio中，工程结构的对话框展示结果如图 Figure 1-5 所示。
“defaultConfig”代码块中配置的默认值就是选中的flavors选项卡，如图Figure1-6所示。
“defaultConfig”代码块的写法，和其他的DSL元素一样，也可以在DSL reference中找到。


Figure 1-5. 工程结构图


Figure1-6. android代码块中的属性

补充说明：
Gradle的android插件中的其他元素（如：buildTypes 或者 pruductFlavors）的用法，我们会在Recipe3.1、recipe3.2、Recipe3.4等等章节会详细的讲解。

1.3 使用命令行执行Gralde构建任务

需求：使用命令行执行构建任务

解决方案：通过命令行，要么使用项目提供的Gradle Wraper，要么安装Gradle后在在自己项目的跟目录下面直接运行gradle。



讨论：
你没有必要为了构建Android项目而专门安装Gradle。Android Studio 一直以来都支持Gradle（通过插件的的形式），并包括专用的功能来支持它。
术语“Gradle Wraper”是指 用于Unix和gradlew.bat的gradlew脚本，路径是在Android  Application的根目录下。“W”就是只Wraper。
Gradle wraper的作用就是允许客户端在没有安装Gradle的情况下也能够运行Gradle。在应用的根目录下，gradle/wraper目录下的gradle-wraper.jar 和gradle-wraper.properties这两个文件，是gradle用来启动进程用的。Properties文件的实例代码如图Example 1-5 所示。

Example 1-5.    gradle-wraper.properties 中的键值对
#Mon Dec 28 10:00:20 PST 2015
distributionBase = GRADLE_USER_HOME
distributionPath = wraper/dists
zipStoreBase = GRADLE_USER_HOME
zipStorePath = wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-2.10-all.zip

	distributionUrl这个属性表示wrapper会去下载和安装2.10的Gradle。第一次运行之后，Gradle会缓存到zipStoreBase所指定的目录和zipStorePath所指定的路径下面。随后Gradle所执行的任务都会使用Gradle的缓存。
	
	在Unix下使用./gradlew命令，在window下面使用gradle.bat 命令来使用wrapper。

Example 1-6. 运行构建任务的输出
>./gradlew build
Downloading
http://services.gradle.org/distributions/gradle-2.10-all.zip
………………………………………..
……….			(download of Gradle 2.10) ……
…………………………………………………………….
Unzipping /Users/kousen/.gradle/wrapper/dists/3i2gob…/gradle-2.10-all.zip
Set executable permissoins for:
/Users/kousen/.gradle/wrapper/dists/gradle-2.10-all/3i2gob…./gradle-2.10/bin/gradle
starting a new Gradle Deamon for this build(subsequent builds will be faster).
:app:preBuild Up-TO-DATE
..lots of tasks …
:app:compileLint
:app:lint
Wrote HTML report to file:…/MyandroidApp/app/build/outputs/lint-results.html
Wrote XML report to ../MyAndroidApp/app/build/ourputs/ling-results.xml
:app:preDubugUnitTestBuild UP-TO-DATE
:app:prepareDubugUnitTestDependencies
… lots of tasks …
:app:test
:app:check
:app:build
BUILD SUCCESSFUL

Total time:51.352 secs // most of which was the download



	初始化下载任务可能会话费大概几分钟的时间，这依赖于你的电脑的网络环境。这仅仅会下载一次，随后的执行任务会直接使用已经下载的缓存来运行。
	在命令行中，你可以执行任何Gradle任务，包括自己定制的任务。在app/build folder 这个路径下面可以找到编译的代码。路径app/build/outputs/apk 是我们所产生apk的路径。
	Example 1-7 展示了在构建过程中可用的Gradle任务。
Example 1-7.Output from tasks
:tasks
All tasks runnable from root project
Android tasks
androidDependencies -Displays the Android dependencies of the project.
SigningReport – Displays the signing info for each bariant.
SourceSets – Prints out all the source sets defined in this project.
Build tasks
assemble – Assembles all vatiants of all aplications and secondary packages.
AssembleAndroidTest – Assembles all Test applications.
AssembleDebuf – Assembles all Release builds.
Build – Assembles and tests this project.
BuildDependents – Assembles and tests this peoject and all projects that depend on it.
BuildNeeded – Assembles and tests this project and all projects it depends on.
CompileDebugAndroidTestSources
compileDebugSources
compileDebugSources
compileDebugUnitTestSources
compileReleaseSources
compileReleaseUnitTestSources
mockableAndroidJar - Creates a version of android.jar that's suitable for unit tests.
Build Setup tasks
-----------------
init - Initializes a new Gradle build. [incubating]
wrapper - Generates Gradle wrapper files. [incubating]
Help tasks
----------
components - Displays the components produced by root project 'MyAndroidApp'.
dependencies - Displays all dependencies declared in root project 'MyAndroidApp'.
dependencyInsight - Displays the insight into a specific dependency in root
project 'MyAndroidApp'.
help - Displays a help message.
model - Displays the configuration model of root project 'MyAndroidApp'. [incubating]
projects - Displays the subprojects of root project 'MyAndroidApp'.
properties - Displays the properties of root project 'MyAndroidApp'.
tasks - Displays the tasks runnable from root project 'MyAndroidApp'
(some of the displayed tasks may belong to subprojects).
Install tasks
-------------
installDebug - Installs the Debug build.
installDebugAndroidTest - Installs the android (on device) tests for the Debug build.
uninstallAll - Uninstall all applications.
uninstallDebug - Uninstalls the Debug build.
uninstallDebugAndroidTest - Uninstalls the android (on device) tests for the build.
uninstallRelease - Uninstalls the Release build.
Verification tasks
------------------
check - Runs all checks.
clean - Deletes the build directory.
connectedAndroidTest - Installs and runs instrumentation tests for all flavors
on connected devices.
connectedCheck - Runs all device checks on currently connected devices.
connectedDebugAndroidTest - Installs and runs the tests for debug connected devices.
deviceAndroidTest - Installs and runs instrumentation tests using all Providers.
deviceCheck - Runs all device checks using Device Providers and Test Servers.
lint - Runs lint on all variants.
lintDebug - Runs lint on the Debug build.
lintRelease - Runs lint on the Release build.
test - Run unit tests for all variants.
testDebugUnitTest - Run unit tests for the debug build.
testReleaseUnitTest - Run unit tests for the release build.
Other tasks
-----------
clean
jarDebugClasses
jarReleaseClasses
lintVitalRelease - Runs lint on just the fatal issues in the Release build.
To see all tasks and more detail, run gradlew tasks --all
To see more detail about a task, run gradlew help --task <task>
BUILD SUCCESSFUL
While this may seem like a lot of tasks, you actually use a small number in practice.
When you add multiple build types and flavors to your project, the number will go up
considerably.


	附加的功能，和命令行标记
你可以在一个命令里面运行多个任务，通过空格来分割，如Example 1-9.
Example 1-9.运行不止一个你命令
>./gradlew lint assembleDebug
注意：重复输入相同的任务名字，但是只会执行一遍任务。

你可以使用-x flag语法排除某个任务，如:Example 1-9所示：
Example 1-9. 排除lintDubug 任务
>./gradlew assembleDebug -x lintDebug
使用--all 这个参数在任务上可以展示工程中所有的任务，并且可以展示出来每个任务之间的依赖关系。


	你还可以使用缩写任务名字，但是这些缩写字母应该能唯一的表示出这个任务的名称，如Example 1-10
Example 1-10. 每个任务的依赖树结构
>./gradlew anDep
>:app:androidDependencies
debug
\---com.android.support:appcompat-v7:23.3.0
    +---com.android.support:support-vector-drawable:23.3.0
    |   \---com.android.support:support-v4:23.3.0
    |       \---LOCAL:internal_impl-23.3.0.jar
    +---com.android.support:animated-vector-drawable:23.3.0
    |   \---com.android.support:supposrt-vector-drawable:23.3.0
    |       \---com.android.support:supposrt-v4:23.3.0
    |           \---LOCAL:internal_impl_impl-23.3.0
    \---com.android.support:supposrt-v4:23.3.0
        \---LOCAL:internal_impl-23.3.0.jar
debugAndroidText
No dependencies

release
\--- com.android.support:appcompat-v7:23.3.0
	+--- com.android.support:support-vector-drawable:23.3.0
	|	\--- com.android.support:support-v4:23.3.0
	|	\--- LOCAL: internal_impl-23.3.0.jar
	+--- c![](http://)om.android.support:animated-vector-drawable:23.3.0
	|	\--- com.android.support:support-vector-drawable:23.3.0
	|	\--- com.android.support:support-v4:23.3.0
	|	\--- LOCAL: internal_impl-23.3.0.jar
			\--- com.android.support:support-v4:23.3.0
			\--- LOCAL: internal_impl-23.3.0.jar
releaseUnitTest
No dependencies
BUILD SUCCESSFUL


	驼峰法写法是生效的（如：anDep 代表androidDependencies）,知道标识能够代表任务的唯一性都是可以的 如Example 1-11
Example 1-11. 字符串不能唯一确定一个任务
>./gradlew pro
FAILURE: Build failed with an exception.
* What went wrong:
Task 'pro' is ambiguous in root project 'MyAndroidApp'. Candidates are:
'projects', 'properties'.
错误日志信息显示的是你输入的命令的错误原因：pro这个任务所指的的模糊的，因为它能匹配到project和properties这两个任务，所以解决办法就是在添加一个字母从而能够确定唯一的一个任务。

最后，如果你的构建你文件不是build.gradle，你可以用-b参数来指定构建文件（如Example 1-12）.

Example 1-12.使用非默认的构建文件
>./gradlew -b app.gradle

提示：
	附录B会讲解Android之外的Gradle的安装和功能。Recipe1.5 讲解构建文件的依赖，Recipe4.3会讲解在构建过程中排除任务




1.4 使用Android Studio用 执行Gradle构建程序

疑问：你想在Android Studo内部运行Gradle吗

方案：使用Gradle视图然后执行Gradle任务

讨论： 当你创建Android工程的时候，Android Studio会为这个多工程的项目创建Gradle的构建文件（已经在Recipe1.1中讲过）。Android Studio还提供了Gradle视图，在这个视图中你可以看到所有的任务列表，如图：Figure 1-7所示
![](https://raw.githubusercontent.com/challengemyself/android-recipies-for-android/master/imgs/2016-12-13.png) Figure 1-7.android studio 中内置的Gradle视图
在android studio的这个Gradle视图中，Gradle的所有任务是被分门别类的组织起来的，分别的类目像android,build,install,other，就像 Figure 1-7 的图解一样。
如果想要运行一个指定的Gradle的任务，你只需要双击列表中的某一个任务。运行结果会像 Figure 1-8 所示。
双击任何任务该任务就会执行，执行的任务信息都会展示在命令行中，这个命令行是在“Run window”这个选项卡中。每次你执行某个特定的任务，都会创建一个任务的配置信息，并且这个配置信息会保存到“Configurations menu”中，所以你想在执行一次该任务的时候你只需要再一次的上级这个任务就ok了。
![](https://github.com/challengemyself/android-recipies-for-android/blob/master/imgs/2016-12-15-00-38-18.png?raw=true)
				Figure 1-8.Running Gradle inside Android Studio
	在“Run window”面板上展示的执行信息仅仅是Gradle的前端提示信息而已，本质上Gradle的执行全都是命令行。
    Android studio 还提供了专门的"Gradle console",如图Figure 1-9所示：
    ![](https://github.com/challengemyself/android-recipies-for-android/blob/master/imgs/2016-12-15-01-03-23.png?raw=true)
    Figure 1-9.Gradle Console view in Android Studio
	另外：
    使用命令行运行Gradle任务，参考 Recipe 1.3
    
    
    
    1.5 添加java依赖工程。
    需求： 在你的Android工程总添加Java工程依赖包。
    解决方案：在你的application模块的build.gradle文件中的“dependencies”闭包里面添加 组，名字，版本。
    讨论：默认情况下，Android application会创建两个build.gradle文件，一个在一级目录下，另一个在applicaton本身这个module的目录下面，这个module在工程的二级目录下面，文件夹名字叫app。
    在app目录下的build.gradle文件里面，有一个代码块叫dependencies。如Example 1-13,创建这个文件的时候，代码块里面默认的代码设置。
    Example1-13 , 新的android工程的默认的dependencies
    dependencies{
    	compile fileTree(include:['*.jar'],dir:'libs')
        testCompile 'junit:junit:4.12'
        compile 'com.android.support:appcompat-v7:23.3.0'
    }
    
    基础语法：
    Gradle支持好几种写法来添依赖。最常用的方式就是最外层是引号，然后中间使用冒号来分割 组、名字、版本。
    
    notes:Gradle文件使用的是Groovy语言，故而它是支持单引号和双引号的。双引号所表示的意思是可以在中间插入值的，或者是变量的替代品或者其他完全相同的东西。更细节的东西请查看 Appendix A。
    
    每一个依赖都关联着一个configuration。Android工程包括 编译，运行时，测试编译和测试运行时配置。使用Android的Gradle插件你可以添加额外的配置，你也可以自定义你自己的配置。
    依赖的完整语法都和 组、名字、版本 严格的相关联。（如Example 1-14）
    Example 1-14 :依赖的完整语法
    testCompile group:'junit',name:'junit',version:'4.12'
    Example 1-14 和 Example 1-15 是完全等价的。
    Example 1-15 : 依赖的精简语法：
    testCompile 'junit:junit:4.12'
    在默认build.gradle使用的就是依赖的缩略模式。
    使用版本号和加号来指定版本号，虽然不推荐这样使用，但是确是符合语法的，如 Example 1-16所示：
    Example 1-16 版本号使用变量（不推荐）
    testCompile 'junit:junit:4.+'

	这句话就是告诉gradle这个项目测试所需要Junit的版本是4.0或者大于4.0的Junit的gradle插件。当你的这种配置生效时，同时也就意味着项目所以来插件的不确定性，因此当有新的版本更新时之前版本的缓存是不可在利用的。与这相反的是配置准确的版本号能够避免更新的版本API而导致不可用。
    
    
    
    
提示： 配置精确的版本号，避免因更新版本而造成依赖不可用，并且缓存是始终是可用的。


	如果你想依赖本地的一些文件并且还不想把他们放到远程的仓库里面，那么你可以在dependencies这个代码块中使用files或者fileTree语法，如Example 1-17所示
    denpendencies{
    	compile files('libs/a.jar','libs/b.jar')
        compile fileTree(dir:'libs',include:'*.jar')
    }
    最后那句使用相同的语法，默认的gradle构建文件也是使用相同的语法。
   	接着，Gradle会解析这些依赖，但是需要知道去哪里去找这些资源，这就是repositories代码块所起的作用。
    
    
    
    
    同步项目
    
    Android Studio会时刻的监视Gradle构建文件的变化，并且在Gradle变化时提供了同步操作。
    
    举个例子：考虑在app项目中添加Retrofit到项目中。
    
    如图 igure 1-10 所示：当改变了build.gradle文件的内容的时候，Android Studio提供了同步项目的操作。这个操作会触发项目去下载所依赖的库并且将他们加入到当前工程的依赖。
    ![](https://raw.githubusercontent.com/challengemyself/android-recipies-for-android/master/imgs/2016-12-25%2014-01-33figure1-10.png)
    Figure 1-10.Android Studio提供了同步项目依赖的操作，
    点完SysNow链接之后，已经在下载的库会出现在工程窗口的 External Libraries 代码块中。如Figure 1-11中。
    ![](https://raw.githubusercontent.com/challengemyself/android-recipies-for-android/master/imgs/2016-12-25%2014-02-16figure1-11.png)
    Figure 1-11 ,External Libraries
    在这种情况下，在添加retrofit依赖的同时，okhttp和okio的依赖库也会随着被添加过来，如图Fiture1-12所示。
    如果错过了点击Sync Now link的时机，Android Studio也特意在toolbar的快捷键列表里面提供了一个同步的功能。
    ![](https://raw.githubusercontent.com/challengemyself/android-recipies-for-android/master/imgs/2016-12-25%2014-02-52figure1-12.png)
    Figure1-12，使用专门提供的按钮来同步Gradle文件。
    
    
    传递依赖
    有一个老的笑话，将Maven定义为DSL目的是为了从网上去下载依赖。如果这可行的话，那么对于Gradle也是同样使用的。两者都支持下载间接依赖，所谓间接依赖就是项目所依赖的库还依赖着别的库。
    提到Example 1-13 denpendencies的代码块。运行androidDependencies任务 看看实处日志，如 Example 1-18所示：
    Exmaple 1-18 查看Android dependencies
    > ./gradlew androidDependencies
        :app:androidDependencies
        debug
        \--- com.android.support:appcompat-v7:23.3.0
        +--- com.android.support:support-vector-drawable:23.3.0
        |
        \--- com.android.support:support-v4:23.3.0
        |
        \--- LOCAL: internal_impl-23.3.0.jar
        +--- com.android.support:animated-vector-drawable:23.3.0
        |
        \--- com.android.support:support-vector-drawable:23.3.0
        |
        \--- com.android.support:support-v4:23.3.0
        |
        \--- LOCAL: internal_impl-23.3.0.jar
        \--- com.android.support:support-v4:23.3.0
        \--- LOCAL: internal_impl-23.3.0.jar
        debugAndroidTest
        No dependencies
        
       	debugUnitTest
        No dependencies
        release
        \--- com.android.support:appcompat-v7:23.3.0
        +--- com.android.support:support-vector-drawable:23.3.0
        |
        \--- com.android.support:support-v4:23.3.0
        |
        \--- LOCAL: internal_impl-23.3.0.jar
        +--- com.android.support:animated-vector-drawable:23.3.0
        |
        \--- com.android.support:support-vector-drawable:23.3.0
        |
        \--- com.android.support:support-v4:23.3.0
        |
        \--- LOCAL: internal_impl-23.3.0.jar
        \--- com.android.support:support-v4:23.3.0
        \--- LOCAL: internal_impl-23.3.0.jar
        releaseUnitTest
        No dependencies
   debug和release两个任务都依赖appcompat-v7库，而appcompat-v7库又依赖support-v4库，但是这个库又依赖Android Sdk中的通用库。
   手动管理间接依赖库听起来像是好的主意，但是你实际上亲自动手的时候才知道其中的困难。现实的复杂情况会致使依赖增长的非常快，而且很难瘦身。但是Gradle却在项目不断迭代的情况下解决的依赖的情况下处理的非常好。
   仍然，Gradle并没有专门提供语法来导入和导出相对独立的依赖库的操作。
   默认情况下Gradle是支持间接依赖的。当然如果你想要在某个特定的依赖库中取消掉这个功能，你可以在使用transitive这个标签，如Example 1-19所示：
   denpendencies{
    runtime	group:'com.squareup.retrofit2',name:'retrofit',version:'2.0.1',transitive:false
   }
   改变transitive标签的值为false，避免了Gradle去下载库的间接依赖，所以你就必须手动的去添加你想要添加的间接依赖库。
   
   如果你仅仅想模块化管理jar，而不想额外的添加依赖，你也可以专门指定他们。如Example 1-20所示：
   Example 1-20，单独模块化管理jar文件完整语法
   dependencies{
   	complile 'org.codehaus.groovy-all:2.4.4@jar'
    compile group:'org.codehaus.groovy',name:'groovy-all',version:'2.4.4',ext:'jar'
   }
   第一行内代码使用的是简洁的语法，第二行使用是完整的语法。
   简洁语法的写法使用了@符号。但是完整语法的写法确实使用了ext（作为扩展）来赋值。
   
   你也可以在denpendencies代码块儿中来导出简洁依赖库，如Example 1-21 所示：
   Example 1-21，导出依赖
   denpendencies{
   	androidTestCompile('org.spockframework:spock-core:1.0-groovy-2.4'){
    	exclude group:'org.codehaus.groovy'
        exclude group:'junit'
    }
   }
   
   通过这种方法，spock-core工程就导出了Groovy依赖和Junit库，两者都可以被间接依赖。
   另外：1.6章节讲述了如何通过Android Studio添加依赖。1.7章节讲述了如何去解析远程的依赖库。4.5章节讲述了lib依赖lib的解决方案。
   
   
   1.6 使用Android Studio添加Lib
   
   问题：相比较直接编辑build.config文件添加依赖包引用，使用Android Studio这个IDE添加应该会更加的方便。
   解决办法：在Android studio的Project Structure面板中的Dependencies选项卡。
   讨论：有经验的Gradle开发者还是比较倾向于直接编辑build.gradle文件来添加依赖等等，主要原因还是IDE没有直接提供可视化的代码助手来帮助开发者来做这件事情。
   你可以通过Structure菜单随时查看全部的展示，这个菜单在File的菜单下。然后选择包含在你的工程中module，如Figure1-13所示。
   ![](https://github.com/challengemyself/android-recipies-for-android/blob/master/imgs/2017-01-07%20figure1-13.png?raw=true)
   Figure 1-13，工程结构ui（在之前的Figure1-5有提到）
   
   打开面板默认选中的选项卡是app，并且在右侧会默认高亮选中Properties。内容区域展示的内容有Compile Sdk Version和Build Tools Version等等。
   点击Dependencies选项卡，会看见该module所有的现已经存在的依赖库，而且在此时的视图中还可以添加新的依赖（如Figure 1-14）。
   ![](https://github.com/challengemyself/android-recipies-for-android/blob/master/imgs/2017-01-07%2019-05-51figure1-14.png?raw=true)
   Figure 1-14.Dependencies tab in Properties
   在“Scope”这列选项中你可以配置这项依赖所管辖的范围，目前的选项有：Compile，Provided，APK，Test Compile,Debug compile,Release compile
   点击最右侧的加号，会出现三个不同类型依赖的选项，如图Figure 1-15.
   ![](https://github.com/challengemyself/android-recipies-for-android/blob/master/imgs/2017-01-07%2019-06-24figure1-15.png?raw=true)
   Figure 1-15.添加依赖的弹出菜单。
   选择File dependencies你可以浏览文件系统中的所有的jar文件。选择Module dependencies则会涉及到在这个项目中的其他的module，这个选项会在library project章节进行讨论。
   选择Library Dependencies则会额外的弹出一个对话框，你可以在这个对话框中搜索存在于Maven Central上的依赖库。默认情况下所展示的就是所有的支持的库和Google服务（Figure 1-16）。
   ![](https://github.com/challengemyself/android-recipies-for-android/blob/master/imgs/2017-01-07%2019-06-50figure1-16.png?raw=true)
   Figure 1-16.选择依赖库
   在搜索框中，输入字符串然后点击搜索的图标，之后就会去搜索所有存在于Maven上的符合条件的依赖库。再然后点击ok按钮，这个操作会触发Gradle的同步，然后就会去网上去下载所选中的依赖库（如图 Figure 1-17）。
   ![](https://github.com/challengemyself/android-recipies-for-android/blob/master/imgs/2017-01-07%2019-24-17figure1-17.png?raw=true)
   Figure 1-17.寻找Gson依赖库
   提示： 1.5章节讲述如何直接编辑Gradle文件去添加依赖。1.7章节讲述如何配置Gradle的资源库。
   
   
   
   1.7 配置Gradle仓库
   问题：如何配置仓库地址，该地址是告诉Gradle去哪里寻找Library的具体的网上的位置
   解决方案：在Gradle文件中去配置repositories代码块
   讨论：
	声明仓库地址：
   	repositories代码块告诉Gradle去哪里寻找依赖库地址。默认情况下，Android使用的是jcenter或者mavenCentral,如Example 1-22所示：
    Example 1-22，默认的JCenter仓库
    repositories{
    	jcenter()
    }
    这句代码所代表的意思就是JCenter仓库，JCenter仓库的地址是：https://jcenter.bintray.com。注意网址使用的是HTTPS连接。
    mavenContral()这句代码所代表的意思是Maven2远程仓库，对应的网址是：http://repo1.maven.org/maven2。mavenLocal()代码所代表的就是本地的Maven缓存（如Example 1-23）所示：
    Example 1-23，在repositories代码块中添加maven仓库
    repositories{
    	mavenLocal()//本地maven缓存
        mavenCentral()//远程的maven地址
    }
    任何的maven仓库都可以被添加到默认的列表中，在maven代码块中使用url符号（如Example 1-24）。
    Example 1-24。添加Maven仓库使用URL
    repositories{
    	maven{
        	url 'http://repo.spring.io/milestone'
        }
    }
    有权限保护的maven仓库可以使用credentials代码块，如（Example 1-25  摘自Gradle的使用指南）所示：
    repositories{
    	maven{
        	credentials{
            	username 'username'
                password 'password'
            }
            url 'http://repo.mycompany.com/maven2'
        }
    }
    你还可以将完整的用户名和密码写在gradle.properties文件中。我们在2.1章节中会仔细的讲解这个。
    Ivy和本地的配置也使用相同的语法添加
    Example 1-26.使用Ivy仓库
    repositories{
    	ivy{
        url 'http://my.ivy.repo'
        }
    }
    如果你在本地有仓库文件，你也可以使用一个本地路径作为仓库，语法是使用flatDir符号（如Example 1-27）
    repositories{
    	flatDir{
        	dir 'lib'
        }
    }
    严格的来说，还有一种添加依赖的方法（以前所讲述的方法只是在Dependencies中配置添加），还有一种添加以来的方法就是添加文件依赖或者文件树。
    当你在build.gradle添加依赖仓库时，程序会从上到下去解析，直到解析完所有的的依赖。
    补充：1.5章节和1.6章节主要讲述依赖的配置。
    
    第二章：从导入项目到发版
    
    2.1 设置项目属性
    问题：你想要给项目添加额外的属性，或者想要抽出来硬编码的数值
    解决方案：在ext代码块中定义普通的数值。目的是为了将他们从构建文件中提取出来，或者将这些值放到gradle.properties文件中，在或者使用命令行对他们进行赋值（使用-P 标签）。
    讨论：gradle支持使用ext标签来定义变量，ext代表的意思就是extra。有了这个功能我们定义变量就会变得很容易，即使在这个文件之外也能使用这个变量。
    如果你想写死在build文件中的化也是可以的。Example 2-1 就是在构建文件中写死的一个例子。
    ext{
    	def AAVersion = '4.0-SNAPSHOT'//将这个写成你想要的数值
    }
    dependencies{
    	apt "org.androidannotations:androidannotations:$AAVersion"
        compile "org.androidannotations:androidannotations-api:$AAVersion"
    }
    
    
    正常的Groovy语法在这个例子中已经所运用到、在这里AAVersion仅仅是一个被赋值的字符串，但是它同时被两句话所引用。
    def关键字在这里是指在这个构建文件里面有一个本地变量，而不用def关键字所表达的意思就是在工程里面添加一个属性，这也就是说在该工程里面所有的子项目都可以访问得到这个属性。
    小提示：在ext代码块里面添加无类型的变量，实际上也是给project添加属性，但是这个属性是跟每次运行的工程实例紧紧相关联的.
    假如你可能不想从build文件中获得实际的参数，那么你可以直接在使用maven验证用户名和密码的时候写死值。如Example2-2所示：
    Example 2-2.在Maven仓库中写入凭证
    repositories{
    	maven{
        	url 'http://rep.mycompany.com/maven2'
            credentials{
            	username 'user'①
                password 'password'①
            }
        }
    }
    
    ①这种情况是写死值的方法
    你很可能不想在build文件中直接写入用户名和密码。而另一种可行的方法就是在项目根目录下的gradle.properties定义两个变量如Example 2-3所示
    Example 2-3 gradle.properties文件
    login = 'user'
    pass = 'my_long_and_highly_complex_password'
    
    到此为止在Example 2-2中的认证代码块可以用定义的变量来代替了，如Example2-4所示：
    Example 2-4，移除在credentials代码块中明确写用户名密码的写法
    reposities{
    	maven{
        	url 'http://repo.mycompany.com/maven2'
            credentials{
            	username login①
                password pass①
            }
        }
    }
    
    ①从gradle.properties文件中定义的变量或者从命令行中提供的
    
    你还可以选择从命令行中设置变量的值，通过使用-P参数，如Example 2-5
    Example 2-5。运行gradle使用-P标志
    >gradle -plogin = me -Ppassword = this_is_my_password assemDebug
    
    当你使用了多种方法的时候，你可以通过如Example 2-6的例子来证明到底是使用了哪种方法来配置用户名和密码的。
    Example 2-6，动态判断变量
    ext{
    	if(!project.hasProperty('user')){①
        	user = 'user_from_build_file'
        }
        if(!project.hasProperty('pass')){①
        	pass = 'pass_from_build_file'
        }
    }
    task printProperties{
    	doLast{
        	println "username = $user"
            println "password = $pass"
        }
    }
    
    ① 检查工程的属相是否存在
    ② 自定义任务打印是否存在属性值
    
    不用任何额外的配置而执行printProperties任务去给ext代码块中的变量去赋值，如（Exmaple 2-7）所示：
    Example 2-7.运行Gradle 打印出的ext代码块中所附的值
    >./gradlew printProperties
    :app:printProperties
    username = user_from_build_file
    password = pass_from_build_file
    
    如果是在工程项目的根目录下的gradle.properties文件中赋的值，那么打印结果就会不同了（如Example 2-8 和 2-9）.
    
    Example 2-8.使用grale.properties 设置用户名和密码的的值
    user = user_from_gradle_properties
    pass = pass_from_gradle_properties
    
    Example 2-9.使用gradle.properties给变量赋值 运行gradle的打印结果
    >./gradlew printProperties
    :app:printProperties
    username = user_from_gradle_properties
    password = pass_form_gradle_properties
    
    也可以使用命令行来进行赋值，但是需要提醒的是这种赋值的方式的优先级是最高的（如Example 2-10）
    
    Example 2-10 . 使用命令行进行赋值时运行gralde
    >./gradlew -Puser = user_from_pflag -Ppass = pass_from_pflag printProperties
    :app:printProperties
    username = user_from_pflag
    password = pass_from_pflag
    
    gralde提供ext代码块，属性文件，以及命令行标志，就是尽可能的希望能够给你足够多的选择空间来方便你的使用。
    
    最后提示： 自定义task会在Recipe4.1进行讲解。建立仓库是Recipe1.7的内容中会讲解
    
    
    
    2.2 从Eclipse开发 转向 Android Studio 开发
    
    问题：你可能想使用Android Studio工具导入Eclipse安卓项目。
    
    解决方案：
    Android studio提供了导入项目向导的功能，通过这个功能你可以导入已经存在的项目工程
    
    讨论：
    Figure 2-1 展示的是在Android Studio的欢迎页面有导入studio工程的快捷链接方式，这些工程即可以是Eclipse项目也可以是Gralde 构建的项目
    
![](https://github.com/challengemyself/android-recipies-for-android/blob/master/imgs/2017-03-26%2022-53-26figure2-1.png?raw=true)
    
    Figure 1-2 . Android studio 欢迎页展示了导入选项
    
    点击那个快捷方式会出现引导你选择已经存在于电脑上的Eclipse项目
    Figure 2-2 就是会展示这样一个项目。这个项目使用的老的工程结构，在它的根目录下有src res，以及Androidmanifest.xml文件
    当你选择了目标文件夹之后（向导是不会覆盖原始的工程的），向导还会自动帮你将lib文件夹下面的jar包的依赖转换成gradle文件中的依赖，在其他的选项中，如Figure2-3所示
    
![](https://github.com/challengemyself/android-recipies-for-android/blob/master/imgs/2017-03-26%2023-15-43-figure-2-2.png?raw=true)
    
    Figure 2-2,选择Eclipse 工程
    
![](https://github.com/challengemyself/android-recipies-for-android/blob/master/imgs/2017-03-26%2023-17-01-figure-2-3.png?raw=true)
    
   	Figure 2-3.导入工程选项
    
    
    
    想到在导入工程之后会重构工程的结构然后构建它。默认情况下，import-summary.txt 窗口会展示工程主要的改变。如Example2-11 所示
    ECLIPSE ANDROID PROJECT IMPORT SUMMARY
    ======================================
    Ignored Files:
    --------------
    The following files were *not* copied into the new Gradle project; you
    should evaluate whether these are still needed in your project and if
    so manually move them:
    * proguard-project.txt
    Moved Files:
    ------------
    Android Gradle projects use a different directory structure than ADT
    Eclipse projects. Here's how the projects were restructured:
    *
    *
    *
    *
    AndroidManifest.xml => app/src/main/AndroidManifest.xml
    assets/ => app/src/main/assets
    res/ => app/src/main/res/
    src/ => app/src/main/java/
    Next Steps:
    -----------
    You can now build the project. The Gradle project needs network
    connectivity to download dependencies.
    Bugs:
    -----
    If for some reason your project does not build, and you determine that
    it is due to a bug or limitation of the Eclipse to Gradle importer,
    please file a bug at http://b.android.com with category
    Component-Tools.
    (This import summary is for your information only, and can be deleted
    after import once you are satisfied with the results.)
    
    
    
    除了Proguard文件在着重说明之外，剩下的最大的变化就是文件的位置的改变。
    
    在这创建的一级目录下的gradle.build文件是和新创建的工程中的gradle.build文件是一样的，如Example 2-12
    
    
   	Example 2-12,一级创建的构建文件
    sub-projects/modules.
    buildscript {
        repositories {
            jcenter()
        }
        dependencies {
        	classpath 'com.android.tools.build:gradle:2.0.0'
        }
        }
        allprojects {
            repositories {
            	jcenter()
            }
    }
    在app文件夹下面有原始的工程文件，也会有一个很相似的gralde文件，如Example 2-13 所示
    
    