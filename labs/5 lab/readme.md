# Часть 1
## Шаг 1
## Шаг 2
## Шаг 3 Настройте маршрутизатор
```sh
    Router(config)#no ip domain-lookup
    Router(config)#enable secret class
    Router(config)#int g0/0/1
    Router(config-if)#Enter configuration commands, one per line.  End with CNTL/Z.
    Router(config)#Enter configuration commands, one per line.  End with CNTL/Z.
    Router(config)#line vty 0 4
    Router(config-line)#exit
    Router(config)#line con 0
    Router(config-line)#password cisco
    Router(config-line)#login
    Router(config-line)#exit
    Router(config)#line vty 0 5
    Router(config-line)#password cisco
    Router(config-line)#login
    Router(config-line)#service password-encryption
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

## Шаг 4 Настройте компьютер PC-A

![image](https://user-images.githubusercontent.com/99355274/159234811-384ecc47-f50d-4d56-9819-5c84638e0aa8.png)


## Шаг 5 Проверьте подключение к сети

![image](https://user-images.githubusercontent.com/99355274/159234623-beb44043-e73c-4aa9-ac39-83f2210299da.png)


# Часть 2. Настройка маршрутизатора для доступа по SSH

