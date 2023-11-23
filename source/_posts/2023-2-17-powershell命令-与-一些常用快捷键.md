---
title: powershell命令 与 一些常用快捷键
tags:
  - idea
  - powershell
  - cmd
categories:
  - 记录一下
abbrlink: dae06e64
date: 2023-02-17 01:19:01
---



# 快捷键

> 记录一些常用快捷键和cmd命令

## pwoershell命令

- `powershell`就是`cmd`的增强版，命令几乎与`shell`中相同
- 推荐使用`powershell`，还兼容了`cmd`中的命令
- 官方手册: https://learn.microsoft.com/zh-cn/powershell
- https://learn.microsoft.com/zh-cn/powershell/module/microsoft.powershell.core/about/about_command_syntax?view=powershell-7.3#command-name

> 使用技巧: 对于需要经常使用的命令, 可以创建编辑后缀为`.bat`的文件，点击直接运行命令
>
> 或者使用后缀`.ps1`的powershell脚本文件

|        命令(不区分大小写)         | 作用                                                         |
| :-------------------------------: | ------------------------------------------------------------ |
|              `help`               | 查看所有可用命令及其简要说明                                 |
|             `help cd`             | 查看`cd`命令的详细使用说明                                   |
|           `powershell`            | `cmd`黑窗口进入`powershell`，路径前带`PS`，eg：`PS D:\>`     |
|                                   |                                                              |
|           `ping +域名`            | 解析域名IP地址                                               |
|   `netstat -ano |findstr "80"`    | 查找本机占用`80`端口的进程                                   |
|       `taskkill /pid 4 /f`        | 强制停止`pid`为`4`的进程                                     |
|                                   |                                                              |
|               `cd`                | 显示当前所在路径                                             |
|              `cd ..`              | 返回上一级                                                   |
|             `cd dir`              | 进入名为`dir`的文件夹                                        |
|               `d:`                | 切换到`D`盘                                                  |
|               `dir`               | 显示当前目录下的所有文件及文件夹, 类似`shell`中的`ls`，`powershell`支持`ls` |
|               `del`               | 删除文件                                                     |
|          `rd /s /q dir`           | 删除整个`dir`文件夹，不显示是否删除                          |
|            `Linux.md`             | 直接输入文件名, 将会使用默认程序打开文件，类似GUI中双击文件的效果 |
|         `type Linux.txt`          | 查看文本文件内容                                             |
|                                   |                                                              |
|               `cls`               | 清屏，类似`shell`中的`clear`                                 |
|                                   |                                                              |
| `iwr` / `wget` / `curl` + `<uri>` | Invoke-WebRequest, 解析web请求                               |

## IDEA

