# JVM参数

## 概要-Synopsis

java \[options] classname \[args]
java \[options] -jar filename \[args]

**options 选项**
Command-line options separated by spaces. See Options.

**classname**
The name of the class to be launched. 要启动的类的名称

**filename**
The name of the Java Archive (JAR) file to be called. Used only with the -jar option.
要调用的 Java 归档 (JAR) 文件的名称。仅与-jar选项一起使用。\[一般通过打包jar文件形成jar包即可调用]

**args 参数**
The arguments passed to the main() method separated by space 传递给main()方法的参数以空格分隔。

## 描述 Description

The java command starts a Java application. It does this by starting the Java Runtime Environment (JRE), loading the specified class, and calling that class's main() method. The method must be declared `public and static`, it must not return any value, and it must accept a String array as a parameter. The method declaration has the following form:

` static void main(String[] args)`
The java command can be used to launch a `JavaFX application` by loading a class that either has a main() method or that extends `javafx.application.Application`. In the latter case, the launcher constructs an instance of the Application class, calls its `init()` method, and then calls the `start(javafx.stage.Stage)` method.

By default, the first argument that is not an option of the java command is the fully qualified name of the class to be called. If the -jar option is specified, its argument is the name of the JAR file containing class and resource files for the application. The startup class must be indicated by the Main-Class manifest header in its source code.

The JRE searches for the startup class (and other classes used by the application) in three sets of locations: the bootstrap class path, the installed extensions, and the user's class path.

Arguments after the class file name or the JAR file name are passed to the main() method.

这个描述只是简单的介绍main方法必须使用public 和 static 和 void 的形式

## Options 参数

The java command supports a wide range of options that can be divided into the following categories: 该java命令支持范围广泛的选项，可分为以下几类

