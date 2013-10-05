---
layout: post
title: "getEqualSumSubstring関数の問題を考える"
date: 2013-06-30 00:00
comments: true
categories: [JavaScript]
keywords: JavaScript
description: "getEqualSumSubstring関数の問題を考えてみます（JavaScript）。"
breadcrumb:
  - title: Blog
    url: /
  - title: JavaScript
    url: /blog/categories/javascript/

---

　getEqualSumSubstring関数の問題を考えてみます。この問題の出典についてはわかりませんが、Stack Overflowには同問題に関する質問がいくつか寄せられているように、プログラミングの分野では有名な問題であるようです。

### 問題

　問題文を日本語に訳してみると、だいたい次のような内容です。

> 　0以外の数字だけを含む文字列sを引数にとるgetEqualSumSubstring関数を定義します。<br>
> 　この関数はsの連続する部分列のうち、長さは2*Nで、左側N個の数字の合計と右側N個の数字の合計が等しくなるような最も長い部分列の長さを表示します。<br>
> 　適当な部分列がない場合には0と表示します。
>
> [To find the longest substring with equal sum in left and right in C++ - Stack Overflow](http://stackoverflow.com/questions/8469407/to-find-the-longest-substring-with-equal-sum-in-left-and-right-in-c)

#### 入力例＃１

　入力された文字列が123231であるとき、連続する最も長い部分列は123231で、出力する部分列の長さは6です。

#### 入力例＃２

　入力された文字列が986561517416921217551395112859219257312であるとき、連続する最も長い部分列は656151741692121755139511285921925731で、出力する部分列の長さは36です。

### 解答例

　JavaScriptの解答例です。順を追って問題を理解するためにも、文字列sから適当な部分列を抜き出す処理と、抜き出した部分列から適当な部分列を探す処理とで分けて考えてみます。

```
function getEqualSumSubstring(s) {
  var substrings = [], longest = "";

  for (var begin=0; begin<s.length; begin++)
    for (var end=begin+2; end<=s.length; end=end+2)
      substrings[substrings.length] = s.slice(begin, end);

  for (var i in substrings)
    if (substrings[i].length <= longest.length)
      continue;
    else if ((function (s) {
      var begin=0, end=s.length/2, left=0, right=0;
      for (begin; begin<end; begin++) {
        left  += parseInt(s[begin]);
        right += parseInt(s[end + begin]);
      }
      return (left === right);
    })(substrings[i]))
      longest = substrings[i];

  return longest.length;
}
```

　まず、出力するべき部分列の長さは偶数であることが問題文からわかるので、長さが偶数になる部分列だけをsから抜き出して配列に格納しておきます。
このときに抜き出す部分列の個数は`floor((sの長さ+1)/2) * floor(sの長さ/2)`となればよさそうです。
次に、部分列の左側半分の数字の合計と右側半分の数字の合計が等しくなるような部分列の最も長いものを配列から探して、その長さを出力します。

#### 別解

　上記の解答例は、文字列sの一文字目から部分列が短い順に適当な部分列を探しているので、この点にリファクタリングの余地がありそうです。
最も長い部分列を効率よく見つけるために、部分列が長い順に適当な部分列を探すように改善を考えてみます。

```
function getEqualSumSubstring(s) {
  var substring="";

  for (var end=s.length-s.length%2; end>0; end=end-2)
    for (var begin=0; s.length>=(begin+end); begin++)
      if ((function (s) {
        var begin=0, end=s.length/2, left=0, right=0;
        for (begin; begin<end; begin++) {
          left  += parseInt(s[begin]);
          right += parseInt(s[end + begin]);
        }
        return (left === right);
      })(substring = s.slice(begin, (end+begin))))
        return substring.length;

  return 0;
}
```

　この別解では、部分列が長い順に適当な部分列を探しているので、候補の部分列を配列に格納する必要がなくなりました。部分列の左右の和が等しいかどうかを調べる部分は変更していません。

#### 実装

　上記の解答例にHTMLのユーザインタフェースを加えた実装例です。

<form id="finding-the-longest-contiguous-equal-sum-substring-of-length-2n">
<label>入力 <textarea rows="4" style="width: 100%;"></textarea></label>
<label>出力 <output><textarea rows="4" style="width: 100%;" readonly=True></textarea></output></label>
<script> (function(){

function getEqualSumSubstring(s) {
  var substring="";

  for (var end=s.length-s.length%2; end>0; end=end-2)
    for (var begin=0; s.length>=(begin+end); begin++)
      if ((function (s) {
        var begin=0, end=s.length/2, left=0, right=0;
        for (begin; begin<end; begin++) {
          left  += parseInt(s[begin]);
          right += parseInt(s[end + begin]);
        }
        return (left === right);
      })(substring = s.slice(begin, (end+begin))))
        return substring.length;

  return 0;
}

function eventhandler(event) {
  if (text === i.value && event.type !== "change") return;
  else o.value = getEqualSumSubstring(text = i.value);
}
var text = "";
var f = document.getElementById("finding-the-longest-contiguous-equal-sum-substring-of-length-2n");
var i = f.getElementsByTagName("textarea")[0];
var o = f.getElementsByTagName("textarea")[1];
i.onkeydown = eventhandler;
i.onkeyup   = eventhandler;
})(); </script>
</form>

### 参考

- [To find the longest substring with equal sum in left and right in C++ - Stack Overflow](http://stackoverflow.com/questions/8469407/to-find-the-longest-substring-with-equal-sum-in-left-and-right-in-c)
- [Array.slice - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)
