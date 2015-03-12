---
layout: post
title: picoの使い方
category: howto
tags : [pico, PHP, CMS]
description: 軽量CMS "pico"をインストールし使ってみる。
---
# pico
[http://picocms.org/](http://picocms.org/)

Markdownで記述する軽量CMS

# ターゲットOS

Fedora20

 
# apacheとphpをインストール

~~~

$sudo yum install httpd php

$sudo systemctl start httpd

$sudo systemctl enable httpd

~~~

 
index.phpを開いてくれるように設定

~~~

$sudo /etc/httpd/conf/httpd.conf

<IfModule dir_module>

  DirectoryIndex index.html index.php

</IfModule>

~~~

 
設定を変更したらapacheを再起動

~~~

$sudo systemctl restart httpd

~~~

 
apacheのユーザ・グループをapacheから変えている場合は、

PHPセッションのセッションの保存先ディレクトリを書き込めるようにする

~~~

$sudo chmod 0777 /var/lib/php/session/

~~~

 
# picoのインストール

ダウンロードし解凍して、公開しているディレクトリに配置するだけ。

~~~

$wget https://github.com/picocms/Pico/archive/master.zip

$unzip master.zip

$mv Pico-master ~/public_html

~~~

 
# picoの設定

解凍したものの中に入っている`config.php`を編集する。

 
~~~

$vi config.php

 
$config['site_title'] = 'Title';

$config['base_url'] = 'http://www.example.com';

$config['theme'] = 'default';

$config['date_format'] = 'Y-m-d';

$config['pages_order_by'] = 'date';

$config['pages_order'] = 'desc';

~~~

 
- `site_title`はサイトのタイトル。

- `base_url`はpicoを置いた場所のURL。最後に/を付けてはいけない。相対パスでもOK。

- `theme`はテーマ名。themes/ディレクトリの中に入っているテーマディレクトリ名を指定する。

- `date_format`は日付のフォーマット。PHPの[フォーマット](http://php.net/manual/ja/function.date.php)を参照。

- `pages_order_by`は`alpha`でアルファベット順、`date`で日付順になる。

- `pages_order`は昇順か降順を選ぶ。`date`かつ`desc`で新しい記事が上に来る。

 
# Pico Editor Pluginを使う

`content`ディレクトリにMarkdownファイルを入れると記事が投稿できるが、

面倒くさいので、[Pico Editor Plugin](https://github.com/gilbitron/Pico-Editor-Plugin)を使う。

 
ダウンロードして`plugins/pico_editor`ディレクトリに入れる。

~~~

$wget https://github.com/gilbitron/Pico-Editor-Plugin/archive/master.zip

$unzip master.zip

$mv Pico-Editor-Plugin-master plugins/pico_editor

~~~

 
パスワードを[sha1](http://www.sha1-online.com/)でハッシュ化して、設定ファイルに書き込む。

~~~

$vi plugins/pico_editor/pico_editor_config.php

 
$pico_editor_password = 'ここにSHA1ハッシュを入れる';

~~~

 
`/admin`にアクセスしてログイン。

「+」ボタンから新規記事を作成できる。

 
以下のフォーマットが無いファイルは編集できない。

初めからある`index.md`とかは編集できない模様。

~~~

/ *

Title:  
Author:  
Date:  
*/

~~~

 
# テーマの変更

[テーマ](https://github.com/picocms/Pico/wiki/Pico-Themes)とかからダウンロードして、

`themes`に入れて、`config.php`の`$config['theme']`を変更する。

 
ただのHTML/CSS/JavaScriptなので自分で作るのも簡単。

