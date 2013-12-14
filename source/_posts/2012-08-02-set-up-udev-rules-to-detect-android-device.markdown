---
layout: post
title: "Android端末を検出するためのudevの設定"
date: 2012-08-02 16:45
comments: true
categories: [Android]
keywords: udev, Android, USBベンダーID, adb
description:

---

デバイス管理ツールとしてudevを使用しているLinuxシステムで、USB接続したAndroid端末を検出できない場合、USBベンダーIDごとにudevルールを加える必要がある。

<!-- more -->

例えば、USBベンダーIDが`0bb4`になるHTCの端末を検出する場合、次の内容のファイルを`51-android.rules`として`/etc/udev/rules.d`に加える。

``` plain /etc/udev/rules.d/51-android.rules
SUBSYSTEM=="usb", ATTR{idVendor}=="0bb4", MODE="0666", GROUP="plugdev"
```

各メーカーに対応するUSBベンダーIDは、http://developer.android.com/tools/device.htmlで参照できる。

また、`/etc/udev/rules.d`への書き込みにはroot権限が必要で、ファイルを追加した後は読み出しを許可する必要がある。

``` 
$ sudo chmod a+r /etc/udev/rules.d/51-android.rules
```

USB接続したAndroid端末を検出できているかどうかは、`lsusb`またはAndroid SDKに含まれる`adb`コマンドで確認できる。

```
$ lsusb 

  Bus 002 Device 008: ID 0bb4:0c8d High Tech Computer Corp. EVO 4G (debug)
```

``` 
$ ~/android-sdks/platform-tools/adb devices

  List of devices attached 
  SHTBB106827	device
```

adbから検出できれば、EclipseプラグインADTの「Device Chooser」からも同様に認識できる。

{% img /blog/images/2012-08-02-set-up-udev-rules-to-detect-android-device/01.png %}

#### 参考
- [Using Hardware Devices | Android Developers](http://developer.android.com/tools/device.html)
- [udevルールの書き方](http://www.gentoo.gr.jp/transdocs/udevrules/udevrules.html)

