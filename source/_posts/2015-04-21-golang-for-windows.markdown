---
layout: post
title: "Golang 编译windows应用程序"
date: 2015-04-21 22:54:38 +0800
comments: true
author: Sleepy Programmer
categories: 
---

因为我们更喜欢在Linux上开发程序，
所以生成交叉编译器，以便在Linux上交叉编译出windows程序。

{% highlight console %}
# 安装minGW：在Linux上运行gcc交叉编译生成windows程序
# 我们用到Cgo，因此需要安装 C 语言交叉编译器
sudo apt-get install gcc-mingw-w64

# 下载Go语言的源代码
git clone https://github.com/golang/go.git

# 32-bit go-compiler for windows
GOOS=windows GOARCH=386 CGO_ENABLED=1 CXX_FOR_TARGET=i686-w64-mingw32-g++ CC_FOR_TARGET=i686-w64-mingw32-gcc ./make.bash

# 64-bit 编译器
GOOS=windows GOARCH=amd64 CGO_ENABLED=1 CXX_FOR_TARGET=x86_64-w64-mingw32-g++ CC_FOR_TARGET=x86_64-w64-mingw32-gcc ./make.bash 

# 非交叉编译，生成Linux程序
GOOS=linux GOARCH=amd64 CGO_ENABLED=1 CXX_FOR_TARGET=g++ CC_FOR_TARGET=gcc ./make.bash 

# 生成Go编译器之后，以下命令执行交叉编译，生成windows程序
GOOS=windows GOARCH=386 go build
{% endhighlight %}

Cgo 
===========
{% img right /assets/2015/ffmpeg-win.png 627%}
最终的目标是要生成windows程序，所以Cgo引用的库文件也必须是windows版本的。
以ffmpeg库为例：参考截图，在ffmpeg文件夹之下，新建一个文件夹，名为libwin，
用于保存windows版本的库文件（dll文件）。

pkgconfig 也需要做相应调整。而头文件无需变动。

准备工作只有这么多了。通过指定pkgconfig的路径，就能够交叉编译Cgo程序了。
```GOOS=windows GOARCH=386 PKG_CONFIG_PATH=/foo/bar go build```


