---
layout: post
title: "Ubuntu Server 13.04をインストールする"
date: 2013-06-23 00:00
comments: true
categories: [GNU/Linux]
keywords: GNU/Linux, Ubuntu Server 13.04, Raring Ringtail
description: "Ubuntu Server 13.04をインストールした手順の記録です。インストールイメージはubuntu-13.04-server-amd64.isoを使用しています。"
breadcrumb:
  - title: Blog
    url: /
  - title: GNU/Linux
    url: /blog/categories/gnu-slash-linux/

---

　Ubuntu Server 13.04をインストールした手順の記録です。インストールイメージはubuntu-13.04-server-amd64.isoを使用しています。

<!-- more -->

 1. #### インストールイメージの起動

    　インストールイメージは[releases.ubuntu.com](http://releases.ubuntu.com/13.04/)で配布されています。
    光ディスクやUSBメモリ等のメディアをつかって、取得したインストールイメージからコンピュータを起動してください。
    Ubuntuのドキュメントには、インストールイメージのDVDへの書き込み方やUSBメモリから起動する方法を説明するページが用意されています。

    - [How to run Ubuntu from a DVD or USB stick](http://www.ubuntu.com/download/desktop/try-ubuntu-before-you-install)

    {% img /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/01.png 500 Ubuntu Server 13.04 インストーラの起動 %}

 2. #### 言語、地域、キーボードの設定

    - 　インストーラでつかわれる言語を選択します。ここで選択した言語はこれからインストールするUbuntuの標準言語にもなります。日本語を選択することもできますが、この記事では英語を選択してインストールをすすめます。

      {% img /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/02-01.png 500 言語の選択 %}

      　適当な言語を選択するか、または地域化を行わない場合はC (No localization)を選択します。

    - 　次に、地域を選択します。ここで選択した地域がタイムゾーンの基準になります。

      {% img left /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/02-02.png 300 選択した「言語」に基づく地域の一覧 %}
      {% img left /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/02-03.png 300 地域の選択 %}
      {% img      /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/02-04.png 300 国の選択 %}

    - 　次に、キーボード配列を選択します。
    
      {% img left /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/02-05.png 300 キーボード配列の検出 %}
      {% img left /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/02-06.png 300 使用しているキー配列が一般的な国を選択 %}
      {% img      /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/02-07.png 300 キーボード配列の選択 %}

    - 　適当なLocale情報がない場合（上記のように、言語は英語、地域は日本を選択したような場合）、有効なLocales情報から選ぶことができます。ここではen_US.UTF-8を選択します。
    
      {% img /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/02-08.png 500 Locale情報の選択 %}

      　ここまでに選択した地域情報は/etc/default/localeに設定されます。インストール後に設定を変更する場合は[Locales](http://packages.ubuntu.com/raring/locales)をつかいます。

 3. #### ネットワークの設定

    　ネットワークインタフェースの自動検出の後、ホスト名を設定します。ホスト名は/etc/hostnameに設定されます。

    {% img /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/03.png 500 ホスト名の設定 %}

    　インストール後に設定を変更する場合は[hostname](http://packages.ubuntu.com/raring/hostname)をつかいます。

 4. #### ユーザとパスワードの設定

    　ユーザ名、ログイン名、パスワードを設定します。ユーザ名は[pinky](http://manpages.ubuntu.com/manpages/raring/en/man1/pinky.1.html)の書式で、ログイン名とともに/etc/passwdに設定されます。

    {% img left /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/04-01.png 300 ユーザ名の設定 %}
    {% img left /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/04-02.png 300 ログイン名の設定 %}
    {% img      /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/04-03.png 300 パスワードの設定 %}

    　ホームディレクトリを暗号化するかどうかを選択します。[eCryptfs](https://help.ubuntu.com/13.04/serverguide/ecryptfs.html)による暗号化のようです。

    {% img /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/04-04.png 500 ホームディレクトリの暗号化 %}

 5. #### 時刻の設定

    　タイムゾーンを設定します。タイムゾーン情報は/etc/timezoneに設定されます。

    {% img /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/05.png 500 タイムゾーンの設定 %}

    　インストール後に設定を変更する場合は`dpkg-reconfigure tzdata`をつかいます。

 6. #### ディスクのパーティション分割

    　ディスクのパーティションを設定します。パーティション分割にはガイド（自動）で設定する方法と、手動で設定する方法があります。

    1. 　パーティション分割をガイドで行うか、手動で行うかを選択します。ガイドで行う場合はさらに、LVMを設定するかどうか、LVMを設定する場合はLUKSによる暗号化を設定するかどうかを選択します。
       この記事では"Guided - use entire disk and set up LVM"を選択してインストールをつづけます。

       {% img /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/06-01.png 300 パーティショニング方法の選択 %}

    2. 　パーティショニングの対象となるストレージデバイスを選択します。デバイスを正しく認識していれば、この画面の選択肢に現れているはずです。

       {% img /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/06-02.png 300 パーティショニング対象のストレージデバイスの選択 %}

    3. 　LVMを設定する前に、パーティションテーブルの変更をディスクに書き込みます。書込み対象のデバイスをよく確かめて次の画面へすすんでください。

       {% img /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/06-03.png 300 パーティションテーブルの変更の書込み %}

    4. 　LVMのボリュームグループ（VG）に割り当てる容量を指定します。GB単位で指定するか、パーセンテージまたは`max`などで指定できます。

       {% img /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/06-04.png 300 ボリューム・グループ（VG）に割り当てる容量の指定 %}

    5. 　ここまでに設定したパーティションとマウントポイントを確認します。設定を終了し、変更を書き込むには Finish partitioning and write changes to disk を選択します。

       {% img /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/06-05.png 300 パーティションとマウントポイントの確認 %}

    6. 　ストレージデバイスに対する書込みを確認します。書込みを行うとそれまで記録されていた情報は削除されますので、対象デバイスとパーティションをよく確かめて画面をすすめてください。

       {% img /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/06-06.png 300 ストレージデバイスへの書込みの確認 %}

 9. #### パッケージマネージャの設定

    　HTTPプロキシをつかう必要がある場合はプロキシ情報を入力します。必要ない場合は空のまま画面をすすめます。

    {% img /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/07-01.png 500 HTTPプロキシの設定 %}

    　アップデートを自動で適用するかどうかを選択します。また、Canonical社の[Landscape](https://landscape.canonical.com/)を使用する場合の選択肢も用意されています。

    {% img /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/07-02.png 500 自動アップデートの設定 %}

10. #### ソフトウェアの選択

    　ベースシステムと同時にインストールするソフトウェアスイートを選択します。ここで表示されるリストは[tasksel](http://packages.ubuntu.com/raring/tasksel)によって抽象化された「タスク」です。
    どのパッケージをインストールするか、を具体的に指定する必要がある場合（インストールしたいのはmysql-serverではなくpostgresqlである等）は、ここでは選択せずに、ベースシステムのインストール後にインストールします。

    {% img /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/08.png 500 追加ソフトウェアの選択 %}

11. #### ブートローダのインストール

    　GRUBブートローダをインストールします。

    {% img /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/09.png 500 GRUBブートローダのインストール %}

12. #### インストールの完了

    　インストールが完了したら、システムを再起動します。

    {% img /blog/images/2013-06-23-installing-ubuntu-13-dot-04-server-amd64/10.png 500 インストールの完了 %}

## 参考

- [Documentation for Ubuntu 13.04](https://help.ubuntu.com/13.04/)

## 関連記事

- [Debian 7.1.0をインストールする](/blog/2013/06/23/install-debian-7-dot-1-0-amd64-netinst/)
- [CentOS 6.4をインストールする](/blog/2013/04/06/install-centos-6-dot-4-i386-netinstall/)
- [Gentoo Linuxをインストールする](/blog/2013/03/16/install-x86-minimal-20121213/)
- [Arch Linux 2012.12.01をインストールする](/blog/2013/01/05/install-arch-linux/)
