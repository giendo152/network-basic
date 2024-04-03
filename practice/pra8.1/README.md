# Лабораторная работа - Реализация DHCPv4 

# Топология

![Image alt](https://github.com/giendo152/network-basic/blob/main/practice/pra8.1/1.png)

# 1.	Таблица адресации

| Устройство | Интерфейс	| IP-адрес	| Маска подсети | Шлюз по умолчанию |
| ---------------- |:------------------:| -----------------:|--------------:| --------------:|
| R1               |	G0/0/0	| 10.0.0.1 |	255.255.255.252 |	N/A |
| R1               |	G0/0/1	| N/A      |	N/A |	N/A |
| R1               |	G0/0/1.100	|  192.168.1.1 |	255.255.255.192  |	N/A |
| R1               |	G0/0/1.200	| 192.168.1.65  |	255.255.255.224  |	N/A |
| R1               |	G0/0/1.1000	| N/A |	N/A |	N/A |
| R2               |	G0/0/0	| 10.0.0.2 |	255.255.255.252 |	N/A |
| R2               |	G0/0/1	| 192.168.1.97  |	255.255.255.240  |	N/A |
| S1               |	VLAN 200	| 192.168.1.66  |255.255.255.224	  |192.168.1.65  |
| S2              |	VLAN 1	|  192.168.1.98 |255.255.255.240	  |192.168.1.97  |
| PC-A            |	NIC	| DHCP |	DHCP |	DHCP |
| PC-B               |	NIC	| DHCP |	DHCP |	DHCP |

# 2.	Таблица VLAN

|VLAN | Имя	| Назначенный интерфейс	|
| ---------------- |:------------------:| -----------------:|
| 1              |	Нет| S2: F0/18 |
|100             |	Клиенты	| S1: F0/6  |
|200               |	Управление	| S1: VLAN 200   |
| 999      |	Parking_Lot	| S1: F0/1-4, F0/7-24, G0/1-2 |
| 1000              |	Собственная	| N/A |

# 3.	Задачи

# Часть 1. Создание сети и настройка основных параметров устройства

# Часть 2. Настройка и проверка двух серверов DHCPv4 на R1

# Часть 3. Настройка и проверка DHCP-ретрансляции на R2

# Часть 1.	Создание сети и настройка основных параметров устройства

В первой части лабораторной работы вам предстоит создать топологию сети и настроить базовые параметры для узлов ПК и коммутаторов

# Шаг 1.	Создание схемы адресации

Подсеть сети 192.168.1.0/24 в соответствии со следующими требованиями:

a.	Одна подсеть «Подсеть A», поддерживающая 58 хостов (клиентская VLAN на R1).

Подсеть A:

192.168.1.0/26 (.1 -.63)

Запишите первый IP-адрес в таблице адресации для R1 G0/0/1.100 . 



b.	Одна подсеть «Подсеть B», поддерживающая 28 хостов (управляющая VLAN на R1). 

Подсеть B:


192.168.1.64/27 (.65-.95)

Запишите первый IP-адрес в таблице адресации для R1 G0/0/1.200. Запишите второй IP-адрес в таблице адресов для S1 VLAN 200 и введите соответствующий шлюз по умолчанию.

c.	Одна подсеть «Подсеть C», поддерживающая 12 узлов (клиентская сеть на R2).

Подсеть C:

192.168.1.96/28 (.97-.111)

Запишите первый IP-адрес в таблице адресации для R2 G0/0/1.

# Шаг 2.	Создайте сеть согласно топологии.

Подключите устройства, как показано в топологии, и подсоедините необходимые кабели.

# Шаг 3.	Произведите базовую настройку маршрутизаторов.

a.	Назначьте маршрутизатору имя устройства.

Откройте окно конфигурации

router(config)# hostname R1

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f.	Зашифруйте открытые пароли.

g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

i.	Установите часы на маршрутизаторе на сегодняшнее время и дату.

Примечание. Вопросительный знак (?) позволяет открыть справку с правильной последовательностью параметров, необходимых для выполнения этой команды.

```
R1(config)#no ip domain-lookup
R1(config)#enable secret class
R1(config)#line console 0
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#line vty 0 4
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#service password-encryption
R1(config)#banner motd $ Authorized Users Only! $
R1(config)#end
R1#
%SYS-5-CONFIG_I: Configured from console by console

R1#copy running-config startup-config
Destination filename [startup-config]? 
Building configuration...
[OK]
R1#clock set 21:33:00 03 Apr 2024
R1#
```

# Шаг 4.	Настройка маршрутизации между сетями VLAN на маршрутизаторе R1

a.	Активируйте интерфейс G0/0/1 на маршрутизаторе.

```
R1(config)# interface g0/0/1
R1(config-if)# no shutdown
R1(config-if)# exit
```

b.	Настройте подинтерфейсы для каждой VLAN в соответствии с требованиями таблицы IP-адресации. Все субинтерфейсы используют инкапсуляцию 802.1Q и назначаются первый полезный адрес из вычисленного пула IP-адресов. 
Убедитесь, что подинтерфейсу для native VLAN не назначен IP-адрес. Включите описание для каждого подинтерфейса.

```
R1(config)#interface g0/0/1.100
R1(config-subif)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1.100, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.100, changed state to up

R1(config-subif)#description Client Network
R1(config-subif)#encapsulation dot1q 100
R1(config-subif)#ip address 192.168.1.1 255.255.255.192
R1(config-subif)#interface g0/0/1.200
R1(config-subif)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1.200, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.200, changed state to up

R1(config-subif)#encapsulation dot1q 200
R1(config-subif)#description Management Network
R1(config-subif)#ip address 192.168.1.65 255.255.255.224
R1(config-subif)#interface g0/0/1.1000
R1(config-subif)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1.1000, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.1000, changed state to up

R1(config-subif)#encapsulation dot1q 1000 native
R1(config-subif)#description Native VLAN
R1(config-subif)#
```

c.	Убедитесь, что вспомогательные интерфейсы работают.

```
R1#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   unassigned      YES unset  administratively down down 
GigabitEthernet0/0/1   unassigned      YES unset  up                    up 
GigabitEthernet0/0/1.100192.168.1.1     YES manual up                    up 
GigabitEthernet0/0/1.200192.168.1.65    YES manual up                    up 
GigabitEthernet0/0/1.1000unassigned      YES unset  up                    up 
Vlan1                  unassigned      YES unset  administratively down down
R1#
```

# Шаг 5.	Настройте G0/1 на R2, затем G0/0/0 и статическую маршрутизацию для обоих маршрутизаторов

a.	Настройте G0/0/1 на R2 с первым IP-адресом подсети C, рассчитанным ранее.

```
R2(config)#interface g0/0/1
R2(config-if)#ip address 192.168.1.97 255.255.255.240
R2(config-if)#no shutdown

R2(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

R2(config-if)#exit
R2(config)#
```

b.	Настройте интерфейс G0/0/0 для каждого маршрутизатора на основе приведенной выше таблицы IP-адресации.

```
R2(config)#interface g0/0/0
R2(config-if)#ip address 10.0.0.1 255.255.255.252
R2(config-if)#no shutdown

R2(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/0, changed state to up

R2(config-if)#interface g0/0/0
R2(config-if)#ip address 10.0.0.2 255.255.255.252
R2(config-if)#no shutdown
R2(config-if)#
```

c.	Настройте маршрут по умолчанию на каждом маршрутизаторе, указываемом на IP-адрес G0/0/0 на другом маршрутизаторе.

```
R1(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.2
R2(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.1
```

d.	Убедитесь, что статическая маршрутизация работает с помощью пинга до адреса G0/0/1 R2 от R1.

```
R1#ping 192.168.1.97

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.97, timeout is 2 seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max = 0/3/15 ms

```

e.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

R1# copy running-config startup-config

# Шаг 6.	Настройте базовые параметры каждого коммутатора.

a.	Присвойте коммутатору имя устройства.

Откройте окно конфигурации

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f.	Зашифруйте открытые пароли.

g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

i.	Установите часы на маршрутизаторе на сегодняшнее время и дату.

Примечание. Вопросительный знак (?) позволяет открыть справку с правильной последовательностью параметров, необходимых для выполнения этой команды.

j.	Скопируйте текущую конфигурацию в файл загрузочной конфигурации.

```
S1(config)#no ip domain-lookup
S1(config)#enable secret class
S1(config)#line console 0
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#line vty 0 4
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#service password-encryption
S1(config)#banner motd $ Authorized Users Only! $
S1(config)#exit
S1#
%SYS-5-CONFIG_I: Configured from console by console

S1#copy running-config startup-config
Destination filename [startup-config]? 
Building configuration...
[OK]
S1#clock set 21:33:00 03 Apr 2024
S1#
```

# Шаг 7.	Создайте сети VLAN на коммутаторе S1.

Примечание. S2 настроен только с базовыми настройками. 

a.	Создайте необходимые VLAN на коммутаторе 1 и присвойте им имена из приведенной выше таблицы.

```
S1(config)# vlan 100
S1(config-vlan)# name Clients
S1(config-vlan)# vlan 200
S1(config-vlan)# name Management
S1(config-vlan)# vlan 999
S1(config-vlan)# name Parking_Lot
S1(config-vlan)# vlan 1000
S1(config-vlan)# name Native
S1(config-vlan)# exit
```

b.	Настройте и активируйте интерфейс управления на S1 (VLAN 200), используя второй IP-адрес из подсети, рассчитанный ранее. Кроме того установите шлюз по умолчанию на S1.

```
S1(config)# interface vlan 200
S1(config-if)# ip address 192.168.1.66 255.255.255.224
S1(config-if)# no shutdown
S1(config-if)# exit
S1(config)# ip default-gateway 192.168.1.65
```

c.	Настройте и активируйте интерфейс управления на S2 (VLAN 1), используя второй IP-адрес из подсети, рассчитанный ранее. Кроме того, установите шлюз по умолчанию на S2

```
S2(config)# interface vlan 1
S2(config-if)# ip address 192.168.1.98 255.255.255.240
S2(config-if)# no shutdown
S2(config-if)# exit
S2(config)# ip default-gateway 192.168.1.97
```

d.	Назначьте все неиспользуемые порты S1 VLAN Parking_Lot, настройте их для статического режима доступа и административно деактивируйте их. На S2 административно деактивируйте все неиспользуемые порты.

Примечание. Команда interface range полезна для выполнения этой задачи с минимальным количеством команд.

Закройте окно настройки.

```
S1(config)# interface range f0/1 - 4, f0/7 - 24, g0/1 - 2
S1(config-if-range)# switchport mode access
S1(config-if-range)# switchport access vlan 999
S1(config-if-range)# shutdown
S1(config-if-range)# exit

S2(config)# interface range f0/1 - 4, f0/6 - 17, f0/19 - 24, g0/1 - 2
S2(config-if-range)# switchport mode access
S2(config-if-range)# shutdown
S2(config-if-range)# exit
```

# Шаг 8.	Назначьте сети VLAN соответствующим интерфейсам коммутатора.

a.	Назначьте используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и настройте их для режима статического доступа.

Откройте окно конфигурации

```
S1(config)# interface f0/6
S1(config-if)# switchport mode access
S1(config-if)# switchport access vlan 100
```
b.	Убедитесь, что VLAN назначены на правильные интерфейсы.

```
S1#show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/5
100  Clients                          active    Fa0/6
200  Management                       active    
999  Parking_Lot                      active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/7, Fa0/8, Fa0/9, Fa0/10
                                                Fa0/11, Fa0/12, Fa0/13, Fa0/14
                                                Fa0/15, Fa0/16, Fa0/17, Fa0/18
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23, Fa0/24, Gig0/1, Gig0/2
1000 Native                           active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
S1#
```
Вопрос:

Почему интерфейс F0/5 указан в VLAN 1?

Ответ:

Порт 5 находится в VLAN по умолчанию и не был настроен как магистраль 802.1Q.

# Шаг 9.	Вручную настройте интерфейс S1 F0/5 в качестве транка 802.1Q.

a.	Измените режим порта коммутатора, чтобы принудительно создать магистральный канал.

```
S1(config)#interface f0/5
S1(config-if)#switchport mode trunk

S1(config-if)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/5, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/5, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan200, changed state to up

S1(config-if)#
```

b.	В рамках конфигурации транка  установите для native  VLAN значение 1000.

c.	В качестве другой части конфигурации магистрали укажите, что VLAN 100, 200 и 1000 могут проходить по транку.

d.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

e.	Проверьте состояние транка.

```
S1(config-if)#switchport trunk native vlan 1000
S1(config-if)#switchport trunk allowed vlan 100,200,1000
S1(config-if)#exit
S1(config)#end
S1#
%SYS-5-CONFIG_I: Configured from console by console

S1#copy running-config startup-config
Destination filename [startup-config]? 
Building configuration...
[OK]
S1#show interfaces trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/5       on           802.1q         trunking      1000

Port        Vlans allowed on trunk
Fa0/5       100,200,1000

Port        Vlans allowed and active in management domain
Fa0/5       100,200,1000

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/5       100,200,1000

S1#
```

Вопрос:

Какой IP-адрес был бы у ПК, если бы он был подключен к сети с помощью DHCP?

Ответ: 

Они будут самостоятельно настраиваться с использованием автоматического частного IP-адреса (APIPA) в диапазоне 169.254.x.x.

# Часть 2.	Настройка и проверка двух серверов DHCPv4 на R1

В части 2 необходимо настроить и проверить сервер DHCPv4 на R1. Сервер DHCPv4 будет обслуживать две подсети, подсеть 

A и подсеть C.

# Шаг 1.	Настройте R1 с пулами DHCPv4 для двух поддерживаемых подсетей. Ниже приведен только пул DHCP для подсети A

a.	Исключите первые пять используемых адресов из каждого пула адресов.

Откройте окно конфигурации

b.	Создайте пул DHCP (используйте уникальное имя для каждого пула).

c.	Укажите сеть, поддерживающую этот DHCP-сервер.

d.	В качестве имени домена укажите CCNA-lab.com.

e.	Настройте соответствующий шлюз по умолчанию для каждого пула DHCP.

f.	Настройте время аренды на 2 дня 12 часов и 30 минут.

g.	Затем настройте второй пул DHCPv4, используя имя пула R2_Client_LAN и вычислите сеть, маршрутизатор по умолчанию, и используйте то же имя домена и время аренды, что и предыдущий пул DHCP.

```
R1(config)#ip dhcp excluded-address 192.168.1.1 192.168.1.5
R1(config)#ip dhcp pool R1_Client_LAN
R1(dhcp-config)#network 192.168.1.0 255.255.255.192
R1(dhcp-config)#domain-name ccna-lab.com
R1(dhcp-config)#default-router 192.168.1.1
R1(dhcp-config)#lease 2 12 30
                ^
% Invalid input detected at '^' marker.
	
R1(dhcp-config)#le
R1(dhcp-config)#lea
R1(dhcp-config)#leas
R1(dhcp-config)#lease
R1(dhcp-config)#lease 2 12 30
                ^
% Invalid input detected at '^' marker.
	
R1(dhcp-config)#lease 3 12 0
                ^
% Invalid input detected at '^' marker.
	
R1(dhcp-config)#exit
R1(config)#ip dhcp excluded-address 192.168.1.97 192.168.1.101
R1(config)#ip dhcp pool R2_Client_LAN
R1(dhcp-config)#network 192.168.1.96 255.255.255.240
R1(dhcp-config)#default-router 192.168.1.97
R1(dhcp-config)#domain-name ccna-lab.com
R1(dhcp-config)#end
R1#
%SYS-5-CONFIG_I: Configured from console by console

R1#copy running-config startup-config
Destination filename [startup-config]? 
Building configuration...
[OK]
R1#
R1#
```

P.S. По поводу назначения времени аренды, на CPT не получилось сделать, в сети прочитал что на данной программе это невозможно сделать.

# Шаг 2.	Сохраните конфигурацию.

Сохраните текущую конфигурацию в файл загрузочной конфигурации.

Закройте окно настройки.

# Шаг 3.	Проверка конфигурации сервера DHCPv4

a.	Чтобы просмотреть сведения о пуле, выполните команду show ip dhcp pool .

b.	Выполните команду show ip dhcp bindings для проверки установленных назначений адресов DHCP.

c.	Выполните команду show ip dhcp server statistics для проверки сообщений DHCP.

# Шаг 4.	Попытка получить IP-адрес от DHCP на PC-A

a.	Из командной строки компьютера PC-A выполните команду ipconfig /all.

b.	После завершения процесса обновления выполните команду ipconfig для просмотра новой информации об IP-адресе.

c.	Проверьте подключение с помощью пинга IP-адреса интерфейса R0 G0/0/1.


```
C:\>ping 192.168.1.1

Pinging 192.168.1.1 with 32 bytes of data:

Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time=3ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.1.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 3ms, Average = 0ms

C:\>
```

# Часть 3.	Настройка и проверка DHCP-ретрансляции на R2

В части 3 настраивается R2 для ретрансляции DHCP-запросов из локальной сети на интерфейсе G0/0/1 на DHCP-сервер (R1). 

# Шаг 1.	Настройка R2 в качестве агента DHCP-ретрансляции для локальной сети на G0/0/1

a.	Настройте команду ip helper-address на G0/0/1, указав IP-адрес G0/0/0 R1.

Откройте окно конфигурации

b.	Сохраните конфигурацию.

# Шаг 2.	Попытка получить IP-адрес от DHCP на PC-B

a.	Из командной строки компьютера PC-B выполните команду ipconfig /all.

b.	После завершения процесса обновления выполните команду ipconfig для просмотра новой информации об IP-адресе.

c.	Проверьте подключение с помощью пинга IP-адреса интерфейса R1 G0/0/1.

```
C:\>ping 192.168.1.1

Pinging 192.168.1.1 with 32 bytes of data:

Reply from 192.168.1.1: bytes=32 time<1ms TTL=254
Reply from 192.168.1.1: bytes=32 time<1ms TTL=254
Reply from 192.168.1.1: bytes=32 time<1ms TTL=254
Reply from 192.168.1.1: bytes=32 time<1ms TTL=254

Ping statistics for 192.168.1.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>
```


d.	Выполните show ip dhcp binding для R1 для проверки назначений адресов в DHCP.

```
R1#show ip dhcp binding
IP address       Client-ID/              Lease expiration        Type
                 Hardware address
192.168.1.6      0060.70ED.9A79           --                     Automatic
192.168.1.102    0009.7CA8.05A3           --                     Automatic
R1#
```

e.	Выполните команду show ip dhcp server statistics для проверки сообщений DHCP.

