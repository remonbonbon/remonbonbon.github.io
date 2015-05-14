---
layout: post
title: gitlabのインストール
category: howto
tags : [gitlab, git]
description: gitlabをインストールする方法。
---
### 手順
<https://about.gitlab.com/downloads/>にOSごとのインストール方法が載っているので、その通りに実施する。

### 実施環境
Ubuntu 14.04 (OpenStack VM)

Install and configure the necessary dependencies
------------------------------------------------------

~~~
$ sudo apt-get install openssh-server ca-certificates postfix
~~~

インストール時に出てくる「Postfix Configuration」ダイアログは`Internet Site`を選択する。


Download the Omnibus package and install everything
--------------------------------------------------------

~~~
$ wget https://downloads-packages.s3.amazonaws.com/ubuntu-14.04/gitlab-ce_7.10.1~omnibus.2-1_amd64.deb
$ sudo dpkg -i gitlab-ce_7.10.1~omnibus.2-1_amd64.deb

Selecting previously unselected package gitlab-ce.
(Reading database ... 77143 files and directories currently installed.)
Preparing to unpack gitlab-ce_7.10.1~omnibus.2-1_amd64.deb ...
Unpacking gitlab-ce (7.10.1~omnibus.2-1) ...
Setting up gitlab-ce (7.10.1~omnibus.2-1) ...
dpkg-query: package 'gitlab' is not installed
Use dpkg --info (= dpkg-deb --info) to examine archive files,
and dpkg --contents (= dpkg-deb --contents) to list their contents.
gitlab: Thank you for installing GitLab!
gitlab: Configure and start GitLab by running the following command:
gitlab:
gitlab: sudo gitlab-ctl reconfigure
gitlab:
gitlab: GitLab should be reachable at http://gitlab
gitlab: Otherwise configure GitLab for your system by editing /etc/gitlab/gitlab.rb file
gitlab: And running reconfigure again.
gitlab:
gitlab: For a comprehensive list of configuration options please see the Omnibus GitLab readme
gitlab: https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/README.md
gitlab:
It looks like GitLab has not been installed yet; skipping the upgrade script.
~~~

Configure and start GitLab
--------------------------------

~~~
$ sudo gitlab-ctl reconfigure
(省略)
Chef Client finished, 168/189 resources updated in 52.182203484 seconds
gitlab Reconfigured!
~~~

chefが走る。


Browse to the hostname and login
--------------------------------------
<http://127.0.0.1/>にアクセス。ログイン画面が表示される。

~~~
Username: root
Password: 5iveL!fe
~~~

新しくパスワードを設定する画面になる。
`Password is too short (minimum is 8 characters)`などと言われる。  
新しいパスワードを設定する。


外部URLを修正
----------------------
ログイン画面でユーザを作成できる。
必要な情報を入力して「Sign up」を押すと、確認メールが送られる。  
このメールのリンク`Confirm your account`のURLのホスト名がVMのホスト名になっていてアクセスできない。  
これを修正する。

<https://gitlab.com/gitlab-org/omnibus-gitlab/blob/629def0a7a26e7c2326566f0758d4a27857b52a3/README.md#configuring-the-external-url-for-gitlab>
のとおり、`/etc/gitlab/gitlab.rb`の`external_url`を編集すると、
確認メールが飛んでこなくなったため元に戻した。  

`gitlab.yml`と`gitlab-http.conf`を手動で修正する。

~~~
$ sudo vi /var/opt/gitlab/gitlab-rails/etc/gitlab.yml
       ## GitLab settings
       gitlab:
         ## Web server settings (note: host is the FQDN, do not include http://)
    -    host: gitlab
    +    host: 192.168.0.1

$ sudo vi /var/opt/gitlab/nginx/conf/gitlab-http.conf
     server {
       listen *:80;
    -  server_name gitlab;
    +  server_name 192.168.0.1;

$ sudo gitlab-ctl restart
~~~

メールのリンクがIPアドレスになった。
