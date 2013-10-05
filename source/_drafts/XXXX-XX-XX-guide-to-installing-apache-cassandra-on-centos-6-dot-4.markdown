---
layout: post
title: "Guide to installing Apache Cassandra on CentOS 6.4"
date: 2013-09-02 20:14
comments: true
categories: 
---

### JDKインストール

    [vagrant@localhost ~]$ sudo yum install java-1.7.0-openjdk

### Cassandraインストール

    [vagrant@localhost src]$ sudo wget http://ftp.tsukuba.wide.ad.jp/software/apache/cassandra/1.2.9/apache-cassandra-1.2.9-bin.tar.gz
    --2013-09-02 11:25:14--  http://ftp.tsukuba.wide.ad.jp/software/apache/cassandra/1.2.9/apache-cassandra-1.2.9-bin.tar.gz
    Resolving ftp.tsukuba.wide.ad.jp... 203.178.132.80
    Connecting to ftp.tsukuba.wide.ad.jp|203.178.132.80|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 16720513 (16M) [application/x-gzip]
    Saving to: “apache-cassandra-1.2.9-bin.tar.gz”

    100%[===============================================================================================================================>] 16,720,513  2.58M/s   in 6.3s

    2013-09-02 11:25:26 (2.53 MB/s) - “apache-cassandra-1.2.9-bin.tar.gz” saved [16720513/16720513]

    [vagrant@localhost src]$ sudo tar xfz apache-cassandra-1.2.9-bin.tar.gz
    [vagrant@localhost src]$ cd apache-cassandra-1.2.9

### クライアントの起動

    [vagrant@localhost ~]$ /usr/local/src/apache-cassandra-1.2.9/bin/cassandra-cli
    Connected to: "Test Cluster" on 127.0.0.1/9160
    Welcome to Cassandra CLI version 1.2.9

    Type 'help;' or '?' for help.
    Type 'quit;' or 'exit;' to quit.

    [default@unknown]

### show keyspaces;

    [default@unknown] show keyspaces;

    WARNING: CQL3 tables are intentionally omitted from 'show keyspaces' output.
    See https://issues.apache.org/jira/browse/CASSANDRA-4377 for details.

    Keyspace: system:
      ...
    Keyspace: system_traces:
      Replication Strategy: org.apache.cassandra.locator.SimpleStrategy
      Durable Writes: true
        Options: [replication_factor:1]
      Column Families

### use <keyspace>

    [default@unknown] use system;
    Authenticated to keyspace: system

### 
