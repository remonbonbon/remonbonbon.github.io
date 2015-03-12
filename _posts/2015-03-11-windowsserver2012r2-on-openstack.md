---
layout: post
title: Windows Server 2012 R2をOpenStackで使う
category: howto
tags : [OpenStack, Windows Server]
description: OpenStackでWindows Server 2012 R2のインスタンスを作成・利用する。
---
Windows Server 2012 R2イメージ
====================================
[cloudbase](http://www.cloudbase.it/ws2012r2/)から
Windows Server 2012 R2にKVM用イメージをダウンロード。
(windows_server_2012_r2_standard_eval_kvm_20140607.qcow2.gz)

gz圧縮を解凍してからOpenStack Dashboardでアップロードする。
(ファイルサイズが大きく、一部のアーカイバでは解凍できないため注意)

~~~
$ gnuzip windows_server_2012_r2_standard_eval_kvm_20140607.qcow2.gz
~~~

このイメージは180日間の評価用ライセンスとなっているため、使い続けるにはプロダクトキーが必要(後述)。

初期設定
====================================

ログイン
---------------------------
起動後、OpenStack Dashboardのインスタンスのコンソールを見ると、
Administratorのパスワードを変更するように言われている。

パスワードを短すぎたり単純すぎるとNGになる。(初期状態でパスワード複雑性の設定がONになっているため)

起動直後のストレージは、Cドライブの使用量 14.0GB

ネットワーク
---------------------------
DNSを変える。IPv6はオフにする。
これでインターネットに接続できるようになる。

ユーザ作成
---------------------------
まずパスワードポリシーを変えて制限を無くす。

1. **Control Panel > System and Security > Administrative Tools**から**Local Security Policy**を起動。
2. **Account Policies > Password Policy**を選択し、次の値を変更。
  1. **Maximum password age**を0にする。
  2. **Password must meet complexity requirements**をDisabledにする。

これでパスワードの制限がなくなる。
ここで、Administratorのパスワードを入力しやすいものに変えてしまう。

不要なアカウントAdminは削除。

個人ユーザを作成。管理者アカウントにする。
リモートデスクトップの接続可能ユーザに追加する。
これでリモートデスクトップで接続できるようになる。

言語設定
---------------------------
初期状態は英語になっているので日本語にする。

1. **Control Panel > Clock, Language, and Region > Language**から**Add a language**をクリック
2. 日本語を追加してリストの一番上に持ってくる。
3. 日本語の**options**をクリックし、"Windows display language"のチェックが終わるのを待つ。
4. 終わったら**Download and install language pack**をクリック。
5. インストールが終わったら、同じところから**Make this the primary language**をクリック。
6. 再ログオンすると日本語になっている。

システムロケールも英語になっていて一部のプログラムが文字化けするので、
**コントロールパネル > 時計、言語、および地域 > 値域 > 管理 > システムロケールの変更**
から変更する。

ping応答
---------------------------
初期状態ではWindows Firewallがpingを無視するようになっている。
これを応答するようにする。

1. **コントロール パネル > Windows ファイアウォール > 詳細設定 > 受信の規則**を開く。
2. **仮想マシンの監視 (エコー要求 - ICMPv4 受信)**を有効にする。

壁紙を変える
---------------------------
初期状態ではcloudbaseの壁紙に固定されていて、変更しようとしても出来ない。
これを変更できるようにする。

1. `gpedit.msc`を起動。
2. **ユーザーの構成 > 管理用テンプレート > デスクトップ > デスクトップ**
3. **デスクトップの壁紙**を未構成にする。  
(初期状態では、この値が有効になっておりcloudbaseの壁紙が指定されている)
4. `gpupdate`コマンドを実行する。


評価版から製品版にアップグレード
====================================
[Windows Server 2012 の評価バージョンとアップグレード オプション](https://technet.microsoft.com/ja-jp/library/jj574204.aspx)

Windows Server 2012 R2のプロダクトキーが必要。
standardエディションかdatacenterエディションにアップグレードできる。

現在のエディションの確認。評価版になっている。

~~~
> DISM /online /Get-CurrentEdition
Deployment Image Servicing and Management tool
Version: 6.3.9600.17031
Image Version: 6.3.9600.17031
Current edition is:
Current Edition : ServerStandardEval
The operation completed successfully.
~~~

> エディション名の簡略形式であるエディション ID をメモしておきます。

とあるがメモするコマンドはこれではなく、次のコマンド。

~~~
> DISM /online /Get-TargetEditions
Deployment Image Servicing and Management tool
Version: 6.3.9600.17031
Image Version: 6.3.9600.17031
Editions that can be upgraded to:
Target Edition : ServerStandard
Target Edition : ServerDatacenter
The operation completed successfully.
~~~

Target Editionのいずれか(ServerStandard or ServerDatacenter)を`/Set-Edition:`に指定する。
次のコマンドを実行するとサーバが2回再起動し、standardエディション製品版にアップグレードされる。

~~~
> DISM /online /Set-Edition:ServerStandard /ProductKey:XXXXX-XXXXX-XXXXX-XXXXX-XXXXX /AcceptEula
~~~

