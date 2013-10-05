---
layout: post
title: "tips for apache's mod_rewrite"
date: 2013-02-24 22:27
comments: true
categories: 
keywords:
---


* mod_rewrite [#h742ae7c]
** RewriteEngine on [#h68870a7]
 <ifmodule mod_rewrite.c="">
     RewriteEngine on
 </ifmodule>

** RewriteRule [パターン] [置換対象] ([フラグ]) [#c4486b9c]

 RewriteRule ^/([0-9A-Za-z]+/)$ /index.php?param=$1 [last]

例えば http://www.example.net/test/ というURLであれば、~
http://www.example.net/index.php?param=test というアクセスになる。~
[last]は、「これ以上書き換えをしない」という意味。

** RewriteCond [テスト文字列] [条件パターン] [#k0271ba5]
RewriteRuleの前に記述することで以降のRewriteRuleは~
自身のパターンとRewriteCondのパターンを満たした場合のみ置換される。
 RewriteCond %{REQUEST_FILENAME} !-d
 RewriteCond %{REQUEST_FILENAME} !-f
 RewriteRule ^(.*)$ index.php?url=$1 [QSA,L]
リクエストされたURIがディレクトリでもなくて
かつ、ファイルでもないときに、RewriteRuleの置換が発動

** フラグ [#w18d431f]

*** [redirect|R=code] [#h2ce9879]
外部にリダイレクトする場合このフラグをつける。~
[R=code]とするとレスポンスコードを300～400の間で指定できる。デフォルト302。

*** [forbidden|F] [#u4e6805e]
アクセス禁止。レスポンスコード403を返す。

*** [gone|G] [#h67379d1]
消去済み(gone)。レスポンスコード410が返す。

*** [last|L] [#a07f2a82]
書き換え処理を中止して、それ以上の書き換えルールを適用しない。

*** [type|T=MIME-type] [#y54b7cb3]
MIME type を指定する。

*** [nocase|NC] [#k0df0514]
パターンについて大文字小文字を区別しない。

*** [qsappend|QSA] [#y080c434]
もともとGETクエリがあれば、それを残して置換

*** [noescape|NE] [#i8917261]
mod_rewrite が書き換え結果に対して通常行なわれる URL エスケープルールを適用しないようにする。エスケープされたURLを利用したい時に利用する。

*** [skip|S=num] [#ebddefc3]
次のnum番目の処理をスキップする。

*** [proxy|P] [#lfcbd5a1]
*** [next|N] [#cd209537]
*** [chain|C] [#pa2d9458]
*** [nosubreq|NS] [#sdd6e5d5]
*** [passthrough|PT] [#x1a77bed]
*** [env|E=VAR:VAL] [#s147b4cb]

