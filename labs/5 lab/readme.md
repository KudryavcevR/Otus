![image](https://user-images.githubusercontent.com/99355274/159246614-0013d85f-30a1-4e3b-8778-a9f28f0439dd.png)

# Часть 1.
## Шаг 1.
## Шаг 2.
## Шаг 3. Настройте маршрутизатор.
```sh
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

