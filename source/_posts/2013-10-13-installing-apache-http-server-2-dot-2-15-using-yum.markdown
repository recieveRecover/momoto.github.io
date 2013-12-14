---
layout: post
title: "YumをつかってApache HTTP Server 2.2をインストールする"
date: 2013-10-13 11:29
comments: true
categories: [WWW, Apache]
keywords: Apache HTTP Server 2.2.15, httpd, GNU/Linux, CentOS 6, Yum
description: "パッケージ管理システムのYumをつかって、Apache HTTP Server 2.2.15をインストールします。OSはCentOS 6を使用しています。"
breadcrumb:
  - title: Blog
    url: /
  - title: WWW
    url: /blog/categories/www/

---

　パッケージ管理システムのYumをつかって、Apache HTTP Server 2.2.15をインストールします。OSはCentOS 6を使用しています。

<!-- more -->

　RHEL 6互換のCentOSの標準リポジトリではhttpdパッケージでApache HTTP Serverが提供されています。パッケージの情報は`yum info <パッケージ名>`で調べることができます。
パッケージ情報が示すとおり、httpdパッケージ（i686）からインストールできるApacheのバージョンは**2.2.15**になります。

    $ yum info httpd

    Available Packages
    Name        : httpd
    Arch        : i686
    Version     : 2.2.15
    Release     : 29.el6.centos
    Size        : 828 k
    Repo        : updates
    Summary     : Apache HTTP Server
    URL         : http://httpd.apache.org/
    License     : ASL 2.0
    Description : The Apache HTTP Server is a powerful, efficient, and extensible
                : web server.

　インストールには`yum install <パッケージ名>`を実行するだけで、パッケージ管理システムが依存関係を解決し、必要なパッケージと共にインストールが完了します。

    $ sudo yum install httpd
    Setting up Install Process

    ...
    
    Installed:
      httpd.i686 0:2.2.15-29.el6.centos

    Dependency Installed:
      apr.i686 0:1.3.9-5.el6_2                    apr-util.i686 0:1.3.9-3.el6_0.1
      apr-util-ldap.i686 0:1.3.9-3.el6_0.1        httpd-tools.i686 0:2.2.15-29.el6.centos
      mailcap.noarch 0:2.1.31-2.el6

    Complete!

　httpdパッケージが提供しているファイルは`rpm -ql httpd`で一覧を得られますが、出力は300行以上に及ぶためディレクトリごとにファイルの役割をみてみます。

    $ rpm -ql httpd | gawk -F/ '{print "/"$2"/"$3}' | uniq -c
         10 /etc/httpd
          1 /etc/logrotate.d
          2 /etc/rc.d
          2 /etc/sysconfig
         66 /usr/lib
          8 /usr/sbin
         12 /usr/share
          1 /var/cache
          1 /var/lib
          1 /var/log
          1 /var/run
        252 /var/www

<table class="table">
<caption>（表）httpdパッケージが提供するファイルやディレクトリの概要</caption>
<tbody>
<tr><td>/etc/httpd/</td><td>conf/httpd.confなど設定ファイルを格納しているディレクトリ（サーバルート）</td></tr>
<tr><td>/etc/logrotate.d/httpd</td><td>httpdのログローテーションの設定</td></tr>
<tr><td>/etc/rc.d/init.d/{htcacheclean, httpd}</td><td>httpdとhtcachecleanの起動スクリプト</td></tr>
<tr><td>/etc/sysconfig/{htcacheclean, httpd}</td><td>preforkかworkerの切り替え等、サービスの設定ファイル</td></tr>
<tr><td>/usr/lib/httpd/modules/mod_*.so</td><td>Apacheモジュールの共有ライブラリファイル</td></tr>
<tr><td>/usr/sbin/{apachectl, httpdなど}</td><td>Apache各種コマンドの実行ファイル</td></tr>
<tr><td>/usr/share/{doc/, man/}</td><td>Apacheのドキュメントやmanページ</td></tr>
<tr><td>/var/cache/mod_proxy/</td><td>mod_disk_cacheのCacheRootディレクトリ</td></tr>
<tr><td>/var/lib/dav/</td><td>mod_davのロックデータベースディレクトリ</td></tr>
<tr><td>/var/log/httpd/</td><td>httpdのログを格納するディレクトリ</td></tr>
<tr><td>/var/run/httpd/</td><td>httpdのプロセスIDファイルを格納するディレクトリ</td></tr>
<tr><td>/var/www/</td><td>HTMLファイル等のコンテンツを格納しているディレクトリ（/var/www/htmlがデフォルトのドキュメントルート）</td></tr>
</tbody>
</table>

