---
layout: post
title: "Longest palindromic substring問題を考える"
date: 2013-06-30 00:00
comments: true
categories: [JavaScript]
keywords: JavaScript
description: "与えられた文字列の中から、最も長い回文となる部分を探す問題を考えます。"
breadcrumb:
  - title: Blog
    url: /
  - title: JavaScript
    url: /blog/categories/javascript/

---

> 与えられた文字列の中から、最も長い回文となる部分を探す問題を考えます。


<form id="convert-text-to-character-references">
<label>入力 <textarea rows="4" style="width: 100%;"></textarea></label>
<label>出力 <output><textarea rows="4" style="width: 100%;" readonly=True></textarea></output></label>
<script> (function(){

function find_palindrome(string) {
  function is_palindrome(i, j) {
    if (i>=j) return true;
    return (string[i] === string[j]) ? is_palindrome(i+1, j-1) : false;
  }
  var longest = "";
  for (var i=0; i<string.length; i++)
  for (var j=string.length-1; j>0; j--)
  if (is_palindrome(i, j) && (j-i)>longest.length)
    longest = string.slice(i, j+1);
  return longest;
}

function eventhandler(event) {
  if (text === i.value && event.type !== "change") return;
  else o.value = find_palindrome(text = i.value);
}
var text = "";
var f = document.getElementById("convert-text-to-character-references");
var i = f.getElementsByTagName("textarea")[0];
var o = f.getElementsByTagName("textarea")[1];
i.onkeydown = eventhandler;
i.onkeyup   = eventhandler;
})(); </script>
</form>

## 参考

- [Find The Longest Palindrome In A String | Programming Praxis](http://programmingpraxis.com/2010/10/15/find-the-longest-palindrome-in-a-string/)
- [Finding the Longest Palindromic Substring in Linear Time](http://www.akalin.cx/longest-palindrome-linear-time)を読んでから。
