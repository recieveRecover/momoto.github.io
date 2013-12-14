---
layout: post
title: "Debian 7.1.0をインストールする"
date: 2013-06-23 05:54
comments: true
categories: [GNU/Linux]
keywords: GNU/Linux, Debian 7.1.0, Wheezy, debian-7.1.0-amd64-netinst
description: "Debian 7.1.0をインストールした手順の記録です。インストールメディアはdebian-7.1.0-amd64-netinst.isoを使用しています。"
breadcrumb:
  - title: Blog
    url: /
  - title: GNU/Linux
    url: /blog/categories/gnu-slash-linux/

---

　Debian 7.1.0をインストールした手順の記録です。インストールメディアはdebian-7.1.0-amd64-netinst.isoを使用しています。インターネットへ接続できる環境を前提にしています。

{% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/01.png 400 インストーラの起動 %}

<!-- more -->

 1. ### 言語、地域、キーボードの選択

    1. 　インストーラで使用される言語とベースシステムに含める言語パッケージを選択します。この記事では日本語を選択してインストールをすすめます。
    
       {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/02-01.png 400 言語の選択 %}

    2. 　次に、地域を選択します。先に選択した言語に関連する地域にあらかじめ焦点があてられています。
    
       {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/02-02.png 400 場所の選択 %}

    3. 　続いて、キーボード配列を選択します。
    
       {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/02-03.png 400 キーボードの設定 %}

 2. ### ネットワークの設定

    1. ホスト名を入力します。

       {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/03-01.png 400 ホスト名の入力 %}

    2. ドメイン名を入力します。

       {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/03-02.png 400 ドメイン名の入力 %}

 3. ### ユーザとパスワードのセットアップ

    1. rootのパスワードを入力します。確認のため入力は二度、必要です。
    
       {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/04-01.png 400 rootのパスワードを設定 %}

    2. 一般ユーザの名前を入力します。名前は次の画面でも入力しますが、ここで入力する名前はGUIのログイン画面等で表示されます。

       {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/04-02.png 400 非管理者権限ユーザの名前（本名）を入力 %}

    3. 続いて、ホームディレクトリのディレクトリ名にもなるユーザ名を入力します。前の画面で入力した名前よりも、使用出来る文字は制限されます。

       {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/04-03.png 400 非管理者権限ユーザの名前（ログイン名）を入力 %}

    4. 一般ユーザのパスワードを入力します。rootのときと同様に入力は二度、必要です。
    
       {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/04-04.png 400 非管理者権限ユーザのパスワードを設定 %}

 4. ### ディスクのパーティション分割

    　ディスクのパーティションを設定します。パーティション分割にはガイド（自動）で設定する方法と、手動で設定する方法があります。

    1. 　パーティション分割をガイドで行うか、手動で行うかを選択します。ガイドで行う場合はさらに、LVMを設定するかどうか、LVMを設定する場合はLUKSによる暗号化を設定するかどうかを選択します。
       この記事では「ガイド - ディスク全体を使う」でインストールを進めます。
    
       {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/05-01.png 400 パーティショニングを自動で行うか、手動で行うかを選択する %}

    2. 　パーティショニングの対象となるストレージデバイスを選択します。デバイスを正しく認識していれば、この画面の選択肢に現れているはずです。

       {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/05-02.png 400 パーティショニングの対象になるストレージデバイスを選択する %}

    3. 　ガイドに従ってパーティション構成を選択します。ここで選択できるものより複雑なパーティショニングが必要な場合は手動で設定しなければなりません。この記事では「すべてのファイルを１つのパーティションに」を選択してインストールをすすめます。

       {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/05-03.png 400 パーティション構成を選択する %}

    4. 　ここまでのパーティション設定を確認します。
    
       {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/05-04.png 400 パーティション設定を確認する %}

    5. 　ストレージデバイスに対する書込みの最後の確認です。書込みを行うとそれまで記録されていた情報は削除されますので、対象デバイスとパーティションをよく確かめて画面をすすめてください。
    
       {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/05-05.png 400 ストレージデバイスに対する書込みを確認する %}

 5. ### パッケージマネージャの設定

    * 　Debianアーカイブミラーをホストしている地域を選択します。「自国でさえ最適の選択とは限らない」とありますが、国内で利用する場合においては「日本」を選択するのが最も適当です。
    
      {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/06-01.png 400 Debianアーカイブミラーをホストしている地域を選択する %}

    * 　選択した地域からミラーサーバを選択します。[ftp.jp.debian.org](ftp://ftp.jp.debian.org/)と[ftp.jaist.ac.jp](ftp://ftp.jaist.ac.jp/)については、管理者のブログが公開されています（[debiancdn | AWS, Content Delivery Network and Debian](http://debiancdn.wordpress.com/)、[ftp-adminの憂鬱](http://ftp-admin.blogspot.jp/)）。

      {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/06-02.png 400 Debianアーカイブミラーサーバを選択する %}

    * 　HTTPプロキシをつかう必要がある場合はプロキシ情報を入力します。必要ない場合は空のまま画面をすすめます。
    
      {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/06-03.png 400 HTTPプロキシ情報を入力する %}

    * 　Debianパッケージ利用調査へ参加するかどうかを選択します。この調査結果は[Debian Popularity Contest](http://popcon.debian.org/)で公開されています。

      {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/06-04.png 400 Debianパッケージ利用調査への参加 %}

 6. ### ソフトウェアの選択

    　ベースシステムと同時にインストールするソフトウェアスイートを選択します。ここで表示されるリストは[tasksel](http://wiki.debian.org/tasksel)によって抽象化された「タスク」です。
    どのパッケージをインストールするか、を具体的に指定する必要がある場合（インストールしたいのはmysql-serverではなくpostgresqlである等）は、ここでは選択せずに、ベースシステムのインストール後にインストールします。

    {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/07.png 400 ソフトウェアの選択 %}

 7. ### ブートローダのインストール

    　GRUBブートローダをインストールします。

    {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/08.png 400 ブートローダのインストール %}

 8. ### インストールの完了

    　インストールが完了したら、システムを再起動します。

    {% img /blog/images/2013-06-23-debian-7.1.0-amd64-netinst/09.png 400 インストールの完了 %}

### 参考

* [Debian wheezy -- インストールガイド](http://www.debian.org/releases/stable/installmanual)

### 関連記事

- [Ubuntu Server 13.04をインストールする](/blog/2013/06/23/installing-ubuntu-13-dot-04-server-amd64/)
- [CentOS 6.4をインストールする](/blog/2013/04/06/install-centos-6-dot-4-i386-netinstall/)
- [Gentoo Linuxをインストールする](/blog/2013/03/16/install-x86-minimal-20121213/)
- [Arch Linux 2012.12.01をインストールする](/blog/2013/01/05/install-arch-linux/)
