---
layout: post
title: "Arch Linux 2012.12.01をインストールする"
date: 2013-01-05 00:00
comments: true
categories: [GNU/Linux]
keywords: GNU/Linux, Arch Linux, Pacman
description: "Arch Linux 2012.12.01をインストールした手順の記録です。インストールメディアはarchlinux-2012.12.01-dual.isoを使用しています。"
breadcrumb:
  - title: Blog
    url: /
  - title: GNU/Linux
    url: /blog/categories/gnu-slash-linux/

---

　Arch Linux 2012.12.01をインストールした手順の記録です。インストールメディアはarchlinux-2012.12.01-dual.isoを使用しています。
[Installation Guide (日本語) - ArchWiki](https://wiki.archlinux.org/index.php/Installation_Guide_%28%E6%97%A5%E6%9C%AC%E8%AA%9E%29)の内容と重なる部分が多いですが、自分なりに注釈を加えて記録します。

　インストールにはインターネットに接続できる環境が必要です。
また、パーティションの構成やインストールするブートローダをあらかじめ決めておく必要があります。
次の手順ではパーティションテーブルにGPTを使用し、ブートローダにはGRUB2をインストールしています。<!-- more -->

### 1. キーボード配列の指定

使用しているキーボード配列を指定する。ここで行う設定は一時的なものであるため、インストール後には改めて設定する必要がある。

    root@archiso ~ # loadkeys jp106

### 2. ストレージデバイスの設定

接続しているストレージデバイスのパーティショニング、フォーマット、マウントを行う。

パーティショニングはインストールメディアに含まれているパーティショニングツールを使用して行う。その際、パーティションテーブルの規格（MBR、GPT）によって使用するツールは異なる。

    root@archiso ~ # gdisk /dev/sdx

フォーマットはmkfsを使用して行う。ここでフォーマット対象のパーティションとファイルシステムを指定する。

    root@archiso ~ # mkfs.ext4 /dev/sdx1

マウントは/mntにrootパーティションが対応するようにして行う。bootパーティションであれば/mnt/bootへマウントする。

    root@archiso ~ # mount /dev/sdx1 /mnt

### 3. システムのインストール

　Arch Linuxのシステムをインストールする。インストール後に行える設定（ホスト名やタイムゾーンの設定）については省略する。

　pacstrapを使用して、[base](https://www.archlinux.org/groups/i686/base/)と[base-devel](https://www.archlinux.org/groups/i686/base-devel/)のパッケージをインストールする。
その際、国内のミラーサーバーからパッケージをダウンロードするようにmirrorlistを編集する。
日本のミラーサーバーには[tsukuba.wide.ad.jp](http://ftp.tsukuba.wide.ad.jp/Linux/archlinux/)と[jaist.ac.jp](http://ftp.jaist.ac.jp/pub/Linux/ArchLinux/)がある。

    root@archiso ~ # vi /etc/pacman.d/mirrorlist
    root@archiso ~ # pacstrap /mnt base base-devel

genfstabを使用して、fstabを生成する。

    root@archiso ~ # genfstab -U -p /mnt >> /mnt/etc/fstab

arch-chrootを使用して、新しいシステムの環境へ移動する。移動後には再度、使用しているキーボード配列を指定する。

    root@archiso ~ # arch-chroot /mnt

ブートローダのインストールと設定を行う。ここでの手順はブートローダによって異なる。

#### GRUB2

    sh-4.2# pacman -S grub2-bios
    sh-4.2# grub-install /dev/sda
    sh-4.2# grub-mkconfig -o /boot/grub/grub.cfg

#### Syslinux

    sh-4.2# pacman -S syslinux gptfdisk
    sh-4.2# vi /boot/syslinux/syslinux.cfg
    sh-4.2# syslinux-install_update -i -a -m

### 4. アンマウントと再起動

chroot環境を終了させて、上の手順でマウントしたパーティションをアンマウントする。その後、システムを再起動してインストールが完了する。

    sh-4.2# exit
    root@archiso ~ # umount /mnt
    root@archiso ~ # reboot

### システムの設定

再起動の後、インストール時に省略した設定を行う。

#### ホスト名の設定

    echo myarchlinux > /etc/hostname

#### タイムゾーンとハードウェアクロックの設定

    ln -s /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
    hwclock --systohc --utc

#### 新しいユーザーの追加とそのパスワードの設定

    # useradd -m -g users -G wheel -s /bin/bash guest
    # passwd guest

### 参考

- [Installation Guide (日本語) - ArchWiki](https://wiki.archlinux.org/index.php/Installation_Guide_%28%E6%97%A5%E6%9C%AC%E8%AA%9E%29)
- [Beginners' Guide (日本語) - ArchWiki](https://wiki.archlinux.org/index.php/Beginners%27_Guide_%28%E6%97%A5%E6%9C%AC%E8%AA%9E%29)
- パーティション
    - [Master Boot Record (日本語) - ArchWiki](https://wiki.archlinux.org/index.php/Master_Boot_Record_%28%E6%97%A5%E6%9C%AC%E8%AA%9E%29)
    - [GUID Partition Table - ArchWiki](https://wiki.archlinux.org/index.php/GUID_Partition_Table)
- ブートローダ
    - [Syslinux - ArchWiki](https://wiki.archlinux.org/index.php/Syslinux)
    - [GRUB2 - ArchWiki](https://wiki.archlinux.org/index.php/GRUB2)

