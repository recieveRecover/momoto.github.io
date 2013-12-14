---
layout: post
title: "QEMUをつかって仮想マシンを作成する"
date: 2013-08-11 14:54
comments: true
categories: [仮想化]
keywords: QEMU version 1.5.2, KVM, Virtualization, 仮想化
description: "QEMUをつかって仮想マシンを作成する手引きです。ホスト側に仮想化支援機能が備わっていることを確かめたあと、QEMUをつかってゲストOSを構築します。"
breadcrumb:
  - title: Blog
    url: /
  - title: 仮想化
    url: /blog/categories/jia-xiang-hua/

---

　QEMUをつかって仮想マシンを作成する手引きです。ホスト側に仮想化機能が備わっていることを確かめたあと、QEMUをつかってゲストOSを構築します。

<!-- more -->

## 仮想化機能を確かめる

　ホストとなるGNU/Linuxシステムで、CPUのハードウェア仮想化支援機能とLinux KVMのサポートを確かめます。

 1. ### ハードウェアサポートを確かめる

    　lscpuの仮想化（Virtualization）の行、または/proc/cpuinfoのflagsの行から、ハードウェア仮想化支援機能（Hardware-Assisted Virtualization）を確認します。
    この機能はベンダーごとに実装が異なるので、インテル製品であればVT-xを、AMD製品であればAMD-Vを確かめます。

        $ lscpu | grep -Ei "(vt-x|amd-v)"
        仮想化:             AMD-V

    　/proc/cpuinfoでは、VT-xの動作モード VMX、または、AMD-VのSecure virtual machine（svm）を確かめます。

        $ grep -E "(vmx|svm)" /proc/cpuinfo
        flags : ... svm ... svm_lock ...

 2. ### カーネルサポートを確かめる

    Linux KVMカーネルモジュールが有効になっていることを確認します。/proc/config.gzまたはlsmodから確かめます。

        $ zgrep -E "KVM|VIRTUALIZATION" /proc/config.gz
        CONFIG_KVM_GUEST=y
        CONFIG_HAVE_KVM=y
        CONFIG_HAVE_KVM_IRQCHIP=y
        CONFIG_HAVE_KVM_IRQ_ROUTING=y
        CONFIG_HAVE_KVM_EVENTFD=y
        CONFIG_KVM_APIC_ARCHITECTURE=y
        CONFIG_KVM_MMIO=y
        CONFIG_KVM_ASYNC_PF=y
        CONFIG_HAVE_KVM_MSI=y
        CONFIG_HAVE_KVM_CPU_RELAX_INTERCEPT=y
        CONFIG_VIRTUALIZATION=y
        CONFIG_KVM=m
        CONFIG_KVM_INTEL=m
        CONFIG_KVM_AMD=m
        CONFIG_KVM_MMU_AUDIT=y
        CONFIG_KVM_DEVICE_ASSIGNMENT=y

        $ lsmod | grep kvm
        kvm_amd                52151  0
        kvm                   376394  1 kvm_amd

    　また、ユーザは/dev/kvmへアクセスできる権限が必要です。Arch Linuxではkvmグループが用意されているので、ハイパーバイザを利用するユーザをこのグループに追加します。

        $ sudo gpasswd -a $(whoami) kvm

## ゲストOSを構築する

 1. ### 仮想ディスクイメージを作る

    　qemu-imgをつかって、ゲストOSのディスクイメージを作ります。
    オプションとして、ディスクイメージのファイル形式、ファイル名、サイズを指定します。qcow2はQEMUでつかわれるイメージファイル形式です。

        $ qemu-img create -f qcow2 ~/Workspace/CentOS-6.4-x86_64-netinstall.qcow2 8G
        Formatting '/home/guest/Workspace/CentOS-6.4-x86_64-netinstall.qcow2', fmt=qcow2 size=8589934592 encryption=off cluster_size=65536 lazy_refcounts=off

 2. ### ゲストOSをインストールする

    　ゲストOSをインストールするために、qemu-system-&lt;architecture&gt;をつかって仮想マシンを起動します。
    このときのオプションには、OSインストールの方法にしたがって適当なオプションを付ける必要があります。
    インストールメディアをCentOS-6.4-x86_64-netinstall.isoとして、光学ドライブから起動させる場合は`-boot order=d`と`-cdrom <file>`のオプションを付けます。

        $ qemu-system-x86_64 \
        -enable-kvm \
        -m 512 \
        -boot order=d \
        -cdrom ~/Downloads/CentOS-6.4-x86_64-netinstall.iso \
        ~/Workspace/CentOS-6.4-x86_64-netinstall.qcow2

    [![Cent OS 6.4をゲストOSとしてインストールする](/blog/images/2013-08-11-guide-to-creating-virtual-machine-with-qemu/installing-centos-6-4-as-a-guest-os.thumbnail.png)](/blog/images/2013-08-11-guide-to-creating-virtual-machine-with-qemu/installing-centos-6-4-as-a-guest-os.png)

    - 　`failed to initialize KVM: Device or resource busy` とでて先にすすめない場合は、ホスト側で別のハイパーバイザが起動していないか確認してください。
    - 　ゲスト側で `Trying to unpack rootfs image as initramfs` とでるところで起動がとまってしまう場合は、ゲストOSに必要なメモリが足りていないようです。
      QEMUの`-m`オプションの値を引き上げて試してみてください。

    　インストール後はインストールメディアのオプションを外して仮想マシンを起動します。

        $ qemu-system-x86_64 \
        -enable-kvm \
        -m 512 \
        ~/Workspace/CentOS-6.4-x86_64-netinstall.qcow2

    [![ゲストOSの起動](/blog/images/2013-08-11-guide-to-creating-virtual-machine-with-qemu/booting-guest-os.thumbnail.png)](/blog/images/2013-08-11-guide-to-creating-virtual-machine-with-qemu/booting-guest-os.png)

## 参考

- [QEMU](http://wiki.qemu.org/Main_Page)
- [The QCOW2 Image Format](https://people.gnome.org/~markmc/qcow-image-format.html)

### 関連記事

- [CentOS 6.4をインストールする](/blog/2013/04/06/install-centos-6-dot-4-i386-netinstall/)
- [Debian 7.1.0をインストールする](/blog/2013/06/23/install-debian-7-dot-1-0-amd64-netinst/)
- [Gentoo Linuxをインストールする](/blog/2013/03/16/install-x86-minimal-20121213/)
- [Arch Linux 2012.12.01をインストールする](/blog/2013/01/05/install-arch-linux/)
