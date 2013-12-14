---
layout: post
title: "CentOS 6のシステム設定を確認する"
date: 2013-09-01 00:00
comments: true
categories: [GNU/Linux]
keywords: RHEL 6, CentOS 6, GNU/Linux, Administration
description: "CentOS 6の基本的なシステム設定を確認します。地域化、ネットワーク、セキュリティ、サービス管理などの設定を対象にしています。"
breadcrumb:
  - title: Blog
    url: /
  - title: GNU/Linux
    url: /blog/categories/gnu-slash-linux/
updated: 2013-09-22 21:14

---

　CentOS 6の基本的なシステム設定を確認します。
ここでいうシステム設定は、地域化、ネットワーク、セキュリティ、サービス管理などの設定を対象にしています。デスクトップ環境や外観の設定は含みません。
また、各機能については基本的な設定までを概観するにとどめて、詳細な設定までは踏み込みません。

<!-- more -->

#### もくじ

- <a href="{{ page.url }}#language">言語の設定</a>
- <a href="{{ page.url }}#clock">システム時刻の設定</a>
- <a href="{{ page.url }}#keyboard">キー配列の設定</a>
- <a href="{{ page.url }}#network">ネットワークの設定</a>
- <a href="{{ page.url }}#name-resolution">名前解決の設定</a>
- <a href="{{ page.url }}#selinux">SELinuxの設定</a>
- <a href="{{ page.url }}#packet-filtering">パケットフィルタリングの設定</a>
- <a href="{{ page.url }}#auto-start-services">自動起動の設定</a>

<!-- 地域化 -->

## <span id="language">言語の設定（/etc/sysconfig/i18n）</span>

