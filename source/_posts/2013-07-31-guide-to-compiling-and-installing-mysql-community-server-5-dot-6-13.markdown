---
layout: post
title: "MySQL Community Server 5.6.13をインストールする（ソースからビルドする方法）"
date: 2013-07-31 22:15
comments: true
categories: [MySQL]
keywords: MySQL Community Server 5.6.13, mysqld, cmake, curses, mysql_install_db, mysql_secure_installation
description: "MySQL Community Server 5.6.13をソースコードからビルドして、Unix系システムへインストールする手引きです。"

---

　MySQL Community Server 5.6.13をソースコードからビルドして、Unix系システムへインストールする手引きです。
この記事ではCentOS 6.4環境でインストールの例を示しますが、自身でソースからビルドする方法は特定のLinuxディストリビューションに依りません。

<!-- more -->

　インストールを行うユーザは、インストール先ディレクトリに対して書込み権限をもっている必要があります。/usr/local/* にインストールする場合、通常、root権限が必要です。

 1. ### ソースコードを取得する

    　ソースコードをインストールするマシンに用意します。
    インターネットからダウンロードする場合、[dev.mysql.com](http://dev.mysql.com/)にソースコードのURLが示されています。
    [MySQL Downloads](http://dev.mysql.com/downloads/)から[Download MySQL Community Server](http://dev.mysql.com/downloads/mysql/)へとすすみ、Select Platform... では Source Code を選択して、[Generic Linux (Architecture Independent), Compressed TAR Archive (mysql-5.6.13.tar.gz)](http://dev.mysql.com/downloads/mirror.php?id=413981)をダウンロードします。ダウンロードの際、Oracleウェブアカウントの登録を促されますが、インストールに必要なものではありません。

    　ダウンロードにはcURLやGNU Wgetなどのダウンロードマネージャやウェブブラウザを使用します。
    次の例では、cURLをつかってmysql-5.6.13.tar.gzをダウンロードします。

        [root@localhost ~]# curl -Lso mysql-5.6.13.tar.gz http://dev.mysql.com/get/Downloads/MySQL-5.6/mysql-5.6.13.tar.gz/from/http://cdn.mysql.com/

    　取得したアーカイブファイルは適宜、解凍・展開して、ワーキングディレクトリを移動させます。
    [FHS](http://www.pathname.com/fhs/pub/fhs-2.3.html#USRLOCALLOCALHIERARCHY "Filesystem Hierarchy Standard 2.3")に従うシステムであれば、独自にインストールするソフトウェアのためのディレクトリ /usr/local が予め用意されていますので、この記事ではソースコードを /usr/local/src に配置してインストールをすすめます。

        [root@localhost ~]# tar xfz mysql-5.6.13.tzr.gz -C /usr/local/src/
        [root@localhost ~]# cd /usr/local/src/mysql-5.6.13
        [root@localhost mysql-5.6.13]#

 2. ### インストール要件を満たす

    　マシンがインストールの要件を満たしている必要があります。
    要件については[Reference Manual](http://dev.mysql.com/doc/refman/5.6/en/source-installation.html)に詳しい説明があります。

    - C++コンパイラ

          [root@localhost ~]# yum install gcc-c++

    - Make

          [root@localhost ~]# yum install make

    - CMake - [www.cmake.org](http://www.cmake.org/cmake/resources/software.html)や各種ディストリビューションで配布されています。

          [root@localhost ~]# yum install cmake

    - Cursesライブラリ

          [root@localhost ~]# yum install ncurses-devel

    - Perl

          [root@localhost ~]# yum install perl

    　インストール要件ではありませんが、mysqlサーバのためのグループとユーザを用意しておきます。UIDとGIDは[Red Hat Enterprise Linux 4 Reference Guide 6.2. 標準ユーザ](https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/4/html/Reference_Guide/s1-users-groups-standard-users.html)に従って27としています。

        [root@localhost ~]# groupadd -g 27 mysql
        [root@localhost ~]# useradd -u 27 -r -g 27 mysql

 3. ### ビルドファイルを作成する

    　cmakeをつかってビルドファイルを作成します。cmakeにオプションを与えることで、インストール先のディレクトリなどを調整することができます。
    指定できるオプションについては`cmake . -LH`や[Reference Manual](http://dev.mysql.com/doc/refman/5.6/en/source-configuration-options.html)を参照してください。
    ビルドの要件を満たしていない場合、cmakeの処理は中断されます。

    　例えば、インストール先のディレクトリを /usr/local/mysql-5.6.13 として、
    デフォルトの文字セットをUTF-8、
    照合順序をutf8_general_ciと指定する場合は次のようにcmakeを実行します。

        [root@localhost mysql-5.6.13]# cmake . -DCMAKE_INSTALL_PREFIX=/usr/local/mysql-5.6.13 -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci

    　次に、cmakeのエラー例をいくつか示します。

    * #### "CMAKE_CXX_COMPILER-NOTFOUND" was not found.

          CMake Error: your CXX compiler: "CMAKE_CXX_COMPILER-NOTFOUND" was not found.   Please set CMAKE_CXX_COMPILER to a valid compiler path or name.

      　C++コンパイラが正しくインストールされているか確認してください。

    * #### Curses library not found.

          -- Could NOT find Curses  (missing:  CURSES_LIBRARY CURSES_INCLUDE_PATH)
          CMake Error at cmake/readline.cmake:85 (MESSAGE):
            Curses library not found.  Please install appropriate package,

                remove CMakeCache.txt and rerun cmake.On Debian/Ubuntu, package name is libncurses5-dev, on Redhat and derivates it is ncurses-devel.

      　Cursesライブラリが正しくインストールされているか確認してください。

    　cmakeの処理が終了してMakefileが作成されていれば、ビルドの段階へすすみます。

 4. ### ビルドとインストール

    　ビルドとインストールにはmakeを使います。ディスクの容量が充分足りていることを確認してください。

        [root@localhost mysql-5.6.13]# make && make install

    　makeの処理が無事に終了したらインストールは完了です。
    cmakeの`-DCMAKE_INSTALL_PREFIX`に指定した位置にファイルが展開されているはずです。PREFIXを指定していなかった場合は"/usr/local/mysql"になります。

        [root@localhost mysql-5.6.13]# /usr/local/mysql-5.6.13/bin/mysqld --version
        /usr/local/mysql-5.6.13/bin/mysqld  Ver 5.6.13 for Linux on x86_64 (Source distribution)

## インストール後

　データディレクトリの初期化、MySQLユーザの設定などを行なっていきます。

- ### データディレクトリを初期化する（scripts/mysql_install_db）

  　`PREFIX/scripts/mysql_install_db`をつかってデータディレクトリを初期化します。
  Perlスクリプトであるため実行するにはPerlがインストールされている必要があります。
  オプションとしてMySQLサーバの実行ユーザ、MySQLをインストールしているディレクトリ、データディレクトリを指定できます。
  その他使用できるオプションは[Reference Manualの4.4.3](http://dev.mysql.com/doc/refman/5.6/en/mysql-install-db.html)を参照してください。

      [root@localhost mysql-5.6.13]# cd /usr/local/mysql-5.6.13/
      [root@localhost mysql-5.6.13]# chown -R mysql:mysql .
      [root@localhost mysql-5.6.13]# ./scripts/mysql_install_db \
      > --user=mysql \
      > --basedir=/usr/local/mysql-5.6.13/ \
      > --datadir=/usr/local/mysql-5.6.13/data/

        Installing MySQL system tables...
        ...
        OK

        Filling help tables...
        ...
        OK

      [root@localhost mysql-5.6.13]# chown -R root:root .
      [root@localhost mysql-5.6.13]# chown -R mysql:mysql data/

  　データディレクトリやソケットファイルの格納ディレクトリなどが、実行ユーザにとって適当な権限設定になっていることを確認してください。
  Reference Manualには[Problems Running mysql_install_db](http://dev.mysql.com/doc/refman/5.6/en/mysql-install-db-problems.html)というページも用意されています。

  　続いて、`PREFIX/bin/mysqld_safe`をつかってMySQLサーバの起動を試します。

      [root@localhost mysql-5.6.13]# ./bin/mysqld_safe --user=mysql &
        
        130806 19:18:40 mysqld_safe Logging to '/usr/local/mysql-5.6.13/data/localhost.localdomain.err'.
        130806 19:18:40 mysqld_safe Starting mysqld daemon with databases from /usr/local/mysql-5.6.13/data

- ### rootパスワードの設定（bin/mysql_secure_installation）

  　`PREFIX/bin/mysql_secure_installation`をつかってrootパスワードを設定します。
  パスワード設定の他、匿名ユーザの削除やリモートからログインできるrootの削除などの操作を対話形式で指示できます。

      [root@localhost mysql-5.6.13]# ./bin/mysql_secure_installation

      NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MySQL
            SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

  　現時点のMySQL rootユーザのパスワードを入力して対話を開始します。
  初期化して直後の状態であれば、まだパスワードは設定されていないので、そのままEnterをおします。

      In order to log into MySQL to secure it, we'll need the current
      password for the root user.  If you've just installed MySQL, and
      you haven't set the root password yet, the password will be blank,
      so you should just press enter here.

      Enter current password for root (enter for none):
      OK, successfully used password, moving on...

  　MySQL rootユーザのパスワードを設定します。

      Setting the root password ensures that nobody can log into the MySQL
      root user without the proper authorisation.

      Set root password? [Y/n] y
      New password:
      Re-enter new password:
      Password updated successfully!
      Reloading privilege tables..
       ... Success!

  　匿名ユーザを削除するかどうかを選択します。

      By default, a MySQL installation has an anonymous user, allowing anyone
      to log into MySQL without having to have a user account created for
      them.  This is intended only for testing, and to make the installation
      go a bit smoother.  You should remove them before moving into a
      production environment.

      Remove anonymous users? [Y/n] y
       ... Success!

  　外部のホストからrootへのログインを禁止するかどうかを選択します。

      Normally, root should only be allowed to connect from 'localhost'.  This
      ensures that someone cannot guess at the root password from the network.

      Disallow root login remotely? [Y/n] y
       ... Success!

  　初期化の時点で作成されたtestデータベースを削除するかどうかを選択します。通常、使用しないデータベースです。

      By default, MySQL comes with a database named 'test' that anyone can
      access.  This is also intended only for testing, and should be removed
      before moving into a production environment.

      Remove test database and access to it? [Y/n] y
       - Dropping test database...
        ... Success!
         - Removing privileges on test database...
          ... Success!

  　ここまでのユーザ権限に対する変更を直ちに適用するかどうかを選択します。

      Reloading the privilege tables will ensure that all changes made so far
      will take effect immediately.

      Reload privilege tables now? [Y/n] y
       ... Success!

      All done!  If you've completed all of the above steps, your MySQL
      installation should now be secure.

      Thanks for using MySQL!

      Cleaning up...

　そのほか、必要に応じて、my.cnfの設定、起動スクリプトの設置、自動起動の設定、実行バイナリへのパスの設定を行なってください。
ソースコードのアーカイブには、標準のLSB起動スクリプトが用意されています。

      [root@localhost mysql-5.6.13]# cp support-files/mysql.server /etc/init.d/mysqld

### 動作を確認する

  MySQLクライアントからMySQLサーバへアクセスして動作を確認してみます。

    [root@localhost mysql-5.6.13]# ./bin/mysql -u root -p -h localhost
    
      Enter password:
      Welcome to the MySQL monitor.  Commands end with ; or \g.
      Your MySQL connection id is 1
      Server version: 5.6.13 Source distribution

      Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.

      Oracle is a registered trademark of Oracle Corporation and/or its
      affiliates. Other names may be trademarks of their respective
      owners.

      Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

    mysql> SELECT version();
    
      +-----------+
      | version() |
      +-----------+
      | 5.6.13    |
      +-----------+
      1 row in set (0.00 sec)

### 参考

- [MySQL :: MySQL 5.6 Reference Manual](http://dev.mysql.com/doc/refman/5.6/en/index.html)

### 関連記事

- [Apache HTTP Server 2.4.6をインストールする（ソースからビルドする方法）](/blog/2013/07/31/guide-to-compiling-and-installing-apache-http-server-2-dot-4-6/)
- [PHP 5.5.1をインストールする（ソースからビルドする方法）](/blog/2013/07/31/guide-to-compiling-and-installing-php-5-dot-5-1/)
- [CentOS 6.4をインストールする](/blog/2013/04/06/install-centos-6-dot-4-i386-netinstall/)
