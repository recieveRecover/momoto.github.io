---
layout: post
title: "libvirtを用いて仮想マシンを構築、インストールする"
date: 2012-08-04 09:49
comments: true
categories: [仮想化]
keywords: libvirt, python-libvirt, virt-install
description: "virt-installを使って、CUIから仮想マシンのディスクイメージを作成する手順の記録です。"

---

virt-installを使って、CUIから仮想マシンのディスクイメージを作成する。

動作要件として、ホストOSにハイパーバイザ（KVMやXen）とlibvirtがインストールされていて、libvirtdが起動している必要がある。
virt-install自体は、RPMでは"python-virtinst"パッケージに含まれている。
（参考：[RPM resource virt-install](http://rpmfind.net/linux/rpm2html/search.php?query=virt-install)）

<!-- more -->

virt-installのオプションには仮想マシンに割り当てるリソースなどを指定する。
例えば、仮想マシン名は「myvirtualmachine」、インストールイメージにはkddilabs.jpのミラーが配布しているCentOS6.3[x86_64]を
指定した準仮想化ゲストOSを/var/lib/libvirt/images/myvirtualmachine.imgに作成する場合は下のようなオプションになる。

```
# virt-install \
--name=myvirtualmachine \
--ram=512 \
--vcpus=1 \
--paravirt \
--location='ftp://ftp.kddilabs.jp/Linux/packages/CentOS/6.3/os/x86_64/' \
--file=/var/lib/libvirt/images/myvirtualmachine.img \
--file-size=8 \
--nographics \
--keymap=jp106
```

また、`--prompt`オプションを使うと対話的に各項目を指定することができる。

```
# virt-install --prompt

  Would you like a fully virtualized guest (yes or no)? This will allow you to run unmodified operating systems.
  // 完全仮想化を行うかどうか（noで準仮想化）
    no

  What is the name of your virtual machine?
  // 仮想マシンの名称
    myvirtualmachine

  How much RAM should be allocated (in megabytes)?
  // 仮想マシンに割り当てるメインメモリの容量
    512

  What would you like to use as the disk (file path)?
  // イメージディスクのファイルパス
    /var/lib/libvirt/images/myvirtualmachine.img

  How large would you like the disk (/var/lib/xen/images/centos-6.3-x86_64.img) to be (in gigabytes)?
  // イメージディスクに割り当てる容量
    8

  Would you like to enable graphics support? (yes or no)
  // グラフィクスサポートを有効にするかどうか
    no

  What is the install CD-ROM/ISO or URL?
  // ISOイメージのファイルパス、または配布元のURL
    ftp://ftp.kddilabs.jp/Linux/packages/CentOS/6.3/os/x86_64/
```

`--nographics`オプションを指定していれば、テキストユーザインタフェースでゲストOSのインストールが始まる。
ゲストOSとのコンソール接続を終了する場合は、'Ctrl + ]'でホストOSへ戻ることができる。

#### 参考
- [libvirt 仮想化ライブラリーの徹底調査](http://www.ibm.com/developerworks/jp/linux/library/l-libvirt/)
- [第6章 仮想化ゲストインストールの概要](http://docs.redhat.com/docs/ja-JP/Red_Hat_Enterprise_Linux/5/html/Virtualization/chap-Virtualization-Virtualized_guest_installation_overview.html)
- [libvirt探訪（基礎編） － ＠IT](http://www.atmarkit.co.jp/flinux/rensai/linuxkvm03/03a.html)

