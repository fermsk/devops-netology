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


#5. Сколько IP адресов в сети с маской /29 ? Сколько /29 подсетей можно получить из сети с маской /24. Приведите несколько примеров /29 подсетей внутри сети 10.10.10.0/24.


#6. Задача: вас попросили организовать стык между 2-мя организациями. Диапазоны 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 уже заняты. Из какой подсети допустимо взять частные IP адреса? Маску выберите из расчета максимум 40-50 хостов внутри подсети.


#7. Как проверить ARP таблицу в Linux, Windows? Как очистить ARP кеш полностью? Как из ARP таблицы удалить только один нужный IP?