　httpd-2.2.15-29.el6.centos.i686では、実行ファイルのhttpd（prefork）とhttpd.workerはそれぞれ次のようにコンパイルされています。

    $ /usr/sbin/httpd -V
    ...
    Server compiled with....
     -D APACHE_MPM_DIR="server/mpm/prefork"
     -D APR_HAS_SENDFILE
     -D APR_HAS_MMAP
     -D APR_HAVE_IPV6 (IPv4-mapped addresses enabled)
     -D APR_USE_SYSVSEM_SERIALIZE
     -D APR_USE_PTHREAD_SERIALIZE
     -D SINGLE_LISTEN_UNSERIALIZED_ACCEPT
     -D APR_HAS_OTHER_CHILD
     -D AP_HAVE_RELIABLE_PIPED_LOGS
     -D DYNAMIC_MODULE_LIMIT=128
     -D HTTPD_ROOT="/etc/httpd"
     -D SUEXEC_BIN="/usr/sbin/suexec"
     -D DEFAULT_PIDLOG="run/httpd.pid"
     -D DEFAULT_SCOREBOARD="logs/apache_runtime_status"
     -D DEFAULT_LOCKFILE="logs/accept.lock"
     -D DEFAULT_ERRORLOG="logs/error_log"
     -D AP_TYPES_CONFIG_FILE="conf/mime.types"
     -D SERVER_CONFIG_FILE="conf/httpd.conf"

    $ /usr/sbin/httpd.worker -V
    ...
    Server compiled with....
     -D APACHE_MPM_DIR="server/mpm/worker"
     -D APR_HAS_SENDFILE
     -D APR_HAS_MMAP
     -D APR_HAVE_IPV6 (IPv4-mapped addresses enabled)
     -D APR_USE_SYSVSEM_SERIALIZE
     -D APR_USE_PTHREAD_SERIALIZE
     -D SINGLE_LISTEN_UNSERIALIZED_ACCEPT
     -D APR_HAS_OTHER_CHILD
     -D AP_HAVE_RELIABLE_PIPED_LOGS
     -D DYNAMIC_MODULE_LIMIT=128
     -D HTTPD_ROOT="/etc/httpd"
     -D SUEXEC_BIN="/usr/sbin/suexec"
     -D DEFAULT_SCOREBOARD="logs/apache_runtime_status"
     -D DEFAULT_ERRORLOG="logs/error_log"
     -D AP_TYPES_CONFIG_FILE="conf/mime.types"
     -D SERVER_CONFIG_FILE="conf/httpd.conf"

　serviceユーティリティをつかってhttpdを起動させたあと、curlでウェブサーバの動作を確認してみます。

    $ sudo service httpd start
                                                               [  OK  ]
    $ curl -D - -s -o /dev/null localhost
    HTTP/1.1 403 Forbidden
    Date: Sun, 13 Oct 2013 09:08:07 GMT
    Server: Apache/2.2.15 (CentOS)
    Accept-Ranges: bytes
    Content-Length: 5039
    Connection: close
    Content-Type: text/html; charset=UTF-8
    

　デフォルトの設定のままではDocumentRoot（/var/www/html）のDirectoryIndex（index.html）が見つからず、/etc/httpd/conf.d/welcome.confでは`Options -Indexes`が設定されているため、
HTTPステータスコードは`403 Forbidden`、メッセージボディには/var/www/error/noindex.htmlを応答しています。

　/var/www/html/index.htmlを作成したあとに再度リクエストしてみると`200 OK`を応答するようになります。

    $ echo '<html><body>It works!</body></html>' | sudo tee /var/www/html/index.html
    <html><body>It works!</body></html>
    $ curl -D - localhost
    HTTP/1.1 200 OK
    Date: Sun, 13 Oct 2013 09:12:35 GMT
    Server: Apache/2.2.15 (CentOS)
    Last-Modified: Sun, 13 Oct 2013 09:12:16 GMT
    ETag: "20584-24-4e8a167dd7f45"
    Accept-Ranges: bytes
    Content-Length: 36
    Connection: close
    Content-Type: text/html; charset=UTF-8

    <html><body>It works!</body></html>

#### 参考

- [Red Hat Enterprise Linux 6 Deployment Guide 第15章 Web サーバー – Red Hat Customer Portal](https://access.redhat.com/site/documentation/ja-JP/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/ch-Web_Servers.html)
- [Red Hat Enterprise Linux 6.4 Managing Confined Services 第3章 Apache HTTP Server – Red Hat Customer Portal](https://access.redhat.com/site/documentation/ja-JP/Red_Hat_Enterprise_Linux/6/html/Managing_Confined_Services/chap-Managing_Confined_Services-The_Apache_HTTP_Server.html)
- [Apache HTTP サーバ バージョン 2.2 ドキュメント - Apache HTTP サーバ](http://httpd.apache.org/docs/2.2/ja/)
- [実用 Apache 2.0運用・管理術（1）：Apache 2.0の必須設定と基本セキュリティ対策 - ＠IT](http://www.atmarkit.co.jp/ait/articles/0508/02/news102.html)
