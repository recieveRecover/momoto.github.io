---
layout: post
title: "Google Feed API Tutorial"
date: 2013-02-24 22:50
comments: true
categories: 
keywords:
---

<h3>Feed</h3>
<pre>
script type="text/javascript" src="https://www.google.com/jsapi"/script
script type="text/javascript"
function feedrender(url) {
  var feed = new google.feeds.Feed(url);
  feed.load(function(result) {
    if (!result.error) {
      var container = document.getElementById("feed");
      for (var i = 0; i < result.feed.entries.length; i++) {
        var entry = result.feed.entries[i];
        console.log(entry);
      }
    }
  });
}
function initialize() {
  feedrender("http://example.com/rss.xml");
}
google.setOnLoadCallback(initialize);
/script
</pre>

### references
- https://developers.google.com/feed/v1/reference

