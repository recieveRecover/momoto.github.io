---
layout: post
title: "Red Hat OpenShiftクライアントツールをインストールする"
date: 2013-08-24 11:08
comments: true
categories: [クラウドコンピューティング, OpenShift]
keywords: Red Hat OpenShift, rhc
description: "Red Hat OpenShiftクライアントツールをインストールして、アプリケーションのギアへSSH接続する手引きです。"
breadcrumb:
  - title: Blog
    url: /
  - title: クラウドコンピューティング
    url: /blog/categories/kuraudokonpiyuteingu/

---

　Red Hat OpenShiftに用意されている２通りのユーザインタフェースのうちの１つ、クライアントツールをインストールします。
クライアントツールをつかうと、もう一方のユーザインタフェースであるマネジメントコンソールでは提供されていない機能を利用できます。

<!-- more -->

## インストール

　クライアントツール（以下、rhc）の要件として、Ruby、RubyGems、Gitがあらかじめインストールされている必要があります。
インストールにはパッケージ管理システムごとに何通りかの方法がありますが、OpenShift開発チームの調整が最も反映されているのはGEMパッケージであるようです。

#### RPMパッケージ

　YumをつかってRPMパッケージ版rhcをインストールします。

    $ sudo yum install rhc

#### AURパッケージ

　Pacmanをつかって[AURパッケージ版rhc](https://aur.archlinux.org/packages/rhc/)をインストールします。

    $ curl -Os https://aur.archlinux.org/packages/rh/rhc/rhc.tar.gz
    $ tar xfz rhc.tar.gz
    $ cd rhc
    $ makepkg -i -s

#### GEMパッケージ

　RubyGemsをつかって[gemパッケージ版rhc](http://rubygems.org/gems/rhc)をインストールします。

    $ sudo gem install rhc

## 初期設定

　`rhc setup`コマンドでセットアップウィザードを起動させます。
OpenShiftへのログイン情報を入力するので、あらかじめ[OpenShift](http://openshift.redhat.com/)のアカウントを作成している必要があります。

    $ rhc setup
    OpenShift Client Tools (RHC) Setup Wizard

    This wizard will help you upload your SSH keys, set your application namespace, and check that other programs like Git are properly installed.

    Login to openshift.redhat.com: 

　認証トークンを生成するかどうかを選択します。
トークンを生成しておくと、その有効期限の間はログイン情報の入力を省略できます。このトークンは`rhc logout`で消すことができます。

    OpenShift can create and store a token on disk which allows to you to access the server without using your password. The key is stored in your home directory and should
    be kept secret.  You can delete the key at any time by running 'rhc logout'.
    Generate a token now? (yes|no) yes
    Generating an authorization token for this client ... lasts about 1 day

    Saving configuration to ~/.openshift/express.conf ... done

　SSHの認証につかう公開鍵をOpenShiftリモートサーバへアップロードするかどうかを選択します。
セットアップウィザードに従う場合、ホームディレクトリから見つかったid_rsaとid_rsa.pubのペアのうち、id_rsa.pubのほうをアップロードします。

    Your public SSH key must be uploaded to the OpenShift server to access code.  Upload now? (yes|no) 

## SSH接続

　`rhc ssh <app>`コマンドをつかって、SSH接続を試してみます。今までに作成したアプリケーションは`rhc apps`コマンドで確認することができます。

    $ rhc ssh <app>
    Connecting to ********@<app>-<namespace>.rhcloud.com ...

    *********************************************************************

    You are accessing a service that is for use only by authorized users.
    If you do not have authorization, discontinue use at once.
    Any use of the services is subject to the applicable terms of the
    agreement which can be found at:
    https://www.openshift.com/legal

    *********************************************************************

    Welcome to OpenShift shell

    This shell will assist you in managing OpenShift applications.

    !!! IMPORTANT !!! IMPORTANT !!! IMPORTANT !!!
    Shell access is quite powerful and it is possible for you to
    accidentally damage your application.  Proceed with care!
    If worse comes to worst, destroy your application with 'rhc app delete'
    and recreate it
    !!! IMPORTANT !!! IMPORTANT !!! IMPORTANT !!!

    Type "help" for more info.

    [<app>-<namespace>.rhcloud.com ********]\>

　`help`では、OpenShiftアプリケーション環境で利用できるコマンドを参照することができます。

    Help menu: The following commands are available to help control your openshift
    application and environment.

    ctl_app         control your application (start, stop, restart, etc)
    ctl_all         control application and deps like mysql in one command
    tail_all        tail all log files
    export          list available environment variables
    rm              remove files / directories
    ls              list files / directories
    ps              list running applications
    kill            kill running applications
    mysql           interactive MySQL shell
    mongo           interactive MongoDB shell
    psql            interactive PostgreSQL shell
    quota           list disk usage

## 参考

- [User Guide - Red Hat Customer Portal](https://access.redhat.com/site/documentation/en-US/OpenShift_Online/2.0/html/User_Guide/index.html)
- [Client Tools Installation Guide - Red Hat Customer Portal](https://access.redhat.com/site/documentation/en-US/OpenShift_Online/2.0/html/Client_Tools_Installation_Guide/index.html)

## 関連記事

- [Red Hat OpenShiftにRedmine 2.0を展開する](/blog/2013/08/24/deploying-redmine-2-dot-0-on-openshift/)

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_top&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=seijimomotobl-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4798121622" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_top&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=seijimomotobl-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4798034401" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
