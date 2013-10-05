---
layout: post
title: "Vagrantをつかって仮想マシンを管理する"
date: 2013-08-28 21:41
comments: true
categories: [仮想化]
keywords: 仮想化, Vagrant
description: "Vagrantをつかって仮想マシンを管理する手引きです。"
breadcrumb:
  - title: Blog
    url: /
  - title: 仮想化
    url: /blog/categories/jia-xiang-hua/

---

　Vagrantをつかって仮想マシンを管理する手引きです。Vagrantを利用するにはVirtualBoxやAWSなどのプロバイダを利用できる状態である必要があります。<!-- more -->

 1. ### 試してみる

    　[Vagrantbox.es](http://www.vagrantbox.es/)で紹介されているBoxファイルから仮想マシンを新しく作成します。
    Boxファイルは次のウェブサイトでも公開されています。

    - [vagrantup.com](http://www.vagrantup.com/)
      - [Ubuntu 12.04 precise 64-bit](http://files.vagrantup.com/precise64.box) (323MB)
      - [Ubuntu 12.04 precise 32-bit](http://files.vagrantup.com/precise32.box) (299MB)
    - [Ubuntu Cloud Images](http://cloud-images.ubuntu.com/)
      - [Ubuntu 13.10 saucy 64-bit](http://cloud-images.ubuntu.com/vagrant/saucy/current/saucy-server-cloudimg-amd64-vagrant-disk1.box) (324MB)
      - [Ubuntu 13.10 saucy 32-bit](http://cloud-images.ubuntu.com/vagrant/saucy/current/saucy-server-cloudimg-i386-vagrant-disk1.box) (324MB)
    - [米エネルギー省 国立再生可能エネルギー研究所（NREL）GitHub](http://nrel.github.io/vagrant-boxes/)
      - [CentOS 6.4 x86_64](http://developer.nrel.gov/downloads/vagrant-boxes/CentOS-6.4-x86_64-v20130731.box) (494MB)
      - [CentOS 6.4 i386](http://developer.nrel.gov/downloads/vagrant-boxes/CentOS-6.4-i386-v20130731.box) (459MB)

    　例えばFedora 18 x86 MinimalのBoxファイルをつかう場合、次のように`vagrant init [box-name] [box-url]`を実行して、Vagrantのプロジェクトを開始します。

        $ mkdir vagrant-fedora-18
        $ cd vagrant-fedora-18/
        $ vagrant init fedora-18 http://static.stasiak.at/fedora-18-x86-2.box
        A `Vagrantfile` has been placed in this directory. You are now
        ready to `vagrant up` your first virtual environment! Please read
        the comments in the Vagrantfile as well as documentation on
        `vagrantup.com` for more information on using Vagrant.

    　次に`vagrant up [vm-name]`を実行して、作成したVagrantのプロジェクトから仮想マシンを起動します。

        vagrant-fedora-18 $ vagrant up
        Bringing machine 'default' up with 'virtualbox' provider...
        [default] Box 'fedora-18' was not found. Fetching box from specified URL for
        the provider 'virtualbox'. Note that if the URL does not have
        a box for this provider, you should interrupt Vagrant now and add
        the box yourself. Otherwise Vagrant will attempt to download the
        full box prior to discovering this error.
        Downloading or copying the box...
        Extracting box...te: 456k/s, Estimated time remaining: 0:00:01))
        Successfully added box 'fedora-18' with provider 'virtualbox'!
        [default] Importing base box 'fedora-18'...
        [default] Matching MAC address for NAT networking...
        [default] Setting the name of the VM...
        [default] Clearing any previously set forwarded ports...
        [default] Creating shared folders metadata...
        [default] Clearing any previously set network interfaces...
        [default] Preparing network interfaces based on configuration...
        [default] Forwarding ports...
        [default] -- 22 => 2222 (adapter 1)
        [default] Booting VM...
        [default] Waiting for VM to boot. This can take a few minutes.
        [default] VM booted and ready for use!
        [default] Mounting shared folders...
        [default] -- /vagrant

    　仮想マシンの状態は`vagrant status`やプロバイダのユーザインタフェースから確かめることができます。

        vagrant-fedora-18 $ vagrant status
        Current machine states:

        default                   running (virtualbox)

    {% img /blog/images/2013-08-30-guide-to-managing-guests-using-vagrant/01-verify-the-vm-booted-by-looking-at-the-virtualbox.png 600 VirtualBoxのGUIから仮想マシンの動作を確認する %}

    　起動している仮想マシンへSSH接続するには`vagrant ssh`を実行します。

        $ vagrant ssh
        [vagrant@localhost ~]$
        [vagrant@localhost ~]$ cat /etc/fedora-release
        Fedora release 18 (Spherical Cow)

 2. ### Boxを管理する

    - Boxの一覧 `vagrant box list`
    - Boxの追加 `vagrant box add <name> <url>`（VagrantfileをつくらずにBoxの追加だけを行います）
    - Boxの削除 `vagrant box remove <name>`

    　仮想マシンからBoxファイルを作成するには`vagrant package`をつかいます。
    Boxファイルは仮想マシンディスクイメージ（VMDK）、OVFファイル、そしてVagrantfile等をTAR形式でまとめたものであるようです。

        $ vagrant package
        [default] Attempting graceful shutdown of VM...
        [default] Clearing any previously set forwarded ports...
        [default] Creating temporary directory for export...
        [default] Exporting VM...
        [default] Compressing package to: vagrant-fedora-18/package.box

 3. ### 仮想マシンを管理する

    　Vagrantのプロジェクトディレクトリ内から、ゲストOSの起動や停止などを操作できます。

    - 仮想マシンの起動 `vagrant up`
    - 仮想マシンの停止 `vagrant halt`
    - 仮想マシンの再起動 `vagrant reload`
    - 仮想マシンの一時停止 `vagrant suspend`
    - 仮想マシンの再開 `vagrant resume`
    - 仮想マシンへSSH接続 `vagrant ssh`

    　仮想マシンの状態を確認するには`vagrant status`を実行します。

        $ vagrant status
        Current machine states:

        default                   running (virtualbox)

 4. ### MULTI-MACHINEを利用する

    　複数の仮想マシンをコントロールできるMULTI-MACHINE環境を利用するには、Vagrantfileに仮想マシンの情報を定義します。
    例えば、仮想マシンを２つ、異なるホスト名とBoxで定義する場合、Vagrantfileの`Vagrant.configure(VAGRANTFILE_API_VERSION) do |config| ... end`の間に、次のように記述します。

        config.vm.define :node1 do |node1_config|
          node1_config.vm.hostname = "node1"
        end
        config.vm.define :node2 do |node2_config|
          node2_config.vm.box = "centos-6.3"
          node2_config.vm.hostname = "node2"
        end

    　上のようにVagrantfileを書き換えたあと`vagrant up`を実行してみると、ゲストのFedoraとCentOSが同時に起動します。

        $ vagrant up
        
        Bringing machine 'node1' up with 'virtualbox' provider...
        Bringing machine 'node2' up with 'virtualbox' provider...
        [node1] Importing base box 'fedora-18'...
        [node1] Matching MAC address for NAT networking...
        [node1] Setting the name of the VM...
        [node1] Clearing any previously set forwarded ports...
        [node1] Creating shared folders metadata...
        [node1] Clearing any previously set network interfaces...
        [node1] Preparing network interfaces based on configuration...
        [node1] Forwarding ports...
        [node1] -- 22 => 2222 (adapter 1)
        [node1] Booting VM...
        [node1] Waiting for VM to boot. This can take a few minutes.
        [node1] VM booted and ready for use!
        [node1] Setting hostname...
        [node1] Mounting shared folders...
        [node1] -- /vagrant
        [node2] Importing base box 'centos-6.3'...
        [node2] Matching MAC address for NAT networking...
        [node2] Setting the name of the VM...
        [node2] Clearing any previously set forwarded ports...
        [node2] Fixed port collision for 22 => 2222. Now on port 2200.
        [node2] Creating shared folders metadata...
        [node2] Clearing any previously set network interfaces...
        [node2] Preparing network interfaces based on configuration...
        [node2] Forwarding ports...
        [node2] -- 22 => 2200 (adapter 1)
        [node2] Booting VM...
        [node2] Waiting for VM to boot. This can take a few minutes.
        [node2] VM booted and ready for use!
        [node2] Setting hostname...
        [node2] Mounting shared folders...
        [node2] -- /vagrant
        
        $ vagrant status
        Current machine states:

        node1                     running (virtualbox)
        node2                     running (virtualbox)

    {% img /blog/images/2013-08-30-guide-to-managing-guests-using-vagrant/02-multi-machine.png 600 VirtualBoxからMULTI-MACHINEの動作を確認する %}

    　Vagrantfileにはホスト名以外にも細かく設定を記述することができます。詳しい設定項目については[Documentation](http://docs.vagrantup.com/v2/vagrantfile/index.html)を参照してください。

## 参考

- [Vagrant Documentation](http://docs.vagrantup.com/v2)

## 関連記事

- [Virshをつかって仮想マシンを管理する](/blog/2013/08/17/guide-to-managing-guests-using-virsh/)
- [QEMUをつかって仮想マシンを作成する](/blog/2013/08/11/guide-to-creating-virtual-machine-with-qemu/)

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_top&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=seijimomotobl-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4798121401" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_top&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=seijimomotobl-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=477415038X" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_top&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=seijimomotobl-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4822262731" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
