+++
title= "[OpenGL]GLSL Cookbook源码编译步骤流程"
date= "2017-09-06T22:38:28+08:00"
categories= ["OpenGL"]
tags= ["Shading", "Graphics", "OpenGL"]
+++

{{< alert danger>}}
以下文章内容是基于glslcookbook所在github上2017年8月份的提交版本，当前文章所述的配置glm和glfw的步骤与github上最新CMakeList.txt已不再适用！针对2018.06的github代码，我新增了文章《[OpenGL]GLSL Cookbook源码编译步骤流程(2018.06新版)》。如果是萌新，建议先把当前旧文章看完再看新文章。
{{< /alert >}}



这里讲解《OpenGL 4 Shading Language Cookbook, Second Edition》 这本书的示例代码如何编译。

##### 编译步骤
 
1，下载glm的源码，并用cmake-gui生成出VisualStudio的工程文件。  
https://github.com/g-truc/glm/releases

2，下载glfw。  
https://github.com/glfw/glfw/releases  
注意是下载bin文件不是src源码。（如果你想用src编译lib，则无视）

3，下载glslcookbook源码，这是《OpenGL 4 Shading Language Cookbook, Second Edition》的书籍源码，下载后同样用cmake-gui生成vs工程文件：
https://github.com/daw42/glslcookbook

4，cmake-gui首次生成glshcookbook源码时会报错
{{< figure src="/img/20170906-[OpenGL]GLSHCookbook源码编译步骤流程/[OpenGL]GLSHCookbook源码编译步骤流程-01.jpg">}}
这时需要指定GLM的依赖库位置，这个位置就是步骤1中的生成的GLM库的bin目录。注意这个bin目录不是GLM自带的，是通过cmake-gui指定生成的。

在指定GLM_INCLUDE_DIR之后，再点击Generate，又会出现新的错误，提示GLFW找不到，这时需要再指定GLFW的相关目录，即步骤2中下载的GLFW lib和头文件。
{{< figure src="/img/20170906-[OpenGL]GLSHCookbook源码编译步骤流程/[OpenGL]GLSHCookbook源码编译步骤流程-02.jpg">}}

参数解释：

+ CMAKE_BUILD_TYPE ：默认，不做修改
+ CMAKE_CONFIGURATION_TYPES ：默认，不做修改
+ CMAKE_INSTALL_PREFIX ：不清楚，删掉也不影响
+ GLFW3_INCLUDE_DIR ：GLFW的头文件目录
+ GLFW3_LIBRARY ：GLFW的静态库目录
+ GLM_INCLUDE_DIR ：GLM的头文件目录

最后生成成功的提示为：
{{< figure src="/img/20170906-[OpenGL]GLSHCookbook源码编译步骤流程/[OpenGL]GLSHCookbook源码编译步骤流程-03.jpg">}}

打开VS工程，可以看到每一章的示例代码并可编译。（这里看不到.sln的图标是因为我电脑之前卸载了旧版本的vs，导致新版本vs的图标显示不正常，可以无视）
{{< figure src="/img/20170906-[OpenGL]GLSHCookbook源码编译步骤流程/[OpenGL]GLSHCookbook源码编译步骤流程-04.jpg">}}
{{< figure src="/img/20170906-[OpenGL]GLSHCookbook源码编译步骤流程/[OpenGL]GLSHCookbook源码编译步骤流程-05.jpg">}}


##### 运行程序

每个章节的exe无法双击运行，需要命令输入相关参数。
以章节5为例，先CD到exe所在目录：

	cd /d D:\glslcookbook\bin\chapter05\Debug>
	
然后输入：

	chapter05.exe
	
则会提示：

	Recipe names:
	  blur     : Gaussian blur
	  deferred : deferred rendering
	  edge     : edge detection filter
	  gamma    : gamma correction
	  msaa     : multisample anti-aliasing
	  tone-map : tone mapping example.
	  hdr-bloom: bloom example with HDR tone mapping.
	  oit      : order independent transparency

然后再CD上一层目录（因为程序中使用了相对路径：chapter05/shader，如果cd到Debug目录则会提示找不到这些shader）

	cd ..
	
然后执行（以hdr-bloom为例）：

	start Debug/chapter05.exe hdr-bloom

程序运行效果
{{< figure src="/img/20170906-[OpenGL]GLSHCookbook源码编译步骤流程/[OpenGL]GLSHCookbook源码编译步骤流程-05-1.jpg">}}

##### 相关错误：

1，之前在用源码生成GLFW库时，cmake老报一个错误：

    CMake Error at CMakeLists.txt:6 (find_package):
    Could not find a package configuration file provided by "glm" with any of
    the following names:

    glmConfig.cmake
    glm-config.cmake

    Add the installation prefix of "glm" to CMAKE_PREFIX_PATH or set "glm_DIR"
    to a directory containing one of the above files. If "glm" provides a
    separate development package or SDK, be sure it has been installed.
    Configuring incomplete, errors occurred!
    See also "D:/glslcookbook_bin/CMakeFiles/CMakeOutput.log".

原因：我电脑上之前用cmake-gui构建了几个不同目录的GLFW，可能导致GLFW的路径指向错误。  
解决办法：删除之前构建的GLFW库，新建一个目录重新生成GLFW库。

2，64位编译错误  
64位模式下，会提示找不到uint符号
{{< figure src="/img/20170906-[OpenGL]GLSHCookbook源码编译步骤流程/[OpenGL]GLSHCookbook源码编译步骤流程-06.jpg">}}

解决办法：将uint替换为GLuint。

***
`纵使相逢应不识，尘满面，鬓如霜。夜来幽梦忽还乡。小轩窗，正梳妆。----北宋·苏轼《江城子·乙卯正月二十日夜记梦》`