![image](https://user-images.githubusercontent.com/99355274/169709986-9e2bb61f-fcc3-4d2a-a3e1-59e019d08781.png)
![image](https://user-images.githubusercontent.com/99355274/169710042-3a5d8022-2522-47a9-b7e6-faa1d0a98206.png)

## Часть 1: Настройка основного сетевого устройства.

### Шаг 1. Создайте сеть.
![image](https://user-images.githubusercontent.com/99355274/169803518-0f708f12-dce9-483b-b263-8cf55e87349d.png)

### Шаг 2. Настройте маршрутизатор R1.
```sh
enable
configure terminal
hostname R1
no ip domain lookup
ip dhcp excluded-address 192.168.10.1 192.168.10.9
ip dhcp excluded-address 192.168.10.201 192.168.10.202
!
ip dhcp pool Students
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 domain-name CCNA2.Lab-11.6.1
!
interface Loopback0
 ip address 10.10.1.1 255.255.255.0
!
interface GigabitEthernet0/1
 description Link to S1
 ip dhcp relay information trusted
 ip address 192.168.10.1 255.255.255.0
 no shutdown
!
line con 0
 logging synchronous
 exec-timeout 0 0
 ```
 ![image](https://user-images.githubusercontent.com/99355274/169803642-108d718d-329f-4073-bc86-c407aaca70ff.png)

### Шаг 3. Настройка и проверка основных параметров коммутатора.
**Коммутатор S1**
```sh
Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#
May 22 18:02:56.712: %PNP-6-PNP_DISCOVERY_STOPPED: PnP Discovery stopped (Config Wizard)
Switch(config)#
Switch(config)#
Switch(config)#hostna
Switch(config)#hostname S1
S1(config)#no ip domain-lookup
S1(config-if)#
May 22 18:07:31.038: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan10, changed state to down
S1(config)#int g0/0
S1(config-if)#des
S1(config-if)#description to PC
S1(config-if)#int g0/2
S1(config-if)#des
S1(config-if)#description to R1
S1(config-if)#
S1(config-if)#
S1(config-if)#int g0/1
S1(config-if)#des
S1(config-if)#description to S2
S1(config-if)#
S1(config)#ip default-gateway 192.168.10.1
```
**Коммутатор S2**
```sh
Switch(config)#
Switch(config)#hostname S2
S2(config)#no ip domain-lookup
S2(config)#int g0/0
S2(config-if)#desc
S2(config-if)#description to PC
S2(config-if)#int g0/1
S2(config-if)#description to S1
S2(config-if)#ex
S2(config)#ip defaul
S2(config)#ip default-g
S2(config)#ip default-gateway 192.168.10.1
S2(config)#
```

## Часть 2: Настройка сетей VLAN на коммутаторах.
### Шаг 1. Сконфигруриуйте VLAN 10.
**Коммутатор S1**
```sh
S1(config)#vlan 10
S1(config-vlan)#name Managment
S1(config-vlan)#
S1(config-if)#ip address 192.168.10.201 255.255.255.0
```
**Коммутатор S2**
```sh
S2(config)#vlan 10
S2(config-vlan)#name Managment
S2(config-vlan)#
S2(config-vlan)#int vlan 10
S2(config-if)#ip add 192.168.10.202 255.255.255.0
```

### Шаг 3. Настройте VLAN 333 с именем Native, VLAN 999 с именем ParkingLo на S1 и S2.
**Коммутатор S1**
```sh
S1(config)#vlan 333
S1(config-vlan)#name Native
S1(config-vlan)#vlan 999
S1(config-vlan)#name ParkingLot
S1(config-vlan)#
```
**Коммутатор S2**
```sh
S2(config)#vlan 333
S2(config-vlan)#name Native
S2(config-vlan)#vlan 999
S2(config-vlan)#name ParkingLot
S2(config-vlan)#
```

## Часть 3: Настройки безопасности коммутатора.
### Шаг 1. Релизация магистральных соединений 802.1Q.
**Коммутатор S1**
```sh
S1(config)#int g0/1
S1(config-if)#switchport trunk encapsulation dot1q
S1(config-if)#sw m t
S1(config-if)#
May 22 18:58:19.387: %LINK-3-UPDOWN: Interface Vlan10, changed state to up
May 22 18:58:20.387: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan10, changed state to up
S1(config-if)#switchport trunk native vlan 333
S1(config-if)#do sh int trunk

Port        Mode             Encapsulation  Status        Native vlan
Gi0/1       on               802.1q         trunking      333

Port        Vlans allowed on trunk
Gi0/1       1-4094

Port        Vlans allowed and active in management domain
Gi0/1       1,10,333,999

Port        Vlans in spanning tree forwarding state and not pruned
Gi0/1       1,10,333,999

S1(config-if)#switchport nonegotiate
S1(config-if)#do sh int g0/1 sw | include Negotiation
Negotiation of Trunking: Off
```
**Коммутатор S2**
```sh
S2(config)#int g0/1
S2(config-if)#sw
S2(config-if)#switchport trunk encapsulation dot1q
S2(config-if)#sw m t
S2(config-if)#
S2(config-if)#sw trunk native vlan 333
S2(config)#do sh int trunk

Port        Mode             Encapsulation  Status        Native vlan
Gi0/1       on               802.1q         trunking      333

Port        Vlans allowed on trunk
Gi0/1       1-4094

Port        Vlans allowed and active in management domain
Gi0/1       1,10,333,999

Port        Vlans in spanning tree forwarding state and not pruned
Gi0/1       1,10,333,999

S2(config-if)#switchport nonegotiate
S2(config-if)#do sh int g0/1 sw | include Negotiation
Negotiation of Trunking: Off
```
### Шаг 2. Настройка портов доступа.
**Коммутатор S1**
```sh
S1(config-if)#int range g0/0,g0/2
S1(config-if-range)#switchport access vlan 10
```
**Коммутатор S2**
```sh
S2(config-if)#int g0/0
S2(config-if)#switchport access vlan 10
```
### Шаг 3. Безопасность неиспользуемых портов коммутатора.
**Коммутатор S1**
```sh
S1(config)#int g0/3
S1(config-if-range)#sw a vla 999
S1(config)#int g1/0
S1(config-if)#int range g1/0-3
S1(config-if-range)#sw a vla 999

S1#sh int status
Port      Name               Status       Vlan       Duplex  Speed Type
Gi0/0     to PC              connected    10         a-full   auto RJ45
Gi0/1     to S2              connected    trunk      a-full   auto RJ45
Gi0/2     to R1              connected    10         a-full   auto RJ45
Gi0/3                        disabled     999          auto   auto RJ45
Gi1/0                        disabled     999          auto   auto RJ45
Gi1/1                        disabled     999          auto   auto RJ45
Gi1/2                        disabled     999          auto   auto RJ45
Gi1/3                        disabled     999          auto   auto RJ45
```
**Коммутатор S2**
```sh
S2(config)#int range g0/2-3
S2(config-if-range)#sw a vla 999
S2(config)#int g1/0
S2(config-if)#int range g1/0-3
S2(config-if-range)#sw a vla 999

S2#sh int status
Gi0/0     to PC              connected    10         a-full   auto RJ45
Gi0/1     to S1              connected    trunk      a-full   auto RJ45
Gi0/2                        disabled     999          auto   auto RJ45
Gi0/3                        disabled     999          auto   auto RJ45
Gi1/0                        disabled     999          auto   auto RJ45
Gi1/1                        disabled     999          auto   auto RJ45
Gi1/2                        disabled     999          auto   auto RJ45
Gi1/3                        disabled     999          auto   auto RJ45
```

### Шаг 4. Документирование и реализация функций безопасности порта.
**Коммутатор S1**
```sh
S1#sh port-security int g0/0
Port Security              : Disabled
Port Status                : Secure-down
Violation Mode             : Shutdown
Aging Time                 : 0 mins
Aging Type                 : Absolute
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 1
Total MAC Addresses        : 0
Configured MAC Addresses   : 0
Sticky MAC Addresses       : 0
Last Source Address:Vlan   : 0000.0000.0000:0
Security Violation Count   : 0

S1(config)#int g0/0
S1(config-if)#switchport port-security
S1(config-if)#switchport port-security maximum 3
S1(config-if)#switchport port-security violation restrict
S1(config-if)#switchport port-security aging time 60
S1(config-if)#switchport port-security aging type inactivity

S1(config-if)#do sh port-sec int g0/0
Port Security              : Enabled
Port Status                : Secure-up
Violation Mode             : Restrict
Aging Time                 : 60 mins
Aging Type                 : Inactivity
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 3
Total MAC Addresses        : 0
Configured MAC Addresses   : 0
Sticky MAC Addresses       : 0
Last Source Address:Vlan   : 0000.0000.0000:0
Security Violation Count   : 0
```
**Коммутатор S2**
```sh
S2(config)#int g0/0
S2(config-if)#switchport port-security
S2(config-if)#switchport port-security maximum 2
S2(config-if)#switchport port-security violation protect
S2(config-if)#switchport port-security agi
S2(config-if)#switchport port-security aging time 60

S2(config-if)#do sh port-sec address
               Secure Mac Address Table
-----------------------------------------------------------------------------
Vlan    Mac Address       Type                          Ports   Remaining Age
                                                                   (mins)
----    -----------       ----                          -----   -------------
  10    5000.0002.0000    SecureDynamic                 Gi0/0       60
-----------------------------------------------------------------------------
Total Addresses in System (excluding one mac per port)     : 0
Max Addresses limit in System (excluding one mac per port) : 4096
```
### Шаг 5. Реализовать безопасность DHCP snooping.
```sh
S2(config)#ip dhcp snooping
S2(config)#ip dhcp snooping vlan 10
S2(config)#int g0/1
S2(config-if)#ip dhcp snooping trust

S2(config-if)#do sh ip dhcp snooping
Switch DHCP snooping is enabled
Switch DHCP gleaning is disabled
DHCP snooping is configured on following VLANs:
10
DHCP snooping is operational on following VLANs:
10
DHCP snooping is configured on the following L3 Interfaces:

Insertion of option 82 is enabled
   circuit-id default format: vlan-mod-port
   remote-id: 5000.0004.0000 (MAC)
Option 82 on untrusted port is not allowed
Verification of hwaddr field is enabled
Verification of giaddr field is enabled
DHCP snooping trust/rate is configured on the following Interfaces:

Interface                  Trusted    Allow option    Rate limit (pps)
-----------------------    -------    ------------    ----------------
GigabitEthernet0/1         yes        yes             unlimited
  Custom circuit-ids:
```
### В командной строке на PC-B освободите, а затем обновите IP-адрес.

### Проверьте привязку отслеживания DHCP с помощью команды show ip dhcp snooping binding.
```sh
S2#sh ip dhcp snooping binding
MacAddress          IpAddress        Lease(sec)  Type           VLAN  Interface
------------------  ---------------  ----------  -------------  ----  --------------------
50:00:00:02:00:00   192.168.10.11    86369       dhcp-snooping   10    GigabitEthernet0/0
Total number of bindings: 1
```
### Шаг 6. Реализация PortFast и BPDU Guard.
**Коммутатор S1**
```sh
S1(config)#int range g0/0-2
S1(config-if)#spanning-tree portfast
S1(config)#int range g0/0
S1(config-if)#spanning-tree bpduguard enable
S1(config-if)#
```
**Коммутатор S2**
```sh
S2(config)#int g0/0-1
S2(config-if)#spanning-tree portfast
S2(config)#int g0/0
S2(config-if)#spanning-tree bpduguard enable
S2(config-if)#
```

### Убедитесь, что защита BPDU и PortFast включены на соответствующих портах.
```sh
S1(config-if)#do sh spanning-tree int g0/0 detail
 Port 1 (GigabitEthernet0/0) of VLAN0010 is designated forwarding
   Port path cost 4, Port priority 128, Port Identifier 128.1.
   Designated root has priority 32778, address 5000.0003.0000
   Designated bridge has priority 32778, address 5000.0003.0000
   Designated port id is 128.1, designated path cost 0
   Timers: message age 0, forward delay 0, hold 0
   Number of transitions to forwarding state: 1
   The port is in the portfast edge mode
   Link type is point-to-point by default
   Bpdu guard is enabled
   BPDU: sent 1694, received 0
 ```  
 
 ### Шаг 7. Проверьте наличие сквозного ⁪подключения.
 
![image](https://user-images.githubusercontent.com/99355274/169803428-638d9c58-df22-4cea-af57-9095ff2bc24d.png)

   
