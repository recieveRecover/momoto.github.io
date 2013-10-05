---
layout: post
title: "Amazon EC2インスタンスを作成し、SSH接続する"
date: 2012-12-06 00:00
comments: true
categories: [クラウドコンピューティング, AWS]
keywords: Amazon Web Services, AWS, EC2, SSH, AMI
description: "Amazon EC2インスタンスを作成し、SSH接続するまでの手順の記録です。"
breadcrumb:
  - title: Blog
    url: /
  - title: クラウドコンピューティング
    url: /blog/categories/kuraudokonpiyuteingu/

---

　Amazon EC2インスタンスを作成し、SSH接続するまでの手順の記録です。

<!-- more -->

1.  　Amazon EC2 Console Dashboardから「Launch Instance」を選択する。
    必要な操作はすべてウェブインタフェースから行うので、ブラウザさえあればEC2インスタンスを作成することができる。

    [{% img /blog/images/2012-12-06-create-and-ssh-to-ec2-instance/01.png 600 %}](/blog/images/2012-12-06/01.png)

2.  　インスタンス作成ウィザードの種類を選択する。
    2012年12月現在、選択できる種類はClassic、Quick Launch、AWS Marketplaceの三通り。
    以下、Classic Wizardでインスタンスの作成を行う。

    [{% img /blog/images/2012-12-06-create-and-ssh-to-ec2-instance/02.png 600 %}](/blog/images/2012-12-06/02.png)
    
3.  　AMI（Amazon Machine Image）を選択する。
    選択できる主なOSにはRed Hat Enterprise、Ubuntu、Microsoft Windowsなどがある。

    [{% img /blog/images/2012-12-06-create-and-ssh-to-ec2-instance/03.png 600 %}](/blog/images/2012-12-06/03.png)

4.  　インスタンスの詳細を設定する。
    設定できる項目は、インスタンスタイプ、カーネルID、RAMディスク、ストレージデバイスなど。

    [{% img /blog/images/2012-12-06-create-and-ssh-to-ec2-instance/04-01.png 300 %}](/blog/images/2012-12-06/04-01.png)

    [{% img /blog/images/2012-12-06-create-and-ssh-to-ec2-instance/04-02.png 300 %}](/blog/images/2012-12-06/04-02.png)

    [{% img /blog/images/2012-12-06-create-and-ssh-to-ec2-instance/04-03.png 300 %}](/blog/images/2012-12-06/04-03.png)

    　インスタンスタイプは、その種類によって性能（CPUやI/O）や発生する費用が異なってくるため、稼働させるアプリケーションや予算にあわせて選択する。

5.  　公開鍵暗号の鍵のペアを作成する。
    ウィザードには鍵の名前を入力するだけでRSA暗号方式の鍵のペアが生成され、pemファイルをダウンロードできる。
    このpemファイルがSSH接続に必要な秘密鍵になる。

    [{% img /blog/images/2012-12-06-create-and-ssh-to-ec2-instance/05.png 600 %}](/blog/images/2012-12-06/05.png)

6.  　EC2のファイアウォール「Security Groups」を設定する。
    グループごとにプロトコル（HTTPやSMTP）、ポート番号、IPアドレスを指定してインバウンドルールを設定することができる。

    [{% img /blog/images/2012-12-06-create-and-ssh-to-ec2-instance/06.png 600 %}](/blog/images/2012-12-06/06.png)

7.  　EC2インスタンスが完成。ダッシュボードの"Instances"から起動状態や公開ドメイン名などを確認できる。

    [{% img /blog/images/2012-12-06-create-and-ssh-to-ec2-instance/07-01.png 600 %}](/blog/images/2012-12-06/07-01.png)

    [{% img /blog/images/2012-12-06-create-and-ssh-to-ec2-instance/07-02.png 600 %}](/blog/images/2012-12-06/07-02.png)

### SSH接続

　SSHクライアントからEC2インスタンスへSSH接続を行う。
接続先に公開ドメイン名（Public DNS : ec2-\*\*\*-\*\*\*-\*\*\*-\*\*\*.compute-1.amazonaws.com）と、秘密鍵には先に取得したpemファイルを指定して、EC2インスタンスへ接続する。

[{% img /blog/images/2012-12-06-create-and-ssh-to-ec2-instance/08.png 600 %}](/blog/images/2012-12-06/08.png)

### EC2インスタンスの終了

　EC2インスタンスの利用を終える場合は、終了（Terminate）を指定する。停止（Stop）ではインスタンスは削除されない。
逆に、利用を一時的に停止するだけであればTerminateではなくStopを指定する。

[{% img /blog/images/2012-12-06-create-and-ssh-to-ec2-instance/09.png 600 %}](/blog/images/2012-12-06/09.png)

### 参考
- [Amazon EC2 (仮想サーバー Amazon Elastic Compute Cloud) | アマゾン ウェブ サービス（AWS 日本語）](http://aws.amazon.com/jp/ec2/)

<iframe src="http://rcm-jp.amazon.co.jp/e/cm?lt1=_top&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=alqet049-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4822211983" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
<iframe src="http://rcm-jp.amazon.co.jp/e/cm?lt1=_top&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=alqet049-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4873115817" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