　言語情報は地域、文字セットとともにLocaleとして設定します。
現在のLocaleの設定や利用できるLocale名を表示するには[GNU Cライブラリ](http://www.gnu.org/software/libc/)の[locale](http://www.gnu.org/software/libc/manual/html_node/Locales.html)をつかいます。

    $ locale
    LANG=C
    LC_CTYPE="C"
    LC_NUMERIC="C"
    LC_TIME="C"
    LC_COLLATE="C"
    LC_MONETARY="C"
    LC_MESSAGES="C"
    LC_PAPER="C"
    LC_NAME="C"
    LC_ADDRESS="C"
    LC_TELEPHONE="C"
    LC_MEASUREMENT="C"
    LC_IDENTIFICATION="C"
    LC_ALL=
    $ locale -a | grep -Ei "ja|jp"
    ja_JP
    ja_JP.eucjp
    ja_JP.ujis
    ja_JP.utf8
    japanese
    japanese.euc

　LocaleはLANG変数に設定します。
一時的な変更であれば`$ export LANG=ja_JP.UTF-8`のようにコマンドで定義するか、永続的な変更であればファイルに変数を定義します。
変数を定義するファイルは、システム全体に適用する場合は/etc/sysconfig/i18n、ユーザごとに適用する場合は~/.bashrcなどです。

    $ LANG=ja_JP.UTF-8
    $ locale | sudo tee /etc/sysconfig/i18n
    LANG=ja_JP.UTF-8
    LC_CTYPE="ja_JP.UTF-8"
    LC_NUMERIC="ja_JP.UTF-8"
    LC_TIME="ja_JP.UTF-8"
    LC_COLLATE="ja_JP.UTF-8"
    LC_MONETARY="ja_JP.UTF-8"
    LC_MESSAGES="ja_JP.UTF-8"
    LC_PAPER="ja_JP.UTF-8"
    LC_NAME="ja_JP.UTF-8"
    LC_ADDRESS="ja_JP.UTF-8"
    LC_TELEPHONE="ja_JP.UTF-8"
    LC_MEASUREMENT="ja_JP.UTF-8"
    LC_IDENTIFICATION="ja_JP.UTF-8"
    LC_ALL=

　日本語を設定する場合の注意点として、Locale名はja_JP.utf8ではなくja_JP.UTF-8を使用したほうが良いようです。

#### 参考

- [ただのメモ: ja_JP.UTF-8 vs ja_JP.utf8](http://blog.at-dk.info/2011/03/jajputf-8-vs-jajputf8.html)
- [Locales - The GNU C Library](http://www.gnu.org/software/libc/manual/html_node/Locales.html#Locales)

## <span id="clock">システム時刻の設定（/etc/sysconfig/clock）</span>

　時刻系やタイムゾーンは/etc/sysconfig/clockに設定します。また、タイムゾーン情報は[tzdata](https://www.iana.org/time-zones)のバイナリファイルを/etc/localtimeに保存します。

    $ sudo vi /etc/sysconfig/clock

    ZONE="Asia/Tokyo"
    UTC=false

    $ sudo ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

#### 参考

- [Tips & Tricks: Changing timezone | Red Hat](http://www.redhat.com/advice/tips/timezone.html)
- [Date and Time - The GNU C Library](http://www.gnu.org/software/libc/manual/html_node/Date-and-Time.html#Date-and-Time)

## <span id="keyboard">キー配列の設定（/etc/sysconfig/keyboard）</span>

　キー配列は/etc/sysconfig/keyboardに設定します。KEYTABLEに指定できる値は/lib/kbd/keymaps/i386/<*配列*>/<*表名*>.map.gzにある表名です。日本語106であればjp106を設定します。

    $ sudo vi /etc/sysconfig/keyboard
        
    KEYTABLE="jp106"
    KEYBOARDTYPE="pc"

　[kbd](http://www.kbd-project.org/)の[loadkeys](http://www.kbd-project.org/manpages/man1/loadkeys.1.html)をつかって一時的に変更することもできます。

    $ loadkeys jp106
    Loading /lib/kbd/keymaps/i386/qwerty/jp106.map.gz

　/etc/sysconfig/keyboardの設定はX Window Systemのキーボード設定とは異なります。

#### 参考

- [man pages | KBD](http://www.kbd-project.org/manpages/index.html)

<!-- ネットワーク -->

## <span id="network">ネットワークの設定（/etc/sysconfig/{network,network-scripts/}）</span>

　ネットワークの設定は、ネットワークインタフェースによらないグローバルな設定とインタフェースごとに固有の設定とでファイルがわかれています。
    
　グローバルな設定では、/etc/sysconfig/networkにホスト名やゲートウェイなどの情報を設定します。

    $ sudo vi /etc/sysconfig/network
        
    NETWORKING=yes
    HOSTNAME=localhost.localdomain

　インタフェースごとの設定では、/etc/sysconfig/ifcfg-<*インタフェース名*>にDHCPの利用するかどうか、DHCPを利用しない場合のIP情報などを設定します。

    $ sudo vi /etc/sysconfig/network-scripts/ifcfg-eth0

    DEVICE="eth0"
    BOOTPROTO="dhcp"
    NM_CONTROLLED="yes"
    ONBOOT="yes"
    TYPE="Ethernet"

　設定ファイルを書き換えたらネットワークサービスを再起動して変更を反映します。

    $ sudo service network restart
    インターフェース eth0 を終了中:                            [  OK  ]
    ループバックインターフェースを終了中                       [  OK  ]
    ループバックインターフェイスを呼び込み中                   [  OK  ]
    インターフェース eth0 を活性化中:
    eth0 のIP情報を検出中... 完了。
                                                               [  OK  ]

#### 参考

- [パート III. ネットワーキング – Red Hat Customer Portal](https://access.redhat.com/site/documentation/ja-JP/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/part-Networking.html)

## <span id="name-resolution">名前解決の設定（nsswitch.conf、host.conf、hosts、resolv.conf）</span>

- 　名前解決の方法の順序は/etc/nsswitch.confに設定します。

        $ sudo vi /etc/nsswitch.conf

        ...
        #hosts:     db files nisplus nis dns
        hosts:      files dns
        ...

  　/etc/host.confにも同様の設定がありますが、CentOS 6ではnsswitch.confが優先されるようです（System V系とBSD系のちがい？）。

- 　ローカルホストでのみ有効なホストテーブルは/etc/hostsに設定します。

        $ sudo vi /etc/hosts
         
        127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
        ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

- 　ネームサーバのIPアドレスは/etc/resolv.confに設定します。

        $ sudo vi /etc/resolv.conf
        ; generated by /sbin/dhclient-script
        nameserver 10.0.2.3

#### 参考

- [Man page of NSSWITCH.CONF](http://linuxjm.sourceforge.jp/html/LDP_man-pages/man5/nsswitch.conf.5.html)
- [Man page of HOST.CONF](http://linuxjm.sourceforge.jp/html/LDP_man-pages/man5/host.conf.5.html)
- [Man page of HOSTS](http://linuxjm.sourceforge.jp/html/LDP_man-pages/man5/hosts.5.html)
- [Man page of RESOLV.CONF](http://linuxjm.sourceforge.jp/html/LDP_man-pages/man5/resolver.5.html)

## <span id="selinux">SELinuxの設定（/etc/sysconfig/selinux）</span>

　現在のSELinuxの状態を表示するには`getenforce`をつかいます。
SELinuxの状態はEnforcing（SELinuxが有効、ポリシーは強制される）、Permissive（SELinuxが有効、ポリシーは強制されない）、Disabled（SELinuxが無効）のいずれかです。
ポリシーが強制されるとルールに基づいてアクセスが制限されます。強制されない場合はアクセスの制限はありませんが、ルールに反するアクセスがログに記録されます。

    $ getenforce
    Enforcing

　一時的にEnforcing、Permissiveを切替えるには`setenforce`をつかいます。`setenforce 0`でPermissiveへ、`setenforce 1`でEnforcingへ切り替わります。

    $ sudo setenforce 0
    $ getenforce
    Permissive

　永続的に状態を切り替えるには/etc/sysconfig/selinuxのSELINUX=<*状態*>を設定して、システムを再起動します。SELinuxを無効化する場合はSELINUX=disabledを設定します。

    $ sudo vi /etc/sysconfig/selinux
    
    # This file controls the state of SELinux on the system.
    # SELINUX= can take one of these three values:
    #       enforcing - SELinux security policy is enforced.
    #       permissive - SELinux prints warnings instead of enforcing.
    #       disabled - SELinux is fully disabled.
    SELINUX=disabled
    
    $ sudo reboot

　SELinuxには、ポリシー記述の知識がなくても変数のon/offを切り替えるだけでポリシーを変更できるブール値が用意されています。このブール値の一覧を表示するには`getsebool -a`をつかいます。

    $ getsebool -a | head
    abrt_anon_write --> off
    abrt_handle_event --> off
    allow_console_login --> on
    allow_cvs_read_shadow --> off
    allow_daemons_dump_core --> on
    ...

　ブール値を変更するには`setsebool -P <*ブール値名*> <*値*>`をつかいます。

    $ sudo setsebool -P httpd_can_network_connect_db on

#### 参考

- [第一人者がやさしく教える新SELinux入門 - 第一人者がやさしく教える新SELinux入門---目次：ITpro](http://itpro.nikkeibp.co.jp/article/COLUMN/20070827/280411/)
- [Scientific Linux 6 でのSELinux管理コマンドまとめ | 複眼中心](http://rewse.jp/blog/p/4825)
- [コマンドラインでちょっとSELinux  |  MorphMorph](http://morphmorph.com/archives/103)

## <span id="packet-filtering">パケットフィルタリングの設定（iptables）</span>

　iptables管理ツールをつかって、パケットフィルタリングのルール（Netfilterのfilterテーブル）までを簡単に見ていきます。
この記事で扱うのはiptablesの機能のほんの一部で、その他の機能（nat、mangleやip6tables）については扱いません。

　現在のパケットフィルタリングのルールを表示するには`iptables -L`をつかいます。

```
$ sudo iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
ACCEPT     all  --  anywhere             anywhere            state RELATED,ESTABLISHED
ACCEPT     icmp --  anywhere             anywhere
ACCEPT     all  --  anywhere             anywhere
ACCEPT     tcp  --  anywhere             anywhere            state NEW tcp dpt:ssh
REJECT     all  --  anywhere             anywhere            reject-with icmp-host-prohibited

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination
REJECT     all  --  anywhere             anywhere            reject-with icmp-host-prohibited

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
```

　上の出力例は、テーブル（filter）に設定された３つのチェーン（INPUT、FORWARD、OUTPUT）と各チェーンのルール（INPUTに５行、FORWARDに１行、OUTPUTに０行）の内容を表示しています。
この場合、OUTPUTチェーンにはルールが何も設定されておらず、OUTPUTのポリシーにはACCEPTが設定されているので、ローカルからネットワークへ出ていくパケットに適用されるのは常にACCEPTです。
一方、INPUTチェーンのポリシーにもACCEPTが設定されていますが、INPUTにはルールが５つ設定されているので、ネットワークからローカルへ入ってくるパケットはルールのマッチオプションに一致するtargetが適用されます。

　iptablesの`-L (--list)`オプションだけではルールのマッチオプションまで詳しく表示されないので、より詳しい出力を表示させるには`-v (--verbose)`オプションを追加します。

```
    $ sudo iptables -L -v
    Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
     pkts bytes target     prot opt in     out     source               destination
     2290  234K ACCEPT     all  --  any    any     anywhere             anywhere            state RELATED,ESTABLISHED
       27  2268 ACCEPT     icmp --  any    any     anywhere             anywhere
       26  1560 ACCEPT     all  --  lo     any     anywhere             anywhere
        3   132 ACCEPT     tcp  --  any    any     anywhere             anywhere            state NEW tcp dpt:ssh
        1    40 REJECT     all  --  any    any     anywhere             anywhere            reject-with icmp-host-prohibited

    Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
     pkts bytes target     prot opt in     out     source               destination
        0     0 REJECT     all  --  any    any     anywhere             anywhere            reject-with icmp-host-prohibited

    Chain OUTPUT (policy ACCEPT 1534 packets, 283K bytes)
     pkts bytes target     prot opt in     out     source               destination
```

　-L (--list)だけでは表示されていなかったpkts、bytes、in、outの列が表示されています。それぞれの列が示しているのは次のような内容です。

- **pkts** ルールにマッチしたパケット数
- **bytes** 同バイト数
- **target** ルールにマッチしたパケットに対するターゲット
- **prot** ルールを適用するプロトコル
- **opt** パケットの断片化の区別
- **in** ルールを適用する入力インタフェース
- **out** ルールを適用する出力インタフェース、INPUTチェーンでは指定不可
- **source** ルールを適用するパケットの送信元アドレス
- **destination** ルールを適用するパケットの送信先アドレス
- **追加のマッチオプション** 拡張マッチモジュール（stateやreject-withなど）

　上記例のINPUTチェーン１行目のルールだけ見てみると、プロトコル、インタフェース、送信元・送信先のいずれも指定していませんが、
stateモジュールによって既に接続が確立している状態（ESTABLISHED）と既存の接続に関連して新しく接続された状態（RELATED）を指定していて、
プロトコル、インタフェース等を問わず既存の接続はACCEPTするという内容になります。

#### 参考

- [2.8. ファイアウォール – Red Hat Customer Portal](https://access.redhat.com/site/documentation/ja-JP/Red_Hat_Enterprise_Linux/6/html/Security_Guide/sect-Security_Guide-Firewalls.html)
- [Man page of IPTABLES](http://linuxjm.sourceforge.jp/html/iptables/man8/iptables.8.html)
- [Man page of iptables-extensions](http://ipset.netfilter.org/iptables-extensions.man.html)

## <span id="auto-start-services">自動起動の設定（chkconfig）</span>

　サービスの自動起動はchkconfigで設定します。サービスと現在の設定の一覧を表示するには`chkconfig --list`をつかいます。

    $ chkconfig --list
    NetworkManager  0:off   1:off   2:on    3:on    4:on    5:on    6:off
    abrtd           0:off   1:off   2:off   3:on    4:off   5:on    6:off
    acpid           0:off   1:off   2:on    3:on    4:on    5:on    6:off
    anamon          0:off   1:off   2:off   3:off   4:off   5:off   6:off
    atd             0:off   1:off   2:off   3:on    4:on    5:on    6:off
    auditd          0:off   1:off   2:on    3:on    4:on    5:on    6:off
    ...

　上記の一覧は一行ずつ、サービス名とどのランレベル（0〜6）で起動するか（on/off）を示しています。

　サービスの自動起動を有効にするには`chkconfig <サービス名> on`をつかいます。無効にするには`chkconfig <サービス名> off`にしてください。

    $ sudo chkconfig httpd on
    $ chkconfig --list httpd
    httpd          0:off   1:off   2:on    3:on    4:on    5:on    6:off

　ランレベルとは、システムの状態（シングルユーザモードかマルチユーザモードなど）によって切り替わる数字です。CUIのマルチユーザモードであれば3、GUIのマルチユーザモードであれば5が、CentOS 6のデフォルトです。
ランレベルについて詳しく踏み込みませんが、/etc/inittabのコメントで少し説明されています。現在のランレベルを確認するには`runlevel`をつかいます。

    $ tail /etc/inittab

    # Default runlevel. The runlevels used are:
    #   0 - halt (Do NOT set initdefault to this)
    #   1 - Single user mode
    #   2 - Multiuser, without NFS (The same as 3, if you do not have networking)
    #   3 - Full multiuser mode
    #   4 - unused
    #   5 - X11
    #   6 - reboot (Do NOT set initdefault to this)
    #
    id:3:initdefault:

    $ runlevel
    N 3

#### 参考

- [10.2.3. chkconfig &#12518;&#12540;&#12486;&#12451;&#12522;&#12486;&#12451;&#12398;&#20351;&#29992; - Red Hat Customer Portal](https://access.redhat.com/site/documentation/ja-JP/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/s2-services-chkconfig.html)

<!-- ## リソース管理 -->

## 参考

- [導入ガイド - Red Hat Customer Portal](https://access.redhat.com/site/documentation/ja-JP/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/index.html)
- [セキュリティガイド – Red Hat Customer Portal](https://access.redhat.com/site/documentation/ja-JP/Red_Hat_Enterprise_Linux/6/html/Security_Guide/index.html)
- [Security-Enhanced Linux - Red Hat Customer Portal](https://access.redhat.com/site/documentation/ja-JP/Red_Hat_Enterprise_Linux/6/html/Security-Enhanced_Linux/index.html)

## 関連記事

- [CentOS 6.4をインストールする](/2013/04/06/install-centos-6-dot-4-i386-netinstall/)

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_top&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=seijimomotobl-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4774146757" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_top&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=seijimomotobl-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4774158135" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_top&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=seijimomotobl-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=B00BWCSUYS" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_top&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=seijimomotobl-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4774155934" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
