---
layout: post
title: "Stored Procedure Tutorial"
date: 2013-02-24 22:42
comments: true
categories: 
keywords:
---

** とりあえず体験してみよう
+ vi procedure.sql
 DELIMITER //
 CREATE PROCEDURE test()
 BEGIN
   SHOW FULL PROCESSLIST;
 END //
 DELIMITER ;
+ source `procedure.sql`
-- databaseにプロシージャを読み込ませる
-- databaseを選んでいないと読み込めない
-- 必要ならファイルパスを指定する
+ SHOW PROCEDURE STATUS;
-- 読み込んだプロシージャを確認
+ CALL test()
-- プロシージャに記述したSQLが発動
+ DROP PROCEDURE `database`.`procedure name`;
-- databaseに読み込ませたプロシージャを消す



** 最大の特徴は、制御構文・例外処理も記述できること

