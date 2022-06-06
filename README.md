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
1. HISTFILESIZE - максимальное число строк в файле истории для сохранения, 
строка 1155
2. HISTSIZE - число команд для сохранения  , 
строка 1178

#что делает директива ignoreboth в bash?
ignoreboth это сокращение для 2х директив ignorespace and ignoredups, 
    ignorespace - не сохранять команды начинающиеся с пробела, 
    ignoredups - не сохранять команду, если такая уже имеется в истории

#В каких сценариях использования применимы скобки {} и на какой строчке man bash это описано?
{} - зарезервированные слова, список, also список команд в отличие от ... исполнятся в текущем сеансе, 
используется в различных условных циклах, условных операторах, или ограничивает тело функции, 
В командах выполняет подстановку элементов из списка , если упрощенно то  цикличное выполнение команд с подстановкой 
например mkdir ./DIR_{A..Z} - создаст каталоги сименами DIR_A, DIR_B и т.д. до DIR_Z
строка 343

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
