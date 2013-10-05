---
layout: post
title: "ウィンドウマネージャawesomeの基本操作"
date: 2013-03-27 21:13
comments: true
categories: [デスクトップ環境]
keywors: awesome, タイル型ウィンドウマネージャ
description: "タイル型ウィンドウマネージャawesomeの特徴とショートカットキーを簡単にまとめています。"

---

タイル型ウィンドウマネージャ「awesome」の特徴とショートカットキーを簡単にまとめています。

#### awesomeが提供しているレイアウトのスクリーンショット

- タイル配置（awful.layout.suit.tile）

  {% img /blog/images/2013-03-27-awesome-tutorials/01.png 300 awesomeのタイル配置 %}
  {% img /blog/images/2013-03-27-awesome-tutorials/02.png 300 awesomeのタイル配置 %}

<!-- more -->

- 均等配置（awful.layout.suit.fair）

  {% img /blog/images/2013-03-27-awesome-tutorials/03.png 300 awesomeの均等配置 %}
  {% img /blog/images/2013-03-27-awesome-tutorials/04.png 300 awesomeの均等配置 %}

- 螺旋配置（awful.layout.suit.spiral）

  {% img /blog/images/2013-03-27-awesome-tutorials/05.png 300 awesomeの螺旋配置 %}

レイアウトはここで挙げているもの以外にも複数の種類が用意されています。

#### ショートカットキー

- ウィンドウ操作
  - `modkey + j` フォーカスを次のウィンドウへ移動する
  - `modkey + k` フォーカスを前のウィンドウへ移動する
  - `modkey + Tab` フォーカスを直前に使用していたウィンドウへ移動する
  - `modkey + Shift + c` ウィンドウを閉じる
  - `modkey + f` ウィンドウをフルスクリーン化する
  - `modkey + m` ウィンドウを最大化させる
  - `modkey + n` ウィンドウを最小化させる
  - `modkey + Control + n` 最小化したウィンドウを復元する
  - レイアウト操作
    - `modkey + Control + space` ウィンドウをフローティングウィンドウにする（またはフロート解除）
    - `modkey + Control + Return` ウィンドウをマスターウィンドウにする（レイアウトの中で最も目立つ配置へ移動する）
    - `modkey + space` 次のレイアウトへ切り替える
    - `modkey + Shift + space` 前のレイアウトへ切り替える
    - `modkey + Shift + j` ウィンドウの配置を前のウィンドウと入れ替える
    - `modkey + Shift + k` ウィンドウの配置を次のウィンドウと入れ替える
    - `modkey + l` ウィンドウを右方向へ伸縮させる
    - `modkey + h` ウィンドウを左方向へ伸縮させる
- タグ操作
  - `modkey + Left` 前のタグへ移動
  - `modkey + Right` 次のタグへ移動
  - `modkey + [1-9]` 焦点を指定のタグへ移動する
  - `modkey + "Shift" + [1-9]` ウィンドウを指定のタグへ移動する
  - `modkey + Escape` 直前に使用していたタグへ移動
- ウインドウマネージャ機能
  - `modkey + Return` ターミナルエミュレータを起動する
  - `modkey + r` コマンド実行プロンプトを起動する
  - `modkey + x` Luaコード実行プロンプトを起動する
  - `modkey + w` awesomeのメニューを開く
  - `modkey + Control + r` awesomeを再起動する
  - `modkey + Shift + q` awesomeを終了する

#### 参考
- [Getting started - awesome](http://awesome.naquadah.org/wiki/Getting_started)
- [doc - awesome window manager](http://awesome.naquadah.org/doc/manpages/awesome.1.html)
