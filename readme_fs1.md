#Домашнее задание к занятию "3.5. Файловые системы"
__________________________________________________________________________________________
#1. Узнайте о sparse (разряженных) файлах.

        Ознакомился:
        https://habr.com/ru/company/hetmansoftware/blog/553474/

#2. Могут ли файлы, являющиеся жесткой ссылкой на один объект, иметь разные права доступа и владельца? Почему?

    hardlink -это ссылка на тот же самый файл и имеет тот же inode, владелец и права будут соответственно аналогичными.
    femsk@femsk-virtual-machine:~$ cd ./Netology/
    femsk@femsk-virtual-machine:~/Netology$ touch hardlink.hl
    femsk@femsk-virtual-machine:~/Netology$ ln hardlink.hl hardlink.hl_link
    femsk@femsk-virtual-machine:~/Netology$ ls -ilh
    total 12K
    1443159 -rw-rw-r-- 1 femsk femsk 12K июн 21 16:20 file.new
    1443075 -rw-rw-r-- 2 femsk femsk   0 июн 22 09:00 hardlink.hl
    1443075 -rw-rw-r-- 2 femsk femsk   0 июн 22 09:00 hardlink.hl_link
    femsk@femsk-virtual-machine:~/Netology$ chmod 0555 hardlink.hl
    femsk@femsk-virtual-machine:~/Netology$ ls -ilh
    total 12K
    1443159 -rw-rw-r-- 1 femsk femsk 12K июн 21 16:20 file.new
    1443075 -r-xr-xr-x 2 femsk femsk   0 июн 22 09:00 hardlink.hl
    1443075 -r-xr-xr-x 2 femsk femsk   0 июн 22 09:00 hardlink.hl_link
    femsk@femsk-virtual-machine:~/Netology$


#3. Сделайте vagrant destroy на имеющийся инстанс Ubuntu. Замените содержимое Vagrantfile следующим:

    !!!В офисе использую VMware, машина для выполнения заданий развернута в нём!!!
    
    Vagrant.configure("2") do |config|
      config.vm.box = "bento/ubuntu-20.04"
      config.vm.provider :virtualbox do |vb|
        lvm_experiments_disk0_path = "/tmp/lvm_experiments_disk0.vmdk"
        lvm_experiments_disk1_path = "/tmp/lvm_experiments_disk1.vmdk"
        vb.customize ['createmedium', '--filename', lvm_experiments_disk0_path, '--size', 2560]
        vb.customize ['createmedium', '--filename', lvm_experiments_disk1_path, '--size', 2560]
        vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk0_path]
        vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 2, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk1_path]
      end
    end
        Данная конфигурация создаст новую виртуальную машину с двумя дополнительными неразмеченными дисками по 2.5 Гб.
        femsk@femsk-virtual-machine:~$ lsblk
        NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
        loop0    7:0    0     4K  1 loop /snap/bare/5
        loop1    7:1    0  61,9M  1 loop /snap/core20/1328
        loop2    7:2    0  61,9M  1 loop /snap/core20/1518
        loop3    7:3    0 254,1M  1 loop /snap/gnome-3-38-2004/106
        loop4    7:4    0  65,2M  1 loop /snap/gtk-common-themes/1519
        loop5    7:5    0 248,8M  1 loop /snap/gnome-3-38-2004/99
        loop6    7:6    0  81,3M  1 loop /snap/gtk-common-themes/1534
        loop7    7:7    0  54,2M  1 loop /snap/snap-store/558
        loop8    7:8    0    47M  1 loop /snap/snapd/16010
        loop9    7:9    0  43,6M  1 loop /snap/snapd/14978
        sda      8:0    0    30G  0 disk
        ├─sda1   8:1    0   512M  0 part /boot/efi
        ├─sda2   8:2    0     1K  0 part
        └─sda5   8:5    0  29,5G  0 part /
        sdb      8:16   0   2,5G  0 disk
        sdc      8:32   0   2,5G  0 disk
        sr0     11:0    1  1024M  0 rom
             
