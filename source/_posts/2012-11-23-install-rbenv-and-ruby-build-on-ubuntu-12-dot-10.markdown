---
layout: post
title: "Ubuntu 12.10にrbenvとruby-buildをインストール"
date: 2012-11-23 20:23
comments: true
categories: [GNU/Linux, Ruby]
keywords: rbenv, ruby-build, Ubuntu
description: "rbenvとruby-buildを使用して、Ubuntu 12.10にRuby 1.9.3をインストールしたときの記録です。"

---

rbenvとruby-buildを使用して、Ubuntu 12.10にRuby 1.9.3をインストールしたときの記録です。
rbenvとruby-buildはディストリビューションが配布するパッケージに含まれているため、Ubuntuのバージョン管理システム（APT）からインストールすることもできますが、次の記録はgithub.comからクローンする方法によるものです。

(\* *[rbenv][1]はprecise (12.04)以降、[ruby-build][2]はquantal (12.10)以降にuniverseに追加されたようです* )

<!-- more -->

### rbenvのインストール

    $ git clone git://github.com/sstephenson/rbenv.git .rbenv
      Cloning into '.rbenv'...
      remote: Counting objects: 1040, done.
      remote: Compressing objects: 100% (413/413), done.
      remote: Total 1040 (delta 649), reused 962 (delta 599)
      Receiving objects: 100% (1040/1040), 138.43 KiB | 115 KiB/s, done.
      Resolving deltas: 100% (649/649), done.

    $ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
    $ echo 'eval "$(rbenv init -)"' >> ~/.bashrc

### ruby-installのインストール

    $ mkdir -p ~/.rbenv/plugins
    $ cd ~/.rbenv/plugins
    $ git clone git://github.com/sstephenson/ruby-build.git
      Cloning into 'ruby-build'...
      remote: Counting objects: 1329, done.
      remote: Compressing objects: 100% (631/631), done.
      remote: Total 1329 (delta 691), reused 1183 (delta 555)
      Receiving objects: 100% (1329/1329), 146.68 KiB | 150 KiB/s, done.
      Resolving deltas: 100% (691/691), done.

    $ cd ruby-build
    $ sudo bash ./install.sh
      Installed ruby-build at /usr/local

### Rubyの特定のバージョンをインストール（1.9.3-p327の場合）

    ## インストールが可能なバージョンのリストから"1.9.3"のバージョンを抽出
    $ rbenv install --list | grep "1.9.3"
      1.9.3-dev
      1.9.3-p0
      1.9.3-p125
      1.9.3-p194
      1.9.3-p286
      1.9.3-p327
      1.9.3-preview1
      1.9.3-rc1

    ## 1.9.3-p327を指定して、目的のバージョンをインストール
    $ rbenv install 1.9.3-p327
      Downloading yaml-0.1.4.tar.gz...
      -> http://cloud.github.com/downloads/sstephenson/ruby-build-download-mirror/36c852831d02cf90508c29852361d01b
      Installing yaml-0.1.4...
      Installed yaml-0.1.4 to $HOME/.rbenv/versions/1.9.3-p327

      Downloading ruby-1.9.3-p327.tar.gz...
      -> http://cloud.github.com/downloads/sstephenson/ruby-build-download-mirror/96118e856b502b5d7b3a4398e6c6e98c
      Installing ruby-1.9.3-p327...
      Installed ruby-1.9.3-p327 to $HOME/.rbenv/versions/1.9.3-p327

### Rubyの特定のバージョンを使用するように設定する（1.9.3-p194の場合）

    ## 既にインストールしたRubyのバージョンのリストを取得する
    $ rbenv versions
      1.9.3-p194
      1.9.3-p327

    ## 1.9.3-p194を指定して、目的のバージョンを設定する（.rbenv-versionが作成されます）
    ~/workspace$ rbenv local 1.9.3-p194

    ## 設定されているRubyのバージョンの確認（この場合`rbenv local`でも同じ結果が得られます）
    ~/workspace$ rbenv version
      1.9.3-p194 (set by $HOME/workspace/.rbenv-version)

    ## カレントディレクトリで設定されているRubyのバージョンを解除
    ~/workspace$ rbenv local --unset

### 参考
- [packages.ubuntu.com/quantal/rbenv][1]
- [packages.ubuntu.com/quantal/ruby-build][2]
- [github.com/sstephenson/rbenv][3]
- [github.com/sstephenson/ruby-build][4]

  [1]: http://packages.ubuntu.com/quantal/rbenv      "Ubuntu -- quantal の rbenv パッケージに関する詳細"
  [2]: http://packages.ubuntu.com/quantal/ruby-build "Ubuntu -- quantal の ruby-build パッケージに関する詳細"
  [3]: https://github.com/sstephenson/rbenv      "sstephenson/rbenv"
  [4]: https://github.com/sstephenson/ruby-build "sstephenson/ruby-build"