*   [Standard Options](#Standard_Options)标准选项
*   [Non-Standard Options](#Non-Standard_Options)非标准选项
*   [Advanced Runtime Options](#dvanced_Runtime_Options)运行时选项
*   [Advanced JIT Compiler Options](#Advanced_JIT_Compiler_Options)JIT编译器选项
*   [Advanced Serviceability Options](#Advanced_Serviceability_Options)维护选项
*   [Advanced Garbage Collection Options](#Advanced_Garbage_Collection_Options)垃圾收集选项

Standard options are guaranteed to be supported by all implementations of the Java Virtual Machine (JVM). They are used for common actions, such as checking the version of the JRE, setting the class path, enabling verbose output, and so on.

Non-standard options are general purpose options that are specific to the Java HotSpot Virtual Machine, so they are not guaranteed to be supported by all JVM implementations, and are subject to change. These options start with -X.

Advanced options are not recommended for casual use. These are developer options used for tuning specific areas of the Java HotSpot Virtual Machine operation that often have specific system requirements and may require privileged access to system configuration parameters. They are also not guaranteed to be supported by all JVM implementations, and are subject to change. Advanced options start with -XX.

To keep track of the options that were deprecated or removed in the latest release, there is a section named Deprecated and Removed Options at the end of the document.

Boolean options are used to either enable a feature that is disabled by default or disable a feature that is enabled by default. Such options do not require a parameter. Boolean -XX options are enabled using the plus sign (-XX:+OptionName) and disabled using the minus sign (-XX:-OptionName).

For options that require an argument, the argument may be separated from the option name by a space, a colon (:), or an equal sign (=), or the argument may directly follow the option (the exact syntax differs for each option). If you are expected to specify the size in bytes, you can use no suffix, or use the suffix k or K for kilobytes (KB), m or M for megabytes (MB), g or G for gigabytes (GB). For example, to set the size to 8 GB, you can specify either 8g, 8192m, 8388608k, or 8589934592 as the argument. If you are expected to specify the percentage, use a number from 0 to 1 (for example, specify 0.25 for 25%).

Java 虚拟机 (JVM) 的所有实现都保证支持标准选项。它们用于常见操作，例如检查 JRE 的版本、设置类路径、启用详细输出等。

非标准选项是特定于 Java HotSpot 虚拟机的通用选项，因此不能保证所有 JVM 实现都支持它们，并且可能会发生变化。这些选项以-X.

不建议随意使用高级\[Advance]选项。这些是用于调整 Java HotSpot 虚拟机操作的特定区域的开发人员选项，这些区域通常具有特定的系统要求并且可能需要对系统配置参数的特权访问。它们也不保证所有 JVM 实现都支持，并且可能会发生变化。高级选项以 开头-XX。

为了跟踪最新版本中弃用或删除的选项，文档末尾有一个名为“弃用和删除的选项”的部分。

布尔选项用于启用默认禁用的功能或禁用默认启用的功能。此类选项不需要参数。布尔-XX选项使用加号 (-XX: +OptionName ) 启用，使用减号 (-XX: -OptionName )

对于需要参数的选项，参数可以通过空格、冒号 (:) 或等号 (=) 与选项名称分隔，或者参数可以直接跟在选项后面（每个选项的确切语法不同） ). 如果您希望以字节为单位指定大小，则可以不使用后缀，或者使用后缀或千字节 (KB)、兆字节 (MB) 或k千兆K字节( mGB )。例如，要将大小设置为 8 GB，您可以指定`8g,8192m,8388608k`或作`8589934592`为参数。如果您需要指定百分比，请使用 0 到 1 之间的数字（例如，指定`0.25` 作为25%）

<a id="Standard_Options"></a>**Standard Options**
These are the most commonly used options that are supported by all implementations of the JVM.
这些是 JVM 的所有实现都支持的最常用的选项。

*   **-agentlib\:libname\[=options]**
    Loads the specified native agent library. After the library name, a comma-separated list of options specific to the library can be used.加载指定的本机代理库。在库名称之后，可以使用以逗号分隔的特定于库的选项列表。

    If the option -agentlib\:foo is specified, then the JVM attempts to load the library named libfoo.so in the location specified by the LD\_LIBRARY\_PATH system variable (on OS X this variable is DYLD\_LIBRARY\_PATH).如果-agentlib\:foo指定了该选项，则 JVM 会尝试加载在系统变量libfoo.so指定的位置中命名的库LD\_LIBRARY\_PATH（在 OS X 上，此变量为DYLD\_LIBRARY\_PATH）。

    The following example shows how to load the heap profiling tool (HPROF) library and get sample CPU information every 20 ms, with a stack depth of 3:
    以下示例展示了如何加载堆分析工具 (HPROF) 库并每 20 毫秒获取一次示例 CPU 信息，堆栈深度为 3：

    \-agentlib\:hprof=cpu=samples,interval=20,depth=3
    The following example shows how to load the Java Debug Wire Protocol (JDWP) library and listen for the socket connection on port 8000, suspending the JVM before the main class loads:
    \-agentlib\:hprof=cpu=samples,interval=20,depth=3
    以下示例展示了如何加载 Java 调试线协议 (JDWP) 库并侦听端口 8000 上的套接字连接，从而在加载主类之前暂停 JVM：

    \-agentlib\:jdwp=transport=dt\_socket,server=y,address=8000
    For more information about the native agent libraries, refer to the following:
    \-agentlib\:jdwp=transport=dt\_socket,server=y,address=8000
    有关本地代理库的更多信息，请参阅以下内容：

    The java.lang.instrument package description at <http://docs.oracle.com/javase/8/docs/api/java/lang/instrument/package-summary.html>
    java.lang.instrument包裹说明在<http://docs.oracle.com/javase/8/docs/api/java/lang/instrument/package-summary.html>

    Agent Command Line Options in the JVM Tools Interface guide at <http://docs.oracle.com/javase/8/docs/platform/jvmti/jvmti.html#starting>
    JVM 工具界面指南中的代理命令行选项位于<http://docs.oracle.com/javase/8/docs/platform/jvmti/jvmti.html#starting>

*   **-agentpath\:pathname\[=options]**
    Loads the native agent library specified by the absolute path name. This option is equivalent to -agentlib but uses the full path and file name of the library.
    加载由绝对路径名指定的本机代理库。此选项等效于-agentlib但使用库的完整路径和文件名。

*   **-client**
    Selects the Java HotSpot Client VM. The 64-bit version of the Java SE Development Kit (JDK) currently ignores this option and instead uses the Server JVM.

    For default JVM selection, see Server-Class Machine Detection at
    <http://docs.oracle.com/javase/8/docs/technotes/guides/vm/server-class.html>
    选择 Java HotSpot 客户端 VM。64 位版本的 Java SE 开发工具包 (JDK) 目前忽略此选项，而是使用服务器 JVM。

    对于默认的 JVM 选择，请参阅服务器类机器检测，网址为
    <http://docs.oracle.com/javase/8/docs/technotes/guides/vm/server-class.html>

*   **-Dproperty=value**
    Sets a system property value. The property variable is a string with no spaces that represents the name of the property. The value variable is a string that represents the value of the property. If value is a string with spaces, then enclose it in quotation marks (for example -Dfoo="foo bar").
    设置系统属性值。属性变量是一个没有空格的字符串，表示属性的名称。value变量是表示属性值的字符串。如果值是带空格的字符串，则将其括在引号中（例如-Dfoo="foo bar"）。

*   **-d32**
    Runs the application in a 32-bit environment. If a 32-bit environment is not installed or is not supported, then an error will be reported. By default, the application is run in a 32-bit environment unless a 64-bit system is used.在 32 位环境中运行应用程序。如果没有安装或者不支持32位环境，那么就会报错。默认情况下，应用程序在 32 位环境中运行，除非使用 64 位系统。

*   **-d64**
    Runs the application in a 64-bit environment. If a 64-bit environment is not installed or is not supported, then an error will be reported. By default, the application is run in a 32-bit environment unless a 64-bit system is used.
    在 64 位环境中运行应用程序。如果没有安装或者不支持64位环境，那么就会报错。默认情况下，应用程序在 32 位环境中运行，除非使用 64 位系统。

    Currently only the Java HotSpot Server VM supports 64-bit operation, and the -server option is implicit with the use of -d64. The -client option is ignored with the use of -d64. This is subject to change in a future release.
    目前只有 Java HotSpot Server VM 支持 64 位操作，-server使用-d64. -client使用 时会忽略该选项-d64。这可能会在未来的版本中发生变化。

*   **-disableassertions\[:\[packagename]...|\:classname]**
    **-da\[:\[packagename]...|\:classname]**
    Disables assertions. By default, assertions are disabled in all packages and classes.
    禁用断言。默认情况下，断言在所有包和类中都是禁用的。

    With no arguments, -disableassertions (-da) disables assertions in all packages and classes. With the packagename argument ending in ..., the switch disables assertions in the specified package and any subpackages. If the argument is simply ..., then the switch disables assertions in the unnamed package in the current working directory. With the classname argument, the switch disables assertions in the specified class.
    如果没有参数，-disableassertions( -da) 将禁用所有包和类中的断言。使用以结尾的packagename...参数，开关禁用指定包和任何子包中的断言。如果参数是simply...的 ，那么该开关将禁用当前工作目录中未命名包中的断言。使用classname参数，开关禁用指定类中的断言。

    The -disableassertions (-da) option applies to all class loaders and to system classes (which do not have a class loader). There is one exception to this rule: if the option is provided with no arguments, then it does not apply to system classes. This makes it easy to disable assertions in all classes except for system classes. The -disablesystemassertions option enables you to disable assertions in all system classes.
    `-disableassertions (-da)`选项适用于所有类加载器和系统类（没有类加载器）。此规则有一个例外：如果提供的选项没有参数，则它不适用于系统类。这使得在除系统类之外的所有类中禁用断言变得容易。该`-disablesystemassertions`选项使您能够禁用所有系统类中的断言。

    To explicitly enable assertions in specific packages or classes, use the `-enableassertions (-ea)` option. Both options can be used at the same time. For example, to run the `MyClass` application with assertions enabled in package `com.wombat.fruitbat` (and any subpackages) but disabled in class com.wombat.fruitbat.Brickbat, use the following command:
    要在特定包或类中显式启用断言，请使用`-enableassertions( -ea)` 选项。两个选项可以同时使用。例如，要`MyClass`在包`com.wombat.fruitbat`（和任何子包）中启用断言但在类中禁用断言的情况下运行应用程序`com.wombat.fruitbat.Brickbat`，请使用以下命令：
    `java -ea:com.wombat.fruitbat... -da:com.wombat.fruitbat.Brickbat MyClass`

*   **-disablesystemassertions**
    **-dsa**
    Disables assertions in all system classes.
    在所有系统类中禁用断言。

*   **-enableassertions\[:\[packagename]...|\:classname]**
    **-ea\[:\[packagename]...|\:classname]**
    Enables assertions. By default, assertions are disabled in all packages and classes.
    启用断言。默认情况下，断言在所有包和类中都是禁用的。

    With no arguments, `-enableassertions (-ea)` enables assertions in all packages and classes. With the `packagename` argument ending in ..., the switch enables assertions in the specified package and any subpackages. If the argument is `simply ...`, then the switch enables assertions in the unnamed package in the current working directory. With the classname argument, the switch enables assertions in the specified class.

    没有参数，`-enableassertions( -ea)` 在所有包和类中启用断言。使用以结尾的`packagename...`参数，开关启用指定包和任何子包中的断言。如果参数是`simply...`的，则开关在当前工作目录的未命名包中启用断言。使用类名参数，开关启用指定类中的断言。

    The -enableassertions (-ea) option applies to all class loaders and to system classes (which do not have a class loader). There is one exception to this rule: if the option is provided with no arguments, then it does not apply to system classes. This makes it easy to enable assertions in all classes except for system classes. The -enablesystemassertions option provides a separate switch to enable assertions in all system classes.
    `-enableassertions (-ea) ` 选项适用于所有类加载器和系统类（没有类加载器）。此规则有一个例外：如果提供的选项没有参数，则它不适用于系统类。这使得在除系统类之外的所有类中启用断言变得容易。该`-enablesystemassertions`选项提供了一个单独的开关来启用所有系统类中的断言。

    To explicitly disable assertions in specific packages or classes, use the -disableassertions (-da) option. If a single command contains multiple instances of these switches, then they are processed in order before loading any classes. For example, to run the MyClass application with assertions enabled only in package com.wombat.fruitbat (and any subpackages) but disabled in class com.wombat.fruitbat.Brickbat, use the following command:
    `java -ea:com.wombat.fruitbat... -da:com.wombat.fruitbat.Brickbat MyClass`
    要显式禁用特定包或类中的断言，请使用`-disableassertions( -da)` 选项。如果单个命令包含这些开关的多个实例，则在加载任何类之前按顺序处理它们。例如，要运行仅在包（和任何子包）`MyClass`中启用但在类中禁用断言的应用程序，请使用以下命令：
    `java -ea:com.wombat.fruitbat... -da:com.wombat.fruitbat.Brickbat MyClass`

*   **-enablesystemassertions**
    **-esa**
    Enables assertions in all system classes.
    在所有系统类中启用断言。

*   **-help**
    **-?**
    Displays usage information for the java command without actually running the JVM.
    java在不实际运行 JVM的情况下显示命令的使用信息。

*   **-jar filename**
    Executes a program encapsulated in a JAR file. The filename argument is the name of a JAR file with a manifest that contains a line in the form `Main-Class:classname` that defines the class with the `public static void main(String[] args)` method that serves as your application's starting point.

    执行封装在 JAR 文件中的程序。文件名参数是带有清单的 JAR 文件的名称，清单中包含一行，该行定义`Main-Class:classname`类和`public static void main(String[] args)`用作应用程序起点的方法。

    When you use the -jar option, the specified JAR file is the source of all user classes, and other class path settings are ignored.
    当您使用该-jar选项时，指定的 JAR 文件是所有用户类的源，其他类路径设置将被忽略。

    For more information about JAR files, see the following resources:

    *   jar(1)

    *   The Java Archive (JAR) Files guide at Java 存档 (JAR) 文件指南位于 <http://docs.oracle.com/javase/8/docs/technotes/guides/jar/index.html>

    *   Lesson: Packaging Programs in JAR Files at 在 JAR 文件中打包程序 <http://docs.oracle.com/javase/tutorial/deployment/jar/index.html>

*   **-javaagent\:jarpath\[=options]**
    Loads the specified Java programming language agent. For more information about instrumenting Java applications, see the java.lang.instrument package description in the Java API documentation at <http://docs.oracle.com/javase/8/docs/api/java/lang/instrument/package-summary.html>

    加载指定的 Java 编程语言代理。有关检测 Java 应用程序的更多信息，请参阅java.lang.instrumentJava API 文档中的包说明，网址为<http://docs.oracle.com/javase/8/docs/api/java/lang/instrument/package-summary.html>

*   **-jre-restrict-search**
    Includes user-private JREs in the version search.
    在版本搜索中包括用户私有的 JRE。

*   **-no-jre-restrict-search**
    Excludes user-private JREs from the version search.
    从版本搜索中排除用户私有的 JRE。

*   **-server**
    Selects the Java HotSpot Server VM. The 64-bit version of the JDK supports only the Server VM, so in that case the option is implicit.
    选择 Java HotSpot 服务器 VM。64 位版本的 JDK 仅支持服务器 VM，因此在这种情况下该选项是隐式的。

    For default JVM selection, see Server-Class Machine Detection at <http://docs.oracle.com/javase/8/docs/technotes/guides/vm/server-class.html>
    对于默认的 JVM 选择，请参阅服务器类机器检测，网址为
    <http://docs.oracle.com/javase/8/docs/technotes/guides/vm/server-class.html>

*   **-showversion**
    Displays version information and continues execution of the application. This option is equivalent to the -version option except that the latter instructs the JVM to exit after displaying version information.
    显示版本信息并继续执行应用程序。该选项等同于-version选项，只是后者指示 JVM 在显示版本信息后退出。

*   **-splash\:imgname**
    Shows the splash screen with the image specified by `imgname`. For example, to show the splash.gif file from the images directory when starting your application, use the following option:
    使用imgname指定的图像显示初始屏幕。例如，要在启动应用程序时显示目录中的splash.gif文件，请使用以下选项：

          `-splash:images/splash.gif`

*   **-verbose\:class**
    Displays information about each loaded class.
    显示有关每个已加载类的信息。

*   **-verbose\:gc**
    Displays information about each garbage collection `(GC)` event.
    显示有关每个垃圾回收 (GC) 事件的信息。

*   **-verbose\:jni**
    Displays information about the use of native methods and other Java Native Interface (JNI) activity.
    显示有关使用本机方法和其他 Java 本机接口 (JNI) 活动的信息。

*   **-version**
    Displays version information and then exits. This option is equivalent to the -showversion option except that the latter does not instruct the JVM to exit after displaying version information.
    显示版本信息然后退出。该选项等同于-showversion选项，只是后者不指示 JVM 在显示版本信息后退出。

*   **-version\:release**
    Specifies the release version to be used for running the application. If the version of the java command called does not meet this specification and an appropriate implementation is found on the system, then the appropriate implementation will be used.
    指定要用于运行应用程序的发行版本。如果调用的java命令版本不符合此规范，并且在系统上找到了适当的实现，则将使用适当的实现。

    The release argument specifies either the exact version string, or a list of version strings and ranges separated by spaces. A version string is the developer designation of the version number in the following form: 1.x.0\_u (where x is the major version number, and u is the update version number). A version range is made up of a version string followed by a plus sign (+) to designate this version or later, or a part of a version string followed by an asterisk (\*) to designate any version string with a matching prefix. Version strings and ranges can be combined using a space for a logical OR combination, or an ampersand (&) for a logical AND combination of two version strings/ranges. For example, if running the class or JAR file requires either JRE 6u13 (1.6.0\_13), or any JRE 6 starting from 6u10 (1.6.0\_10), specify the following:
    release参数指定确切的版本字符串，或由空格分隔的版本字符串和范围列表。版本字符串是开发者对版本号的指定，格式如下：1.x .0\_u（其中x是主版本号，u是更新版本号）。版本范围由版本字符串后跟加号 ( +) 组成，以指定此版本或更高版本，或者版本字符串的一部分后跟星号 ( \*) 以指定具有匹配前缀的任何版本字符串。版本字符串和范围可以使用用于逻辑或组合的空格或与符号 (&) 用于两个版本字符串/范围的逻辑AND组合。例如，如果运行类或 JAR 文件需要 JRE 6u13 (1.6.0\_13) 或从 6u10 (1.6.0\_10) 开始的任何 JRE 6，请指定以下内容：

          `-version:"1.6.0_13 1.6* & 1.6.0_10+"`

    Quotation marks are necessary only if there are spaces in the release parameter.
    仅当发布参数中有空格时才需要引号。

    For JAR files, the preference is to specify version requirements in the JAR file manifest rather than on the command line.
    对于 JAR 文件，首选是在 JAR 文件清单中而不是在命令行中指定版本要求。

<a id="Non-Standard_Options"></a>**Non-Standard Options**
这些选项是特定于 Java HotSpot 虚拟机的通用选项。
- *-X*
    Displays help for all available -X options.
显示所有可用-X选项的帮助。
- **-Xbatch**
    Disables background compilation. By default, the JVM compiles the method as a background task, running the method in interpreter mode until the background compilation is finished. The `-Xbatch` flag disables background compilation so that compilation of all methods proceeds as a foreground task until completed.
    禁用后台编译。默认情况下，JVM 将方法编译为后台任务，以解释器模式运行方法，直到后台编译完成。该`-Xbatch`标志禁用后台编译，以便所有方法的编译作为前台任务进行，直到完成。
    This option is equivalent to `-XX:-BackgroundCompilation.`  
该选项等同于 `-XX:-BackgroundCompilation.`  
- **-Xbootclasspath:path**
    Specifies a list of directories, JAR files, and ZIP archives separated by colons (:) to search for boot class files. These are used in place of the boot class files included in the JDK.
    指定以冒号 (:) 分隔的目录、JAR 文件和 ZIP 归档的列表，以搜索引导类文件。这些用于代替 JDK 中包含的引导类文件。

    Do not deploy applications that use this option to override a class in rt.jar, because this violates the JRE binary code license.
    不要部署使用此选项覆盖 中的类的应用程序rt.jar，因为这违反了 JRE 二进制代码许可。
- **-Xbootclasspath/a:path**
    Specifies a list of directories, JAR files, and ZIP archives separated by colons (:) to append to the end of the default bootstrap class path.
    指定以冒号 (:) 分隔的目录、JAR 文件和 ZIP 归档的列表，以附加到默认引导程序类路径的末尾。

    Do not deploy applications that use this option to override a class in rt.jar, because this violates the JRE binary code license.
    不要部署使用此选项覆盖 中的类的应用程序rt.jar，因为这违反了 JRE 二进制代码许可。
- **-Xbootclasspath/p:path**
    Specifies a list of directories, JAR files, and ZIP archives separated by colons (:) to prepend to the front of the default bootstrap class path.
    指定以冒号 (:) 分隔的目录、JAR 文件和 ZIP 归档的列表，以添加到默认引导程序类路径的前面。

    Do not deploy applications that use this option to override a class in rt.jar, because this violates the JRE binary code license.
    不要部署使用此选项覆盖 中的类的应用程序rt.jar，因为这违反了 JRE 二进制代码许可。
- **-Xcheck:jni**
    Performs additional checks for Java Native Interface (JNI) functions. Specifically, it validates the parameters passed to the JNI function and the runtime environment data before processing the JNI request. Any invalid data encountered indicates a problem in the native code, and the JVM will terminate with an irrecoverable error in such cases. Expect a performance degradation when this option is used.
对 Java 本机接口 (JNI) 函数执行额外检查。具体来说，它在处理 JNI 请求之前验证传递给 JNI 函数的参数和运行时环境数据。遇到任何无效数据都表明本机代码存在问题，在这种情况下，JVM 将终止并出现不可恢复的错误。使用此选项时预计性能会下降。
- **-Xcomp**
    Forces compilation of methods on first invocation. By default, the Client VM (`-client`) performs 1,000 interpreted method invocations and the Server VM (`-server`) performs 10,000 interpreted method invocations to gather information for efficient compilation. Specifying the `-Xcomp` option disables interpreted method invocations to increase compilation performance at the expense of efficiency.
    在第一次调用时强制编译方法。默认情况下，客户端 VM ( `-client`) 执行 1,000 次解释方法调用，服务器 VM ( `-server`) 执行 10,000 次解释方法调用以收集信息以进行高效编译。指定该-Xcomp选项会禁用解释方法调用，以牺牲效率为代价提高编译性能。

    You can also change the number of interpreted method invocations before compilation using the `-XX:CompileThreshold` option.
    您还可以使用该`-XX:CompileThreshold`选项在编译之前更改解释方法调用的数量。
- **-Xdebug**
    Does nothing. Provided for backward compatibility.
    什么也没做。提供向后兼容性。
- **-Xdiag**
    Shows additional diagnostic messages.
    显示额外的诊断信息。
- **-Xfuture**
    Enables strict class-file format checks that enforce close conformance to the class-file format specification. Developers are encouraged to use this flag when developing new code because the stricter checks will become the default in future releases.
    启用严格的类文件格式检查，强制严格遵守类文件格式规范。鼓励开发人员在开发新代码时使用此标志，因为更严格的检查将成为未来版本中的默认设置。
- **-Xint**
    Runs the application in interpreted-only mode. Compilation to native code is disabled, and all bytecode is executed by the interpreter. The performance benefits offered by the just in time (JIT) compiler are not present in this mode.
    以仅解释模式运行应用程序。禁止编译为本机代码，所有字节码都由解释器执行。即时 (JIT) 编译器提供的性能优势在此模式中不存在。
- **-Xinternalversion**
    Displays more detailed JVM version information than the `-version` option, and then exits.
    显示比-version选项更详细的 JVM 版本信息，然后退出。
- **-Xloggc:filename**
    Sets the file to which verbose GC events information should be redirected for logging. The information written to this file is similar to the output of `-verbose:gc` with the time elapsed since the first GC event preceding each logged event. The `-Xloggc` option overrides `-verbose:gc` if both are given with the same java command.
    设置详细 GC 事件信息应重定向到的文件以进行日志记录。写入此文件的信息类似于-verbose:gc自每个记录事件之前的第一个 GC 事件以来经过的时间的输出。
    如果两者都使用相同的java命令，则该`-Xloggc`选项会覆盖`-verbose:gc`
    Example:
        `-Xloggc:garbage-collection.log`
- **-Xmaxjitcodesize=size**
    Specifies the maximum code cache size (in bytes) for JIT-compiled code. Append the letter `k` or `K` to indicate kilobytes, `m` or `M` to indicate megabytes, `g` or `G` to indicate gigabytes. The default maximum code cache size is 240 MB; if you disable tiered compilation with the option `-XX:-TieredCompilation`, then the default size is 48 MB:
        `-Xmaxjitcodesize=240m`
    指定 JIT 编译代码的最大代码缓存大小（以字节为单位）。附加字母k或K表示千字节，m或M表示兆字节，g或G表示千兆字节。默认最大代码缓存大小为 240 MB；如果您使用选项禁用分层编译`-XX:-TieredCompilation`，则默认大小为 48 MB：
        `-Xmaxjitcodesize=240m`

    This option is equivalent to `-XX:ReservedCodeCacheSize`.
    该选项等同于`-XX:ReservedCodeCacheSize`.
- **-Xmixed**
    Executes all bytecode by the interpreter except for hot methods, which are compiled to native code.
    解释器执行所有字节码，热方法除外，这些方法被编译为本机代码。

- **-Xmnsize**
    Sets the initial and maximum size (in bytes) of the heap for the young generation (nursery). Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes.

    The young generation region of the heap is used for new objects. GC is performed in this region more often than in other regions. If the size for the young generation is too small, then a lot of minor garbage collections will be performed. If the size is too large, then only full garbage collections will be performed, which can take a long time to complete. Oracle recommends that you keep the size for the young generation between a half and a quarter of the overall heap size.

    The following examples show how to set the initial and maximum size of young generation to 256 MB using various units:
    为新生代（nursery）设置堆的初始大小和最大大小（以字节为单位）。附加字母k或K表示千字节，m或M表示兆字节，g或G表示千兆字节。

    堆的年轻代区域用于新对象。GC 在这个区域比在其他区域更频繁地执行。如果新生代的大小太小，那么会执行很多小的垃圾收集。如果大小太大，则只会执行完整的垃圾收集，这可能需要很长时间才能完成。Oracle 建议您将新生代的大小保持在整个堆大小的一半到四分之一之间。

    以下示例显示如何使用各种单位将年轻代的初始大小和最大大小设置为 256 MB：

        `-Xmn256m` 
        `-Xmn262144k`
        `-Xmn268435456`
    Instead of the `-Xmn` option to set both the initial and maximum size of the heap for the young generation, you can use `-XX:NewSize` to set the initial size and `-XX:MaxNewSize` to set the maximum size.
    您可以使用`-XX:NewSize`设置初始大小和最大大小来代替为年轻代设置堆的初始大小和最大`-XX:MaxNewSize`大小的选项。
- **-Xmssize**
    Sets the minimum and the initial size (in bytes) of the heap. This value must be a multiple of 1024 and greater than 1 MB. Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes.

    The following examples show how to set the size of allocated memory to 6 MB using various units:

        `-Xms6291456`
        `-Xms6144k`
        `-Xms6m`
    If you do not set this option, then the initial size will be set as the sum of the sizes allocated for the old generation and the young generation. The initial size of the heap for the young generation can be set using the `-Xmn` option or the `-XX:NewSize option`.

    Note that the `-XX:InitalHeapSize` option can also be used to set the initial heap size. If it appears after -Xms on the command line, then the initial heap size gets set to the value specified with `-XX:InitalHeapSize`.
    设置堆的最小和初始大小（以字节为单位）。此值必须是 1024 的倍数且大于 1 MB。附加字母k或K表示千字节，m或M表示兆字节，g或G表示千兆字节。

    以下示例显示如何使用各种单位将已分配内存的大小设置为 6 MB：

    `-Xms6291456 `
    `-Xms6144k `
    `-Xms6m`
    如果你不设置这个选项，那么初始大小将被设置为分配给老年代和年轻代的大小之和。可以使用-Xmn 选项或`-XX:NewSize` 选项设置年轻代堆的初始大小。

    请注意，该`-XX:InitalHeapSize` 选项也可用于设置初始堆大小。如果它出现在`-Xms` 命令行之后，则初始堆大小将设置为使用`-XX:InitalHeapSize`指定的值。
- **-Xmxsize**
    Specifies the maximum size (in bytes) of the memory allocation pool in bytes. This value must be a multiple of 1024 and greater than 2 MB. Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes. The default value is chosen at runtime based on system configuration. For server deployments, `-Xms` and `-Xmx` are often set to the same value. See the section "Ergonomics" in Java SE HotSpot Virtual Machine Garbage Collection Tuning Guide at http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/index.html.
    以字节为单位指定内存分配池的最大大小（以字节为单位）。此值必须是 1024 的倍数且大于 2 MB。附加字母k或K表示千字节，m或M表示兆字节，g或G表示千兆字节。默认值是在运行时根据系统配置选择的。对于服务器部署，-Xms通常-Xmx设置为相同的值。请参阅Java SE HotSpot 虚拟机垃圾收集调优指南中的“人体工程学”部分http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/index.html。

    The following examples show how to set the maximum allowed size of allocated memory to 80 MB using various units:

        `-Xmx83886080`
        `-Xmx81920k`
        `-Xmx80m`
        
    以下示例显示如何使用各种单位将分配内存的最大允许大小设置为 80 MB：
        `-Xmx83886080 `
        `-Xmx81920k `
        `-Xmx80m`  
    该`Xmx`选项相当于`-XX:MaxHeapSize`.

    The `-Xmx` option is equivalent to `XX:MaxHeapSize`.
- **-Xnoclassgc**
    Disables garbage collection (GC) of classes. This can save some GC time, which shortens interruptions during the application run.

    When you specify -Xnoclassgc at startup, the class objects in the application will be left untouched during GC and will always be considered live. This can result in more memory being permanently occupied which, if not used carefully, will throw an out of memory exception.
    禁用类的垃圾回收 (GC)。这可以节省一些 GC 时间，从而缩短应用程序运行期间的中断。

    当您`-Xnoclassgc`在启动时指定时，应用程序中的类对象将在 GC 期间保持不变，并且始终被视为活动的。这会导致更多的内存被永久占用，如果不小心使用，将会抛出内存不足异常。
- **-Xprof**
    Profiles the running program and sends profiling data to standard output. This option is provided as a utility that is useful in program development and is not intended to be used in production systems.
    分析正在运行的程序并将分析数据发送到标准输出。此选项作为实用程序提供，在程序开发中很有用，并不打算在生产系统中使用。
- **-Xrs**
    Reduces the use of operating system signals by the JVM.
    VM 对操作系统信号的使用。

    Shutdown hooks enable orderly shutdown of a Java application by running user cleanup code (such as closing database connections) at shutdown, even if the JVM terminates abruptly.
    关闭挂钩通过在关闭时运行用户清理代码（例如关闭数据库连接）来实现 Java 应用程序的有序关闭，即使 JVM 突然终止也是如此。

    The JVM catches signals to implement shutdown hooks for unexpected termination. The JVM uses SIGHUP, SIGINT, and SIGTERM to initiate the running of shutdown hooks.
    JVM 捕获信号以实现意外终止的关闭挂钩。JVM 使`SIGHUP`、`SIGINT`和`SIGTERM`启动关闭挂钩的运行。

    The JVM uses a similar mechanism to implement the feature of dumping thread stacks for debugging purposes. The JVM uses SIGQUIT to perform thread dumps.
    JVM 使用类似的机制来实现转储线程堆栈以进行调试的功能。JVM 用于`SIGQUIT`执行线程转储。
    

    Applications embedding the JVM frequently need to trap signals such as SIGINT or SIGTERM, which can lead to interference with the JVM signal handlers. The -Xrs option is available to address this issue. When -Xrs is used, the signal masks for SIGINT, SIGTERM, SIGHUP, and SIGQUIT are not changed by the JVM, and signal handlers for these signals are not installed.

    There are two consequences of specifying -Xrs:

        - SIGQUIT thread dumps are not available.

        - User code is responsible for causing shutdown hooks to run, for example, by calling System.exit() when the JVM is to be terminated.
    嵌入 JVM 的应用程序经常需要捕获 SIGINT 或 SIGTERM 等信号，这可能会干扰 JVM 信号处理程序。 -Xrs 选项可用于解决此问题。 当使用 -Xrs 时，JVM 不会更改 SIGINT、SIGTERM、SIGHUP 和 SIGQUIT 的信号掩码，并且不会安装这些信号的信号处理程序。

    指定 有两个后果-Xrs：

        - `SIGQUIT`线程转储不可用。

        - 用户代码负责使关闭挂钩运行，例如，通过`System.exit()`在 JVM 终止时调用

- **-Xshare:mode**
    Sets the class data sharing (CDS) mode. Possible mode arguments for this option include the following:
设置类数据共享 (CDS) 模式。此选项的可能模式参数包括以下内容：
    **auto**
    Use CDS if possible. This is the default value for Java HotSpot 32-Bit Client VM.
    尽可能使用 CDS。这是 Java HotSpot 32 位客户端 VM 的默认值。

    **on**
    Require the use of CDS. Print an error message and exit if class data sharing cannot be used.
    需要使用 CDS。如果无法使用类数据共享，则打印错误消息并退出。

    **off**
    Do not use CDS. This is the default value for Java HotSpot 32-Bit Server VM, Java HotSpot 64-Bit Client VM, and Java HotSpot 64-Bit Server VM.
    不要使用 CDS。这是 Java HotSpot 32 位服务器 VM、Java HotSpot 64 位客户端 VM 和 Java HotSpot 64 位服务器 VM 的默认值。

    **dump**
    Manually generate the CDS archive. Specify the application class path as described in "Setting the Class Path".
    手动生成 CDS 存档。如“设置类路径”中所述指定应用程序类路径。

    You should regenerate the CDS archive with each new JDK release.
    您应该为每个新的 JDK 版本重新生成 CDS 存档。
    
- **-XshowSettings:category**
    Shows settings and continues. Possible category arguments for this option include the following:
    显示设置并继续。此选项的可能类别参数包括以下内容：

   **all**
    Shows all categories of settings. This is the default value.
    显示所有类别的设置。这是默认值。

    **locale**
    Shows settings related to locale.
    显示与语言环境相关的设置。

    **properties**
    Shows settings related to system properties.
    显示与系统属性相关的设置。
    
    **vm**
    Shows the settings of the JVM.
    显示 JVM 的设置。

- **-Xsssize**
    Sets the thread stack size (in bytes). Append the letter k or K to indicate KB, m or M to indicate MB, g or G to indicate GB. The default value depends on the platform:
    设置线程堆栈大小（以字节为单位）。附加字母k或K表示KB，m或M表示MB，g或G表示GB。默认值取决于平台：

    - Linux/ARM (32-bit): 320 KB
    
    - Linux/i386 (32-bit): 320 KB
    
    - Linux/x64 (64-bit): 1024 KB
    
    - OS X (64-bit): 1024 KB
    
    - Oracle Solaris/i386 (32-bit): 320 KB
    
    - Oracle Solaris/x64 (64-bit): 1024 KB
    
    The following examples set the thread stack size to 1024 KB in different units:

        -Xss1m
        -Xss1024k
        -Xss1048576
    This option is equivalent to `-XX:ThreadStackSize`.
    该选项等同于`-XX:ThreadStackSize`.

- **-Xusealtsigs**
    Use alternative signals instead of SIGUSR1 and SIGUSR2 for JVM internal signals. This option is equivalent to `-XX:+UseAltSigs`.
    使用替代信号代替JVM 内部信号SIGUSR1和SIGUSR2。该选项等同于`-XX:+UseAltSigs`.

- **-Xverify:mode**
    Sets the mode of the bytecode verifier. Bytecode verification ensures that class files are properly formed and satisfy the constraints listed in section 4.10, Verification of class Files in the The Java Virtual Machine Specification.

    Do not turn off verification as this reduces the protection provided by Java and could cause problems due to ill-formed class files.
    设置字节码验证器的模式。字节码验证确保类文件的格式正确，并满足Java 虚拟机规范中第 4.10 节“文件验证class”中列出的约束。

    不要关闭验证，因为这会降低 Java 提供的保护，并可能由于格式错误的类文件而导致问题。

    此选项的可能模式参数包括以下内容：

    Possible mode arguments for this option include the following:
        **remote**  
            Verifies all bytecodes not loaded by the bootstrap class loader. This is the default behavior if you do not specify the `-Xverify` option.
            验证引导类加载器未加载的所有字节码。如果您未指定该`-Xverify`选项，则这是默认行为。
        **all**  
            Enables verification of all bytecodes.
            启用对所有字节码的验证。
        **none**  
             Disables verification of all bytecodes. Use of `-Xverify:none` is unsupported
             禁用所有字节码的验证。`-Xverify:none`不支持使用。

<a id="dvanced_Runtime_Options"></a>**Advanced Runtime Options**
These options control the runtime behavior of the Java HotSpot VM.
这些选项控制 Java HotSpot VM 的运行时行为。
- **-XX:+CheckEndorsedAndExtDirs**
    Enables the option to prevent the java command from running a Java application if it uses the endorsed-standards override mechanism or the extension mechanism. This option checks if an application is using one of these mechanisms by checking the following:
    启用该选项以防止java命令运行 Java 应用程序（如果它使用认可的标准覆盖机制或扩展机制）。此选项通过检查以下内容来检查应用程序是否正在使用这些机制之一
    - The java.ext.dirs or java.endorsed.dirs system property is set.
    设置了 java.ext.dirs 或 java.endorsed.dirs 系统属性。
    - The lib/endorsed directory exists and is not empty.
    该lib/endorsed目录存在且不为空。
    - The lib/ext directory contains any JAR files other than those of the JDK
    该lib/ext目录包含除 JDK 之外的任何 JAR 文件。
    - The system-wide platform-specific extension directory contains any JAR files.
    系统范围的特定于平台的扩展目录包含任何 JAR 文件。

- **-XX:+DisableAttachMechanism**
    Enables the option that disables the mechanism that lets tools attach to the JVM. By default, this option is disabled, meaning that the attach mechanism is enabled and you can use tools such as `jcmd`, `jstack`, `jmap`, and `jinfo`.
    启用 禁用允许工具附加到 JVM 的机制的选项。默认情况下，此选项处于禁用状态，这意味着附加机制已启用，您可以使用jcmd、jstack、jmap和等工具jinfo。

- **-XX:ErrorFile=filename**
    Specifies the path and file name to which error data is written when an irrecoverable error occurs. By default, this file is created in the current working directory and named hs_err_pidpid.log where pid is the identifier of the process that caused the error. The following example shows how to set the default log file (note that the identifier of the process is specified as %p):
    指定发生不可恢复错误时写入错误数据的路径和文件名。默认情况下，此文件在当前工作目录中创建并命名为hs_err_pidpid.log，其中pid是导致错误的进程的标识符。以下示例显示了如何设置默认日志文件（请注意，进程的标识符指定为%p)
        `-XX:ErrorFile=./hs_err_pid%p.log`
    The following example shows how to set the error log to /var/log/java/java_error.log:
    以下示例显示如何将错误日志设置为/var/log/java/java_error.log：
        `-XX:ErrorFile=/var/log/java/java_error.log`
    If the file cannot be created in the specified directory (due to insufficient space, permission problem, or another issue), then the file is created in the temporary directory for the operating system. The temporary directory is /tmp.
    如果无法在指定目录中创建文件（由于空间不足、权限问题或其他问题），则会在操作系统的临时目录中创建文件。临时目录是/tmp.

- **-XX:+FailOverToOldVerifier**
    Enables automatic failover to the old verifier when the new type checker fails. By default, this option is disabled and it is ignored (that is, treated as disabled) for classes with a recent bytecode version. You can enable it for classes with older versions of the bytecode.
    当新类型检查器失败时，启用自动故障转移到旧验证器。默认情况下，此选项是禁用的，对于具有最新字节码版本的类，它会被忽略（即被视为禁用）。您可以为具有旧版本字节码的类启用它。

- **-XX:+FlightRecorder**
    Enables the use of the Java Flight Recorder (JFR) during the runtime of the application. This is a commercial feature that works in conjunction with the `-XX:+UnlockCommercialFeatures` option as follows:
    允许在应用程序运行时使用 Java 飞行记录器 (JFR)。这是与以下-XX:+UnlockCommercialFeatures选项结合使用的商业功能：
        `java -XX:+UnlockCommercialFeatures -XX:+FlightRecorder`
    If this option is not provided, Java Flight Recorder can still be enabled in a running JVM by providing the appropriate jcmd diagnostic commands.
    如果未提供此选项，Java Flight Recorder 仍然可以通过提供适当的jcmd诊断命令在正在运行的 JVM 中启用。
- **-XX:-FlightRecorder**
    Disables the use of the Java Flight Recorder (JFR) during the runtime of the application. This is a commercial feature that works in conjunction with the `-XX:+UnlockCommercialFeatures` option as follows:
在应用程序运行时禁用 Java 飞行记录器 (JFR)。这是与以下-XX:+UnlockCommercialFeatures选项结合使用的商业功能：
        `java -XX:+UnlockCommercialFeatures -XX:-FlightRecorder`
    If this option is provided, Java Flight Recorder cannot be enabled in a running JVM.
    如果提供此选项，则无法在正在运行的 JVM 中启用 Java Flight Recorder。

- **-XX:FlightRecorderOptions=parameter=value**
    Sets the parameters that control the behavior of JFR. This is a commercial feature that works in conjunction with the `-XX:+UnlockCommercialFeatures` option. This option can be used only when JFR is enabled (that is, the -XX:+FlightRecorder option is specified).
    设置控制 JFR 行为的参数。 这是一项与“-XX:+UnlockCommercialFeatures”选项结合使用的商业功能。 只有在启用 JFR 时（即指定` -XX:+FlightRecorder `选项）才能使用此选项。
    The following list contains all available JFR parameters:
    以下列表包含所有可用的 JFR 参数

    - **defaultrecording={true|false}**
    Specifies whether a default continuous recording should be started for the Java application. By default, this parameter is set to false. To start a default recording automatically, set the parameter to true.
    指定是否应为 Java 应用程序启动默认连续记录。默认情况下，此参数设置为false。要自动开始默认录制，请将参数设置为true。

    - **disk={true|false}**
    Specifies whether to write temporary data to the disk repository. By default, this parameter is set to false. To enable it, set the parameter to true.
    指定是否将临时数据写入磁盘存储库。默认情况下，此参数设置为false。要启用它，请将参数设置为true。

    - **dumponexit={true|false}**
    Specifies whether a dump file of JFR data should be generated when the JVM terminates in a controlled manner. By default, this parameter is set to false (dump file on exit is not generated). To enable it, set the parameter to true, and also set `defaultrecording=true`.
    The dump file is written to the location defined by the dumponexitpath parameter.
    指定当 JVM 以受控方式终止时是否应生成 JFR 数据的转储文件。默认情况下，此参数设置为false（不生成退出时的转储文件）。要启用它，请将参数设置为true，并设置`defaultrecording=true`。

        转储文件写入`dumponexitpath`参数定义的位置。

    - **dumponexitpath=path**
    Specifies the path and name of the dump file with JFR data that is created when the JVM exits in a controlled manner if you set the `dumponexit=true` parameter. Setting the path makes sense only if you also set `defaultrecording=true`.
    如果设置了“dumponexit=true”参数，则指定 JVM 以受控方式退出时创建的带有 JFR 数据的转储文件的路径和名称。 仅当您还设置了`defaultrecording=true`时，设置路径才有意义。

        If the specified path is a directory, the JVM assigns a file name that shows the creation date and time. If the specified path includes a file name and if that file already exists, the JVM creates a new file by appending the date and time stamp to the specified file name.
        如果指定的路径是一个目录，JVM 会分配一个显示创建日期和时间的文件名。如果指定的路径包含文件名并且该文件已经存在，则 JVM 通过将日期和时间戳附加到指定的文件名来创建一个新文件。

    - **globalbuffersize=size**
        Specifies the total amount of primary memory (in bytes) used for data retention. Append k or K, to specify the size in KB, m or M to specify the size in MB, g or G to specify the size in GB. By default, the size is set to 462848 bytes.
        指定用于数据保留的主内存总量（以字节为单位）。追加k或K, 以 KB为单位指定大小，m或M以 MB为单位指定大小，g或G以 GB 为单位指定大小。默认情况下，大小设置为 462848 字节。

    - **loglevel={quiet|error|warning|info|debug|trace}**
        Specify the amount of data written to the log file by JFR. By default, it is set to info.
    指定 JFR 写入日志文件的数据量。默认情况下，它设置为info。

    - **maxage=time**
        Specifies the maximum age of disk data to keep for the default recording. Append s to specify the time in seconds, m for minutes, h for hours, or d for days (for example, specifying 30s means 30 seconds). By default, the maximum age is set to 15 minutes (15m).

        This parameter is valid only if you set the `disk=true` parameter.
    
        指定为默认记录保留的磁盘数据的最长期限。附加s以指定时间（以秒、m分钟、h小时或d天为单位）（例如，指定30s表示 30 秒）。默认情况下，最长期限设置为 15 分钟 ( 15m)。

        只有设置了该disk=true参数，该参数才有效。

    - **maxchunksize=size**
        Specifies the maximum size (in bytes) of the data chunks in a recording. Append k or K, to specify the size in KB, m or M to specify the size in MB, g or G to specify the size in GB. By default, the maximum size of data chunks is set to 12 MB.
        指定记录中数据块的最大大小（以字节为单位）。追加k或K, 以 KB为单位指定大小，m或M以 MB为单位指定大小，g或G以 GB 为单位指定大小。默认情况下，数据块的最大大小设置为 12 MB。

    - **maxsize=size**
        Specifies the maximum size (in bytes) of disk data to keep for the default recording. Append k or K, to specify the size in KB, m or M to specify the size in MB, g or G to specify the size in GB. By default, the maximum size of disk data is not limited, and this parameter is set to 0.

        This parameter is valid only if you set the `disk=true` parameter.
        指定为默认记录保留的磁盘数据的最大大小（以字节为单位）。追加k或K, 以 KB为单位指定大小，m或M以 MB为单位指定大小，g或G以 GB 为单位指定大小。默认情况下，磁盘数据的最大大小没有限制，该参数设置为0。

        只有设置了该disk=true参数，该参数才有效。

    - **repository=path**
        Specifies the repository (a directory) for temporary disk storage. By default, the system's temporary directory is used.
        指定用于临时磁盘存储的存储库（目录）。默认情况下，使用系统的临时目录。

    - **samplethreads={true|false}**
        Specifies whether thread sampling is enabled. Thread sampling occurs only if the sampling event is enabled along with this parameter. By default, this parameter is enabled.
        指定是否启用线程采样。仅当采样事件与此参数一起启用时，才会发生线程采样。默认情况下，启用此参数。

    - **settings=path**
        Specifies the path and name of the event settings file (of type JFC). By default, the `default.jfc` file is used, which is located in `JAVA_HOME/jre/lib/jfr`.
        指定事件设置文件（JFC 类型）的路径和名称。默认情况下，default.jfc使用位于JAVA_HOME/jre/lib/jfr.

    - **stackdepth=depth**
        Stack depth for stack traces by JFR. By default, the depth is set to 64 method calls. The maximum is 2048, minimum is 1.
        JFR 堆栈跟踪的堆栈深度。默认情况下，深度设置为 64 次方法调用。最大值为 2048，最小值为 1。

    - **threadbuffersize=size**
        Specifies the per-thread local buffer size (in bytes). Append k or K, to specify the size in KB, m or M to specify the size in MB, g or G to specify the size in GB. Higher values for this parameter allow more data gathering without contention to flush it to the global storage. It can increase application footprint in a thread-rich environment. By default, the local buffer size is set to 5 KB.
        指定每线程本地缓冲区大小（以字节为单位）。追加k或K, 以 KB为单位指定大小，m或M以 MB为单位指定大小，g或G以 GB 为单位指定大小。此参数的较高值允许收集更多数据而无需争用将其刷新到全局存储。它可以在线程丰富的环境中增加应用程序占用空间。默认情况下，本地缓冲区大小设置为 5 KB。

        You can specify values for multiple parameters by separating them with a comma. For example, to instruct JFR to write a continuous recording to disk, and set the maximum size of data chunks to 10 MB, specify the following:
        您可以通过用逗号分隔多个参数来指定它们的值。例如，要指示 JFR 将连续记录写入磁盘，并将数据块的最大大小设置为 10 MB，请指定以下内容：

            -XX:FlightRecorderOptions=defaultrecording=true,disk=true,maxchunksize=10M
    
- **-XX:LargePageSizeInBytes=size**
    On Solaris, sets the maximum size (in bytes) for large pages used for Java heap. The size argument must be a power of 2 (2, 4, 8, 16, ...). Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes. By default, the size is set to 0, meaning that the JVM chooses the size for large pages automatically.
    在 Solaris 上，设置用于 Java 堆的大页面的最大大小（以字节为单位）。size参数必须是 2的幂 (2, 4, 8, 16, ...)。附加字母k或K表示千字节，m或M表示兆字节，g或G表示千兆字节。默认情况下，大小设置为 0，这意味着 JVM 会自动选择大页面的大小。

    以下示例说明如何将大页面大小设置为 4 兆字节 (MB)：
    The following example illustrates how to set the large page size to 4 megabytes (MB):
        `-XX:LargePageSizeInBytes=4m`

- **-XX:MaxDirectMemorySize=size**
    Sets the maximum total size (in bytes) of the New I/O (the java.nio package) direct-buffer allocations. Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes. By default, the size is set to 0, meaning that the JVM chooses the size for NIO direct-buffer allocations automatically.
    java.nio设置新 I/O（包）直接缓冲区分配的最大总大小（以字节为单位） 。附加字母k或K表示千字节，m或M表示兆字节，g或G表示千兆字节。默认情况下，大小设置为 0，这意味着 JVM 自动选择 NIO 直接缓冲区分配的大小。

    以下示例以不同的单位说明如何将 NIO 大小设置为 1024 KB：
    The following examples illustrate how to set the NIO size to 1024 KB in different units:

        -XX:MaxDirectMemorySize=1m
        -XX:MaxDirectMemorySize=1024k
        -XX:MaxDirectMemorySize=1048576
        
- **-XX:NativeMemoryTracking=mode**
    Specifies the mode for tracking JVM native memory usage. Possible mode arguments for this option include the following:
    定跟踪 JVM 本机内存使用情况的模式。此选项的可能模式参数包括以下内容：
    - off
        Do not track JVM native memory usage. This is the default behavior if you do not specify the -XX:NativeMemoryTracking option.
        不要跟踪 JVM 本机内存使用情况。如果您未指定该`-XX:NativeMemoryTrackin`选项，则这是默认行为。
    - summary
        Only track memory usage by JVM subsystems, such as Java heap, class, code, and thread.
        仅跟踪 JVM 子系统（例如 Java 堆、类、代码和线程）的内存使用情况。
    - detail
        In addition to tracking memory usage by JVM subsystems, track memory usage by individual CallSite, individual virtual memory region and its committed regions.
        除了跟踪 JVM 子系统的内存使用情况外，还跟踪各个CallSite、各个虚拟内存区域及其提交区域的内存使用情况。

- **-XX:ObjectAlignmentInBytes=alignment**
    Sets the memory alignment of Java objects (in bytes). By default, the value is set to 8 bytes. The specified value should be a power of two, and must be within the range of 8 and 256 (inclusive). This option makes it possible to use compressed pointers with large Java heap sizes.
    设置 Java 对象的内存对齐方式（以字节为单位）。默认情况下，该值设置为 8 个字节。指定的值应该是2的幂，并且必须在8和256（含）之间。此选项使得使用具有较大 Java 堆大小的压缩指针成为可能。

    以字节为单位的堆大小限制计算如下：

    The heap size limit in bytes is calculated as:

        4GB * ObjectAlignmentInBytes

    Note: As the alignment value increases, the unused space between objects will also increase. As a result, you may not realize any benefits from using compressed pointers with large Java heap sizes.
    注意：随着对齐值的增加，对象之间未使用的空间也会增加。因此，您可能不会意识到使用具有较大 Java 堆大小的压缩指针的任何好处。
    
- **-XX:OnError=string**
    Sets a custom command or a series of semicolon-separated commands to run when an irrecoverable error occurs. If the string contains spaces, then it must be enclosed in quotation marks.
    将自定义命令或一系列以分号分隔的命令设置为在发生不可恢复的错误时运行。如果字符串包含空格，则必须用引号括起来。

    The following example shows how the -XX:OnError option can be used to run the gcore command to create the core image, and the debugger is started to attach to the process in case of an irrecoverable error (the %p designates the current process):
    以下示例显示了如何使用该-XX:OnError选项来运行gcore创建核心映像的命令，以及在出现不可恢复的错误（%p指定当前进程）的情况下启动调试器以附加到进程：

        -XX:OnError="gcore %p;dbx - %p"
        
- **-XX:OnOutOfMemoryError=string**
    Sets a custom command or a series of semicolon-separated commands to run when an OutOfMemoryError exception is first thrown. If the string contains spaces, then it must be enclosed in quotation marks. For an example of a command string, see the description of the `-XX:OnError` option.
    将自定义命令或一系列以分号分隔的命令设置为在OutOfMemoryError首次抛出异常时运行。如果字符串包含空格，则必须用引号括起来。有关命令字符串的示例，请参阅-XX:OnError选项的描述。
- **-XX:+PerfDataSaveToFile**
    If enabled, saves jstat(1) binary data when the Java application exits. This binary data is saved in a file named `hsperfdata_<pid>`, where `<pid>` is the process identifier of the Java application you ran. Use jstat to display the performance data contained in this file as follows:
    如果启用，则在 Java 应用程序退出时保存 jstat(1) 二进制数据。 此二进制数据保存在名为“hsperfdata_<pid>”的文件中，其中“<pid>”是您运行的 Java 应用程序的进程标识符。 使用jstat显示该文件包含的性能数据如下：

        jstat -class file:///<path>/hsperfdata_<pid>
        jstat -gc file:///<path>/hsperfdata_<pid>
        
- *-XX:-PreferContainerQuotaForCPUCount**
    If true, then calculate the container CPU availability based on the value of its CPU CFS (Completely Fair Scheduler) quota (if set). If false, then use the CPU shares value instead, provided it is less than the CPU quota value.
    如果为真，则根据其 CPU CFS（完全公平调度程序）配额（如果设置）的值计算容器 CPU 可用性。如果为 false，则使用 CPU 份额值代替，前提是它小于 CPU 配额值。

    The VM uses the container CPU availability value to calculate the number of available processors if you haven't specified it with the `-XX:ActiveProcessorCount option`.
    
    如果您没有使用`-XX:ActiveProcessorCount option`指定它，VM 将使用容器 CPU 可用性值来计算可用处理器的数量。
    Note: The CPU count will never exceed the number of active processors available to the process. Active processors may have been limited by having them disabled or by selecting a cpuset (a list of CPUs; see the PrintContainerInfo option) for a container.
    注意：CPU 计数永远不会超过进程可用的活动处理器数。活动处理器可能已通过禁用它们或通过为PrintContainerInfo容器选择一个 cpuset（CPU 列表；请参阅选项）而受到限制。
- **-XX:+PrintCommandLineFlags**
    Enables printing of ergonomically selected JVM flags that appeared on the command line. It can be useful to know the ergonomic values set by the JVM, such as the heap space size and the selected garbage collector. By default, this option is disabled and flags are not printed.
    启用打印出现在命令行上的符合ergonomically(我也不知道为什么叫人体工程学，输出的是人为设置的标记)的 JVM 标志。 了解 JVM 设置的符合ergonomically的值可能很有用，例如堆空间大小和选定的垃圾收集器。 默认情况下，此选项被禁用并且不打印标志。
- **-XX:+PrintContainerInfo**
    Prints the following information about the container:

    - cpuset.cpus: A comma-separated list or hyphen-separated range of CPUs or cores a container can use provided that you have more than one CPU or core. For example, the value 0-3 means that the container can use the first, second, third, and fourth CPU.
    如果您有多个 CPU 或内核，则容器可以使用的 CPU 或内核的逗号分隔列表或连字符分隔范围。例如，值0-3表示容器可以使用第一、第二、第三和第四个CPU。
    - cpuset.mems: A comma-separated list or hyphen-separated range of memory nodes (MEMs) in which to allow execution; running of applications; only effective on non-uniform memory access (NUMA) systems.
    允许在其中执行的以逗号分隔的列表或连字符分隔的内存节点 (MEM) 范围；应用程序的运行；仅对非统一内存访问 (NUMA) 系统有效。
    
    - CPU Shares: The amount of CPU shares available to the process.
    进程可用的 CPU 份额数。
    
    - CPU Quota: The number of milliseconds per period process is guaranteed to run.
    每周期进程保证运行的毫秒数。
    
    - CPU Period: The CPU Completely Fair Scheduler (CFS) period, in ms.
    CPU 完全公平调度器 (CFS) 周期，以毫秒为单位。
    
    - OSContainer::active_processor_count: Number of active processors for the VM to use.
    供 VM 使用的活动处理器数。
    
    - Memory Limit: The maximum amount of memory the container can use, in bytes.
    容器可以使用的最大内存量，以字节为单位。
    
    - Memory Soft Limit: The maximum amount of memory a container should use, in bytes; this limit is smaller than Memory Limit, but because this is a soft limit, the container may exceed it.
    容器应使用的最大内存量，以字节为单位；此限制小于 Memory Limit，但由于这是一个软限制，容器可能会超过它。
    
    - Memory Usage: The amount of used memory for this process, in bytes.
    此进程使用的内存量，以字节为单位。
    - Maximum Memory Usage: The maximum amount of used memory for this process, in bytes.
    此进程使用的最大内存量，以字节为单位。
- **-XX:+PrintNMTStatistics**
    Enables printing of collected native memory tracking data at JVM exit when native memory tracking is enabled (see ·-XX:NativeMemoryTracking·). By default, this option is disabled and native memory tracking data is not printed.
    当启用本机内存跟踪时，允许在 JVM 出口处打印收集的本机内存跟踪数据（参见 参考资料-XX:NativeMemoryTracking）。默认情况下，此选项被禁用并且不打印本机内存跟踪数据。

- **-XX:+RelaxAccessControlCheck**
    Decreases the amount of access control checks in the verifier. By default, this option is disabled, and it is ignored (that is, treated as disabled) for classes with a recent bytecode version. You can enable it for classes with older versions of the bytecode.
    减少验证器中访问控制检查的数量。默认情况下，此选项是禁用的，对于具有最新字节码版本的类，它会被忽略（即被视为禁用）。您可以为具有旧版本字节码的类启用它。

- **-XX:+ResourceManagement**
    Enables the use of Resource Management during the runtime of the application.

    This is a commercial feature that requires you to also specify the `-XX:+UnlockCommercialFeatures option` as follows:
    允许在应用程序运行时使用资源管理。
    这是一项商业功能，需要您还指定`-XX:+UnlockCommercialFeatures`如下选项：

        java -XX:+UnlockCommercialFeatures -XX:+ResourceManagement

- **-XX:ResourceManagementSampleInterval=value (milliseconds)**
    Sets the parameter that controls the sampling interval for Resource Management measurements, in milliseconds.
    设置控制资源管理测量的采样间隔的参数，以毫秒为单位。

    This option can be used only when Resource Management is enabled (that is, the -XX:+ResourceManagement option is specified).
    只有在启用资源管理（即指定该选项）时才能使用此选项`-XX:+ResourceManagement`。
    
- **-XX:SharedArchiveFile=path**
    Specifies the path and name of the class data sharing (CDS) archive file
    指定类数据共享 (CDS) 存档文件的路径和名称

- **-XX:SharedClassListFile=file_name**
    Specifies the text file that contains the names of the class files to store in the class data sharing (CDS) archive. This file contains the full name of one class file per line, except slashes (/) replace dots (.). For example, to specify the classes java.lang.Object and hello.Main, create a text file that contains the following two lines:
    指定包含要存储在类数据共享 (CDS) 存档中的类文件名称的文本文件。此文件每行包含一个类文件的全名，斜线 ( /) 替换点 ( .) 除外。例如，要指定类java.lang.Object和hello.Main，请创建一个包含以下两行的文本文件：
    
        java/lang/Object
        hello/Main

    The class files that you specify in this text file should include the classes that are commonly used by the application. They may include any classes from the application, extension, or bootstrap class paths.
    您在此文本文件中指定的类文件应包括应用程序常用的类。它们可能包括来自应用程序、扩展或引导程序类路径的任何类。

- **-XX:+ShowMessageBoxOnError**
    Enables displaying of a dialog box when the JVM experiences an irrecoverable error. This prevents the JVM from exiting and keeps the process active so that you can attach a debugger to it to investigate the cause of the error. By default, this option is disabled.
    当 JVM 遇到不可恢复的错误时启用对话框的显示。这可以防止 JVM 退出并使进程保持活动状态，以便您可以将调试器附加到它以调查错误原因。默认情况下，此选项被禁用。

- **-XX:StartFlightRecording=parameter=value**
    Starts a JFR recording for the Java application. This is a commercial feature that works in conjunction with the -XX:+UnlockCommercialFeatures option. This option is equivalent to the JFR.start diagnostic command that starts a recording during runtime. You can set the following parameters when starting a JFR recording:
    为 Java 应用程序启动 JFR 记录。这是与该-XX:+UnlockCommercialFeatures选项结合使用的商业功能。此选项等效于JFR.start在运行时启动记录的诊断命令。开始 JFR 录制时，您可以设置以下参数：

    - compress={true|false}
        Specifies whether to compress the JFR recording log file (of type JFR) on the disk using the gzip file compression utility. This parameter is valid only if the filename parameter is specified. By default it is set to false (recording is not compressed). To enable compression, set the parameter to true.
        指定是否使用 gzip 文件压缩实用程序压缩磁盘上的 JFR 记录日志文件（JFR 类型）。 此参数仅在指定文件名参数时有效。 默认情况下，它设置为 false（记录未压缩）。 要启用压缩，请将参数设置为 true。

    - defaultrecording={true|false}
        Specifies whether the recording is a continuous background recording or if it runs for a limited time. By default, this parameter is set to false (recording runs for a limited time). To make the recording run continuously, set the parameter to true.
        指定录制是连续的后台录制还是在有限的时间内运行。默认情况下，此参数设置为false（录制运行有限时间）。要使记录连续运行，请将参数设置为true。

    - delay=time
    Specifies the delay between the Java application launch time and the start of the recording. Append s to specify the time in seconds, m for minutes, h for hours, or d for days (for example, specifying 10m means 10 minutes). By default, there is no delay, and this parameter is set to 0.
    指定 Java 应用程序启动时间和记录开始之间的延迟。追加s以指定时间（以秒、m分钟、h小时或d天为单位）（例如，指定10m表示 10 分钟）。默认情况下没有延迟，此参数设置为 0。

    - dumponexit={true|false}
        Specifies whether a dump file of JFR data should be generated when the JVM terminates in a controlled manner. By default, this parameter is set to false (dump file on exit is not generated). To enable it, set the parameter to true.
        指定当 JVM 以受控方式终止时是否应生成 JFR 数据的转储文件。默认情况下，此参数设置为false（不生成退出时的转储文件）。要启用它，请将参数设置为true。

        The dump file is written to the location defined by the filename parameter.
        转储文件写入filename参数定义的位置。
        
        Example:

            -XX:StartFlightRecording=name=test,filename=D:\test.jfr,dumponexit=true

    - duration=time
        Specifies the duration of the recording. Append s to specify the time in seconds, m for minutes, h for hours, or d for days (for example, specifying 5h means 5 hours). By default, the duration is not limited, and this parameter is set to 0.
        指定录制的持续时间。追加s以指定时间（以秒、m分钟、h小时或d天为单位）（例如，指定5h表示 5 小时）。默认不限时长，该参数设置为0。
        
    - filename=path
        Specifies the path and name of the JFR recording log file.
        指定 JFR 记录日志文件的路径和名称。

    - name=identifier
        Specifies the identifier for the JFR recording. By default, it is set to Recording x.
        指定 JFR 记录的标识符。默认情况下，它设置为Recording x。
        
    - maxage=time
        Specifies the maximum age of disk data to keep for the default recording. Append s to specify the time in seconds, m for minutes, h for hours, or d for days (for example, specifying 30s means 30 seconds). By default, the maximum age is set to 15 minutes (15m).
        指定为默认记录保留的磁盘数据的最长期限。附加s以指定时间（以秒、m分钟、h小时或d天为单位）（例如，指定30s表示 30 秒）。默认情况下，最长期限设置为 15 分钟 ( 15m)。

    - maxsize=size
        Specifies the maximum size (in bytes) of disk data to keep for the default recording. Append k or K, to specify the size in KB, m or M to specify the size in MB, g or G to specify the size in GB. By default, the maximum size of disk data is not limited, and this parameter is set to 0.
        指定为默认记录保留的磁盘数据的最大大小（以字节为单位）。追加k或K, 以 KB为单位指定大小，m或M以 MB为单位指定大小，g或G以 GB 为单位指定大小。默认情况下，磁盘数据的最大大小没有限制，该参数设置为0。

    - settings=path
        Specifies the path and name of the event settings file (of type JFC). By default, the default.jfc file is used, which is located in JAVA_HOME/jre/lib/jfr.
        指定事件设置文件（JFC 类型）的路径和名称。默认情况下，default.jfc使用位于JAVA_HOME/jre/lib/jfr.

    You can specify values for multiple parameters by separating them with a comma. For example, to save the recording to test.jfr in the current working directory, and instruct JFR to compress the log file, specify the following:
    您可以通过用逗号分隔多个参数来指定它们的值。例如，要将记录保存到当前工作目录下的 test.jfr，并指示 JFR 压缩日志文件，指定以下内容：

            -XX:StartFlightRecording=filename=test.jfr,compress=true

- **-XX:ThreadStackSize=size**
    Sets the thread stack size (in bytes). Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes. The default value depends on the platform:
    设置线程堆栈大小（以字节为单位）。附加字母k或K表示千字节，m或M表示兆字节，g或G表示千兆字节。默认值取决于平台：

    - Linux/ARM (32-bit): 320 KB

    - Linux/i386 (32-bit): 320 KB

    - Linux/x64 (64-bit): 1024 KB

    - OS X (64-bit): 1024 KB

    - Oracle Solaris/i386 (32-bit): 320 KB

    - Oracle Solaris/x64 (64-bit): 1024 KB

The following examples show how to set the thread stack size to 1024 KB in different units:
以下示例显示了如何将线程堆栈大小设置为不同单位的 1024 KB：

    -XX:ThreadStackSize=1m
    -XX:ThreadStackSize=1024k
    -XX:ThreadStackSize=1048576
This option is equivalent to `-Xss`.该选项等同于`-Xss`.

- **-XX:+TraceClassLoading**
    Enables tracing of classes as they are loaded. By default, this option is disabled and classes are not traced.
    启用类加载时的跟踪。默认情况下，禁用此选项并且不跟踪类。

- **-XX:+TraceClassLoadingPreorder**
    Enables tracing of all loaded classes in the order in which they are referenced. By default, this option is disabled and classes are not traced.
    启用按引用顺序跟踪所有加载的类。默认情况下，禁用此选项并且不跟踪类。

- **-XX:+TraceClassResolution**
    Enables tracing of constant pool resolutions. By default, this option is disabled and constant pool resolutions are not traced.
    启用对常量池解析的跟踪。默认情况下，此选项被禁用并且不跟踪常量池解析。

- **-XX:+TraceClassUnloading**
    Enables tracing of classes as they are unloaded. By default, this option is disabled and classes are not traced.
    启用类卸载时的跟踪。默认情况下，禁用此选项并且不跟踪类。

- **-XX:+TraceLoaderConstraints**
    Enables tracing of the loader constraints recording. By default, this option is disabled and loader constraints recording is not traced.
    启用加载程序约束记录的跟踪。默认情况下，此选项被禁用并且不跟踪加载程序约束记录。

- **-XX:+UnlockCommercialFeatures**
    Enables the use of commercial features. Commercial features are included with Oracle Java SE Advanced or Oracle Java SE Suite packages, as defined on the Java SE Products page at http://www.oracle.com/technetwork/java/javase/terms/products/index.html
    允许使用商业功能。商业特性包含在 Oracle Java SE Advanced 或 Oracle Java SE Suite 软件包中，如Java SE 产品页面所定义，位于http://www.oracle.com/technetwork/java/javase/terms/products/index.html    

    By default, this option is disabled and the JVM runs without the commercial features. Once they were enabled for a JVM process, it is not possible to disable their use for that process.
    默认情况下，此选项被禁用，JVM 运行时没有商业功能。一旦为 JVM 进程启用了它们，就不可能禁用它们对该进程的使用。
    
    If this option is not provided, commercial features can still be unlocked in a running JVM by using the appropriate jcmd diagnostic commands.
    如果未提供此选项，商业功能仍然可以通过使用适当的jcmd诊断命令在运行的 JVM 中解锁。
- **-XX:+UseAltSigs**
    Enables the use of alternative signals instead of SIGUSR1 and SIGUSR2 for JVM internal signals. By default, this option is disabled and alternative signals are not used. This option is equivalent to `Xusealtsigs`.
    允许使用替代信号代替JVM 内部信号SIGUSR1。SIGUSR2默认情况下，此选项被禁用并且不使用替代信号。该选项等同于`-Xusealtsigs`.

- **-XX:+UseAppCDS**
    Enables application class data sharing (AppCDS). To use AppCDS, you must also specify values for the options `-XX:SharedClassListFile` and `-XX:SharedArchiveFile` during both CDS dump time (see the option -Xshare:dump) and application run time.
    启用应用程序类数据共享 (AppCDS)。 要使用 AppCDS，您还必须在 CDS 转储时间（请参阅选项 -Xshare:dump）和应用程序运行期间为选项`-XX:SharedClassListFile`和`-XX:SharedArchiveFile`指定值。

    This is a commercial feature that requires you to also specify the `-XX:+UnlockCommercialFeatures option`. This is also an experimental feature; it may change in future releases.
    这是一项商业功能，需要您还指定`-XX:+UnlockCommercialFeatures`选项。 这也是一项实验性功能； 它可能会在未来的版本中改变。

    See "Application Class Data Sharing".
    
- **-XX:-UseBiasedLocking**
    Disables the use of biased locking. Some applications with significant amounts of uncontended synchronization may attain significant speedups with this flag enabled, whereas applications with certain patterns of locking may see slowdowns. For more information about the biased locking technique, see the example in Java Tuning White Paper at http://www.oracle.com/technetwork/java/tuning-139912.html#section4.2.5
    禁止使用偏向锁定。 启用此标志后，一些具有大量无竞争同步的应用程序可能会获得显着的加速，而具有某些锁定模式的应用程序可能会变慢。 有关偏向锁定技术的更多信息，请参阅 Java 调优白皮书中的示例，网址为 http://www.oracle.com/technetwork/java/tuning-139912.html#section4.2.5    

    By default, this option is enabled.
    默认情况下，此选项已启用。
    
- **-XX:-UseCompressedOops**
    Disables the use of compressed pointers. By default, this option is enabled, and compressed pointers are used when Java heap sizes are less than 32 GB. When this option is enabled, object references are represented as 32-bit offsets instead of 64-bit pointers, which typically increases performance when running the application with Java heap sizes less than 32 GB. This option works only for 64-bit JVMs.
    禁用压缩指针的使用。默认情况下，启用此选项，并在 Java 堆大小小于 32 GB 时使用压缩指针。启用此选项后，对象引用表示为 32 位偏移量而不是 64 位指针，这通常会在运行 Java 堆大小小于 32 GB 的应用程序时提高性能。此选项仅适用于 64 位 JVM。

    It is also possible to use compressed pointers when Java heap sizes are greater than 32GB. See the `-XX:ObjectAlignmentInBytes option`.
    当 Java 堆大小大于 32GB 时，也可以使用压缩指针。查看`-XX:ObjectAlignmentInBytes option`。
    
- **-XX:-UseContainerSupport**
    The VM provides automatic container detection support, which enables the VM to determine the amount of memory and number of processors that are available to a Java process running in docker containers. It uses this information to allocate system resources. This support is only available on Linux x64 platforms. If supported, then the default value for this flag is true and container support is enabled by default. You can disable it with `-XX:-UseContainerSupport`.
    VM 提供自动容器检测支持，这使 VM 能够确定在 docker 容器中运行的 Java 进程可用的内存量和处理器数量。它使用此信息来分配系统资源。此支持仅适用于 Linux x64 平台。如果支持，则此标志的默认值为true并且默认启用容器支持。您可以使用禁用它`-XX:-UseContainerSupport`。

- **-XX:+UseHugeTLBFS**
    This option for Linux is the equivalent of specifying `-XX:+UseLargePages`. This option is disabled by default. This option pre-allocates all large pages up-front, when memory is reserved; consequently the JVM cannot dynamically grow or shrink large pages memory areas; see `-XX:UseTransparentHugePages` if you want this behavior.
    Linux 的这个选项等同于指定`-XX:+UseLargePages`。 默认情况下禁用此选项。 当内存被保留时，这个选项预先分配所有大页面； 因此，JVM 无法动态地增大或缩小大页面内存区域； 如果您想要此行为，请参阅`-XX:UseTransparentHugePages`。
    For more information, see "Large Pages".

- **-XX:+UseLargePages**
    Enables the use of large page memory. By default, this option is disabled and large page memory is not used.
    启用大页面内存的使用。默认情况下，禁用此选项并且不使用大页面内存。
    For more information, see "Large Pages".
- **-XX:+UseMembar**
    Enables issuing of membars on thread state transitions. This option is disabled by default on all platforms except ARM servers, where it is enabled. (It is recommended that you do not disable this option on ARM servers.)
    允许在线程状态转换时发布成员。默认情况下，此选项在除 ARM 服务器之外的所有平台上都处于禁用状态，在该平台上它已启用。（建议您不要在 ARM 服务器上禁用此选项。）

- **-XX:+UsePerfData**
    Enables the perfdata feature. This option is enabled by default to allow JVM monitoring and performance testing. Disabling it suppresses the creation of the hsperfdata_userid directories. To disable the perfdata feature, specify `-XX:-UsePerfData`.
    启用该perfdata功能。默认情况下启用此选项以允许 JVM 监视和性能测试。禁用它会抑制hsperfdata_userid目录的创建。要禁用该perfdata功能，请指定-XX:-UsePerfData.

- **-XX:+UseTransparentHugePages**
    On Linux, enables the use of large pages that can dynamically grow or shrink. This option is disabled by default. You may encounter performance problems with transparent huge pages as the OS moves other pages around to create huge pages; this option is made available for experimentation.
    在 Linux 上，允许使用可以动态增长或收缩的大页面。默认情况下禁用此选项。当操作系统移动其他页面以创建大页面时，您可能会遇到透明大页面的性能问题；此选项可用于实验。

    For more information, see "Large Pages".
    
- **-XX:+AllowUserSignalHandlers**
    Enables installation of signal handlers by the application. By default, this option is disabled and the application is not allowed to install signal handlers.
    允许应用程序安装信号处理程序。默认情况下，此选项被禁用，并且不允许应用程序安装信号处理程序。
    

<a id="Advanced_JIT_Compiler_Options"></a>**Advanced JIT Compiler Options**
These options control the dynamic just-in-time (JIT) compilation performed by the Java HotSpot VM.
这些选项控制由 Java HotSpot VM 执行的动态即时 (JIT) 编译。
- **-XX:+AggressiveOpts**
    Enables the use of aggressive performance optimization features, which are expected to become default in upcoming releases. By default, this option is disabled and experimental performance features are not used.
    启用积极的性能优化功能，这些功能有望在即将发布的版本中成为默认功能。默认情况下，此选项处于禁用状态，并且不使用实验性性能功能。

- **-XX:AllocateInstancePrefetchLines=lines**
    Sets the number of lines to prefetch ahead of the instance allocation pointer. By default, the number of lines to prefetch is set to 1:
    设置要在实例分配指针之前预取的行数。默认情况下，要预取的行数设置为 1：

        -XX:AllocateInstancePrefetchLines=1
        
    Only the Java HotSpot Server VM supports this option.
    只有 Java HotSpot Server VM 支持此选项。
- **-XX:AllocatePrefetchDistance=size**
    Sets the size (in bytes) of the prefetch distance for object allocation. Memory about to be written with the value of new objects is prefetched up to this distance starting from the address of the last allocated object. Each Java thread has its own allocation point.

    Negative values denote that prefetch distance is chosen based on the platform. Positive values are bytes to prefetch. Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes. The default value is set to -1.
    设置对象分配的预取距离的大小（以字节为单位）。将要用新对象的值写入的内存从最后分配的对象的地址开始预取到这个距离。每个 Java 线程都有自己的分配点。

    负值表示预取距离是根据平台选择的。正值是要预取的字节。附加字母k或K表示千字节，m或M表示兆字节，g或G表示千兆字节。默认值设置为 -1。

    The following example shows how to set the prefetch distance to 1024 bytes:
    以下示例显示如何将预取距离设置为 1024 字节：

        -XX:AllocatePrefetchDistance=1024
        
    Only the Java HotSpot Server VM supports this option.

- **-XX:AllocatePrefetchInstr=instruction**
    Sets the prefetch instruction to prefetch ahead of the allocation pointer. Only the Java HotSpot Server VM supports this option. Possible values are from 0 to 3. The actual instructions behind the values depend on the platform. By default, the prefetch instruction is set to 0:
    将预取指令设置为在分配指针之前预取。只有 Java HotSpot Server VM 支持此选项。可能的值是从 0 到 3。值背后的实际说明取决于平台。默认情况下，预取指令设置为 0：

        -XX:AllocatePrefetchInstr=0
    Only the Java HotSpot Server VM supports this option.

- **-XX:AllocatePrefetchLines=lines**
    Sets the number of cache lines to load after the last object allocation by using the prefetch instructions generated in compiled code. The default value is 1 if the last allocated object was an instance, and 3 if it was an array.
    使用编译代码中生成的预取指令，设置在最后一次对象分配后加载的缓存行数。如果最后分配的对象是实例，则默认值为 1，如果是数组，则默认值为 3。

    The following example shows how to set the number of loaded cache lines to 5:
    以下示例显示如何将加载的缓存行数设置为 5：

        -XX:AllocatePrefetchLines=5
    Only the Java HotSpot Server VM supports this option.
    
- **-XX:AllocatePrefetchStepSize=size**
    Sets the step size (in bytes) for sequential prefetch instructions. Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes. By default, the step size is set to 16 bytes:
    设置顺序预取指令的步长（以字节为单位）。附加字母k或K表示千字节，m或M表示兆字节，g或G表示千兆字节。默认情况下，步长设置为 16 个字节：
        -XX:AllocatePrefetchStepSize=16
    Only the Java HotSpot Server VM supports this option.

- **-XX:AllocatePrefetchStyle=style**
    Sets the generated code style for prefetch instructions. The style argument is an integer from 0 to 3:

    - 0
    Do not generate prefetch instructions.
    不生成预取指令。

    - 1
    Execute prefetch instructions after each allocation. This is the default parameter.
    每次分配后执行预取指令。这是默认参数。

    - 2
    Use the thread-local allocation block (TLAB) watermark pointer to determine when prefetch instructions are executed.
    使用线程本地分配块 (TLAB) 水印指针来确定何时执行预取指令。

    - 3
    Use BIS instruction on SPARC for allocation prefetch.
    在 SPARC 上使用 BIS 指令进行分配预取。

    Only the Java HotSpot Server VM supports this option.

- **-XX:+BackgroundCompilation**
    Enables background compilation. This option is enabled by default. To disable background compilation, specify -XX:-BackgroundCompilation (this is equivalent to specifying -Xbatch).
    启用后台编译。默认情况下启用此选项。要禁用后台编译，请指定`-XX:-BackgroundCompilation`（这等同于指定`-Xbatch`）。

- **-XX:CICompilerCount=threads**
    Sets the number of compiler threads to use for compilation. By default, the number of threads is set to 2 for the server JVM, to 1 for the client JVM, and it scales to the number of cores if tiered compilation is used. The following example shows how to set the number of threads to 2:
    设置用于编译的编译器线程数。默认情况下，服务器 JVM 的线程数设置为 2，客户端 JVM 的线程数设置为 1，如果使用分层编译，它会扩展到核心数。以下示例显示如何将线程数设置为 2：

        -XX:CICompilerCount=2
        
- **-XX:CodeCacheMinimumFreeSpace=size**
    Sets the minimum free space (in bytes) required for compilation. Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes. When less than the minimum free space remains, compiling stops. By default, this option is set to 500 KB. The following example shows how to set the minimum free space to 1024 MB:
    设置编译所需的最小可用空间（以字节为单位）。附加字母k或K表示千字节，m或M表示兆字节，g或G表示千兆字节。当剩余空间小于最小可用空间时，编译停止。默认情况下，此选项设置为 500 KB。以下示例显示如何将最小可用空间设置为 1024 MB：

        -XX:CodeCacheMinimumFreeSpace=1024m

- **-XX:CompileCommand=command,method[,option]**
    Specifies a command to perform on a method. For example, to exclude the indexOf() method of the String class from being compiled, use the following:
    指定要对方法执行的命令。例如，要从编译中排除类的indexOf()方法String，请使用以下内容：

        -XX:CompileCommand=exclude,java/lang/String.indexOf
    Note that the full class name is specified, including all packages and subpackages separated by a slash (/). For easier cut and paste operations, it is also possible to use the method name format produced by the -XX:+PrintCompilation and -XX:+LogCompilation options:
    请注意，已指定完整的类名，包括所有由斜杠 ( /) 分隔的包和子包。为了更轻松地进行剪切和粘贴操作，还可以使用由-XX:+PrintCompilation和-XX:+LogCompilation选项生成的方法名称格式：

        -XX:CompileCommand=exclude,java.lang.String::indexOf
        
    If the method is specified without the signature, the command will be applied to all methods with the specified name. However, you can also specify the signature of the method in the class file format. In this case, you should enclose the arguments in quotation marks, because otherwise the shell treats the semicolon as command end. For example, if you want to exclude only the indexOf(String) method of the String class from being compiled, use the following:
    如果指定的方法没有签名，则该命令将应用于具有指定名称的所有方法。但是，您也可以在类文件格式中指定方法的签名。在这种情况下，您应该将参数括在引号中，否则 shell 会将分号视为命令结束。例如，如果您只想排除类的indexOf(String)方法String被编译，请使用以下内容：

        -XX:CompileCommand="exclude,java/lang/String.indexOf,(Ljava/lang/String;)I"
    You can also use the asterisk (*) as a wildcard for class and method names. For example, to exclude all indexOf() methods in all classes from being compiled, use the following:
    您还可以使用星号 (*) 作为类和方法名称的通配符。例如，要排除indexOf()编译所有类中的所有方法，请使用以下命令：

        -XX:CompileCommand=exclude,*.indexOf
        
    The commas and periods are aliases for spaces, making it easier to pass compiler commands through a shell. You can pass arguments to -XX:CompileCommand using spaces as separators by enclosing the argument in quotation marks:
    逗号和句点是空格的别名，可以更轻松地通过 shell 传递编译器命令。您可以通过`-XX:CompileCommand`将参数括在引号中来将参数传递给使用空格作为分隔符：

        -XX:CompileCommand="exclude java/lang/String indexOf"
        
    Note that after parsing the commands passed on the command line using the -XX:CompileCommand options, the JIT compiler then reads commands from the .hotspot_compiler file. You can add commands to this file or specify a different file using the -XX:CompileCommandFile option.
    请注意，在使用选项解析命令行上传递的命令后-XX:CompileCommand，JIT 编译器随后会从.hotspot_compiler文件中读取命令。您可以将命令添加到此文件或使用该-XX:CompileCommandFile选项指定不同的文件。


    To add several commands, either specify the -XX:CompileCommand option multiple times, or separate each argument with the newline separator (\n). The following commands are available:
    要添加多个命令，请-XX:CompileCommand多次指定选项，或使用换行符 ( ) 分隔每个参数\n。以下命令可用：

    - **break**
        Set a breakpoint when debugging the JVM to stop at the beginning of compilation of the specified method.
        调试JVM时设置断点，在指定方法开始编译时停止。

    - **compileonly**
        Exclude all methods from compilation except for the specified method. As an alternative, you can use the -XX:CompileOnly option, which allows to specify several methods.
        从编译中排除除指定方法之外的所有方法。作为替代方案，您可以使用`-XX:CompileOnly`允许指定多种方法的选项。

    - **dontinline**
        Prevent inlining of the specified method.
        防止内联指定的方法。

    - **exclude**
        Exclude the specified method from compilation.
        从编译中排除指定的方法。

    - **help**
        Print a help message for the -XX:CompileCommand option.
        打印该`-XX:CompileCommand`选项的帮助消息。

    - **inline**
        Attempt to inline the specified method.
        尝试内联指定的方法。

    - **log**
        Exclude compilation logging (with the -XX:+LogCompilation option) for all methods except for the specified method. By default, logging is performed for all compiled methods.
        排除除指定方法之外的所有方法的编译日志记录（使用 `-XX:+LogCompilation option`）。 默认情况下，对所有编译方法执行日志记录。

    - **option**
        This command can be used to pass a JIT compilation option to the specified method in place of the last argument (option). The compilation option is set at the end, after the method name. For example, to enable the BlockLayoutByFrequency option for the append() method of the StringBuffer class, use the following:
        此命令可用于将 JIT 编译选项传递给指定的方法，以代替最后一个参数（选项）。 编译选项设置在最后，在方法名称之后。 例如，要为 StringBuffer 类的 append() 方法启用 BlockLayoutByFrequency 选项，请使用以下命令：

            -XX:CompileCommand=option,java/lang/StringBuffer.append,BlockLayoutByFrequency
        You can specify multiple compilation options, separated by commas or spaces.
        您可以指定多个编译选项，以逗号或空格分隔。

    - **print**
        Print generated assembler code after compilation of the specified method.
        编译指定方法后打印生成的汇编代码。

    - **quiet**
        Do not print the compile commands. By default, the commands that you specify with the -XX:CompileCommand option are printed; for example, if you exclude from compilation the `indexOf()` method of the String class, then the following will be printed to standard output:
        不要打印编译命令。 默认情况下，打印您使用 `-XX:CompileCommand option`指定的命令； 例如，如果您从编译中排除 String 类的 indexOf() 方法，则以下内容将打印到标准输出：
        
            CompilerOracle: exclude java/lang/String.indexOf
        You can suppress this by specifying the -XX:CompileCommand=quiet option before other `-XX:CompileCommand options`.
        您可以通过在其他`-XX:CompileCommand option`之前指定 `-XX:CompileCommand=quiet` 选项来抑制这种情况。

- **-XX:CompileCommandFile=filename**
    Sets the file from which JIT compiler commands are read. By default, the .hotspot_compiler file is used to store commands performed by the JIT compiler.
    设置从中读取 JIT 编译器命令的文件。默认情况下，该.hotspot_compiler文件用于存储 JIT 编译器执行的命令。

    Each line in the command file represents a command, a class name, and a method name for which the command is used. For example, this line prints assembly code for the toString() method of the String class:
    命令文件中的每一行代表一个命令、一个类名和使用该命令的方法名。例如，此行打印类toString()方法的汇编代码String：

        print java/lang/String toString
    For more information about specifying the commands for the JIT compiler to perform on methods, see the -XX:CompileCommand option.
    有关指定 JIT 编译器对方法执行的命令的更多信息，请参阅该`-XX:CompileCommand`选项。
    
- **-XX:CompileOnly=methods**
    Sets the list of methods (separated by commas) to which compilation should be restricted. Only the specified methods will be compiled. Specify each method with the full class name (including the packages and subpackages). For example, to compile only the length() method of the String class and the size() method of the List class, use the following:
    设置应限制编译的方法列表（以逗号分隔）。 只会编译指定的方法。 用完整的类名（包括包和子包）指定每个方法。 例如，要仅编译 String 类的 length() 方法和 List 类的 size() 方法，请使用以下代码：

        -XX:CompileOnly=java/lang/String.length,java/util/List.size
    Note that the full class name is specified, including all packages and subpackages separated by a slash (/). For easier cut and paste operations, it is also possible to use the method name format produced by the -XX:+PrintCompilation and -XX:+LogCompilation options:
    请注意，已指定完整的类名，包括所有包和子包，并用斜杠 (/) 分隔。 为了更轻松地进行剪切和粘贴操作，还可以使用由 -XX:+PrintCompilation 和 -XX:+LogCompilation 选项生成的方法名称格式：
    
        -XX:CompileOnly=java.lang.String::length,java.util.List::size
        
    Although wildcards are not supported, you can specify only the class or package name to compile all methods in that class or package, as well as specify just the method to compile methods with this name in any class:
    虽然不支持通配符，但您可以仅指定类或包名称来编译该类或包中的所有方法，也可以仅指定方法来编译任何类中具有此名称的方法：

        -XX:CompileOnly=java/lang/String
        -XX:CompileOnly=java/lang
        -XX:CompileOnly=.length

- **-XX:CompileThreshold=invocations**
    Sets the number of interpreted method invocations before compilation. By default, in the server JVM, the JIT compiler performs 10,000 interpreted method invocations to gather information for efficient compilation. For the client JVM, the default setting is 1,500 invocations. This option is ignored when tiered compilation is enabled; see the option -XX:+TieredCompilation. The following example shows how to set the number of interpreted method invocations to 5,000:
    设置编译前解释方法调用的数量。默认情况下，在服务器 JVM 中，JIT 编译器执行 10,000 次解释方法调用来收集信息以进行高效编译。对于客户端 JVM，默认设置为 1,500 次调用。启用分层编译时忽略此选项；查看选项-XX:+TieredCompilation。以下示例显示如何将解释方法调用的次数设置为 5,000：

        -XX:CompileThreshold=5000
    You can completely disable interpretation of Java methods before compilation by specifying the `-Xcomp`option.
    您可以通过指定-Xcomp选项在编译之前完全禁用 Java 方法的解释。

- **-XX:+DoEscapeAnalysis**
    Enables the use of escape analysis. This option is enabled by default. To disable the use of escape analysis, specify -XX:-DoEscapeAnalysis. Only the Java HotSpot Server VM supports this option.
    启用逃逸分析。 默认情况下启用此选项。 要禁用逃逸分析，请指定 `-XX:-DoEscapeAnalysis`。 只有 Java HotSpot Server VM 支持此选项。
    
- **-XX:InitialCodeCacheSize=size**
    Sets the initial code cache size (in bytes). Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes. The default value is set to 500 KB. The initial code cache size should be not less than the system's minimal memory page size. The following example shows how to set the initial code cache size to 32 KB:
    设置初始代码缓存大小（以字节为单位）。附加字母k或K表示千字节，m或M表示兆字节，g或G表示千兆字节。默认值设置为 500 KB。初始代码缓存大小应不小于系统的最小内存页面大小。以下示例显示如何将初始代码缓存大小设置为 32 KB：

        -XX:InitialCodeCacheSize=32k
        
- **-XX:+Inline**
    Enables method inlining. This option is enabled by default to increase performance. To disable method inlining, specify `-XX:-Inline`.
    启用方法内联。默认情况下启用此选项以提高性能。要禁用方法内联，请指定-XX:-Inline.

- **-XX:InlineSmallCode=size**
    Sets the maximum code size (in bytes) for compiled methods that should be inlined. Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes. Only compiled methods with the size smaller than the specified size will be inlined. By default, the maximum code size is set to 1000 bytes:
    为应该内联的编译方法设置最大代码大小（以字节为单位）。附加字母k或K表示千字节，m或M表示兆字节，g或G表示千兆字节。只有大小小于指定大小的编译方法才会被内联。默认情况下，最大代码大小设置为 1000 字节：

        -XX:InlineSmallCode=1000
        
- **-XX:+LogCompilation**
    Enables logging of compilation activity to a file named hotspot.log in the current working directory. You can specify a different log file path and name using the -XX:LogFile option.
    
    允许将编译活动记录到hotspot.log在当前工作目录中命名的文件中。-XX:LogFile您可以使用该选项指定不同的日志文件路径和名称。

    By default, this option is disabled and compilation activity is not logged. The `-XX:+LogCompilation` option has to be used together with the `-XX:+UnlockDiagnosticVMOptions` option that unlocks diagnostic JVM options.
    
    默认情况下，此选项被禁用并且不记录编译活动。 `-XX:+LogCompilation` 选项必须与用于解锁诊断 JVM 选项的 `-XX:+UnlockDiagnosticVMOptions` 选项一起使用。

    You can enable verbose diagnostic output with a message printed to the console every time a method is compiled by using the `X:+PrintCompilation` option.
    
    您可以启用详细的诊断输出，并在每次使用该`-XX:+PrintCompilation`选项编译方法时向控制台打印一条消息。
    
- **-XX:MaxInlineSize=size**
    Sets the maximum bytecode size (in bytes) of a method to be inlined. Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes. By default, the maximum bytecode size is set to 35 bytes:
    设置要内联的方法的最大字节码大小（以字节为单位）。附加字母k或K表示千字节，m或M表示兆字节，g或G表示千兆字节。默认情况下，最大字节码大小设置为 35 个字节：

        -XX:MaxInlineSize=35
        
- **-XX:MaxNodeLimit=nodes**
    Sets the maximum number of nodes to be used during single method compilation. By default, the maximum number of nodes is set to 65,000:
    设置在单一方法编译期间要使用的最大节点数。默认情况下，最大节点数设置为 65,000：

        -XX:MaxNodeLimit=65000

- **-XX:MaxTrivialSize=size**
    Sets the maximum bytecode size (in bytes) of a trivial method to be inlined. Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes. By default, the maximum bytecode size of a trivial method is set to 6 bytes:
    设置要内联的普通方法的最大字节码大小（以字节为单位）。附加字母k或K表示千字节，m或M表示兆字节，g或G表示千兆字节。默认情况下，普通方法的最大字节码大小设置为 6 个字节：

        -XX:MaxTrivialSize=6

- **-XX:+OptimizeStringConcat**
    Enables the optimization of String concatenation operations. This option is enabled by default. To disable the optimization of String concatenation operations, specify -XX:-OptimizeStringConcat. Only the Java HotSpot Server VM supports this option.
    启用String串联操作的优化。默认情况下启用此选项。要禁用String串联操作的优化，请指定`-XX:-OptimizeStringConcat`. 只有 Java HotSpot Server VM 支持此选项。

- **-XX:+PrintAssembly**
    Enables printing of assembly code for bytecoded and native methods by using the external disassembler.so library. This enables you to see the generated code, which may help you to diagnose performance issues.
    disassembler.so通过使用外部库启用字节码和本机方法的汇编代码打印。这使您能够查看生成的代码，这可能有助于您诊断性能问题。

    By default, this option is disabled and assembly code is not printed. The -XX:+PrintAssembly option has to be used together with the -XX:+UnlockDiagnosticVMOptions option that unlocks diagnostic JVM options.
    默认情况下，该选项被禁用并且不打印汇编代码。 -XX:+PrintAssembly 选项必须与解锁诊断 JVM 选项的 -XX:+UnlockDiagnosticVMOptions 选项一起使用。

- **-XX:+PrintCompilation**
    Enables verbose diagnostic output from the JVM by printing a message to the console every time a method is compiled. This enables you to see which methods actually get compiled. By default, this option is disabled and diagnostic output is not printed.
    每次编译方法时，通过向控制台打印一条消息，启用 JVM 的详细诊断输出。这使您能够查看实际编译了哪些方法。默认情况下，此选项被禁用并且不打印诊断输出。

    您还可以使用该-XX:+LogCompilation选项将编译活动记录到文件中。

    You can also log compilation activity to a file by using the -XX:+LogCompilation option.
    
- **-XX:+PrintInlining**
    Enables printing of inlining decisions. This enables you to see which methods are getting inlined.

    By default, this option is disabled and inlining information is not printed. The -XX:+PrintInlining option has to be used together with the -XX:+UnlockDiagnosticVMOptions option that unlocks diagnostic JVM options.
    启用内联决策的打印。 这使您能够查看正在内联的方法。

     默认情况下，此选项被禁用并且不打印内联信息。 -XX:+PrintInlining 选项必须与解锁诊断 JVM 选项的 -XX:+UnlockDiagnosticVMOptions 选项一起使用。
     
- **-XX:ReservedCodeCacheSize=size**
    Sets the maximum code cache size (in bytes) for JIT-compiled code. Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes. The default maximum code cache size is 240 MB; if you disable tiered compilation with the option -XX:-TieredCompilation, then the default size is 48 MB. This option has a limit of 2 GB; otherwise, an error is generated. The maximum code cache size should not be less than the initial code cache size; see the option -XX:InitialCodeCacheSize. This option is equivalent to -Xmaxjitcodesize.
    设置 JIT 编译代码的最大代码缓存大小（以字节为单位）。附加字母k或K表示千字节，m或M表示兆字节，g或G表示千兆字节。默认最大代码缓存大小为 240 MB；如果您使用选项禁用分层编译-XX:-TieredCompilation，则默认大小为 48 MB。此选项有 2 GB 的限制；否则，会产生错误。最大代码缓存大小不应小于初始代码缓存大小；查看选项-XX:InitialCodeCacheSize。该选项等同于-Xmaxjitcodesize.

- **-XX:RTMAbortRatio=abort_ratio**
    The RTM abort ratio is specified as a percentage (%) of all executed RTM transactions. If a number of aborted transactions becomes greater than this ratio, then the compiled code will be deoptimized. This ratio is used when the -XX:+UseRTMDeopt option is enabled. The default value of this option is 50. This means that the compiled code will be deoptimized if 50% of all transactions are aborted.
    RTM 中止率指定为所有已执行 RTM 事务的百分比 (%)。如果中止事务的数量大于此比率，则编译代码将被取消优化。-XX:+UseRTMDeopt启用该选项时使用此比率。此选项的默认值为 50。这意味着如果 50% 的事务被中止，编译后的代码将被取消优化。

- **-XX:RTMRetryCount=number_of_retries**
    RTM locking code will be retried, when it is aborted or busy, the number of times specified by this option before falling back to the normal locking mechanism. The default value for this option is 5. The `-XX:UseRTMLocking` option must be enabled.
    RTM 锁定代码将被重试，当它被中止或繁忙时，在回退到正常锁定机制之前该选项指定的次数。此选项的默认值为 5。`-XX:UseRTMLocking`必须启用该选项。

- **-XX:-TieredCompilation**
    Disables the use of tiered compilation. By default, this option is enabled. Only the Java HotSpot Server VM supports this option.
    禁用分层编译。默认情况下，此选项已启用。只有 Java HotSpot Server VM 支持此选项。

- **-XX:+UseAES**
    Enables hardware-based AES intrinsics for Intel, AMD, and SPARC hardware. Intel Westmere (2010 and newer), AMD Bulldozer (2011 and newer), and SPARC (T4 and newer) are the supported hardware. UseAES is used in conjunction with UseAESIntrinsics.
    为 Intel、AMD 和 SPARC 硬件启用基于硬件的 AES 内在函数。Intel Westmere（2010 及更新版本）、AMD Bulldozer（2011 及更新版本）和 SPARC（T4 及更新版本）是受支持的硬件。UseAES 与 UseAESIntrinsics 结合使用。

- **-XX:+UseAESIntrinsics**
    UseAES and UseAESIntrinsics flags are enabled by default and are supported only for Java HotSpot Server VM 32-bit and 64-bit. To disable hardware-based AES intrinsics, specify -XX:-UseAES -XX:-UseAESIntrinsics. For example, to enable hardware AES, use the following flags:
    UseAES 和 UseAESIntrinsics 标志默认启用，并且仅支持 Java HotSpot Server VM 32 位和 64 位。要禁用基于硬件的 AES 内在函数，请指定-XX:-UseAES -XX:-UseAESIntrinsics. 例如，要启用硬件 AES，请使用以下标志：

        -XX:+UseAES -XX:+UseAESIntrinsics
    To support UseAES and UseAESIntrinsics flags for 32-bit and 64-bit use -server option to choose Java HotSpot Server VM. These flags are not supported on Client VM.
    要支持 32 位和 64 位的 UseAES 和 UseAESIntrinsics 标志，请使用-server选项来选择 Java HotSpot Server VM。客户端 VM 不支持这些标志。
    
- **-XX:+UseCodeCacheFlushing**
    Enables flushing of the code cache before shutting down the compiler. This option is enabled by default. To disable flushing of the code cache before shutting down the compiler, specify -XX:-UseCodeCacheFlushing.
    在关闭编译器之前启用代码缓存的刷新。默认情况下启用此选项。要在关闭编译器之前禁用代码缓存刷新，请指定-XX:-UseCodeCacheFlushing.

- **-XX:+UseCondCardMark**
    Enables checking of whether the card is already marked before updating the card table. This option is disabled by default and should only be used on machines with multiple sockets, where it will increase performance of Java applications that rely heavily on concurrent operations. Only the Java HotSpot Server VM supports this option.
    在更新卡片表之前启用检查卡片是否已被标记。默认情况下禁用此选项，并且只能在具有多个套接字的机器上使用，它将提高严重依赖并发操作的 Java 应用程序的性能。只有 Java HotSpot Server VM 支持此选项。

- **-XX:+UseRTMDeopt**
    Auto-tunes RTM locking depending on the abort ratio. This ratio is specified by -XX:RTMAbortRatio option. If the number of aborted transactions exceeds the abort ratio, then the method containing the lock will be deoptimized and recompiled with all locks as normal locks. This option is disabled by default. The -XX:+UseRTMLocking option must be enabled.
    根据中止率自动调整 RTM锁定。该比率由-XX:RTMAbortRatio选项指定。如果中止事务的数量超过中止率，那么包含锁的方法将被去优化并重新编译，所有锁都作为普通锁。默认情况下禁用此选项。-XX:+UseRTMLocking必须启用该选项。

- **-XX:+UseRTMLocking**
    Generate Restricted Transactional Memory (RTM) locking code for all inflated locks, with the normal locking mechanism as the fallback handler. This option is disabled by default. Options related to RTM are only available for the Java HotSpot Server VM on x86 CPUs that support Transactional Synchronization Extensions (TSX).
    为所有膨胀锁生成受限事务内存 (RTM) 锁定代码，使用正常锁定机制作为回退处理程序。默认情况下禁用此选项。与 RTM 相关的选项仅适用于支持事务同步扩展 (TSX) 的 x86 CPU 上的 Java HotSpot Server VM。

    RTM is part of Intel's TSX, which is an x86 instruction set extension and facilitates the creation of multithreaded applications. RTM introduces the new instructions XBEGIN, XABORT, XEND, and XTEST. The XBEGIN and XEND instructions enclose a set of instructions to run as a transaction. If no conflict is found when running the transaction, the memory and register modifications are committed together at the XEND instruction. The XABORT instruction can be used to explicitly abort a transaction and the XEND instruction to check if a set of instructions are being run in a transaction.
    RTM 是英特尔 TSX 的一部分，它是 x86 指令集扩展，有助于创建多线程应用程序。RTM 引入了新指令XBEGIN、XABORT、XEND和XTEST。XBEGIN和XEND指令包含一组作为事务运行的指令。如果在运行事务时没有发现冲突，则内存和寄存器修改将在XEND指令中一起提交。该XABORT指令可用于显式中止事务以及XEND检查一组指令是否正在事务中运行的指令。

    A lock on a transaction is inflated when another thread tries to access the same transaction, thereby blocking the thread that did not originally request access to the transaction. RTM requires that a fallback set of operations be specified in case a transaction aborts or fails. An RTM lock is a lock that has been delegated to the TSX's system.
    当另一个线程试图访问同一事务时，事务上的锁会膨胀，从而阻塞最初未请求访问该事务的线程。RTM 要求指定一组回退操作，以防事务中止或失败。RTM 锁是已委托给 TSX 系统的锁。

    RTM improves performance for highly contended locks with low conflict in a critical region (which is code that must not be accessed by more than one thread concurrently). RTM also improves the performance of coarse-grain locking, which typically does not perform well in multithreaded applications. (Coarse-grain locking is the strategy of holding locks for long periods to minimize the overhead of taking and releasing locks, while fine-grained locking is the strategy of trying to achieve maximum parallelism by locking only when necessary and unlocking as soon as possible.) Also, for lightly contended locks that are used by different threads, RTM can reduce false cache line sharing, also known as cache line ping-pong. This occurs when multiple threads from different processors are accessing different resources, but the resources share the same cache line. As a result, the processors repeatedly invalidate the cache lines of other processors, which forces them to read from main memory instead of their cache.
    RTM 提高了关键区域中具有低冲突的高度竞争锁的性能（这是不能被多个线程同时访问的代码）。RTM 还提高了粗粒度锁定的性能，粗粒度锁定通常在多线程应用程序中表现不佳。（粗粒度锁定是长时间持有锁以最小化获取和释放锁的开销的策略，而细粒度锁定是通过仅在必要时锁定并尽快解锁来尝试实现最大并行性的策略。 ) 此外，对于不同线程使用的轻度竞争锁，RTM 可以减少错误的缓存行共享，也称为缓存行乒乓。当来自不同处理器的多个线程正在访问不同的资源时，会发生这种情况，但资源共享相同的缓存行。结果，处理器反复使其他处理器的缓存行无效，这迫使它们从主内存而不是缓存中读取。

- **-XX:+UseSHA**
    Enables hardware-based intrinsics for SHA crypto hash functions for SPARC hardware. UseSHA is used in conjunction with the UseSHA1Intrinsics, UseSHA256Intrinsics, and UseSHA512Intrinsics options.
    为 SPARC 硬件的 SHA 加密哈希函数启用基于硬件的内部函数。UseSHA与UseSHA1Intrinsics、UseSHA256Intrinsics和UseSHA512Intrinsics选项结合使用。

    The UseSHA and UseSHA*Intrinsics flags are enabled by default, and are supported only for Java HotSpot Server VM 64-bit on SPARC T4 and newer.
    和标志默认启用，并且仅支持 SPARC T4 和更新版本上的 Java HotSpot Server VM 64 位UseSHA。UseSHA*Intrinsics

    This feature is only applicable when using the sun.security.provider.Sun provider for SHA operations.
    此功能仅在使用sun.security.provider.Sun提供程序进行 SHA 操作时适用。

    To disable all hardware-based SHA intrinsics, specify -XX:-UseSHA. To disable only a particular SHA intrinsic, use the appropriate corresponding option. For example: -XX:-UseSHA256Intrinsics.
    要禁用所有基于硬件的 SHA 内在函数，请指定-XX:-UseSHA. 要仅禁用特定的 SHA 内在函数，请使用相应的相应选项。例如：-XX:-UseSHA256Intrinsics。
    
- **-XX:+UseSHA1Intrinsics**
    Enables intrinsics for SHA-1 crypto hash function.
    为 SHA-1 加密哈希函数启用内在函数。

- **-XX:+UseSHA256Intrinsics**
    Enables intrinsics for SHA-224 and SHA-256 crypto hash functions.
    为 SHA-224 和 SHA-256 加密哈希函数启用内部函数。

- **-XX:+UseSHA512Intrinsics**
    Enables intrinsics for SHA-384 and SHA-512 crypto hash functions.
    为 SHA-384 和 SHA-512 加密哈希函数启用内部函数。

- **-XX:+UseSuperWord**
    Enables the transformation of scalar operations into superword operations. This option is enabled by default. To disable the transformation of scalar operations into superword operations, specify -XX:-UseSuperWord. Only the Java HotSpot Server VM supports this option.
    支持将标量运算转换为超字运算。默认情况下启用此选项。要禁止将标量运算转换为超字运算，请指定-XX:-UseSuperWord. 只有 Java HotSpot Server VM 支持此选项。

        

<a id="Advanced_Serviceability_Options"></a>**Advanced Serviceability Options**
These options provide the ability to gather system information and perform extensive debugging.
这些选项提供了收集系统信息和执行广泛调试的能力。

- **-XX:+ExtendedDTraceProbes**
    Enables additional dtrace tool probes that impact the performance. By default, this option is disabled and dtrace performs only standard probes.
启用dtrace影响性能的其他工具探针。默认情况下，此选项被禁用并且dtrace仅执行标准探测。

- **-XX:+HeapDumpOnOutOfMemoryError**
    Enables the dumping of the Java heap to a file in the current directory by using the heap profiler (HPROF) when a java.lang.OutOfMemoryError exception is thrown. You can explicitly set the heap dump file path and name using the -XX:HeapDumpPath option. By default, this option is disabled and the heap is not dumped when an OutOfMemoryError exception is thrown.
    java.lang.OutOfMemoryError抛出异常时，使用堆分析器 (HPROF) 启用将 Java 堆转储到当前目录中的文件。-XX:HeapDumpPath您可以使用该选项显式设置堆转储文件路径和名称。默认情况下，此选项被禁用，并且在OutOfMemoryError抛出异常时不会转储堆。

- **-XX:HeapDumpPath=path**
    Sets the path and file name for writing the heap dump provided by the heap profiler (HPROF) when the -XX:+HeapDumpOnOutOfMemoryError option is set. By default, the file is created in the current working directory, and it is named java_pidpid.hprof where pid is the identifier of the process that caused the error. The following example shows how to set the default file explicitly (%p represents the current process identificator):
设置选项时，设置用于写入堆分析器 (HPROF) 提供的堆转储的路径和文件名-XX:+HeapDumpOnOutOfMemoryError。默认情况下，该文件在当前工作目录中创建，并命名为java_pidpid.hprof，其中pid是导致错误的进程的标识符。以下示例显示如何显式设置默认文件（%p代表当前进程标识符）：

        -XX:HeapDumpPath=./java_pid%p.hprof
    The following example shows how to set the heap dump file to /var/log/java/java_heapdump.hprof:
    以下示例显示如何将堆转储文件设置为/var/log/java/java_heapdump.hprof：

        -XX:HeapDumpPath=/var/log/java/java_heapdump.hprof
        
- **-XX:LogFile=path**
    Sets the path and file name where log data is written. By default, the file is created in the current working directory, and it is named hotspot.log.
设置写入日志数据的路径和文件名。默认情况下，该文件在当前工作目录中创建，并命名为hotspot.log.

    The following example shows how to set the log file to /var/log/java/hotspot.log:
    以下示例显示如何将日志文件设置为/var/log/java/hotspot.log：

        -XX:LogFile=/var/log/java/hotspot.log
        
- **-XX:+PrintClassHistogram**
    Enables printing of a class instance histogram after a Control+C event (SIGTERM). By default, this option is disabled.
    Control+C在事件 ( SIGTERM)之后启用类实例直方图的打印。默认情况下，此选项被禁用。

    Setting this option is equivalent to running the jmap -histo command, or the jcmd pid GC.class_histogram command, where pid is the current Java process identifier.
    设置该选项等同于运行jmap -histo命令或jcmd pid GC.class_histogram命令，其中pid是当前 Java 进程标识符。
    
- **-XX:+PrintConcurrentLocks**
    Enables printing of java.util.concurrent locks after a Control+C event (SIGTERM). By default, this option is disabled.
    在 Control+C 事件 (SIGTERM) 之后启用 java.util.concurrent 锁的打印。 默认情况下，此选项被禁用。

    Setting this option is equivalent to running the jstack -l command or the jcmd pid Thread.print -l command, where pid is the current Java process identifier.
    设置该选项等同于运行 jstack -l 命令或 jcmd pid Thread.print -l 命令，其中 pid 是当前 Java 进程标识符。
    
- **-XX:+UnlockDiagnosticVMOptions**
    Unlocks the options intended for diagnosing the JVM. By default, this option is disabled and diagnostic options are not available.
    解锁用于诊断 JVM 的选项。默认情况下，此选项被禁用并且诊断选项不可用。

<a id="Advanced_Garbage_Collection_Options"></a>**dvanced Garbage Collection Options**
These options control how garbage collection (GC) is performed by the Java HotSpot VM.
这些选项控制 Java HotSpot VM 执行垃圾收集 (GC) 的方式。

- **-XX:ActiveProcessorCount=x**
    Overrides the number of CPUs that the VM uses to calculate the size of thread pools it uses for various operations such as Garbage Collection and ForkJoinPool.
    覆盖 VM 用于计算线程池大小的 CPU 数量，它用于各种操作，例如垃圾收集和 ForkJoinPool【入池】。

    The VM normally determines the number of available processors from the operating system. This flag can be useful for partitioning CPU resources when running multiple Java processes in docker containers. This flag is honored even if UseContainerSupport is not enabled. See -XX:-UseContainerSupport for a description of enabling and disabling container support.
    VM 通常从操作系统中确定可用处理器的数量。 在 docker 容器中运行多个 Java 进程时，此标志可用于分区 CPU 资源。 即使未启用 UseContainerSupport，也会使用此标志。 有关启用和禁用容器支持的说明，请参阅 -XX:-UseContainerSupport。

- **-XX:+AggressiveHeap**
    Enables Java heap optimization. This sets various parameters to be optimal for long-running jobs with intensive memory allocation, based on the configuration of the computer (RAM and CPU). By default, the option is disabled and the heap is not optimized.
    启用 Java 堆优化。这会根据计算机的配置（RAM 和 CPU）为具有密集内存分配的长时间运行的作业设置最佳参数。默认情况下，该选项被禁用并且堆未被优化。

- **-XX:+AlwaysPreTouch**
    Enables touching of every page on the Java heap during JVM initialization. This gets all pages into the memory before entering the main() method. The option can be used in testing to simulate a long-running system with all virtual memory mapped to physical memory. By default, this option is disabled and all pages are committed as JVM heap space fills.
    在 JVM 初始化期间启用接触 Java 堆上的每个页面。这将在进入该main()方法之前将所有页面放入内存。该选项可用于测试以模拟长时间运行的系统，其中所有虚拟内存都映射到物理内存。默认情况下，此选项被禁用，所有页面都在 JVM 堆空间填满时提交。

- **-XX:+CMSClassUnloadingEnabled**
    Enables class unloading when using the concurrent mark-sweep (CMS) garbage collector. This option is enabled by default. To disable class unloading for the CMS garbage collector, specify `-XX:-CMSClassUnloadingEnabled`.
    使用并发标记清除 (CMS) 垃圾收集器时启用类卸载。默认情况下启用此选项。要禁用 CMS 垃圾收集器的类卸载，请指定-XX:-CMSClassUnloadingEnabled.

- **-XX:CMSExpAvgFactor=percent**
    Sets the percentage of time (0 to 100) used to weight the current sample when computing exponential averages for the concurrent collection statistics. By default, the exponential averages factor is set to 25%. The following example shows how to set the factor to 15%:
    设置在计算并发收集统计信息的指数平均值时用于对当前样本加权的时间百分比（0 到 100）。默认情况下，指数平均因子设置为 25%。以下示例显示如何将因子设置为 15%：

        -XX:CMSExpAvgFactor=15

- **-XX:CMSInitiatingOccupancyFraction=percent**
    Sets the percentage of the old generation occupancy (0 to 100) at which to start a CMS collection cycle. The default value is set to -1. Any negative value (including the default) implies that -XX:CMSTriggerRatio is used to define the value of the initiating occupancy fraction.
    设置开始 CMS 收集周期的老年代占用百分比（0 到 100）。默认值设置为 -1。任何负值（包括默认值）都意味着`-XX:CMSTriggerRatio`用于定义初始占用率的值。

    The following example shows how to set the occupancy fraction to 20%:
    以下示例显示如何将占用率设置为 20%：

        -XX:CMSInitiatingOccupancyFraction=20
        
- **-XX:+CMSScavengeBeforeRemark**
    Enables scavenging attempts before the CMS remark step. By default, this option is disabled.
    在 CMS 备注步骤之前启用清理尝试。默认情况下，此选项被禁用。

- **-XX:CMSTriggerRatio=percent**
    Sets the percentage (0 to 100) of the value specified by -XX:MinHeapFreeRatio that is allocated before a CMS collection cycle commences. The default value is set to 80%.
    `-XX:MinHeapFreeRatio`设置在CMS 收集周期开始之前分配的指定值的百分比（0 到 100） 。默认值设置为 80%。

    The following example shows how to set the occupancy fraction to 75%:
    以下示例显示如何将占用率设置为 75%：

        -XX:CMSTriggerRatio=75
        
- **-XX:ConcGCThreads=threads**
    Sets the number of threads used for concurrent GC. The default value depends on the number of CPUs available to the JVM.
    设置用于并发 GC 的线程数。默认值取决于 JVM 可用的 CPU 数量。

    For example, to set the number of threads for concurrent GC to 2, specify the following option:
    例如，要将并发 GC 的线程数设置为 2，请指定以下选项：

        -XX:ConcGCThreads=2
        
- **-XX:+DisableExplicitGC**
    Enables the option that disables processing of calls to System.gc(). This option is disabled by default, meaning that calls to System.gc() are processed. If processing of calls to System.gc() is disabled, the JVM still performs GC when necessary.
    启用禁用对 的调用处理的选项System.gc()。默认情况下禁用此选项，这意味着System.gc()处理对的调用。如果禁用对调用的处理System.gc()，JVM 仍会在必要时执行 GC。

- **-XX:+ExplicitGCInvokesConcurrent**
    Enables invoking of concurrent GC by using the System.gc() request. This option is disabled by default and can be enabled only together with the -XX:+UseConcMarkSweepGC option.
    通过使用System.gc()请求启用并发 GC 调用。默认情况下禁用此选项，只能与该-XX:+UseConcMarkSweepGC选项一起启用。

- **-XX:+ExplicitGCInvokesConcurrentAndUnloadsClasses**
    Enables invoking of concurrent GC by using the System.gc() request and unloading of classes during the concurrent GC cycle. This option is disabled by default and can be enabled only together with the -XX:+UseConcMarkSweepGC option.
    System.gc()通过在并发 GC 周期中使用类的请求和卸载来启用并发 GC 的调用。默认情况下禁用此选项，只能与该-XX:+UseConcMarkSweepGC选项一起启用。

- **-XX:G1HeapRegionSize=size**
    Sets the size of the regions into which the Java heap is subdivided when using the garbage-first (G1) collector. The value can be between 1 MB and 32 MB. The default region size is determined ergonomically based on the heap size.
    设置使用垃圾优先 (G1) 收集器时 Java 堆被细分的区域大小。该值可以介于 1 MB 和 32 MB 之间。默认区域大小是根据堆大小根据ergonomically(人体工程学-不知道是啥玩意)确定的。

    The following example shows how to set the size of the subdivisions to 16 MB:
    以下示例显示如何将细分的大小设置为 16 MB：

        -XX:G1HeapRegionSize=16m
        
- **-XX:+G1PrintHeapRegions**
    Enables the printing of information about which regions are allocated and which are reclaimed by the G1 collector. By default, this option is disabled.
    启用有关哪些区域已分配以及哪些区域已由 G1 收集器回收的信息的打印。默认情况下，此选项被禁用。

- **-XX:G1ReservePercent=percent**
    Sets the percentage of the heap (0 to 50) that is reserved as a false ceiling to reduce the possibility of promotion failure for the G1 collector. By default, this option is set to 10%.
    将保留的堆百分比（0 到 50）设置为假上限，以减少 G1 收集器提升失败的可能性。默认情况下，此选项设置为 10%。

    The following example shows how to set the reserved heap to 20%:
    以下示例显示如何将保留堆设置为 20%：

        -XX:G1ReservePercent=20
        
- **-XX:InitialHeapSize=size**
    Sets the initial size (in bytes) of the memory allocation pool. This value must be either 0, or a multiple of 1024 and greater than 1 MB. Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes. The default value is chosen at runtime based on system configuration. See the section "Ergonomics" in Java SE HotSpot Virtual Machine Garbage Collection Tuning Guide at http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/index.html.
    设置内存分配池的初始大小（以字节为单位）。此值必须为 0 或 1024 的倍数且大于 1 MB。附加字母k或K表示千字节，m或M表示兆字节，g或G表示千兆字节。默认值是在运行时根据系统配置选择的。请参阅Java SE HotSpot 虚拟机垃圾收集调优指南中的“人体工程学”部分http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/index.html。

    The following examples show how to set the size of allocated memory to 6 MB using various units:
    以下示例显示如何使用各种单位将已分配内存的大小设置为 6 MB：

        -XX:InitialHeapSize=6291456
        -XX:InitialHeapSize=6144k
        -XX:InitialHeapSize=6m
    If you set this option to 0, then the initial size will be set as the sum of the sizes allocated for the old generation and the young generation. The size of the heap for the young generation can be set using the -XX:NewSize option.
    如果将此选项设置为 0，则初始大小将设置为分配给老年代和年轻代的大小之和。可以使用该-XX:NewSize选项设置年轻代堆的大小。

    Note that the -Xms option sets both the minimum and the initial heap size of the heap. If -Xms appears after -XX:InitialHeapSize on the command line, then the initial heap size gets set to the value specified with -Xms.
    请注意，该-Xms选项设置堆的最小堆大小和初始堆大小。如果在命令行-Xms之后出现-XX:InitialHeapSize，则初始堆大小将设置为使用 指定的值-Xms。
    
- **-XX:InitialRAMPercentage=percent**
    Sets the initial amount of memory that the JVM will use for the Java heap before applying ergonomics heuristics as a percentage of the maximum amount determined as described in the -XX:MaxRAM option. The default value is 1.5625 percent.
    将 JVM 在应用人体工程学试探法之前用于 Java 堆的初始内存量设置为按-XX:MaxRAM选项中所述确定的最大内存量的百分比。默认值为 1.5625%。

    The following example shows how to set the percentage of the initial amount of memory used for the Java heap:
    以下示例显示如何设置用于 Java 堆的初始内存量的百分比：

        -XX:InitialRAMPercentage=5
        
- **-XX:InitialSurvivorRatio=ratio**
    Sets the initial survivor space ratio used by the throughput garbage collector (which is enabled by the -XX:+UseParallelGC and/or -XX:+UseParallelOldGC options). Adaptive sizing is enabled by default with the throughput garbage collector by using the -XX:+UseParallelGC and -XX:+UseParallelOldGC options, and survivor space is resized according to the application behavior, starting with the initial value. If adaptive sizing is disabled (using the -XX:-UseAdaptiveSizePolicy option), then the -XX:SurvivorRatio option should be used to set the size of the survivor space for the entire execution of the application.
    设置吞吐量垃圾收集器使用的初始幸存者空间比率（由-XX:+UseParallelGC和/或 -XX:+UseParallelOldGC选项启用）。默认情况下，吞吐量垃圾收集器通过使用-XX:+UseParallelGC和-XX:+UseParallelOldGC选项启用自适应大小调整，并且根据应用程序行为调整幸存者空间的大小，从初始值开始。如果禁用自适应大小调整（使用该-XX:-UseAdaptiveSizePolicy选项），-XX:SurvivorRatio则应使用该选项为应用程序的整个执行设置幸存者空间的大小。

    The following formula can be used to calculate the initial size of survivor space (S) based on the size of the young generation (Y), and the initial survivor space ratio (R):
    根据年轻代的大小（Y）和初始幸存者空间比例（R），可以使用以下公式计算幸存者空间的初始大小（S）：

        S=Y/(R+2)
    The 2 in the equation denotes two survivor spaces. The larger the value specified as the initial survivor space ratio, the smaller the initial survivor space size.
    等式中的 2 表示两个幸存者空间。指定为初始幸存者空间比例的值越大，初始幸存者空间大小越小。

    By default, the initial survivor space ratio is set to 8. If the default value for the young generation space size is used (2 MB), the initial size of the survivor space will be 0.2 MB.
    默认情况下，初始survivor空间比例设置为8。如果新生代空间大小使用默认值（2MB），则survivor空间的初始大小将为0.2MB。

    The following example shows how to set the initial survivor space ratio to 4:
    以下示例显示如何将初始幸存者空间比率设置为 4：

        -XX:InitialSurvivorRatio=4
    
- **-XX:InitiatingHeapOccupancyPercent=percent**
    Sets the percentage of the heap occupancy (0 to 100) at which to start a concurrent GC cycle. It is used by garbage collectors that trigger a concurrent GC cycle based on the occupancy of the entire heap, not just one of the generations (for example, the G1 garbage collector).
    设置开始并发 GC 周期的堆占用百分比（0 到 100）。它被垃圾收集器使用，根据整个堆的占用情况触发并发 GC 周期，而不仅仅是其中一个代（例如，G1 垃圾收集器）

    By default, the initiating value is set to 45%. A value of 0 implies nonstop GC cycles. The following example shows how to set the initiating heap occupancy to 75%:
    默认情况下，初始值设置为 45%。值为 0 意味着不间断的 GC 周期。以下示例显示如何将启动堆占用率设置为 75%：

        -XX:InitiatingHeapOccupancyPercent=75
        
- **-XX:MaxGCPauseMillis=time**
    Sets a target for the maximum GC pause time (in milliseconds). This is a soft goal, and the JVM will make its best effort to achieve it. By default, there is no maximum pause time value.
    设置最大 GC 暂停时间的目标（以毫秒为单位）。这是一个软目标，JVM 将尽最大努力实现它。默认情况下，没有最大暂停时间值。

    The following example shows how to set the maximum target pause time to 500 ms:
    以下示例显示如何将最大目标暂停时间设置为 500 毫秒：

        -XX:MaxGCPauseMillis=500
        
- **-XX:MaxHeapSize=size**
    Sets the maximum size (in byes) of the memory allocation pool. This value must be a multiple of 1024 and greater than 2 MB. Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes. The default value is chosen at runtime based on system configuration. For server deployments, -XX:InitialHeapSize and -XX:MaxHeapSize are often set to the same value. See the section "Ergonomics" in Java SE HotSpot Virtual Machine Garbage Collection Tuning Guide at http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/index.html.
    设置内存分配池的最大大小（以字节为单位）。此值必须是 1024 的倍数且大于 2 MB。附加字母k或K表示千字节，m或M表示兆字节，g或G表示千兆字节。默认值是在运行时根据系统配置选择的。对于服务器部署，-XX:InitialHeapSize通常-XX:MaxHeapSize设置为相同的值。请参阅Java SE HotSpot 虚拟机垃圾收集调优指南中的“人体工程学”部分http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/index.html。

    The following examples show how to set the maximum allowed size of allocated memory to 80 MB using various units:
    以下示例显示如何使用各种单位将分配内存的最大允许大小设置为 80 MB：

        -XX:MaxHeapSize=83886080
        -XX:MaxHeapSize=81920k
        -XX:MaxHeapSize=80m
    On Oracle Solaris 7 and Oracle Solaris 8 SPARC platforms, the upper limit for this value is approximately 4,000 MB minus overhead amounts. On Oracle Solaris 2.6 and x86 platforms, the upper limit is approximately 2,000 MB minus overhead amounts. On Linux platforms, the upper limit is approximately 2,000 MB minus overhead amounts.
    在 Oracle Solaris 7 和 Oracle Solaris 8 SPARC 平台上，此值的上限约为 4,000 MB 减去开销量。在 Oracle Solaris 2.6 和 x86 平台上，上限约为 2,000 MB 减去开销量。在 Linux 平台上，上限约为 2,000 MB 减去开销量。

    The -XX:MaxHeapSize option is equivalent to `-Xmx`.
    该-XX:MaxHeapSize选项相当于-Xmx.
- **-XX:MaxHeapFreeRatio=percent**
    Sets the maximum allowed percentage of free heap space (0 to 100) after a GC event. If free heap space expands above this value, then the heap will be shrunk. By default, this value is set to 70%.
    设置 GC 事件后可用堆空间的最大允许百分比（0 到 100）。如果可用堆空间扩展到该值以上，则堆将收缩。默认情况下，此值设置为 70%。

    The following example shows how to set the maximum free heap ratio to 75%:
    以下示例显示如何将最大可用堆比率设置为 75%：

        -XX:MaxHeapFreeRatio=75
        
- **-XX:MaxMetaspaceSize=size**
    Sets the maximum amount of native memory that can be allocated for class metadata. By default, the size is not limited. The amount of metadata for an application depends on the application itself, other running applications, and the amount of memory available on the system.
    设置可以分配给类元数据的最大本机内存量。默认情况下，大小不受限制。应用程序的元数据量取决于应用程序本身、其他正在运行的应用程序以及系统上可用的内存量。

    The following example shows how to set the maximum class metadata size to 256 MB:
    以下示例显示如何将最大类元数据大小设置为 256 MB：

        -XX:MaxMetaspaceSize=256m

- **-XX:MaxNewSize=size**
    Sets the maximum size (in bytes) of the heap for the young generation (nursery). The default value is set ergonomically.
    为年轻一代（新生代）设置堆的最大大小（以字节为单位）。 默认值是根据人体工程学设置的。
    
- **-XX:MaxRAMPercentage=percent**
    Sets the maximum amount of memory that the JVM may use for the Java heap before applying ergonomics heuristics as a percentage of the maximum amount determined as described in the -XX:MaxRAM option. The default value is 25 percent.
    将 JVM 在应用人体工程学试探法之前可用于 Java 堆的最大内存量设置为按-XX:MaxRAM选项中所述确定的最大内存量的百分比。默认值为 25%。

    Specifying this option disables automatic use of compressed oops if the combined result of this and other options influencing the maximum amount of memory is larger than the range of memory addressable by compressed oops. See `-XX:UseCompressedOops` for further information about compressed oops.
    如果此选项和影响最大内存量的其他选项的组合结果大于压缩 oops 可寻址的内存范围，则指定此选项将禁用压缩 oops 的自动使用。有关压缩 oops 的更多信息，请参阅`XX:UseCompressedOops`。

    The following example shows how to set the percentage of the maximum amount of memory used for the Java heap:
    以下示例显示如何设置用于 Java 堆的最大内存量的百分比：

        -XX:MaxRAMPercentage=75
        
- **-XX:MaxTenuringThreshold=threshold**
    Sets the maximum tenuring threshold for use in adaptive GC sizing. The largest value is 15. The default value is 15 for the parallel (throughput) collector, and 6 for the CMS collector.
    设置用于自适应 GC 大小调整的最大使用期阈值。最大值为 15。并行（吞吐量）收集器的默认值为 15，CMS 收集器的默认值为 6。

    The following example shows how to set the maximum tenuring threshold to 10:
    以下示例显示如何将最大任期阈值设置为 10：

        -XX:MaxTenuringThreshold=10

- **-XX:MetaspaceSize=size**
    Sets the size of the allocated class metadata space that will trigger a garbage collection the first time it is exceeded. This threshold for a garbage collection is increased or decreased depending on the amount of metadata used. The default size depends on the platform.
    设置分配的类元数据空间的大小，该空间将在第一次超出时触发垃圾回收。垃圾回收的阈值根据使用的元数据量增加或减少。默认大小取决于平台。

- **-XX:MinHeapFreeRatio=percent**
    Sets the minimum allowed percentage of free heap space (0 to 100) after a GC event. If free heap space falls below this value, then the heap will be expanded. By default, this value is set to 40%.
    设置 GC 事件后可用堆空间的最小允许百分比（0 到 100）。如果可用堆空间低于此值，则将扩展堆。默认情况下，此值设置为 40%。

    The following example shows how to set the minimum free heap ratio to 25%:
    以下示例显示如何将最小空闲堆比率设置为 25%：

        -XX:MinHeapFreeRatio=25
    
- **-XX:MinRAMPercentage=percent**
    Sets the maximum amount of memory that the JVM may use for the Java heap before applying ergonomics heuristics as a percentage of the maximum amount determined as described in the -XX:MaxRAM option for small heaps. A small heap is a heap of approximately 125 MB. The default value is 50 percent.
    将 JVM 在应用人体工程学试探法之前可用于 Java 堆的最大内存量设置为最大内存量的百分比，如-XX:MaxRAM小堆选项中所述。小堆是大约 125 MB 的堆。默认值为 50%。

    The following example shows how to set the percentage of the maximum amount of memory used for the Java heap for small heaps:
    以下示例显示了如何设置用于小型堆的 Java 堆的最大内存量的百分比：

        -XX:MinRAMPercentage=75

- **-XX:NewRatio=ratio**
    Sets the ratio between young and old generation sizes. By default, this option is set to 2. The following example shows how to set the young/old ratio to 1:
    设置年轻代和老年代大小之间的比例。默认情况下，此选项设置为 2。以下示例显示如何将年轻/年老比率设置为 1：

        -XX:NewRatio=1
        
- **-XX:NewSize=size**
    Sets the initial size (in bytes) of the heap for the young generation (nursery). Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes.
    为年轻一代（新生代）设置堆的初始大小（以字节为单位）。附加字母k或K表示千字节，m或M表示兆字节，g或G表示千兆字节。

    The young generation region of the heap is used for new objects. GC is performed in this region more often than in other regions. If the size for the young generation is too low, then a large number of minor GCs will be performed. If the size is too high, then only full GCs will be performed, which can take a long time to complete. Oracle recommends that you keep the size for the young generation between a half and a quarter of the overall heap size.
    堆的年轻代区域用于新对象。GC 在这个区域比在其他区域更频繁地执行。如果年轻代的大小太小，那么将会执行大量的次要 GC。如果大小太高，则只会执行完整的 GC，这可能需要很长时间才能完成。Oracle 建议您将新生代的大小保持在整个堆大小的一半到四分之一之间。

    The following examples show how to set the initial size of young generation to 256 MB using various units:
    以下示例显示如何使用各种单位将年轻代的初始大小设置为 256 MB：

        -XX:NewSize=256m
        -XX:NewSize=262144k
        -XX:NewSize=268435456
    The -XX:NewSize option is equivalent to -Xmn.
    该-XX:NewSize选项相当于-Xmn.
    
- **-XX:ParallelGCThreads=threads**
    Sets the number of threads used for parallel garbage collection in the young and old generations. The default value depends on the number of CPUs available to the JVM.
    设置用于年轻代和老年代并行垃圾回收的线程数。默认值取决于 JVM 可用的 CPU 数量。

    For example, to set the number of threads for parallel GC to 2, specify the following option:
    例如，要将并行 GC 的线程数设置为 2，请指定以下选项：

        -XX:ParallelGCThreads=2
        
- **-XX:+ParallelRefProcEnabled**
    Enables parallel reference processing. By default, this option is disabled.
    启用并行引用处理。默认情况下，此选项被禁用。

- **-XX:+PrintAdaptiveSizePolicy**
    Enables printing of information about adaptive generation sizing. By default, this option is disabled.
    启用有关自适应生成大小调整的信息的打印。默认情况下，此选项被禁用。

- **-XX:+PrintGC**
    Enables printing of messages at every GC. By default, this option is disabled.
    在每次 GC 时启用消息打印。默认情况下，此选项被禁用。

- **-XX:+PrintGCApplicationConcurrentTime**
    Enables printing of how much time elapsed since the last pause (for example, a GC pause). By default, this option is disabled.
    启用打印自上次暂停（例如，GC 暂停）以来经过的时间。默认情况下，此选项被禁用。

- **-XX:+PrintGCApplicationStoppedTime**
    Enables printing of how much time the pause (for example, a GC pause) lasted. By default, this option is disabled.
    允许打印暂停（例如，GC 暂停）持续了多长时间。默认情况下，此选项被禁用。

- **-XX:+PrintGCDateStamps**
    Enables printing of a date stamp at every GC. By default, this option is disabled.
    启用在每个 GC 上打印日期戳。默认情况下，此选项被禁用。

- **-XX:+PrintGCDetails**
    Enables printing of detailed messages at every GC. By default, this option is disabled.
    允许在每次 GC 时打印详细消息。默认情况下，此选项被禁用。

- **-XX:+PrintGCTaskTimeStamps**
    Enables printing of time stamps for every individual GC worker thread task. By default, this option is disabled.
    为每个单独的 GC 工作线程任务启用时间戳打印。默认情况下，此选项被禁用。

- **-XX:+PrintGCTimeStamps**
    Enables printing of time stamps at every GC. By default, this option is disabled.
    在每个 GC 上启用时间戳打印。默认情况下，此选项被禁用。

- **-XX:+PrintStringDeduplicationStatistics**
    Prints detailed deduplication statistics. By default, this option is disabled. See the -XX:+UseStringDeduplication option.
    打印详细的重复数据删除统计信息。默认情况下，此选项被禁用。查看`-XX:+UseStringDeduplication`选项。

- **-XX:+PrintTenuringDistribution**
    Enables printing of tenuring age information. The following is an example of the output:
    启用终身年龄信息的打印。以下是输出示例：

        Desired survivor size 48286924 bytes, new threshold 10 (max 10)
        - age 1: 28992024 bytes, 28992024 total
        - age 2: 1366864 bytes, 30358888 total
        - age 3: 1425912 bytes, 31784800 total
        ...
    Age 1 objects are the youngest survivors (they were created after the previous scavenge, survived the latest scavenge, and moved from eden to survivor space). Age 2 objects have survived two scavenges (during the second scavenge they were copied from one survivor space to the next). And so on.
    年龄为 1 的对象是最年轻的幸存者（它们是在上一次清除之后创建的，在最近一次清除中幸存下来，并从伊甸园移动到幸存者空间）。年龄 2 的对象在两次清除中幸存下来（在第二次清除期间，它们被从一个幸存者空间复制到下一个幸存者空间）。等等。

    In the preceding example, 28 992 024 bytes survived one scavenge and were copied from eden to survivor space, 1 366 864 bytes are occupied by age 2 objects, etc. The third value in each row is the cumulative size of objects of age n or less.
    在前面的示例中，有 28 992 024 字节在一次清除后幸存并从伊甸园复制到幸存者空间，1 366 864 字节被年龄为 2 的对象占用，等等。每行中的第三个值是年龄为 n 或较少的。

    By default, this option is disabled.
    默认情况下，此选项被禁用。

- **-XX:+ScavengeBeforeFullGC**
    Enables GC of the young generation before each full GC. This option is enabled by default. Oracle recommends that you do not disable it, because scavenging the young generation before a full GC can reduce the number of objects reachable from the old generation space into the young generation space. To disable GC of the young generation before each full GC, specify -XX:-ScavengeBeforeFullGC.
    在每次完整 GC 之前启用年轻代的 GC。 默认情况下启用此选项。 Oracle 建议您不要禁用它，因为在 full GC 之前清除年轻代会减少从老年代空间到达年轻代空间的对象数量。 要在每次完整 GC 之前禁用年轻代的 GC，请指定 `-XX:-ScavengeBeforeFullGC`。

- ** -XX:SoftRefLRUPolicyMSPerMB=time**
    Sets the amount of time (in milliseconds) a softly reachable object is kept active on the heap after the last time it was referenced. The default value is one second of lifetime per free megabyte in the heap. The -XX:SoftRefLRUPolicyMSPerMB option accepts integer values representing milliseconds per one megabyte of the current heap size (for Java HotSpot Client VM) or the maximum possible heap size (for Java HotSpot Server VM). This difference means that the Client VM tends to flush soft references rather than grow the heap, whereas the Server VM tends to grow the heap rather than flush soft references. In the latter case, the value of the -Xmx option has a significant effect on how quickly soft references are garbage collected.
    设置软可达对象在上次被引用后在堆上保持活动状态的时间量（以毫秒为单位）。 默认值是堆中每兆字节的生命周期一秒。 -XX:SoftRefLRUPolicyMSPerMB 选项接受整数值，表示当前堆大小（对于 Java HotSpot Client VM）或最大可能堆大小（对于 Java HotSpot Server VM）每兆字节的毫秒数。 这种差异意味着客户端 VM 倾向于刷新软引用而不是增大堆，而服务器 VM 倾向于增大堆而不是刷新软引用。 在后一种情况下，-Xmx 选项的值对软引用的垃圾回收速度有重大影响。


    The following example shows how to set the value to 2.5 seconds:
    以下示例显示如何将值设置为 2.5 秒：

        -XX:SoftRefLRUPolicyMSPerMB=2500
- **-XX:StringDeduplicationAgeThreshold=threshold**
    String objects reaching the specified age are considered candidates for deduplication. An object's age is a measure of how many times it has survived garbage collection. This is sometimes referred to as tenuring; see the -XX:+PrintTenuringDistribution option. Note that String objects that are promoted to an old heap region before this age has been reached are always considered candidates for deduplication. The default value for this option is 3. See the -XX:+UseStringDeduplication option.
    达到指定年龄的字符串对象被视为重复数据删除的候选对象。 一个对象的年龄是衡量它在垃圾收集中幸存下来的次数。 这有时被称为任期； 请参阅 `-XX:+PrintTenuringDistribution` 选项。 请注意，在达到此年龄之前提升到旧堆区域的 String 对象始终被视为重复数据删除的候选对象。 此选项的默认值为 3。请参阅 `-XX:+UseStringDeduplication` 选项。

- **-XX:SurvivorRatio=ratio**
    Sets the ratio between eden space size and survivor space size. By default, this option is set to 8. The following example shows how to set the eden/survivor space ratio to 4:
    设置伊甸园空间大小和幸存者空间大小之间的比率。默认情况下，此选项设置为 8。以下示例显示如何将 eden/survivor 空间比率设置为 4：

        -XX:SurvivorRatio=4
- **-XX:TargetSurvivorRatio=percent**
    Sets the desired percentage of survivor space (0 to 100) used after young garbage collection. By default, this option is set to 50%.
    设置年轻垃圾收集后使用的所需幸存者空间百分比（0 到 100）。默认情况下，此选项设置为 50%。

    The following example shows how to set the target survivor space ratio to 30%:
    以下示例显示如何将目标幸存者空间比率设置为 30%：

        -XX:TargetSurvivorRatio=30
- **-XX:TLABSize=size**
    Sets the initial size (in bytes) of a thread-local allocation buffer (TLAB). Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes. If this option is set to 0, then the JVM chooses the initial size automatically.
    设置线程本地分配缓冲区 (TLAB) 的初始大小（以字节为单位）。附加字母k或K表示千字节，m或M表示兆字节，g或G表示千兆字节。如果此选项设置为 0，则 JVM 会自动选择初始大小。

    The following example shows how to set the initial TLAB size to 512 KB:
    以下示例显示如何将初始 TLAB 大小设置为 512 KB：

        -XX:TLABSize=512k
- **-XX:+UseAdaptiveSizePolicy**
    Enables the use of adaptive generation sizing. This option is enabled by default. To disable adaptive generation sizing, specify -XX:-UseAdaptiveSizePolicy and set the size of the memory allocation pool explicitly (see the -XX:SurvivorRatio option).
    启用自适应生成大小调整的使用。默认情况下启用此选项。要禁用自适应生成大小调整，请明确指定`-XX:-UseAdaptiveSizePolicy`和设置内存分配池的大小（请参阅`-XX:SurvivorRatio`选项）。

- **-XX:+UseCMSInitiatingOccupancyOnly**
    Enables the use of the occupancy value as the only criterion for initiating the CMS collector. By default, this option is disabled and other criteria may be used.
    允许使用占用值作为启动 CMS 收集器的唯一标准。默认情况下，此选项被禁用，可以使用其他标准。

- **-XX:+UseConcMarkSweepGC**
    Enables the use of the CMS garbage collector for the old generation. Oracle recommends that you use the CMS garbage collector when application latency requirements cannot be met by the throughput (-XX:+UseParallelGC) garbage collector. The G1 garbage collector (-XX:+UseG1GC) is another alternative.
    为老年代启用 CMS 垃圾收集器。 当吞吐量 (-XX:+UseParallelGC) 垃圾收集器无法满足应用程序延迟要求时，Oracle 建议您使用 CMS 垃圾收集器。 G1 垃圾收集器 (-XX:+UseG1GC) 是另一种选择。

    By default, this option is disabled and the collector is chosen automatically based on the configuration of the machine and type of the JVM. When this option is enabled, the -XX:+UseParNewGC option is automatically set and you should not disable it, because the following combination of options has been deprecated in JDK 8: -XX:+UseConcMarkSweepGC -XX:-UseParNewGC.
    默认情况下，这个选项是禁用的，收集器是根据机器的配置和 JVM 的类型自动选择的。 启用此选项后，将自动设置 `-XX:+UseParNewGC` 选项，您不应禁用它，因为在 JDK 8 中已弃用以下选项组合：`-XX:+UseConcMarkSweepGC -XX:-UseParNewGC`。

- **-XX:+UseG1GC**
    Enables the use of the garbage-first (G1) garbage collector. It is a server-style garbage collector, targeted for multiprocessor machines with a large amount of RAM. It meets GC pause time goals with high probability, while maintaining good throughput. The G1 collector is recommended for applications requiring large heaps (sizes of around 6 GB or larger) with limited GC latency requirements (stable and predictable pause time below 0.5 seconds).
    启用垃圾优先 (G1) 垃圾收集器的使用。 它是一种服务器风格的垃圾收集器，针对具有大量 RAM 的多处理器机器。 它很有可能满足 GC 暂停时间目标，同时保持良好的吞吐量。 建议将 G1 收集器用于需要大堆（大小约为 6 GB 或更大）且 GC 延迟要求有限（稳定且可预测的暂停时间低于 0.5 秒）的应用程序。

    By default, this option is disabled and the collector is chosen automatically based on the configuration of the machine and type of the JVM.
    默认情况下，这个选项是禁用的，收集器是根据机器的配置和 JVM 的类型自动选择的。

- **-XX:+UseGCOverheadLimit**
    Enables the use of a policy that limits the proportion of time spent by the JVM on GC before an OutOfMemoryError exception is thrown. This option is enabled, by default and the parallel GC will throw an OutOfMemoryError if more than 98% of the total time is spent on garbage collection and less than 2% of the heap is recovered. When the heap is small, this feature can be used to prevent applications from running for long periods of time with little or no progress. To disable this option, specify `-XX:-UseGCOverheadLimit`.
    在抛出 OutOfMemoryError 异常之前，允许使用限制 JVM 在 GC 上花费的时间比例的策略。 默认情况下启用此选项，如果超过 98% 的总时间花在垃圾收集上并且回收不到 2% 的堆，则并行 GC 将抛出 OutOfMemoryError。 当堆很小时，可以使用此功能来防止应用程序长时间运行而没有任何进展。 要禁用此选项，请指定 `-XX:-UseGCOverheadLimit`。

- **-XX:+UseNUMA**
    Enables performance optimization of an application on a machine with nonuniform memory architecture (NUMA) by increasing the application's use of lower latency memory. By default, this option is disabled and no optimization for NUMA is made. The option is only available when the parallel garbage collector is used (-XX:+UseParallelGC).
    通过增加应用程序对低延迟内存的使用，在具有非统一内存架构 (NUMA) 的机器上启用应用程序的性能优化。 默认情况下，此选项被禁用，并且不对 NUMA 进行优化。 该选项仅在使用并行垃圾收集器时可用 (`-XX:+UseParallelGC`)。

- **-XX:+UseParallelGC**
    Enables the use of the parallel scavenge garbage collector (also known as the throughput collector) to improve the performance of your application by leveraging multiple processors.
    启用并行清除垃圾收集器（也称为吞吐量收集器）以通过利用多个处理器来提高应用程序的性能。

    By default, this option is disabled and the collector is chosen automatically based on the configuration of the machine and type of the JVM. If it is enabled, then the -XX:+UseParallelOldGC option is automatically enabled, unless you explicitly disable it.
    默认情况下，这个选项是禁用的，收集器是根据机器的配置和 JVM 的类型自动选择的。 如果启用，则 `-XX:+UseParallelOldGC` 选项会自动启用，除非您明确禁用它。

- **-XX:+UseParallelOldGC**
    Enables the use of the parallel garbage collector for full GCs. By default, this option is disabled. Enabling it automatically enables the -XX:+UseParallelGC option.
    启用对完整 GC 的并行垃圾收集器的使用。 默认情况下，此选项被禁用。 启用它会自动启用 -XX:+UseParallelGC 选项。

- **-XX:+UseParNewGC**
    Enables the use of parallel threads for collection in the young generation. By default, this option is disabled. It is automatically enabled when you set the -XX:+UseConcMarkSweepGC option. Using the -XX:+UseParNewGC option without the -XX:+UseConcMarkSweepGC option was deprecated in JDK 8.
    启用在年轻代中使用并行线程进行收集。 默认情况下，此选项被禁用。 当您设置 `-XX:+UseConcMarkSweepGC` 选项时，它会自动启用。 在 JDK 8 中不推荐使用不带 `-XX:+UseConcMarkSweepGC` 选项的 `-XX:+UseParNewGC` 选项。

- **-XX:+UseSerialGC**
    Enables the use of the serial garbage collector. This is generally the best choice for small and simple applications that do not require any special functionality from garbage collection. By default, this option is disabled and the collector is chosen automatically based on the configuration of the machine and type of the JVM.
    启用串行垃圾收集器的使用。 这通常是不需要垃圾收集的任何特殊功能的小型和简单应用程序的最佳选择。 默认情况下，这个选项是禁用的，收集器是根据机器的配置和 JVM 的类型自动选择的。

- **-XX:+UseSHM**
    On Linux, enables the JVM to use shared memory to setup large pages.
    在 Linux 上，使 JVM 能够使用共享内存来设置大页面。

    For more information, see "Large Pages".

- **-XX:+UseStringDeduplication**
    Enables string deduplication. By default, this option is disabled. To use this option, you must enable the garbage-first (G1) garbage collector. See the -XX:+UseG1GC option.
    启用字符串去重。 默认情况下，此选项被禁用。 要使用此选项，您必须启用垃圾优先 (G1) 垃圾收集器。 请参阅 -XX:+UseG1GC 选项。

    String deduplication reduces the memory footprint of String objects on the Java heap by taking advantage of the fact that many String objects are identical. Instead of each String object pointing to its own character array, identical String objects can point to and share the same character array.
    字符串重复数据删除利用许多 String 对象相同这一事实减少了 Java 堆上 String 对象的内存占用。 相同的 String 对象可以指向并共享同一个字符数组，而不是每个 String 对象都指向它自己的字符数组。

- **-XX:+UseTLAB**
    Enables the use of thread-local allocation blocks (TLABs) in the young generation space. This option is enabled by default. To disable the use of TLABs, specify `-XX:-UseTLAB`.
    启用在新生代空间中使用线程本地分配块 (TLAB)。 默认情况下启用此选项。 要禁用 TLAB，请指定 `-XX:-UseTLAB`。
------------------------
弃用的命令就不再列出，主要是针对有需要查看命令的小伙伴可以查看
