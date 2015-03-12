---
layout: post
title: Picoのexcerptを文字単位にする
category: technics
tags : [pico, PHP]
description: mb_substrを使って抜粋文を文字単位で切り出せるようにする。
---
# mb_substrで切り出す
[Pico](http://picocms.org/)の`config.php`にある`$config['excerpt_length']`は
単語単位のため、日本語の記事だと使いづらい。

そこで、Picoを改造して文字単位に変える。

Picoインストールディレクトリ下の`lib/pico.php`を開き、
250行目あたりを以下のように変える。

~~~
'excerpt' => mb_substr(strip_tags($page_content), 0, $excerpt_length)
~~~

# mb_substrが有効でない？
mbstringが有効になっていないと、ログに以下のように出る。

~~~
PHP Fatal error:  Call to undefined function mb_substr()
~~~

## mbstringを有効にする
PHP 5.5.19, Fedora 20の場合

1. php.iniでmbstringを有効にする。
2. php-mbstringをインストールする。
3. apacheを再起動

~~~
$ sudo vi /etc/php.ini

zend.multibyte = On
zend.script_encoding = UTF-8

mbstring.language = Japanese
mbstring.internal_encoding = UTF-8

$ sudo yum install php-mbstring
$ sudo systemctl restart httpd
~~~

`mbstring.internal_encoding`はデフォルトのエンコーディングで、
[`mb_substr`の第4引数](http://php.net/manual/ja/function.mb-substr.php)を省略した場合に使用される。