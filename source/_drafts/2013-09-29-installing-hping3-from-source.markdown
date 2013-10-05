---
layout: post
title: "Installing Hping3 from source"
date: 2013-09-29 19:08
comments: true
categories: 
---

[www.hping.org](http://www.hping.org/download.php)から

    [vagrant@localhost hping3-20051105]$ cd /usr/local/src/
    [vagrant@localhost hping3-20051105]$ sudo wget http://www.hping.org/hping3-20051105.tar.gz
    [vagrant@localhost hping3-20051105]$ tar xfz hping3-20051105.tar.gz 
    [vagrant@localhost src]$ sudo tar xfz hping3-20051105.tar.gz
    [vagrant@localhost src]$ cd hping3-20051105


## make 

### libpcap

    main.c:29:18: error: pcap.h: No such file or directory
    main.c:169: error: expected ‘=’, ‘,’, ‘;’, ‘asm’ or ‘__attribute__’ before ‘*’ token
    main.c:170: error: ‘PCAP_ERRBUF_SIZE’ undeclared here (not in a function)
    make: *** [main.o] Error 1

    # yum install libpcap-devel

    libpcap_stuff.c:20:21: error: net/bpf.h: No such file or directory
    libpcap_stuff.c: In function ‘pcap_recv’:
    libpcap_stuff.c:61: warning: pointer targets in assignment differ in signedness
    make: *** [libpcap_stuff.o] Error 1

    # ln -s /usr/include/pcap-bpf.h /usr/include/net/bpf.h
    
    In file included from ars.h:20,
                     from arsglue.c:7:
    bytesex.h:22:3: error: #error can not find the byte order for this architecture, fix bytesex.h
    In file included from arsglue.c:7:
    ars.h:190:2: error: #error "Please, edit Makefile and add -DBYTE_ORDER_(BIG|LITTLE)_ENDIAN"
    ars.h:254:2: error: #error "Please, edit Makefile and add -DBYTE_ORDER_(BIG|LITTLE)_ENDIAN"
    ars.h:323:2: error: #error "Please, edit Makefile and add -DBYTE_ORDER_(BIG|LITTLE)_ENDIAN"
    make: *** [arsglue.o] Error 1
    
    # vi bytesex.h
    
    /usr/bin/ld: cannot find -ltcl
    collect2: ld returned 1 exit status
    make: *** [hping3] Error 1
    
    # yum install tcl-devel
    
    
### success

    ./hping3 -v
    hping version 3.0.0-alpha-1 ($Id: release.h,v 1.4 2004/04/09 23:38:56 antirez Exp $)
    This binary is TCL scripting capable
    use `make strip' to strip hping3 binary
    use `make install' to install hping3
    
    [root@localhost hping3-20051105]# make install
    cp -f hping3 /usr/sbin/
    chmod 755 /usr/sbin/hping3
    ln -s /usr/sbin/hping3 /usr/sbin/hping
    ln -s /usr/sbin/hping3 /usr/sbin/hping2
    @@@@@@ WARNING @@@@@@
    Can't install the man page: /usr/local/man/man8 does not exist
        
    [vagrant@localhost hping3-20051105]$ hping3 -v
    hping version 3.0.0-alpha-1 ($Id: release.h,v 1.4 2004/04/09 23:38:56 antirez Exp $)
    NO TCL scripting support compiled in
    
