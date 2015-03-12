---
layout: post
title: rbenvとruby-buildを使ってRubyをインストール
category: howto
tags : [Ruby, 環境, Linux]
description: rbenvとruby-buildによるLinux向けRuby環境の構築
---
下準備
======================

## 必要なパッケージをインストール
opensslとreadlineをインストールしていないとrubyのビルド時にエラーになる。

~~~
$ sudo yum -y install make git gcc-c++ openssl-devel readline-devel
~~~

### Ruby 2.2.0のビルド
[Ruby2.2.0のインストールがlibffi.a: could not read symbols: Bad valueで失敗した件 - まっしろけっけ](http://shiro-16.hatenablog.com/entry/2014/12/26/003810)によると、
libffi-develを入れる。

~~~
$ sudo yum -y install make git gcc-c++ openssl-devel readline-devel libffi-devel
~~~


rbenvのインストール
======================

## rbenvとruby-buildをclone

~~~
$ git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
$ git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
~~~

## 環境設定
~~~
# PATH に追加
$ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile

# .bash_profile に追加
$ echo 'eval "$(rbenv init -)"' >> ~/.bash_profile

# 上記設定の再読み込み
$ source ~/.bash_profile
~~~

## 確認
~~~
$ rbenv --version
rbenv 0.4.0-129-g7e0e85b
~~~

# rbenv-gem-rehashをインストール
そのままだとgemでbundlerをインストールしたりしたときに、`bundle`コマンド等が使えない。
`rbenv rehash`するとシンボリックリンクが更新されて使えるようになるのだが、
面倒なので自動的にやってくれるプラグインをインストールする。

~~~
$ git clone https://github.com/sstephenson/rbenv-gem-rehash.git ~/.rbenv/plugins/rbenv-gem-rehash
~~~


Rubyのインストール
======================

## rubyの最新版を確認
install -- list でインストール可能なバージョン一覧が出力される

~~~
$ rbenv install --list
Available versions:
  2.1.0-rc1
  2.1.0
  2.1.1
  2.1.2
  2.1.3
  2.1.4
  2.1.5
  2.2.0-dev
  2.2.0-preview1
  2.2.0-preview2
  :
~~~

## rubyをビルド
先ほど調べたバージョンを指定する。rubyがビルドされる。

~~~
$ rbenv install -v 2.1.5
~~~

再読み込み

~~~
$ rbenv rehash
~~~

インストールされているruby一覧を確認

~~~
$ rbenv versions
  2.1.5
~~~

先ほどインストールした最新版に設定

~~~
$ rbenv global 2.1.5
~~~

## 確認
~~~
$ ruby -v
ruby 2.1.5p273 (2014-11-13 revision 48405) [x86_64-linux]
~~~


参考サイト
======================
- [rbenv を使って ruby をインストールする(CentOS編)](http://qiita.com/inouet/items/478f4228dbbcd442bfe8)
- [さくらVPS（CentOS 6）にrbenvでRubyをインストール](http://qiita.com/masa2sei/items/c07083e1726c424e3666)
- [rbenv を使っているなら rbenv-gem-rehash を使おう](http://qiita.com/riocampos/items/f0fe7217972b312c4f3a)