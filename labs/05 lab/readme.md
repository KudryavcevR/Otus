![image](https://user-images.githubusercontent.com/99355274/159246614-0013d85f-30a1-4e3b-8778-a9f28f0439dd.png)

# Часть 1. Настройка основных параметров устройств.

## Шаг 1. Создайте сеть согласно топологии.

![image](https://user-images.githubusercontent.com/99355274/159268196-e540a946-5651-40ed-bd59-164e07ae3a01.png)

## Шаг 2. Выполните инициализацию и перезагрузку маршрутизатора и коммутатора.

### Перезагрузка коммутатора
```sh
    S1#reload
    System configuration has been modified. Save? [yes/no]:yes
    Building configuration...
    [OK]
    Proceed with reload? [confirm]
    C2960 Boot Loader (C2960-HBOOT-M) Version 12.2(25r)FX, RELEASE SOFTWARE (fc4)
    Cisco WS-C2960-24TT (RC32300) processor (revision C0) with 21039K bytes of memory.
    2960-24TT starting...
    Base ethernet MAC Address: 00D0.BA83.E0D1
    Xmodem file system is available.
    Initializing Flash...
    flashfs[0]: 2 files, 0 directories
    flashfs[0]: 0 orphaned files, 0 orphaned directories
    flashfs[0]: Total bytes: 64016384
    flashfs[0]: Bytes used: 4671924
    flashfs[0]: Bytes available: 59344460
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
    Base ethernet MAC Address       : 00:D0:BA:83:E0:D1
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
### Перезагрузка маршрутизатора
```sh
    R1#reload
    System configuration has been modified. Save? [yes/no]:yes
    Building configuration...
    [OK]
    Proceed with reload? [confirm]
    Initializing Hardware ...

    Checking for PCIe device presence...done
    System integrity status: 0x610
    Rom image verified correctly


    System Bootstrap, Version 16.7(3r), RELEASE SOFTWARE
    Copyright (c) 1994-2018  by cisco Systems, Inc.

    Current image running: Boot ROM0

    Last reset cause: LocalSoft
    Cisco ISR4331/K9 platform with 4194304 Kbytes of main memory



    no valid BOOT image found
    Final autoboot attempt from default boot device...
    Located isr4300-universalk9.16.06.04.SPA.bin
    ##########################################################################################################################
    Package header rev 1 structure detected
    IsoSize = 550114467
    Calculating SHA-1 hash...Validate package: SHA-1 hash:
            calculated 444F4D02:44C58887:D9C8942B:C557D3CF:2A14247E
            expected   444F4D02:44C58887:D9C8942B:C557D3CF:2A14247E

    RSA Signed RELEASE Image Signature Verification Successful.
    Image validated


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

    Cisco IOS Software[Everest], ISR Software(X86_64_LINUX_IOSD - UNIVERSALK9 - M), Version 16.6.4, RELEASE SOFTWARE(fc3)
    Technical Support : http://www.cisco.com/techsupport
    Copyright(c) 1986 - 2018 by Cisco Systems, Inc.
    Compiled Sun 08 - Jul - 18 04:33 by mcpre



    Cisco IOS - XE software, Copyright(c) 2005 - 2018 by cisco Systems, Inc.
    All rights reserved.Certain components of Cisco IOS - XE software are
    licensed under the GNU General Public License("GPL") Version 2.0.The
    software code licensed under GPL Version 2.0 is free software that comes
    with ABSOLUTELY NO WARRANTY.You can redistribute and / or modify such
    GPL code under the terms of GPL Version 2.0.For more details, see the
    documentation or "License Notice" file accompanying the IOS - XE software,
    or the applicable URL provided on the flyer accompanying the IOS - XE
    software.



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

    cisco ISR4331/K9 (1RU) processor with 1795999K/6147K bytes of memory.
    Processor board ID FLM232010G0
    3 Gigabit Ethernet interfaces
    32768K bytes of non-volatile configuration memory.
    4194304K bytes of physical memory.
    3207167K bytes of flash memory at bootflash:.
    0K bytes of WebUI ODM Files at webui:.


    Press RETURN to get started!
```


## Шаг 3. Настройте маршрутизатор.
```sh
    Router>enable
    Router#conf t
    Router(config)#no ip domain-lookup
    Router(config)#enable secret class
    Router(config)#int g0/0/1
    Router(config-if)#Enter configuration commands, one per line.  End with CNTL/Z.
    Router(config)#line con 0
    Router(config-line)#password cisco
    Router(config-line)#login
    Router(config-line)#exit
    Router(config)#line vty 0 5
    Router(config-line)#password cisco
    Router(config-line)#login
    Router(config-line)#exit
    Router(config)#service password-encryption
    Router(config)#banner motd # Unauthorized access is strictly prohibited. #
    Router(config)#int g0/0/1
    Router(config-if)#ip address 192.168.1.1 255.255.255.0
    Router(config-if)#no sh

    Router(config-if)#
    %LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

    %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

    Router(config-if)#ex
    Router(config)#ex
    Router#
    %SYS-5-CONFIG_I: Configured from console by console

    Router#copy running-config startup-config
    Destination filename [startup-config]? 
    Building configuration...
    [OK]
```

## Шаг 4 Настройте компьютер PC-A.

![image](https://user-images.githubusercontent.com/99355274/159234811-384ecc47-f50d-4d56-9819-5c84638e0aa8.png)

## Шаг 5. Проверьте подключение к сети.

![image](https://user-images.githubusercontent.com/99355274/159234623-beb44043-e73c-4aa9-ac39-83f2210299da.png)

# Часть 2. Настройка маршрутизатора для доступа по SSH.

## Шаг 1. Настройте аутентификацию устройств.
    ```sh
        Router(config)#hostname R1
        R1(config)#ip domain name cisco.com
    ```
 ## Шаг 2. Создайте ключ шифрования с указанием его длины.
    ```sh
        R1(config)#crypto key generate rsa
        The name for the keys will be: R1.R1
        Choose the size of the key modulus in the range of 360 to 2048 for your
          General Purpose Keys. Choosing a key modulus greater than 512 may take
          a few minutes.
        How many bits in the modulus [512]: 1024
        % Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
    ```     
  
## Шаг 3. Создайте имя пользователя в локальной базе учетных записей.

    ```sh
        R1(config)#username admin password Adm1nP@55
    ```    
## Шаг 4. Активируйте протокол SSH на линиях VTY.        
    ```sh    
        R1(config)#line vty 0 5
        R1(config-line)#transport input ssh
        R1(config-line)#login local
    ```    
## Шаг 5.Сохраните текущую конфигурацию в файл загрузочной конфигурации.
    ```sh
        R1#copy running-config startup-config
        Destination filename [startup-config]? 
        Building configuration...
        [OK]
    ```
## Шаг 6.Установите соединение с маршрутизатором по SSH.

![image](https://user-images.githubusercontent.com/99355274/159254118-e0cf99fb-09fd-424d-bc85-2085d9287235.png)
     
# Часть 3. Настройка коммутатора для доступа по протоколу SSH.

## Шаг 1. Настройте основные параметры коммутатора.
    ```sh
        Switch>en
        Switch#conf t
        Enter configuration commands, one per line.  End with CNTL/Z.
        Switch(config)#no ip domain-name
        Switch(config)#enable secret password class
        Switch(config)#line con 0
        Switch(config-line)#password cisco
        Switch(config-line)#login
        Switch(config-line)#line vty 0 5
        Switch(config-line)#password cisco
        Switch(config-line)#login
        Switch(config)#service password-encryption
        Switch(config)#banner motd # Unauthorized access is strictly prohibited. #
        Switch(config)#int vlan 1
        Switch(config-if)#ip address 192.168.1.11 255.255.255.0
        Switch(config-if)#no sh
        Switch(config-if)#
        %LINK-5-CHANGED: Interface Vlan1, changed state to up
        %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
        Switch(config-if)#ex
        Switch(config)#ip default-gateway 192.168.1.1
        Switch(config)#end
        Switch#
        %SYS-5-CONFIG_I: Configured from console by console
        Switch#copy running-config startup-config
        Destination filename [startup-config]? 
        Building configuration...
        [OK]

## Шаг 2. Настройте коммутатор для соединения по протоколу SSH.
    ```sh
        Switch(config)#hostname S1
        S1(config)#ip domain-name cisco.com
        S1(config)#crypto key generate rsa
        The name for the keys will be: S1.cisco.com
        Choose the size of the key modulus in the range of 360 to 2048 for your
          General Purpose Keys. Choosing a key modulus greater than 512 may take
          a few minutes.
        How many bits in the modulus [512]: 1024
        % Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
        S1(config)#username admin password Adm1nP@55
        *Mar 1 4:12:35.600: %SSH-5-ENABLED: SSH 1.99 has been enabled
        S1(config)#line vty 0 5
        S1(config-line)#transport input ssh
        S1(config-line)#login local
     ```
## Шаг 3. Установите соединение с коммутатором по протоколу SSH.

![image](https://user-images.githubusercontent.com/99355274/159266292-3fe30813-226c-43d1-92d8-e71a815564e1.png)

# Часть 4. Настройка протокола SSH с использованием интерфейса командной строки (CLI) коммутатора.

## Шаг 1. Посмотрите доступные параметры для клиента SSH в Cisco IOS.
    ```sh
        S1#ssh ?
          -l  Log in using this user name
          -v  Specify SSH Protocol Version
    ```
## Шаг 2. Установите с коммутатора S1 соединение с маршрутизатором R1 по протоколу SSH.
    ```sh
        S1#ssh -l admin 192.168.1.1
        Password: 
        Unauthorized access is strictly prohibited.
        R1>
        R1>exit
        [Connection to 192.168.1.1 closed by foreign host]
    ```
