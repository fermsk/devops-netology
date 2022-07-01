#Домашнее задание к занятию "3.7. Компьютерные сети, лекция 2"

#1. Проверьте список доступных сетевых интерфейсов на вашем компьютере. Какие команды есть для этого в Linux и в Windows?
    
    femsk@femsk-virtual-machine:~$ ip -br link show
    lo               UNKNOWN        00:00:00:00:00:00 <LOOPBACK,UP,LOWER_UP>
    ens160           UP             00:50:56:8e:af:ed <BROADCAST,MULTICAST,UP,LOWER_UP>
    femsk@femsk-virtual-machine:~$ ip link show
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    2: ens160: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 00:50:56:8e:af:ed brd ff:ff:ff:ff:ff:ff
    altname enp3s0
    femsk@femsk-virtual-machine:~$ netstat -i
    Kernel Interface table
    Iface      MTU    RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
    ens160    1500  2674859      0   3723 0        135162      0      0      0 BMRU
    lo       65536    13495      0      0 0         13495      0      0      0 LRU
    femsk@femsk-virtual-machine:~$ nmcli device sta
    DEVICE  TYPE      STATE      CONNECTION
    ens160  ethernet  connected  Wired connection 1
    lo      loopback  unmanaged  --

![image](https://user-images.githubusercontent.com/104899352/176841383-4df004cb-67d9-4133-8616-be39cff85884.png)

#2. Какой протокол используется для распознавания соседа по сетевому интерфейсу? Какой пакет и команды есть в Linux для этого?

    Используются протоколы обнаружения канального уровня реализованные по стандарту IEEE 802.1AB
    Протоколы обнаружения канального уровня:
    *LLDP (Link Layer Discovery Protocol)
    *CDP (Cisco Discovery Protocol)
    *FDP (Foundry Discovery Protocol)
    *NDP (Nortel Discovery Protocol)
    *EDP (Extreme Discovery Protocol)
    *LLTD (Link Layer Topology Discovery)
    
    В Linux используется пакет lldpd:
    
    femsk@femsk-virtual-machine:~$ lldpcli
    [lldpcli] $ sho nei
    -------------------------------------------------------------------------------
    LLDP neighbors:
    -------------------------------------------------------------------------------
    [lldpcli] $ h

    -- Help
      show  Show running system information
     watch  Monitor neighbor changes
    update  Update information and send LLDPU on all ports
      help  Get help on a possible command
     pause  Pause lldpd operations
    resume  Resume lldpd operations
      exit  Exit interpreter

    [lldpcli] $


#3. Какая технология используется для разделения L2 коммутатора на несколько виртуальных сетей? Какой пакет и команды есть в Linux для этого? Приведите пример конфига.

    Используется технология VLAN стандарта IEEE 802.1Q
    Используются пакеты iproute2, NetworkManager, vconfig, vlan.
    Пример настройки vlan посредством ip:
    
    femsk@femsk-virtual-machine:~$ lsmod | grep 8021q
    8021q                  32768  0
    garp                   16384  1 8021q
    mrp                    20480  1 8021q
    femsk@femsk-virtual-machine:~$ ip -br l
    lo               UNKNOWN        00:00:00:00:00:00 <LOOPBACK,UP,LOWER_UP>
    ens160           UP             00:50:56:8e:af:ed <BROADCAST,MULTICAST,UP,LOWER_UP>
    femsk@femsk-virtual-machine:~$ sudo ip link add link ens160 name ens1.1000 type vlan id 1000
    femsk@femsk-virtual-machine:~$ ip -br l
    lo               UNKNOWN        00:00:00:00:00:00 <LOOPBACK,UP,LOWER_UP>
    ens160           UP             00:50:56:8e:af:ed <BROADCAST,MULTICAST,UP,LOWER_UP>
    ens1.1000@ens160 DOWN           00:50:56:8e:af:ed <BROADCAST,MULTICAST>
    femsk@femsk-virtual-machine:~$

![image](https://user-images.githubusercontent.com/104899352/176846494-19d13236-5638-466a-b11d-62419e56434b.png)

#4. Какие типы агрегации интерфейсов есть в Linux? Какие опции есть для балансировки нагрузки? Приведите пример конфига.

    Режимы агрегации интерфейсов в Linux:

    Mode-0(balance-rr) – Данный режим используется по умолчанию. Balance-rr обеспечивается балансировку нагрузки и отказоустойчивость. В данном режиме сетевые пакеты отправляются “по кругу”, от первого интерфейса к последнему. Если выходят из строя интерфейсы, пакеты отправляются на остальные оставшиеся. Дополнительной настройки коммутатора не требуется при нахождении портов в одном коммутаторе. При разностных коммутаторах требуется дополнительная настройка.

    Mode-1(active-backup) – Один из интерфейсов работает в активном режиме, остальные в ожидающем. При обнаружении проблемы на активном интерфейсе производится             переключение на ожидающий интерфейс. Не требуется поддержки от коммутатора.

    Mode-2(balance-xor) – Передача пакетов распределяется по типу входящего и исходящего трафика по формуле ((MAC src) XOR (MAC dest)) % число интерфейсов. Режим дает     балансировку нагрузки и отказоустойчивость. Не требуется дополнительной настройки коммутатора/коммутаторов.

    Mode-3(broadcast) – Происходит передача во все объединенные интерфейсы, тем самым обеспечивая отказоустойчивость. Рекомендуется только для использования MULTICAST     трафика.

    Mode-4(802.3ad) – динамическое объединение одинаковых портов. В данном режиме можно значительно увеличить пропускную способность входящего так и исходящего             трафика. Для данного режима необходима поддержка и настройка коммутатора/коммутаторов.

    Mode-5(balance-tlb) – Адаптивная балансировки нагрузки трафика. Входящий трафик получается только активным интерфейсом, исходящий распределяется в зависимости от       текущей загрузки канала каждого интерфейса. Не требуется специальной поддержки и настройки коммутатора/коммутаторов.

    Mode-6(balance-alb) – Адаптивная балансировка нагрузки. Отличается более совершенным алгоритмом балансировки нагрузки чем Mode-5). Обеспечивается балансировку         нагрузки как исходящего, так и входящего трафика. Не требуется специальной поддержки и настройки коммутатора/коммутаторов.
    
    Пример конфигурационного файла /etc/network/interfaces:

    auto bond0
    iface bond0 inet static
    bond-slaves wlan0 ens160
    bond-mode active-backup
    bond-primary ens160
    bond-miimon 100
    address <ipv4address>/<maskbits>
    gateway <ipv4address>

    allow-bond0 ens160
    iface eth0 inet manual

    allow-bond0 wlan0
    iface wlan0 inet manual
    #    bond-give-a-chance 10
    wpa-bridge bond0
    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf

#5. Сколько IP адресов в сети с маской /29 ? Сколько /29 подсетей можно получить из сети с маской /24. Приведите несколько примеров /29 подсетей внутри сети 10.10.10.0/24.

    6 хостов:
    femsk@femsk-virtual-machine:~$ ipcalc 10.10.10.0/29
    Address:   10.10.10.0           00001010.00001010.00001010.00000 000
    Netmask:   255.255.255.248 = 29 11111111.11111111.11111111.11111 000
    Wildcard:  0.0.0.7              00000000.00000000.00000000.00000 111
    =>
    Network:   10.10.10.0/29        00001010.00001010.00001010.00000 000
    HostMin:   10.10.10.1           00001010.00001010.00001010.00000 001
    HostMax:   10.10.10.6           00001010.00001010.00001010.00000 110
    Broadcast: 10.10.10.7           00001010.00001010.00001010.00000 111
    Hosts/Net: 6                     Class A, Private Internet

    Из сети с маской /24 можно получить 32 подсети /29:
    femsk@femsk-virtual-machine:~$ ipcalc 10.10.10.0/24 /29
    Address:   10.10.10.0           00001010.00001010.00001010. 00000000
    Netmask:   255.255.255.0 = 24   11111111.11111111.11111111. 00000000
    Wildcard:  0.0.0.255            00000000.00000000.00000000. 11111111
    =>
    Network:   10.10.10.0/24        00001010.00001010.00001010. 00000000
    HostMin:   10.10.10.1           00001010.00001010.00001010. 00000001
    HostMax:   10.10.10.254         00001010.00001010.00001010. 11111110
    Broadcast: 10.10.10.255         00001010.00001010.00001010. 11111111
    Hosts/Net: 254                   Class A, Private Internet

    Subnets after transition from /24 to /29

    Netmask:   255.255.255.248 = 29 11111111.11111111.11111111.11111 000
    Wildcard:  0.0.0.7              00000000.00000000.00000000.00000 111

     1.
    Network:   10.10.10.0/29        00001010.00001010.00001010.00000 000
    HostMin:   10.10.10.1           00001010.00001010.00001010.00000 001
    HostMax:   10.10.10.6           00001010.00001010.00001010.00000 110
    Broadcast: 10.10.10.7           00001010.00001010.00001010.00000 111
    Hosts/Net: 6                     Class A, Private Internet

     2.
    Network:   10.10.10.8/29        00001010.00001010.00001010.00001 000
    HostMin:   10.10.10.9           00001010.00001010.00001010.00001 001
    HostMax:   10.10.10.14          00001010.00001010.00001010.00001 110
    Broadcast: 10.10.10.15          00001010.00001010.00001010.00001 111
    Hosts/Net: 6                     Class A, Private Internet
    .....
     32.
    Network:   10.10.10.248/29      00001010.00001010.00001010.11111 000
    HostMin:   10.10.10.249         00001010.00001010.00001010.11111 001
    HostMax:   10.10.10.254         00001010.00001010.00001010.11111 110
    Broadcast: 10.10.10.255         00001010.00001010.00001010.11111 111
    Hosts/Net: 6                     Class A, Private Internet


    Subnets:   32
    Hosts:     192
    femsk@femsk-virtual-machine:~$


#6. Задача: вас попросили организовать стык между 2-мя организациями. Диапазоны 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 уже заняты. Из какой подсети допустимо взять частные IP адреса? Маску выберите из расчета максимум 40-50 хостов внутри подсети.

    От 100.64.0.0 до 100.127.255.255 с маской подсети 255.192.0.0 или /10; данная подсеть рекомендована согласно rfc6598 для использования в качестве адресов для CGN (Carrier-Grade NAT)
    femsk@femsk-virtual-machine:~$ ipcalc 100.64.0.0/26
    Address:   100.64.0.0           01100100.01000000.00000000.00 000000
    Netmask:   255.255.255.192 = 26 11111111.11111111.11111111.11 000000
    Wildcard:  0.0.0.63             00000000.00000000.00000000.00 111111
    =>
    Network:   100.64.0.0/26        01100100.01000000.00000000.00 000000
    HostMin:   100.64.0.1           01100100.01000000.00000000.00 000001
    HostMax:   100.64.0.62          01100100.01000000.00000000.00 111110
    Broadcast: 100.64.0.63          01100100.01000000.00000000.00 111111
    Hosts/Net: 62                    Class A

#7. Как проверить ARP таблицу в Linux, Windows? Как очистить ARP кеш полностью? Как из ARP таблицы удалить только один нужный IP?

    femsk@femsk-virtual-machine:~$ ip nei
    10.0.22.171 dev ens160 lladdr 00:50:56:8e:c3:48 STALE
    10.0.22.87 dev ens160 lladdr 1c:1b:0d:65:78:24 REACHABLE
    10.0.22.xx dev ens160 lladdr 4c:00:82:14:49:5e STALE
    10.0.22.248 dev ens160 lladdr 00:0c:29:c1:75:a0 STALE
    femsk@femsk-virtual-machine:~$ arp -a
    vmsrv-dhcp-02.aladdin.ru (10.0.22.171) at 00:50:56:8e:c3:48 [ether] on ens160
    wks90.aladdin.ru (10.0.22.87) at 1c:1b:0d:65:78:24 [ether] on ens160
    _gateway (10.0.22.xx) at 4c:00:82:14:49:5e [ether] on ens160
    xxxx.ru (10.0.22.248) at 00:0c:29:c1:75:a0 [ether] on ens160
    femsk@femsk-virtual-machine:~$
    
    Очистка ARP:
    femsk@femsk-virtual-machine:~$ sudo ip nei flush dev ens160
    
    Удаление адреса:
    ip neigh del {IP} dev {DEVICE}
    
    Windows:
    
    arp -a
    
    Очистка ARP:
    
    netsh interface ip delete arpcache
    arp -d IP
    
    
    
    



