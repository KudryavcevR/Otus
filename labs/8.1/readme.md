![image](https://user-images.githubusercontent.com/99355274/171928063-84a550c8-5dfb-4286-9636-ab610845c19b.png)
![image](https://user-images.githubusercontent.com/99355274/171911594-4668dec8-4cbc-4ba2-b6ef-38f4dad816e4.png)

## Часть 1:	Создание сети и настройка основных параметров устройства

### Шаг 1.Создание схемы адресации

![image](https://user-images.githubusercontent.com/99355274/171927993-0f9c208e-cf02-4ba4-a53a-fe4d87d78f91.png)

### Шаг 2.Создайте сеть согласно топологии

![image](https://user-images.githubusercontent.com/99355274/171930028-0fea06a6-f563-4a89-a38d-e8ead54eb35f.png)

### Шаг 3.Произведите базовую настройку маршрутизаторов

**Маршрутизатор R1**
```sh
Router(config)#hostname R1
R1(config)#no ip domain-lookup
R1(config)#enable password class
R1(config)#line con 0
R1(config-line)#password cisco
R1(config-line)#line vty 0 4
R1(config-line)#password cisco
R1(config)#service password-encryption
R1(config)#banner motd # Attention for staff only! #
R1#clock set 14:22:00 02 jun 2022
R1#
*Jun  2 14:22:00.000: %SYS-6-CLOCKUPDATE: System clock has been updated from 14:23:40 UTC Thu Jun 2 2022 to 14:22:00 UTC Thu Jun 2 2022, configured from console by console.
R1#clock ?
  read-calendar    Read the hardware calendar into the clock
  set              Set the time and date
  update-calendar  Update the hardware calendar from the clock

R1#clock upd
R1#clock update-calendar
R1#wr
Building configuration...
[OK]
R1#
Jun  2 14:22:43.013: %GRUB-5-CONFIG_WRITING: GRUB configuration is being updated on disk. Please wait...
Jun  2 14:22:43.799: %GRUB-5-CONFIG_WRITTEN: GRUB configuration was written to disk successfully.
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
R2(config)#service password-encryption
R2(config)#banner motd # Attention for staff only! #
R2#clock set 14:22:00 02 jun 2022
R2#
*Jun  2 14:22:00.000: %SYS-6-CLOCKUPDATE: System clock has been updated from 14:23:40 UTC Thu Jun 2 2022 to 14:22:00 UTC Thu Jun 2 2022, configured from console by console.
R2#clock ?
  read-calendar    Read the hardware calendar into the clock
  set              Set the time and date
  update-calendar  Update the hardware calendar from the clock

R2#clock upd
R2#clock update-calendar
R2#wr
Building configuration...
[OK]
R2#
Jun  2 14:22:43.013: %GRUB-5-CONFIG_WRITING: GRUB configuration is being updated on disk. Please wait...
Jun  2 14:22:43.799: %GRUB-5-CONFIG_WRITTEN: GRUB configuration was written to disk successfully.
```

### Шаг 4.Настройка маршрутизации между сетями VLAN на маршрутизаторе R1.
```sh
R1(config)#int g0/1
R1(config-if)#no sh
R1(config-if)#int g0/1.100
R1(config-if)#encapsulation dot1Q 100
R1(config-if)#description Clients
R1(config-if)#ip address 192.168.1.1 255.255.255.192

R1(config)#int g0/1.200
R1(config-if)#encapsulation dot1Q 200
R1(config-if)#description Managment
R1(config-if)#ip address 192.168.1.65 255.255.255.224

R1(config)#int g0/1.1000
R1(config)#description Own

R1(config-subif)#do sh ip int br
Interface                  IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0         10.0.0.1        YES NVRAM  up                    up  
GigabitEthernet0/1         unassigned      YES unset  up                    up  
GigabitEthernet0/1.100     192.168.1.1     YES NVRAM  up                    up  
GigabitEthernet0/1.200     192.168.1.65    YES NVRAM  up                    up  
GigabitEthernet0/1.1000    unassigned      YES unset  up                    up  
GigabitEthernet0/2         unassigned      YES unset  administratively down down
GigabitEthernet0/3         unassigned      YES unset  administratively down down
```
### Шаг 5.Настройте G0/1 на R2, затем G0/0/0 и статическую маршрутизацию для обоих маршрутизаторов.
**Настройте G0/0/1 на R2 с первым IP-адресом подсети C, рассчитанным ранее.**
```sh
R2#conf t
R2(config)#int g0/1
R2(config-if)#ip address 192.168.1.97 255.255.255.240
R2(config-if)#no sh
R2(config-if)#
*Jun  2 15:22:27.806: %LINK-3-UPDOWN: Interface GigabitEthernet0/1, changed state to up
*Jun  2 15:22:28.806: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up
```

**Настройте интерфейс G0/0/0 для каждого маршрутизатора на основе приведенной выше таблицы IP-адресации.**

**Маршрутизатор R1**
```sh
R1(config-if)#int g0/0
R1(config-if)#ip address 10.0.0.1255.255.255.252
R1(config-if)#no sh
R1(config-if)#
R1(config-if)#
*Jun  2 15:23:32.916: %LINK-3-UPDOWN: Interface GigabitEthernet0/0, changed state to up
*Jun  2 15:23:33.917: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up
R2(config-if)#
R1>
R1>
R1>en
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.

R1(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.2
R1(config)#
R1#wr
Building configuration...
*Jun  2 15:44:50.895: %SYS-5-CONFIG_I: Configured from console by console[OK]
R1#
*Jun  2 15:44:54.874: %GRUB-5-CONFIG_WRITING: GRUB configuration is being updated on disk. Please wait...
*Jun  2 15:44:55.678: %GRUB-5-CONFIG_WRITTEN: GRUB configuration was written to disk successfully.

R1#ping 192.168.1.97
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.97, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 2/2/3 ms
R1#
```

**Маршрутизатор R2**
```sh
R2(config-if)#int g0/0
R2(config-if)#ip address 10.0.0.2 255.255.255.252
R2(config-if)#no sh
R2(config-if)#
R2(config-if)#
*Jun  2 15:23:32.916: %LINK-3-UPDOWN: Interface GigabitEthernet0/0, changed state to up
*Jun  2 15:23:33.917: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up
R2(config-if)#
R2>
R2>
R2>en
R2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.

R2(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.1
R2(config)#
R2#wr
Building configuration...
*Jun  2 15:44:50.895: %SYS-5-CONFIG_I: Configured from console by console[OK]
*Jun  2 15:44:54.874: %GRUB-5-CONFIG_WRITING: GRUB configuration is being updated on disk. Please wait...
*Jun  2 15:44:55.678: %GRUB-5-CONFIG_WRITTEN: GRUB configuration was written to disk successfully.
```

### Шаг 6.Настройте базовые параметры каждого коммутатора.
**Коммутатор S1**
```sh
Switch(config)#hostname S1
S1(config)#no ip domain-lookup
S1(config)#enable password class
S1(config)#line con 0
S1(config-line)#password cisco
S1(config-line)#line vty 0 4
S1(config-line)#password cisco
S1(config-line)#end
S1#conf
Jun  2 15:58:34.049: %SYS-5-CONFIG_I: Configured from console by console t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#service password-encryption
S1(config)#banner motd # Attention For staff only #
S1(config)#
S1(config)#
S1(config)#do wr
Building configuration...
Compressed configuration from 3121 bytes to 1512 bytes[OK]
S1(config)#
S1(config)#
S1(config)#
Jun  2 15:59:39.023: %GRUB-5-CONFIG_WRITING: GRUB configuration is being updated on disk. Please wait...
Jun  2 15:59:39.751: %GRUB-5-CONFIG_WRITTEN: GRUB configuration was written to disk successfully.
S1(config)#
S1#clo
Jun  2 15:59:40.624: %SYS-5-CONFIG_I: Configured from console by consoleck set 16:01:00 02 jul 2022
S1#
Jul  2 16:01:00.000: %SYS-6-CLOCKUPDATE: System clock has been updated from 16:00:09 UTC Thu Jun 2 2022 to 16:01:00 UTC Sat Jul 2 2022, configured from console by console.
S1#
S1#
S1#clock update-calendar
S1#copy running-config startup-config
Destination filename [startup-config]?
Building configuration...
Compressed configuration from 3180 bytes to 1556 bytes[OK]
S1#
Jul  2 16:02:38.518: %GRUB-5-CONFIG_WRITING: GRUB configuration is being updated on disk. Please wait...
Jul  2 16:02:39.264: %GRUB-5-CONFIG_WRITTEN: GRUB configuration was written to disk succes
```
**Коммутатор S2**
```sh
Switch(config)#hostname S2
S2(config)#no ip domain-lookup
S2(config)#enable password class
S2(config)#line con 0
S2(config-line)#password cisco
S2(config-line)#line vty 0 4
S2(config-line)#password cisco
S2(config-line)#end
S2#conf
Jun  2 15:58:34.049: %SYS-5-CONFIG_I: Configured from console by console t
Enter configuration commands, one per line.  End with CNTL/Z.
S2(config)#service password-encryption
S2(config)#banner motd # Attention For staff only #
S2(config)#do wr
Building configuration...
Compressed configuration from 3121 bytes to 1512 bytes[OK]
Jun  2 15:59:39.023: %GRUB-5-CONFIG_WRITING: GRUB configuration is being updated on disk. Please wait...
Jun  2 15:59:39.751: %GRUB-5-CONFIG_WRITTEN: GRUB configuration was written to disk successfully.
S2(config)#
Jun  2 15:59:40.624: %SYS-5-CONFIG_I: Configured from console by consoleck set 16:01:00 02 jul 2022
S2#
Jul  2 16:01:00.000: %SYS-6-CLOCKUPDATE: System clock has been updated from 16:00:09 UTC Thu Jun 2 2022 to 16:01:00 UTC Sat Jul 2 2022, configured from console by console.
S2#clock update-calendar
S2#copy running-config startup-config
Destination filename [startup-config]?
Building configuration...
Compressed configuration from 3180 bytes to 1556 bytes[OK]
Jul  2 16:02:38.518: %GRUB-5-CONFIG_WRITING: GRUB configuration is being updated on disk. Please wait...
Jul  2 16:02:39.264: %GRUB-5-CONFIG_WRITTEN: GRUB configuration was written to disk succes
```

### Шаг 7.Создайте сети VLAN на коммутаторе S1.

**Создайте необходимые VLAN на коммутаторе 1 и присвойте им имена из приведенной выше таблицы.**
```sh
S1(config)#vlan 100
S1(config-vlan)#name Clients
S1(config-vlan)#vlan 200
S1(config-vlan)#name Managment
S1(config)#vlan 999
S1(config-vlan)#name Parking_Lot
S1(config-vlan)#vlan 1000
S1(config-vlan)#name Own
```

**Настройте и активируйте интерфейс управления на S1 (VLAN 200), используя второй IP-адрес из подсети, рассчитанный ранее. Кроме того установите шлюз по умолчанию на S1**
```sh
S1(config-vlan)#int vlan 200
S1(config-if)#no sh
S1(config-if)#ip address 192.168.1.66 255.255.255.224
S1(config)#ip default-gateway 192.168.1.65
```

**Настройте и активируйте интерфейс управления на S2 (VLAN 1), используя второй IP-адрес из подсети, рассчитанный ранее. Кроме того, установите шлюз по умолчанию на S2**
```sh
S2(config-vlan)#int vlan 1
S2(config-if)#no sh
S2(config-if)#ip address 192.168.1.98 255.255.255.240
S2(config)#ip default-gateway 192.168.1.97
```

**Назначьте все неиспользуемые порты S1 VLAN Parking_Lot, настройте их для статического режима доступа и административно деактивируйте их. На S2 административно деактивируйте все неиспользуемые порты**
```sh
S1(config)#int range g0/0
S1(config-if-range)#sh
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#int range g0/3
S1(config-if-range)#sh
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#int range g1/0-3
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#sh
```

### Шаг 8.Назначьте сети VLAN соответствующим интерфейсам коммутатора.
```sh
S1(config-if)#sw a v 100
S1(config-if)#do sh vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Gi0/1
100  Clients                          active    Gi0/2
200  Managment                        active    
999  Parking_Lot                      active    Gi0/0, Gi0/3, Gi1/0, Gi1/1
                                                Gi1/2, Gi1/3
1000 Own                              active
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
1    enet  100001     1500  -      -      -        -    -        0      0
100  enet  100100     1500  -      -      -        -    -        0      0
200  enet  100200     1500  -      -      -        -    -        0      0
999  enet  100999     1500  -      -      -        -    -        0      0
1000 enet  101000     1500  -      -      -        -    -        0      0
1002 fddi  101002     1500  -      -      -        -    -        0      0
1003 tr    101003     1500  -      -      -        -    -        0      0
```

### Шаг 9.Вручную настройте интерфейс S1 F0/5(g0/1) в качестве транка 802.1Q.
```sh
S1(config)#int g0/1
S1(config-if)#switchport mode trunk
S1(config-if)#switchport trunk native vlan 1000
S1(config-if)#switchport trunk allowed vlan 100,200,1000

S1(config)#do sh int g0/1 sw
Name: Gi0/1
Switchport: Enabled
Administrative Mode: trunk
Operational Mode: trunk
Administrative Trunking Encapsulation: dot1q
Operational Trunking Encapsulation: dot1q
Negotiation of Trunking: On
Access Mode VLAN: 1 (default)
Trunking Native Mode VLAN: 1000 (Inactive)
Administrative Native VLAN tagging: enabled
Voice VLAN: none
Administrative private-vlan host-association: none
Administrative private-vlan mapping: none
Administrative private-vlan trunk native VLAN: none
Administrative private-vlan trunk Native VLAN tagging: enabled
Administrative private-vlan trunk encapsulation: dot1q
Administrative private-vlan trunk normal VLANs: none
Administrative private-vlan trunk associations: none
Administrative private-vlan trunk mappings: none
Operational private-vlan: none
Trunking VLANs Enabled: 100,200,1000
Pruning VLANs Enabled: 2-1001
Capture Mode Disabled
Capture VLANs Allowed: ALL

Protected: false
Appliance trust: none
```

## Часть 2:	Настройка и проверка двух серверов DHCPv4 на R1.

### Шаг 1.	Настройте R1 с пулами DHCPv4 для двух поддерживаемых подсетей. Ниже приведен только пул DHCP для подсети A.
```sh
R1(config)#ip dhcp excluded-address 192.168.1.1 192.168.1.6
R1(config)#ip dhcp excluded-address 192.168.1.97 192.168.1.102

R1(dhcp-config)#ip dhcp pool Clients
R1(dhcp-config)#network 192.168.1.0 255.255.255.192
R1(dhcp-config)#default-router 192.168.1.1
R1(dhcp-config)#domain-name CCNA-lab.com
R1(dhcp-config)#dns-server 192.168.1.1
R1(dhcp-config)#lease 2 12 30

R1(dhcp-config)#ip dhcp pool R2_Client_LAN 
R1(dhcp-config)#network 192.168.1.96 255.255.255.240
R1(dhcp-config)#default-router 192.168.1.97
R1(dhcp-config)#domain-name CCNA-lab.com
R1(dhcp-config)#dns-server 192.168.1.97
R1(dhcp-config)#lease 2 12 30
Jun  3 15:32:58.706: %SYS-5-CONFIG_I: Configured from console by consolef t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(dhcp-config)#do wr

R1#sh ip dhcp binding
Bindings from all pools not associated with VRF:
IP address          Client-ID/              Lease expiration        Type
                    Hardware address/
                    User name
192.168.1.7         0100.5079.6668.05       Jun 06 2022 05:15 AM    Automatic

R1#sh ip dhcp pool
Pool Clients :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0
 Total addresses                : 62
 Leased addresses               : 1
 Pending event                  : none
 1 subnet is currently in the pool :
 Current index        IP address range                    Leased addresses
 192.168.1.8          192.168.1.1      - 192.168.1.62      1
Pool R2_Client_LAN :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0
 Total addresses                : 14
 Leased addresses               : 0
 Pending event                  : none
 1 subnet is currently in the pool :
 Current index        IP address range                    Leased addresses
 192.168.1.97         192.168.1.97     - 192.168.1.110     0

R1#sh ip dhcp server stat
Memory usage         34188
Address pools        2
Database agents      0
Automatic bindings   1
Manual bindings      0
Expired bindings     0
Malformed messages   0
Secure arp entries   0
Message              Received
BOOTREQUEST          0
DHCPDISCOVER         18
DHCPREQUEST          2
DHCPDECLINE          0
DHCPRELEASE          0
DHCPINFORM           0
Message              Sent
BOOTREPLY            0
DHCPOFFER            2
DHCPACK              2
DHCPNAK              0
R1#
R1#
R1#
```

## Часть 3:	Настройка и проверка DHCP-ретрансляции на R2

### Шаг 1.	Настройка R2 в качестве агента DHCP-ретрансляции для локальной сети на G0/0/1(g0/1).
```sh
R2(config)#int g0/1
R2(config-if)#ip helper-address 10.0.0.1
R2(config-if)#do wr
```
### Шаг 2.	Попытка получить IP-адрес от DHCP на PC-B.

**Из командной строки компьютера PC-B выполните команду ipconfig /all.**

![image](https://user-images.githubusercontent.com/99355274/172020690-4b5226bc-e71d-4cbb-9415-6851794b5897.png)

**После завершения процесса обновления выполните команду ipconfig для просмотра новой информации об IP-адресе.**

![image](https://user-images.githubusercontent.com/99355274/172020701-a3d63aa1-9619-4a1e-8a74-b212de726182.png)

**Проверьте подключение с помощью пинга IP-адреса интерфейса R1 G0/0/1.**

![image](https://user-images.githubusercontent.com/99355274/172020706-cefaec5b-ec9e-4a81-a9c2-19f1f9a7cd8e.png)

**Выполните show ip dhcp binding для R1 для проверки назначений адресов в DHCP**
```sh
R1(config)#do sh ip dhcp binding
Bindings from all pools not associated with VRF:
IP address          Client-ID/              Lease expiration        Type
                    Hardware address/
                    User name
192.168.1.7         0150.0000.0100.00       Jun 07 2022 06:51 AM    Automatic
192.168.1.104       0150.0000.0200.00       Jun 07 2022 06:47 AM    Automatic
```
**Выполните команду show ip dhcp server statistics для проверки сообщений DHCP.**
```sh
R1(config)#do sh ip dhcp server  stat
Memory usage         42695
Address pools        2
Database agents      0
Automatic bindings   2
Manual bindings      0
Expired bindings     0
Malformed messages   0
Secure arp entries   0

Message              Received
BOOTREQUEST          0
DHCPDISCOVER         10
DHCPREQUEST          7
DHCPDECLINE          0
DHCPRELEASE          0
DHCPINFORM           2

Message              Sent
BOOTREPLY            0
DHCPOFFER            3
DHCPACK              4
DHCPNAK              0
R1(config)#
```
