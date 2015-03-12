---
layout: post
title: Windows7 64bitにおけるirbを速くする。
category: technics
tags : [Ruby, irb, Windows]
description: Windowsにおいてirbの反応が悪い現象を治す。
---
# 事象
Windowsにおいて、irbの入力に対するレスポンスが極めて遅い。
特にWindows7 64bitで起きる。

# 原因

## 原因候補1
[Windows7 64bitでirb.batが異常に重い時 - komorebikoboshiのブログ](http://komorebikoboshi.hatenablog.com/entry/2013/03/30/003518)

> どうもruby.exeが32bit版なのにbatファイルを実行するコマンドプロンプトが64bit版なのが問題らしい。

## 原因候補2
[irbの入力が遅いときはreadlineをオフにしてみては？ - Survive in Net.](http://d.hatena.ne.jp/sinjyama/20130803/1375518114)

> でも、irbの入力処理にはreadlineが使われているらしいこと、起動オプションでオン・オフを切り替えられることが分かった。
> で、起動オプションの"--noreadline"を使ってみたら、サクサクになったよ。

`irb --noreadline`として起動してみたところ、履歴機能（↑キー）が動作しなかったが、
何か文字を打ってから、↑キーを押すと何故か動作した。

# 解決方法
32bit版のcmd.exe「`C:\Windows\SysWOW64\cmd.exe`」を使って、irb.batを呼び出す。

Rubyのbinディレクトリ下の
`irb.bat`、`irb`をそれぞれ
`irb_original.bat`、`irb_original`にリネーム。

irb.batを新規作成し、以下を記述。

~~~
@echo off
C:\Windows\SysWOW64\cmd.exe /c irb_original.bat --noreadline %*
~~~

これで`irb`コマンドのまま高速化できる。

`irb.bat`だけでなく`irb`もリネームするのは、
`irb.bat`から同名のファイル(`irb`)が実行されるため。

## irbの終了方法
`--noreadline`を使うと、`Ctrl+D`で一発終了できなくなる。

`Ctrl+Z`→`Enter`で終了できる。
もちろん、`exit`でも終了できる。

