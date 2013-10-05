---
layout: post
title: "Apache HTTP Server 2.2.15をインストールする（CentOS 6でYumをつかう方法）"
date: 2013-09-07 17:29
comments: true
categories: [WWW, Apache]
keywords: Apache HTTP Server 2.2.15, httpd, GNU/Linux, CentOS 6, Yum
description: ""

---


    $ sudo yum install httpd
    ...
    Installed:
      httpd.x86_64 0:2.2.15-29.el6.centos

    Dependency Installed:
      apr.x86_64 0:1.3.9-5.el6_2         apr-util.x86_64 0:1.3.9-3.el6_0.1      apr-util-ldap.x86_64 0:1.3.9-3.el6_0.1      httpd-tools.x86_64 0:2.2.15-29.el6.centos
      mailcap.noarch 0:2.1.31-2.el6

    Complete!


    [vagrant@localhost ~]$ yum info httpd
    
    Available Packages
    Name        : httpd
    Arch        : x86_64
    Version     : 2.2.15
    Release     : 29.el6.centos
    Size        : 821 k
    Repo        : updates
    Summary     : Apache HTTP Server
    URL         : http://httpd.apache.org/
    License     : ASL 2.0
    Description : The Apache HTTP Server is a powerful, efficient, and extensible
                : web server.
                
### rpm -ql

httpdパッケージが提供しているファイルの一覧

    $ rpm -ql httpd | gawk -F/ '{print "/"$2"/"$3}' | uniq
    /etc/httpd
    /etc/logrotate.d
    /etc/rc.d
    /etc/sysconfig
    /usr/lib64
    /usr/sbin
    /usr/share
    /var/cache
    /var/lib
    /var/log
    /var/run
    /var/www

##

- http://code.google.com/p/mod-spdy/
- http://code.google.com/p/mod-spdy/wiki/GettingStarted
- https://developers.google.com/speed/spdy/mod_spdy/

