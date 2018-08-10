# 项目正常编译运行，R文件报错

## 一：问题描述

项目能正常编译运行，R文件也能在目录下正常生成，但是UI里面R文件应用变红，不能正确跳转。

![](Resources\1.png)

## 二：问题分析

当我们点开build目录，找到生成的R文件的时候，可看到如下：

![](Resources\2.png)

可看到第一行的提示信息：

The file size(3.00M) exceeds configured limit (2.56M). Code insight features not available.

就是说R文件大小超过了默认大小限制，代码跳转功能不能使用。

## 三：解决方案



- 1：在文件 /AndroidStudio/bin/idea.properties 修改idea.max.intellisense.filesize的值，默认为2500，可适量增大。如下所示：

![](Resources\3.png)

![](Resources\4.png)

- 2：也可以在androidstudio中修改：

![](Resources\5.png)

![](Resources\6.png)

