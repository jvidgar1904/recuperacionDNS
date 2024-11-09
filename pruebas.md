# DNS: Maestro y esclavo con subdominios - PRUEBAS

Una vez configurados los servidores DNS `DNSA` (maestro) y `DNSB` (esclavo), se realizan las comprobaciones de resolución de zonas directas e inversas con `nslookup`, `dig` y `host`.

---

## Pruebas con `nslookup`

**Consultas directas (de dominio a IP)**

```bash
vagrant@dnsb:~$ nslookup server01.informatica.ies.test 192.168.57.10
Server:         192.168.57.10
Address:        192.168.57.10#53

Name:   server01.informatica.ies.test
Address: 192.168.57.10
```

```bash
vagrant@dnsa:~$ nslookup server02.aulas.ies.test 192.168.57.10
Server:         192.168.57.10
Address:        192.168.57.10#53

Name:   server02.aulas.ies.test
Address: 192.168.57.100
```

```bash
vagrant@dnsb:~$ nslookup matematicas.departamentos.ies.test 192.168.57.10
Server:         192.168.57.10
Address:        192.168.57.10#53

Name:   matematicas.departamentos.ies.test
Address: 192.168.57.150
```  

**Consultas Inversas (de IP a domninio)**

```bash
vagrant@dnsa:~$ nslookup 192.168.57.10 192.168.57.10
10.57.168.192.in-addr.arpa      name = server01.informatica.ies.test.
```

```bash
vagrant@dnsb:~$ nslookup 192.168.57.100 192.168.57.10
100.57.168.192.in-addr.arpa     name = server02.aulas.ies.test.
```

```bash
vagrant@dnsa:~$ nslookup 192.168.57.150 192.168.57.10
150.57.168.192.in-addr.arpa     name = matematicas.departamentos.ies.test.
```

---

## Pruebas con `dig`

**Consultas Directas**

```bash
vagrant@dnsb:~$ dig @192.168.57.10 pc02.informatica.ies.test

; <<>> DiG 9.18.28-1~deb12u2-Debian <<>> @192.168.57.10 pc02.informatica.ies.test
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 51435
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 31a8e310a4c6d19401000000672fcb020f73ba6ed76a379f (good)
;; QUESTION SECTION:
;pc02.informatica.ies.test.     IN      A

;; ANSWER SECTION:
pc02.informatica.ies.test. 604800 IN    A       192.168.57.22

;; Query time: 0 msec
;; SERVER: 192.168.57.10#53(192.168.57.10) (UDP)
;; WHEN: Sat Nov 09 20:50:10 UTC 2024
;; MSG SIZE  rcvd: 98
```

```bash
vagrant@dnsa:~$ dig @192.168.57.10 pc05.aulas.ies.test

; <<>> DiG 9.18.28-1~deb12u2-Debian <<>> @192.168.57.10 pc05.aulas.ies.test
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 2009
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: ffe882d26c36144f01000000672ff3b948cd60dc0e103ee0 (good)
;; QUESTION SECTION:
;pc05.aulas.ies.test.           IN      A

;; ANSWER SECTION:
pc05.aulas.ies.test.    604800  IN      A       192.168.57.102

;; Query time: 0 msec
;; SERVER: 192.168.57.10#53(192.168.57.10) (UDP)
;; WHEN: Sat Nov 09 23:43:53 UTC 2024
;; MSG SIZE  rcvd: 92
```

```bash
vagrant@dnsb:~$ dig @192.168.57.10 lengua.departamentos.ies.test

; <<>> DiG 9.18.28-1~deb12u2-Debian <<>> @192.168.57.10 lengua.departamentos.ies.test
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 31168
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 62cc156348bf42f001000000672fcb5935f8ca3b53f3f232 (good)
;; QUESTION SECTION:
;lengua.departamentos.ies.test. IN      A

;; ANSWER SECTION:
lengua.departamentos.ies.test. 604800 IN A      192.168.57.153

;; Query time: 4 msec
;; SERVER: 192.168.57.10#53(192.168.57.10) (UDP)
;; WHEN: Sat Nov 09 20:51:37 UTC 2024
;; MSG SIZE  rcvd: 102
```

**Consultas Inversas**

```bash
vagrant@dnsa:~$ dig @192.168.57.10 -x 192.168.57.21

; <<>> DiG 9.18.28-1~deb12u2-Debian <<>> @192.168.57.10 -x 192.168.57.21
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 11882
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 400b5b08901fda5101000000672ff3e539b3d28461b02245 (good)
;; QUESTION SECTION:
;21.57.168.192.in-addr.arpa.    IN      PTR

;; ANSWER SECTION:
21.57.168.192.in-addr.arpa. 604800 IN   PTR     pc01.informatica.ies.test.

;; Query time: 0 msec
;; SERVER: 192.168.57.10#53(192.168.57.10) (UDP)
;; WHEN: Sat Nov 09 23:44:37 UTC 2024
;; MSG SIZE  rcvd: 122
```

