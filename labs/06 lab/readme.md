![image](https://user-images.githubusercontent.com/99355274/160384967-08dafc3a-2271-4b3b-8942-714f9514bac3.png)
![image](https://user-images.githubusercontent.com/99355274/160385008-29bc9ca6-9f54-4347-ab27-141e1eb70f17.png)

## Часть 1. Создание сети и настройка основных параметра устройства.

 ### Шаг 1. Создайте сеть согласно топологии.
![image](https://user-images.githubusercontent.com/99355274/160415751-7c62ada6-8172-427a-acc2-64d979964594.png)

 ### Шаг 2. Настройте базовые параметры для маршрутизатора.
 ```sh 
      Router0>en
      Router0#conf t
      Enter configuration commands, one per line.  End with CNTL/Z.
      Router0(config)#hostname R1
      R1(config)#no ip domain lookup
      R1(config)#enable password class
      R1(config)#line con 0
      R1(config-line)#password cisco
      R1(config-line)#login
      R1(config-line)#line vty 0 5
      R1(config-line)#password cisco
      R1(config-line)#login
      R1(config-line)#exit
      R1(config)#service password-encryption
      R1(config)#banner motd ## Attention!! ##
      R1#copy running-config startup-config 
      Destination filename [startup-config]? 
      Building configuration...
      [OK]
      R1#clock set 14:48 28 march 2022
      R1#
 ```

 ### Шаг 3. Настройте базовые параметры каджого комммутатора.
 **Коммутатор S1**    
```sh  
      Switch0>en 
      Switch0#conf t
      Enter configuration commands, one per line.  End with CNTL/Z.
      Switch0(config)#hostname S2
      S1(config)#no ip domain lookup
      S1(config)#enable password class
      S1(config-line)#line con 0
      S1(config-line)#password cisco
      S1(config-line)#login
      S1(config-line)#line vty 0 5
      S1(config-line)#password cisco
      S1(config-line)#login
      S1(config-line)#exit
      S1(config)#service password-encryption 
      S1(config)#banner motd ## Attention!! ##
      %SYS-5-CONFIG_I: Configured from console by console
      S1#clock set 14:31 28 march 2022
      S1#copy running-config startup-config 
      Destination filename [startup-config]? 
      Building configuration...
      [OK]
 ```   
 **Коммутатор S2**
   ```sh   
      Switch0>en 
      Switch0#conf t
      Enter configuration commands, one per line.  End with CNTL/Z.
      Switch0(config)#hostname S2
      S2(config)#no ip domain lookup
      S2(config)#enable password class
      S2(config-line)#line con 0
      S2(config-line)#password cisco
      S2(config-line)#login
      S2(config-line)#line vty 0 5
      S2(config-line)#password cisco
      S2(config-line)#login
      S2(config-line)#exit
      S2(config)#service password-encryption 
      S2(config)#banner motd ## Attention!! ##
      %SYS-5-CONFIG_I: Configured from console by console
      S2#clock set 14:31 28 march 2022
      S2#copy running-config startup-config 
      Destination filename [startup-config]? 
      Building configuration...
      [OK]
   ```   
 ### Шаг 4. Настройте узлы ПК
![image](https://user-images.githubusercontent.com/99355274/160416622-23b28669-26a1-4032-9eaa-584cbd74ba91.png)
![image](https://user-images.githubusercontent.com/99355274/160416682-b1ec8802-9650-4e0c-8d19-fc1112eeda34.png)
 
## Часть 2. Создание сетей VLAN и назначение портов коммутатора.

 ### Шаг 1. Создайте Vlan на коммутаторах.
    
 **Коммутатор S1**
    
   1.1 Создайте Vlan на S1
   ```sh  
      S1(config)#vlan 10
      S1(config-vlan)#name Managment
      S1(config-vlan)#vlan 20
      S1(config-vlan)#name Sales
      S1(config-vlan)#vlan 30
      S1(config-vlan)#name Operations
      S1(config-vlan)#vlan 999
      S1(config-vlan)#name Parking_lot
      S1(config-vlan)#vlan 1000
      S1(config-vlan)#name Own
   ```
   1.2 Настройте интерфейс управления и шлюз
   ```sh
      S1(config)#int vlan 10
      S1(config-if)#ip address 192.168.10.11 255.255.255.0
      S1(config-if)#ex
      S1(config)#ip default-gateway 192.168.10.1
   ```
   1.3 Назначьте все неиспользуемые порты коммутатора VLAN Parking_Lot
   ```sh
        S1(config)#int range f0/2-4,f0/7-24,g0/1-2
        S1(config-if-range)#switchport mode access
        S1(config-if-range)#switchport access vlan 999
        S1(config-if-range)#sh
   ```   
 **Коммутатор S2**
    
   1.1 Создайте Vlan на S2
   ```sh   
      S2(config)#vlan 10
      S2(config-vlan)#name Managment
      S2(config-vlan)#vlan 20
      S2(config-vlan)#name Sales
      S2(config-vlan)#vlan 30
      S2(config-vlan)#name Operations
      S2(config-vlan)#vlan 999
      S2(config-vlan)#name Parking_lot
      S2(config-vlan)#vlan 1000
      S2(config-vlan)#name Own
   ```
   1.2 Настройте интерфейс управления и шлюз
   ```sh
      S2(config)#int vlan 10
      S2(config-if)#ip address 192.168.10.12 255.255.255.0
      S2(config-if)#ex
      S2(config)#ip default-gateway 192.168.10.1
   ```
   1.3 Назначьте все неиспользуемые порты коммутатора VLAN Parking_Lot
   ```sh
      S2(config)#int range f0/2-17,f0/19-24,g0/1-2
      S2(config-if-range)#switchport mode access
      S2(config-if-range)#switchport access vlan 999
      S2(config-if-range)#sh
   ```
  ### Шаг 2. Назначьте сети VLAN соответствующим интерфейсам коммутатора.
 **Коммутатор S1**
   ```sh
      S1(config)#int f0/6
      S1(config-if)#switchport access vlan 20
      S1(config)#do show vlan
      VLAN Name                             Status    Ports
      ---- -------------------------------- --------- -------------------------------
      1    default                          active    
      10   VLAN0010                         active    
      20   VLAN0020                         active    Fa0/6
      30   VLAN0030                         active    
      999  VLAN0999                         active    Fa0/2, Fa0/3, Fa0/4, Fa0/7
                                                      Fa0/8, Fa0/9, Fa0/10, Fa0/11
                                                      Fa0/12, Fa0/13, Fa0/14, Fa0/15
                                                      Fa0/16, Fa0/17, Fa0/18, Fa0/19
                                                      Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                      Fa0/24, Gig0/1, Gig0/2
   ```
**Коммутатор S2**
   ```sh
      S2(config)#int f0/18
      S2(config-if)#switchport access vlan 30
      S2(config)#do show vlan
      VLAN Name                             Status    Ports
      ---- -------------------------------- --------- -------------------------------
      1    default                          active    
      10   Managment                        active    
      20   Sales                            active    
      30   Operations                       active    Fa0/18
      999  Parking_lot                      active    Fa0/2, Fa0/3, Fa0/4, Fa0/5
                                                      Fa0/6, Fa0/7, Fa0/8, Fa0/9
                                                      Fa0/10, Fa0/11, Fa0/12, Fa0/13
                                                      Fa0/14, Fa0/15, Fa0/16, Fa0/17
                                                      Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                      Fa0/23, Fa0/24, Gig0/1, Gig0/2
   ```
## Часть 3. Конфигурация магистрального канала стандарта 802.1Q между коммутаторами.

  ### Шаг 1. Вручную настройте магистральный интерфейс F0/1 на коммутаторах S1 и S2.
**Коммутатор S1**
  ```sh
      S1(config)#int f0/1
      S1(config-if)#switchport mode trunk
      S1(config-if)#switchport trunk native vlan 1000
      S1(config-if)#switchport trunk allowed vlan 10,20,30,1000
      S1(config-if)#do show int trunk
      Port        Mode         Encapsulation  Status        Native vlan
      Fa0/1       on           802.1q         trunking      1000

      Port        Vlans allowed on trunk
      Fa0/1       10,20,30,1000

      Port        Vlans allowed and active in management domain
      Fa0/1       10,20,30,1000

      Port        Vlans in spanning tree forwarding state and not pruned
      Fa0/1       none
   ``` 
**Коммутатор S2**
   ```sh
      S2(config)#int f0/1
      S2(config-if)#switchport mode trunk
      S2(config-if)#switchport trunk native vlan 1000
      S2(config-if)#switchport trunk allowed vlan 10,20,30,1000
      S2(config-if)#do show int trunk
      Port        Mode         Encapsulation  Status        Native vlan
      Fa0/1       on           802.1q         trunking      1000

      Port        Vlans allowed on trunk
      Fa0/1       10,20,30,1000

      Port        Vlans allowed and active in management domain
      Fa0/1       10,20,30,1000

      Port        Vlans in spanning tree forwarding state and not pruned
      Fa0/1       none
   ```
   ### Шаг 2. Вручную настройте магистральный интерфейс F0/5 на коммутаторе S1. 
```sh
      S1(config)#int f0/5
      S1(config-if)#switchport mode trunk
      S1(config-if)#switchport trunk native vlan 1000
      S1(config-if)#switchport trunk allowed vlan 10,20,30,1000
      S1#copy running-config startup-config 
      Destination filename [startup-config]? 
      Building configuration...
      [OK]
      S1(config-if)#do show int trunk
      Port        Mode         Encapsulation  Status        Native vlan
      Fa0/1       on           802.1q         trunking      1000
      Fa0/5       on           802.1q         trunking      1000

      Port        Vlans allowed on trunk
      Fa0/1       10,20,30,1000
      Fa0/5       10,20,30,1000

      Port        Vlans allowed and active in management domain
      Fa0/1       10,20,30,1000
      Fa0/5       10,20,30,1000

      Port        Vlans in spanning tree forwarding state and not pruned
      Fa0/1       10,20,30,1000
      Fa0/5       none
```
## Часть 4. Настройка маршрутизации  между сетями VLAN
 ### Шаг 1. Настройте маршрутизатор.
```sh
      R1(config)#int g0/0/1.10
      R1(config-subif)#encapsulation dot1Q 10
      R1(config-subif)#ip address 192.168.10.1 255.255.255.0
      R1(config-subif)#no sh
      R1(config)#int g0/0/1.20
      R1(config-subif)#encapsulation dot1Q 20
      R1(config-subif)#ip address 192.168.20.1 255.255.255.0
      R1(config-subif)#no sh
      R1(config)#int g0/0/1.30
      R1(config-subif)#encapsulation dot1Q 30
      R1(config-subif)#ip address 192.168.30.1 255.255.255.0
      R1(config-subif)#no sh
      R1(config)#int g0/0/1.1000
      R1(config-subif)#encapsulation dot1Q 1000
      R1(config-subif)#no sh
```
## Часть 5. Проверьте работает ли маршрутизация между VLAN
 ### Шаг 1. Выполните следующие тесты с PC-A. Все должно быть успешно.
Отправьте эхо-запрос с PC-A на шлюз по умолчанию.
    
![image](https://user-images.githubusercontent.com/99355274/160416860-29b482ba-137e-4287-be5d-a580c3053d93.png)

Отправьте эхо-запрос с PC-A на PC-B.
    
![image](https://user-images.githubusercontent.com/99355274/160416966-7b4eb814-16b6-4280-b77d-71f4e2e8271a.png)

Отправьте команду ping с компьютера PC-A на коммутатор S2.
    
![image](https://user-images.githubusercontent.com/99355274/160417041-9f1b15aa-ab2c-4ac1-b430-6c1b4000bf78.png)

 ### Шаг 2. Пройдите следующий тест с PC-B.
В окне командной строки на PC-B выполните команду tracert на адрес PC-A.
    
![image](https://user-images.githubusercontent.com/99355274/160417109-425d15de-9576-44bc-a7b1-59b1752e034b.png)
