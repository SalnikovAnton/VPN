# VPN
### 1) Между двумя виртуалками поднять vpn в режимах:   
- tun   
- tap   
Описать в чём разница, замерить скорость между виртуальными машинами в туннелях, сделать вывод об отличающихся показателях скорости.   
### 2) Поднять RAS на базе OpenVPN с клиентскими сертификатами, подключиться с локальной машины на виртуалку.

#### 1) Для выполнения первого пункта необходимо:   
* написать [Vagrantfile](https://github.com/SalnikovAnton/VPN/blob/main/Vagrantfile "Vagrantfile"), который будет поднимать 2 виртуальные машины server и client,
* пишем роли в [serverplaybook](https://github.com/SalnikovAnton/VPN/blob/main/serverplaybook.yml "serverplaybook.yml") для сервера и [clientplaybook](https://github.com/SalnikovAnton/VPN/blob/main/clientplaybook.yml "clientplaybook.yml") для слиентской машины,
* создаем [server.conf](https://github.com/SalnikovAnton/VPN/blob/main/server/server.conf "server.conf"), [openvpn@.service](https://github.com/SalnikovAnton/VPN/blob/main/server/openvpn@.service "openvpn@.service") для сервера и [server.conf](https://github.com/SalnikovAnton/VPN/blob/main/client/server.conf "server.conf"), [openvpn@.service](https://github.com/SalnikovAnton/VPN/blob/main/server/openvpn@.service "openvpn@.service") для клиента.
   
```
[root@server ~]# systemctl status openvpn@server
● openvpn@server.service - OpenVPN Tunneling Application On server
   Loaded: loaded (/etc/systemd/system/openvpn@.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2023-07-05 18:23:50 UTC; 18min ago
 Main PID: 11220 (openvpn)
   Status: "Initialization Sequence Completed"
    Tasks: 1 (limit: 2749)
   Memory: 1.5M
   CGroup: /system.slice/system-openvpn.slice/openvpn@server.service
           └─11220 /usr/sbin/openvpn --cd /etc/openvpn/ --config server.conf

Jul 05 18:23:50 server.loc systemd[1]: Starting OpenVPN Tunneling Application On server...
Jul 05 18:23:50 server.loc systemd[1]: Started OpenVPN Tunneling Application On server.

```
#### Замеряем скорость в режиме TAP   
Hа сервере запускаем iperf3 в режиме сервера
```
[root@server ~]# iperf3 -s &
[1] 24487
[root@server ~]# -----------------------------------------------------------
Server listening on 5201
-----------------------------------------------------------
Accepted connection from 10.10.10.2, port 41766
[  5] local 10.10.10.1 port 5201 connected to 10.10.10.2 port 41768
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-1.01   sec  5.99 MBytes  49.8 Mbits/sec                  
[  5]   1.01-2.00   sec  6.71 MBytes  56.7 Mbits/sec                  
[  5]   2.00-3.00   sec  6.88 MBytes  57.7 Mbits/sec                  
[  5]   3.00-4.00   sec  7.20 MBytes  60.2 Mbits/sec                  
[  5]   4.00-5.00   sec  6.64 MBytes  55.8 Mbits/sec                  
[  5]   5.00-6.00   sec  6.75 MBytes  56.7 Mbits/sec                  
[  5]   6.00-7.00   sec  7.36 MBytes  61.7 Mbits/sec                  
[  5]   7.00-8.00   sec  6.74 MBytes  56.6 Mbits/sec                  
[  5]   8.00-9.00   sec  7.40 MBytes  62.1 Mbits/sec                  
[  5]   9.00-10.00  sec  5.49 MBytes  46.0 Mbits/sec                  
[  5]  10.00-11.00  sec  6.40 MBytes  53.7 Mbits/sec                  
[root@server ~]# [  5]  11.00-12.00  sec  6.73 MBytes  56.3 Mbits/sec                  
[  5]  12.00-13.00  sec  6.28 MBytes  52.9 Mbits/sec                  
[  5]  13.00-14.00  sec  6.07 MBytes  50.9 Mbits/sec                  
[  5]  14.00-15.00  sec  7.27 MBytes  61.1 Mbits/sec                  
[  5]  15.00-16.00  sec  5.63 MBytes  47.2 Mbits/sec                  
[  5]  16.00-17.00  sec  5.93 MBytes  49.8 Mbits/sec                  
[  5]  17.00-18.00  sec  6.61 MBytes  55.4 Mbits/sec                  
[  5]  18.00-19.00  sec  7.08 MBytes  59.4 Mbits/sec                  
[  5]  19.00-20.00  sec  6.77 MBytes  56.8 Mbits/sec                  
[  5]  20.00-21.00  sec  6.03 MBytes  50.6 Mbits/sec                  
[  5]  21.00-22.00  sec  6.26 MBytes  52.5 Mbits/sec                  
[  5]  22.00-23.00  sec  6.13 MBytes  51.4 Mbits/sec                  
[  5]  23.00-24.00  sec  6.55 MBytes  55.0 Mbits/sec                  
[  5]  24.00-25.00  sec  7.47 MBytes  62.7 Mbits/sec                  
[  5]  25.00-26.00  sec  7.43 MBytes  62.3 Mbits/sec                  
[  5]  26.00-27.00  sec  5.88 MBytes  49.3 Mbits/sec                  
[  5]  27.00-28.00  sec  7.04 MBytes  59.1 Mbits/sec                  
[  5]  28.00-29.00  sec  7.44 MBytes  62.4 Mbits/sec                  
[  5]  29.00-30.00  sec  6.12 MBytes  51.3 Mbits/sec                  
[  5]  30.00-31.01  sec  5.50 MBytes  45.9 Mbits/sec                  
[  5]  31.01-32.00  sec  3.86 MBytes  32.5 Mbits/sec                  
[  5]  32.00-33.00  sec  5.07 MBytes  42.6 Mbits/sec                  
[  5]  33.00-34.00  sec  6.93 MBytes  58.1 Mbits/sec                  
[  5]  34.00-35.00  sec  7.47 MBytes  62.7 Mbits/sec                  
[  5]  35.00-36.00  sec  6.11 MBytes  51.3 Mbits/sec                  
[  5]  36.00-37.00  sec  7.01 MBytes  58.8 Mbits/sec                  
[  5]  37.00-38.00  sec  6.08 MBytes  51.0 Mbits/sec                  
[  5]  38.00-39.00  sec  5.65 MBytes  47.4 Mbits/sec                  
[  5]  39.00-40.00  sec  7.24 MBytes  60.7 Mbits/sec                  
[  5]  40.00-40.02  sec  94.2 KBytes  35.5 Mbits/sec                  
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-40.02  sec   259 MBytes  54.3 Mbits/sec                  receiver
-----------------------------------------------------------
Server listening on 5201
-----------------------------------------------------------
```
на клиенте запускаем iperf3 в режиме клиента и замеряем скорость в туннеле
```
[root@client ~]# iperf3 -c 10.10.10.1 -t 40 -i 5
Connecting to host 10.10.10.1, port 5201
[  5] local 10.10.10.2 port 41768 connected to 10.10.10.1 port 5201
[ ID] Interval           Transfer     Bitrate         Retr  Cwnd
[  5]   0.00-5.00   sec  33.8 MBytes  56.8 Mbits/sec   16   91.6 KBytes       
[  5]   5.00-10.00  sec  33.7 MBytes  56.5 Mbits/sec    8   78.7 KBytes       
[  5]  10.00-15.00  sec  32.8 MBytes  55.0 Mbits/sec   14   92.9 KBytes       
[  5]  15.00-20.00  sec  32.1 MBytes  53.8 Mbits/sec    7    106 KBytes       
[  5]  20.00-25.01  sec  32.4 MBytes  54.3 Mbits/sec   11   86.4 KBytes       
[  5]  25.01-30.00  sec  33.8 MBytes  56.9 Mbits/sec   14    108 KBytes       
[  5]  30.00-35.01  sec  28.8 MBytes  48.3 Mbits/sec    8    104 KBytes       
[  5]  35.01-40.01  sec  32.1 MBytes  53.8 Mbits/sec   18   80.0 KBytes       
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bitrate         Retr
[  5]   0.00-40.01  sec   260 MBytes  54.4 Mbits/sec   96             sender
[  5]   0.00-40.02  sec   259 MBytes  54.3 Mbits/sec                  receiver

iperf Done.
```
#### Замеряем скорость в режиме TUN   
Правим файл /etc/openvpn/server.conf в директиве dev значение tap меняем на значение tun. Hа сервере запускаем iperf3 в режиме сервера
```
[root@server ~]# iperf3 -s &
[2] 24532
[root@server ~]# iperf3: error - unable to start listener for connections: Address already in use
iperf3: exiting
Accepted connection from 10.10.10.2, port 41770
[  5] local 10.10.10.1 port 5201 connected to 10.10.10.2 port 41772
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-1.00   sec  6.33 MBytes  53.1 Mbits/sec                  
[  5]   1.00-2.00   sec  7.59 MBytes  63.7 Mbits/sec                  
[  5]   2.00-3.01   sec  6.78 MBytes  56.3 Mbits/sec                  
[  5]   3.01-4.00   sec  7.15 MBytes  60.6 Mbits/sec                  
[  5]   4.00-5.00   sec  6.72 MBytes  56.4 Mbits/sec                  
[  5]   5.00-6.00   sec  7.20 MBytes  60.4 Mbits/sec                  
[  5]   6.00-7.00   sec  7.11 MBytes  59.7 Mbits/sec                  
[  5]   7.00-8.00   sec  7.51 MBytes  63.0 Mbits/sec                  
[  5]   8.00-9.00   sec  6.25 MBytes  52.4 Mbits/sec                  
[  5]   9.00-10.00  sec  7.76 MBytes  65.2 Mbits/sec                  
[  5]  10.00-11.00  sec  7.39 MBytes  62.0 Mbits/sec                  
[  5]  11.00-12.00  sec  6.91 MBytes  57.9 Mbits/sec                  
[  5]  12.00-13.00  sec  5.02 MBytes  42.1 Mbits/sec                  
[  5]  13.00-14.00  sec  2.86 MBytes  24.0 Mbits/sec                  
[  5]  14.00-15.00  sec  6.47 MBytes  54.3 Mbits/sec                  
[  5]  15.00-16.00  sec  6.71 MBytes  56.3 Mbits/sec                  
[  5]  16.00-17.00  sec  5.95 MBytes  50.0 Mbits/sec                  
[  5]  17.00-18.00  sec  7.37 MBytes  61.8 Mbits/sec                  
[  5]  18.00-19.00  sec  7.30 MBytes  61.3 Mbits/sec                  
[  5]  19.00-20.01  sec  7.07 MBytes  58.7 Mbits/sec                  
[  5]  20.01-21.00  sec  6.40 MBytes  54.2 Mbits/sec                  
[  5]  21.00-22.00  sec  6.54 MBytes  54.9 Mbits/sec                  
[  5]  22.00-23.00  sec  4.24 MBytes  35.5 Mbits/sec                  
[  5]  23.00-24.00  sec  3.45 MBytes  29.0 Mbits/sec                  
[  5]  24.00-25.00  sec  5.22 MBytes  43.8 Mbits/sec                  
[  5]  25.00-26.00  sec  4.18 MBytes  35.1 Mbits/sec                  
[  5]  26.00-27.01  sec  4.00 MBytes  33.4 Mbits/sec                  
[  5]  27.01-28.00  sec  5.45 MBytes  46.0 Mbits/sec                  
[  5]  28.00-29.00  sec  6.46 MBytes  54.2 Mbits/sec                  
[  5]  29.00-30.00  sec  6.47 MBytes  54.2 Mbits/sec                  
[  5]  30.00-31.00  sec  6.88 MBytes  57.9 Mbits/sec                  
[  5]  31.00-32.00  sec  6.03 MBytes  50.6 Mbits/sec                  
[  5]  32.00-33.00  sec  6.73 MBytes  56.4 Mbits/sec                  
[  5]  33.00-34.00  sec  4.66 MBytes  39.1 Mbits/sec                  
[  5]  34.00-35.00  sec  2.66 MBytes  22.3 Mbits/sec                  
[  5]  35.00-36.00  sec  5.90 MBytes  49.4 Mbits/sec                  
[  5]  36.00-37.00  sec  5.03 MBytes  42.2 Mbits/sec                  
[  5]  37.00-38.00  sec  4.22 MBytes  35.4 Mbits/sec                  
[  5]  38.00-39.00  sec  5.89 MBytes  49.5 Mbits/sec                  
[  5]  39.00-40.00  sec  6.80 MBytes  57.1 Mbits/sec                  
[  5]  40.00-40.14  sec   133 KBytes  7.93 Mbits/sec                  
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-40.14  sec   241 MBytes  50.3 Mbits/sec                  receiver

```
на клиенте запускаем iperf3 в режиме клиента и замеряем скорость в туннеле
```
[root@client ~]# iperf3 -c 10.10.10.1 -t 40 -i 5
Connecting to host 10.10.10.1, port 5201
[  5] local 10.10.10.2 port 41772 connected to 10.10.10.1 port 5201
[ ID] Interval           Transfer     Bitrate         Retr  Cwnd
[  5]   0.00-5.00   sec  35.0 MBytes  58.7 Mbits/sec   16    100 KBytes       
[  5]   5.00-10.00  sec  35.9 MBytes  60.3 Mbits/sec   15   97.8 KBytes       
[  5]  10.00-15.01  sec  28.6 MBytes  48.0 Mbits/sec   12   87.2 KBytes       
[  5]  15.01-20.00  sec  34.4 MBytes  57.7 Mbits/sec    9    110 KBytes       
[  5]  20.00-25.00  sec  25.9 MBytes  43.4 Mbits/sec   10    110 KBytes       
[  5]  25.00-30.01  sec  26.6 MBytes  44.5 Mbits/sec   10    115 KBytes       
[  5]  30.01-35.01  sec  26.8 MBytes  45.0 Mbits/sec   10   88.5 KBytes       
[  5]  35.01-40.00  sec  27.8 MBytes  46.7 Mbits/sec    7    102 KBytes       
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bitrate         Retr
[  5]   0.00-40.00  sec   241 MBytes  50.5 Mbits/sec   89             sender
[  5]   0.00-40.14  sec   241 MBytes  50.3 Mbits/sec                  receiver

iperf Done.
```
#### Отличия tun и tap нитерфесов:   
* tap старается больше походить на реальный сетевой интерфейс, а именно он позволяет себе принимать и отправлять ARP запросы, обладает MAC адресом и может являться одним из интерфейсов сетевого моста, так как он обладает полной поддержкой ethernet - протокола канального уровня (уровень 2).
* Интерфейс tun этой поддержки лишен, поэтому он может принимать и отправлять только IP пакеты и никак не ethernet кадры. Он не обладает MAC-адресом и не может быть добавлен в бридж. Зато он более легкий и быстрый за счет отсутствия дополнительной инкапсуляции и прекрасно подходит для тестирования сетевого стека или построения виртуальных частных сетей (VPN)

# RAS на базе OpenVPN   
#### Для выполнения необходимо:   
* написать [Vagrantfile](https://github.com/SalnikovAnton/VPN/blob/main/Vagrantfile2 "Vagrantfile"), который будет поднимать 1 виртуальную машину serverras,
* пишем роли в [serverras](https://github.com/SalnikovAnton/VPN/blob/main/serverras.yml "serverras.yml") для сервера,
* создаем [server.conf](https://github.com/SalnikovAnton/VPN/blob/main/ras/server.conf "server.conf"), [openvpn@.service](https://github.com/SalnikovAnton/VPN/blob/main/server/openvpn@.service "openvpn@.service") для сервера и [client.conf](https://github.com/SalnikovAnton/VPN/blob/main/ras/client.conf "client.conf") для клиента.

После поднятия машины подключаемся к ней по ssh, переходим в директорию /etc/openvpn/ и инициализируем pki
```
[root@serverras ~]# cd /etc/openvpn/
[root@serverras openvpn]# rpm -qa | grep easy-rsa
easy-rsa-3.0.8-1.el8.noarch
[root@serverras openvpn]# /usr/share/easy-rsa/3.0.8/easyrsa init-pki

init-pki complete; you may now create a CA or requests.
Your newly created PKI dir is: /etc/openvpn/pki
```
Сгенерируем необходимые ключи и сертификаты для сервера
```
[root@serverras openvpn]# echo 'rasvpn' | /usr/share/easy-rsa/3.0.8/easyrsa build-ca nopass
Using SSL: openssl OpenSSL 1.1.1g FIPS  21 Apr 2020
Generating RSA private key, 2048 bit long modulus (2 primes)
***
```
```
[root@serverras openvpn]# echo 'rasvpn' | /usr/share/easy-rsa/3.0.8/easyrsa gen-req server nopass
Using SSL: openssl OpenSSL 1.1.1g FIPS  21 Apr 2020
Generating a RSA private key
***
```
```
[root@serverras openvpn]# echo 'yes' | /usr/share/easy-rsa/3.0.8/easyrsa sign-req server server
Using SSL: openssl OpenSSL 1.1.1g FIPS  21 Apr 2020
***
```
```
[root@serverras openvpn]# /usr/share/easy-rsa/3.0.8/easyrsa gen-dh
Using SSL: openssl OpenSSL 1.1.1g FIPS  21 Apr 2020
Generating DH parameters, 2048 bit long safe prime, generator 2
***
```
```
[root@serverras openvpn]# openvpn --genkey --secret ca.key
```
Сгенерируем сертификаты для клиента
```
[root@serverras openvpn]# echo 'client' | /usr/share/easy-rsa/3/easyrsa gen-req client nopass
Using SSL: openssl OpenSSL 1.1.1g FIPS  21 Apr 2020
Generating a RSA private key
```
```
[root@serverras openvpn]# echo 'yes' | /usr/share/easy-rsa/3/easyrsa sign-req client client
Using SSL: openssl OpenSSL 1.1.1g FIPS  21 Apr 2020
****
```
Зададим параметр iroute для клиента и запускаем openvpn сервер и добавляем его в автозагрузку
```
[root@serverras openvpn]# echo 'iroute 10.10.10.0 255.255.255.0' > /etc/openvpn/client/client
[root@serverras openvpn]# systemctl start openvpn@server
[root@serverras openvpn]# systemctl enable openvpn@server
Created symlink /etc/systemd/system/multi-user.target.wants/openvpn@server.service → /etc/systemd/system/openvpn@.service.
```
Скопируем следующие файлы сертификатов и ключ для клиента на хост-машину.   
* /etc/openvpn/pki/ca.crt
* /etc/openvpn/pki/issued/client.crt
* /etc/openvpn/pki/private/client.key   
После того, как все готово, подключаемся к openvpn сервер с хост-машины.
```
 ~/vpn/ras  sudo openvpn --config client.conf
```
проверяем пинг по внутреннему IP адресу сервера в туннеле.
```
ping -c 4 10.10.10.1    
PING 10.10.10.1 (10.10.10.1) 56(84) bytes of data.
64 bytes from 10.10.10.1: icmp_seq=1 ttl=63 time=3.31 ms
64 bytes from 10.10.10.1: icmp_seq=2 ttl=63 time=15.2 ms
64 bytes from 10.10.10.1: icmp_seq=3 ttl=63 time=38.9 ms
64 bytes from 10.10.10.1: icmp_seq=4 ttl=63 time=4.09 ms

--- 10.10.10.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3003ms
rtt min/avg/max/mdev = 3.313/15.397/38.946/14.391 ms
```
Также проверяем командой ip r (netstat -rn) на хостовой машине, что сеть туннеля импортирована в таблицу маршрутизации.
```
ip r
default via 10.0.2.2 dev eth0 proto dhcp metric 101 
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 101 
10.10.10.5 dev tun0 proto kernel scope link src 10.10.10.6 
192.168.56.0/24 via 10.10.10.5 dev tun0 
192.168.56.0/24 dev eth1 proto kernel scope link src 192.168.56.20 metric 100 
```
