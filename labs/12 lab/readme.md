## Часть 1:Создание сети и настройка основных параметров устройства.

### 1.1 Подключите кабели сети согласно приведенной топологии.
![image](https://user-images.githubusercontent.com/99355274/174094193-6578dbb1-7988-4ec2-a070-cb7479582db1.png)

### 1.2 Произведите базовую настройку маршрутизаторов.
**Коммутатор R1**
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
R1(config)#service password-encryption
R1(config)#banner motd # Attentions! For staff only!!! #
R1(config)#int g0/0
R1(config-if)#ip address 209.165.200.230 255.255.255.248
R1(config-if)#no sh
R1(config-if)#int g0/1
R1(config-if)#ip address 192.168.1.1 255.255.255.0
R1(config-if)#exit
R1(config)#ip route 0.0.0.0 0.0.0.0 209.165.200.225
R1(config)#do wr
```
**Коммутатор R2**
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
R2(config)#banner motd # Attentions! For staff only!!! #
R2(config)#int g0/0
R2(config-if)#ip address 209.165.200.225 255.255.255.248
R2(config-if)#no sh
R2(config-if)#int loopback1
R2(config-if)#ip address 209.165.200.1 255.255.255.224
R2(config-if)#exit
R2(config)#ip route 0.0.0.0 0.0.0.0 209.165.200.230
R2(config)#do wr
```
### 1.3 Настройте базовые параметры каждого коммутатора.
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
S1(config)#banner motd # Attentions! For staff only!!! #
S2(config)#int range g1/0-3
S2(config-if)#sh
S1(config-if)#int vlan 1
S1(config-if)#ip address 192.168.1.2 255.255.255.0
S1(config-if)#exit
S1(config)#ip route 0.0.0.0 0.0.0.0 192.168.1.1
S1(config)#do wr
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
S2(config)#banner motd # Attentions! For staff only!!! #
S2(config)#int range g1/0-3
S2(config-if)#sh
S2(config)#int g0/2
S2(config-if)#sh
S2(config-if)#int vlan 1
S2(config-if)#ip address 192.168.1.3 255.255.255.0
S2(config-if)#exit
S2(config)#ip route 0.0.0.0 0.0.0.0 192.168.1.1
S2(config)#do wr
```
## Часть 2: Настройка и проверка NAT для IPv4.

### 2.1 Настройте NAT на R1, используя пул из трех адресов 209.165.200.226-209.165.200.228.
```sh
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255 
R1(config)# ip nat pool PUBLIC_ACCESS 209.165.200.226 209.165.200.228 netmask 255.255.255.248 
R1(config)# ip nat inside source list 1 pool PUBLIC_ACCESS
R1(config)# interface g0/0/1
R1(config-if)# ip nat inside
R1(config)# interface g0/0/0
R1(config-if)# ip nat outside
R1# show ip nat translations
```

### 2.2 Проверьте и проверьте конфигурацию. 
**Ping PC-B to 209.165.200.1**
```sh
PC-B> ping 209.165.200.1 
84 bytes from 209.165.200.1 icmp_seq=1 ttl=254 time=19.845 ms
PC-B>
```
**R1 show nat translations**
```sh
R1#sh ip nat trans
Pro Inside global      Inside local       Outside local      Outside global
icmp 209.165.200.228:33624 192.168.1.3:33624 209.165.200.1:33624 209.165.200.1:33624
--- 209.165.200.228    192.168.1.3        ---                ---
R1#
```

**Ping PC-A to 209.165.200.1**
```sh
PC-A> ping 209.165.200.1
PC-A>
```
**R1 show nat translations**
```sh
R1#sh ip nat trans
Pro Inside global      Inside local       Outside local      Outside global
icmp 209.165.200.226:345 192.168.1.2:345  209.165.200.1:345  209.165.200.1:345
--- 209.165.200.226    192.168.1.2        ---                ---
--- 209.165.200.228    192.168.1.3        ---                ---
```

**Ping S1 to 209.165.200.1**
```sh
S1#ping 209.165.200.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 209.165.200.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 3/4/6 ms
```
**R1 show nat translations**
```sh
R1#sh ip nat trans
Pro Inside global      Inside local       Outside local      Outside global
--- 209.165.200.226    192.168.1.2        ---                ---
--- 209.165.200.228    192.168.1.3        ---                ---
--- 209.165.200.227    192.168.1.11       ---                ---
```

**Ping S2 to 209.165.200.1**
```sh
S2#ping 209.165.200.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 209.165.200.1, timeout is 2 seconds:
U.U.U
Success rate is 0 percent (0/5)
S2#
```
**R1 show nat translations**
```sh
R1#sh ip nat trans
Pro Inside global      Inside local       Outside local      Outside global
--- 209.165.200.226    192.168.1.2        ---                ---
--- 209.165.200.228    192.168.1.3        ---                ---
--- 209.165.200.227    192.168.1.11       ---                ---

R1#clear ip nat trans *
R1#clear ip nat stat
```
## Часть 3: Настройка и проверка PAT для IPv4.
### 3.1 Удалите команду преобразования на R1.
```sh
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#no ip nat inside source list 1 pool PUBLIC_ACCESS
R1(config)#
```
### 3.2 Добавьте команду PAT на R1.
```sh
R1(config)# ip nat inside source list 1 pool PUBLIC_ACCESS overload 
```
### 3.3 Протестируйте и проверьте конфигурацию.
**Ping PC-A to 209.165.200.1**
```sh
PC-A> ping 209.165.200.1 -c 1
84 bytes from 209.165.200.1 icmp_seq=1 ttl=254 time=11.441 ms
PC-A>
```
**R1 show nat translations**
```sh
R1(config)#do sh ip nat trans
Pro      Inside global        Inside local      Outside local        Outside global
icmp 209.165.200.228:32603 192.168.1.3:32603 209.165.200.1:32603 209.165.200.1:32603
```
**R1 show nat translations verbose**
```sh
R1#show ip nat translations verbose
Pro     Inside global      Inside local       Outside local      Outside global
icmp 209.165.200.228:52572 192.168.1.2:52572 209.165.200.1:52572 209.165.200.1:52572
    create 00:00:33, use 00:00:33 timeout:60000, left 00:00:26, Map-Id(In): 2,
    flags:
extended, use_count: 0, entry-id: 69, lc_entries: 0
R1#
```

**Ping PC-A & PC-B to 209.165.200.1**
```sh
PC-A> ping 209.165.200.1 -t
84 bytes from 209.165.200.1 icmp_seq=1 ttl=254 time=15.539 ms

PC-B> ping 209.165.200.1 -t
84 bytes from 209.165.200.1 icmp_seq=1 ttl=254 time=11.539 ms
```
**R1 show nat translations**
```sh
R1#show ip nat translations
Pro     Inside global      Inside local       Outside local      Outside global
icmp 209.165.200.228:49501 192.168.1.2:49501 209.165.200.1:49501 209.165.200.1:49501
icmp 209.165.200.228:49757 192.168.1.2:49757 209.165.200.1:49757 209.165.200.1:49757
icmp 209.165.200.228:50013 192.168.1.2:50013 209.165.200.1:50013 209.165.200.1:50013
icmp 209.165.200.228:50269 192.168.1.2:50269 209.165.200.1:50269 209.165.200.1:50269
icmp 209.165.200.228:50525 192.168.1.2:50525 209.165.200.1:50525 209.165.200.1:50525
 --More--

R1# clear ip nat translations * 
R1# clear ip nat statistics 
```

### 3.4 На R1 удалите команды преобразования nat pool.
```sh
R1(config)# no ip nat inside source list 1 pool PUBLIC_ACCESS overload 
R1(config)# no ip nat pool PUBLIC_ACCESS
```
### 3.5 Добавьте команду PAT overload, указав внешний интерфейс.
```sh
R1(config)#ip nat inside source list 1 interface g0/0 overload
```

### 3.6 Протестируйте и проверьте конфигурацию.
**Ping PC-B to 209.165.200.1**
```sh
PC-B> ping 209.165.200.1
84 bytes from 209.165.200.1 icmp_seq=1 ttl=254 time=11.950 ms
```
**R1 show nat translations**
```sh
R1#show ip nat translations
Pro    Inside global      Inside local       Outside local          Outside global
icmp 209.165.200.230:49504 192.168.1.3:49504 209.165.200.1:49504 209.165.200.1:49504
```

**Сделайте трафик с нескольких устройств**
```sh
PC-A> ping 209.165.200.1 -t
PC-B> ping 209.165.200.1 -t
S1#ping 209.165.200.1 repeat 2000
S2#ping 209.165.200.1 repeat 2000
```
**R1 show nat translations**
```
R1#sh ip nat trans
Pro Inside global      Inside local       Outside local      Outside global
icmp 209.165.200.230:0 192.168.1.2:99     209.165.200.1:99   209.165.200.1:0
icmp 209.165.200.230:1 192.168.1.2:355    209.165.200.1:355  209.165.200.1:1
icmp 209.165.200.230:512 192.168.1.2:611  209.165.200.1:611  209.165.200.1:512
icmp 209.165.200.230:513 192.168.1.2:867  209.165.200.1:867  209.165.200.1:513
icmp 209.165.200.230:1174 192.168.1.2:1123 209.165.200.1:1123 209.165.200.1:1174
icmp 209.165.200.230:1175 192.168.1.2:1379 209.165.200.1:1379 209.165.200.1:1175
icmp 209.165.200.230:1176 192.168.1.2:1635 209.165.200.1:1635 209.165.200.1:1176
icmp 209.165.200.230:1177 192.168.1.2:1891 209.165.200.1:1891 209.165.200.1:1177
icmp 209.165.200.230:1178 192.168.1.2:2147 209.165.200.1:2147 209.165.200.1:1178
icmp 209.165.200.230:1179 192.168.1.2:2403 209.165.200.1:2403 209.165.200.1:1179
icmp 209.165.200.230:1180 192.168.1.2:2659 209.165.200.1:2659 209.165.200.1:1180
icmp 209.165.200.230:1181 192.168.1.2:2915 209.165.200.1:2915 209.165.200.1:1181
icmp 209.165.200.230:1182 192.168.1.2:3171 209.165.200.1:3171 209.165.200.1:1182
icmp 209.165.200.230:1183 192.168.1.2:3427 209.165.200.1:3427 209.165.200.1:1183
icmp 209.165.200.230:1184 192.168.1.2:3683 209.165.200.1:3683 209.165.200.1:1184
icmp 209.165.200.230:1185 192.168.1.2:3939 209.165.200.1:3939 209.165.200.1:1185
icmp 209.165.200.230:4195 192.168.1.2:4195 209.165.200.1:4195 209.165.200.1:4195
icmp 209.165.200.230:4451 192.168.1.2:4451 209.165.200.1:4451 209.165.200.1:4451
icmp 209.165.200.230:4707 192.168.1.2:4707 209.165.200.1:4707 209.165.200.1:4707
icmp 209.165.200.230:4963 192.168.1.2:4963 209.165.200.1:4963 209.165.200.1:4963
icmp 209.165.200.230:5219 192.168.1.2:5219 209.165.200.1:5219 209.165.200.1:5219
icmp 209.165.200.230:5475 192.168.1.2:5475 209.165.200.1:5475 209.165.200.1:5475
icmp 209.165.200.230:5731 192.168.1.2:5731 209.165.200.1:5731 209.165.200.1:5731
icmp 209.165.200.230:5987 192.168.1.2:5987 209.165.200.1:5987 209.165.200.1:5987
icmp 209.165.200.230:6243 192.168.1.2:6243 209.165.200.1:6243 209.165.200.1:6243
icmp 209.165.200.230:6499 192.168.1.2:6499 209.165.200.1:6499 209.165.200.1:6499
icmp 209.165.200.230:6755 192.168.1.2:6755 209.165.200.1:6755 209.165.200.1:6755
icmp 209.165.200.230:1169 192.168.1.2:64610 209.165.200.1:64610 209.165.200.1:1169
icmp 209.165.200.230:1170 192.168.1.2:64866 209.165.200.1:64866 209.165.200.1:1170
icmp 209.165.200.230:1171 192.168.1.2:65122 209.165.200.1:65122 209.165.200.1:1171
icmp 209.165.200.230:1172 192.168.1.2:65378 209.165.200.1:65378 209.165.200.1:1172
icmp 209.165.200.230:99 192.168.1.3:99    209.165.200.1:99   209.165.200.1:99
icmp 209.165.200.230:355 192.168.1.3:355  209.165.200.1:355  209.165.200.1:355
icmp 209.165.200.230:611 192.168.1.3:611  209.165.200.1:611  209.165.200.1:611
icmp 209.165.200.230:867 192.168.1.3:867  209.165.200.1:867  209.165.200.1:867
icmp 209.165.200.230:1173 192.168.1.3:1123 209.165.200.1:1123 209.165.200.1:1173
icmp 209.165.200.230:1379 192.168.1.3:1379 209.165.200.1:1379 209.165.200.1:1379
icmp 209.165.200.230:1635 192.168.1.3:1635 209.165.200.1:1635 209.165.200.1:1635
icmp 209.165.200.230:1891 192.168.1.3:1891 209.165.200.1:1891 209.165.200.1:1891
icmp 209.165.200.230:2147 192.168.1.3:2147 209.165.200.1:2147 209.165.200.1:2147
icmp 209.165.200.230:2403 192.168.1.3:2403 209.165.200.1:2403 209.165.200.1:2403
icmp 209.165.200.230:2659 192.168.1.3:2659 209.165.200.1:2659 209.165.200.1:2659
icmp 209.165.200.230:2915 192.168.1.3:2915 209.165.200.1:2915 209.165.200.1:2915
icmp 209.165.200.230:3171 192.168.1.3:3171 209.165.200.1:3171 209.165.200.1:3171
icmp 209.165.200.230:3427 192.168.1.3:3427 209.165.200.1:3427 209.165.200.1:3427
icmp 209.165.200.230:3683 192.168.1.3:3683 209.165.200.1:3683 209.165.200.1:3683
icmp 209.165.200.230:3939 192.168.1.3:3939 209.165.200.1:3939 209.165.200.1:3939
icmp 209.165.200.230:65122 192.168.1.3:65122 209.165.200.1:65122 209.165.200.1:65122
icmp 209.165.200.230:65378 192.168.1.3:65378 209.165.200.1:65378 209.165.200.1:65378
icmp 209.165.200.230:7 192.168.1.12:7     209.165.200.1:7    209.165.200.1:7
R1#
```

## Часть 4. Настройка и проверка статического NAT для IPv4.
### 4.1 На R1 очистите текущие трансляции и статистику.
```sh
R1#clear ip nat trans *
R1#clear ip nat stat
R1#
```
### 4.2 На R1 настройте команду NAT, необходимую для статического сопоставления внутреннего адреса с внешним адресом.
```sh
R1(config)# ip nat inside source static 192.168.1.2 209.165.200.229 
```
### 4.3 Протестируйте и проверьте конфигурацию.
**R1 show nat translations**
```sh
R1(config)#do sh ip nat trans
Pro Inside global      Inside local       Outside local      Outside global
--- 209.165.200.229    192.168.1.2        ---                ---
R1(config)#
```
**Ping R2 to R1**
```sh
R2#ping 209.165.200.229
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 209.165.200.229, timeout is 2 seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max = 7/10/18 ms
R2#
```
**R1 show nat translations**
```sh
R1#sh ip nat trans
Pro Inside global      Inside local       Outside local      Outside global
icmp 209.165.200.229:4 192.168.1.2:4      209.165.200.225:4  209.165.200.225:4
--- 209.165.200.229    192.168.1.2        ---                ---
```
