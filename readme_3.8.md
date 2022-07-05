#Домашнее задание к занятию "3.8. Компьютерные сети, лекция 3"

#1. Подключитесь к публичному маршрутизатору в интернет. Найдите маршрут к вашему публичному IP
    
    telnet route-views.routeviews.org
    Username: rviews
    show ip route x.x.x.x/32
    show bgp x.x.x.x/32
    
    route-views>sh ip ro 8.8.8.8
    Routing entry for 8.8.8.0/24
    Known via "bgp 6447", distance 20, metric 0
    Tag 2497, type external
    Last update from 202.232.0.2 1w6d ago
    Routing Descriptor Blocks:
    * 202.232.0.2, from 202.232.0.2, 1w6d ago
    Route metric is 0, traffic share count is 1
    AS Hops 2
    Route tag 2497
    MPLS label: none
    route-views>show bgp 8.8.8.8
    BGP routing table entry for 8.8.8.0/24, version 2327039946
    Paths: (22 available, best #20, table default)
    Not advertised to any peer
    Refresh Epoch 1
    20912 15169
    212.66.96.126 from 212.66.96.126 (212.66.96.126)
    Origin IGP, localpref 100, valid, external
    Community: 20912:65016
    path 7FE01D904668 RPKI State valid
    rx pathid: 0, tx pathid: 0
    Refresh Epoch 1
    1351 15169
    132.198.255.253 from 132.198.255.253 (132.198.255.253)
    Origin IGP, localpref 100, valid, external
    path 7FE0949627A0 RPKI State valid
    rx pathid: 0, tx pathid: 0
    Refresh Epoch 1
    3333 15169
    193.0.0.56 from 193.0.0.56 (193.0.0.56)
    Origin IGP, localpref 100, valid, external
    path 7FE0ADF76210 RPKI State valid
    rx pathid: 0, tx pathid: 0
    Refresh Epoch 1
    101 15169
    209.124.176.223 from 209.124.176.223 (209.124.176.223)
    Origin IGP, localpref 100, valid, external
    Community: 101:20400 101:22200
    path 7FE04E63E718 RPKI State valid
    rx pathid: 0, tx pathid: 0
    Refresh Epoch 1
    49788 1299 15169
    91.218.184.60 from 91.218.184.60 (91.218.184.60)
    Origin IGP, localpref 100, valid, external
    Community: 1299:20000
    Extended Community: 0x43:100:1
    path 7FE01D4EBF18 RPKI State valid
    rx pathid: 0, tx pathid: 0
    Refresh Epoch 1
    3267 15169
    194.85.40.15 from 194.85.40.15 (185.141.126.1)
    Origin IGP, metric 0, localpref 100, valid, external
    path 7FE02ED87DE0 RPKI State valid
    rx pathid: 0, tx pathid: 0
    Refresh Epoch 1
    852 15169
    154.11.12.212 from 154.11.12.212 (96.1.209.43)
    Origin IGP, metric 0, localpref 100, valid, external
    path 7FE13D5A2CC8 RPKI State valid
    rx pathid: 0, tx pathid: 0
    --More--
    
#2. Создайте dummy0 интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации.

    femsk@femsk-virtual-machine:~$ sudo modprobe -v dummy numdummies=2
    [sudo] password for femsk:
    insmod /lib/modules/5.13.0-51-generic/kernel/drivers/net/dummy.ko numdummies=0 numdummies=2
    femsk@femsk-virtual-machine:~$ ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
    2: ens160: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 00:50:56:8e:af:ed brd ff:ff:ff:ff:ff:ff
    altname enp3s0
    inet 10.0.22.81/24 brd 10.0.22.255 scope global dynamic noprefixroute ens160
       valid_lft 595158sec preferred_lft 595158sec
    inet6 fe80::f63f:7cd4:c58b:5c8/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
    3: ens1.1000@ens160: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether 00:50:56:8e:af:ed brd ff:ff:ff:ff:ff:ff
    4: dummy0: <BROADCAST,NOARP> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether 56:8e:19:11:1a:9c brd ff:ff:ff:ff:ff:ff
    5: dummy1: <BROADCAST,NOARP> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether 7e:4f:12:d8:be:d1 brd ff:ff:ff:ff:ff:ff
    root@femsk-virtual-machine:~# ip ro add 10.0.35.0/24 via 10.0.22.81
    root@femsk-virtual-machine:~# ip ro add 10.0.36.0/24 dev ^C
    root@femsk-virtual-machine:~# ip ro add 10.0.36.0/24 dev ens160
    root@femsk-virtual-machine:~# routel
         target            gateway          source    proto    scope    dev tbl
    /usr/bin/routel: 48: shift: can't shift that many
        default        10.0.22.254                     dhcp          ens160
     10.0.22.0/ 24                      10.0.22.81   kernel     link ens160
     10.0.35.0/ 24      10.0.22.81                                   ens160
     10.0.36.0/ 24                                              link ens160
    169.254.0.0/ 16                                              link ens160
      10.0.22.0          broadcast      10.0.22.81   kernel     link ens160 local
     10.0.22.81              local      10.0.22.81   kernel     host ens160 local
    10.0.22.255          broadcast      10.0.22.81   kernel     link ens160 local
      127.0.0.0          broadcast       127.0.0.1   kernel     link     lo local
     127.0.0.0/ 8            local       127.0.0.1   kernel     host     lo local
      127.0.0.1              local       127.0.0.1   kernel     host     lo local
    127.255.255.255          broadcast       127.0.0.1   kernel     link     lo local
            ::1                                      kernel              lo
        fe80::/ 64                                   kernel          ens160
            ::1              local                   kernel              lo local
    fe80::f63f:7cd4:c58b:5c8              local                   kernel          ens160 local
    root@femsk-virtual-machine:~#

#3. Проверьте открытые TCP порты в Ubuntu, какие протоколы и приложения используют эти порты? Приведите несколько примеров.

    root@femsk-virtual-machine:~# sudo -i
    root@femsk-virtual-machine:~# ss -lnpt
    State   Recv-Q  Send-Q    Local Address:Port      Peer Address:Port  Process
    LISTEN  0       4096          127.0.0.1:8125           0.0.0.0:*      users:(("netdata",pid=864,fd=62))
    LISTEN  0       4096          127.0.0.1:19999          0.0.0.0:*      users:(("netdata",pid=864,fd=4))
    LISTEN  0       4096      127.0.0.53%lo:53             0.0.0.0:*      users:(("systemd-resolve",pid=660,fd=13))
    LISTEN  0       128             0.0.0.0:22             0.0.0.0:*      users:(("sshd",pid=809,fd=3))
    LISTEN  0       5             127.0.0.1:631            0.0.0.0:*      users:(("cupsd",pid=104337,fd=7))
    LISTEN  0       4096                  *:9100                 *:*      users:(("node_exporter",pid=865,fd=3))
    LISTEN  0       128                [::]:22                [::]:*      users:(("sshd",pid=809,fd=4))
    LISTEN  0       5                 [::1]:631               [::]:*      users:(("cupsd",pid=104337,fd=6))
    root@femsk-virtual-machine:~#

#4. Проверьте используемые UDP сокеты в Ubuntu, какие протоколы и приложения используют эти порты?

    root@femsk-virtual-machine:~# ss -lpnu
    State   Recv-Q  Send-Q    Local Address:Port      Peer Address:Port  Process
    UNCONN  0       0               0.0.0.0:5353           0.0.0.0:*      users:(("avahi-daemon",pid=709,fd=12))
    UNCONN  0       0               0.0.0.0:44656          0.0.0.0:*      users:(("avahi-daemon",pid=709,fd=14))
    UNCONN  0       0               0.0.0.0:631            0.0.0.0:*      users:(("cups-browsed",pid=104339,fd=7))
    UNCONN  0       0             127.0.0.1:8125           0.0.0.0:*      users:(("netdata",pid=864,fd=41))
    UNCONN  0       0         127.0.0.53%lo:53             0.0.0.0:*      users:(("systemd-resolve",pid=660,fd=12))
    UNCONN  0       0                  [::]:5353              [::]:*      users:(("avahi-daemon",pid=709,fd=13))
    UNCONN  0       0                  [::]:56326             [::]:*      users:(("avahi-daemon",pid=709,fd=15))
    root@femsk-virtual-machine:~#

#5. Используя diagrams.net, создайте L3 диаграмму вашей домашней сети или любой другой сети, с которой вы работали.

    






