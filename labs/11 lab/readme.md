![image](https://user-images.githubusercontent.com/99355274/173903231-38d1a46c-aa7b-4a02-ab20-7ba1e8bf5d5d.png)

## Часть 1: Создание сети и настройка основных параметров устройства
### Шаг 1. Создайте сеть согласно топологии.
![image](https://user-images.githubusercontent.com/99355274/173903265-a06a2e4e-55e7-4930-a7ef-f5b0898aedb0.png)


### Шаг 2. Произведите базовую настройку маршрутизаторов и коммутаторов.
**Маршрутизатор R1**
```sh
Router(config)#hostname R1
R1(config)#no ip domain-lookup
R1(config)#enable password class
R1(config)#line con 0
R1(config-line)#password cisco
R1(config-line)#line vty 0 4
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
R1(config)#service password-encryption
R1(config)#banner motd # Attention! For staff only! #
R1(config)#do wr
Building configuration...
[OK]
R1(config)#
```
**Маршрутизатор R2**
```sh
Router(config)#hostname R2
R2(config)#no ip domain-lookup
R2(config)#enable password class
R2(config)#line con 0
R2(config-line)#password cisco
R2(config-line)#line vty 0 4
R2(config-line)#password cisco
R2(config-line)#login
R2(config-line)#exit
R2(config)#service password-encryption
R2(config)#banner motd # Attention! For staff only! #
R2(config)#do wr
Building configuration...
[OK]
R2(config)#
```
**Коммутатор S1**
```sh
Switch(config)#hostname S1
S1(config)#no ip domain-lookup
S1(config)#enable password class
S1(config)#line con 0
S1(config-line)#
S1(config-line)#password cisco
S1(config-line)#line vty 0 4
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#service password-encryption
S1(config)#banner motd # Attention! For staff only! #
S1(config)#do wr
```
**Коммутатор S2**
```sh
Switch(config)#hostname S2
S2(config)#no ip domain-lookup
S2(config)#enable password class
S2(config)#line con 0
S2(config-line)#
S2(config-line)#password cisco
S2(config-line)#line vty 0 4
S2(config-line)#password cisco
S2(config-line)#login
S2(config-line)#exit
S2(config)#service password-encryption
S2(config)#banner motd # Attention! For staff only! #
S2(config)#do wr
```
## Часть 2: Настройка сетей VLAN на коммутаторах.
### Шаг 1. Создайте сети VLAN на коммутаторах.
**Коммутатор S1**
```sh
S1(config)#vlan 20
S1(config-vlan)#name Managment
S1(config-vlan)#vlan 30
S1(config-vlan)#name Operations
S1(config-vlan)#vlan 40
S1(config-vlan)#name Sales
S1(config-vlan)#vlan 999
S1(config-vlan)#name ParkingLot
S1(config-vlan)#vlan 1000
S1(config-vlan)#name Own
```
**Коммутатор S2**
```sh
S2(config)#vlan 20
S2(config-vlan)#name Managment
S2(config-vlan)#vlan 30
S2(config-vlan)#name Operations
S2(config-vlan)#vlan 40
S2(config-vlan)#name Sales
S2(config-vlan)#vlan 999
S2(config-vlan)#name ParkingLot
S2(config-vlan)#vlan 1000
S2(config-vlan)#name Own
```
**Коммутатор S1**
```sh
S1(config-if)#ip address 10.20.0.2 255.255.255.0
S1(config)#ip default-gateway 10.20.0.1
S1(config)#do sh ip int br
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     unassigned      YES unset  up                    up
GigabitEthernet0/1     unassigned      YES unset  up                    up
GigabitEthernet0/2     unassigned      YES unset  up                    up
GigabitEthernet0/3     unassigned      YES unset  up                    up
GigabitEthernet1/0     unassigned      YES unset  up                    up
GigabitEthernet1/1     unassigned      YES unset  up                    up
GigabitEthernet1/2     unassigned      YES unset  up                    up
GigabitEthernet1/3     unassigned      YES unset  up                    up
Vlan20                 10.20.0.2       YES NVRAM  up                    up
S1(config)#int g0/3
S1(config-if)#sw a vlan 999
S1(config-if)#int range g1/0-3
S1(config-if-range)#sw a vlan 999
S1(config-if-range)#
```
**Коммутатор S2**
```sh
S2(config-if)#ip address 10.20.0.3 255.255.255.0
S2(config)#ip default-gateway 10.20.0.1
S2(config)#do sh ip int br
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     unassigned      YES unset  up                    up
GigabitEthernet0/1     unassigned      YES unset  up                    up
GigabitEthernet0/2     unassigned      YES unset  up                    up
GigabitEthernet0/3     unassigned      YES unset  up                    up
GigabitEthernet1/0     unassigned      YES unset  up                    up
GigabitEthernet1/1     unassigned      YES unset  up                    up
GigabitEthernet1/2     unassigned      YES unset  up                    up
GigabitEthernet1/3     unassigned      YES unset  up                    up
Vlan20                 10.20.0.3       YES NVRAM  up                    up
S2(config)#int g0/3
S2(config-if)#sw a vlan 999
S2(config-if)#int range g1/0-3
S2(config-if-range)#sw a vlan 999
S2(config-if-range)#
```
### Шаг 2. Назначьте сети VLAN соответствующим интерфейсам коммутатора.
**Коммутатор S1**
```sh
S1(config-if)#int g0/2
S1(config-if)#sw m a
S1(config-if)#sw a vlan 30
S1(config-if)#
S1#sh vlan br

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Gi0/0, Gi0/1
20   Managment                        active
30   Operations                       active    Gi0/2
40   Sales                            active
999  ParkingLot                       active    Gi0/3, Gi1/0, Gi1/1, Gi1/2
                                                Gi1/3
1000 Own                              active
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup

```
**Коммутатор S2**
```sh
S2(config)#int g0/2
S2(config-if)#sw m a
S2(config-if)#sw a vlan 40
S2(config-if)#
S2#sh vlan br

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Gi0/0, Gi0/1
20   Managment                        active
30   Operations                       active
40   Sales                            active    Gi0/2
999  ParkingLot                       active    Gi0/3, Gi1/0, Gi1/1, Gi1/2
                                                Gi1/3
1000 Own                              active
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup
```

## Часть 3: Настройте транки (магистральные каналы).
### Шаг 1. Вручную настройте магистральный интерфейс F0/1(g0/1).
**Коммутатор S1**
```sh
S1(config)#int g0/1
S1(config-if)#switchport trunk encapsulation dot1q
S1(config-if)#switchport mode trunk
S1(config-if)#
S1(config-if)#
S1(config-if)#switchport trunk native vlan 1000
S1(config-if)#switchport trunk allowed vlan 10,20,30,1000
S1#sh int trunk

Port        Mode             Encapsulation  Status        Native vlan
Gi0/1       on               802.1q         trunking      1000

Port        Vlans allowed on trunk
Gi0/1       10,20,30,1000

Port        Vlans allowed and active in management domain
Gi0/1       20,30,1000

Port        Vlans in spanning tree forwarding state and not pruned
Gi0/1       20,30,1000
S1#
```
**Коммутатор S2**
```sh
S2(config)#int g0/1
S2(config-if)#switchport trunk encapsulation dot1q
S2(config-if)#switchport mode trunk
S2(config-if)#
S2(config-if)#
S2(config-if)#switchport trunk native vlan 1000
S2(config-if)#switchport trunk allowed vlan 10,20,30,1000
S2#sh int trunk

Port        Mode             Encapsulation  Status        Native vlan
Gi0/1       on               802.1q         trunking      1000

Port        Vlans allowed on trunk
Gi0/1       10,20,30,1000

Port        Vlans allowed and active in management domain
Gi0/1       20,30,1000

Port        Vlans in spanning tree forwarding state and not pruned
Gi0/1       20,30,1000
S2#
```
### Шаг 2. Вручную настройте магистральный интерфейс F0/5(g0/0) на коммутаторе S1.
```sh
S1(config)#int g0/0
S1(config-if)#switchport trunk encapsulation dot1q
S1(config-if)#switchport mode trunk
S1(config-if)#switchport trunk native vlan 1000
S1(config-if)#switchport trunk allowed vlan 10,20,30,1000
S1(config-if)do #wr

S1#sh interfaces trunk

Port        Mode             Encapsulation  Status        Native vlan
Gi0/0       on               802.1q         trunking      1000
Gi0/1       on               802.1q         trunking      1000

Port        Vlans allowed on trunk
Gi0/0       10,20,30,1000
Gi0/1       10,20,30,1000

Port        Vlans allowed and active in management domain
Gi0/0       20,30,1000
Gi0/1       20,30,1000

Port        Vlans in spanning tree forwarding state and not pruned
Gi0/0       20,30,1000
Gi0/1       20,30,1000
```
## Часть 4: Настройте маршрутизацию.
### Шаг 1. Настройка маршрутизации между сетями VLAN на R1.
```sh
R1(config)#int g0/1
R1(config-if)#no sh
R1(config-if)#int g0/1.20
R1(config-subif)#encapsulation dot1Q 20
R1(config-subif)#ip address 10.20.0.1 255.255.255.0
R1(config-subif)#description Managment
R1(config-subif)#int g0/1.30
R1(config-subif)#encapsulation dot1Q 30
R1(config-subif)#ip address 10.30.0.1 255.255.255.0
R1(config-subif)#description Operations
R1(config-subif)#int g0/1.40
R1(config-subif)#encapsulation dot1Q 40
R1(config-subif)#ip address 10.40.0.1 255.255.255.0
R1(config-subif)#description Sales
R1(config-subif)#int g0/1.1000
R1(config-subif)#des
R1(config-subif)#description Own
R1(config-subif)#ex
R1(config)#
```
```sh
R1(config)#int loopback1
R1(config-if)#ip address 172.16.1.1 255.255.255.0
R1(config-if)#no sh
R1(config-if)#

R1#sh ip int br
Interface                  IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0         unassigned      YES unset  administratively down down
GigabitEthernet0/1         unassigned      YES unset  up                    up  
GigabitEthernet0/1.20      10.20.0.1       YES manual up                    up  
GigabitEthernet0/1.30      10.30.0.1       YES manual up                    up  
GigabitEthernet0/1.40      10.40.0.1       YES manual up                    up  
GigabitEthernet0/1.1000    unassigned      YES unset  up                    up  
GigabitEthernet0/2         unassigned      YES unset  administratively down down
GigabitEthernet0/3         unassigned      YES unset  administratively down down
Loopback1                  172.16.1.1      YES manual up                    up  
```
### Шаг 2. Настройка интерфейса R2 g0/0/1 с использованием адреса из таблицы и маршрута по умолчанию с адресом следующего перехода 10.20.0.1
```sh
R2(config)#int g0/1
R2(config-if)#no sh
R2(config-if)#ip address 10.20.0.4 255.255.255.0
R2(config-if)#ex
R2(config)#ip default-gateway 10.20.0.1
R2(config)#
```
## Часть 5: Настройте удаленный доступ
### Шаг 1. Настройте все сетевые устройства для базовой поддержки SSH.
**Маршрутизатор R1**
```sh
R1(config)#username SSHadmin secret $cisco123!
R1(config)#ip domain name ccna-lab.com
R1(config)#crypto key generate rsa modulus 1024
R1(config)#line vty 0 4
R1(config)#transport input ssh
R1(config)#ip ssh version 2
```
**Маршрутизатор R2**
```sh
R2(config)#username SSHadmin secret $cisco123!
R2(config)#ip domain name ccna-lab.com
R2(config)#crypto key generate rsa modulus 1024
R2(config)#line vty 0 4
R2(config)#transport input ssh
R2(config)#ip ssh version 2
```
**Коммутатор S1**
```sh
S1(config)#username SSHadmin secret $cisco123!
S1(config)#ip domain name ccna-lab.com
S1(config)#crypto key generate rsa modulus 1024
S1(config)#line vty 0 4
S1(config)#transport input ssh
S1(config)#ip ssh version 2
```
**Коммутатор S2**
```sh
S2(config)#username SSHadmin secret $cisco123!
S2(config)#ip domain name ccna-lab.com
S2(config)#crypto key generate rsa modulus 1024
S2(config)#line vty 0 4
S2(config)#transport input ssh
S2(config)#ip ssh version 2
```
### Шаг 2. Включите защищенные веб-службы с проверкой подлинности на R1.
```sh
R1(config)#ip http secure-server
R1(config)# ip http authentication local
```
## Часть 6: Проверка подключения
### Шаг 1. Настройте узлы ПК.

![image](https://user-images.githubusercontent.com/99355274/173118441-9afa8c8b-d77d-41a5-a047-514c3ee93c46.png)

![image](https://user-images.githubusercontent.com/99355274/173118504-875f416e-a317-4b6f-96af-fe6242ed5d95.png)

### Шаг 2. Выполните следующие тесты. Эхозапрос должен пройти успешно.

![image](https://user-images.githubusercontent.com/99355274/173132732-bb85fe76-01e7-4270-ac7f-2e05a7cd1d4e.png)

![image](https://user-images.githubusercontent.com/99355274/173132775-34bfcfc2-9e07-4665-b0df-ff6004834159.png)

![image](https://user-images.githubusercontent.com/99355274/173132964-fe1c64e0-f604-4511-96df-b3b5d98d22fe.png)

![image](https://user-images.githubusercontent.com/99355274/173132995-875b784c-c4e1-4aa4-96a6-b70e5463df99.png)

![image](https://user-images.githubusercontent.com/99355274/173133047-4629d9eb-df19-4235-99bf-612b864ec4ce.png)

![image](https://user-images.githubusercontent.com/99355274/173133074-b5023b7a-867d-4ac0-bf76-3c1f298538d8.png)

## Часть 7: Настройка и проверка списков контроля доступа (ACL)


### Шаг 1. Проанализируйте требования к сети и политике безопасности для планирования реализации ACL.

Политика1. Сеть Sales не может использовать SSH в сети Management (но в  другие сети SSH разрешен).

Политика 2. Сеть Sales не имеет доступа к IP-адресам в сети Management с помощью любого веб-протокола (HTTP/HTTPS). Сеть Sales также не имеет доступа к интерфейсам R1 с помощью любого веб-протокола. Разрешён весь другой веб-трафик (обратите внимание — Сеть Sales  может получить доступ к интерфейсу Loopback 1 на R1).

Политика3. Сеть Sales не может отправлять эхо-запросы ICMP в сети Operations или Management. Разрешены эхо-запросы ICMP к другим адресатам. 

Политика 4: Cеть Operations  не может отправлять ICMP эхозапросы в сеть Sales. Разрешены эхо-запросы ICMP к другим адресатам. 

### Шаг 2. Разработка и применение расширенных списков доступа, которые будут соответствовать требованиям политики безопасности.
```sh
R1(config)#ip access-list extended ALL_POLICY
R1(config-ext-nacl)#deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 22
R1(config-ext-nacl)#deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 443
R1(config-ext-nacl)#deny icmp 10.40.0.0 0.0.0.255 10.30.0.0 0.0.0.255
R1(config-ext-nacl)#deny icmp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255
R1(config-ext-nacl)#permit ip any any
R1(config)#int g0/1.40
R1(config-subif)#ip access-group ALL_POLICY in

R1(config)#ip access-list extended ICMP_OP
R1(config-ext-nacl)#deny icmp 10.30.0.0 0.0.0.255 10.40.0.0 0.0.0.255
R1(config-ext-nacl)#permit ip any any
R1(config-ext-nacl)#int g0/1.30
R1(config-subif)#ip access-group ICMP_OP in
```
### Шаг 3. Убедитесь, что политики безопасности применяются развернутыми списками доступа.
**PC-A ping 10.40.0.10 | 10.20.0.1**
![image](https://user-images.githubusercontent.com/99355274/173897827-88432483-a9e0-48ed-a655-33c654543b39.png)

**PC-B ping 10.30.0.10 | 10.20.0.1**
![image](https://user-images.githubusercontent.com/99355274/173898217-90e436fa-b39a-458b-af07-9a7de2ecf046.png)

**PC-B ping 172.16.1.1**
![image](https://user-images.githubusercontent.com/99355274/173898278-72ffe15e-0de3-423b-a646-0ca63f95cb84.png)

**PC-B https 10.20.0.1**
![image](https://user-images.githubusercontent.com/99355274/173898429-a7b4ae1a-3ec7-4c2e-9e54-8a185a6daa7c.png)

**PC-B https 172.16.1.1**
![image](https://user-images.githubusercontent.com/99355274/173898526-883ea5fe-575d-4bea-ab8c-c2a3dd60a1d7.png)

**PC-B ssh 10.20.0.4*
![image](https://user-images.githubusercontent.com/99355274/173899160-fffb889d-a62e-4865-bbfd-79844d5a4cd4.png)

**PC-B ssh 172.16.1.1**
![image](https://user-images.githubusercontent.com/99355274/173899245-abd60d52-2b5a-43b4-b2ab-cec7cedbc562.png)







