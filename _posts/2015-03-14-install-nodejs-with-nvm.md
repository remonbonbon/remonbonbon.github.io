---
layout: post
title: nvmを使ってnode.jsをインストール
category: howto
tags : [node.js, nvm]
description: node.jsバージョン管理ツールnvmを使ってnode.jsをインストールする。
---
# nvmとは？
インストール・使用するnode.jsのバージョンを管理し、簡単に切り替えられるようにするツール。

# 必要なもの
- git

# インストール手順

## nvmのセットアップ
~~~
$ git clone git://github.com/creationix/nvm.git ~/.nvm
$ source ~/.nvm/nvm.sh
$ nvm --version
0.24.0

$ vi ~/.bashrc
if [[ -s ~/.nvm/nvm.sh ]];
 then source ~/.nvm/nvm.sh
fi
~~~

## node.jsのインストール
node.jsのバージョンを変えるときも同様。

~~~
$ nvm ls-remote
:
       v0.11.15
       v0.11.16
        v0.12.0
    iojs-v1.0.0
:

$ nvm install 0.11.16
Now using node v0.11.16

$ node -v
v0.11.16

$ nvm alias default v0.11.16
default -> v0.11.16
~~~

# npmのグローバルインストール先
`npm install -g`でグローバルインストールすると、
`~/.nvm/v*/lib/node_modules/`にインストールされる。

バイナリは、
`~/.nvm/v*/bin/`にシンボリックリンクができ、PATHが通る。
