---
layout: post
title: "Apache HTTP Server 2.4.6をインストールする（ソースからビルドする方法）"
date: 2013-07-31 22:10
comments: true
categories: [WWW, Apache]
keywords: Apache HTTP Server 2.4.6, httpd, APR, PCRE, iptables, SELinux
description: "Apache HTTP Server 2.4.6をソースコードからビルドして、Unix系システムへインストールする手引きです。"
breadcrumb:
  - title: Blog
    url: /
  - title: WWW
    url: /blog/categories/www/

---

　Apache HTTP Server 2.4.6をソースコードからビルドして、Unix系システムへインストールする手引きです。
この記事ではCentOS 6.4環境でインストールの例を示しますが、自身でソースからビルドする方法は特定のLinuxディストリビューションに依りません。<!-- more -->
インストールを行うユーザは、インストール先ディレクトリに対して書込み権限をもっている必要があります。/usr/local/* にインストールする場合、通常、root権限が必要です。

 1. ### ソースコードを取得する

    　ソースコードをインストールするマシンに用意します。
    インターネットからダウンロードする場合、[httpd.apache.org][1]にソースコードのURLが示されています。
    bzip2、gzipのアーカイブが用意されていますので、マシンで解凍・展開できる形式を選択してください。

    　ダウンロードにはcURLやGNU Wgetなどのダウンロードマネージャやウェブブラウザを使用します。
    次の例では、cURLをつかって[理化学研究所][2]のミラーからhttpd-2.4.6.tar.gzをダウンロードします。
    
        [root@localhost ~]# curl -LsO http://ftp.riken.jp/net/apache//httpd/httpd-2.4.6.tar.gz

    　取得したアーカイブファイルは適宜、解凍・展開して、ワーキングディレクトリを移動させます。
    [FHS][3]に従うシステムであれば、独自にインストールするソフトウェアのためのディレクトリ /usr/local が予め用意されていますので、この記事ではソースコードを /usr/local/src に配置してインストールをすすめます。

        [root@localhost ~]# tar xfz httpd-2.4.6.tar.gz -C /usr/local/src/
        [root@localhost ~]# cd /usr/local/src/httpd-2.4.6/
        [root@localhost httpd-2.4.6]# 

 2. ### インストール要件を満たす

    　マシンがインストールの要件を満たしている必要があります。
    要件については[Documentation][8]に詳しい説明があります。

    - Cコンパイラ

          [root@localhost ~]# yum install gcc

    - Make

          [root@localhost ~]# yum install make

    - [APR][7] - 予めシステムに備わっていない場合（またはシステムが提供するバージョンを使用したくない場合）は[apr.apache.org][9]から取得してhttpdに含めてビルドします。取得したAPRとAPR-Utilはバージョン番号を取り除いて"./srclib/apr"と"./srclib/apt-util"に展開します。

          [root@localhost httpd-2.4.6]# cd srclib/
          [root@localhost srclib]# curl -Lso apr-1.4.8.tar.gz http://ftp.jaist.ac.jp/pub/apache//apr/apr-1.4.8.tar.gz
          [root@localhost srclib]# tar xfz apr-1.4.8.tar.gz
          [root@localhost srclib]# mv apr-1.4.8/ apr
          [root@localhost srclib]# curl -Lso apr-util-1.5.2.tar.gz http://ftp.jaist.ac.jp/pub/apache//apr/apr-util-1.5.2.tar.gz
          [root@localhost srclib]# tar xfz apr-util-1.5.2.tar.gz
          [root@localhost srclib]# mv apr-util-1.5.2/ apr-util

    - PCREライブラリ - [www.pcre.org][12]やディストリビューションからインストールします。

          [root@localhost httpd-2.4.6]# yum install pcre-devel

 3. ### ビルドファイルを作成する

    　configureをつかってビルドファイルを作成します。configureにオプションを与えることで、インストール先のディレクトリや有効にするモジュールを調整できます。
    指定できるオプションについては`configure --help`や[Documentation](http://httpd.apache.org/docs/2.4/en/programs/configure.html)を参照してください。
    ビルドの要件を満たしていない場合、configureの処理は中断されます。

    　例えば、インストール先のディレクトリを /usr/local/httpd-2.4.6 として、
    [DSOサポート][4]を有効、[MPM][5]を[prefork][6]と明示して、
    新たに取得したAPRを使用する場合は次のようにconfigureを実行します。

        [root@localhost httpd-2.4.6]# ./configure --prefix=/usr/local/httpd-2.4.6 --enable-so --with-mpm=prefork --with-included-apr

    　次に、configureのエラー例をいくつか示します。

    * #### APR not found.

          checking for APR... no
          configure: error: APR not found.  Please read the documentation.

      　APRが正しくインストールされているか確認してください。

    * #### no acceptable C compiler found in $PATH

          checking for gcc... no
          checking for cc... no
          checking for cl.exe... no
          configure: error: in `/usr/local/src/httpd-2.4.6/srclib/apr':
          configure: error: no acceptable C compiler found in $PATH

      　Cコンパイラが正しくインストールされているか確認してください。

    * #### pcre-config for libpcre not found.

          checking for pcre-config... false
          configure: error: pcre-config for libpcre not found. PCRE is required and available from http://pcre.org/

      　PCREライブラリが正しくインストールされているか確認してください。

    　configureの処理が終了してMakefileが作成されていれば、ビルドの段階へすすみます。

 4. ### ビルドとインストール

    　ビルドとインストールにはmakeを使います。ディスクの容量が充分足りていることを確認してください。

        [root@localhost httpd-2.4.6]# make && make install

    　makeの処理が無事に終了したらインストールは完了です。
    configureの`--prefix`に指定した位置にファイルが展開されているはずです。PREFIXを指定していなかった場合は"/usr/local/apache2"になります。

        [root@localhost httpd-2.4.6]# /usr/local/httpd-2.4.6/bin/httpd -v
        Server version: Apache/2.4.6 (Unix)

## インストール後

　必要に応じて、httpd.confの設定、起動スクリプトの設置、自動起動の設定、実行バイナリへのパスの設定を行なってください。
ソースコードのアーカイブには、標準のLSB起動スクリプトが用意されています。

    [root@localhost httpd-2.4.6]# cp /usr/local/src/httpd-2.4.6/build/rpm/httpd.init /etc/init.d/httpd

### 動作を確認する

　最後に、ウェブサーバの動作を少し確認してみます。
ウェブサーバにとって外部のホストから動作を確認する場合はファイアウォール等の制限を受けやすいので、まずはインストールを行ったマシンからループバックで確認するとネットワークアクセスの問題と区別できます。

　FirefoxやLynxなどのブラウザや、GNU WgetやcURLをつかって、自身に対してHTTPリクエストを送信します（Wget、cURLを使う場合、HTMLの描写はされません）。
うまく動作していればドキュメントルートに予め用意されているHTMLファイルを応答するはずです。

    [root@localhost ~]# lynx localhost

{% img /blog/images/2013-07-31-guide-to-compiling-and-installing-apache-http-server-2-dot-4-6/apache-http-server-works.png LynxでApache HTTP Server 2.4.6の動作を確認する %}

　ネットワークアクセスを介すると動作が確認できなくなる場合、様々な原因が考えられますが、GNU/LinuxのシステムでよくあるのはiptablesやSELinuxなどのセキュリティによるものです。
外部からのアクセスを受け入れようとする場合はHTTPのネットワーク通信を許可する必要がありますが、これらの作業は実質、セキュリティ強度を緩和しているので、マシンが接続しているネットワークや記録しているコンテンツをよく確かめて設定するべきです。
Apache HTTP Serverの領域から少し外れますが、iptablesとSELinuxの設定の確認方法を示します。

* 　iptablesを使用している場合、パケットフィルタのルールを確認します。
  例えばCentOS 6.4の初期状態では下記のように、ポート80の受信の許可を明示していないため、外部のクライアントからのリクエストに応答できません。

      [root@localhost ~]# iptables --list-rules
      -P INPUT ACCEPT
      -P FORWARD ACCEPT
      -P OUTPUT ACCEPT
      -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
      -A INPUT -p icmp -j ACCEPT
      -A INPUT -i lo -j ACCEPT
      -A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT
      -A INPUT -j REJECT --reject-with icmp-host-prohibited
      -A FORWARD -j REJECT --reject-with icmp-host-prohibited

  　上記のようなルールの場合、`-A INPUT -j REJECT --reject-with icmp-host-prohibited`の上に、`-A INPUT -p tcp -m tcp --dport 80 -j ACCEPT`というルールを新しく追加する等の変更が必要になります。
  変更の方法も様々ですが、CentOS 6.4であれば/etc/sysconfig/iptablesを書き換えた後にiptablesを再起動するとルールが再設定されます。

* 　SELinuxを使用している場合、`httpd_can_network_connect`の値を確認します。

      [root@localhost ~]# getsebool httpd_can_network_connect
      httpd_can_network_connect --> off

  　ネットワークを通してウェブサーバを使用する場合は、この値にonを設定します。

      [root@localhost ~]# setsebool -P httpd_can_network_connect on

  　`httpd_can_network_connect`の他、CGIやDBを制限する項目もあるので、用途に応じて適当なポリシーを設定する必要があります。

### 参考

- [Apache HTTP Server Version 2.4 Documentation - Apache HTTP Server][10]
- [The Apache Portable Runtime Project][7]

### 関連記事

- [MySQL Community Server 5.6.13をインストールする（ソースからビルドする方法）](/blog/2013/07/31/guide-to-compiling-and-installing-mysql-community-server-5-dot-6-13/)
- [PHP 5.5.1をインストールする（ソースからビルドする方法）](/blog/2013/07/31/guide-to-compiling-and-installing-php-5-dot-5-1/)
- [CentOS 6.4をインストールする](/blog/2013/04/06/install-centos-6-dot-4-i386-netinstall/)

 [1]: http://httpd.apache.org/download.cgi#apache24 "Download - The Apache HTTP Server Project"
 [2]: http://ftp.riken.jp/net/apache/ "Software archives at ftp.riken.jp" 
 [3]: http://www.pathname.com/fhs/pub/fhs-2.3.html#USRLOCALLOCALHIERARCHY "Filesystem Hierarchy Standard 2.3"
 [4]: http://httpd.apache.org/docs/2.4/en/dso.html "Dynamic Shared Object (DSO) Support - Apache HTTP Server"
 [5]: http://httpd.apache.org/docs/2.4/en/mpm.html "Multi-Processing Modules (MPMs) - Apache HTTP Server"
 [6]: http://httpd.apache.org/docs/2.4/en/mod/prefork.html "prefork - Apache HTTP Server"
 [7]: http://apr.apache.org/ "The Apache Portable Runtime Project" 
 [8]: http://httpd.apache.org/docs/2.4/en/install.html "Compiling and Installing - Apache HTTP Server"
 [9]: http://apr.apache.org/download.cgi "Download - The Apache Portable Runtime Project"
[10]: http://httpd.apache.org/docs/2.4/ "Apache HTTP Server Version 2.4 Documentation - Apache HTTP Server"
[11]: http://gcc.gnu.org/ "GCC, the GNU Compiler Collection - GNU Project - Free Software Foundation (FSF)"
[12]: http://www.pcre.org/ "PCRE - Perl Compatible Regular Expressions"
[13]: http://www.atmarkit.co.jp/ait/articles/1106/24/news112.html "仕事で使える魔法のLAMP（11）：配布パッケージの中身と、configureの役目を知る - ＠IT"
[14]: http://rat.cis.k.hosei.ac.jp/article/rat/linuxliteracy/2005/network.html "Linux リテラシ - 第5回 ネットワーク"
[15]: http://www.atmarkit.co.jp/fsecurity/rensai/selinux205/selinux02.html "トラブルシューティングはCentOS 5におまかせ － ＠IT"
