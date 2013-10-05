---
layout: post
title: "jQuery Plugin Tutorial"
date: 2013-02-24 23:25
comments: true
categories: 
keywords:
---

## 規則
### ファイル名
ファイル名はjquery.__PLUG_IN_NAME__.js
### ファンクション
追加するファンクションはjQueryオブジェクトに付け加える
### メソッド
追加するメソッドはjQuery.fnオブジェクトに付け加える
追加するメソッドはjQueryオブジェクトを返す
追加するメソッド内で一致する要素を繰り返し処理する場合はthis.each()を使用する

### 注意点
thisはjQueryオブジェクトへの参照する
プラグインコードでは「$」を使用せず「jQuery」を指定する
プラグインコードのスコープを独立させる

### 参考
[jQuery を扱う: 第 3 回 中級レベルの jQuery: 独自のプラグインを作成する](http://www.ibm.com/developerworks/jp/web/library/wa-aj-jquery6/)