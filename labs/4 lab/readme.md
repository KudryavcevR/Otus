## Задачи

![image](https://user-images.githubusercontent.com/99355274/158801632-16b7973d-de3b-41be-8ecb-3b97522bef25.png)

## Часть 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора

![image](https://user-images.githubusercontent.com/99355274/158765881-86009a80-1d80-4a4e-9740-abc0c3a184e7.png)

  1.1 Настройка маршрутизатора.
  ```sh
          Router(config)# hostname R1
          R1(config)#
          R1(config)#enable secret class
   ```       
  1.2 Настройка коммутатора
   ```sh         
          Switch(config)# hostname S1
          S1(config)#
          S1(config)#enable secret class
   ```     
## Часть 2. Ручная настройка IPv6-адресов

  2.1 Настройка мартрутизатора
   ```sh  
          R1(config)#int g0/0/0
          R1(config-if)#ipv6 address 2001:db8:acad:a::1/64
          R1(config-if)#no sh
          R1(config-if)#
          %LINK-5-CHANGED: Interface GigabitEthernet0/0/0, changed state to up
          %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/0, changed state to up
         
         R1(config-if)#int g0/0/1
          R1(config-if)#ipv6 address 2001:db8:acad:1::1/64
          R1(config-if)#no sh
          R1(config-if)#
          %LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up
          %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up
          R1(config-if)#end
          R1#
          %SYS-5-CONFIG_I: Configured from console by console
          
          R1#show ipv6 int br
          GigabitEthernet0/0/0       [up/up]
              FE80::201:43FF:FE06:4B01
              2001:DB8:ACAD:A::1
          GigabitEthernet0/0/1       [up/up]
              FE80::201:43FF:FE06:4B02
              2001:DB8:ACAD:1::1
          Vlan1                      [administratively down/down]
              unassigned
          R1#conf t
          Enter configuration commands, one per line.  End with CNTL/Z.
          R1(config)#int g0/0/0
          R1(config-if)#ipv6 address fe80::1 link-local
          R1(config-if)#int g0/0/1
          R1(config-if)#ipv6 address fe80::1 link-local
          R1(config-if)#exit 
   ```       
   
  2.2 Активируйте IPv6-маршрутизацию на R1.
  
   ```sh   
          R1>en
          R1>en
          R1#conf t
          Enter configuration commands, one per line.  End with CNTL/Z.
          R1(config)#ipv6 unicast-routing
   ```      
 Введите команду ipconfig на PC-B. Проверьте данные IPv6-адреса
  
  ![image](https://user-images.githubusercontent.com/99355274/158790969-eba3e05e-a468-46d7-9399-5c704e7144dd.png)

  2.3 Назнаьте IPv6-адреса интерфейсу управления(SVI) на S1
      
          S1(config)#sdm prefer dual-ipv4-and-ipv6 default
          S1(config)#do show sdm prefer
          The current template is "default" template.
          The selected template optimizes the resources in
          the switch to support this level of features for
          0 routed interfaces and 1024 VLANs.
          number of unicast mac addresses:                  8K
          number of IPv4 IGMP groups + multicast routes:    0.25K
          number of IPv4 unicast routes:                    0
          number of IPv6 multicast groups:                  0
          number of directly-connected IPv6 addresses:      0
          number of indirect IPv6 unicast routes:           0
          number of IPv4 policy based routing aces:         0
          number of IPv4/MAC qos aces:                      0.125k
          number of IPv4/MAC security aces:                 0.375k
          number of IPv6 policy based routing aces:         0
          number of IPv6 qos aces:                          20
          number of IPv6 security aces:                     25
          On next reload, template will be "dual-ipv4-and-ipv6 default" template.
          S1(config)#end
          
          S1#reload
          System configuration has been modified. Save? [yes/no]:y
          Building configuration...
          [OK]
          Proceed with reload? [confirm]
          
          S1h#en
          S1#
          S1#conf t
          Enter configuration commands, one per line.  End with CNTL/Z.
          S1(config)#int vlan 1
          S1(config-if)#ipv6 address 2001:db8:acad:1::b/64
          S1(config-if)#no sh
          S1(config-if)#
          %LINK-5-CHANGED: Interface Vlan1, changed state to up
          %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
          
          S1(config-if)#do show ipv6 int vlan 1
          Vlan1 is up, line protocol is up
          IPv6 is enabled, link-local address is FE80::207:ECFF:FE87:C40B
          No Virtual link-local address(es):
          Global unicast address(es):
          2001:DB8:ACAD:1::B, subnet is 2001:DB8:ACAD:1::/64
          Joined group address(es):
          FF02::1
          FF02::1:FF00:B
          FF02::1:FF87:C40B
          MTU is 1500 bytes
          ICMP error messages limited to one every 100 milliseconds
          ICMP redirects are enabled
          ICMP unreachables are sent
          Output features: Check hwidb
          ND DAD is enabled, number of DAD attempts: 1
          ND reachable time is 30000 milliseconds
          
          
  2.4 Назначьте компьютерам статические IPv6-адреса
      
  PC-A
  
  ![image](https://user-images.githubusercontent.com/99355274/158796767-3cd91484-2088-4d9a-b7af-e649cdaa2152.png)

  PC-B
  
  ![image](https://user-images.githubusercontent.com/99355274/158796798-439defe9-4407-48bf-addb-6dfe45b8fceb.png)


## Часть 3. Проверка сквозного подключения

  Эхо запрос с PC-A на локальный адрес канала(FE80::1)
  
  ![image](https://user-images.githubusercontent.com/99355274/158800422-f0a6ba3d-9983-4422-94a5-23fa72e4a955.png)

  Эхо запрос с PC-A на S1
  
  ![image](https://user-images.githubusercontent.com/99355274/158800454-3cb8e2c8-bde7-48d0-88b0-6bc9a7bea7aa.png)

  Проверка скозного подключения с PC-A на PC-B
  
  ![image](https://user-images.githubusercontent.com/99355274/158800482-75b41c83-4a24-4465-8032-085ac7e77f11.png)

  Эхо запрос с PC-B на PC-A
  
  ![image](https://user-images.githubusercontent.com/99355274/158800516-3f387529-2037-4b2f-bdb8-a0aa6cf047d5.png)

  Эхо запрос с PC-B на локальный адрес канала G0/0/0 на R1
  
  ![image](https://user-images.githubusercontent.com/99355274/158800549-abd43628-d041-430f-bf36-93f4c939739f.png)
