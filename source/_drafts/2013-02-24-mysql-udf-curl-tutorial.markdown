---
layout: post
title: "mysql-udf-curl Tutorial"
date: 2013-02-24 22:39
comments: true
categories: 
keywords:
---

** mysql-udf-curl [#de165175]
*** 追加される関数 [#w4cc70e4]
- curl_fetch(URL, [リクエストメソッド, [リクエスト内容] ])
-- URLで指定された場所にアクセスし、内容を文字列として返す
- curl_esc(文字列)
-- 指定の文字列に含まれるURL特殊文字 ("&" など) をエスケープする
- curl_setopt(オプション名, 文字列)
-- リクエストにあたって、オプションを設定する。オプション名に指定できるのは USERAGENT, PROXY, PROXYUSERPWD, PROXYUSER, PROXYPASSWORD, INTERFACE, USERPWD, USERNAME, PASSWORD, HTTPAUTH
*** インストール方法 [#wc0aa122]
+ autoconf、automake、libtool、gitコマンドとcURLライブラリをインストールします。(パッケージ名はautoconf, automake, libtool, git-core, libcurl3-dev)
+ http://github.com/moriyoshi/mysql-udf-curl.gitよりgitコマンドでソースコードを取得します。
+ ソースコードが格納されたディレクトリに移動します。
+ autogen.sh を実行します。
+ configure を実行します。必要があれば、--with-mysql オプションを与え、mysql のインストールプレフィクスを指定します。
+ make を実行します。
+ make install を実行します。
*** UDFの定義((User Defined Functionの定義しないと追加関数つかえません)) [#c1f5ab0b]
+ CREATE FUNCTION curl_fetch RETURNS STRING SONAME 'curl.so';
+ CREATE FUNCTION curl_esc RETURNS STRING SONAME 'curl.so';
+ CREATE FUNCTION curl_setopt RETURNS INTEGER SONAME 'curl.so';
** mysql-udf-libxml2 [#md35609d]
*** 追加される関数 [#vd787254]
- xml_parse(XMLまたはHTML, [HTMLフラグ, [エンコーディング, [オプション] ] ])
-- XMLまたはHTMLをパースして、その結果の要素ツリーをメモリーに格納し、結果を表すハンドルを返す。HTMLフラグが1の場合、与えられた文字列がHTML文章であるとみなす
- xml_select(ハンドル, XPath)
-- ハンドルで指定された要素ツリーにおいて、XPathを評価し、その結果を文字列化したものを返す
- xml_free(ハンドル)
-- ハンドルで指定された要素ツリーをメモリーから削除する
*** インストール方法 [#b76fd9d4]
+ autoconf、automake、libtool、gitコマンドとlibxml2ライブラリをインストールします。(パッケージ名はautoconf, automake, libtool, git-core, libxml2-dev)
+ http://github.com/moriyoshi/mysql-udf-libxml2.gitよりgitコマンドでソースコードを取得します。
+ MySQLのインクルードファイルがあるディレクトリ (${prefix}/include/mysql) に、MySQLのソースコードから、my_tree.h と my_base.h をコピーします。
+ ソースコードが格納されたディレクトリに移動します。
+ autogen.sh を実行します。
+ configure を実行します。必要があれば、--with-mysql オプションを与え、mysql のインストールプレフィクスを指定します。
+ make を実行します。
+ make install を実行します
*** UDFの定義 [#o0506536]
+ CREATE FUNCTION xml_parse RETURNS INTEGER SONAME 'xml-libxml2.so';
+ CREATE FUNCTION xml_select RETURNS STRING SONAME 'xml-libxml2.so';
+ CREATE FUNCTION xml_free RETURNS INTEGER SONAME 'xml-libxml2.so';

