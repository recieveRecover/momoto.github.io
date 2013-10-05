---
layout: post
title: "ソースからAndroidをビルドする"
date: 2013-09-05 20:02
comments: true
categories: 
---

- GNU/Linux
- Gingerbread (2.3.x)
- 30GB free disk space
- Python 2.6-2.7
- GNU Make 3.81-3.82
- JDK 6, JDK 5
- Git

## GNU/Linuxのビルド環境をセットアップする

### ビルド環境の初期化

    $ sudo aptitude install sun-java6-jdk
    $ sudo aptitude install git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev libc6-dev lib32ncurses5-dev ia32-libs x11proto-core-dev libx11-dev lib32readline5-dev lib32z-dev libgl1-mesa-dev g++-multilib mingw32 tofrodos python-markdown libxml2-utils xsltproc
    $ sudo vim.tiny /etc/udev/rules.d/51-android.rules


    vagrant@precise64:~$ export USE_CCACHE=1

### ダウンロード

    vagrant@precise64:~$ mkdir ~/bin
    vagrant@precise64:~$ PATH=~/bin/:$PATH
    vagrant@precise64:~$ curl https://dl-ssl.google.com/dl/googlesource/git-repo/repo > ~/bin/repo
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100 22889  100 22889    0     0  70762      0 --:--:-- --:--:-- --:--:--   98k
    vagrant@precise64:~$ chmod a+x ~/bin/repo

    vagrant@precise64:~$ mkdir Workspace
    vagrant@precise64:~$ cd Workspace/
    vagrant@precise64:~/Workspace$ repo init -u https://android.googlesource.com/platform/manifest
    vagrant@precise64:~/Workspace$ repo init -u https://android.googlesource.com/platform/manifest -b android-4.0.1_r1
    .repo/manifests/: discarding 89 commits

    Your identity is: Seiji Momoto <seiji.momoto@facebook.com>
    If you want to change this, please re-run 'repo init' with --config-name

    repo has been initialized in /home/vagrant/Workspace

### ビルドと実行


    

## 参考

- http://source.android.com/source/building.html
