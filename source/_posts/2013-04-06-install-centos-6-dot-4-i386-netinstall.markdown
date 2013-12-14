---
layout: post
title: "CentOS 6.4をインストールする"
date: 2013-04-06 01:37
comments: true
categories: [GNU/Linux]
keywords: RHEL, CentOS 6.4, CentOS-6.4-i386-netinstall, GNU/Linux
description: "CentOS 6.4をインストールした手順の記録です。インストールメディアはCentOS-6.4-i386-netinstall.isoを使用しています。"
breadcrumb:
  - title: Blog
    url: /
  - title: GNU/Linux
    url: /blog/categories/gnu-slash-linux/

---

　CentOS 6.4をインストールした手順の記録です。インストールメディアはCentOS-6.4-i386-netinstall.isoを使用しています。インターネットへ接続できる環境を前提にしています。

<!-- more -->

 1. ### Anacondaの起動
    
    [{% img /blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/01_thumbnail.png Anacondaの起動 %}](/blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/01.png)

    　インストールメディアからCentOSのインストーラ"Anaconda"を起動します。このとき、インストールメディア（光学ドライブやUSB、あるいはISOイメージファイル）が優先的に起動されるようにコンピュータ側（BIOSや仮想化環境）で正しく設定されている必要があります。
    
    　インストールを開始するために、"Install or upgrade an existing system"を選択します。

 2. ### インストールメディアのチェック

    [{% img /blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/02_thumbnail.png インストールメディアのチェック %}](/blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/02.png)

    　必要に応じてメディアチェックを行います。このチェックはインストールに必ずしも必要ではありません。チェックを行う場合は「OK」を、チェックを省略してインストールを開始する場合は「Skip」を選択します。

 3. ### 言語の選択

    [{% img /blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/03_thumbnail.png 言語の選択 %}](/blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/03.png)

    　インストーラで使用される言語とベースシステムに含める言語パッケージを選択します。日本語も用意されていますが、翻訳が未対応の箇所では原文が表示されます。この記事では英語を選択してインストールを進めます。

 4. ### キーボード配列の選択

    [{% img /blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/04_thumbnail.png キーボード配列の選択 %}](/blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/04.png)

    使用しているキーボードにあわせて、キーボード配列を選択します。日本語キーボードであれば"jp106"を選択します。

 5. ### インストール方法の選択

    [{% img /blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/05_thumbnail.png インストール方法の選択 %}](/blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/05.png)

    ネットワークインストールの場合、"URL"を選択します。

    [{% img /blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/06_thumbnail.png TCP&frasl;IPの設定 %}](/blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/06.png)

    　続いて、TCP/IPを設定します。IPv4、IPv6サポートの有無と、それぞれのプロトコルにおいて動的IP（DHCP）を使用するかどうかを選択します。この記事ではIPv4のみ有効化し、動的IPを使用するように設定してインストールを進めます。

    [{% img /blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/07_thumbnail.png ネットワークインストールのURLの設定 %}](/blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/07.png)

    　[CentOS Mirrors](http://www.centos.org/modules/tinycontent/index.php?id=32)にあるミラー等を参考に、対象アーキテクチャの"images/install.img"のURLを入力します。例えば、アーキテクチャが"i386"で、[理研](http://www.riken.jp/)のミラーを使用する場合のURLは http://ftp.riken.jp/Linux/centos/6.4/os/i386/images/install.img になります。

    [{% img /blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/08_thumbnail.png ネットワークインストールのURLの入力 %}](/blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/08.png)

    　`OSError: /lib64/libudev.so.0: wrong ELF class: ELFCLASS64`と表示されて処理が止まった場合は、install.imgのアーキテクチャが正しいかどうか確認してください。

 6. ### グラフィカルインストールの開始

    [{% img /blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/09_thumbnail.png Anacondaのグラフィカルユーザインタフェース %}](/blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/09.png)

    　続いて、AnacondaのGUIにしたがってインストールを進めていきます。ブートメニューに指定するオプション（`boot: linux text`）によって、テキストモードインストールを使用することも可能ですが、テキストモードでは設定できない項目もあるため、通常はグラフィカルモードを使用します。

 7. ### ストレージ形式の選択

    [{% img /blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/10_thumbnail.png ストレージ形式の選択 %}](/blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/10.png)
    
    特殊なストレージを使用する必要がなければ"Basic Storage Devices"を選択します。

 8. ### ホスト名の入力

    [{% img /blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/11_thumbnail.png ホスト名の入力 %}](/blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/11.png)

    ホスト名を入力します。

 9. ### タイムゾーンの指定

    [{% img /blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/12_thumbnail.png タイムゾーンの指定 %}](/blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/12.png)

    タイムゾーンの指定と、システムクロックにUTCを使用するかどうかを選択します。

10. ### rootパスワードの設定

    [{% img /blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/13_thumbnail.png rootパスワードの設定 %}](/blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/13.png)

11. ### パーティションの設定

    [{% img /blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/14_thumbnail.png パーティションの設定 %}](/blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/14.png)

    　ストレージデバイスのパーティション設定を行います。上４つの選択肢を選んだ場合、自動的にデフォルトのパーティションが構成されます。手動でパーティション構成を設定する場合は一番下の"Create Custom Layout"を選択します。
    また、LUKS（Linux Unified Key Setup）による暗号化を行うかどうかを、この画面で指定出来ます。

    [{% img /blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/15_thumbnail.png ストレージへの書き込みに対する警告 %}](/blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/15.png)

    　この画面以降、ストレージデバイスへの書込みが行われます。この時点で接続しているストレージデバイスや上書きが行われる記憶領域をよく確かめて、次の画面へ進んでください。

12. ### パッケージグループの選択

    [{% img /blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/16_thumbnail.png パッケージグループの選択 %}](/blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/16.png)

    　用途にあわせてパッケージグループを選択します。パッケージはOSインストール後にも当然、インストールすることができます。単一目的サーバーの土台として、最小限のパッケージのみをインストールする場合は"Minimal"を選択します。

    [{% img /blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/17_thumbnail.png ベースシステムのインストール %}](/blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/17.png)

13. ### 再起動

    [{% img /blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/18_thumbnail.png インストールの完了 %}](/blog/images/2013-04-06-install-centos-6-dot-4-i386-netinstall/18.png)

    　インストールが完了し、システムを再起動します。

### 参考

- [Red Hat Enterprise Linux 6 インストールガイド](https://access.redhat.com/site/documentation/ja-JP/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/index.html)
- [CentOS Asian Mirrors](http://www.centos.org/modules/tinycontent/index.php?id=31)
<iframe src="http://rcm-jp.amazon.co.jp/e/cm?lt1=_top&bc1=000000&IS2=1&npa=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=alqet049-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4774145017" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
<iframe src="http://rcm-jp.amazon.co.jp/e/cm?lt1=_top&bc1=000000&IS2=1&npa=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=alqet049-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4789840875" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
