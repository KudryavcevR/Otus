## Часть 1. Создание сети и настройка основных параметров устройства
### Шаг 1. Создайте сеть согласно топологии.
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
### Шаг 2. Произведите базовую настройку маршрутизаторов.
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
## Часть 2. Настройка сетей VLAN на коммутаторах.
### Шаг 1. Создайте сети VLAN на коммутаторах.
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
Vlan20                 10.20.0.2       YES manual administratively down down
S1(config)#int g0/3
S1(config-if)#sw a vlan 999
S1(config-if)#int range g1/0-3
S1(config-if-range)#sw a vlan 999
S1(config-if-range)#
```
### Шаг 2. Назначьте сети VLAN соответствующим интерфейсам коммутатора.
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
## Часть 4. Настройте маршрутизацию.
### Шаг 1. Настройка маршрутизации между сетями VLAN на R1.
