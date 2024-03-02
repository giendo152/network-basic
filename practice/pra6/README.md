# Лабораторная работа. Настройка IPv6-адресов на сетевых устройствах 

# Топология

![Image alt](https://github.com/giendo152/network-basic/blob/main/practice/pra6/1.png)
 
# Таблица адресации

| Устройство | Интерфейс	| IP-адрес	| Маска	| Шлюз по умолчанию |
| ---------------- |:------------------:| -----------------:|--------------:|------------: |
| R1               |	G0/0/1.10	| 192.168.10.1 |	255.255.255.0 |	— |
| R1               |	G0/0/1.20	| 192.168.20.1 |	255.255.255.0 |	— |
| R1               |	G0/0/1.30	| 192.168.30.1 |	255.255.255.0 |	— |
| R1               |	G0/0/1.1000	| — |	— |	— |
| S1 | VLAN 10	| 192.168.10.11	| 255.255.255.0	| 192.168.10.1 |
| S2 | VLAN 10	| 192.168.10.12	| 255.255.255.0	| 192.168.10.1 |
| PC-A |	NIC	| 192.168.20.3 |	255.255.255.0	| 192.168.20.1 |
| PC-A |	NIC	| 192.168.30.3 |	255.255.255.0	| 192.168.30.1 |

# Таблица VLAN

| VLAN | Имя	| Назначенный интерфейс	|
| ---------------- |:------------------:| -----------------:|
| 10               |	Управление	| S1: VLAN 10 |
| 10               |	Управление	| S2: VLAN 10 |
| 20               |	Sales	| S1: F0/6 |	
| 30               |	Operations	| S2: F0/18 |	
| 999              |	Parking_Lot	| С1: F0/2-4, F0/7-24, G0/1-2 С2: F0/2-17, F0/19-24, G0/1-2 |	
| 999              |	Parking_Lot	| С2: F0/2-17, F0/19-24, G0/1-2|	
| 1000               |	Собственная	| —  |	


# Задачи

 Часть 1. Создание сети и настройка основных параметров устройства

 Часть 2. Создание сетей VLAN и назначение портов коммутатора

 Часть 3. Настройка транка 802.1Q между коммутаторами.

 Часть 4. Настройка маршрутизации между сетями VLAN

 Часть 5. Проверка, что маршрутизация между VLAN работает

# Инструкции

# Часть 1. Создание сети и настройка основных параметров устройства

В первой части лабораторной работы вам предстоит создать топологию сети и настроить базовые параметры для узлов ПК и коммутаторов.

# Шаг 1. Создайте сеть согласно топологии.

Подключите устройства, как показано в топологии, и подсоедините необходимые кабели.

# Шаг 2. Настройте базовые параметры для маршрутизатора.

a.	Подключитесь к маршрутизатору с помощью консоли и активируйте привилегированный режим EXEC.

Откройте окно конфигурации

b.	Войдите в режим конфигурации.

c.	Назначьте маршрутизатору имя устройства.

d.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

e.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

f.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

g.	Установите cisco в качестве пароля виртуального терминала и активируйте вход.

h.	Зашифруйте открытые пароли.

i.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

j.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

k.	Настройте на маршрутизаторе время.

Закройте окно настройки.

```Router(config)#hostname R1
R1(config)#no ip domain loo
R1(config)#no ip domain lookup 
R1(config)#en
R1(config)#ena
R1(config)#enable se
R1(config)#enable secret class
R1(config)#line con
R1(config)#line console 0
R1(config-line)#pas
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#line vty 0 15
R1(config-line)#pass
R1(config-line)#password cis
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
R1(config)#copy runn
R1(config)#copy run
R1(config)#copy runn
R1(config)#copy ?
% Unrecognized command
R1(config)#end
R1#
%SYS-5-CONFIG_I: Configured from console by console

R1#copy runn
R1#copy running-config st
R1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
R1#clock set 13:59:00 mar 02 2024
R1#show cl
R1#show clo
R1#show clock 
13:59:22.296 UTC Sat Mar 2 2024
R1#
```

# Шаг 3. Настройте базовые параметры каждого коммутатора.

a.	Присвойте коммутатору имя устройства.

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e.	Установите cisco в качестве пароля виртуального терминала и активируйте вход.

f.	Зашифруйте открытые пароли.

g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h.	Настройте на коммутаторах время.

i.	Сохранение текущей конфигурации в качестве начальной.

Закройте окно настройки.

```S1>en 
S1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#no domain loo
S1(config)#no domain look
S1(config)#no ip domain loo
S1(config)#no ip domain lookup 
S1(config)#ena
S1(config)#enable se
S1(config)#enable secret class
S1(config)#line cinsole
S1(config)#line con
S1(config)#line console 0
S1(config-line)#pass
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#line vty 0 15
S1(config-line)#pass
S1(config-line)#password cis
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#end
S1#
%SYS-5-CONFIG_I: Configured from console by console

S1#clock set 14:08:00 mar 02 2024
S1#
```
```S2>en
S2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S2(config)#no ip doma
S2(config)#no ip domain loo
S2(config)#no ip domain lookup 
S2(config)#ena
S2(config)#enable se
S2(config)#enable secret class
S2(config)#line con
S2(config)#line console 0
S2(config-line)#pass
S2(config-line)#password cis
S2(config-line)#password cisco
S2(config-line)#login
S2(config-line)#line vty 0 15
S2(config-line)#pass
S2(config-line)#password cisco
S2(config-line)#login
S2(config-line)#end
S2#
%SYS-5-CONFIG_I: Configured from console by console

S2#clock set 14:11:00 mar 02 2024
S2#
```


# Шаг 4. Настройте узлы ПК.

Адреса ПК можно посмотреть в таблице адресации.

# Часть 2. Создание сетей VLAN и назначение портов коммутатора

Во второй части вы создадите VLAN, как указано в таблице выше, на обоих коммутаторах. Затем вы назначите VLAN соответствующему интерфейсу и проверите настройки конфигурации. Выполните следующие задачи на каждом коммутаторе.

# Шаг 1. Создайте сети VLAN на коммутаторах.

a.	Создайте и назовите необходимые VLAN на каждом коммутаторе из таблицы выше.

Откройте окно конфигурации

```S1(config)#vlan 10
S1(config-vlan)#name Uprava
S1(config-vlan)#vlan 20
S1(config-vlan)#name Sales
S1(config)#vlan 30
S1(config-vlan)#name Operations
S1(config-vlan)#vlan 999
S1(config-vlan)#name Parking_Lot
S1(config-vlan)#vlan 1000
S1(config-vlan)#name my_vlan
S1(config-vlan)#
```

b.	Настройте интерфейс управления и шлюз по умолчанию на каждом коммутаторе, используя информацию об IP-адресе в таблице адресации. 

```S1(config)#int vlan 10
S1(config-if)#
%LINK-5-CHANGED: Interface Vlan10, changed state to up

S1(config-if)#ip add
S1(config-if)#ip address 192.168.10.11 255.255.255.0
S1(config-if)#
```
```S2(config-vlan)#int vlan 10
S2(config-if)#
%LINK-5-CHANGED: Interface Vlan10, changed state to up

S2(config-if)#ip add
S2(config-if)#ip address 192.168.10.12 255.255.255.0
S2(config-if)#
```

c.	Назначьте все неиспользуемые порты коммутатора VLAN Parking_Lot, настройте их для статического режима доступа и административно деактивируйте их.

Примечание. Команда interface range полезна для выполнения этой задачи с минимальным количеством команд.

```S1(config)#int
S1(config)#interface ra
S1(config)#interface range f0/2-4, f0/7-24, g0/1-2
S1(config-if-range)#swi
S1(config-if-range)#switchport mode acc
S1(config-if-range)#switchport mode access 
S1(config-if-range)#swi
S1(config-if-range)#switchport acc
S1(config-if-range)#switchport access vlan 999
```
```S2(config)#int
S2(config)#interface ra
S2(config)#interface range f0/2-17, f0/19-24, g0/1-2
S2(config-if-range)#sw
S2(config-if-range)#switchport mode acc
S2(config-if-range)#switchport mode access 
S2(config-if-range)#sw
S2(config-if-range)#switchport acc
S2(config-if-range)#switchport access vlan 999
```

# Шаг 2. Назначьте сети VLAN соответствующим интерфейсам коммутатора.

a.	Назначьте используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и настройте их для режима статического доступа.

b.	Убедитесь, что VLAN назначены на правильные интерфейсы.

```S2(config-if-range)#int f0/18
S2(config-if)#swi
S2(config-if)#switchport mode acc
S2(config-if)#switchport mode access 
S2(config-if)#swi
S2(config-if)#switchport acc
S2(config-if)#switchport access vlan 30
S2(config-if)#
```
```S1(config-if-range)#int f0/6
S1(config-if)#swi
S1(config-if)#switchport mode acc
S1(config-if)#switchport mode access 
S1(config-if)#sw
S1(config-if)#switchport acc
S1(config-if)#switchport access vlan 20
S1(config-if)#
S1(config-if)#
```
```S1#show vlan brief 

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/5
10   Uprava                           active    
20   Sales                            active    Fa0/6
30   Operations                       active    
999  Parking_Lot                      active    Fa0/2, Fa0/3, Fa0/4, Fa0/7
                                                Fa0/8, Fa0/9, Fa0/10, Fa0/11
                                                Fa0/12, Fa0/13, Fa0/14, Fa0/15
                                                Fa0/16, Fa0/17, Fa0/18, Fa0/19
                                                Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                Fa0/24, Gig0/1, Gig0/2
1000 my_vlan                          active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
S1#
```
# Часть 3. Конфигурация магистрального канала стандарта 802.1Q между коммутаторами

В части 3 вы вручную настроите интерфейс F0/1 как транк.

# Шаг 1. Вручную настройте магистральный интерфейс F0/1 на коммутаторах S1 и S2.

a.	Настройка статического транкинга на интерфейсе F0/1 для обоих коммутаторов.

Откройте окно конфигурации

```S1(config-if-range)#int f0/6
S1(config-if)#swi
S1(config-if)#switchport mode acc
S1(config-if)#switchport mode access 
S1(config-if)#sw
S1(config-if)#switchport acc
S1(config-if)#switchport access vlan 20
S1(config-if)#
S1(config-if)#int f0/5
S1(config-if)#exit
S1(config)#int f0/5
S1(config-if)#
S1(config-if)#sw
S1(config-if)#switchport mode trunk
S1(config-if)#no switchport mode trunk
S1(config-if)#exit
S1(config)#int f0/1
S1(config-if)#switchport mode trunk
```

b.	Установите native VLAN 1000 на обоих коммутаторах.

```S2(config-if)#switchport trunk na
S2(config-if)#switchport trunk native vlan 1000
S2(config-if)#
```

c.	Укажите, что VLAN 10, 20, 30 и 1000 могут проходить по транку.
```
S1(config-if)#switchport trunk allowed vlan 27,37,47,1000
S2(config-if)#switchport trunk allowed vlan 27,37,47,1000
```

d.	Проверьте транки, native VLAN и разрешенные VLAN через транк.

```S1#show interfaces trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      1000

Port        Vlans allowed on trunk
Fa0/1       10,20,30,1000

Port        Vlans allowed and active in management domain
Fa0/1       10,20,30,1000

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       10,20,30,1000

S1#
```

# Шаг 2. Вручную настройте магистральный интерфейс F0/5 на коммутаторе S1.

a.	Настройте интерфейс S1 F0/5 с теми же параметрами транка, что и F0/1. Это транк до маршрутизатора.

b.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

c.	Проверка транкинга.

```S1(config)#int f0/5
S1(config-if)#sw
S1(config-if)#switchport mode tr
S1(config-if)#switchport mode trunk 
S1(config-if)#sw
S1(config-if)#switchport tr
S1(config-if)#switchport trunk na
S1(config-if)#switchport trunk native vlan 1000
```

# Часть 4. Настройка маршрутизации между сетями VLAN

# Шаг 1. Настройте маршрутизатор.

Откройте окно конфигурации

a.	При необходимости активируйте интерфейс G0/0/1 на маршрутизаторе.

b.	Настройте подинтерфейсы для каждой VLAN, как указано в таблице IP-адресации. Все подинтерфейсы используют инкапсуляцию 802.1Q. Убедитесь, что подинтерфейсу для native VLAN не назначен IP-адрес. Включите описание для каждого подинтерфейса.

c.	Убедитесь, что вспомогательные интерфейсы работают

Закройте окно настройки.

```R1#sh ip int brief
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   unassigned      YES NVRAM  administratively down down 
GigabitEthernet0/0/1   unassigned      YES NVRAM  up                    up 
GigabitEthernet0/0/1.10192.168.10.1    YES NVRAM  up                    up 
GigabitEthernet0/0/1.20192.168.20.1    YES NVRAM  up                    up 
GigabitEthernet0/0/1.30192.168.30.1    YES NVRAM  up                    up 
GigabitEthernet0/0/1.1000unassigned      YES NVRAM  up                    up 
GigabitEthernet0/0/2   unassigned      YES NVRAM  administratively down down 
Vlan1                  unassigned      YES NVRAM  administratively down down
R1#
```

# Часть 5. Проверьте, работает ли маршрутизация между VLAN

# Шаг 1. Выполните следующие тесты с PC-A. Все должно быть успешно.

Примечание. Возможно, вам придется отключить брандмауэр ПК для работы ping

a.	Отправьте эхо-запрос с PC-A на шлюз по умолчанию.

```C:\>ping 192.168.20.1

Pinging 192.168.20.1 with 32 bytes of data:

Reply from 192.168.20.1: bytes=32 time=1ms TTL=255
Reply from 192.168.20.1: bytes=32 time<1ms TTL=255
Reply from 192.168.20.1: bytes=32 time<1ms TTL=255
Reply from 192.168.20.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.20.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 1ms, Average = 0ms
```

b.	Отправьте эхо-запрос с PC-A на PC-B.

```C:\>ping 192.168.30.3

Pinging 192.168.30.3 with 32 bytes of data:

Reply from 192.168.30.3: bytes=32 time=2ms TTL=127
Reply from 192.168.30.3: bytes=32 time<1ms TTL=127
Reply from 192.168.30.3: bytes=32 time<1ms TTL=127

Ping statistics for 192.168.30.3:
    Packets: Sent = 3, Received = 3, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 2ms, Average = 0ms
```

c.	Отправьте команду ping с компьютера PC-A на коммутатор S2.

```
C:\>ping 192.168.10.11

Pinging 192.168.10.11 with 32 bytes of data:

Request timed out.
Request timed out.

Ping statistics for 192.168.10.11:
    Packets: Sent = 2, Received = 0, Lost = 2 (100% loss),
```
# Шаг 2. Пройдите следующий тест с PC-B

В окне командной строки на PC-B выполните команду tracert на адрес PC-A.

```
C:\>tracert 192.168.30.3

Tracing route to 192.168.30.3 over a maximum of 30 hops: 

  1   0 ms      0 ms      0 ms      192.168.20.1
  2   0 ms      0 ms      0 ms      192.168.30.3

Trace complete.

C:\>
```

