---
layout: post
title: "Apache HTTP ServerのSPDYサポートを有効化する"
date: 2013-09-08 01:00
comments: true
categories: [WWW, Apache]
keywords: Apache HTTP Server 2.2.15, httpd, mod_spdy
description: "mod_spdyをつかってApache HTTP ServerのSPDYサポートを有効化します。"
breadcrumb:
  - title: Blog
    url: /
  - title: WWW
    url: /blog/categories/www/

---

　mod_spdyをつかってApache HTTP ServerのSPDYサポートを有効化します。mod_spdyは[code.google.com](http://code.google.com/p/mod-spdy/)で配布されています。

<!-- more -->

#### mod_spdyのRPMパッケージ 64ビットをインストールする場合

    $ wget https://dl-ssl.google.com/dl/linux/direct/mod-spdy-beta_current_x86_64.rpm
    $ rpm -U mod-spdy-*.rpm

　at, httpd, mod_sslなどに依存しているようです。

#### mod-spdyが提供するファイル

    $ rpm -ql mod-spdy-beta
    /etc/cron.daily/mod-spdy
    /etc/httpd/conf.d/load_ssl_with_npn.conf
    /etc/httpd/conf.d/spdy.conf
    /usr/lib64/httpd/modules/mod_spdy.so
    /usr/lib64/httpd/modules/mod_ssl_with_npn.so

　設定ファイルはload_ssl_with_npn.confとspdy.confです。SSL/TLSが有効になっていること、spdy.confの`SpdyEnabled`がonになっていること等を確認したらApacheを再起動します。

    $ apachectl -M | grep -E "spdy|ssl"
     ssl_module (shared)
     spdy_module (shared)
    Syntax OK
    $ /etc/init.d/httpd restart
    Stopping httpd:                                            [  OK  ]
    Starting httpd:                                            [  OK  ]

#### 画像表示の比較

　同じドメインの画像ファイル20個をブラウザに表示する様子を簡単に比較してみます。
正確な比較ではありませんが、リクエストのタイムラインが変化している様子をブラウザの開発ツールからも確認できます。

- ##### 通常のHTTPS

  　左がFirefoxのネットワークモニタ、右がChrome DevToolsのネットワークパネルです。

  {% img left /blog/images/2013-09-08-enabling-spdy-for-apache-http-server/0101-ssl-with-firefox.png 500 %}
  {% img /blog/images/2013-09-08-enabling-spdy-for-apache-http-server/0102-ssl-with-chrome.png 500 %}

- ##### SPDYが有効

  　通常のHTTPSからSPDYを有効に切り替えて、同じコンテンツをリクエストした様子です。

  {% img left /blog/images/2013-09-08-enabling-spdy-for-apache-http-server/0201-spdy-with-firefox.png 500 %}
  {% img /blog/images/2013-09-08-enabling-spdy-for-apache-http-server/0202-spdy-with-chrome.png 500 %}

  　Firefoxでは画像ファイルのSending（リクエスト送信にかかる時間）がほとんどなくなってWaiting（イニシャルレスポンスを待つ時間）に入れ替わっています。
  Chromeでは画像ファイルのサイズが0Bになっています（code.google.comのChromiumプロジェクトにバグとして報告されているようです[Issue #154706](https://code.google.com/p/chromium/issues/detail?id=154706)）。

- ##### SPDYが有効（画像ファイルをサーバープッシュ）

  　画像ファイルをX-Associated-Contentでサーバープッシュしています。

  {% img left /blog/images/2013-09-08-enabling-spdy-for-apache-http-server/0301-server-push-with-firefox.png 500 %}
  {% img /blog/images/2013-09-08-enabling-spdy-for-apache-http-server/0302-server-push-with-chrome.png 500 %}

  　Chromeでは画像ファイルがCacheからの読み込みとなってタイムラインが大幅に短くなりました（キャッシュはちゃんと消してアクセスしたと思います・・・）。
  X-Associated-Contentを追加した代わりにHTMLファイルの読み込み時間は伸びています。

#### 参考

- [SPDY - SPDY &mdash; Google Developers](https://developers.google.com/speed/spdy/)
- [How to tune your server to serve pages over SPDY efficiently - Apache SPDY module - Google Project Hosting](http://code.google.com/p/mod-spdy/wiki/OptimizingForSpdy)
- [Web表示の高速化を実現するSPDYとHTTP/2.0の標準化 | 最新の技術動向 | IIJ](http://www.iij.ad.jp/company/development/tech/activities/spdy/)
- [ネットワークモニタ - 開発ツール | MDN](https://developer.mozilla.org/ja/docs/Tools/Network_Monitor)
- [Evaluating network performance - Chrome DevTools &mdash; Google Developers](https://developers.google.com/chrome-developer-tools/docs/network)

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_top&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=seijimomotobl-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4774150363" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_top&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=seijimomotobl-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=477415783X" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
