---
layout: post
title: "FuelPHP Restコントローラの使い方"
date: 2013-03-03 23:38
comments: true
categories: [PHP]
keywords: RESTful, FuelPHP, Controller_Rest
description: "FuelPHPのRESTコントローラ導入の手引きです。"

---

　FuelPHPのRestコントローラを試用してみます。クラスファイルは、通常のコントローラと同様に`fuel/app/classes/controller`へ設置して使用します。まずは次のような内容でapi.phpを作成し、ブラウザから出力を確認してみます。Restコントローラを使用する場合、継承するクラスはControllerではなく`Controller_Rest`になります。
    <?php
    class Controller_Api extends Controller_Rest {
      public function action_range() {
        $this->response(array(1,2,3));
      }
    }
<!-- more -->
　ブラウザから`http://localhost/api/range`へアクセスをしてみると、responseメソッドへ引数として渡した配列がPHPが整形する形式（Content-type:text/html）で出力されます。
    array(3) {
      [0]=>
      string(1) "1"
      [1]=>
      string(1) "2"
      [2]=>
      string(1) "3"
    }
　このままでは他のプログラムからデータを読み込めんでもらえないので、JSONやXMLなどの一般的な形式で出力するため、次のプロパティを加えて出力形式を指定します。
    <?php
    class Controller_Api extends Controller_Rest {
      protected $format = 'json';
      public function post_q() {
        $this->response(array(1,2,3));
      }
    }
    ---
    Content-Type:application/json
    
    [1,2,3]
　そうするとHTTP応答ヘッダが変わり、出力もJSON形式へと変換されます。PHPの定義済み変数である$_SERVERを、XML形式で出力してみると次のようになります。
    <?php
    class Controller_Api extends Controller_Rest {
      protected $format = 'xml';
      public function action_server() {
        $this->response($_SERVER);
      }
    }
    ---
    Content-Type:application/xml

    <xml>
    <SERVER_SOFTWARE>PHP 5.4.12 Development Server</SERVER_SOFTWARE>
    <SERVER_PROTOCOL>HTTP/1.1</SERVER_PROTOCOL>
    <SERVER_NAME>localhost</SERVER_NAME>
    <SERVER_PORT>8000</SERVER_PORT>
    <REQUEST_URI>/api/server</REQUEST_URI>
    <REQUEST_METHOD>GET</REQUEST_METHOD>
    <HTTP_HOST>localhost:8000</HTTP_HOST>
    <HTTP_USER_AGENT>Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.22 (KHTML, like Gecko) Chrome/25.0.1364.97 Safari/537.22</HTTP_USER_AGENT>
    ...(略)
　FuelPHP 1.4で対応されている形式については`fuel/core/classes/controller/rest.php`の`$_supported_formats`変数に設定されています。
    protected $_supported_formats = array(
            'xml' => 'application/xml',
            'rawxml' => 'application/xml',
            'json' => 'application/json',
            'jsonp'=> 'text/javascript',
            'serialized' => 'application/vnd.php.serialized',
            'php' => 'text/plain',
            'html' => 'text/html',
            'csv' => 'application/csv',
    );
