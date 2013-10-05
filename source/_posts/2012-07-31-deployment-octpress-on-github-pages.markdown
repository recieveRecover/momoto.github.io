---
layout: post
title: "OctopressをGithub pagesにデプロイ"
date: 2012-07-31 10:14
comments: true
categories: [Ruby, Git]
keywords: Ruby, GitHub, Octopress
description: "OctopressをGithub pagesにデプロイした際の手順の記録です。"

---

#### 要件
- github
- ruby-1.9.2-p290
- (rbenv and ruby-build) || rvm

#### Octopressのセットアップ
    $ git clone git://github.com/imathis/octopress.git octopress
    $ cd octopress/
    
    $ gem install bundler
    $ bundle install
    
    $ rake install
    $ rake setup_github_pages
    
      Enter the read/write url for your repository
      (For example, 'git@github.com:your_username/your_username.github.com)
      Repository url: 
    
    $ rake generate
    $ rake deploy

#### "source"ブランチに、ブログのソースを残す
    $ git add .
    $ git commit -m 'your message'
    $ git push origin source

#### 記事を投稿
$ rake new_post["hello, world"]
$ vi source/_posts/YYYY-MM-DD-hello.world.markdown

    ---
    layout: post
    title: "hello, world"
    date: YYYY-MM-DD hh:mm
    comments: true
    categories:
    ---
    
    hello, world 

$ rake generate
$ rake deploy

#### 参考
- [Octopress Setup - Octopress](http://octopress.org/docs/setup/)
- [Deploying to Github Pages - Octopress](http://octopress.org/docs/deploying/github/)
- [Blogging Basics - Octopress](http://octopress.org/docs/blogging/)

