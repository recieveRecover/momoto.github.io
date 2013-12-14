---
layout: post
title: "テキストを文字参照記述へ変換する"
date: 2012-08-11 00:00
comments: true
categories: [JavaScript]
keywords: ISO 10646, Unicode, HTML, 数値文字参照, 変換
description: 入力された文字列の符号位置（Unicode）を出力します。
updated: 2013-08-10 19:49

---

　入力された文字列の符号位置（Unicode）を出力します。テキストをHTML数値文字参照に変換するように動作します。

<!-- more -->

<form id="convert-text-to-character-references">
<label>形式 <select><option>HTML文字参照（10進数）</option><option>HTML文字参照（16進数）</option><option>符号位置のみ（10進数）</option><option>符号位置のみ（16進数）</option></select></label><br>
<label>入力 <textarea rows="4" style="width: 100%;"></textarea></label>
<label>出力 <output><textarea rows="4" style="width: 100%;" readonly=True></textarea></output></label>
<script> (function(){
function decimal_form(codepoint) { return "&#" + codepoint.toString(10) + ";"; }
function hexadecimal_form(codepoint) { return "&#x" + codepoint.toString(16) + ";"; }
function decimal_delimit_space(codepoint) { return codepoint.toString(10) + " "; }
function hexadecimal_delimit_space(codepoint) { return codepoint.toString(16) + " "; }
function convert_text_to_character_references(text) {
  for (var encoded="", i=0; i<text.length; i++)
    if (s.selectedIndex === 0) encoded += decimal_form(text.charCodeAt(i));
    else if (s.selectedIndex === 1) encoded += hexadecimal_form(text.charCodeAt(i));
    else if (s.selectedIndex === 2) encoded += decimal_delimit_space(text.charCodeAt(i));
    else if (s.selectedIndex === 3) encoded += hexadecimal_delimit_space(text.charCodeAt(i));
  o.value = encoded;
}
function eventhandler(event) {
  if (text === i.value && event.type !== "change")
    return;
  else
    convert_text_to_character_references(text = i.value);
}
var text = "";
var f = document.getElementById("convert-text-to-character-references");
var s = f.getElementsByTagName("select")[0];
var i = f.getElementsByTagName("textarea")[0];
var o = f.getElementsByTagName("textarea")[1];
i.onkeydown = eventhandler;
i.onkeyup   = eventhandler;
s.onchange  = eventhandler;
})(); </script>
</form>

### 参考

- [The Unicode Character Database (UCD)](http://www.unicode.org/Public/UCD/latest/ucd/)
- [RFC 1815 - Character Sets ISO-10646 and ISO-10646-J-1](http://www.ietf.org/rfc/rfc1815.txt)
- [8.1.4 Character references — HTML5](http://www.w3.org/TR/html5/syntax.html#character-references)
