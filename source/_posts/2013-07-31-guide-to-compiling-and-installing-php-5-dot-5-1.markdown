---
layout: post
title: "PHP 5.5.1をインストールする（ソースからビルドする方法）"
date: 2013-07-31 23:03
comments: true
categories: [PHP]
keywords: PHP 5.5.1, apxs, libxml, php.ini, php5_module, mysqli, pdo_mysql
description: "PHP 5.5.1をソースコードからビルドして、Unix系システムへインストールする手引きです。"

---

　PHP 5.5.1をソースコードからビルドして、Unix系システムへインストールする手引きです。
この記事ではCentOS 6.4環境でインストールの例を示しますが、自身でソースからビルドする方法は特定のLinuxディストリビューションに依りません。<!-- more -->
インストールを行うユーザは、インストール先ディレクトリに対して書込み権限をもっている必要があります。/usr/local/* にインストールする場合、通常、root権限が必要です。

 1. ### ソースコードを取得する

    　ソースコードをインストールするマシンに用意します。
    インターネットからダウンロードする場合、[php.net](http://www.php.net/downloads.php)にソースコードのURLが示されています。
    bzip2、gzip、xzのアーカイブが用意されていますので、マシンで解凍・展開できる形式を選択してください。

    　ダウンロードにはcURLやGNU Wgetなどのダウンロードマネージャやウェブブラウザを使用します。
    次の例では、cURLをつかってphp-5.5.1.tar.gzをダウンロードします。

        [root@localhost ~]# curl -Lso php-5.5.1.tar.gz http://jp2.php.net/get/php-5.5.1.tar.gz/from/jp1.php.net/mirror

    　取得したアーカイブファイルは適宜、解凍・展開して、ワーキングディレクトリを移動させます。
    [FHS](http://www.pathname.com/fhs/pub/fhs-2.3.html#USRLOCALLOCALHIERARCHY "Filesystem Hierarchy Standard 2.3")に従うシステムであれば、独自にインストールするソフトウェアのためのディレクトリ /usr/local が予め用意されていますので、この記事ではソースコードを /usr/local/src に配置してインストールをすすめます。

        [root@localhost ~]# tar xfz php-5.5.1.tar.gz -C /usr/local/src/
        [root@localhost ~]# cd /usr/local/src/php-5.5.1
        [root@localhost php-5.5.1]#

 2. ### インストール要件を満たす

    　マシンがインストールの要件を満たしている必要があります。
    また、必要な拡張モジュールがある場合は、そのモジュールの要件も満たす必要があります。

    - Cコンパイラ

          [root@localhost ~]# yum install gcc

    - Make

          [root@localhost ~]# yum install make

 3. ### ビルドファイルを作成する

    　configureをつかってビルドファイルを作成します。configureにオプションを与えることで、インストール先のディレクトリや有効にするモジュールを調整できます。
    指定できるオプションについては`configure --help`や[Manual](http://php.net/manual/ja/configure.about.php)を参照してください。
    ビルドの要件を満たしていない場合、configureの処理は中断されます。

    　例えば、インストール先のディレクトリを /usr/local/php-5.5.1 として、
    [Apacheモジュール](http://www.php.net/manual/ja/book.apache.php)、[MySQLiモジュール](http://www.php.net/manual/ja/book.mysqli.php)、
    [PDO MySQLモジュール](http://www.php.net/manual/ja/ref.pdo-mysql.php)、[OpenSSLモジュール](http://www.php.net/manual/ja/book.openssl.php)を有効にする場合は次のようにconfigureを実行します。

        [root@localhost php-5.5.1]# ./configure \
        --prefix=/usr/local/php-5.5.1 \
        --with-apxs2=/usr/local/httpd-2.4.6/bin/apxs \
        --with-mysql=mysqlnd \
        --with-mysqli=mysqlnd \
        --with-pdo-mysql=mysqlnd \
        --with-openssl

    　次に、configureのエラー例をいくつか示します。

    * #### xml2-config not found.

          checking for xml2-config path...
          configure: error: xml2-config not found. Please check your libxml2 installation.

      　[libxmlモジュール](http://www.php.net/manual/ja/book.libxml.php)（デフォルトで有効）の要件であるlibxmlが必要です。

          [root@localhost php-5.5.1]# yum install libxml2-devel

    * #### configure: error: Cannot find OpenSSL's <evp.h>

          configure: error: Cannot find OpenSSL's <evp.h>

      　[OpenSSLモジュール](http://www.php.net/manual/ja/book.openssl.php)の要件であるOpenSSLが必要です。

          [root@localhost php-5.5.1]# yum search install openssl-devel

    　configureの処理が終了してMakefileが作成されていれば、ビルドの段階へすすみます。

 4. ### ビルドとインストール

    　ビルドとインストールにはmakeを使います。ディスクの容量が充分足りていることを確認してください。

        [root@localhost php-5.5.1]# make && make install

    　makeの処理が無事に終了したらインストールは完了です。
    configureの`--prefix`に指定した位置にファイルが展開されているはずです。

        [root@localhost php-5.5.1]# /usr/local/php-5.5.1/bin/php -v
        PHP 5.5.1 (cli)
        Copyright (c) 1997-2013 The PHP Group
        Zend Engine v2.5.0, Copyright (c) 1998-2013 Zend Technologies

## インストール後

　必要に応じて、php.iniの設定、実行バイナリへのパスの設定を行なってください。
ソースコードのアーカイブには標準のphp.iniが、開発環境向けと製品向けの２通り用意されています。

    [root@localhost php-5.5.1]# cp /usr/local/src/php-5.5.1/php.ini-development /usr/local/php-5.5.1/lib/php.ini

　新しく設置したphp.iniが正常に認識されているかどうかは`--ini`オプションで出力される Loaded Configuration File などで確認できます。

    [root@localhost php-5.5.1]# ./bin/php --ini
    Configuration File (php.ini) Path: /usr/local/php-5.5.1/lib
    Loaded Configuration File:         /usr/local/php-5.5.1/lib/php.ini
    Scan for additional .ini files in: (none)
    Additional .ini files parsed:      (none)

　標準のままの状態ではタイムゾーン未設定の警告がでますが、date.timezoneディレクティブにタイムゾーンを設定するとこの警告は出なくなります。その他のディレクティブも用途にあわせて設定してください。

    PHP Warning:  Unknown: It is not safe to rely on the system's timezone settings. You are *required* to use the date.timezone setting or the date_default_timezone_set() function. In case you used any of those methods and you are still getting this warning, you most likely misspelled the timezone identifier. We selected the timezone 'UTC' for now, but please set date.timezone to select your timezone. in Unknown on line 0

    [Date]
    ; Defines the default timezone used by the date functions
    ; http://php.net/date.timezone
    date.timezone = Asia/Tokyo

### 動作を確認する

　最後に、PHP 5.5.1の動作を少し確認してみます。
ここではPHPの拡張モジュールが正常に機能しているかどうかを中心に確かめていきます。

  * #### apache2.x

    　Apacheの設定ファイル（httpd.confなど）に LoadModuleディレクティブを正しく記述していることを確認します。

        LoadModule php5_module        modules/libphp5.so

    　また、PHPファイルをApacheハンドラに指定するため、SetHandlerやAddHandlerなどのディレクティブも必要です。

        <FilesMatch \.php$>
            SetHandler application/x-httpd-php
        </FilesMatch>

    　libphp5.soは、configureの`--with-apxs2=FILE`オプションを指定していれば、PHPのインストール時に作成されているはずです。

        [root@localhost httpd-2.4.6]# file modules/libphp5.so
        modules/libphp5.so: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, not stripped

    　apachectlの`-M`オプションをつかって、読み込まれているモジュールを確認することもできます。

        [root@localhost httpd-2.4.6]# apachectl -M | grep php5_module
         php5_module (shared)

  * #### MySQLiモジュール

    　mysqliモジュールを確認します。
    `PHP Warning:  PHP Startup: Unable to load dynamic library 'php_mysqli.dll'`と出る場合はモジュールがうまく読み込まれていません。

        [root@localhost php-5.5.1]# php -m | grep mysqli
        mysqli

        [root@localhost php-5.5.1]# php -r "var_dump(class_exists('mysqli'));"
        bool(true)

  * #### PDO MySQLモジュール

    　pdo_mysqlモジュールを確認します。
    `PHP Warning:  PHP Startup: Unable to load dynamic library 'php_pdo_mysql.dll'`と出る場合はモジュールがうまく読み込まれていません。

        [root@localhost php-5.5.1]# php -m | grep -i pdo
        PDO
        pdo_mysql
        pdo_sqlite

        [root@localhost php-5.5.1]# php -r 'print_r(PDO::getAvailableDrivers());'
        Array
        (
            [0] => mysql
            [1] => sqlite
        )

### 参考

- [PHP: PHP マニュアル - Manual](http://www.php.net/manual/ja/)

### 関連記事

- [Apache HTTP Server 2.4.6をインストールする（ソースからビルドする方法）](/blog/2013/07/31/guide-to-compiling-and-installing-apache-http-server-2-dot-4-6/)
- [MySQL Community Server 5.6.13をインストールする（ソースからビルドする方法）](/blog/2013/07/31/guide-to-compiling-and-installing-mysql-community-server-5-dot-6-13/)
- [CentOS 6.4をインストールする](/blog/2013/04/06/install-centos-6-dot-4-i386-netinstall/)
