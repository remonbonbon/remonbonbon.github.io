---
layout: post
title: Linuxの設定
category: memo
tags : [Linux, 設定]
description: vim, iptablesなどの設定値
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

set list
set listchars=tab:>_,trail:_,extends:>,precedes:<,nbsp:%
~~~

## iptables
~~~
*filter
:INPUT    DROP [0:0]
:FORWARD  ACCEPT [0:0]
:OUTPUT   ACCEPT [0:0]

-A INPUT -i lo -j ACCEPT
-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

## HTTP
-A INPUT -p tcp --dport 80 -j ACCEPT

## ping (local only)
-A INPUT -s 192.168.11.0/24 -p icmp --icmp-type 0 -j ACCEPT
-A INPUT -s 192.168.11.0/24 -p icmp --icmp-type 8 -j ACCEPT

## ssh (local only)
-A INPUT -s 192.168.11.0/24 -p tcp --dport 22 -j ACCEPT

## samba (local only)
-A INPUT -m state --state NEW -m udp -p udp -s 192.168.11.0/24 --dport 137 -j ACCEPT
-A INPUT -m state --state NEW -m udp -p udp -s 192.168.11.0/24 --dport 138 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp -s 192.168.11.0/24 --dport 139 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp -s 192.168.11.0/24 --dport 445 -j ACCEPT

COMMIT
~~~

## NTP
~~~
server ntp1.jst.mfeed.ad.jp
server ntp2.jst.mfeed.ad.jp
server ntp3.jst.mfeed.ad.jp
server ntp.nict.jp
~~~

## samba
~~~
[global]
workgroup = WORKGROUP
dos charset = CP932
unix charset = UTF-8

wide links = yes
unix extensions  = no
map archive = no
load printers = no
disable spoolss = yes

[xxxx]
path = /home/xxxx
writeable = yes
read only = no
create mask = 0644
directory mask = 0755
force group = xxxx
force user = xxxx
~~~
