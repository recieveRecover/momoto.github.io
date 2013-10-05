---
layout: post
title: "Markdownの概要"
date: 2012-07-31 11:06
comments: true
categories: [Markdown]
keywords: Markdown
description: 

---

覚えておきたいと思うMarkdown記法の備忘録です。複数の記法がある場合のその記述や、詳しい構文についてはふれていません。

<!-- more -->

### 見出し
#### Markdown記法
``` plain
# Heading1
## Heading2
### Heading3
#### Heading4
##### Heading5
###### Heading6
```
#### HTML変換例
``` plain
<h1>Heading1</h1>
<h2>Heading2</h2>
<h3>Heading3</h3>
<h4>Heading4</h4>
<h5>Heading5</h5>
<h6>Heading6</h6>
```

### リスト
#### 記法（unordered）
``` plain
- リスト１
- リスト２
    - リスト２ー１
    - リスト２ー２
- リスト３
- リスト４
```
#### HTML変換例
``` plain
<ul>
  <li>リスト１</li>
  <li>リスト２
    <ul>
      <li>リスト２ー１</li>
      <li>リスト２ー２</li>
    </ul>
  </li>
  <li>リスト３</li>
  <li>リスト４</li>
</ul>
```
#### 記法（ordered）
``` plain
1.  - unordered1
    - unordered2
    - unordered3
2.  1.  ordered1
    2.  ordered2
    3.  ordered3
```
#### HTML変換例
``` plain
<ol>
  <li><ul>
    <li>unordered1</li>
    <li>unordered2</li>
    <li>unordered3</li>
  </ul></li>
  <li><ol>
    <li>ordered1</li>
    <li>ordered2</li>
    <li>ordered3</li>
  </ol></li>
</ol>
```

### 罫線
#### 記法
``` plain
***
* * *
- - -
```
#### HTML変換例
``` plain
<hr />
<hr />
<hr />
```

#### 引用

##### 記法

``` plain
> BACKQUOTES
> BACKQUOTES
>> BACKQUOTES
> BACKQUOTES
```

##### 表示例
> > BACKQUOTES
> > BACKQUOTES
> >> BACKQUOTES
> > BACKQUOTES


#### 参考
* [Daring Fireball: Markdown Syntax Documentation](http://daringfireball.net/projects/markdown/syntax)
* [Markdown - Wikipedia](http://ja.wikipedia.org/wiki/Markdown)

