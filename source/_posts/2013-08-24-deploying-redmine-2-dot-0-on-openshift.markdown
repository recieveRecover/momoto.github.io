---
layout: post
title: "Red Hat OpenShiftにRedmine 2.0を展開する"
date: 2013-08-24 12:06
comments: true
categories: [クラウドコンピューティング, OpenShift, Ruby]
keywords: Red Hat OpenShift, rhc, Ruby, Redmine
description: "Red Hat OpenShiftにプロジェクト管理ソフトウェアのRedmine 2.0を展開します。"
breadcrumb:
  - title: Blog
    url: /
  - title: クラウドコンピューティング
    url: /blog/categories/kuraudokonpiyuteingu/

---

　Red Hat OpenShiftにプロジェクト管理ソフトウェアのRedmine 2.0を展開します。rhcとredmine-2.0-openshift-quickstartを使用しています。<!--more-->
あらかじめOpenShiftのアカウントを作成している必要があります。

 1. #### Ruby 1.9のアプリケーションを作成する

    　`rhc app create`でアプリケーションを作成します。-aにはアプリケーション名、-tにはウェブカートリッジを指定します。

        $ rhc app create -a redmine -t ruby-1.9
        Application Options
        -------------------
          Namespace:  momoto
          Cartridges: ruby-1.9
          Gear Size:  default
          Scaling:    no

        Creating application 'redmine' ... done

        Waiting for your DNS name to be available ... done

        Cloning into 'redmine'...
        Checking connectivity... done

        Your application 'redmine' is now available.

          URL:        http://redmine-momoto.rhcloud.com/
          SSH to:     *
          Git remote: *
          Cloned to:  ~/Workspace/redmine

        Run 'rhc show-app redmine' for more details about your app.

    　アプリケーションの作成と同時に、Gitリポジトリがローカルにクローンされています。

 2. #### MySQL Database 5.1カートリッジを追加する

    　`rhc cartridge add`でMySQL Database 5.1アドオンカートリッジを追加します。追加できるカートリッジは`rhc cartridges`または`rhc cartridge list`で確認できます。

        $ rhc cartridge add -a redmine -c mysql-5.1
        Adding mysql-5.1 to application 'redmine' ... done

        mysql-5.1 (MySQL Database 5.1)
        ------------------------------
          Gears:          Located with ruby-1.9
          Connection URL: mysql://$OPENSHIFT_MYSQL_DB_HOST:$OPENSHIFT_MYSQL_DB_PORT/
          Database Name:  *
          Password:       *
          Username:       *

        MySQL 5.1 database added.  Please make note of these credentials:
               Root User: *
           Root Password: *
           Database Name: *
        Connection URL: mysql://$OPENSHIFT_MYSQL_DB_HOST:$OPENSHIFT_MYSQL_DB_PORT/
        You can manage your new MySQL database by also embedding phpmyadmin-3.4.
        The phpmyadmin username and password will be the same as the MySQL credentials above.

 3. #### redmine-2.0-openshift-quickstartを併合する

     1. 　OpenShiftからクローンしたローカルリポジトリにワーキングディレクトリをうつします
     2. 　GitHubの[redmine-2.0-openshift-quickstart](https://github.com/openshift/redmine-2.0-openshift-quickstart)を`upstream`としてリモートリポジトリに追加します
     3. 　ローカルリポジトリにupstreamを併合します（MERGE STRATEGYはrecursive、recursive strategyのオプションはtheirs）

    　もし、OpenShiftからリポジトリをクローンしなおす場合は`rhc git-clone <app>`をつかいます。

        $ cd redmine/
        $ git remote add upstream -m master git://github.com/openshift/redmine-2.0-openshift-quickstart.git
        $ git pull -s recursive -X theirs upstream master
        warning: no common commits
        remote: Counting objects: 2003, done.
        remote: Compressing objects: 100% (1725/1725), done.
        remote: Total 2003 (delta 300), reused 1897 (delta 199)
        Receiving objects: 100% (2003/2003), 4.06 MiB | 272.00 KiB/s, done.
        Resolving deltas: 100% (300/300), done.
        From git://github.com/openshift/redmine-2.0-openshift-quickstart
         * branch            master     -> FETCH_HEAD
        Auto-merging config.ru
        Auto-merging README.md
        Auto-merging .openshift/cron/weekly/jobs.allow
        Auto-merging .openshift/cron/README.cron
        ...

 4. #### OpenShiftに展開する

    　redmine-2.0-openshift-quickstartと併合したローカルリポジトリを、OpenShiftのリモートリポジトリ（origin）に更新します。

        $ git push origin master
        Counting objects: 2011, done.
        Delta compression using up to 2 threads.
        Compressing objects: 100% (1625/1625), done.
        Writing objects: 100% (2001/2001), 4.06 MiB | 1.58 MiB/s, done.
        Total 2001 (delta 307), reused 1989 (delta 300)
        remote: Stopping Ruby cart
        remote: Running build on Ruby cart
        remote: Bundling RubyGems based on Gemfile/Gemfile.lock to repo/vendor/bundle with 'bundle install --deployment'
        remote: Fetching gem metadata from http://rubygems.org/.........
        ...

    　`git push`の処理が終わったら、ウェブブラウザからアプリケーションのURLへアクセスして動作を確認します。アプリケーションのURLは`rhc show-app <app>`からも確認できます。

    {% img /blog/images/2013-08-24-deploying-redmine-2-dot-0-on-openshift/redmine-home.png 500 Red Hat OpenShiftにRedmine 2.0を展開する %}

　OpenShiftではRedmineのほか、[Django](https://www.openshift.com/quickstarts/django)、[CakePHP](https://www.openshift.com/quickstarts/cakephp)、[WordPress 3.x](https://www.openshift.com/quickstarts/wordpress-3x)などのクイックスタートも用意されています。

## 参考

- [Understanding OpenShift | OpenShift by Red Hat](https://www.openshift.com/developers/documentation)
- [openshift (OpenShift Origin) · GitHub](https://github.com/openshift)
  - [openshift/redmine-2.0-openshift-quickstart](https://github.com/openshift/redmine-2.0-openshift-quickstart)

## 関連記事

- [Red Hat OpenShiftクライアントツールをインストールする](/blog/2013/08/24/installation-guide-for-openshift-rhc-client-tools/)
- [HerokuにRedmine 2.3.1を展開する](/blog/2013/05/31/deploy-redmine-2-dot-3-1-on-heroku/)

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_top&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=seijimomotobl-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4822211983" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_top&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=seijimomotobl-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4774155934" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
