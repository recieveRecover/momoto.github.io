---
layout: post
title: "User Defined Function Tutorial"
date: 2013-02-24 22:41
comments: true
categories: 
keywords:
---

** User Defined Function
- ユーザーが定義する関数

+ 関数の処理をC言語で書いて''.so拡張子''のファイル名でコンパイル
+ CREATE FUNCTION {関数名} RETURNS STRING SONAME '{作成したファイル名}.so';
-- もちろんパスが通ってないと意味がない。MySQLの環境変数LD_LIBRARY_PATHに指定しているところならOK