```bash
vagrant@dnsb:~$ dig @192.168.57.10 -x 192.168.57.103

; <<>> DiG 9.18.28-1~deb12u2-Debian <<>> @192.168.57.10 -x 192.168.57.103
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 5420
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 6cf6067971875a7a01000000672fcbca12f14dcba7919a6f (good)
;; QUESTION SECTION:
;103.57.168.192.in-addr.arpa.   IN      PTR

;; ANSWER SECTION:
103.57.168.192.in-addr.arpa. 604800 IN  PTR     pc06.aulas.ies.test.

;; Query time: 4 msec
;; SERVER: 192.168.57.10#53(192.168.57.10) (UDP)
;; WHEN: Sat Nov 09 20:53:30 UTC 2024
;; MSG SIZE  rcvd: 117
```

```bash
vagrant@dnsa:~$ dig @192.168.57.10 -x 192.168.57.152

; <<>> DiG 9.18.28-1~deb12u2-Debian <<>> @192.168.57.10 -x 192.168.57.152
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 11557
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: a6c45d7d6e6353e401000000672ff40017314663d51e5a39 (good)
;; QUESTION SECTION:
;152.57.168.192.in-addr.arpa.   IN      PTR

;; ANSWER SECTION:
152.57.168.192.in-addr.arpa. 604800 IN  PTR     ingles02.departamentos.ies.test.

;; Query time: 0 msec
;; SERVER: 192.168.57.10#53(192.168.57.10) (UDP)
;; WHEN: Sat Nov 09 23:45:04 UTC 2024
;; MSG SIZE  rcvd: 129
```

**Transferencia de zonas**

```bash
vagrant@dnsb:~$ dig @192.168.57.11 aulas.ies.test AXFR

; <<>> DiG 9.18.28-1~deb12u2-Debian <<>> @192.168.57.11 aulas.ies.test AXFR
; (1 server found)
;; global options: +cmd
aulas.ies.test.         604800  IN      SOA     ns1.aulas.ies.test. admin.ies.test. 2023110801 604800 86400 2419200 604800
aulas.ies.test.         604800  IN      NS      ns1.aulas.ies.test.
ns1.aulas.ies.test.     604800  IN      A       192.168.57.10
pc04.aulas.ies.test.    604800  IN      A       192.168.57.101
pc05.aulas.ies.test.    604800  IN      A       192.168.57.102
pc06.aulas.ies.test.    604800  IN      A       192.168.57.103
pc07.aulas.ies.test.    604800  IN      A       192.168.57.104
server02.aulas.ies.test. 604800 IN      A       192.168.57.100
aulas.ies.test.         604800  IN      SOA     ns1.aulas.ies.test. admin.ies.test. 2023110801 604800 86400 2419200 604800
;; Query time: 0 msec
;; SERVER: 192.168.57.11#53(192.168.57.11) (TCP)
;; WHEN: Sat Nov 09 23:34:56 UTC 2024
;; XFR size: 9 records (messages 1, bytes 292)
```


## Pruebas con `host`

**Consultas Directas**

```bash
vagrant@dnsa:~$ host pc04.aulas.ies.test 192.168.57.10
Using domain server:
Name: 192.168.57.10
Address: 192.168.57.10#53
Aliases: 

pc04.aulas.ies.test has address 192.168.57.101
```

```bash
vagrant@dnsb:~$ host tic.departamentos.ies.test 192.168.57.10
Using domain server:
Name: 192.168.57.10
Address: 192.168.57.10#53
Aliases: 

tic.departamentos.ies.test has address 192.168.57.154
```

```bash
vagrant@dnsa:~$ host pc03.informatica.ies.test 192.168.57.10
Using domain server:
Name: 192.168.57.10
Address: 192.168.57.10#53
Aliases: 

pc03.informatica.ies.test has address 192.168.57.23
```

**Consultas Inversas**

```bash
vagrant@dnsb:~$ host 192.168.57.101 192.168.57.10
Using domain server:
Name: 192.168.57.10
Address: 192.168.57.10#53
Aliases: 

101.57.168.192.in-addr.arpa domain name pointer pc04.aulas.ies.test.
```

```bash
vagrant@dnsa:~$ host 192.168.57.153 192.168.57.10
Using domain server:
Name: 192.168.57.10
Address: 192.168.57.10#53
Aliases: 

153.57.168.192.in-addr.arpa domain name pointer lengua.departamentos.ies.test.
```

```bash
vagrant@dnsb:~$ host 192.168.57.23 192.168.57.10
Using domain server:
Name: 192.168.57.10
Address: 192.168.57.10#53
Aliases: 

23.57.168.192.in-addr.arpa domain name pointer pc03.informatica.ies.test.
```

Tras realizar todas las pruebas con `nslookup`, `dig`, y `host`, en ambos servidores `DNSA` y `DNSB`, se confirma que la configuración de DNS funciona correctamente y cumple con los requisitos. Las consultas directas e inversas se resuelven de manera correcta en todas las zonas y las transferencias de zona entre `DNSA` y `DNSB` funcionan efectivamente.