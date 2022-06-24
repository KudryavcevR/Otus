## Задачи

![image](https://user-images.githubusercontent.com/99355274/153927058-8f24ae0d-70b2-4a4e-8f93-4f995c79e8f6.png)


## 1.Подключаем PC к Switch

![image](https://user-images.githubusercontent.com/99355274/153922552-777bbdaa-7b25-4114-8c0b-b0db05a2a52e.png)


## 2.Проверяем настройки поумолчанию и подключаем Ethernet кабель
```sh
    enable
    Show running-config
    show interface vlan 1
    show version
    show interface f0/6
    show flash
    dir flash
```
  Подключаем Ethernet кабель
 
   ![image](https://github.com/KudryavcevR/Otus/blob/main/labs/1%20lab/scrn/ethernet.JPG)

## 3.Задаем базовые настройки на свитче 

- Заходим в режим глобальной конфигурации
    ```sh
    enable
    configure terminal
    ```

- Задаем имя узла
   ```sh
   hostname S1
   ``` 
   
- Настройка шифрования пароля
   ```sh
   service password-encryprion
   ```   
   
- Задаем пароль для привилегированного режима
    ```sh
    enable secret class
    ```   
    
- Отменяем поиск в ДНС
    ```sh
    no ip domain-lookup
    ```
    
- Настройка банера
    ```sh
    banner motd # Unauthorized access is strictly prohibited. #
    ```
    
 - Настройка шлюза на коммутаторе
    ```sh
    ip default-gateway 192.168.1.1
    ```   
    
- Включаем интерфейс 0/6
   ```sh
   config
   interface f0/6
   no shutdown
   ```
   
- Создаем Vlan,назначаем ip,включаем
    ```sh
    vlan 2
    exit
    interface vlan2
    ip address 192.168.1.2 255.255.255.0
    no shutdown
    ```
    
- Привязываем Vlan к интерфейсу
    ```sh
    interface f0/6
    switchport access vlan 2
    ```
    
- Настройка пароля для консольного подключения
     ```sh
    line con 0
    password cisco
    login
    ```
    
- Настройка VTY, для удаленного подключения line vty
    ```sh
    line vty 0 15
    password cisco
    login
    ```

## 4.Задаем найстроки на ПК

![image](https://user-images.githubusercontent.com/99355274/153926956-eb0864e9-16ec-4fca-a948-6a9e5bd5b3b3.png)


## 5.Конфигурация свитча

 ```sh
 S1#sh run
Building configuration...

Current configuration : 2018 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname S1
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
!
!
!
no ip domain-lookup
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
!
interface FastEthernet0/2
!
interface FastEthernet0/3
!
interface FastEthernet0/4
!
interface FastEthernet0/5
!
interface FastEthernet0/6
 switchport access vlan 2
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan2
 ip address 192.168.1.2 255.255.255.0
!
ip default-gateway 192.168.1.1
!
banner motd # Unauthorized access is strictly prohibited. #
!
!
!
line con 0
 password 7 0822455D0A16
 logging synchronous
 login
!
line vty 0 4
 password 7 0822455D0A16
 login
line vty 5 15
 password 7 0822455D0A16
 login
!
!
!
!
end
```

## 6.Проверяем сетевое подключение

Пинг с PC-A

![image](https://user-images.githubusercontent.com/99355274/153922461-103be4e5-b4da-400f-9a7a-8118859445a4.png)

Подключение по Telnet

![image](https://user-images.githubusercontent.com/99355274/153922502-62bd7865-7198-423c-85ba-88d36354b90f.png)


