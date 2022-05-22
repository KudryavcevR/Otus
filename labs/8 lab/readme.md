![image](https://user-images.githubusercontent.com/99355274/165709304-7b1aa725-c1da-4463-b948-ac0161898a05.png)


## Часть 1: Создание сети и настройка основных параметров устройства.

### 1.1 Создайте сеть согласно топологии.

![image](https://user-images.githubusercontent.com/99355274/169703991-0ee48ff6-6b50-43f3-882d-7358cd104fe7.png)

### 1.2 Произведите базовую настройку маршрутизаторов.
```sh
R1(config)#hostname R1
R1(config)#no ip domain-lookup
R1(config)#enable password class
R1(config)#line con 0
R1(config-line)#password cisco
R1(config-line)#line vty 0 4
R1(config-line)#password cisco
R1(config-line)#exit
R1(config)#service password-encryption
R1(config)#banner motd ## Attention!! For staff only!! ##
R1#wr
```
### 1.3 Настройка интерфейсов и маршрутизации для обоих маршрутизаторов.
```sh
R1(config)#int g0/0
R1(config-if)#ipv6 address FE80::1 link-local
R1(config-if)#ipv6 address 2001:DB8:ACAD:2::1/64
R1(config)#int g0/1
R1(config-if)#ipv6 address FE80::1 link-local
R1(config-if)#ipv6 address 2001:DB8:ACAD:1::1/64
R1#
R1#ping 2001:DB8:ACAD:3::1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 2001:DB8:ACAD:3::1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/4 ms
R1#
R1#wr
```

## Часть 2: Проверка назначения адреса SLAAC от R1.
```sh
VPCS> sh ipv6 all

NAME   IP/MASK                                 ROUTER LINK-LAYER  MTU
VPCS1  fe80::250:79ff:fe66:6807/64
       2001:db8:acad:1:2050:79ff:fe66:6807/64  50:00:00:04:00:01  1500

```

## Часть 3: Настройка и проверка сервера DHCPv6 на R1

### 3.1 Более подробно изучите конфигурацию PC-A.

![image](https://user-images.githubusercontent.com/99355274/169704555-61fe0fa5-813b-43ea-adbb-5f6f94203ce9.png)

### 3.2 Настройте R1 для предоставления DHCPv6 без состояния для PC-A.
```sh
R1(config)# ipv6 dhcp pool R1-STATELESS
R1(config-dhcp)# dns-server 2001:db8:acad::254
R1(config-dhcp)# domain-name STATELESS.com

R1(config)# interface g0/0/1
R1(config-if)# ipv6 nd other-config-flag 
R1(config-if)# ipv6 dhcp server R1-STATELESS
```
### 3.3 Выведите результат.

![image](https://user-images.githubusercontent.com/99355274/169702979-b4410cb9-3ed0-4f49-b0ac-8a644cf32ddc.png)

![image](https://user-images.githubusercontent.com/99355274/169703170-d723f2ce-b9e2-4558-8997-a462a8a454ec.png)


## Часть 4: Настройка сервера DHCPv6 с сохранением состояния на R1.

### 4.1 Создайте пул DHCPv6 на R1 для сети 2001:db8:acad:3:aaa::/80.
```sh
R1(config)# ipv6 dhcp pool R2-STATEFUL
R1(config-dhcp)# address prefix 2001:db8:acad:3:aaa::/80
R1(config-dhcp)# dns-server 2001:db8:acad::254
R1(config-dhcp)# domain-name STATEFUL.com
```
### 4.2 Назначьте только что созданный пул DHCPv6 интерфейсу g0/0/0 на R1.
```sh
R1(config)# interface g0/0/0
R1(config-if)# ipv6 dhcp server R2-STATEFUL
```

## Часть 5: Настройка и проверка ретрансляции DHCPv6 на R2.

### 5.1 Включите PC-B и проверьте адрес SLAAC, который он генерирует.

![image](https://user-images.githubusercontent.com/99355274/169703792-c3ec3411-1f70-425b-bd44-c701de0f75f5.png)

### 5.2 Настройте R2 в качестве агента DHCP-ретрансляции для локальной сети на G0/0/1.
```sh
R2 (конфигурация) # int g0/1
R2(config-if)# ipv6 nd managed-config-flag
R2(config-if)# ipv6 dhcp relay destination 2001:db8:acad:2::1 g0/0
```
### 5.3 Попытка получить адрес IPv6 из DHCPv6 на PC-B.

![image](https://user-images.githubusercontent.com/99355274/169703819-5231d35f-b8fc-4845-880e-9c6aff185d9b.png)

![image](https://user-images.githubusercontent.com/99355274/169703832-5f869f1e-5ad1-46d1-b89c-5efd80fb4c48.png)

