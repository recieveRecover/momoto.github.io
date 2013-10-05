---
layout: post
title: "virshをつかって仮想マシンを管理する"
date: 2013-08-17 15:51
comments: true
categories: [仮想化]
keywords: 仮想化, libvirt, virsh
description: "libvirtのコマンドラインツールであるvirshをつかって、仮想マシンを管理する手引きです。"
breadcrumb:
  - title: Blog
    url: /
  - title: 仮想化
    url: /blog/categories/jia-xiang-hua/

---

　libvirtのコマンドラインツールであるvirshをつかって、仮想マシンを管理する手引きです。virt-installなどで作成した仮想マシンの起動、停止、接続などの操作を行います。<!-- more -->

### 仮想マシンの一覧

　virt-manager（virt-installを含む）で作成した仮想マシンや、XMLファイルから定義した仮想マシンの一覧を表示します。
オプションを何も付けない場合、シャットオフ状態の仮想マシンは一覧に表示されないので、状態に依らずすべての一覧を得るには`--all`のオプションを付けます。
他のユーザが作成した仮想マシンも一覧には表示されません。

    $ virsh list --all
     Id    名前                         状態
    ----------------------------------------------------
     -     ubuntu-13.04                   シャットオフ
     6     centos-6.4                     実行中

### 仮想マシンの起動

　定義済みの仮想マシン（libvirtに認識されていてlistに現れる仮想マシン）を起動する場合、`virsh start <仮想マシンの名前>`のコマンドを使います。

    $ virsh start ubuntu-13.04
    ドメイン ubuntu-13.04 が起動されました

　未定義の仮想マシンをXMLファイルから定義して、起動する場合、`virsh create <XMLファイル>`のコマンドを使います。

    $ virsh create centos-6.4.backup.xml
    ドメイン centos-6.4 が centos-6.4.backup.xml から作成されました

### 仮想マシンのコンソールへ接続する

　実行中の仮想マシンのシリアルポートへコンソール接続するには`virsh console <仮想マシンのIDまたは名前>`をつかいます。

    $ virsh console 6
    ドメイン centos-6.4 に接続しました
    エスケープ文字は ^] です

    CentOS release 6.4 (Final)
    Kernel 2.6.32-358.el6.x86_64 on an x86_64

    localhost.localdomain login: 

　コンソールへ接続するには、仮想マシンのXMLファイルにシリアルデバイスが定義されていること（ホスト側）と、シリアルポートによる接続が許可されていること（ゲスト側）が必要です。

- 参考
  - [lost and found ( for me ? ): KVM: virsh コンソール接続&#12288;( guest OS CentOS )](http://lost-and-found-narihiro.blogspot.jp/2010/10/kvm-virsh-guest-os-centos.html)
  - [lost and found ( for me ? ): KVM: virsh コンソール接続&#12288;( guest OS Ubuntu )](http://lost-and-found-narihiro.blogspot.jp/2010/10/kvm-virsh-guest-os-ubuntu_18.html)

### 一時停止と再開

    $ virsh suspend 5
    ドメイン 5 は一時停止されました

    $ virsh resume 5
    ドメイン 5 が再開されました

### 停止する

　ホスト側から仮想マシンを停止させるには`virsh shutdown`または`virsh destroy`をつかいます。

    $ virsh shutdown 6
    ドメイン 6 はシャットダウン中です

    $ virsh destroy 6
    ドメイン 6 は強制停止されました

### 定義と定義の解除

　仮想マシンの定義を消すには`virsh undefine <仮想マシンの名前>`をつかいます。

    $ virsh undefine ubuntu-13.04
    ドメイン ubuntu-13.04 の定義が削除されました

　XMLファイルから仮想マシンを定義するには`virsh define <XMLファイル>`をつかいます。

    $ virsh define ubuntu-13.04.backup.xml
    ドメイン ubuntu-13.04 が ubuntu-13.04.backup.xml から定義されました

　定義済みの仮想マシンの情報をXML形式で出力するには`virsh dumpxml <仮想マシンのIDまたは名前>`をつかいます。

    $ virsh dumpxml ubuntu-13.04 > ubuntu-13.04.backup.xml

### 参考

- [Virsh Command Reference](http://libvirt.org/sources/virshcmdref/html-single/)
- [&#31532;14&#31456; virsh &#12434;&#20351;&#12387;&#12390;&#12466;&#12473;&#12488;&#12434;&#31649;&#29702;&#12377;&#12427; - Red Hat Customer Portal](https://access.redhat.com/site/documentation/ja-JP/Red_Hat_Enterprise_Linux/6/html/Virtualization_Administration_Guide/chap-Virtualization_Administration_Guide-Managing_guests_with_virsh.html)
- [第28章 Managing guests with virsh](http://docs.fedoraproject.org/ja-JP/Fedora/13/html/Virtualization_Guide/chap-Virtualization-Managing_guests_with_virsh.html)
- [openSUSE 12.3: パート II. libvirt を利用した仮想マシンの管理](http://opensuse-man-ja.berlios.de/opensuse-html/bk05pt02.html)
- [KVM/Managing - Community Ubuntu Documentation](https://help.ubuntu.com/community/KVM/Managing)
- [libvirt - ArchWiki](https://wiki.archlinux.org/index.php/Libvirt)
