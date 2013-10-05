---
layout: post
title: "grep Tutorial"
date: 2013-02-24 23:35
comments: true
categories: 
keywords:
---


ファイルまたは標準出力から、正規表現に一致する行を検索して出力するコマンド<br />
<h3>
grep (GNU grep)</h3>
よくつかうオプションと、用途別使用例をまとめておきたい。<br />
<br />
<h3>
オプション</h3>
<table>
<thead>
<tr><th colspan="2">文法の選択</th></tr>
</thead>
<tbody>
<tr><td>-E</td><td>POSIX拡張正規表現</td></tr>
<tr><td>-P</td><td>Perl拡張正規表現</td></tr>
<tr><td>-F</td><td>固定文字列の検索</td></tr>
</tbody>
</table>
文法による違いは、使用できるメタ文字、文字クラス、最長・最短マッチの選択可否など。<br />
文法を指定しなかった場合に適用される「基本正規表現」は下位互換のために残されているそうです。<br />
その時限りの対話処理では、自分もよくオプションを省略しますが<br />
長く使い回すようなスクリプトに記述する場合は明示的に文法を指定した方がよさそうです。<br />
<br />
<table>
<tbody>
<tr><td>-i, --ignore-case</td><td>アルファベットの大文字小文字を区別しない</td></tr>
<tr><td>-v, --invert-match</td><td>パターンと一致しない行を表示する</td></tr>
<tr><td>-R, --recursive</td><td>指定したディレクトリ以下にあるファイルを再帰的に走査する</td></tr>
<tr><td>-l, --files-with-matches</td><td>マッチする行を含むファイル名だけを表示する</td></tr>
<tr><td>-L, --files-without-match</td><td>マッチしなかったファイル名だけを表示する</td></tr>
<tr><td>-c, --count</td><td>マッチした行数だけをファイルごとに表示する</td></tr>
</tbody>
</table>
<br />
用途別使用例<br />
# grep -E "/([^/]+/)+" /etc/httpd/conf/httpd.conf<br />
<br />
参考<br />
<a href="http://linux.die.net/man/7/regex">http://linux.die.net/man/7/regex</a><br />
<a href="https://cims.nyu.edu/cgi-systems/man.cgi?section=3&amp;topic=pcresyntax">https://cims.nyu.edu/cgi-systems/man.cgi?section=3&amp;topic=pcresyntax</a>

