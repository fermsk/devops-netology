Домашнее задание ОС 2
____________________________________________________________________________________________________

#1. На лекции мы познакомились с node_exporter. В демонстрации его исполняемый файл запускался в background. Этого достаточно для демо, но не для настоящей production-системы, где процессы должны находиться под внешним управлением. Используя знания из лекции по systemd, создайте самостоятельно простой unit-файл для node_exporter:

    -поместите его в автозагрузку;
    -предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на systemctl cat cron);
    -удостоверьтесь, что с помощью systemctl процесс корректно стартует, завершается, а после перезагрузки автоматически поднимается.
    
![image](https://user-images.githubusercontent.com/104899352/174306006-3fa0ebeb-93e8-44fb-b1a2-d891557a2e07.png)
        
        Сервис стартует и перезапускается корректно:
        femsk@femsk-virtual-machine:~$ ps -e |grep node_exporter
        1952 ?        00:00:00 node_exporter
        femsk@femsk-virtual-machine:~$ systemctl stop node_exporter
        ==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
        Authentication is required to stop 'node_exporter.service'.
        Authenticating as: admin,,, (femsk)
        Password:
        ==== AUTHENTICATION COMPLETE ===
        femsk@femsk-virtual-machine:~$ ps -e |grep node_exporter
        femsk@femsk-virtual-machine:~$ systemctl start node_exporter
        ==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
        Authentication is required to start 'node_exporter.service'.
        Authenticating as: admin,,, (femsk)
        Password:
        ==== AUTHENTICATION COMPLETE ===
        femsk@femsk-virtual-machine:~$ ps -e |grep node_exporter
        2420 ?        00:00:00 node_exporter
        femsk@femsk-virtual-machine:~$
        
        Прописан конфигруационный файл:
        [Unit]
        Description=Prometheus Node Exporter
        Wants=network-online.target
        After=network-online.target

        [Service]
        User=node_exporter
        Group=node_exporter
        Type=simple
        ExecStart=/usr/local/bin/node_exporter

        [Install]
        WantedBy=multi-user.target
        
        
#2. Ознакомьтесь с опциями node_exporter и выводом /metrics по-умолчанию. Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.

![image](https://user-images.githubusercontent.com/104899352/174309950-18d1dd79-0e61-4afa-9ef6-a6c666a9e845.png)

![image](https://user-images.githubusercontent.com/104899352/174310154-8eb07b0b-2bef-4c2b-83e9-37c150925f67.png)

![image](https://user-images.githubusercontent.com/104899352/174310325-0a0493d3-edad-44f3-9598-df01e71fdea7.png)
      

#3.Установите в свою виртуальную машину Netdata. Воспользуйтесь готовыми пакетами для установки (sudo apt install -y netdata). После успешной установки:

    -в конфигурационном файле /etc/netdata/netdata.conf в секции [web] замените значение с localhost на bind to = 0.0.0.0,
    -добавьте в Vagrantfile проброс порта Netdata на свой локальный компьютер и сделайте vagrant reload:
    
    config.vm.network "forwarded_port", guest: 19999, host: 19999
    
    После успешной перезагрузки в браузере на своем ПК (не в виртуальной машине) вы должны суметь зайти на localhost:19999. Ознакомьтесь с метриками, которые по      умолчанию    собираются Netdata и с комментариями, которые даны к этим метрикам.
    
![image](https://user-images.githubusercontent.com/104899352/174284428-187886fb-6c0b-4ca6-a5c7-6b5017b5c201.png)

#4. Можно ли по выводу dmesg понять, осознает ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?
        
        Да, причем даже тип ВМ, так как есть соответсвующая строка:
        femsk@femsk-virtual-machine:~/Netology$ dmesg |grep virtualiz
        [    0.021747] Booting paravirtualized kernel on VMware hypervisor
        [    3.524419] systemd[1]: Detected virtualization vmware.


#5. Как настроен sysctl fs.nr_open на системе по-умолчанию? Узнайте, что означает этот параметр. Какой другой существующий лимит не позволит достичь такого числа (ulimit --help)?

        Это максимальное число открытых дескрипторов для ядра (системы), для пользователя задать больше этого числа нельзя (если не менять).
        femsk@femsk-virtual-machine:~/Netology$ /sbin/sysctl -n fs.nr_open
        1048576
        
        Максимальный предел ОС
        femsk@femsk-virtual-machine:~/Netology$ cat /proc/sys/fs/file-max
        9223372036854775807


#6. Запустите любой долгоживущий процесс (не ls, который отработает мгновенно, а, например, sleep 1h) в отдельном неймспейсе процессов; покажите, что ваш процесс работает под PID 1 через nsenter. Для простоты работайте в данном задании под root (sudo -i). Под обычным пользователем требуются дополнительные опции (--map-root-user) и т.д.

        root@femsk-virtual-machine:~# unshare --fork --pid --mount-proc sleep 1h
        root@femsk-virtual-machine:~# ps -e |grep sleep
        13086 pts/1    00:00:00 sleep
        root@femsk-virtual-machine:~# nsenter --target 13086 --pid --mount
        root@femsk-virtual-machine:/# ps
        PID TTY          TIME CMD
        1 pts/1    00:00:00 sleep
        2 pts/1    00:00:00 bash
        11 pts/1    00:00:00 ps
        root@femsk-virtual-machine:/#
        

#7. Найдите информацию о том, что такое :(){ :|:& };:. Запустите эту команду в своей виртуальной машине Vagrant с Ubuntu 20.04 (это важно, поведение в других ОС не проверялось). Некоторое время все будет "плохо", после чего (минуты) – ОС должна стабилизироваться. Вызов dmesg расскажет, какой механизм помог автоматической стабилизации. Как настроен этот механизм по-умолчанию, и как изменить число процессов, которое можно создать в сессии?

        Это функция с именем ":" внутри "{}", которая после опредения в строке пораждает два фоновых процесса самой себя.
        
        После запуска система стабилизируется через некоторое время.
        Ограничить можно:
        ulimit -u 50 
        /etc/security/limits.conf
            * hard nproc 50

