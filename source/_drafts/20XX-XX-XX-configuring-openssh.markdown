---
layout: post
title: "OpenSSHを利用する"
date: 2013-07-27 00:00
comments: true
categories: [暗号化, OpenSSH]
keywords: OpenSSH, Secure Shell
description: ""
breadcrumb:
  - title: Blog
    url: /
  - title: 暗号化
    url: /blog/categories/an-hao-hua/

---

　OpenSSH各種コマンドの利用方法を簡単にまとめています。
接続する側にはSSHクライアントが、接続される側にはSSHサーバーが必要です。<!-- more -->

### クライアント側

　クライアント側で、接続先情報（接続先アドレス、ユーザ名、パスワード、秘密鍵など）を引数にしてsshコマンドを実行します。

    # ssh username@hostname

### サーバ側

　サーバ側でsshdが起動していることを確かめます。

    # /etc/init.d/sshd status
    openssh-daemon (pid  1292) is running...

## 参考

- [遠隔端末接続（SSH）サービスの利用手引き](http://www.kyoto-su.ac.jp/ccinfo/from_home/ssh/index.html)
- [](http://www.openssh.com/ja/)
  - [](http://www.openssh.org/ja/manual.html)

CentOS 6.4には標準で

    [root@localhost ~]# yum info openssh-server
    Loaded plugins: fastestmirror
    Loading mirror speeds from cached hostfile
     * base: ftp.riken.jp
     * extras: ftp.riken.jp
     * updates: ftp.riken.jp
    Installed Packages
    Name        : openssh-server
    Arch        : x86_64
    Version     : 5.3p1
    Release     : 84.1.el6
    Size        : 652 k
    Repo        : installed
    From repo   : anaconda-CentOS-201303020151.x86_64
    Summary     : An open source SSH server daemon
    URL         : http://www.openssh.com/portable.html
    License     : BSD
    Description : OpenSSH is a free version of SSH (Secure SHell), a program for logging
                : into and executing commands on a remote machine. This package contains
                : the secure shell daemon (sshd). The sshd daemon allows SSH clients to
                : securely connect to your SSH server.

    [root@localhost ~]# yum info openssh-clients
    Loaded plugins: fastestmirror
    Loading mirror speeds from cached hostfile
     * base: ftp.riken.jp
     * extras: ftp.riken.jp
     * updates: ftp.riken.jp
    Installed Packages
    Name        : openssh-clients
    Arch        : x86_64
    Version     : 5.3p1
    Release     : 84.1.el6
    Size        : 1.0 M
    Repo        : installed
    From repo   : base
    Summary     : An open source SSH client applications
    URL         : http://www.openssh.com/portable.html
    License     : BSD
    Description : OpenSSH is a free version of SSH (Secure SHell), a program for logging
                : into and executing commands on a remote machine. This package includes
                : the clients necessary to make encrypted connections to SSH servers.

