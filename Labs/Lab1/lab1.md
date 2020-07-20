## 2 - VLAN и маршрутизация между VLAN 
## Lab - Configure Router-on-a-Stick Inter-VLAN Routing

### EVE-NG исполнение.

#### Топология сети.
![](eve.png)

#### Таблица адресов.

| Устройство | Интерфейс | IP адрес | Маска сети | Шлюз |
| --------| --------- | --------- | -------- | ------- |
|  R1 | Ethernet1/1.3 | 192.168.3.1 | 255.255.255.0  | нет |
|  R1 | Ethernet1/1.4 | 192.168.4.1 | 255.255.255.0  | нет |
|  R1 | Ethernet1/1.8 | нет | нет  | нет |
|  S1 | VLAN 3 | 192.168.3.11 | 255.255.255.0  | 192.168.3.1 |
|  S2 | VLAN 3 | 192.168.3.12 | 255.255.255.0  | 192.168.3.1 |
|  PC-A | NIC | 192.168.3.3 | 255.255.255.0  | 192.168.3.1 |
|  PC-B | NIV | 192.168.4.3 | 255.255.255.0  | 192.168.4.1 |


#### Таблица VLAN.

| VLAN | Имя | Интерфейс | 
| --------| --------- | --------- | 
|  3 | Management | S1: VLAN 3, S2: VLAN 3, S1: e1/1 | 
|  4 | Operations | S2: e1/2 | 
|  7 | ParkingLot | остальные | 
|  8 | Native | нет | 

#### Конфигурация устройств.
[Конфигурация R1](r1-eve.txt)
[Конфигурация S1](s1-eve.txt)
[Конфигурация S2](s2-eve.txt)


#### Проверка.


[результат теста](test-eve.txt)

Ping from PC-A to its default gateway.
```
PC-A> ping 192.168.3.1
84 bytes from 192.168.3.1 icmp_seq=1 ttl=255 time=29.392 ms
84 bytes from 192.168.3.1 icmp_seq=2 ttl=255 time=9.070 ms
```
Ping from PC-A to PC-B
```
PC-A> ping 192.168.4.3
84 bytes from 192.168.4.3 icmp_seq=1 ttl=63 time=49.956 ms
84 bytes from 192.168.4.3 icmp_seq=2 ttl=63 time=18.897 ms
```
Ping from PC-A to S2
```
PC-A> ping 192.168.3.12
84 bytes from 192.168.3.12 icmp_seq=1 ttl=255 time=0.556 ms
84 bytes from 192.168.3.12 icmp_seq=2 ttl=255 time=0.580 ms
```
From the command prompt on PC-B, issue the tracert command to the address of PC-A.
```
PC-B> trace 192.168.3.3
trace to 192.168.3.3, 8 hops max, press Ctrl+C to stop
 1   192.168.4.1   15.794 ms  9.784 ms  9.169 ms
 2   *192.168.3.3   19.675 ms (ICMP type:3, code:3, Destination port unreachable)
PC-B> 
```
What intermediate IP addresses are shown in the results?
```
192.168.4.1 
```


### cisco packet tracer исполнение.

#### Топология сети.
![](pt.png)

#### Таблица адресов.

| Устройство | Интерфейс | IP адрес | Маска сети | Шлюз |
| --------| --------- | --------- | -------- | ------- |
|  R1 | GigabitEthernet0/0/1.3 | 192.168.3.1 | 255.255.255.0  | нет |
|  R1 | GigabitEthernet0/0/1.4 | 192.168.4.1 | 255.255.255.0  | нет |
|  R1 | GigabitEthernet0/0/1.8 | нет | нет  | нет |
|  S1 | VLAN 3 | 192.168.3.11 | 255.255.255.0  | 192.168.3.1 |
|  S2 | VLAN 3 | 192.168.3.12 | 255.255.255.0  | 192.168.3.1 |
|  PC-A | NIC | 192.168.3.3 | 255.255.255.0  | 192.168.3.1 |
|  PC-B | NIC | 192.168.4.3 | 255.255.255.0  | 192.168.4.1 |


#### Таблица VLAN.

 VLAN | Имя | Интерфейс 
 --------| --------- | --------- 
  3 | Management | S1: VLAN 3, S2: VLAN 3, S1: Fa0/10 
  4 | Operations | S2: Fa0/20 
  7 | ParkingLot | остальные 
  8 | Native | нет 

#### Конфигурация устройств.
[Конфигурация R1](r1-pt.txt)
[Конфигурация S1](s1-pt.txt)
[Конфигурация S2](s2-pt.txt)


#### Проверка.


[результат теста](test-pt.txt)

