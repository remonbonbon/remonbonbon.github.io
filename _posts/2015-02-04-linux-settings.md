---
layout: post
title: Linuxの設定
category: memo
tags : [Linux, 設定]
description: vim, iptablesの基本設定値
---
## .vimrc
~~~
set incsearch
set hlsearch

set number
set title

set cmdheight=2
set laststatus=2
set showcmd
set display=lastline
set tabstop=2

colorscheme desert
syntax on
~~~

## iptables
~~~
*filter
# chain definitions
:INPUT    DROP [0:0]
:FORWARD  ACCEPT [0:0]
:OUTPUT   ACCEPT [0:0]

# filter rules
## loopback
-A INPUT -i lo -j ACCEPT

## state
-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

## http
-A INPUT -p tcp --dport 80 -j ACCEPT

## ping (local only)
-A INPUT -s 192.168.11.0/24 -p icmp --icmp-type 0 -j ACCEPT
-A INPUT -s 192.168.11.0/24 -p icmp --icmp-type 8 -j ACCEPT

## ssh (local only)
-A INPUT -s 192.168.11.0/24 -p tcp --dport 22 -j ACCEPT

## samba (local only)
-A INPUT -m state --state NEW -m tcp -p tcp -s 192.168.11.0/24 --dport 139 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp -s 192.168.11.0/24 --dport 445 -j ACCEPT

COMMIT
~~~