#4. Используя fdisk, разбейте первый диск на 2 раздела: 2 Гб, оставшееся пространство.
        
        femsk@femsk-virtual-machine:~$ sudo -i
        root@femsk-virtual-machine:~# fdisk /dev/sdb
        Welcome to fdisk (util-linux 2.34).
        ...
        Command (m for help): n
        Partition type
        p   primary (0 primary, 0 extended, 4 free)
        e   extended (container for logical partitions)
        Select (default p): p
        Partition number (1-4, default 1): 1
        First sector (2048-5242879, default 2048): 2048
        Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-5242879, default 5242879): +2Gb
        Created a new partition 1 of type 'Linux' and of size 1,9 GiB.
        Command (m for help): n
        Partition type
        p   primary (1 primary, 0 extended, 3 free)
        e   extended (container for logical partitions)
        Select (default p): p
        Partition number (2-4, default 2): 2
        First sector (3907584-5242879, default 3907584): 3907584
        Last sector, +/-sectors or +/-size{K,M,G,T,P} (3907584-5242879, default 5242879): 5242879
        Created a new partition 2 of type 'Linux' and of size 652 MiB.
        Command (m for help): w
        The partition table has been altered.
        Calling ioctl() to re-read partition table.
        Syncing disks.
        root@femsk-virtual-machine:~# lsblk
        NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
        ...        
        sdb      8:16   0   2,5G  0 disk
        ├─sdb1   8:17   0   1,9G  0 part
        └─sdb2   8:18   0   652M  0 part
    
#5. Используя sfdisk, перенесите данную таблицу разделов на второй диск.

        root@femsk-virtual-machine:~# sfdisk -d /dev/sdb|sfdisk --force /dev/sdc
        Checking that no-one is using this disk right now ... OK
        Disk /dev/sdc: 2,51 GiB, 2684354560 bytes, 5242880 sectors
        Disk model: Virtual disk
        ...
        Device     Boot   Start     End Sectors  Size Id Type
        /dev/sdc1          2048 3907583 3905536  1,9G 83 Linux
        /dev/sdc2       3907584 5242879 1335296  652M 83 Linux
        The partition table has been altered.
        ...
        root@femsk-virtual-machine:~# lsblk
        NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
        sdb      8:16   0   2,5G  0 disk
        ├─sdb1   8:17   0   1,9G  0 part
        └─sdb2   8:18   0   652M  0 part
        sdc      8:32   0   2,5G  0 disk
        ├─sdc1   8:33   0   1,9G  0 part
        └─sdc2   8:34   0   652M  0 part
        sr0     11:0    1  1024M  0 rom

#6. Соберите mdadm RAID1 на паре разделов 2 Гб.

        root@femsk-virtual-machine:~# mdadm --create --verbose /dev/md1 -l 1 -n 2 /dev/sd{b1,c1}
        mdadm: Note: this array has metadata at the start and
        may not be suitable as a boot device.  If you plan to
        store '/boot' on this device please ensure that
        your boot-loader understands md/v1.x metadata, or use
        --metadata=0.90
        mdadm: size set to 1950720K
        Continue creating array? y
        mdadm: Defaulting to version 1.2 metadata
        mdadm: array /dev/md1 started.
        root@femsk-virtual-machine:~# lsblk
        NAME    MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
        ...
        sdb       8:16   0   2,5G  0 disk
        ├─sdb1    8:17   0   1,9G  0 part
        │ └─md1   9:1    0   1,9G  0 raid1
        └─sdb2    8:18   0   652M  0 part
        sdc       8:32   0   2,5G  0 disk
        ├─sdc1    8:33   0   1,9G  0 part
        │ └─md1   9:1    0   1,9G  0 raid1
        └─sdc2    8:34   0   652M  0 part
        sr0      11:0    1  1024M  0 rom
       
#7. Соберите mdadm RAID0 на второй паре маленьких разделов.



#8. Создайте 2 независимых PV на получившихся md-устройствах.



#9. Создайте общую volume-group на этих двух PV.



#10. Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.



#11. Создайте mkfs.ext4 ФС на получившемся LV.



#12. Смонтируйте этот раздел в любую директорию, например, /tmp/new.


#13. Поместите туда тестовый файл, например wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz.

#14. Прикрепите вывод lsblk.

#15. Протестируйте целостность файла:

    root@vagrant:~# gzip -t /tmp/new/test.gz
    root@vagrant:~# echo $?
    0
    
    
#16. Используя pvmove, переместите содержимое PV с RAID0 на RAID1.



#17. Сделайте --fail на устройство в вашем RAID1 md.



#18. Подтвердите выводом dmesg, что RAID1 работает в деградированном состоянии.



#19. Протестируйте целостность файла, несмотря на "сбойный" диск он должен продолжать быть доступен:

    root@vagrant:~# gzip -t /tmp/new/test.gz
    root@vagrant:~# echo $?
    0
    
#20. Погасите тестовый хост, vagrant destroy.