| 快捷键                                               | 备注                                                         |
| ---------------------------------------------------- | ------------------------------------------------------------ |
| ***Ctrl+E***                                         | 最近打开的文件                                               |
| ***Ctrl+Shift+V***                                   | 历史粘贴板                                                   |
| ***Ctrl+W***                                         | 按一定规则扩选                                               |
| ***Shift+F6***                                       | 选中目录结构的文件，重命名                                   |
| Ctrl + Shift + R                                     | 搜索指定范围文件，替换文字                                   |
| ***Ctrl+R***                                         | 当前文件 查询替换                                            |
| Ctrl + Tab                                           | 切换最近一个标签,类似window的Alt+tab效果                     |
| Ctrl+  [                                             | 移动光标到当前所在代码的花括号开始位置，反之移动到结束位置   |
| Ctrl + Shift + J                                     | 自动将下一行合并到当前行末尾                                 |
| Ctrl+N或 双击Shift                                   | 搜索文件/类                                                  |
| 光标在某一个词上-Ctrl+Q                              | 显示JavaDoc的结果                                            |
| Ctrl+Alt+S                                           | 打开设置                                                     |
| ***Ctrl+F***                                         | 搜索当前页面 词                                              |
| Shift+F2                                             | 提示当前页面可优化的地方                                     |
| ***Alt+F8***                                         | 输入变量名，查看当前变量的值--deBug时使用                    |
| ***Ctrl + G***                                       | 在当前文件跳转到指定行处                                     |
| Ctrl + J                                             | 给出提示，例：输入sout如果提示消失，Ctrl+J给出提示           |
| Ctrl+P                                               | 输入方法名后，使用给出方法参数提示                           |
| ***Ctrl + U***                                       | 前往当前光标所在的方法的父类的方法 / 接口定义                |
| Ctrl+B  /  F4                                        | 选中方法后，进入光标所在的方法/变量的接口或是定义出，等效于 Ctrl + 左键单击 |
| ***Ctrl + H***                                       | 显示当前类的层次结构，继承或实现                             |
| ***Ctrl + O***                                       | 选择可重写的方法                                             |
| Ctrl + I                                             | 选择可继承的方法                                             |
| ***Ctrl + F12***                                     | 弹出当前文件结构层，可以在弹出的层上直接输入，进行筛选，类似Alt+7 |
| ***Ctrl + F11***                                     | 选中文件 / 文件夹  /某一行，使用助记符设定 / 取消书签        |
| F11                                                  | 选中文件 / 文件夹  /某一行,  标记/消除 书签                  |
| Shift + F11                                          | 弹出书签显示层                                               |
| Ctrl+ 1~9                                            | 跳转书签所标记数字的文件夹                                   |
| Ctrl + Home/End                                      | 跳到文件头/尾，等同于PgUp/PgDn                               |
| Ctrl + 前/后方向键                                   | 等效于鼠标滚轮向 前/后 效果                                  |
| Ctrl + 左/右方向键                                   | 光标跳转到当前单词 / 中文句的 左/右 侧开头位置               |
| Ctrl + T/K                                           | 版本控制 更新/提交 项目                                      |
| ***Alt + F3***                                       | 选中文本，逐个往下查找相同文本，并高亮显示                   |
| ***Alt + F7***                                       | 查找光标所在的方法 / 变量 / 类被调用的地方                   |
| Alt + 左/右方向键                                    | 按左/右 方向切换当前已打开的导航标签页                       |
| Alt + 前/后方向键                                    | 当前光标跳转到当前文件的前/后一个方法名位置                  |
| Shift + Home/End                                     | 选中光标到当前行头/尾位置                                    |
| Shift + 滚轮前后滚动                                 | 当前文件的横向滚动轴滚动                                     |
| Ctrl + Alt + O                                       | 优化导入的类，可以对当前文件和整个包目录使用 （必备）        |
| ***Ctrl + Shift + Z***                               | 取消撤销 （必备）                                            |
| Ctrl + Shift + U                                     | 对选中的代码进行大 / 小写轮流转换 （必备）                   |
| ***Ctrl+P***                                         | 光标在方法括号内,提示参数信息                                |
| Ctrl + Shift + Space                                 | 智能代码提示                                                 |
| ***Ctrl + Shift + Enter***                           | 自动结束代码，行末自动添加分号 （必备）                      |
| ***Ctrl + Shift + Alt + C***                         | 复制参考信息,可用于复制当前文件的**相对路径** ,注意:光标应在文件中空白地方 |
| ***Ctrl+Shift+C***                                   | 复制当前文件**绝对路径**                                     |
| Ctrl + Shift + Alt + V                               | 无格式黏贴                                                   |
| ***F2***                                             | 跳转到下一个高亮错误 或 警告位置 （必备）                    |
| F7                                                   | 在 Debug 模式下，进入下一步，如果当前行断点是一个方法，则进入当前方法体内，如果该方法体还有方法，则不会进入该内嵌的方法中 |
| F8                                                   | 在 Debug 模式下，进入下一步，如果当前行断点是一个方法，则不进入当前方法体内 |
| ***Alt + Shift + C***                                | 查看最近操作项目的变化情况列表                               |
| Ctrl+Alt+V<br>按住Alt,双击Enter<br>在后面输入" .var" | ***<font color = "red">自动生成左边变量</font>***，三种方式  |
| **后面输入 .arg**                                    | 包裹整体   例:Comparable.arg  ==>  (Comparable)              |
| **后面输入 .return**                                 | 返回参数  例:Comparable.return  ==>  return Comparable       |
| **Ctrl+Alt+T**                                       | 选中代码块,  带注释的自定义代码块                            |
| Ctrl+Shift+Del                                       | 删除包裹代码块外层,例: 需求，我想删除外面的循环语句，留下循环语句内的内容（光标需在for循环内） |
| 插件Ace Jump                                         | 快速移动光标                                                 |
| 去除拼接字符串的引号与换行符                         | 选中，右键，Show Content Actions ，Copy string concatenation text to the clipboard |
| ***Ctrl+Alt+O***                                     | 删除未使用的 import                                          |
| ctrl+shift+T                                         | 创建测试类                                                   |

## 浏览器快捷键

| 命令                 | 说明                 |
| -------------------- | -------------------- |
| Ctrl+F               | 此页面查询内容       |
| Ctrl+H/Ctrl+Shift+H  | 打开历史浏览记录窗口 |
| Ctrl+Shift+S         | 截图                 |
| **Ctrl + Shift + T** | 撤销关闭标签页       |
|                      |                      |

## 电脑快捷键

| 快捷键              | 备注               |
| ------------------- | ------------------ |
| **Ctrl+Shilft+Esc** | 打开任务管理器     |
| Alt+F4              | 关机界面           |
| win+E               | 打开我的电脑       |
| WIN+Shift+S         | 系统截图           |
| Ctrl+Alt+Del        | 系统界面           |
| WIN+I               | 打开设置           |
| 拖住窗口轻微晃动    | 最小化其他所有窗口 |
| WIN+X               | 多功能窗口         |
| Ctrl+Alt+F12        | 显卡控制面板       |
| WIN+tab             | 时间线窗口         |
| Alt+tab             | 切换窗口           |

## WIN+R

| WIN+R 运行界面与命令 | 说明                                         |
| -------------------- | -------------------------------------------- |
| control              | 打开控制面板                                 |
| powershell           | Stop-Process -Name explorer 重启explorer服务 |
| services.msc         | 打开服务面板                                 |
| regedit              | 注册表----一定一定不能轻易改!!!!!            |
| cmd                  | 打开cmd命令窗口                              |
| msconfig             | 系统配置,系统信息                            |

