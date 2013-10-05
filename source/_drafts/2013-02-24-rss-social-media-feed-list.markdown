---
layout: post
title: "RSS Social Media Feed List"
date: 2013-02-24 21:06
comments: true
categories: 
keywords: RSS Feed, Twitter, Facebook
---

### Twitter

http://api.twitter.com/1/statuses/user_timeline.format 
- [GET statuses/user_timeline | Twitter Developers](https://dev.twitter.com/docs/api/1/get/statuses/user_timeline)


## Facebook Wall RSS



http://www.facebook.com/feeds/page.php?id=20531316728&format=rss20

APIの仕様変更でRSSリーダからタイムラインを読めなくなってた。タイムラインをRSSで取得するURIのメモ。
変更前 "https://twitter.com/statuses/user_timeline/{user_id}.rss"
変更後 "http://api.twitter.com/1/statuses/user_timeline.rss?screen_name={screen_name}"

URLをリンクさせずにツイートする方法 http://arinogotokuatumarite.blog19.fc2.com/blog-entry-220.html