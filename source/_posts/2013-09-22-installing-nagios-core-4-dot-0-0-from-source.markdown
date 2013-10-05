---
layout: post
title: "Nagios Core 4.0.0をソースからインストールする"
date: 2013-09-22 22:32
comments: true
categories: [ネットワーク監視]
keywords: ネットワーク監視, Nagios Core 4.0.0, Nagios Plugins 1.4.16
description: "Nagios Core 4.0.0とNagios Plugins 1.4.16をソースからインストールします。OSはCentOS 6.4を使用しています。"
breadcrumb:
  - title: Blog
    url: /
  - title: ネットワーク監視
    url: /blog/categories/netutowakujian-shi/

---

　Nagios Core 4.0.0とNagios Plugins 1.4.16をソースからインストールします。OSはCentOS 6.4を使用しています。<!-- more -->

 1. #### ソースコードを取得する

    　[www.nagios.org](http://www.nagios.org/download)からNagios Core 4.0.0とNagios Plugins 1.4.16のソースコードを取得します。

        $ cd /usr/local/src/
        $ sudo wget http://prdownloads.sourceforge.net/sourceforge/nagios/nagios-4.0.0.tar.gz
        $ sudo wget http://prdownloads.sourceforge.net/sourceforge/nagiosplug/nagios-plugins-1.4.16.tar.gz
        $ sudo tar xfz nagios-4.0.0.tar.gz
        $ sudo tar xfz nagios-plugins-1.4.16.tar.gz

 2. #### 要件を満たす

    　[Documentation](http://assets.nagios.com/downloads/nagioscore/docs/Installing_Nagios_Core_From_Source.pdf)に従って、必要なパッケージをインストールします。

        # RHEL/CentOSの場合
        $ sudo yum install wget httpd php gcc glibc glibc-common gd gd-devel make net-snmp
        
        # Ubuntuの場合
        $ sudo apt-get install wget build-essential apache2 php5-gd libgd2-xpm libgd2-xpm-dev libapache2-mod-php5

    　続いて、Nagiosのプロセスを実行するユーザとグループを用意します。

        $ sudo useradd nagios
        $ sudo groupadd nagcmd
        $ sudo usermod -a -G nagcmd nagios

 3. #### Nagios Coreをインストールする

    　ソースコードを展開したディレクトリに移ってNagios Coreをインストールしていきます。

        # cd nagios

        # RHEL/CentOSの場合
        # ./configure --with-command-group=nagcmd

        # Ubuntuの場合
        # ./configure --with-nagios-group=nagios --with-command-group=nagcmd -–with-mail=/usr/bin/sendmail

        *** Configuration summary for nagios 4.0.0 09-20-2013 ***:

         General Options:
         -------------------------
                Nagios executable:  nagios
                Nagios user/group:  nagios,nagios
               Command user/group:  nagios,nagcmd
                     Event Broker:  yes
                Install ${prefix}:  /usr/local/nagios
            Install ${includedir}:  /usr/local/nagios/include/nagios
                        Lock file:  ${prefix}/var/nagios.lock
           Check result directory:  ${prefix}/var/spool/checkresults
                   Init directory:  /etc/rc.d/init.d
          Apache conf.d directory:  /etc/httpd/conf.d
                     Mail program:  /bin/mail
                          Host OS:  linux-gnu

         Web Interface Options:
         ------------------------
                         HTML URL:  http://localhost/nagios/
                          CGI URL:  http://localhost/nagios/cgi-bin/
         Traceroute (used by WAP):

    　configureが終わると、Nagiosの構成の要約を表示してくれます。

        # make all

    　コンパイルまで済んだら、次のmakeオプションでインストールを続けていきます。

    - `make install` メインプログラム、CGI、HTMLファイルをインストール
    - `make install-init` /etc/rc.d/init.dの起動スクリプトをインストール
    - `make install-commandmode` ディレクトリの権限のインストールと設定
    - `make install-config` /usr/local/nagios/etcの設定ファイルのサンプルをインストール
    - `make install-webconf` ウェブインタフェースのためのApache設定ファイルをインストール
    - `make install-exfoliation` ウェブインタフェースのExfoliationテーマをインストール    
    - `make install-classicui` ウェブインタフェースのクラシックテーマをインストール

    　ウェブインタフェースのテーマは必要なければ省略できるようです。

        # make install
        # make install-init
        # make install-config
        # make install-commandmode
        # make install-webconf
        # cp -R contrib/eventhandlers/ /usr/local/nagios/libexec/
        # chown -R nagios:nagios /usr/local/nagios/libexec/eventhandlers/

 4. #### Nagios Pluginをインストールする

    　ソースコードを展開したディレクトリに移ってNagios Pluginをインストールしていきます。

        # cd ../nagios-plugins-1.4.16
        # ./configure --with-nagios-user=nagios --with-nagios-group=nagios
        # make
        # make install

 5. #### NagiosとApacheを起動する

    　ウェブインタフェースの認証につかうApacheのパスワードファイルを用意します。

        # htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
        New password:
        Re-type new password:
        Adding password for user nagiosadmin

    　Nagiosの設定ファイルを検証した後、Nagios、Apacheを起動します。

        # /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
        # service nagios start
        nagios is stopped
        Starting nagios:                                           [  OK  ]
        # service httpd start
                                                                   [  OK  ]

    　Nagiosのウェブインタフェースへアクセスして動作を確かめてみます。認証には上のhtpasswdで設定したユーザ名とパスワードを入力します。

    {% img /blog/images/2013-09-22-installing-nagios-core-4-dot-0-0-from-source/01-nagios-access.png 500 Nagios Access %}

    {% img /blog/images/2013-09-22-installing-nagios-core-4-dot-0-0-from-source/02-nagios-web-interface.png 500 Nagiosのウェブインタフェース %}

 6. #### 自動起動を設定する

    　必要に応じて、Nagiosの自動起動を設定します。

        # chkconfig --add nagios
        # chkconfig nagios on
        # chkconfig httpd on

## 参考

- [Nagios – Installing Nagios Core From Source](http://assets.nagios.com/downloads/nagioscore/docs/Installing_Nagios_Core_From_Source.pdf)
