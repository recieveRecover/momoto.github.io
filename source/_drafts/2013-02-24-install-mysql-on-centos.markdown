---
layout: post
title: "Install MySQL on CentOS"
date: 2013-02-24 22:49
comments: true
categories: 
keywords:
---

** 5.5.16 インストール
*** 必要なパッケージ
- Cコンパイラ
 yum install gcc
 gcc-c++？
- makeとcmake
 yum install make cmake
- curses
 yum install ncurses-devel

*** ユーザーとグループの用意
 groupadd -g 27 mysql
 useradd -r -u 27 -g 27 -s /bin/false -m mysql


*** インストール
- ソースの取得
 curl --output mysql-5.5.16.tar.gz http://ftp.iij.ad.jp/pub/db/mysql/Downloads/MySQL-5.5/mysql-5.5.16.tar.gz

- コンパイル
 cmake . -DDEFAULT_CHARSET=utf8
 make && make instsall

- ./configure --prefix=/usr/local --with-charset=utf8 --with-extra-charsets=all

- cmakeをやりおすとき
 rm CMakeCache.txt
- makeをやりおなすとき
 make clean

*** その後
 chown -R mysql:mysql /usr/local/mysql
 /usr/local/mysql/scripts/mysql_install_db --user=mysql
 chown -R root:root /usr/local/mysql/
 chown -R mysql:mysql /usr/local/mysql/data/
 cp support-files/my-large.cnf /etc/my.cnf
 cp support-files/mysql.server /etc/init.d/mysql.server


- mysqlサーバーの起動
 /etc/init.d/mysql.server start
- mysqlクライアントの起動
 /usr/local/mysql/bin/mysql


: bin/mysql | MySQLサーバーへの接続
: bin/mysqladmin | rootのパスの設定など
: bin/mysql_secure_installation | 
: bin/mysqld_safe | 起動
: bin/mysql_install_db | 新しい権限テーブルの生成


** 設定
- /etc/my.cnf

 [client]
 default-character-set = utf8
 [mysqld]
 default-character-set = utf8
 skip-character-set-client-handshake
 [mysqldump]
 default-character-set = utf8
 [mysql]
 default-character-set = utf8

 mysql> show variables like "char%"; 
 +--------------------------+----------------------------+
 | Variable_name            | Value                      |
 +--------------------------+----------------------------+
 | character_set_client     | latin1                     | 
 | character_set_connection | latin1                     | 
 | character_set_database   | latin1                     | 
 | character_set_filesystem | binary                     | 
 | character_set_results    | latin1                     | 
 | character_set_server     | latin1                     | 
 | character_set_system     | utf8                       | 
 | character_sets_dir       | /usr/share/mysql/charsets/ | 
 +--------------------------+----------------------------+

