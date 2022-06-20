
## Часть 1: Создание сети и настройка основных параметров устройства.

### Шаг 1. Создайте сеть согласно топологии.
![image](https://user-images.githubusercontent.com/99355274/174661332-34a40526-eb3e-4e2f-8fd8-00880ccfbd02.png)

### Шаг 2. Настройте базовые параметры для маршрутизатора.
```sh
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
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
R1(config)#banner motd # Attention! For Staff only! #
R1(config)#int g0/2
R1(config-if)#ip address 10.22.0.1 255.255.255.0
R1(config-if)#do wr
Building configuration...
[OK]
```
### Шаг 3. Настройте базовые параметры каждого коммутатора.
**Коммутатор S1**
```sh
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname S1
S1(config)#no ip domain-lookup
S1(config)#enable password class
S1(config)#line con 0
S1(config-line)#password cisco
S1(config-line)#line vty 0 4
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#service password-encryption
S1(config)#banner motd # Attention!For staff only! #
S1(config)#int vlan 1
S1(config-if)#ip address 10.22.0.2 255.255.255.0
S1(config-if)#exit
S1(config)#ip default-gateway 10.22.0.1
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
Vlan1                  10.22.0.2       YES NVRAM  up                    up
S1(config)#int range g0/2-3
S1(config-if-range)#sh
S1(config-if-range)#int range g1/0-3
S1(config-if-range)#sh
S1(config)#do wr
```
**Коммутатор S2**
```sh
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname S2
S2(config)#no ip domain-lookup
S2(config)#enable password class
S2(config)#line con 0
S2(config-line)#password cisco
S2(config-line)#line vty 0 4
S2(config-line)#password cisco
S2(config-line)#login
S2(config-line)#exit
S2(config)#service password-encryption
S2(config)#banner motd # Attention!For staff only! #
S2(config)#int vlan 1
S2(config-if)#ip address 10.22.0.3 255.255.255.0
S2(config-if)#exit
S2(config)#ip default-gateway 10.22.0.1
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
Vlan1                  10.22.0.3       YES NVRAM  up                    up
S2(config)#int range g0/2-3
S2(config-if-range)#sh
S2(config-if-range)#int range g1/0-3
S2(config-if-range)#sh
S2(config)#do wr
```

## Часть 2: Обнаружение сетевых ресурсов с помощью протокола CDP.
```sh
R1(config)#do sh cdp
Global CDP information:
        Sending CDP packets every 60 seconds
        Sending a holdtime value of 180 seconds
        Sending CDPv2 advertisements is  enabled
R1(config)#
```
```sh
R1#sh cdp traffic
CDP counters :
        Total packets output: 20, Input: 12
        Hdr syntax: 0, Chksum error: 0, Encaps failed: 0
        No memory: 0, Invalid packet: 0,
        CDP version 1 advertisements output: 0, Input: 0
        CDP version 2 advertisements output: 20, Input: 12
```
```sh
R1(config)#do sh cdp entry S1
-------------------------
Device ID: S1
Entry address(es):
  IP address: 10.22.0.2
Platform: Cisco ,  Capabilities: Router Switch IGMP
Interface: GigabitEthernet0/1,  Port ID (outgoing port): GigabitEthernet0/2
Holdtime : 157 sec

Version :
Cisco IOS Software, vios_l2 Software (vios_l2-ADVENTERPRISEK9-M), Version 15.2(4.0.55)E, TEST ENGINEERING ESTG_WEEKLY BUILD, synced to  END_OF_FLO_ISP
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2015 by Cisco Systems, Inc.
Compiled Tue 28-Jul-15 18:52 by sasyamal

advertisement version: 2
VTP Management Domain: ''
Native VLAN: 1
Duplex: half
Management address(es):
  IP address: 10.22.0.2
```
```sh
R1(config)#no cdp run
```

## Часть 3: Обнаружение сетевых ресурсов с помощью протокола LLDP.
```sh
S1(config)#lldp run
S1(config)#do sh lldp entry S2

Capability codes:
    (R) Router, (B) Bridge, (T) Telephone, (C) DOCSIS Cable Device
    (W) WLAN Access Point, (P) Repeater, (S) Station, (O) Other

Total entries displayed: 0
```
## Часть 4: Настройка NTP.
### Шаг 1. Выведите на экран текущее время.


   Дата     |     Время    | Часовой пояс  | Источник времени 
------------|--------------|---------------|----------------- 
Jun 20 2022 | 15:16:18.268 |      UTC      |      NTP         


### Шаг 2. Установите время.

```sh 
 R1#clock set 20:00:00 20 Jun 2022
 ```
 
### Шаг 3. Настройте главный сервер NTP.

```sh
R1(config)#ntp server 10.22.0.1
R1(config)#ntp master 4
```

### Шаг 4. Настройте клиент NTP.

   Дата     |     Время    | Часовой пояс 
------------|--------------|-------------
Jun 20 2022 | 15:20:06.268 |      UTC      
 
 
 **Коммутатор S1**
 
 ```sh
 S1(config)#ntp server 10.22.0.1 source g0/0
```
**Коммутатор S2**

```sh
S2(config)#ntp server 10.22.0.1 source g0/1
```

### Шаг 5. Проверьте настройку NTP.

 **Коммутатор S1**
```sh
S1#sh ntp stat
Clock is synchronized, stratum 5, reference is 10.22.0.1
nominal freq is 1000.0003 Hz, actual freq is 999.9296 Hz, precision is 2**16
ntp uptime is 402700 (1/100 of seconds), resolution is 1001
reference time is E65B528E.F3D040BC (20:09:50.952 UTC Mon Jun 20 2022)
clock offset is -10.5279 msec, root delay is 3.37 msec
root dispersion is 7948.91 msec, peer dispersion is 5.49 msec
loopfilter state is 'CTRL' (Normal Controlled Loop), drift is 0.000070667 s/s
system poll interval is 64, last update was 638 sec ago.

S1#sh clock
20:22:43.566 UTC Mon Jun 20 2022
```

 **Коммутатор S2**
 ```sh
 S2#sh ntp stat
Clock is synchronized, stratum 5, reference is 10.22.0.1
nominal freq is 1000.0003 Hz, actual freq is 999.9690 Hz, precision is 2**16
ntp uptime is 408900 (1/100 of seconds), resolution is 1001
reference time is E65B544A.30DDB3E3 (20:17:14.190 UTC Mon Jun 20 2022)
clock offset is 3.5453 msec, root delay is 4.10 msec
root dispersion is 25.23 msec, peer dispersion is 3.06 msec
loopfilter state is 'CTRL' (Normal Controlled Loop), drift is 0.000031234 s/s
system poll interval is 64, last update was 281 sec ago.

S2#sh clock
20:23:00.415 UTC Mon Jun 20 2022
```
 
