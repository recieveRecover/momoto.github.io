---
layout: post
title: "UML 2.0の概要"
date: 2012-08-02 19:37
comments: true
categories: [UML]
keywords:
description:

---

統一モデリング言語（UML）は、システムの構造や振る舞いを図で表現するために標準化されたモデリング言語の一つ。
規定されている13種類の図を必要に応じて使い分ける。

1. クラス図
2. 複合構造図
3. コンポーネント図
4. 配置図
5. オブジェクト図
6. パッケージ図
7. アクティビティ図
8. ユースケース図
9. ステートマシン図
10. シーケンス図
11. コミュニケーション図
12. 相互作用概要図
13. タイミング図

<!-- more -->

#### 各図の特徴
1. クラス図

    システムの静的な構造を記述する。構成される主な要素は**クラス**と、それらの**関連**。
    {% img /blog/images/2012-08-02-overview-uml/01-classdiagram.png %}

2. 複合構造図

    

3. コンポーネント図

    

4. 配置図

    

5. オブジェクト図

    システムの特定の状況での静的な構造を記述する。構成される主な要素は**オブジェクト**と、それらの**リンク**。

    {% img /blog/images/2012-08-02-overview-uml/02-objectdiagram.png %}

6. パッケージ図

    クラスなどの要素をグループ化し、階層構造を記述できる。

    {% img /blog/images/2012-08-02-overview-uml/03-packagediagram.png %}

7. アクティビティ図

    オブジェクトの振る舞いとその遷移を記述する。構成される主な要素は**Activity**と**Activity Edge**。

8. ユースケース図

    システムの機能とユーザを記述する。構成される主な要素は**アクタ**と**ユースケース**。

9. ステートマシン図

    オブジェクトの状態とその遷移を記述する。構成される主な要素は**State**と**Transition**。

10. シーケンス図

    オブジェクト間の相互作用を記述する。構成される主な要素はオブジェクトの**ライフライン**と、それらの**メッセージ**。
    コミュニケーション図と比べると、メッセージの時系列を強調できる。

11. コミュニケーション図

    オブジェクト間の相互作用を記述する。構成される主な要素は**オブジェクト**と、それらの**メッセージフロー**、**関連**。
    シーケンス図と比べると、オブジェクト間の関連を強調できる。

12. 相互作用概要図
13. タイミング図

<iframe frameborder="0" marginheight="0" marginwidth="0" scrolling="no" src="http://rcm-jp.amazon.co.jp/e/cm?t=seijimomotobl-22&amp;o=9&amp;p=8&amp;l=as1&amp;asins=4894712636&amp;ref=qf_sp_asin_til&amp;fc1=000000&amp;IS2=1&amp;lt1=_top&amp;m=amazon&amp;lc1=0000FF&amp;bc1=FFFFFF&amp;bg1=FFFFFF&amp;f=ifr" style="height: 240px; width: 120px;"></iframe>

#### 参考
- [Unified Modeling Language - OMG Modeling and Metadata Specifications](http://www.omg.org/technology/documents/modeling_spec_catalog.htm#UML)
- [OMG Document -- formal/05-07-04 (UML Superstructure Specification, v2.0)](http://www.omg.org/cgi-bin/doc?formal/05-07-04)
- [統一モデリング言語 - Wikipedia](http://ja.wikipedia.org/wiki/統一モデリング言語)

