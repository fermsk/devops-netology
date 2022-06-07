#Домашнее задание к занятию "3.2. Работа в терминале, лекция 2"

#Какого типа команда cd? Попробуйте объяснить, почему она именно такого типа; опишите ход своих мыслей, если считаете что она могла бы быть другого типа.
        
        cd - является встроенной командой;
        работать в сессии терминала и менять указатель на текущую дерикторию логичнее внутренней функцией;
        если cd являлась бы внешней командой, то при её вызове она работала бы в своём окружениии, и меняла текущий каталог внутри своего окружения, на shell влияние           не будет. Если сделать CD внешней программой, то после смены деректории необходимо вызвать bash из этой директории, но тогда мы получим новый shell, и выходя           из сесии придется выходить из всех сессий которые создались при каждом вызове внешней CD.
        
#Какая альтернатива без pipe команде grep <some_string> <some_file> | wc -l? man grep поможет в ответе на этот вопрос. Ознакомьтесь с документом о других подобных некорректных вариантах использования pipe.

        vagrant@vagrant:~$ cat tst_bash
        if [[ -d /tmp ]];
        netology
        netologynetology
        54321
        vagrant@vagrant:~$ grep 54321 tst_bash -c
        1
        vagrant@vagrant:~$ grep 54321 tst_bash |wc -l
        1
        
#Какой процесс с PID 1 является родителем для всех процессов в вашей виртуальной машине Ubuntu 20.04?
        
        vagrant@vagrant:~$ pstree -p
        systemd(1)─┬─VBoxService(754)─┬─{VBoxService}(755)
                   │                  ├─{VBoxService}(756)
                   │                  ├─{VBoxService}(757)
                   
#Как будет выглядеть команда, которая перенаправит вывод stderr ls на другую сессию терминала?

        Вызов из pts/0:
        vagrant@vagrant:~$ ls -l \root 2>/dev/pts/1
        vagrant@vagrant:~$
        
#Получится ли одновременно передать команде файл на stdin и вывести ее stdout в другой файл? Приведите работающий пример.

        vagrant@vagrant:~$ cat tst_bash
        if [[ -d /tmp ]];
        netology
        netologynetology
        54321
        new line
        vagrant@vagrant:~$ cat tst_bash_out
        cat: tst_bash_out: No such file or directory 
        vagrant@vagrant:~$ cat <tst_bash >tst_bash_out
        vagrant@vagrant:~$ cat tst_bash_out
        if [[ -d /tmp ]];
        netology
        netologynetology
        54321
        new line
        vagrant@vagrant:~$
        
#Получится ли находясь в графическом режиме, вывести данные из PTY в какой-либо из эмуляторов TTY? Сможете ли вы наблюдать выводимые данные?


        

        

        

        








<======================================================================================>
#Домашнее задание по лекции "Работа в терминале (лекция 1)"

#С помощью базового файла конфигурации запустите Ubuntu 20.04 в VirtualBox посредством Vagrant:

![image](https://user-images.githubusercontent.com/104899352/172226542-ff3387e7-6875-4dc9-90b8-cb018c28d838.png)
![image](https://user-images.githubusercontent.com/104899352/172227123-88b12fbb-dd7f-43ec-8781-a5e41bf5ee1a.png)

#Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: документация. Как добавить оперативной памяти или ресурсов процессора виртуальной машине?

    VagrantFile:
      v.memory = 1024
      v.cpus = 2
или 
      config.vm.provider "virtualbox" do |vb|
        vb.memory = "1024"
        vb.cpu = "2"
        end

#какой переменной можно задать длину журнала history, и на какой строчке manual это описывается?

    1. HISTFILESIZE - максимальное число строк в файле истории для сохранения, строка 1155
    2. HISTSIZE - число команд для сохранения, строка 1178

#что делает директива ignoreboth в bash?

    ignoreboth это сокращение для 2х директив ignorespace and ignoredups, 
       ignorespace - не сохранять команды начинающиеся с пробела, 
       ignoredups - не сохранять команду, если такая уже имеется в истории

#В каких сценариях использования применимы скобки {} и на какой строчке man bash это описано?

    {} - зарезервированные слова, список, also список команд в отличие от ... исполнятся в текущем сеансе, используется в различных условных циклах, условных               операторах, или ограничивает тело функции, в командах выполняет подстановку элементов из списка , если упрощенно то  цикличное выполнение команд с подстановкой 
     например mkdir ./DIR_{A..Z} - создаст каталоги сименами DIR_A, DIR_B и т.д. до DIR_Z строка 343

#С учётом ответа на предыдущий вопрос, как создать однократным вызовом touch 100000 файлов? Получится ли аналогичным образом создать 300000? Если нет, то почему?

    touch {000001..100000}.md - создаёт в текущей директории 100000 файлов
    300000 - создать не удалось, список аргументов, максимально допустимое число - экспериментально - 110188))

#В man bash поищите по /\[\[. Что делает конструкция [[ -d /tmp ]]

     проверяет условие у -d /tmp и возвращает его статус (0 или 1), наличие катаолга /tmp
     Например :
    if [[ -d /tmp ]]
    then
    echo "dir exists"
    else
    echo "dir not exists"

#Основываясь на знаниях о просмотре текущих (например, PATH) и установке новых переменных; командах, которые мы рассматривали, добейтесь в выводе type -a bash в виртуальной машине наличия первым пунктом в списке:

        vagrant@vagrant:~$ mkdir /tmp/new_path_dir/
        vagrant@vagrant:~$ cp /bin/bash /tmp/new_path_dir/
        vagrant@vagrant:~$ type -a bash
        bash is /usr/bin/bash
        bash is /bin/bash
        vagrant@vagrant:~$ PATH=/tmp/new_path_dir/:$PATH
        vagrant@vagrant:~$ type -a bash
        bash is /tmp/new_path_dir/bash
        bash is /usr/bin/bash
        bash is /bin/bash

#Чем отличается планирование команд с помощью batch и at?

    at - команда запускается в указанное время (в параметре)
    batch - запускается когда уровень загрузки системы снизится ниже 1.5.

#Завершите работу виртуальной машины чтобы не расходовать ресурсы компьютера и/или батарею ноутбука.

    vagrant@vagrant:~$ exit
    logout
    Connection to 127.0.0.1 closed.
    PS C:\Windows\system32> vagrant halt
    ==> default: Attempting graceful shutdown of VM...


<============================================================================================================================>


# devops-netology
Hello World!

will ignore:
# all files with extensions:
*.tfstate
*.tfstate.*
*.tfvars
*.tfvars.json
# Crash log files
crash.log
crash.*.log
# Ignore CLI configuration files
.terraformrc
terraform.rc
# Ignore override files
override.tf
override.tf.json
*_override.tf
*_override.tf.json

#new line
add new line!
