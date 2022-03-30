![image](https://user-images.githubusercontent.com/99355274/160562707-a0ec27cf-60c4-42dd-a5e2-b52ec7f84c9c.png)

## Часть 1. Создание сети и настройка основных параметров устройства

 ### Шаг 1. Создайте сеть согласно топологии.

![image](https://user-images.githubusercontent.com/99355274/160563005-20901514-e8e1-4605-9674-d811af7ae1cd.png)

 ### Шаг 2. Выполните инициализацию и перезагрузку коммутаторов.
 ```sh
        Switch#reload
        System configuration has been modified. Save? [yes/no]:y
        Building configuration...
        [OK]
        Proceed with reload? [confirm]
        C2960 Boot Loader (C2960-HBOOT-M) Version 12.2(25r)FX, RELEASE SOFTWARE (fc4)
        Cisco WS-C2960-24TT (RC32300) processor (revision C0) with 21039K bytes of memory.
        2960-24TT starting...
        Base ethernet MAC Address: 00E0.A31E.B4DD
        Xmodem file system is available.
        Initializing Flash...
        flashfs[0]: 2 files, 0 directories
        flashfs[0]: 0 orphaned files, 0 orphaned directories
        flashfs[0]: Total bytes: 64016384
        flashfs[0]: Bytes used: 4671555
        flashfs[0]: Bytes available: 59344829
        flashfs[0]: flashfs fsck took 1 seconds.
        ...done Initializing Flash.

        Boot Sector Filesystem (bs:) installed, fsid: 3
        Parameter Block Filesystem (pb:) installed, fsid: 4


        Loading "flash:/2960-lanbasek9-mz.150-2.SE4.bin"...
        ########################################################################## [OK]
        Smart Init is enabled
        smart init is sizing iomem
                          TYPE      MEMORY_REQ
                        TOTAL:      0x00000000
        Rounded IOMEM up to: 0Mb.
        Using 6 percent iomem. [0Mb/512Mb]

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
        Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE4, RELEASE SOFTWARE (fc1)
        Technical Support: http://www.cisco.com/techsupport
        Copyright (c) 1986-2013 by Cisco Systems, Inc.
        Compiled Wed 26-Jun-13 02:49 by mnguyen
        Initializing flashfs...
        fsck: Disable shadow buffering due to heap fragmentation.
        flashfs[2]: 2 files, 1 directories
        flashfs[2]: 0 orphaned files, 0 orphaned directories
        flashfs[2]: Total bytes: 32514048
        flashfs[2]: Bytes used: 11952128
        flashfs[2]: Bytes available: 20561920
        flashfs[2]: flashfs fsck took 2 seconds.
        flashfs[2]: Initialization complete....done Initializing flashfs.
        Checking for Bootloader upgrade..
        Boot Loader upgrade not required (Stage 2)
        POST: CPU MIC register Tests : Begin
        POST: CPU MIC register Tests : End, Status Passed
        POST: PortASIC Memory Tests : Begin
        POST: PortASIC Memory Tests : End, Status Passed
        POST: CPU MIC interface Loopback Tests : Begin
        POST: CPU MIC interface Loopback Tests : End, Status Passed
        POST: PortASIC RingLoopback Tests : Begin
        POST: PortASIC RingLoopback Tests : End, Status Passed
        POST: PortASIC CAM Subsystem Tests : Begin
        POST: PortASIC CAM Subsystem Tests : End, Status Passed
        POST: PortASIC Port Loopback Tests : Begin
        POST: PortASIC Port Loopback Tests : End, Status Passed
        Waiting for Port download...Complete

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
        cisco WS-C2960-24TT-L (PowerPC405) processor (revision B0) with 65536K bytes of memory.
        Processor board ID FOC1010X104
        Last reset from power-on
        1 Virtual Ethernet interface
        24 FastEthernet interfaces
        2 Gigabit Ethernet interfaces
        The password-recovery mechanism is enabled.
        64K bytes of flash-simulated non-volatile configuration memory.
        Base ethernet MAC Address       : 00:E0:A3:1E:B4:DD
        Motherboard assembly number     : 73-10390-03
        Power supply part number        : 341-0097-02
        Motherboard serial number       : FOC10093R12
        Power supply serial number      : AZS1007032H
        Model revision number           : B0
        Motherboard revision number     : B0
        Model number                    : WS-C2960-24TT-L
        System serial number            : FOC1010X104
        Top Assembly Part Number        : 800-27221-02
        Top Assembly Revision Number    : A0
        Version ID                      : V02
        CLEI Code Number                : COM3L00BRA
        Hardware Board Revision Number  : 0x01

        Switch Ports Model              SW Version            SW Image
        ------ ----- -----              ----------            ----------
        *    1 26    WS-C2960-24TT-L    15.0(2)SE4            C2960-LANBASEK9-M

        Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE4, RELEASE SOFTWARE (fc1)
        Technical Support: http://www.cisco.com/techsupport
        Copyright (c) 1986-2013 by Cisco Systems, Inc.
        Compiled Wed 26-Jun-13 02:49 by mnguyen
        Press RETURN to get started!
```
 ### Шаг 3. Настройте базовые параметры каждого коммутатора.
 
**Коммутатор S3**
```sh
        Switch(config)#no ip domain lookup
        Switch(config)#hostname S3
        S3(config)#enable password class
        S3(config)#service pass
        S3(config)#service password-encryption 
        S3(config)#line vty 0 5
        S3(config-line)#password cisco
        S3(config-line)#login
        S3(config-line)#line con 0
        S3(config-line)#logging synch
        S3(config-line)#logging synchronous 
        S3(config-line)#exit
        S3(config)#banner motd ## Attention!! ##
        S3(config)#int vlan 1
        S3(config-if)#ip address 192.168.1.3 255.255.255.0
        S3(config-if)#no sh
        S3(config-if)#
        %LINK-5-CHANGED: Interface Vlan1, changed state to up
        %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
        S3(config-if)#ex
        S3#copy running-config startup-config 
        Destination filename [startup-config]? 
        Building configuration...
        [OK]
```

  **Коммутатор S2**
  
```sh
        Switch>en
        Switch#conf t
        Enter configuration commands, one per line.  End with CNTL/Z.
        Switch(config)#no ip domain lookup
        Switch(config)#hostname S2
        S2(config)#enable password class
        S2(config)#service password-encryption
        S2(config)#line vty 0 5
        S2(config-line)#password cisco
        S2(config-line)#login
        S2(config-line)#line con 0
        S2(config-line)#logging synchronous
        S2(config-line)#exit
        S2(config)#banner motd ## Attention!! ##
        S2(config)#int vlan 1
        S2(config-if)#ip address 192.168.1.2 255.255.255.0
        S2(config-if)#no sh
        S2(config-if)#
        %LINK-5-CHANGED: Interface Vlan1, changed state to up
        %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
        S2(config-if)#ex
        S2(config)#ex
        S2#
        %SYS-5-CONFIG_I: Configured from console by console
        S2#
        S2#copy running-config startup-config 
        Destination filename [startup-config]? 
        Building configuration...
        [OK]
        S2#
```

**Коммутатор S1**
```sh
        Switch>en
        Switch#conf t
        Enter configuration commands, one per line.  End with CNTL/Z.
        Switch(config)#hostname S1
        S1(config)#enable password class
        S1(config)#service password-encryption
        S1(config)#line vty 0 5
        S1(config-line)#password cisco
        S1(config-line)#login
        S1(config-line)#line con 0
        S1(config-line)#logging synchronous
        S1(config-line)#exit
        S1(config)#banner motd ## Attention!! ##
        S1(config)#int vlan 1
        S1(config-if)#ip address 192.168.1.1 255.255.255.0
        S1(config-if)#no sh
        S1(config-if)#
        %LINK-5-CHANGED: Interface Vlan1, changed state to up
        %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
        S1(config-if)#ex
        S1(config)#ex
        S1#
        %SYS-5-CONFIG_I: Configured from console by console
        S1#copy running-config startup-config 
        Destination filename [startup-config]? 
        Building configuration...
        [OK]
        S1#
```

### Шаг 4. Проверьте связь.
 
**Проверка с S1 на S2** 
```sh
S1#ping 192.168.1.2

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms
```

**Проверка с S1 на S3**
```sh
S1#ping 192.168.1.3

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.3, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/2 ms
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
```sh
  S1(config)#int range f0/1-24,g0/1-2
  S1(config-if-range)#sh
```

  ### Шаг 2. Настройте подключенные порты в качестве транковых.
```sh
  S1(config)#int range f0/1-4
  S1(config-if-range)#switchport mode trunk  
```

  ### Шаг 3. Включите порты f0/2,f0/4 на всех коммутаторах
```sh
  S1(config)#int f0/2
  S1(config-if)#no sh
  S1(config-if)#
  %LINK-5-CHANGED: Interface FastEthernet0/2, changed state to up
  %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/2, changed state to up
  %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
  S1(config-if)#int f0/4
  S1(config-if)#no sh
  S1(config-if)#
  %LINK-5-CHANGED: Interface FastEthernet0/4, changed state to up
  %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/4, changed state to up
  S1(config-if)#  
```

### Шаг 4. Отобразите данные протокола spanning-treeю

**Коммутатор S1**
```sh
  S1#show spanning-tree
  VLAN0001
    Spanning tree enabled protocol ieee
    Root ID    Priority    32769
               Address     0001.64E5.23CA
               Cost        19
               Port        2(FastEthernet0/2)
               Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

    Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
               Address     0005.5E40.543E
               Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
               Aging Time  20

  Interface        Role Sts Cost      Prio.Nbr Type
  ---------------- ---- --- --------- -------- --------------------------------
  Fa0/2            Root FWD 19        128.2    P2p
  Fa0/4            Desg FWD 19        128.4    P2p
 ``` 
 
**Коммутатор S2**
```sh 
S2#show spanning-tree
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.64E5.23CA
             This bridge is the root
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0001.64E5.23CA
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Desg FWD 19        128.2    P2p
Fa0/4            Desg FWD 19        128.4    P2p
```

**Коммутатор S3**
```sh
S3#show spanning-tree
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.64E5.23CA
             Cost        19
             Port        4(FastEthernet0/4)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     00E0.A31E.B4DD
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/4            Root FWD 19        128.4    P2p
Fa0/2            Altn BLK 19        128.2    P2p 
```
 ### Шаг 1.Определите коммутатор с заблокированным портом.
 ```sh
S3#show spanning-tree
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.64E5.23CA
             Cost        19
             Port        4(FastEthernet0/4)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     00E0.A31E.B4DD
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/4            Root FWD 19        128.4    P2p
Fa0/2            Altn BLK 19        128.2    P2p 
```

 ### Измените стоимость порта.
 ```sh
 
