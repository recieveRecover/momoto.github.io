---
layout: post
title: "Tutorial On systemd"
date: 2013-03-22 20:41
comments: true
categories: 
---

[guest@arch-ideapad fuelphp-1.4]$ sudo systemctl enable mysqld
ln -s '/usr/lib/systemd/system/mysqld.service' '/etc/systemd/system/multi-user.target.wants/mysqld.service'

[guest@arch-ideapad fuelphp-1.4]$ sudo systemctl start mysqld.service

[guest@arch-ideapad fuelphp-1.4]$ systemctl status mysqld.service
mysqld.service - MySQL database server
                 Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled)
                   Active: active (running) since 金 2013-03-22 20:47:28 JST; 1min 7s ago
                    Process: 4672 ExecStartPost=/usr/bin/mysqld-post (code=exited, status=0/SUCCESS)
                    Main PID: 4671 (mysqld)
                      CGroup: name=systemd:/system/mysqld.service
                                └─4671 /usr/bin/mysqld --pid-file=/run/mysqld/mysqld.pid

[guest@arch-ideapad fuelphp-1.4]$ sudo systemctl is-enabled mysqld.service
enabled

