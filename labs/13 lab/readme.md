
## Часть 1: Создание сети и настройка основных параметров устройства.
### 1.2 Создайте сеть согласно топологии.
### 1.3 Настройте базовые параметры для маршрутизатора.
### 1.4 Настройте базовые параметры каждого коммутатора.

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
### 4.1 Выведите на экран текущее время.

    Дата     |     Время    | Часовой пояс  | Источник времени
 ----------- | ------------ | ------------- | ---------------- 
 Jun 20 2022 | 15:16:18.268 |      UTC      | Content Cell 

### 4.2 Установите время.

```sh 
 R1#clock set 15:36:00 20 Jun 2022
 ```
 
### 4.3 Настройте главный сервер NTP.

