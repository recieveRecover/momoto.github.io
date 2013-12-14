---
layout: post
title: "Arch Linux extra/emacs 24.2-4で日本語を入力する"
date: 2013-02-24 14:44
comments: true
categories: [GNU/Linux]
keywords: Emacs, 日本語, Mozc
description: "Arch LinuxのEmacsで日本語を入力するための設定の記録です。"

---

　Arch LinuxのEmacsで日本語を入力するための設定の記録です。この方法では、Emacsの他、UIM等の入力メソッドフレームワークと、AnthyやMozc等の入力メソッドがインストールされている必要があります。

{% img /blog/images/2013-02-24-input-japanese-on-emacs-24-dot-2-4/01.png Mozcを使用して、Emacsで日本語を入力する %}

<!-- more -->

###### UIMとAnthy (UTF-8)を使用する場合
    extra/anthy 9100h-3
        Hiragana text to Kana Kanji mixed text Japanese input method
    extra/uim 1.8.4-2
        Multilingual input method library
上記のパッケージをインストールした状態で、下記の設定をEmacs設定用ファイルに追記します。
    ;; uim /usr/share/emacs/site-lisp/uim-el/uim-leim.el
    (require 'uim-leim)
    (setq default-input-method "japanese-anthy-utf8-uim") ; Anthy (UTF-8)
    ;(setq default-input-method "japanese-mozc-uim")      ; Mozc

###### Mozcを使用する場合に必要
    pnsft-pur/emacs-mozc 1.6.1187.102-4 (mozc-im)
        Mozc for Emacs
    pnsft-pur/ibus-mozc 1.6.1187.102-4 (mozc-im)
        IBus engine module for Mozc
    pnsft-pur/mozc 1.6.1187.102-4 (mozc-im)
        A Japanese Input Method for Chromium OS, Windows, Mac and Linux (the Open Source Edition of Google Japanese Input)
上記のパッケージをインストールした状態で、下記の設定をEmacs設定用ファイルに追記します。
    ;; emacs-mozc /usr/share/emacs/site-lisp/emacs-mozc/mozc.el
    (require 'mozc)
    (setq default-input-method "japanese-mozc")
    (setq mozc-candidate-style 'overlay) ; Mozc overlayモード

###### 参考
- [Input Japanese using uim (日本語) - ArchWiki](https://wiki.archlinux.org/index.php/Input_Japanese_using_uim_%28%E6%97%A5%E6%9C%AC%E8%AA%9E%29#Emacs_.E3.81.A7_.E6.97.A5.E6.9C.AC.E8.AA.9E.E3.82.92.E5.85.A5.E5.8A.9B.E3.81.99.E3.82.8B)
- [Mozc (日本語) - ArchWiki](https://wiki.archlinux.org/index.php/Mozc_%28%E6%97%A5%E6%9C%AC%E8%AA%9E%29)

<iframe src="http://rcm-jp.amazon.co.jp/e/cm?lt1=_top&bc1=000000&IS2=1&npa=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=alqet049-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=487311277X" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
<iframe src="http://rcm-jp.amazon.co.jp/e/cm?lt1=_top&bc1=000000&IS2=1&npa=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=alqet049-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4774150029" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
