---
layout: post
title: Gradleのインストール
category: howto
tags : [Gradle, Java]
description: Gradleをインストールする方法。
---
Gradleセットアップ方法
============================
1. gvmのインストール
2. gvmでgradleをインストール

gvmのインストール
=========================
<http://gvmtool.net/>

~~~
$ curl -s get.gvmtool.net | bash
$ source ~/.bashrc

$ gvm version
Groovy enVironment Manager 2.4.1
~~~

gvmでgradleをインストール
=========================

最新バージョンを確認

~~~
$ gvm list gradle

================================================================================
Available Gradle Versions
================================================================================
     2.4                  1.11
     2.3                  1.10
     2.2.1                1.1
     2.2                  1.0
     2.1                  0.9.2
     2.0                  0.9.1
     1.9                  0.9
     1.8                  0.8
     1.7                  0.7
     1.6
     1.5
     1.4
     1.3
     1.2
     1.12

================================================================================
+ - local version
* - installed
> - currently in use
================================================================================
~~~

最新版をインストール

~~~
$ gvm install gradle 2.4

Downloading: gradle 2.4

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
  0   354    0     0    0     0      0      0 --:--:--  0:00:01 --:--:--     0
100 62.4M  100 62.4M    0     0  6589k      0  0:00:09  0:00:09 --:--:-- 9644k

Installing: gradle 2.4
Done installing!

Do you want gradle 2.4 to be set as default? (Y/n): Y

Setting gradle 2.4 as default.
~~~

確認

~~~
$ gradle --version

------------------------------------------------------------
Gradle 2.4
------------------------------------------------------------

Build time:   2015-05-05 08:09:24 UTC
Build number: none
Revision:     5c9c3bc20ca1c281ac7972643f1e2d190f2c943c

Groovy:       2.3.10
Ant:          Apache Ant(TM) version 1.9.4 compiled on April 29 2014
JVM:          1.7.0_79 (Oracle Corporation 24.79-b02)
OS:           Linux 3.13.0-52-generic amd64
~~~
