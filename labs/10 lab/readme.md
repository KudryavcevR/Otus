![image](https://user-images.githubusercontent.com/99355274/172456904-1b4b103b-177a-45a7-bbe8-2f802b6ecb82.png)

## Часть 1. Создание сети и настройка основных параметров устройства
### Шаг 1. Создайте сеть согласно топологии.
![image](https://user-images.githubusercontent.com/99355274/172456987-09525db8-8dd2-4367-8428-224ab59d8f68.png)

### Шаг 2. Произведите базовую настройку маршрутизаторов.
**Маршрутизатор R1**
```sh
Router(config)#hostname R1
R1(config)#no ip domain-lookup
R1(config)#enable password class
R1(config)#line con 0
R1(config-line)#password cisco
R1(config-line)#line vty 0 4
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
R1(config)#service enc
R1(config)#service password-encryption
R1(config)#banner motd ## Attention! For staff only!! #
R1(config)#do wr
Building configuration...
[OK]
R1(config)#
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
R2(config-line)#login
R2(config-line)#exit
R2(config)#service password-encryption
R2(config)#banner motd ## Attention! For staff only!! #
R2(config)#do wr
Building configuration...
[OK]
```
### Шаг 3. Настройте базовые параметры каждого коммутатора.
**Коммутатор S1**
```sh
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
S1(config)#banner motd ## Attention! For staff only!! #
S1(config)#do wr
Building configuration...
[OK]
S1(config)#
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
S2(config-line)#login
S2(config-line)#exit
S2(config)#service password-encryption
S2(config)#banner motd ## Attention! For staff only!! #
S2(config)#do wr
Building configuration...
[OK]
S2(config)#
```
## Часть 2. Настройка и проверка базовой работы протокола OSPFv2 для одной области

### Шаг 1. Настройте адреса интерфейса и базового OSPFv2 на каждом маршрутизаторе.

**Маршрутизатор R2**
```sh
Router(config)#hostname R2
R2(config)#no ip domain-lookup
R2(config)#int g0/0
R2(config-if)#ip add
R2(config-if)#ip address 10.53.0.2 255.255.255.0
R2(config-if)#no sh

R2(config-if)#int loopback1
Jun  7 16:37:12.997: %LINK-3-UPDOWN: Interface GigabitEthernet0/0, changed state to up
Jun  7 16:37:13.997: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to uppback1
R2(config-if)#
R2(config-if)#
R2(config-if)#
Jun  7 16:37:17.127: %LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up add
R2(config-if)#ip address 192.168.1.1 255.255.255.0
R2(config-if)#no sh

R2(config-if)#router ospf 56
R2(config-router)#router-id 2.2.2.2
R2(config-router)#network 10.53.0.0 0.0.0.255 area 0
Jun  7 16:38:34.679: %OSPF-5-ADJCHG: Process 56, Nbr 1.1.1.1 on GigabitEthernet0/0 from LOADING to FULL, Loading Done
R2(config-router)#network 192.168.1.0 255.255.255.255 area 0
```

**Маршрутизатор R1**
```sh
Router(config)#hostname R1
R1(config)#no ip domain-lookup
R1(config)#int g0/0
R1(config-if)#ip address 10.53.0.1 255.255.255.0
R1(config-if)#no sh

R1(config-if)#int loopback1
Jun  7 16:37:12.997: %LINK-3-UPDOWN: Interface GigabitEthernet0/0, changed state to up
Jun  7 16:37:13.997: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to uppback1
R1(config-if)#
Jun  7 16:37:17.127: %LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up add
R1(config-if)#ip address 172.16.1.1 255.255.255.0
R1(config-if)#no sh

R1(config-if)#router ospf 56
R1(config-router)#router-id 1.1.1.1
R1(config-router)#network 10.53.0.0 0.0.0.255 area 0
Jun  7 16:38:34.679: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0 from LOADING to FULL, Loading Done
R1(config-router)#network 172.16.1.0 255.255.255.255 area 0

R1#sh ip route ospf
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR

Gateway of last resort is not set

      192.168.1.0/32 is subnetted, 1 subnets
O        192.168.1.1 [110/1501] via 10.53.0.2, 00:16:08, GigabitEthernet0/0

R1#ping 192.168.1.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 4/4/5 ms
R1#
```
## Часть 3. Оптимизация и проверка конфигурации OSPFv2 для одной области
### Шаг 1. Реализация различных оптимизаций на каждом маршрутизаторе.

**На R1 настройте приоритет OSPF**
```sh
R1(config)#int g0/0
R1(config-if)#ip ospf priority 50
```

**Настройте таймеры OSPF**
```sh
R1(config)#int g0/0
R1(config-if)#ip ospf hello-interval 30
```
```sh
R2(config)#int g0/0
R2(config-if)#ip ospf hello-interval 30
```

**На R1 настройте статический маршрут по умолчанию, который использует интерфейс Loopback 1. Затем распространите маршрут по умолчанию в OSPF**
```sh
R1(config)#ip route 0.0.0.0 0.0.0.0 loopback1
R1(config)#router ospf 56
R1(config-router)#default-information originate
```

**Добавьте конфигурацию, необходимую для OSPF и конфигурацию для предотвращения отправки объявлений OSPF в сеть Loopback 1.**
```sh
R2(config-if)#ip ospf network point-to-point
R2(config-if)#router ospf 56
R2(config-router)#passive-interface loopback1
```

**Измените базовую пропускную способность для маршрутизаторов. После этой настройки перезапустите OSPF с помощью команды clear ip ospf process**
```sh
R1(config)#int g0/0
R1(config-if)#ip ospf cost 1501
R1(config-if)#
R1#c
*Jun  7 17:54:36.936: %SYS-5-CONFIG_I: Configured from console by consolelear
R1#clear ip os
R1#clear ip ospf pro
R1#clear ip ospf process
Reset ALL OSPF processes? [no]:
R1#sh ip ospf int g0/0
GigabitEthernet0/0 is up, line protocol is up
  Internet Address 10.53.0.1/24, Area 0, Attached via Network Statement
  Process ID 56, Router ID 1.1.1.1, Network Type BROADCAST, Cost: 1501
  Topology-MTID    Cost    Disabled    Shutdown      Topology Name
        0           1501      no          no            Base
  Transmit Delay is 1 sec, State DR, Priority 50
  Designated Router (ID) 1.1.1.1, Interface address 10.53.0.1
  Backup Designated router (ID) 2.2.2.2, Interface address 10.53.0.2
  Timer intervals configured, Hello 30, Dead 120, Wait 120, Retransmit 5
    oob-resync timeout 120
    Hello due in 00:00:21
  Supports Link-local Signaling (LLS)
  Cisco NSF helper support enabled
  IETF NSF helper support enabled
  Index 1/1/1, flood queue length 0
  Next 0x0(0)/0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 1, Adjacent neighbor count is 1
    Adjacent with neighbor 2.2.2.2  (Backup Designated Router)
  Suppress hello for 0 neighbor(s)
R1#
R1#
```

### Шаг 2. Убедитесь, что оптимизация OSPFv2 реализовалась.

**На R1 выполните команду show ip route ospf, чтобы убедиться, что сеть R2 Loopback1 присутствует в таблице маршрутизации**
```sh
R1#sh ip route ospf
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR

Gateway of last resort is 0.0.0.0 to network 0.0.0.0

O     192.168.1.0/24 [110/1502] via 10.53.0.2, 00:01:50, GigabitEthernet0/0
R1#
```

**Введите команду show ip route ospf на маршрутизаторе R2. Единственная информация о маршруте OSPF должна быть распространяемый по умолчанию маршрут R1.**
```sh
R2#sh ip route ospf
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR

Gateway of last resort is 10.53.0.1 to network 0.0.0.0

O*E2  0.0.0.0/0 [110/1] via 10.53.0.1, 00:22:17, GigabitEthernet0/0
```

**Запустите Ping до адреса интерфейса R1 Loopback 1 из R2. Выполнение команды ping должно быть успешным.**
```sh
R2#ping 192.168.1.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms
R2#
```
