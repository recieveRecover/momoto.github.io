---
layout: post
title: "HerokuにRedmine 2.3.1を展開する"
date: 2013-05-31 23:19
comments: true
categories: [クラウドコンピューティング, Heroku, Ruby]
keywords: Heroku, Ruby, Redmine
description: "Herokuが提供するPaaSにプロジェクト管理ソフトウェア Redmine 2.3.1を展開します。"
breadcrumb:
  - title: Blog
    url: /
  - title: クラウドコンピューティング
    url: /blog/categories/kuraudokonpiyuteingu/

---

　Herokuが提供するPaaSにプロジェクト管理ソフトウェア Redmine 2.3.1を展開します。
Redmineのバージョンは2.3.1、Rubyは1.9.2-p320、[heroku-toolbelt](https://aur.archlinux.org/packages/heroku-client/)は2.39.4を使用しました。<!-- more -->

#### Redmine 2.3.1のダウンロード

2013.05.31の時点で最新版である2.3.1をGitでクローンします。また、productionというブランチを作成し、チェックアウトします。

    $ git clone git://github.com/redmine/redmine.git

      Cloning into 'redmine'...
      remote: Counting objects: 101645, done.
      remote: Compressing objects: 100% (23821/23821), done.
      remote: Total 101645 (delta 79311), reused 98395 (delta 76195)
      Receiving objects: 100% (101645/101645), 25.39 MiB | 256.00 KiB/s, done.
      Resolving deltas: 100% (79311/79311), done.

    $ cd redmine
    $ git checkout -b production

      Switched to a new branch 'production'

#### Rubyの確認

ここではRubyのバージョン管理にrbenvを使用しています。また、インストールの過程でBundlerが必要になります。

    $ rbenv local 1.9.2-p320
    $ ruby --version

      ruby 1.9.2p320 (2012-04-20 revision 35421) [x86_64-linux]

    $ gem install bundler

      Successfully installed bundler-1.3.5
      1 gem installed
      Installing ri documentation for bundler-1.3.5...
      Building YARD (yri) index for bundler-1.3.5...
      Installing RDoc documentation for bundler-1.3.5...

#### .gitignoreの編集

次の記述を削除します。

    -/config/configuration.yml
    -/config/email.yml
    -/config/initializers/session_store.rb
    -/config/initializers/secret_token.r

    -/public/plugin_assets

    -/Gemfile.lock
    -/Gemfile.local

#### Gemfileの編集

次のように書き換えました。

    source 'https://rubygems.org'

    gem "rails", "3.2.13"
    gem "jquery-rails", "~> 2.0.2"
    gem "i18n", "~> 0.6.0"
    gem "coderay", "~> 1.0.6"
    gem "fastercsv", "~> 1.5.0", :platforms => [:mri_18, :mingw_18, :jruby]
    gem "builder", "3.0.0"

    group :ldap do
      gem "net-ldap", "~> 0.3.1"
    end

    group :openid do
      gem "ruby-openid", "~> 2.1.4", :require => "openid"
      gem "rack-openid"
    end

    platforms :mri, :mingw do
      group :rmagick do
        gem "rmagick", ">= 2.0.0"
      end
    end

    platforms :mri, :mingw do  
      group :postgresql do  
        gem "pg", ">= 0.11.0"  
      end  
    end  

    platforms :jruby do  
      gem "jruby-openssl"  

      group :postgresql do  
        gem "activerecord-jdbcpostgresql-adapter"  
      end  
    end

    group :development do
      gem "rdoc", ">= 2.4.2"
      gem "yard"
    end

    group :test do
      gem "shoulda", "~> 3.3.2"
      gem "mocha", ">= 0.14", :require => 'mocha/api'
      if RUBY_VERSION >= '1.9.3'
        gem "capybara", "~> 2.1.0"
        gem "selenium-webdriver"
      end
    end

    local_gemfile = File.join(File.dirname(__FILE__), "Gemfile.local")
    if File.exists?(local_gemfile)
      puts "Loading Gemfile.local ..." if $DEBUG # `ruby -d` or `bundle -v`
      instance_eval File.read(local_gemfile)
    end

    # Load plugins' Gemfiles
    Dir.glob File.expand_path("../plugins/*/Gemfile", __FILE__) do |file|
      puts "Loading #{file} ..." if $DEBUG # `ruby -d` or `bundle -v`
      #TODO: switch to "eval_gemfile file" when bundler >= 1.2.0 will be required (rails 4)
      instance_eval File.read(file), file
    end

#### config/application.rbの編集

次の記述を追記しました。

    +    config.assets.initialize_on_precompile = false

#### config/environment.rbの編集

次の記述をコメントアウトします。

    -vendor_plugins_dir = File.join(Rails.root, "vendor", "plugins")
    -if Dir.glob(File.join(vendor_plugins_dir, "*")).any?
    -  $stderr.puts "Plugins in vendor/plugins (#{vendor_plugins_dir}) are no longer allowed. " +
    -    "Please, put your Redmine plugins in the `plugins` directory at the root of your " +
    -    "Redmine directory (#{File.join(Rails.root, "plugins")})"
    -  exit 1
    -end
    +#vendor_plugins_dir = File.join(Rails.root, "vendor", "plugins")
    +#if Dir.glob(File.join(vendor_plugins_dir, "*")).any?
    +#  $stderr.puts "Plugins in vendor/plugins (#{vendor_plugins_dir}) are no longer allowed. " +
    +#    "Please, put your Redmine plugins in the `plugins` directory at the root of your " +
    +#    "Redmine directory (#{File.join(Rails.root, "plugins")})"
    +#  exit 1
    +#end

#### Gemパッケージのインストール

必要なGemパッケージをインストールします。config/database.ymlの設定を促されていますが、現時点では設定しません。

    $ bundle install

      Please configure your config/database.yml first
      Fetching gem metadata from https://rubygems.org/.........
      Resolving dependencies...
      ...
      Your bundle is complete!

セッションストアを生成します。

    $ bundle exec rake generate_secret_token

      Please configure your config/database.yml first
      Please configure your config/database.yml first

#### アプリケーションの作成

Herokuツールベルトを使用して、アプリケーションを作成します。ツールベルトと同様に、Herokuアカウントの登録が必要です。

    $ heroku login

      Enter your Heroku credentials.
      Email: ********
      Password (typing will be hidden): 
      Authentication successful.

    $ heroku create [NAME]

      Creating [NAME]... done, stack is cedar
      http://[NAME].herokuapp.com/ | git@heroku.com:[NAME].git
      Git remote heroku added

#### Herokuへ配置

Gitを使用します。Pushの時点でconfig/database.ymlが自動的に作られるようです。

    $ git add .
    $ git commit -m "init"
    $ git push heroku production:master

      -----> Ruby/Rails app detected
      -----> Installing dependencies using Bundler version 1.3.2
      -----> Writing config/database.yml to read from DATABASE_URL
      -----> Preparing app for Rails asset pipeline

      -----> Launching... done, v8

#### テーブルやデフォルトデータの用意

言語を選択するプロンプトが現れます。

    $ heroku run rake db:migrate

      Connecting to database specified by DATABASE_URL
      Creating scope :system. Overwriting existing method Enumeration.system.
      ...

    $ heroku run rake redmine:load_default_data

      Connecting to database specified by DATABASE_URL
      Creating scope :system. Overwriting existing method Enumeration.system.

      Select language: ar, az, bg, bs, ca, cs, da, de, el, en, en-GB, es, et, eu, fa, fi, fr, gl, he, hr, hu, id, it, ja, ko, lt, lv, mk, mn, nl, no, pl, pt, pt-BR, ro, ru, sk, sl, sq, sr, sr-YU, sv, th, tr, uk, vi, zh, zh-TW [en] ja
      ====================================
      Default configuration data loaded.

#### 動作確認

HerokuのアプリケーションURLへアクセスして、動作を確認します。

{% img /blog/images/2013-05-31-deploy-redmine-2-dot-3-1-on-heroku/01.png 500 %}

#### 参考

- [Redmine](https://github.com/redmine/redmine)
- [Redmineのインストール &mdash; Redmine Guide 日本語訳](http://redmine.jp/guide/RedmineInstall/)
- [Getting Started with Rails 3.x on Heroku | Heroku Dev Center](https://devcenter.heroku.com/articles/rails3)

#### 関連記事

- [Red Hat OpenShiftにRedmine 2.0を展開する](/blog/2013/08/24/deploying-redmine-2-dot-0-on-openshift/)
