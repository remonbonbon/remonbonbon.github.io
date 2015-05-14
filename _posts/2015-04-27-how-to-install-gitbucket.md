---
layout: post
title: gitbucketのインストール
category: howto
tags : [gitbucket, git]
description: gitbucketをインストールする方法。
---
### 手順

1. openjdkのインストール
2. gitbucketのインストール
3. gitbucket起動スクリプトの作成

<http://qiita.com/tanacasino/items/4d683debd9bcb2f6e06e>

### 実施環境
Ubuntu 14.04 (OpenStack VM)


openjdkのインストール
=================================

~~~
$ sudo aptitude install openjdk-7-jre
$ java -version
java version "1.7.0_79"
OpenJDK Runtime Environment (IcedTea 2.5.5) (7u79-2.5.5-0ubuntu0.14.04.2)
OpenJDK 64-Bit Server VM (build 24.79-b02, mixed mode)
~~~

gitbucketの動作確認 (optional)
=================================
最新版をダウンロード

~~~
$ cd ~
$ wget https://github.com/takezoe/gitbucket/releases/download/3.2/gitbucket.war
$ java -jar gitbucket.war
~~~

http://127.0.0.1:8080/
にアクセスして動いていることを確認。
初期状態ではユーザ名root/パスワードrootでログインできる。
確認後、Ctrl + Cで止める
ホームディレクトリに出来たgitbucketフォルダも(確認用なので)不要なので削除


gitbucketのインストール
=================================
公式で用意されているスクリプトを使う。
ちなみに、`install`がwarファイルをダウンロードしてくるので、warファイルは不要。

Ubuntu

~~~
$ wget https://raw.githubusercontent.com/takezoe/gitbucket/master/contrib/gitbucket.init
$ wget https://raw.githubusercontent.com/takezoe/gitbucket/master/contrib/gitbucket.conf
$ wget https://raw.githubusercontent.com/takezoe/gitbucket/master/contrib/install
$ chmod +x install
~~~

Red Hat系。こっちはgitbucketユーザで実行する模様。

~~~
$ wget https://raw.githubusercontent.com/takezoe/gitbucket/master/contrib/linux/redhat/gitbucket.init
~~~

`gitbucket.conf`を編集する。
バージョンが古いので最新版にする。

~~~
# GITBUCKET_VERSION=2.1
GITBUCKET_VERSION=3.2
~~~

インストール

~~~
$ sudo ./install
Opening port 8080 in firewall.
Please use iptables-persistent:
  sudo apt-get install iptables-persistent
After installed, you can save/reload iptables rules anytime:
  sudo /etc/init.d/iptables-persistent save
  sudo /etc/init.d/iptables-persistent reload
Making /var/lib/gitbucket directory.
Fetching GitBucket v3.2 and saving as /usr/share/gitbucket/lib/gitbucket.war
Copying gitbucket.conf to /usr/share/gitbucket
update-rc.d: warning: /etc/init.d/gitbucket missing LSB information
update-rc.d: see <http://wiki.debian.org/LSBInitScripts>
 Adding system startup for /etc/init.d/gitbucket ...
   /etc/rc0.d/K02gitbucket -> ../init.d/gitbucket
   /etc/rc1.d/K02gitbucket -> ../init.d/gitbucket
   /etc/rc6.d/K02gitbucket -> ../init.d/gitbucket
   /etc/rc2.d/S98gitbucket -> ../init.d/gitbucket
   /etc/rc3.d/S98gitbucket -> ../init.d/gitbucket
   /etc/rc4.d/S98gitbucket -> ../init.d/gitbucket
   /etc/rc5.d/S98gitbucket -> ../init.d/gitbucket
Starting GitBucket service
Starting GitBucket server:  Success
~~~

<http://127.0.0.1:8080/>にアクセス。動いている。

再起動後、再度確認。
なお、rootで実行される。


gitbucketの初期設定
=================================
管理者アカウントは、root/root
必要に応じてパスワードを変える。

SSH有効化
-----------------------
初期状態だと(push等が)HTTPのみなので、sshを有効にする。

1. rootでログインして、画面左上のスパナアイコンをクリック。
2. System Settingsをクリックし、「Enable SSH access to git repository」にチェックを入れる。
3. 「Base URL」に外部からアクセスできるgitbucketのURLを入力
4. Apply changesをクリック。
5. なぜか応答が返ってこないが、再度gitbucketにアクセスすると設定が反映されている。

ユーザ作成自由化 (optional)
-----------------------
誰でもユーザを作成できるようにする。

1. rootでログインして、画面左上のスパナアイコンをクリック。
2. System Settingsをクリック。
3. 「Account registration」をAllowにする。
4. Apply changesをクリック。
5. Sign in画面に「Create new account」ボタンが出現する。

設定ファイルベース
------------------------
上記の設定は設定ファイルからも行える。

~~~
$ vi /var/lib/gitbucket/gitbucket.conf
~~~


SSHで既存リポジトリのpush
=================================

1. gitbucketにユーザを作成する
2. ユーザホーム画面の「Edit Your Profile」をクリック。
3. SSH Keysをクリック。
4. Keyにpublicキーをコピペ
5. `~/.ssh/config`を適当に設定。(ここではHostをgitbucketと設定)
6. `git remote add origin ssh://gitbucket/user/test.git`
7. `git push origin master`
