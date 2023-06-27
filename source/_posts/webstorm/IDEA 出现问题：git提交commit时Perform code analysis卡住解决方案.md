---
title: IDEA 出现问题：git提交commit时Perform code analysis卡住解决方案
date: 2023-06-27 16:28:31
tags: ["webstorm","git"]
categories: ["webstorm"]
---

git提交commit时Perform code analysis卡住很久

## 解决方案
1、打开 IntelliJ IDEA，进入 File -> Settings（或者使用快捷键 Ctrl+Alt+S）。

2、在弹出的 Settings 窗口中，找到 Version Control -> Commit Dialog 选项。

3、在右侧的窗口中，找到 Perform code analysis 选项，并取消勾选该选项。然后单击 OK 按钮保存设置。

Perform code analysis:提交前代码分析

![](https://oss-wuhan.sangforcloud.com:12000/jhtech-fileserver-bucket/crm/68/954ad45cb4584dee88258887e904e879.png "")

4、关闭所有已打开的项目窗口，关闭IDEA,重新打开IDEA。

5、打开你需要提交代码的项目。然后尝试执行 git commit 操作，确保 Perform code analysis 选项没有被勾选上。