Ping from PC-A to its default gateway.
```
C:\>ping 192.168.3.1 
Pinging 192.168.3.1 with 32 bytes of data:
Reply from 192.168.3.1: bytes=32 time=1ms TTL=255
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255
Ping statistics for 192.168.3.1:
    Packets: Sent = 2, Received = 2, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 1ms, Average = 0ms
```
Ping from PC-A to PC-B
```
C:\>ping 192.168.4.3 
Pinging 192.168.4.3 with 32 bytes of data:
Reply from 192.168.4.3: bytes=32 time<1ms TTL=127
Reply from 192.168.4.3: bytes=32 time<1ms TTL=127
Reply from 192.168.4.3: bytes=32 time<1ms TTL=127
Ping statistics for 192.168.4.3:
    Packets: Sent = 3, Received = 3, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
Ping from PC-A to S2
```
C:\>ping 192.168.3.12 
Pinging 192.168.3.12 with 32 bytes of data:
Request timed out.
Reply from 192.168.3.12: bytes=32 time<1ms TTL=255
Reply from 192.168.3.12: bytes=32 time<1ms TTL=255
Ping statistics for 192.168.3.12:
    Packets: Sent = 3, Received = 2, Lost = 1 (34% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
From the command prompt on PC-B, issue the tracert command to the address of PC-A.
```
C:\>tracert 192.168.3.3
Tracing route to 192.168.3.3 over a maximum of 30 hops: 
  1   0 ms      0 ms      0 ms      192.168.4.1
  2   0 ms      0 ms      0 ms      192.168.3.3
Trace complete.
```
What intermediate IP addresses are shown in the results?
```
192.168.4.1 
```

### Основные команды.


Пример базовой стартовой настройки оборудования. Уточняйте количество линий vty.

```
SSS>
SSS>enable 
SSS#configure terminal 
SSS(config)#hostname S2
S2(config)#no ip domain-lookup
S2(config)#enable secret class
S2(config)#line con 0
S2(config-line)#password cisco
S2(config-line)#logging synchronous
S2(config-line)#login
S2(config-line)#exit
S2(config)#line vty 0 4
S2(config-line)#password cisco     
S2(config-line)#login         
S2(config-line)#exit
S2(config)#service password-encryption
S2(config)#banner login ^CUnauthorized access to this device is prohibited!^C
S2(config)#clock timezone UTC +3                                             
S2(config)#exit
S2#clock set 11:22:00 Jul 15 2020
S2#write memory 
```

Создание влан и настройка его под упровление.
```
S2#
S2#configure terminal 
S2(config)#vlan 3
S2(config-vlan)#name Management
S2(config-vlan)#exit
S2(config)#interface vlan 3
S2(config-if)#ip address 192.168.3.12 255.255.255.0
S2(config-if)#no shutdown
S2(config-if)#exit
S2(config)#ip default-gateway 192.168.3.1
S2(config)#exit
S2#
```

Настройка магистрального порта. В данном примере остальные вланы запрещаются, не прострелите ногу.
```
S2#
S2#configure terminal 
S2(config)#interface Ethernet0/0 
S2(config-if)#description TRUNK to S1
S2(config-if)#switchport trunk allowed vlan 3,4,8
S2(config-if)#switchport trunk encapsulation dot1q
S2(config-if)#switchport trunk native vlan 8
S2(config-if)#switchport mode trunk
S2(config-if)#no shutdown
S2(config-if)#exit
S2(config)#exit
S2#
```
Пример настройки припаркованных порта, с использованием range.
```
S2#
S2#configure terminal 
S2(config)#interface range Ethernet0/1 - 3  
S2(config-if-range)#description FREE
S2(config-if-range)#switchport access vlan 7
S2(config-if-range)#switchport mode access
S2(config-if-range)#shutdown
S2(config-if-range)#exit
S2(config)#exit
S2#
```

Пример настройки саб-интерфейса.
```
R1#
R1#configure terminal 
R1(config)#interface Ethernet1/1.4
R1(config-subif)#description Operations net
R1(config-subif)#encapsulation dot1Q 4
R1(config-subif)#ip address 192.168.4.1 255.255.255.0
R1(config-subif)#exit
R1(config)#interface Ethernet1/1                
R1(config-if)#description TRUNK to S1
R1(config-if)#no shutdown 
R1(config-if)#exit
R1(config)#exit
R1#
```
Вланы не отображаются в файле конфигурации, просмотреть их можно так.
```
S2#show vlan brief 

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    
3    Management                       active    
4    Operations                       active    Et1/2
7    ParkingLot                       active    Et0/1, Et0/2, Et0/3, Et1/0
                                                Et1/1, Et1/3
8    Native                           active    
1002 fddi-default                     act/unsup 
1003 token-ring-default               act/unsup 
1004 fddinet-default                  act/unsup 
1005 trnet-default                    act/unsup 
S2#
```

