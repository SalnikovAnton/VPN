# VPN
### 1) Между двумя виртуалками поднять vpn в режимах:   
- tun   
- tap   
Описать в чём разница, замерить скорость между виртуальными машинами в туннелях, сделать вывод об отличающихся показателях скорости.   
### 2) Поднять RAS на базе OpenVPN с клиентскими сертификатами, подключиться с локальной машины на виртуалку.

#### 1) Для выполнения первого пункта необходимо:   
* написать [Vagrantfile](https://github.com/SalnikovAnton/VPN/blob/main/Vagrantfile "Vagrantfile"), который будет поднимать 2 виртуальные машины server и client,
* пишем роли в [serverplaybook](https://github.com/SalnikovAnton/VPN/blob/main/serverplaybook.yml "serverplaybook.yml") для сервера и [clientplaybook](https://github.com/SalnikovAnton/VPN/blob/main/clientplaybook.yml "clientplaybook.yml") для слиентской машины,
* создаем [server.conf](https://github.com/SalnikovAnton/VPN/blob/main/server/server.conf "server.conf") для сервера и [server.conf](https://github.com/SalnikovAnton/VPN/blob/main/client/server.conf "server.conf") для клиента.
   
```

```

