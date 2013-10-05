---
layout: post
title: "Ruby各種ライブラリに必要なパッケージ"
date: 2012-08-01 03:18
comments: true
categories: [Ruby]
keywords: Ruby, curses, readline, openssl, zlib, rubypython
description: "Ruby各種ライブラリに必要なパッケージの記録です。"

---

### no such file to load -- curses (LoadError)

- RPM
```
$ sudo yum install ncurses-devel
```

### no such file to load -- readline (LoadError)

- RPM
```
$ sudo yum install readline-devel
```

### no such file to load -- openssl (RuntimeError)

- RPM
```
$ yum install openssl-devel
```

- deb
```
$ sudo apt-get install libssl-dev
```

### no such file to load -- zlib

- RPM
```
$ sudo yum install zlib-devel
```

### cannot load such file -- rubypython (LoadError)

- RPM
```
$ sudo yum install python-devel
```

- deb
```
$ sudo apt-get install python-dev
```

### 参考
- [Ruby 1.9.3 リファレンスマニュアル > ライブラリ一覧 ](http://doc.ruby-lang.org/ja/1.9.3/library/)
- [Ruby 2.0.0 リファレンスマニュアル > ライブラリ一覧 ](http://doc.ruby-lang.org/ja/2.0.0/library/)
- [RAA - Ruby Application Archive](http://raa.ruby-lang.org/)
