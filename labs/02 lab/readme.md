## Задачи
![image](https://user-images.githubusercontent.com/99355274/155677637-f6be6955-8c71-4c5f-b281-aa6140312e32.png)


## Часть 1.

  1.1 Подключаем сеть согласно топологии

  ![image](https://user-images.githubusercontent.com/99355274/155678618-74a161dc-f630-47eb-a7d4-21fe5c744e6f.png)


  1.2 Настраиваем Ip адреса узлов.
 ```sh
        PC-A 192.168.1.1
        PC-B 192.168.1.2
 ```
  
  1.3 Выполняем инициализацию и перезагрузку коммутаторов
 
 ```sh
        Switch>en
        Switch#reload
        Proceed with reload? [confirm]
        C2960 Boot Loader (C2960-HBOOT-M) Version 12.2(25r)FX, RELEASE SOFTWARE (fc4)
        Cisco WS-C2960-24TT (RC32300) processor (revision C0) with 21039K bytes of memory.
        2960-24TT starting...
        Base ethernet MAC Address: 0003.E4C5.1207
        Xmodem file system is available.
        Initializing Flash...
        flashfs[0]: 1 files, 0 directories
        flashfs[0]: 0 orphaned files, 0 orphaned directories
        flashfs[0]: Total bytes: 64016384
        flashfs[0]: Bytes used: 4670455
        flashfs[0]: Bytes available: 59345929
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
        Base ethernet MAC Address       : 00:03:E4:C5:12:07
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


        %LINK-5-CHANGED: Interface FastEthernet0/1, changed state to up

        %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to up

        %LINK-5-CHANGED: Interface FastEthernet0/6, changed state to up

        %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/6, changed state to up


        Switch>
```

  1.4 Настраиваем базовые параметры каждого коммутатора.
  
Настройка имен устройств
 
- Для S1
```sh
        Switch(config)#> hostname S1
        S1(config)#
```
 - Для S2
```sh
        Switch(config)#> hostname S2
        S2(config)#
```

Настройка IP адресов

 - Для S1
```sh
        S1>en
        S1#conf
        Configuring from terminal, memory, or network [terminal]? 
        Enter configuration commands, one per line.  End with CNTL/Z.
        S1(config)#int vlan 1
        S1(config-if)#ip address 192.168.1.11 255.255.255.0
        S1(config-if)#no sh
        %LINK-5-CHANGED: Interface Vlan1, changed state to up
        %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
```
  
 - Для S2
```sh
        S2>en
        S2#conf
        Configuring from terminal, memory, or network [terminal]? 
        Enter configuration commands, one per line.  End with CNTL/Z.
        S2(config)#int vlan 1
        S2(config-if)#ip address 192.168.1.12 255.255.255.0
        S2(config-if)#no sh
        %LINK-5-CHANGED: Interface Vlan1, changed state to up
        %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
 ```
  
Начначаем пароль для VTY
      
- Для S1
```sh
        S1(config)#line vty 0 15
        S1(config-line)#password cisco
        S1(config-line)#login
```
  
- Для S2
```sh
        S2(config)#line vty 0 15
        S2(config-line)#password cisco
        S2(config-line)#login
```
  
Начначаем пароль для режима EXEC

- Для S1 
```sh
        S1>en
        S1#conf
        Configuring from terminal, memory, or network [terminal]? 
        Enter configuration commands, one per line.  End with CNTL/Z.
        S1(config)#enable secret class
```
  
- Для S2
```sh
        S1>en
        S1#conf
        Configuring from terminal, memory, or network [terminal]? 
        Enter configuration commands, one per line.  End with CNTL/Z.
        S1(config)#enable secret class
```
  
  ## Часть 2.
   
 1. Запишите MAC-адреса сетевых уст-в
```sh
     PC-A                                                    PC-B
     Physical Address................: 0002.4A44.9B51        Physical Address................: 0001.42A1.7C13
     
     S1                                                      S2
     Hardware is Lance, address is 0030.f2d6.6101            Hardware is Lance, address is 0060.70bb.4901  
```
     
  2. Посмотрите таблицу MAC-адресов коммутатора
```sh
      S2#show mac address-table
          Vlan    Mac Address       Type       Ports
         ----   -----------      --------      ----
           1    0001.42a1.7c13    DYNAMIC     Fa0/18
           1    0002.4a44.9b51    DYNAMIC     Fa0/1
           1    0030.f2d6.6101    DYNAMIC     Fa0/1
```
  3. Очистите таблицу МАС-адресов коммутатора S2 и отобразите ее снова
```sh
     S2#clear mac address-table dynamic
     S2#show mac address-table
              Mac Address Table
      -------------------------------------------

     Vlan    Mac Address       Type        Ports
     ----    -----------       --------    -----
     
       1    0030.f2d6.6101    DYNAMIC     Fa0/1
```
  4. С компьютера PC-B отправьте эхо-запросы устройствам в сети и просмотрите таблицу МАС-адресов коммутатора.

- Эхо запрос с PC-B узлам сети  
     ![image](https://user-images.githubusercontent.com/99355274/156026650-533f1fe8-6f5c-481f-a183-87ec1862c5a5.png)
     
- ARP таблица с PC-B    
     ![image](https://user-images.githubusercontent.com/99355274/156027651-ff8e084a-90a9-431b-9020-c781941c8f13.png)

- Таблица МАС Switch2
```sh
     S2#show mac-address-table
            Mac Address Table
      -----------------------------------------
    Vlan    Mac Address       Type        Ports
    ----   -----------      --------      ----
      1    0001.42a1.7c13    DYNAMIC     Fa0/18
      1    0002.4a44.9b51    DYNAMIC     Fa0/1
      1    0030.f2d6.6101    DYNAMIC     Fa0/1
```
 
       
    
