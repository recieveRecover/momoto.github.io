---
layout: post
title: "GPT規格のディスクにSyslinuxをインストールする"
date: 2013-04-08 20:46
comments: true
categories: [GNU/Linux]
keywords: GPT, gptfdisk, sgdisk, syslinux
description: "GUIDパーティションテーブル（GPT）規格のディスクにSyslinuxをインストールした手順の記録です。"
breadcrumb:
  - title: Blog
    url: /
  - title: GNU/Linux
    url: /blog/categories/gnu-slash-linux/

---

　GUIDパーティションテーブル（GPT）規格のディスクにSyslinuxをインストールした手順の記録です。OSはArch Linux、パーティショニングにはgptfdisk（gdisk）を使用しています。GPTの利点は2TiBを超える記憶容量を管理できることにありますが、ここでは練習のため8GBの仮想ディスクを使用します。パーティションは以下のようにrootとbootだけに分割します。

    Number  Start (sector)    End (sector)  Size       Code  Name
       1            2048          206847   100.0 MiB   EF02  BIOS boot partition
       2          206848        16777182   7.9 GiB     8300  Linux filesystem
<!-- more -->
### 1. gdiskを使用してパーティションを区切る

　対象のストレージデバイスを指定してgdiskを起動します。本稿では/dev/sdaに対してパーティションを区切っていきます。

    root@archiso ~ # gdisk /dev/sda
    GPT fdisk (gdisk) version 0.8.5

    Partition table scan:
      MBR: not present
      BSD: not present
      APM: not present
      GPT: not present

    Creating new GPT entries.

    Command (? for help): 

　`o`で空のパーティションテーブルを作成します。このときデバイスのすべてのパーティションが削除されることに注意してください。

    Command (? for help): o
    This option deletes all partitions and creates a new protective MBR.
    Proceed? (Y/N): Y

　`n`で新しいパーティションを作成します。前述の構成のとおり、２つのパーティションを作成します。

    Command (? for help): n
    Partition number (1-128, default 1): 
    First sector (34-16777182, default = 2048) or {+-}size{KMGTP}: 
    Last sector (2048-16777182, default = 16777182) or {+-}size{KMGTP}: +100M
    Current type is 'Linux filesystem'
    Hex code or GUID (L to show codes, Enter = 8300): ef02
    Changed type of partition to 'BIOS boot partition'

    Command (? for help): n
    Partition number (2-128, default 2): 
    First sector (34-16777182, default = 206848) or {+-}size{KMGTP}: 
    Last sector (206848-16777182, default = 16777182) or {+-}size{KMGTP}: 
    Current type is 'Linux filesystem'
    Hex code or GUID (L to show codes, Enter = 8300): 
    Changed type of partition to 'Linux filesystem'

　`p`で現時点のパーティションを表示し、構成を確認します。

    Command (? for help): p
    Disk /dev/sda: 16777216 sectors, 8.0 GiB
    Logical sector size: 512 bytes
    Disk identifier (GUID): ********-****-****-****-************
    Partition table holds up to 128 entries
    First usable sector is 34, last usable sector is 16777182
    Partitions will be aligned on 2048-sector boundaries
    Total free space is 2014 sectors (1007.0 KiB)

    Number  Start (sector)    End (sector)  Size       Code  Name
       1            2048          206847   100.0 MiB   EF02  BIOS boot partition
       2          206848        16777182   7.9 GiB     8300  Linux filesystem

　`w`でテーブルをストレージデバイスに書き込みます。

    Command (? for help): w

    Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
    PARTITIONS!!

    Do you want to proceed? (Y/N): Y
    OK; writing new GUID partition table (GPT) to /dev/sda.
    The operation has completed successfully.

　ファイルシステムまで作成したらストレージデバイスの用意は完了です。

    # mkfs.ext4 /dev/sda1
    # mkfs.ext4 /dev/sda2

### 2. ブートローダのインストール

　ディストリビューション等で配布されているSyslinuxをインストールします。また、Arch Linuxのようにchroot環境でブートローダをインストールする場合、sgdiskが含まれているgptfdiskパッケージも同時にインストールします。syslinux.cfgではルートのファイルシステム（前述の構成では/dev/sda2）が正しく指定されている必要があります。

    # pacman -S gptfdisk syslinux

        ...
        Optional dependencies for syslinux
            gptfdisk: For GPT support [installed]
            util-linux: For isohybrid [installed]

    # vi /boot/syslinux/syslinux.cfg

        ...
        LABEL arch
                MENU LABEL Arch Linux
                LINUX ../vmlinuz-linux
                APPEND root=/dev/sda2 ro
                INITRD ../initramfs-linux.img
        ...

    # syslinux-install_update -i -a

        Syslinux install successful
        Attribute Legacy Bios Bootable Set - /dev/sda1

　sgdiskでパーティション属性を指定した後、ブートコード（gptmbr.bin）を設置します。

    # sgdisk /dev/sda --attributes=1:set:2

        Warning: The kernel is still using the old partition table.
        The new table will be used at the next reboot.
        The operation has completed successfully.

    # sgdisk /dev/sda --attributes=1:show

        1:2:1 (legacy BIOS bootable)

    # dd bs=440 conv=notrunc count=1 if=/usr/lib/syslinux/gptmbr.bin of=/dev/sda

        1+0 records in
        1+0 records out
        440 bytes (440 B) copied, 0.00221059 s, 199 kB/s

### 関連

- [Arch Linux 2012.12.01のインストール](/blog/2013/01/05/install-arch-linux/)

### 参考

- [Partitioning - ArchWiki](https://wiki.archlinux.org/index.php/Partitioning)
- [GUID Partition Table - ArchWiki](https://wiki.archlinux.org/index.php/GUID_Partition_Table)
- [Syslinux - ArchWiki](https://wiki.archlinux.org/index.php/Syslinux)
- [Mbr - Syslinux Wiki](http://www.syslinux.org/wiki/index.php/Mbr)
- [Common Problems - Syslinux Wiki](http://www.syslinux.org/wiki/index.php/Common_Problems#Missing_OS_.28gptmbr.bin.29)
