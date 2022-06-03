![image](https://user-images.githubusercontent.com/99355274/171911571-8f8154b2-a458-43fb-9ce8-89d9f5caf3b1.png)
![image](https://user-images.githubusercontent.com/99355274/171911594-4668dec8-4cbc-4ba2-b6ef-38f4dad816e4.png)


## Часть 1:	Создание сети и настройка основных параметров устройства

### Шаг 1. Создание схемы адресации
Устройство	Интерфейс	IP-адрес	Маска подсети	Шлюз по умолчанию
R1	G0/0/0	10.0.0.1	255.255.255.252	—
R1	G0/0/1	—	—	—
R1	G0/0/1.100	192.168.1.1	255.255.255.192	—
R1	G0/0/1.200	192.168.1.65	255.255.255.224	—
R1	G0/0/1.1000	—	—	—
R2	G0/0	10.0.0.2	255.255.255.252	—
R2	G0/0/1	192.168.1.97	255.255.255.240	—
S1	VLAN 200	192.168.1.66	255.255.255.224	192.168.1.65
S2	VLAN 1	192.168.1.98	255.255.255.240	192.168.1.97
PC-A	NIC	DHCP	DHCP	DHCP
PC-B	NIC	DHCP	DHCP	DHCP


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



## Настройка сети Роутера

R2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R2(config)#int g0/1
R2(config-if)#ip address 192.168.1.97 255.255.255.240
R2(config-if)#no sh
R2(config-if)#
*Jun  2 15:22:27.806: %LINK-3-UPDOWN: Interface GigabitEthernet0/1, changed state to up
*Jun  2 15:22:28.806: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up
R2(config-if)#int g0/0
R2(config-if)#ip add
R2(config-if)#ip address 10.0.0.2 255.255.255.252
R2(config-if)#no sh
R2(config-if)#
R2(config-if)#
*Jun  2 15:23:32.916: %LINK-3-UPDOWN: Interface GigabitEthernet0/0, changed state to up
*Jun  2 15:23:33.917: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up
R2(config-if)#
*Jun  2 15:36:11.542: %SYS-5-CONFIG_I: Configured from console by console
**************************************************************************
* IOSv is strictly limited to use for evaluation, demonstration and IOS  *
* education. IOSv is provided as-is and is not supported by Cisco's      *
* Technical Advisory Center. Any use or disclosure, in whole or in part, *
* of the IOSv Software or Documentation to any third party for any       *
* purposes is expressly prohibited except as otherwise authorized by     *
* Cisco in writing.                                                      *
**************************************************************************
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
R2#
*Jun  2 15:44:54.874: %GRUB-5-CONFIG_WRITING: GRUB configuration is being updated on disk. Please wait...
*Jun  2 15:44:55.678: %GRUB-5-CONFIG_WRITTEN: GRUB configuration was written to disk successfully.


## Switch
Switch(config)#hostname S1
S1(config)#no ip domain-loo
S1(config)#no ip domain-lookup
S1(config)#enable password class
S1(config)#line con 0
S1(config-line)#password cisco
S1(config-line)#line vty 0 4
S1(config-line)#password cisco
S1(config-line)#end
S1#conf
*Jun  2 15:58:34.049: %SYS-5-CONFIG_I: Configured from console by console t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#service passwor
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
*Jun  2 15:59:39.023: %GRUB-5-CONFIG_WRITING: GRUB configuration is being updated on disk. Please wait...
*Jun  2 15:59:39.751: %GRUB-5-CONFIG_WRITTEN: GRUB configuration was written to disk successfully.
S1(config)#
S1#clo
*Jun  2 15:59:40.624: %SYS-5-CONFIG_I: Configured from console by consoleck set 16:01:00 02 jul 2022
S1#
*Jul  2 16:01:00.000: %SYS-6-CLOCKUPDATE: System clock has been updated from 16:00:09 UTC Thu Jun 2 2022 to 16:01:00 UTC Sat Jul 2 2022, configured from console by console.
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

## Настройка Влан на свитчах(шаг 7-8)
S1(config)#vlan 100
S1(config-vlan)#name Clients
S1(config-vlan)#vlan 200
S1(config-vlan)#QQ
S1(config-vlan)#QQQ
S1#
Jul  2 16:27:34.778: %SYS-5-CONFIG_I: Configured from console by console
S1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#vlan 200
S1(config-vlan)#name Managment
S1(config-vlan)#vlan 999
S1(config-vlan)#name Parking_
S1(config-vlan)#name Parking_Lot
S1(config-vlan)#do sh vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Gi0/0, Gi0/1, Gi0/2, Gi0/3
                                                Gi1/0, Gi1/1, Gi1/2, Gi1/3
100  Clients                          active
200  Managment                        active
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
1    enet  100001     1500  -      -      -        -    -        0      0
100  enet  100100     1500  -      -      -        -    -        0      0
200  enet  100200     1500  -      -      -        -    -        0      0
1002 fddi  101002     1500  -      -      -        -    -        0      0
1003 tr    101003     1500  -      -      -        -    -        0      0
1004 fdnet 101004     1500  -      -      -        ieee -        0      0
1005 trnet 101005     1500  -      -      -        ibm  -        0      0

Primary Secondary Type              Ports
------- --------- ----------------- ------------------------------------------

S1(config-vlan)#
S1(config-vlan)#
S1(config-vlan)#
S1(config-vlan)#
S1#co
Jul  2 16:29:14.559: %SYS-5-CONFIG_I: Configured from console by consolenf t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#vlan 999
S1(config-vlan)#name Parking_Lot
S1(config-vlan)#vlan 1000
S1(config-vlan)#name Own
S1(config-vlan)#int vlan 200
S1(config-if)#
Jul  2 16:33:40.908: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan200, ch              anged state to down
S1(config-if)#ip add
% Incomplete command.

S1(config-if)#ip add
S1(config-if)#ip address 192.168.1.66 255.255.255.224
S1(config-if)#ip def
S1(config-if)#ip defalt
S1(config-if)#ip defalt-gat
S1(config-if)#ip defalt-gat?
% Unrecognized command
S1(config-if)#ex
S1(config)#ip def
S1(config)#ip default-gat
S1(config)#ip default-gateway 192.168.1.65
S1(config)#do sh int br
                      ^
% Invalid input detected at '^' marker.

S1(config)#do sh ip  int br
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     unassigned      YES unset  up                    up
GigabitEthernet0/1     unassigned      YES unset  up                    up
GigabitEthernet0/2     unassigned      YES unset  up                    up
GigabitEthernet0/3     unassigned      YES unset  up                    up
GigabitEthernet1/0     unassigned      YES unset  up                    up
GigabitEthernet1/1     unassigned      YES unset  up                    up
GigabitEthernet1/2     unassigned      YES unset  up                    up
GigabitEthernet1/3     unassigned      YES unset  up                    up
Vlan200                192.168.1.66    YES manual administratively down down
S1(config)#sw m a vlan ?
% Unrecognized command
S1(config)#sw m a ?
% Unrecognized command
S1(config)#sw a ?
% Unrecognized command
S1(config)#sw m a vlan 200
            ^
% Invalid input detected at '^' marker.

S1(config)#int range g0/0
S1(config-if-range)#sw m a vlan 999
                           ^
% Invalid input detected at '^' marker.

S1(config-if-range)#sw
S1(config-if-range)#switchport ?
  access         Set access mode characteristics of the interface
  autostate      Include or exclude this port from vlan link up calculation
  dot1q          Set interface dot1q properties
  host           Set port host
  mode           Set trunking mode of the interface
  nonegotiate    Device will not engage in negotiation protocol on this
                 interface
  port-security  Security related command
  private-vlan   Set the private VLAN configuration
  protected      Configure an interface to be a protected port
  trunk          Set trunking characteristics of the interface
  voice          Voice appliance attributes
  <cr>

S1(config-if-range)#switchport ac
S1(config-if-range)#switchport access ?
  vlan  Set VLAN when interface is in access mode

S1(config-if-range)#switchport access vla
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#int range g0/3
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#int range g1/0-3
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#sh
S1(config-if-range)#int g
Jul  2 16:40:34.277: %LINK-5-CHANGED: Interface GigabitEthernet1/0, changed stat              e to administratively down
Jul  2 16:40:34.324: %LINK-5-CHANGED: Interface GigabitEthernet1/1, changed stat              e to administratively down
Jul  2 16:40:34.361: %LINK-5-CHANGED: Interface GigabitEthernet1/2, changed stat              e to administratively down
Jul  2 16:40:34.395: %LINK-5-CHANGED: Interface GigabitEthernet1/3, changed stat              e to administratively down
Jul  2 16:40:35.276: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthe              rnet1/0, changed state to down0/
Jul  2 16:40:35.324: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthe              rnet1/1, changed state to down
Jul  2 16:40:35.363: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthe              rnet1/2, changed state to down
Jul  2 16:40:35.395: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthe              rnet1/3, changed state to down3
S1(config-if)#sh
S1(config-if)#int g0/
Jul  2 16:40:42.895: %LINK-5-CHANGED: Interface GigabitEthernet0/3, changed stat              e to administratively down
Jul  2 16:40:43.895: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthe              rnet0/3, changed state to down0~
                      ^
% Invalid input detected at '^' marker.

S1(config)#int g0/0
S1(config-if)#sh
S1(config-if)#
Jul  2 16:40:50.444: %LINK-5-CHANGED: Interface GigabitEthernet0/0, changed stat              e to administratively down
Jul  2 16:40:51.444: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthe              rnet0/0, changed state to down
S1(config-if)#int vlan 200
S1(config-if)#no sh
S1(config-if)#int
Jul  2 16:41:04.670: %LINK-3-UPDOWN: Interface Vlan200, changed state to downg0/              2
S1(config-if)#no sh
S1(config-if)#int g0/1
S1(config-if)#no sh
S1(config-if)#int g0/2
S1(config-if)#no sh
S1(config-if)#do sh ip unt br
                       ^
% Invalid input detected at '^' marker.

S1(config-if)#do sh ip int br
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     unassigned      YES unset  administratively down down
GigabitEthernet0/1     unassigned      YES unset  up                    up
GigabitEthernet0/2     unassigned      YES unset  up                    up
GigabitEthernet0/3     unassigned      YES unset  administratively down down
GigabitEthernet1/0     unassigned      YES unset  administratively down down
GigabitEthernet1/1     unassigned      YES unset  administratively down down
GigabitEthernet1/2     unassigned      YES unset  administratively down down
GigabitEthernet1/3     unassigned      YES unset  administratively down down
Vlan200                192.168.1.66    YES manual down                  down
S1(config-if)#int g0/2
S1(config-if)#sw a v 200
S1(config-if)#int vl
S1(config-if)#int vla
S1(config-if)#int vlan 200
S1(config-if)#no sh
S1(config-if)#
Jul  2 16:43:26.221: %LINK-3-UPDOWN: Interface Vlan200, changed state to up
Jul  2 16:43:27.220: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan200, changed state to up
S1(config-if)#
S1(config-if)#
S1(config-if)#do sh ip int br
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     unassigned      YES unset  administratively down down
GigabitEthernet0/1     unassigned      YES unset  up                    up
GigabitEthernet0/2     unassigned      YES unset  up                    up
GigabitEthernet0/3     unassigned      YES unset  administratively down down
GigabitEthernet1/0     unassigned      YES unset  administratively down down
GigabitEthernet1/1     unassigned      YES unset  administratively down down
GigabitEthernet1/2     unassigned      YES unset  administratively down down
GigabitEthernet1/3     unassigned      YES unset  administratively down down
Vlan200                192.168.1.66    YES manual up                    up
S1(config-if)#
S1(config-if)#
S1(config-if)#
S1(config-if)#do sh vlkan
                      ^
% Invalid input detected at '^' marker.

S1(config-if)#do sh vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Gi0/1
100  Clients                          active
200  Managment                        active    Gi0/2
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


  ## Настройка DHCPv4 na R1
  
R1(dhcp-config)#ip dhcp pool Managment
R1(dhcp-config)#network 192.168.1.64 255.255.255.224
R1(dhcp-config)#default-router 192.168.1.65
R1(dhcp-config)#domain-name CCNA-lab.com
R1(dhcp-config)#dns-server 192.168.1.65
R1(dhcp-config)#lease 2 12 30
Jun  3 15:32:58.706: %SYS-5-CONFIG_I: Configured from console by consolef t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#ip dhcp excluded-address 192.168.1.1 192.168.1.6
R1(config)#ip dhcp excluded-address 192.168.1.97 192.168.1.102
  
  
  
  
  R1#sh ip dhcp binding
Bindings from all pools not associated with VRF:
IP address          Client-ID/              Lease expiration        Type
                    Hardware address/
                    User name
192.168.1.7         0100.5079.6668.05       Jun 06 2022 05:15 AM    Automatic
R1#
R1#
R1#
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
R1#
R1#
R1#
R1#
R1#
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


## Настройка и проверка DHCP-ретрансляции на R2
