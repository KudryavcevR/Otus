![image](https://user-images.githubusercontent.com/99355274/160562707-a0ec27cf-60c4-42dd-a5e2-b52ec7f84c9c.png)

## Часть 1. Создание сети и настройка основных параметров устройства

 ### Шаг 1. Создайте сеть согласно топологии.

![image](https://user-images.githubusercontent.com/99355274/168442937-a2d05487-a083-4786-979b-c9bad359b3bb.png)

 ### Шаг 2. Выполните инициализацию и перезагрузку коммутаторов.
 ```sh
        Switch#reload
        System configuration has been modified. Save? [yes/no]:y
        Building configuration...
        [OK]
        Proceed with reload? [confirm]
          Booting `IOSv' Booting `IOSv

              Restricted Rights Legend

Use, duplication, or disclosure by the Government is
subject to restrictions as set forth in subparagraph
(c) of the Commercial Computer Software - Restricted
Rights clause at FAR sec. 52.227-19 and subparagraph
(c) (1) (ii) of the Rights in Technical Data and Computer
Software clause at DFARS sec. 252.227-7013.

           cisco Systems, Inc.
           170 West Tasman Drive
           San Jose, California 95134-1706



Cisco IOS Software, vios_l2 Software (vios_l2-ADVENTERPRISEK9-M), Version 15.2(E
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2018 by Cisco Systems, Inc.
Compiled Thu 10-May-18 05:13 by mmen


This product contains cryptographic features and is subject to United
States and local country laws governing import, export, transfer and
use. Delivery of Cisco cryptographic products does not imply
third-party authority to import, export, distribute or use encryption.
Importers, exporters, distributors and users are responsible for
compliance with U.S. and local country laws. By using this product you
agree to comply with applicable laws and regulations. If you are unable
to comply with U.S. and local laws, return this product immediately.

A summary of U.S. laws governing Cisco cryptographic products may be found at:
http://www.cisco.com/wwl/export/crypto/tool/stqrg.html

If you require further assistance please contact us by sending email to
export@cisco.com.

Cisco IOSv () processor (revision 1.0) with 935161K/111616K bytes of memory.
Processor board ID 9B986K8JGOC
8 Gigabit Ethernet interfaces
DRAM configuration is 72 bits wide with parity disabled.
256K bytes of non-volatile configuration memory.
2097152K bytes of ATA System CompactFlash 0 (Read/Write)
0K bytes of ATA CompactFlash 1 (Read/Write)
0K bytes of ATA CompactFlash 2 (Read/Write)
0K bytes of ATA CompactFlash 3 (Read/Write)



Press RETURN to get started!


*Mar  1 00:00:00.957: %ATA-6-DEV_FOUND: device 0x1F0
*Mar  1 00:00:01.053: %NVRAM-5-CONFIG_NVRAM_READ_OK: NVRAM configuration 'flash.
*May 14 16:23:13.578: %SPANTREE-5-EXTENDED_SYSID: Extended SysId enabled for tyn
*May 14 16:23:16.385: %LINK-5-CHANGED: Interface GigabitEthernet0/0, changed stt
*May 14 16:23:16.385: %LINK-5-CHANGED: Interface GigabitEthernet0/1, changed stt
*May 14 16:23:16.386: %LINK-5-CHANGED: Interface GigabitEthernet0/2, changed stt
*May 14 16:23:16.386: %LINK-5-CHANGED: Interface GigabitEthernet0/3, changed stt
*May 14 16:23:16.386: %LINK-5-CHANGED: Interface GigabitEthernet1/0, changed stt
*May 14 16:23:16.387: %LINK-5-CHANGED: Interface GigabitEthernet1/1, changed stt
*May 14 16:23:16.387: %LINK-5-CHANGED: Interface GigabitEthernet1/2, changed stt
*May 14 16:23:17.537: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEtp
*May 14 16:23:17.540: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEtp
*May 14 16:23:17.541: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEtp
*May 14 16:23:17.543: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEtp
*May 14 16:23:17.544: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEtp
*May 14 16:23:17.545: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEtp
*May 14 16:23:17.547: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEtp
*May 14 16:23:17.548: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEtp
*May 14 16:23:18.423: %LINK-3-UPDOWN: Interface GigabitEthernet1/3, changed stap
*May 14 16:23:18.439: %LINK-3-UPDOWN: Interface GigabitEthernet1/2, changed stap
*May 14 16:23:18.444: %LINK-3-UPDOWN: Interface GigabitEthernet1/1, changed stap
*May 14 16:23:18.449: %LINK-3-UPDOWN: Interface GigabitEthernet1/0, changed stap
*May 14 16:23:18.454: %LINK-3-UPDOWN: Interface GigabitEthernet0/3, changed stap
*May 14 16:23:18.458: %LINK-3-UPDOWN: Interface GigabitEthernet0/2, changed stap
*May 14 16:23:18.462: %LINK-3-UPDOWN: Interface GigabitEthernet0/1, changed stap
*May 14 16:23:18.466: %LINK-3-UPDOWN: Interface GigabitEthernet0/0, changed stap
*May 14 16:23:22.224: %SYS-5-RESTART: System restarted --
Cisco IOS Software, vios_l2 Software (vios_l2-ADVENTERPRISEK9-M), Version 15.2(E
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2018 by Cisco Systems, Inc.
Compiled Thu 10-May-18 05:13 by mmen
*May 14 16:23:55.579: %PLATFORM-5-SIGNATURE_VERIFIED: Image 'flash0:/vios_l2-adn
IOSv - Cisco Systems Confidential -

Supplemental End User License Restrictions

This IOSv software is provided AS-IS without warranty of any kind. Under no cir.

By using the software, you agree to abide by the terms and conditions of the Ci.
```

  **Коммутатор S1**
  
```sh
Switch(config)#hostname S1
S1(config)#no ip domain lookup
S1(config)#enable password class
S1(config)#service password-encryption
S1(config)#line vty 0 5
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#line con 0
S1(config-line)#logging syn
S1(config-line)#logging synchronous
S1(config-line)#exit
S1(config)#banner mord ##Attention!!##
S1(config)#int vlan 1
S1(config-if)#ip add
S1(config-if)#ip address
*May 14 16:32:26.508: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to down
S1(config-if)#ip address 192.168.1.1 255.255.255.0
S1(config-if)#no sh
S1(config-if)#
*May 14 16:32:45.705: %LINK-3-UPDOWN: Interface Vlan1, changed state to up
*May 14 16:32:46.705: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
S1(config-if)#ex
S1(config)#
S1(config)#
*May 14 16:33:08.485: %PNP-6-PNP_DISCOVERY_STOPPED: PnP Discovery stopped (Config Wizard)
S1(config)#exit
S1#
*May 14 16:33:12.335: %SYS-5-CONFIG_I: Configured from console by console
S1#copy running-config startup-config
Destination filename [startup-config]?
Building configuration...
Compressed configuration from 2982 bytes to 1718 bytes[OK]
S1#
*May 14 16:33:30.098: %GRUB-5-CONFIG_WRITING: GRUB configuration is being updated on disk. Please wait...
*May 14 16:33:30.772: %GRUB-5-CONFIG_WRITTEN: GRUB configuration was written to disk successfully.
S1#

```

**Коммутатор S2**
```sh
Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#no ip domain
Switch(config)#no ip domain lo
Switch(config)#no ip domain lookup
Switch(config)#hostn
Switch(config)#hostname S2
S2(config)#enable paswor
*May 14 16:35:19.828: %PNP-6-PNP_DISCOVERY_STOPPED: PnP Discovery stopped (Config Wizard)
S2(config)#enable password class
S2(config)#service password-en
S2(config)#service password-encryption
S2(config)#line vty 0 5
S2(config-line)#password cisco
S2(config-line)#login
S2(config-line)#line con 0
S2(config-line)#logging syn
S2(config-line)#logging synchronous
S2(config-line)#exit
S2(config)#banner motd ## Attention! ##
S2(config)#int vlan 1
S2(config-if)#
*May 14 16:36:46.630: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to down
S2(config-if)#
S2(config-if)#
S2(config-if)#
S2(config-if)#ip add
S2(config-if)#ip address 192.168.1.2 255.255.255.0
S2(config-if)#no sh
S2(config-if)#
*May 14 16:37:16.666: %LINK-3-UPDOWN: Interface Vlan1, changed state to up
*May 14 16:37:17.666: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
S2(config-if)#^Z
S2#co
*May 14 16:37:17.904: %SYS-5-CONFIG_I: Configured from console by console
S2#copy runn
S2#copy running-config star
S2#copy running-config startup-config
Destination filename [startup-config]?
Building configuration...
Compressed configuration from 2960 bytes to 1711 bytes[OK]
S2#
S2#
*May 14 16:37:25.854: %GRUB-5-CONFIG_WRITING: GRUB configuration is being updated on disk. Please wait...
*May 14 16:37:26.526: %GRUB-5-CONFIG_WRITTEN: GRUB configuration was written to disk successfully.

```

**Коммутатор S3**
```sh
Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#no ip domain lookup
Switch(config)#hostname S3
*May 14 16:39:58.081: %PNP-6-PNP_DISCOVERY_STOPPED: PnP Discovery stopped (Config Wizard)
S3(config)#enable password class
S3(config)#service password-en
S3(config)#service password-encryption
S3(config-line)#line vty 0 5
S3(config-line)#password cisco
S3(config-line)#login
S3(config-line)#line con 0
S3(config-line)#logging synchronous
S3(config-line)#exit
S3(config)#banner motd ## Attention!! ##
S3(config)#int vlan 1
*May 14 16:41:49.594: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to down
S3(config-if)#ip address 192.168.1.3 255.255.255.0
S3(config-if)#no sh
S3(config-if)#
*May 14 16:42:06.570: %LINK-3-UPDOWN: Interface Vlan1, changed state to up
*May 14 16:42:07.570: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
S3(config-if)#^Z
S3#
*May 14 16:42:11.332: %SYS-5-CONFIG_I: Configured from console by console
S3#
S3#copy runn
S3#copy running-config star
S3#copy running-config startup-config
Destination filename [startup-config]?
Building configuration...
Compressed configuration from 2960 bytes to 1708 bytes[OK]
S3#
*May 14 16:42:23.693: %GRUB-5-CONFIG_WRITING: GRUB configuration is being updated on disk. Please wait...
*May 14 16:42:24.364: %GRUB-5-CONFIG_WRITTEN: GRUB configuration was written to disk successfully.
```

### Шаг 4. Проверьте связь.
 
**Проверка с S1 на S2** 
```sh
S1#ping 192.168.1.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.2, timeout is 2 seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max = 4/4/4 ms
```

**Проверка с S1 на S3**
```sh
S1#ping 192.168.1.3
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.3, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 2/2/3 ms
```

**Проверка с S2 на S3**
```sh
S2#ping 192.168.1.3

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.3, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms
```

## Часть 2. Определение корневого моста
  ### Шаг 1. Отключите все порты на коммутаторах.
  
**Коммутатор S1**

```sh
  S1(config)#int range g0/0-3
  S1(config-if-range)#sh
  S1(config)#int range g1/0-3
  S1(config-if-range)#sh
```
**Коммутатор S2**
```sh
  S2(config)#int range g0/0-3
  S2(config-if-range)#sh
  S2(config)#int range g1/0-3
  S2(config-if-range)#sh
```
**Коммутатор S3**
```sh
  S3(config)#int range g0/0-3
  S3(config-if-range)#sh
  S3(config)#int range g1/0-3
  S3(config-if-range)#sh
```

  ### Шаг 2. Настройте подключенные порты в качестве транковых.

**Коммутатор S1**
```sh
  S1(config)#int range g0/1-3
  S1(config-if-range)#sw m t
  S1(config-if-range)#int g1/1
  S1(config-if)#sw m t
  S1(config-if)#
```
**Коммутатор S2**
```sh
  S2(config)#int range g0/1-3
  S2(config-if-range)#sw m t
  S2(config-if-range)#int g1/1
  S2(config-if)#sw m t
  S2(config-if)#
```
**Коммутатор S3**
```sh
  S3(config)#int range g0/1-3
  S3(config-if-range)#sw m t
  S3(config-if-range)#int g1/1
  S3(config-if)#sw m t
  S3(config-if)#
```

  ### Шаг 3. Включите порты g0/2(f0/2),g1/1(f0/4) на всех коммутаторах

**Коммутатор S1**
```sh
  S1(config)#int g0/2
  S1(config-if)#no sh
  S1(config-if)#int g1/1
  S1(config-if)#no sh
```
**Коммутатор S2**
```sh
  S2(config)#int g0/2
  S2(config-if)#no sh
  S2(config-if)#int g1/1
  S2(config-if)#no sh
```
**Коммутатор S3**
```sh
  S3(config)#int g0/2
  S3(config-if)#no sh
  S3(config-if)#int g1/1
  S3(config-if)#no sh
```

### Шаг 4. Отобразите данные протокола spanning-tree

**Коммутатор S1**
```sh
S1#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     5000.0001.0000
             Cost        4
             Port        6 (GigabitEthernet1/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     5000.0002.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/2               Desg FWD 4         128.3    P2p
Gi1/1               Root FWD 4         128.6    P2p
 ``` 
 
**Коммутатор S2**
```sh 
S2#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     5000.0001.0000
             Cost        4
             Port        6 (GigabitEthernet1/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     5000.0003.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  15  sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/2               Altn BLK 4         128.3    P2p
Gi1/1               Root LRN 4         128.6    P2p
```

**Коммутатор S3**
```sh
S3#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     5000.0001.0000
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     5000.0001.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/2               Desg FWD 4         128.3    P2p
Gi1/1               Desg FWD 4         128.6    P2p

```


 ### Шаг 1.Определите коммутатор с заблокированным портом.
 
**Коммутатор S2**
 ```sh
S2#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     5000.0001.0000
             Cost        4
             Port        6 (GigabitEthernet1/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     5000.0003.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  15  sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/2               Altn BLK 4         128.3    P2p
Gi1/1               Root LRN 4         128.6    P2p

```

 ### Шаг 2. Измените стоимость порта.
 
 **Коммутатор S2**
 ```sh
S2(config)#int g1/1
S2(config-if)#spanning-tree cost 3
```
 ### Шаг 3. Посмотрите изменения протокола spanning-tree.
 
 **Коммутатор S2**
 ```sh
S2#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     5000.0001.0000
             Cost        3
             Port        6 (GigabitEthernet1/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     5000.0003.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/2               Desg FWD 4         128.3    P2p
Gi1/1               Root FWD 3         128.6    P2p
 ```  
**Коммутатор S1**
 ```sh 
S1#sh spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     5000.0001.0000
             Cost        4
             Port        6 (GigabitEthernet1/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     5000.0002.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  15  sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/2               Altn BLK 4         128.3    P2p
Gi1/1               Root FWD 4         128.6    P2p
```

 ### Шаг 4. Удалите изменения стоимости порта.
```sh 
S2(config)#int g1/1
S2(config-if)#no spanning-tree cost 3
```
 ### Повторно выведите spanning-tree.
 
**Коммутатор S2**
```sh
S2#sh spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     5000.0001.0000
             Cost        4
             Port        6 (GigabitEthernet1/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     5000.0003.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  15  sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/2               Altn BLK 4         128.3    P2p
Gi1/1               Root FWD 4         128.6    P2p
```

**Коммутатор S1**
```sh
S1#sh spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     5000.0001.0000
             Cost        4
             Port        6 (GigabitEthernet1/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     5000.0002.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/2               Desg FWD 4         128.3    P2p
Gi1/1               Root FWD 4         128.6    P2p
``` 
## Часть 4.	Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов.

### Включите g0/1(f0/1) и g0/3(f0/3) на всех коммутаторах

**Коммутатор S1**
```sh
S1(config)#int g0/1
S1(config-if)#no sh
S1(config-if)#int g0/3
S1(config-if)#no sh
```
**Коммутатор S2**
```sh
S2(config)#int g0/1
S2(config-if)#no sh
S2(config-if)#int g0/3
S2(config-if)#no sh
```
**Коммутатор S3**
```sh
S3(config)#int g0/1
S3(config-if)#no sh
S3(config-if)#int g0/3
S3(config-if)#no sh
```

 ### Выполните команду show spanning-tree
 
**Коммутатор S1**
```sh
S1#sh spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     5000.0001.0000
             Cost        4
             Port        4 (GigabitEthernet0/3)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     5000.0002.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/1               Desg FWD 4         128.2    P2p
Gi0/2               Desg FWD 4         128.3    P2p
Gi0/3               Root FWD 4         128.4    P2p
Gi1/1               Altn BLK 4         128.6    P2p
```

**Коммутатор S2**
```sh
S2#sh spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     5000.0001.0000
             Cost        4
             Port        4 (GigabitEthernet0/3)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     5000.0003.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/1               Altn BLK 4         128.2    P2p
Gi0/2               Altn BLK 4         128.3    P2p
Gi0/3               Root FWD 4         128.4    P2p
Gi1/1               Altn BLK 4         128.6    P2p
```
