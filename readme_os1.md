#Домашнее задание ОС 1
_________________________________________
#1. Какой системный вызов делает команда cd? В прошлом ДЗ мы выяснили, что cd не является самостоятельной программой, это shell builtin, поэтому запустить strace непосредственно на cd не получится. Тем не менее, вы можете запустить strace на /bin/bash -c 'cd /tmp'. В этом случае вы увидите полный список системных вызовов, которые делает сам bash при старте. Вам нужно найти тот единственный, который относится именно к cd. Обратите внимание, что strace выдаёт результат своей работы в поток stderr, а не в stdout.
        
        Решение: 
        femsk@femsk-virtual-machine:~$ strace /bin/bash -c 'cd /tmp'
        .....
        chdir("/tmp") - вызов команды cd

#2. Попробуйте использовать команду file на объекты разных типов на файловой системе. Например:

    vagrant@netology1:~$ file /dev/tty
    /dev/tty: character special (5/0)
    vagrant@netology1:~$ file /dev/sda
    /dev/sda: block special (8/0)
    vagrant@netology1:~$ file /bin/bash
    /bin/bash: ELF 64-bit LSB shared object, x86-64
    Используя strace выясните, где находится база данных file на основании которой она делает свои догадки.
    
    Решение:
    femsk@femsk-virtual-machine:~$ strace /usr/bin/file
    ....
    openat(AT_FDCWD, "/usr/share/misc/magic.mgc", O_RDONLY) = 3
    В man file также указан файл usr/share/misc/magic.mgc
    
#3. Предположим, приложение пишет лог в текстовый файл. Этот файл оказался удален (deleted в lsof), однако возможности сигналом сказать приложению переоткрыть файлы или просто перезапустить приложение – нет. Так как приложение продолжает писать в удаленный файл, место на диске постепенно заканчивается. Основываясь на знаниях о перенаправлении потоков предложите способ обнуления открытого удаленного файла (чтобы освободить место на файловой системе).
        
        femsk@femsk-virtual-machine:~/Netology$ touch file_log.txt
        femsk@femsk-virtual-machine:~/Netology$ less +F file_log.txt >/dev/null &
        [7] 9299
        femsk@femsk-virtual-machine:~/Netology$ rm -f file_log.txt
        femsk@femsk-virtual-machine:~/Netology$ sudo lsof | grep deleted
        less    9299     femsk   REG  253,2 1073741824   11 /home/femsk/Netology/file_log (deleted)
        sudo kill -9 9299
        ________________________________________________
       Исправление:
       femsk@femsk-virtual-machine:~/Netology$ cat /proc/9299/fd/4 > /home/femsk/Netology/file.new

#4. Занимают ли зомби-процессы какие-то ресурсы в ОС (CPU, RAM, IO)?

    Зомби процессы, в отличии от "сирот" освобождают свои ресурсы, но не освобождают запись в таблице процессов. 
    запись освободиться при вызове wait() родительским процессом.

#5. В iovisor BCC есть утилита opensnoop:

   root@vagrant:~# dpkg -L bpfcc-tools | grep sbin/opensnoop
   /usr/sbin/opensnoop-bpfcc

    На какие файлы вы увидели вызовы группы open за первую секунду работы утилиты? Воспользуйтесь пакетом bpfcc-tools для Ubuntu 20.04. Дополнительные сведения по      установке.
    femsk@femsk-virtual-machine:~$ sudo apt install bpfcc-tools
    ....
    femsk@femsk-virtual-machine:~$ dpkg -L bpfcc-tools | grep sbin/opensnoop
    /usr/sbin/opensnoop-bpfcc
    femsk@femsk-virtual-machine:~$ sudo -i
    root@femsk-virtual-machine:~# ^C
    root@femsk-virtual-machine:~# /usr/sbin/opensnoop-bpfcc
    In file included from <built-in>:2:
    In file included from /virtual/include/bcc/bpf.h:12:
    In file included from include/linux/types.h:6:
    In file included from include/uapi/linux/types.h:14:
    In file included from ./include/uapi/linux/posix_types.h:5:
    In file included from include/linux/stddef.h:5:
    In file included from include/uapi/linux/stddef.h:2:
    3 warnings generated.
    PID    COMM               FD ERR PATH
    682    vmtoolsd            7   0 /etc/mtab
    682    vmtoolsd            9   0 /proc/devices
    682    vmtoolsd            7   0 /sys/class/block/sda5/../device/../../../class
    682    vmtoolsd           -1   2 /sys/class/block/sda5/../device/../../../label
    682    vmtoolsd            7   0 /sys/class/block/sda1/../device/../../../class
    682    vmtoolsd           -1   2 /sys/class/block/sda1/../device/../../../label
    682    vmtoolsd            7   0 /run/systemd/resolve/resolv.conf
    682    vmtoolsd            7   0 /proc/net/route
    682    vmtoolsd            7   0 /proc/net/ipv6_route
    682    vmtoolsd            7   0 /proc/uptime
    682    vmtoolsd            7   0 /proc/meminfo
    682    vmtoolsd            7   0 /proc/vmstat


#6. Какой системный вызов использует uname -a? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в /proc, где можно узнать версию ядра и релиз ОС.

    root@femsk-virtual-machine:~# uname ()
    ...
    femsk@femsk-virtual-machine:~$ man 2 uname
    ...
    Part of the utsname information is also accessible  via  /proc/sys/kernel/{ostype, hostname, osrelease, version, domainname}.
    
#7. Чем отличается последовательность команд через ; и через && в bash? Например:

    root@netology1:~# test -d /tmp/some_dir; echo Hi
    Hi
    root@netology1:~# test -d /tmp/some_dir && echo Hi
    root@netology1:~#

    Есть ли смысл использовать в bash &&, если применить set -e?
    
    Решение:
    && -  условный оператор; 
    ;  - разделитель последовательных команд.
    в конструкции test -d /tmp/some_dir && echo Hi - echo отработает только при успешном заверщении команды test.
    set -e - прерывает сессию при любом ненулевом значении исполняемых команд в конвеере кроме последней.
    &&  вместе с set -e- не имеет смысла, так как при ошибке, выполнение команд прекратит
    ся. 

#8. Из каких опций состоит режим bash set -euxo pipefail и почему его хорошо было бы использовать в сценариях?
    
    -e прерывает выполнение исполнения по ошибке любой команды кроме последней в последовательности; 
    -x вывод трейса простых команд; 
    -u неустановленные/не заданные параметры и переменные считаются как ошибки, с выводом в stderr текста ошибки и выполнение завершения неинтерактивного вызова;
    -o pipefail возвращает код возврата набора/последовательности команд, ненулевой при последней команде или 0 для успешного выполнения команд.
     Для сценария: повышает деталезацию лога и завершит сценарий при наличии ошибок на любом этапе выполнения сценария кроме последней завершающей команды.

#9. Используя -o stat для ps, определите, какой наиболее часто встречающийся статус у процессов в системе. В man ps ознакомьтесь (/PROCESS STATE CODES) что значат дополнительные к основной заглавной буквы статуса процессов. Его можно не учитывать при расчете (считать S, Ss или Ssl равнозначными).

    S*(S,S+,Ss,Ssl,Ss+) - Процессы ожидающие завершения (спящие с прерыванием "сна");
    I*(I,I<) - фоновые процессы ядра.
    дополнительные символы это характеристики, например приоритет.

