---
title: Google gn简明教程
tags:
  - gn
categories:
  - gn
date: 2021-07-04 13:30:10
---

https://gn.googlesource.com/gn/

generates build files for [Ninja](https://ninja-build.org/).

## 重要文件

.gn : Defines root of GN build tree

共享的变量放在gni文件中，通过 import导入

查有哪些targets

```
gn ls out/Default “//base/*”

```

## 查看可用参数列表及其默认值

`gn args --list out/my_build > args_list.log`，生成的log不在my_build目录，在运行命令的目录，应该是src下, 需要先运行gn args out/my_build

https://gn.googlesource.com/gn/+/main/docs/quick_start.md  备份https://elesos.github.io/2021/07/04/docs/gn/quick_start/

先下载示例代码 

`git clone https://gn.googlesource.com/gn`

我的在 https://github.com/elesos/gn

 要运行gn，需要先安装depot_tools

## 安装depot_tools

A collection of tools for dealing with Chromium development.

Chromium and Chromium OS use a package of scripts called [**depot_tools**](https://dev.chromium.org/developers/how-tos/depottools) to manage checkouts and code reviews.  The **depot_tools** package includes `gclient`, `gcl`, `git-cl`, `repo`, and others.

https://dev.chromium.org/developers/how-tos/install-depot-tools

https://commondatastorage.googleapis.com/chrome-infra-docs/flat/depot_tools/docs/html/depot_tools_tutorial.html

https://www.chromium.org/developers/how-tos/depottools

首先Confirm git and python are installed. 

#### LINUX / MAC

```
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
或 https://github.com/elesos/depot_tools使用里面的 depot_tools.tar.bz2
```

Add depot_tools to your PATH:

```
export PATH=/path/to/depot_tools:$PATH   //put depot_tools ahead of everything else放在最前面
```

#### windows

使用里面的压缩包depot_tools.zip  https://github.com/elesos/depot_tools

From a `cmd.exe` shell, run the command `gclient` (without arguments). On first run, gclient will install all the Windows-specific bits needed to work with the code, including msysgit and python.

## DEPS file

A DEPS file specifies dependencies of a project.



## 交叉编译 

target_os = "android" 

target_cpu = "arm" 

target_cpu = "x86" 

target_cpu = "x64"

## 入门

根目录下有.gn 这个文件所在目录为根目录 ，这个文件里面可能会指定A master build config file. 如Chrome’s is `//build/config/BUILDCONFIG.gn`

子模块目录新建BUILD.gn

根目录也有这个BUILD.gn(GN starts by loading this root
file)

创建一个静态库

tools/gn/tutorial/BUILD.gn

static_library("hello") {  sources = [    "hello.cc",  ] }

现在让我们添加一个依赖于这个库的可执行文件：

executable("say_hello") {  sources = [    "say_hello.cc",  ]  deps = [    ":hello",  ] }

也可以用 全部的//tools/gn/tutorial:hello  同一个文件中可以直接用:hello

 ninja -C out/Default say_hello



Create a `tools/gn/tutorial/BUILD.gn` file

```
executable("hello_world") {  sources = [    "hello_world.cc",  ] }

```

然后在根目录，BUILD.gn file

```
group("root") {  deps = [    ...    "//url",    "//tools/gn/tutorial:hello_world",  ] }


```

**“//” (代表 source root)**， 冒号后面是 target name

在根目录下

gn gen out/Default 

ninja -C out/Default hello_world

或

ninja -C out/Default tools/gn/tutorial:hello_world   不要前面的//符号



## 标签含义

```
//chrome/browser:version
→ Looks for “version” in chrome/browser/BUILD.gn

//base
→ Shorthand for //base:base

In current file当前文件里面的，只需要在前面加冒号
:baz
```



