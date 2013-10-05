---
layout: post
title: "Backup and Restore MySQL"
date: 2013-02-24 22:45
comments: true
categories: 
keywords:
---

- mysqldump -u `ユーザー名` -p -D `データベース名` > `出力先ファイル名`
- mysql -u `ユーザー名` -p `データベース名` < `ダンプファイル名`

: -w, --where='where-condition' | 選択したレコード
: -X, --xml | XMLとして

: -n, --no-create-db | CREATE DATABASE /*!32312 IF NOT EXISTS*/ db_name; が出力に含まれない。--databases オプションまたは --all-databases オプションを指定した場合は、上記の行が追加される。
: -t, --no-create-info | テーブル作成情報（CREATE TABLE ステートメント）を書き込まない。
: -d, --no-data | テーブルのレコード情報を一切書き込まない。テーブルの構造だけをダンプする場合、非常に便利である。

- テーブルをダンプするには、SELECT * INTO OUTFILE 'file_name' FROM tbl_name を使用する。
- テーブルをリロードするには、LOAD DATA INFILE 'file_name' REPLACE ... を使用する。

 $ mysqldump -u control -p __DATABASE __TABLE \
 > "--where=1='1'" --no-create-info > __DATABASE.__TABLE.dat.sql


 -- MySQL dump 9.11
 --
 -- Host: localhost    Database: ...
 -- ------------------------------------------------------
 -- Server version       4.0.27-log
 
 --
 -- Dumping data for table `...`
 --
 -- WHERE:  1=1
 
 INSERT INTO ... VALUES (...);

