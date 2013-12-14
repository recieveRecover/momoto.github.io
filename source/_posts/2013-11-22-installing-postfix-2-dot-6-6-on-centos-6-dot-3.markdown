---
layout: post
title: "Postfix 2.6.6をCentOS 6にインストールする"
date: 2013-11-23 17:01
comments: true
categories: [メールサーバ]
keywords: Postfix 2.6.6, CentOS 6.3, GNU/Linux
description: "RHEL 6に標準で搭載されているPostfix 2.6.6をつかって、スタンドアロンのMTAを設定するまでの手順をまとめています。"
breadcrumb:
  - title: Blog
    url: /
  - title: メールサーバ
    url: /blog/categories/merusaba/

---

　RHEL 6に標準で搭載されているPostfix 2.6.6をつかって、スタンドアロンのMTAを設定するまでの手順をまとめています。
OSはRHELクローンのCentOS 6.3を使用しています。

　注意点として、SMTPサーバが不正中継に利用されないように、リレー制限の慎重な設定が必要になります。
この記事ではサーバからメールを送信できることと、外部からメールをリレーできないことまでを確認します。
受け取ったメールの配送やメールボックスへのアクセス（POPやIMAP）については扱っていません。

<!-- more -->

### 1. postfixパッケージの確認

　postfixパッケージがシステムにインストールされているかどうか、を`yum list installed postfix`で確認します。
もし、インストールされていなければ`yum install postfix`でインストールすることができます。
ソフトウェアライセンスは[IPL](http://www.ibm.com/developerworks/opensource/library/os-i18n2/os-ipl.html)になるので、
とくに商用利用の責任において[GPL](http://www.gnu.org/copyleft/gpl.html)と異なる部分があります。

    $ yum list installed postfix
    
    Installed Packages
    postfix.x86_64                 2:2.6.6-2.2.el6_1                 @anaconda-CentOS-201207061011.x86_64/6.3

　postfixパッケージの設定ファイルは`rpm -qc postfix`で確認することができます。

<table class="table">
<caption>表１　postfixパッケージの設定ファイル</caption>
<thead>
<tr><th>ファイルの名前</th><th>説明</th></tr>
</thead>
<tbody>
<tr><th>/etc/pam.d/smtp.postfix</th><td>PAM (Pluggable Authentication Modules)の設定</td></tr>
<tr><th>/etc/postfix/access</th><td>SMTPサーバのアクセステーブル</td></tr>
<tr><th>/etc/postfix/canonical</th><td>canonicalテーブルの書式</td></tr>
<tr><th>/etc/postfix/generic</th><td>genericテーブルの書式</td></tr>
<tr><th>/etc/postfix/header_checks</th><td>正規表現による内容検査の設定</td></tr>
<tr><th>/etc/postfix/main.cf</th><td>基本的な設定</td></tr>
<tr><th>/etc/postfix/master.cf</th><td>デーモンプロセスの定義</td></tr>
<tr><th>/etc/postfix/relocated</th><td>relocatedテーブルの書式</td></tr>
<tr><th>/etc/postfix/transport</th><td>transportテーブルの書式</td></tr>
<tr><th>/etc/postfix/virtual</th><td>仮想エイリアステーブルの書式</td></tr>
<tr><th>/etc/sasl2/smtpd.conf</th><td>Cyrus SASLの設定</td></tr>
</tbody>
</table>

### 2. Postfixの設定

　用途に応じて設定を変更していきます。
設定できる項目は600以上あるようですが、単純な用途であれば、ほとんどの項目は初期値のまま利用できます。

<table class="table">
<caption>表２　/etc/postfix/main.cfの基本的な設定</caption>
<thead><tr><th>変数名</th><th>初期値</th><th>説明</th></tr></thead>
<tbody>
<tr><th>myhostname</th><td>hostname.localdomain</td><td>メールシステムのFQDN。この変数は他の多くの設定で参照されます。</td></tr>
<tr><th>mydomain</th><td>localdomain</td><td>メールシステムのインターネットドメイン名。この変数は他の多くの設定で参照されます。</td></tr>
<tr><th>myorigin</th><td>$myhostname</td><td>メールシステムから送信されたメールにおいて、差出人のメールアドレスにつかわれるドメイン。</td></tr>
<tr><th>inet_interfaces</th><td>all</td><td>メールシステムが通信できるネットワークアドレス。ここで許可されていないアドレスとは、メールの受信も送信もできません。</td></tr>
<tr><th>mydestination</th><td>$myhostname, localhost.$mydomain, localhost</td><td>メールシステムに到達したメールにおいて、自分宛のメールとみなすドメインのリスト。</td></tr>
<tr><th>mynetworks</th><td>127.0.0.0/8</td><td>メールシステムがリレーを許可するSMTPクライアントのネットワークアドレスのリスト。</td></tr>
</tbody>
</table>

　現時点の設定は`postconf`コマンドで確認することもできます。
オプションに`-d`を付けると設定の初期値、`-n`を付けると初期値から変更した設定だけを表示します。

### 3. Postfixの起動/再起動

　設定を変更した後は、`postfix check`で設定や権限を検証し、サーバを起動または再起動して変更を反映させます。
`postfix reload`でも設定を反映させることができますが、`inet_interfaces`など一部設定の変更にはサーバ再起動が必要です。

    $ sudo service postfix check
                                                [  OK  ]
    $ sudo service postfix status
    master is stopped
    $ sudo service postfix start
    Starting postfix:                           [  OK  ]

　サービスの自動起動を有効にする場合は`chkconfig`をつかいます。

    $ chkconfig --list postfix
    postfix        	0:off	1:off	2:off	3:off	4:off	5:off	6:off
    $ sudo chkconfig postfix on
    $ chkconfig --list postfix
    postfix        	0:off	1:off	2:on	3:on	4:on	5:on	6:off

### 4. メールの送信テスト

#### 4-1. Sendmail互換インタフェース

　PostfixのSendmail互換インタフェースをつかって、メールサーバのホストからメールを送信してみます。
Sendmail互換インタフェースのパスは`sendmail_path`で設定されています。

    $ sendmail somebody@example.com
    from: user@hostname.localdomain
    to: somebody@example.com
    subject: test email

    this is a test message.
    .

　送信したメールのログは`/var/log/maillog`から、ローカルのメールボックスへ配送されたメールは`/var/mail/{ユーザ名}`（mail_spool_directory）から確認することができます。

#### 4-2. Telnet

　Telnetをつかって、mynetworksで指定していないネットワークアドレスから、relay_domains (mydestination)にないドメインへメールをリレーできないことを確認します。

    $ telnet <SMTP_HOST> 25
    Trying <SMTP_HOST>...
    Connected to <SMTP_HOST>.
    Escape character is '^]'.
    220 hostname.localdomain ESMTP Postfix
    HELO hostname.localdomain
    250 hostname.localdomain
    MAIL FROM: user@hostname.localdomain
    250 2.1.0 Ok
    RCPT TO: somebody@example.com
    554 5.7.1 <somebody@example.com>: Relay access denied

　この例では、SMTP応答コード554 (Transaction failed)を返されて、リレーが失敗しています。

　オープンリレーとなっていないかどうかの確認は[RBL.JP](http://www.rbl.jp/)の[Third Party Relay Check](http://www.rbl.jp/svcheck.php)からも確認することができます。
    
### 参考

- [第16章 メールサーバー - Red Hat Customer Portal](https://access.redhat.com/site/documentation/ja-JP/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/ch-Mail_Servers.html)
- [第13章 Postfix - Red Hat Customer Portal](https://access.redhat.com/site/documentation/ja-JP/Red_Hat_Enterprise_Linux/6/html/Managing_Confined_Services/chap-Managing_Confined_Services-Postfix.html)
- [Postfix Standard Configuration Examples](http://www.postfix.org/STANDARD_CONFIGURATION_README.html)
- [Postfix 標準設定の例 (2.3.x)](http://www.postfix-jp.info/trans-2.3/jhtml/STANDARD_CONFIGURATION_README.html)
