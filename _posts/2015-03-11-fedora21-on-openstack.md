---
layout: post
title: fedora21 cloudをOpenStackで使う
category: howto
tags : [OpenStack, fedora]
description: OpenStackでfedora21 cloudのインスタンスを作成・利用する。
---
イメージの作成
=============================
[公式サイトのクラウドページ](https://getfedora.org/en/cloud/download/)に行き、
**General Purpose**の**Are you and OpenStack user?**をクリックし、DownloadボタンのURLをコピーする。
(クライアントPCにダウンロードする必要は無い)

OpenStack Dashboardからイメージを作成するときに、コピーしたURLを指定する。
イメージ形式にqcow2を指定する。

~~~
http://download.fedoraproject.org/pub/fedora/linux/releases/21/Cloud/Images/x86_64/Fedora-Cloud-Base-20141203-21.x86_64.qcow2
~~~

インスタンスの作成
=============================

## パスワードでログインする場合
OpenStack Dashboardの「作成後」タブに以下を入力。デフォルトユーザのパスワードを変える。
(デフォルトユーザにはパスワードが設定されていないため、そのままだとログインできない？)  
[Fedora 21 - Cloud | Richard Bucker](http://www.richardbucker.com/2015/02/fedora-21-cloud.html)

ユーザ名fedora, パスワードfedoraになる。

~~~
#cloud-config
password: fedora
chpasswd: { expire: False }
ssh_pwauth: True
~~~

## 公開鍵暗号を使う場合
1. OpenStack Dashboardでキーペアを作成し、秘密鍵をダウンロードしておく。
2. インスタンス作成時に、作成したキーペアを指定する。
3. SSHログイン時に、ダウンロードした秘密鍵を使う。


DNSの変更
=============================
~~~
$ sudo vi /etc/resolv.conf
nameserver xxx.xxx.xxx.xxx
~~~
fedoraはhostsに自分自身がなくてもsudo時にメッセージが出ないのでhostsはそのまま。

ユーザの作成
=============================
wheelグループに追加することで、sudoできるようになる。

~~~
$ sudo adduser -G wheel xxxx
$ sudo passwd xxxx
~~~

visudo
--------------------------
初期設定だとパスワードを聞かれるので解除

~~~
$ sudo visudo
## Allows people in group wheel to run all commands
# %wheel  ALL=(ALL)       ALL

## Same thing without a password
%wheel  ALL=(ALL)       NOPASSWD: ALL
~~~

タイムゾーンの設定
--------------------------
初期設定ではUTCになっているので、JSTにする。
`/etc/localtime`が`/usr/share/zoneinfo/Etc/UTC`へのシンボリックリンクになっているので
これを置き換える。

~~~
$ date
Wed Mar 10 18:12:27 UTC 2015
$ sudo rm /etc/localtime
$ sudo ln -s /usr/share/zoneinfo/Japan /etc/localtime
$ date
Wed Mar 11 03:16:04 JST 2015
~~~

bash-completionのインストール (optional)
--------------------------
~~~
$ sudo yum install bash-completion
~~~

インストール後、一旦ログアウトする。

