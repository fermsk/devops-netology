#Домашнее задание к занятию "3.6. Компьютерные сети, лекция 1"
 
#1.Работа c HTTP через телнет.

    Подключитесь утилитой телнет к сайту stackoverflow.com telnet stackoverflow.com 80
    отправьте HTTP запрос
    GET /questions HTTP/1.0
    HOST: stackoverflow.com
    [press enter]
    [press enter]
    В ответе укажите полученный HTTP код, что он означает?
    
    femsk@femsk-virtual-machine:~$ telnet stackoverflow.com 80
    Trying 151.101.193.69...
    Connected to stackoverflow.com.
    Escape character is '^]'.
    GET /questions HTTP/1.0
    HOST: stackoverflow.com
    HTTP/1.1 400 Bad Request
    Connection: close
    Content-Length: 0
    Connection closed by foreign host.
    femsk@femsk-virtual-machine:~$
    
    Отправленный запрос приводит к сбою еще до того, как его обработает сервер, соединенине закрывается.
    Если изменить запрос, то мы увидим сообщение 301 о перемещении ресурса на URL указанный в заголовке Location:
    GET /index.html HTTP/1.1
    HOST: stackoverflow.com
    HTTP/1.1 301 Moved Permanently
    cache-control: no-cache, no-store, must-revalidate
    location: https://stackoverflow.com/index.html
    x-request-guid: 3d69673f-50af-4ceb-9b49-82716acb7b63
    feature-policy: microphone 'none'; speaker 'none'
    content-security-policy: upgrade-insecure-requests; frame-ancestors 'self' https://stackexchange.com
    Accept-Ranges: bytes
    Transfer-Encoding: chunked
    Date: Wed, 29 Jun 2022 05:24:10 GMT
    Via: 1.1 varnish
    Connection: keep-alive
    
#2. Повторите задание 1 в браузере, используя консоль разработчика F12.

    откройте вкладку Network
    отправьте запрос http://stackoverflow.com
    найдите первый ответ HTTP сервера, откройте вкладку Headers
    укажите в ответе полученный HTTP код.
    проверьте время загрузки страницы, какой запрос обрабатывался дольше всего?
    приложите скриншот консоли браузера в ответ.
    
![image](https://user-images.githubusercontent.com/104899352/176360310-cb1314e9-65fb-49dc-bb71-714c75665aa5.png)
    
    HTTP код перенаправления  307 Temporary Redirect означает, что запрошенный ресурс был временно перемещён в URL-адрес, указанный в заголовке Location.
    
![image](https://user-images.githubusercontent.com/104899352/176360904-d02c7159-2584-4b19-a2e4-37c9bd9d2f8c.png)
    
#3. Какой IP адрес у вас в интернете?

    Задание выполняю в офисе, светить полный IP не могу:
    femsk@femsk-virtual-machine:~$ wget -qO- eth0.me
    91.199.X.X

#4. Какому провайдеру принадлежит ваш IP адрес? Какой автономной системе AS? Воспользуйтесь утилитой whois

    femsk@femsk-virtual-machine:~$ whois -h whois.ripe.net 91.199.X.X
    % Information related to '91.199.X.0 - 91.199.X.255'
    route:          91.199.X.0/24
    descr:          xxxxxx
    origin:         AS44799
    mnt-by:         AS5537-MNT
    mnt-routes:     MASTERTEL-MNT
    created:        2008-03-28T09:09:05Z
    last-modified:  2011-12-29T09:09:24Z
    source:         RIPE

#5. Через какие сети проходит пакет, отправленный с вашего компьютера на адрес 8.8.8.8? Через какие AS? Воспользуйтесь утилитой traceroute

![image](https://user-images.githubusercontent.com/104899352/176363868-5d69afd9-ed6a-41a4-bd44-6b593c4f36e5.png)

#6. Повторите задание 5 в утилите mtr. На каком участке наибольшая задержка - delay?

    Между 6-7-8-9 хопами:
   
![image](https://user-images.githubusercontent.com/104899352/176364610-86fe2a52-dc39-43e4-aef3-2f7d8ba9af88.png)

#7. Какие DNS сервера отвечают за доменное имя dns.google? Какие A записи? воспользуйтесь утилитой dig

![image](https://user-images.githubusercontent.com/104899352/176365322-312ba777-a54a-43ec-9532-551553447a8f.png)

#8. Проверьте PTR записи для IP адресов из задания 7. Какое доменное имя привязано к IP? воспользуйтесь утилитой dig

    femsk@femsk-virtual-machine:~$ dig -x 8.8.4.4 +noall +answer
    4.4.8.8.in-addr.arpa.   85020   IN      PTR     dns.google.
    femsk@femsk-virtual-machine:~$ dig -x 8.8.8.8 +noall +answer
    8.8.8.8.in-addr.arpa.   86399   IN      PTR     dns.google.


