#Домашнее задание к занятию "3.9. Элементы безопасности информационных систем"

#1. Установите Bitwarden плагин для браузера. Зарегестрируйтесь и сохраните несколько паролей.

![image](https://user-images.githubusercontent.com/104899352/178409212-2560f6bc-90c1-45f8-af60-adb73e0afd71.png)

#2. Установите Google authenticator на мобильный телефон. Настройте вход в Bitwarden акаунт через Google authenticator OTP.

![2fa](https://user-images.githubusercontent.com/104899352/178411521-28cf08dd-bfc6-4bd1-ab14-3d2b8b117274.png)

#3. Установите apache2, сгенерируйте самоподписанный сертификат, настройте тестовый сайт для работы по HTTPS.

    root@femsk-virtual-machine:~# sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-       selfsigned.crt -subj "/C=RU/ST=Moscow/L=Moscow/O=OOO/OU=Comp/CN=w           ww.sample.com"
    Generating a RSA private key
    ....................+++++
    .........+++++
    writing new private key to '/etc/ssl/private/apache-selfsigned.key'
    -----

    root@femsk-virtual-machine:~# sudo cat /etc/ssl/private/apache-selfsigned.key
    -----BEGIN PRIVATE KEY-----
    MIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQDRQ8JZse9k4FIA
    v4DXzrHmiCke8LLg+yrtDSwP1g/XkAeG5ADC0f6fNkMNYxe6f5tIba3R073ruZLn
    ZTDa4TNY4aAujqs0EzvLR3xwmKfcYdaKmlDnJAwapAXWpn5/dqzfitAoRmtg7D1W
    OieuqnF+9j/MGhauei7zay2blJcs9Y30w1AW3JHalabUq4i58qnwSrKPlU0+r84q
    FEk+V81/wSEIgdJ6Wm1qryC6EoQz9yzXp2cXoldNkYWRBJrdw/M9MfM5HhcEVBiz
    nmOYJZ96kL4Byc4RKjS/KVXeS8aAgayuUwmkaAeN+7IO2Krh99i7vGyNaQ6mldTU
    ye8zeKNNAgMBAAECggEBAKtg+sHKX2wV6hKCw1n6BOfviv2z0ks+Z6FLsFIn1UGM
    yx1FjPoAUK7DGZbVGJG7/8gJStkUt+1dRVoMHT6kQBECdtSqMrurJMUN/nOYEaEC
    31kTmD4z31XSDpYENITfBCTu4hqZ0UbHMlRzvBvnqpt3wDe/BeROXDUuCWPpBQXj
    eI66q0Mwabr5Ibx4ikOygMwMAtEcGjnseR0oPMFNaUAJJrzuSOwsng1ViiAhRvxA
    9iN8vBxNqko/nXAAOpSy/h8yGBeWcVV8x+8Dti+rej3LrMoFbH9TOQ2I7UPXSqdD
    17LsWmA21AtRart6JRcP2hIYo7K1A0x2eNKBfqTrmGECgYEA8241i0q/3GS3hB23
    jUEy0FDH4vgph8ryB6abrxAYtjXWRNJgC6f0yWlNqYQu5ICEkgehRhzPJvbPlGqe
    7tbIC7gGe6v/otv87dRnuUndoGz6wbHFQX7l+tYRBEmk9IyAn2qmSKU3mWbiJT6J
    tHH4b5Gpnoe1uPgASmEZXgUyNA8CgYEA3BHtsAy711QL3T2tUf49jurPtD8DxpFv
    J/XmS+lwEsKZSO6P8rrKBd7eeQ2d1m5OYbUb6zXsOhuQ32iS4GTwmKNGcqi2xl5h
    e3sli7mv7cL1mLGdVu/oJl7Djpw5KX5oxPYeego2BOdP7x9sm8TgAGmES9mziPV5
    AwaMMoTh5uMCgYBeG6UnjGZP1b/8m2Byg1oZnqEn5bhoftTCkG5vZ1GmX3nOcWYg
    G3ZOxx73Adr/C5A0xC5c5JZRAemN/woiW3ZK0YHwHbZeR52odA0FXMEJXBg0+XzA
    rUZLiqZZef8Da63t81UFkJnF/DhBHcQutkDNIQrp9p0SPQ4fsxoTdv7JkQKBgQCo
    /Sds+cpAZmyZ3mO6Q6XXmh4WxhDLKSCXKe9HOaFy9nWomHB3LtI1QdfKUxdx8tBD
    nUQsEQMt977+nxmyMDDEtRRCtaVsnEr0/DJvog4jYIMhVrqAaMb2t+wpFXObllMz
    c98hTbf/efRapeHLl5l/F4ecizafJAhht2Ru4rVpiwKBgAVm/QLRbaRKucfR+2lO
    56GfSZMWucDkfKf6yDFQGyQJpGW2PTc6AOkqf3+9LtyxjkIrr7+OxpKzOeedOPfd
    eDzqSwcZuq8X+Kv1413ym2ifmQNQYD9aAWqVUP77OnKznDl9AOuB88gJ7+o5TPW5
    lYa4I9Y07IScxOaZFs4RAsQ+
    -----END PRIVATE KEY-----
    root@femsk-virtual-machine:~# sudo apt install apache2
    ...
    root@femsk-virtual-machine:~# systemctl start apache2
    femsk@femsk-virtual-machine:/etc/apache2/sites-available$ sudo chmod 777 000-default.conf
    femsk@femsk-virtual-machine:/etc/apache2/sites-available$ ls -lh
    total 12K
    -rwxrwxrwx 1 root root 1,6K июл 12 08:43 000-default.conf
    -rw-r--r-- 1 root root 6,2K фев 23  2021 default-ssl.conf
    femsk@femsk-virtual-machine:/etc/apache2/sites-available$ sudo systemctl restart apache2
    femsk@femsk-virtual-machine:/etc/apache2/sites-available$
    
![image](https://user-images.githubusercontent.com/104899352/178421120-c29fe23d-6b5d-4852-bbca-34a6ab9e0121.png)

#4. Проверьте на TLS уязвимости произвольный сайт в интернете (кроме сайтов МВД, ФСБ, МинОбр, НацБанк, РосКосмос, РосАтом, РосНАНО и любых госкомпаний, объектов КИИ, ВПК ... и тому подобное).

#5. Установите на Ubuntu ssh сервер, сгенерируйте новый приватный ключ. Скопируйте свой публичный ключ на другой сервер. Подключитесь к серверу по SSH-ключу.

#6. Переименуйте файлы ключей из задания 5. Настройте файл конфигурации SSH клиента, так чтобы вход на удаленный сервер осуществлялся по имени сервера.

#7. Соберите дамп трафика утилитой tcpdump в формате pcap, 100 пакетов. Откройте файл pcap в Wireshark.

