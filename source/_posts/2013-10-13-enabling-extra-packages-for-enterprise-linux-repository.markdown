---
layout: post
title: "EPEL 6のリポジトリを有効化する"
date: 2013-10-13 11:09
comments: true
categories: [GNU/Linux]
keywords: Extra Packages for Enterprise Linux 6, EPEL 6, RPM, Yum
descriptions: "CentOS 6でExtra Packages for Enterprise Linux 6（EPEL 6）リポジトリを有効化します。"
breadcrumb:
  - title: Blog
    url: /
  - title: GNU/Linux
    url: /blog/categories/gnu-slash-linux/

---

　CentOS 6でExtra Packages for Enterprise Linux 6（EPEL 6）リポジトリを有効化します。
このリポジトリを有効化することでRHEL 6やそのクローンでもFedoraパッケージを利用できるようになります。

<!-- more -->

　まず、パッケージの検証につかう公開鍵をRPMデータベースにインポートします。
EPEL6でつかう公開鍵は[Fedora Project - List of GPG keys](https://fedoraproject.org/keys)の[RPM-GPG-KEY-EPEL-6](https://fedoraproject.org/static/0608B895.txt)です。

    $ sudo rpm --import https://fedoraproject.org/static/0608B895.txt

　次に、EPEL6のミラーからRPMパッケージをインストールして、リポジトリの有効化は完了です。

    $ sudo rpm -Uvh http://ftp.iij.ad.jp/pub/linux/fedora/epel/6/i386/epel-release-6-8.noarch.rpm
    Retrieving http://ftp.iij.ad.jp/pub/linux/fedora/epel/6/i386/epel-release-6-8.noarch.rpm
    Preparing...                ########################################### [100%]
       1:epel-release           ########################################### [100%]

　RPMファイルのURLは、ミラーに[IIJ](http://www.iij.ad.jp/)を選んだ場合だと次のようになります。

- **i386** http://ftp.iij.ad.jp/pub/linux/fedora/epel/6/i386/epel-release-6-8.noarch.rpm
- **x86_64** http://ftp.iij.ad.jp/pub/linux/fedora/epel/6/x86_64/epel-release-6-8.noarch.rpm

　追加したEPELのリポジトリが有効になっているかどうかは`yum repolist`をつかって確認することができます。

    $ yum repolist
    
    repo id           repo name                                               status
    base              CentOS-6 - Base                                         4,802
    epel              Extra Packages for Enterprise Linux 6 - i386            8,113
    extras            CentOS-6 - Extras                                          12
    updates           CentOS-6 - Updates                                      1,000
    repolist: 13,927

　yumオプションに`--enablerepo=epel`を指定したときだけEPELを利用し、普段は無効化にしておきたい場合は/etc/yum.repos.d/epel.repoの`enabled=1`を`enabled=0`に変更しておきます。

    $ sudo vi /etc/yum.repos.d/epel.repo
    
    [epel]
    name=Extra Packages for Enterprise Linux 6 - $basearch
    #baseurl=http://download.fedoraproject.org/pub/epel/6/$basearch
    mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-6&arch=$basearch
    failovermethod=priority
    enabled=0
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-6

#### 参考

- [EPEL - FedoraProject](http://fedoraproject.org/wiki/EPEL)
- [AdditionalResources/Repositories - CentOS Wiki](http://wiki.centos.org/AdditionalResources/Repositories)
