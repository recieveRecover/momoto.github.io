---
layout: post
title: "Add and Drop MySQL Users"
date: 2013-02-24 22:46
comments: true
categories: 
keywords:
---

** ユーザー管理
ユーザーの追加には、二通りの方法がある
-- GRANTステートメント（この方法が推奨されている
-- MySQL権限テーブルを直接操作

-- MySQL権限テーブル
--- user ユーザーのグローバルレベルの権限とパスワードを管理するテーブル
--- db ユーザーのデータベースレベルの権限を管理するテーブル
--- host dbテーブルにホスト名が指定されていない場合に適用される権限を管理するためのテーブル
--- tables_priv ユーザーのテーブルレベルの権限を管理するテーブル
--- columns_priv ユーザーのフィールドレベルの権限を管理するテーブル


** ユーザー権限の閲覧
 SHOW GRANTS [FOR user]

** 権限の追加
 GRANT ALL PRIVILEGES ON `desire_NAICHU`.* TO 'control'@'192.168.1.201'   

** ユーザーの追加
GRANT 権限 ON データベース名.テーブル名　TO ユーザ名 IDENTIFIED BY 'パスワード'

- 権限
-- all ''全ての権限を与える。''
-- usage ''全ての権限を与えない''
-- create ''テーブルを作成する権限を付与する''
-- alter ''テーブルを変更する権限を付与する''
-- drop ''テーブルを削除する権限を付与する''
-- index ''インデックスを作成/削除する権限を付与する''
-- Select,Update,Insert,Delete ''テーブル操作の権限を付与する''

** パスワード設定
SET PASSWORD FOR root=password('パスワード')

** ユーザーの権限の削除 [#o4086321]
REVOKE

** 参考
[[MySQLリファレンス:http://dev.mysql.com/doc/refman/4.1/ja/adding-users.html]]

