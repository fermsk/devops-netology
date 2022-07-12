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

    femsk@femsk-virtual-machine:/a2sv$ sudo apt-get install testssl.sh
    ...
    femsk@femsk-virtual-machine:/a2sv$ testssl randstuff.ru

    No engine or GOST support via engine with your /usr/bin/openssl

    ###########################################################
        testssl       3.0 from https://testssl.sh/

          This program is free software. Distribution and
                 modification under GPLv2 permitted.
          USAGE w/o ANY WARRANTY. USE IT AT YOUR OWN RISK!

           Please file bugs @ https://testssl.sh/bugs/

    ###########################################################

     Using "OpenSSL 1.1.1f  31 Mar 2020" [~79 ciphers]
     on femsk-virtual-machine:/usr/bin/openssl
     (built: "Jul  4 11:24:28 2022", platform: "debian-amd64")


     Start 2022-07-12 09:48:21        -->> 188.127.241.67:443 (randstuff.ru) <<--

     rDNS (188.127.241.67):  --
     Service detected:       HTTP


     Testing protocols via sockets except NPN+ALPN

     SSLv2      not offered (OK)
     SSLv3      not offered (OK)
     TLS 1      offered (deprecated)
     TLS 1.1    offered (deprecated)
     TLS 1.2    offered (OK)
     TLS 1.3    offered (OK): final
     NPN/SPDY   not offered
     ALPN/HTTP2 h2, http/1.1 (offered)

     Testing cipher categories

     NULL ciphers (no encryption)                  not offered (OK)
     Anonymous NULL Ciphers (no authentication)    not offered (OK)
     Export ciphers (w/o ADH+NULL)                 not offered (OK)
     LOW: 64 Bit + DES, RC[2,4] (w/o export)       not offered (OK)
     Triple DES Ciphers / IDEA                     not offered
     Obsolete: SEED + 128+256 Bit CBC cipher       offered
     Strong encryption (AEAD ciphers)              offered (OK)


     Testing robust (perfect) forward secrecy, (P)FS -- omitting Null Authentication/Encryption, 3DES, RC4

     PFS is offered (OK)          TLS_AES_256_GCM_SHA384 TLS_CHACHA20_POLY1305_SHA256 ECDHE-RSA-AES256-GCM-SHA384
                                  ECDHE-RSA-AES256-SHA384 ECDHE-RSA-AES256-SHA ECDHE-RSA-CHACHA20-POLY1305
                                  TLS_AES_128_GCM_SHA256 ECDHE-RSA-AES128-GCM-SHA256 ECDHE-RSA-AES128-SHA256
                                  ECDHE-RSA-AES128-SHA
     Elliptic curves offered:     secp224r1 prime256v1 secp384r1 secp521r1 X25519


     Testing server preferences

     Has server cipher order?     yes (OK) -- TLS 1.3 and below
     Negotiated protocol          TLSv1.3
     Negotiated cipher            TLS_AES_128_GCM_SHA256, 253 bit ECDH (X25519)
     Cipher order
        TLSv1:     ECDHE-RSA-AES128-SHA AES128-SHA ECDHE-RSA-AES256-SHA AES256-SHA
        TLSv1.1:   ECDHE-RSA-AES128-SHA AES128-SHA ECDHE-RSA-AES256-SHA AES256-SHA
        TLSv1.2:   ECDHE-RSA-AES128-GCM-SHA256 ECDHE-RSA-CHACHA20-POLY1305 ECDHE-RSA-AES128-SHA256 ECDHE-RSA-AES128-SHA
                   AES128-GCM-SHA256 AES128-CCM8 AES128-CCM AES128-SHA256 AES128-SHA ECDHE-RSA-AES256-GCM-SHA384
                   ECDHE-RSA-AES256-SHA384 ECDHE-RSA-AES256-SHA AES256-GCM-SHA384 AES256-CCM8 AES256-CCM AES256-SHA256
                   AES256-SHA
        TLSv1.3:   TLS_AES_128_GCM_SHA256 TLS_CHACHA20_POLY1305_SHA256 TLS_AES_256_GCM_SHA384


     Testing server defaults (Server Hello)

     TLS extensions (standard)    "renegotiation info/#65281" "server name/#0" "EC point formats/#11" "session ticket/#35"
                                  "supported versions/#43" "key share/#51" "max fragment length/#1"
                                  "application layer protocol negotiation/#16" "encrypt-then-mac/#22"
                                  "extended master secret/#23"
     Session Ticket RFC 5077 hint 64800 seconds, session tickets keys seems to be rotated < daily
     SSL Session ID support       yes
     Session Resumption           Tickets: yes, ID: no
     TLS clock skew               Random values, no fingerprinting possible
     Signature Algorithm          SHA256 with RSA
     Server key size              RSA 2048 bits
     Server key usage             Digital Signature, Key Encipherment
     Server extended key usage    TLS Web Server Authentication, TLS Web Client Authentication
     Serial / Fingerprints        031965E5383404BE45C2FC7C6C3108208170 / SHA1 1CC00110868D110FCB8681615BF642EF781D2C1C
                                  SHA256 A7AE46EB0314799BA5BC256F13620A4112A95D8FA313378F75FC6B1795A1694A
     Common Name (CN)             randstuff.ru , (request w/o SNI: no CN field in subject)
     subjectAltName (SAN)         randstuff.ru www.randstuff.ru
     Issuer                       R3 (Let's Encrypt from US)
     Trust (hostname)             Ok via SAN (SNI mandatory)
     Chain of trust               Ok
     EV cert (experimental)       no
     ETS/"eTLS", visibility info  not present
     Certificate Validity (UTC)   86 >= 60 days (2022-07-09 07:14 --> 2022-10-07 07:14)
     # of certificates provided   3
     Certificate Revocation List  --
     OCSP URI                     http://r3.o.lencr.org
     OCSP stapling                not offered
     OCSP must staple extension   --
     DNS CAA RR (experimental)    not offered
     Certificate Transparency     yes (certificate extension)


     Testing HTTP header response @ "/"

     HTTP Status Code             200 OK
     HTTP clock skew              0 sec from localtime
     Strict Transport Security    not offered
     Public Key Pinning           --
     Server banner                ddos-guard
     Application banner           X-Powered-By: PHP/7.1.33
     Cookie(s)                    5 issued: NONE secure, 1/5 HttpOnly
     Security headers             Cache-Control no-store, no-cache, must-revalidate
                                  Pragma no-cache
     Reverse Proxy banner         --


     Testing vulnerabilities

     Heartbleed (CVE-2014-0160)                not vulnerable (OK), no heartbeat extension
     CCS (CVE-2014-0224)                       not vulnerable (OK)
     Ticketbleed (CVE-2016-9244), experiment.  not vulnerable (OK), no session tickets
     ROBOT                                     not vulnerable (OK)
     Secure Renegotiation (RFC 5746)           supported (OK)
     Secure Client-Initiated Renegotiation     VULNERABLE (NOT ok), DoS threat
     CRIME, TLS (CVE-2012-4929)                not vulnerable (OK)
     BREACH (CVE-2013-3587)                    potentially NOT ok, uses gzip HTTP compression. - only supplied "/" tested
                                               Can be ignored for static pages or if no secrets in the page
     POODLE, SSL (CVE-2014-3566)               not vulnerable (OK), no SSLv3 support
     TLS_FALLBACK_SCSV (RFC 7507)              Check failed, unexpected result , run testssl -Z --debug=1 and look at /tmp/testssl.VZjOup/*tls_fallback_scsv.txt
     SWEET32 (CVE-2016-2183, CVE-2016-6329)    not vulnerable (OK)
     FREAK (CVE-2015-0204)                     not vulnerable (OK)
     DROWN (CVE-2016-0800, CVE-2016-0703)      not vulnerable on this host and port (OK)
                                               make sure you don't use this certificate elsewhere with SSLv2 enabled services
                                               https://censys.io/ipv4?q=A7AE46EB0314799BA5BC256F13620A4112A95D8FA313378F75FC6B1795A1694A could help you to find out
     LOGJAM (CVE-2015-4000), experimental      not vulnerable (OK): no DH EXPORT ciphers, no DH key detected with <= TLS 1.2
     BEAST (CVE-2011-3389)                     TLS1: ECDHE-RSA-AES128-SHA AES128-SHA ECDHE-RSA-AES256-SHA AES256-SHA
                                               VULNERABLE -- but also supports higher protocols  TLSv1.1 TLSv1.2 (likely mitigated)
     LUCKY13 (CVE-2013-0169), experimental     potentially VULNERABLE, uses cipher block chaining (CBC) ciphers with TLS. Check patches
     RC4 (CVE-2013-2566, CVE-2015-2808)        no RC4 ciphers detected (OK)
      

#5. Установите на Ubuntu ssh сервер, сгенерируйте новый приватный ключ. Скопируйте свой публичный ключ на другой сервер. Подключитесь к серверу по SSH-ключу.

    femsk@femsk-virtual-machine:/$ ssh-keygen -t rsa
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/femsk/.ssh/id_rsa):
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /home/femsk/.ssh/id_rsa
    Your public key has been saved in /home/femsk/.ssh/id_rsa.pub
    The key fingerprint is:
    SHA256:CQXNtcHuLTB7Lgo8vJkhR9IrjfjT7LJTKUvum20FShA femsk@femsk-virtual-machine
    The key's randomart image is:
    +---[RSA 3072]----+
    |E.    .+.oo      |
    |.      .o .o     |
    | .    .  ..      |
    |  ...  .o..      |
    | ...oo  S= .     |
    | .+Bo.. . + .    |
    |.o+*X.   o .     |
    | .**+B  . .      |
    | .*O* .. .       |
    +----[SHA256]-----+

    femsk@femsk-virtual-machine:~/.ssh$ ssh-copy-id admin@10.0.22.1
    /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/femsk/.ssh/id_rsa.pub"
    The authenticity of host '10.0.22.1 (10.0.22.1)' can't be established.
    ECDSA key fingerprint is SHA256:8ygOXEpveRfKA0n0v4GkozRPf43vE49JdBnGy3BGUNs.
    Are you sure you want to continue connecting (yes/no/[fingerprint])? y
    Please type 'yes', 'no' or the fingerprint: yes
    /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
    /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
    admin@10.0.22.1's password:

    Number of key(s) added: 1

    Now try logging into the machine, with:   "ssh 'admin@10.0.22.1'"
    and check to make sure that only the key(s) you wanted were added.

    femsk@femsk-virtual-machine:~/.ssh$ ssh 'admin@10.0.22.1'
    Activate the web console with: systemctl enable --now cockpit.socket

    Last login: Mon Jan 24 04:33:14 2022
    [admin@alkms ~]$

#6. Переименуйте файлы ключей из задания 5. Настройте файл конфигурации SSH клиента, так чтобы вход на удаленный сервер осуществлялся по имени сервера.

    femsk@femsk-virtual-machine:~/.ssh$ mv id_rsa id_rsa_1
    femsk@femsk-virtual-machine:~/.ssh$ chmod 600 config
    femsk@femsk-virtual-machine:~/.ssh$ vim config
    femsk@femsk-virtual-machine:~/.ssh$ cat config
    Host alkms
            HostName 10.0.22.1
            IdentityFile ~/.ssh/id_rsa_1
            User admin
    femsk@femsk-virtual-machine:~/.ssh$ ssh alkms
    Activate the web console with: systemctl enable --now cockpit.socket

    Last login: Tue Jul 12 04:21:00 2022 from 10.0.22.81
    [admin@alkms ~]$

#7. Соберите дамп трафика утилитой tcpdump в формате pcap, 100 пакетов. Откройте файл pcap в Wireshark.

    femsk@femsk-virtual-machine:~/exch$ sudo tcpdump -c 100 -w dump.pcap
    tcpdump: listening on ens160, link-type EN10MB (Ethernet), capture size 262144 bytes
    100 packets captured
    100 packets received by filter
    0 packets dropped by kernel
![image](https://user-images.githubusercontent.com/104899352/178451812-0b60fdbb-fdd4-4026-8958-49097753003d.png)



    


