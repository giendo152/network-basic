# Лабораторная работа. Настройка и проверка расширенных списков контроля доступа.

# Топология

![Image alt](https://github.com/giendo152/network-basic/blob/main/practice/pra11/1.png)

# Таблица адресации

| Устройство | interface| IP-адрес	| Маска подсети |Шлюз по умолчанию |
| ---------------- |:------------------:| -----------------:| -----------------:|-----------------:|
| R1               |	G0/0/1	| - |	- |- |
| R1               |	G0/0/1.20	| 10.20.0.1 |	255.255.255.0 |- |
| R1               |	G0/0/1.30	| 10.30.0.1 |	255.255.255.0 |-|
| R1               |	G0/0/1.40	| 10.40.0.1|	255.255.255.0 |-|
| R1               |	G0/0/1.1000	| - |	- |- |
| R1               |	Loopback1	| 172.16.1.1|	255.255.255.0 |- |
| R2               |	G0/0/1	| 10.20.0.4|	255.255.255.0 |- |
| S1               |	VLAN 20	| 10.20.0.2|	255.255.255.0 |10.20.0.1 |
| S2               |	VLAN 20	| 10.20.0.3|	255.255.255.0 |10.20.0.1 |
| PC-A               |	NIC	| 10.30.0.10|	255.255.255.0 |10.30.0.1 |
| PC-B              |	NIC	| 10.40.0.10|	255.255.255.0 |10.40.0.1 |

# Таблица VLAN

| VLAN | Имя| Назначенный интерфейс	| 
| ---------------- |:------------------:| -----------------:| 
|20               |	Management	| S2: F0/5 |
|30             |	Operations	|S1: F0/6 |
| 40               |	Sales	| S2: F0/18|
| 999               |	ParkingLot	|S1: F0/2-4, F0/7-24, G0/1-2 S2: F0/2-4, F0/6-17, F0/19-24, G0/1-2 |
| 1000               |	Собственная	| - |

# Задачи

# Часть 1. Создание сети и настройка основных параметров устройства

# Часть 2. Настройка и проверка списков расширенного контроля доступа

# Инструкции

# Часть 1. Создание сети и настройка основных параметров устройства

# Шаг 1. Создайте сеть согласно топологии.

Подключите устройства, как показано в топологии, и подсоедините необходимые кабели.

# Шаг 2. Произведите базовую настройку маршрутизаторов.

Откройте окно конфигурации

a.	Назначьте маршрутизатору имя устройства.

```
router(config)# hostname R1
```

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

```
R1(config)# no ip domain lookup
```

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

```
R1(config)# enable secret class
```

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

```
R1(config)# line console 0
R1(config-line)# password cisco
R1(config-line)# login
```

e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

```
R1(config)# line vty 0 4
R1(config-line)# password cisco
R1(config-line)# login
```

f.	Зашифруйте открытые пароли.

```
R1(config)# service password-encryption
```

g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

```
R1(config)# banner motd $ Authorized Users Only! $
```

h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

```
R1# copy running-config startup-config
```

Закройте окно настройки.

# Шаг 3. Настройте базовые параметры каждого коммутатора.

Откройте окно конфигурации

a.	Присвойте коммутатору имя устройства.

```
switch(config)# hostname S1
```

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

```
S1(config)# no ip domain-lookup
```

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

```
S1(config)# enable secret class
```

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

```
S1(config)# line console 0
S1(config-line)# password cisco
S1(config-line)# login
```

e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

```
S1(config)# line vty 0 15
S1(config-line)# password cisco
S1(config-line)# login
```

f.	Зашифруйте открытые пароли.

```
S1(config)# service password-encryption
```

g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

```
S1(config)# banner motd $ Authorized Users Only! $
```

h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

```
S1# copy running-config startup-config
```

Закройте окно настройки.

# Часть 2. Настройка сетей VLAN на коммутаторах.

# Шаг 1. Создайте сети VLAN на коммутаторах.

Откройте окно конфигурации

a.	Создайте необходимые VLAN и назовите их на каждом коммутаторе из приведенной выше таблицы.

```
S1(config)# vlan 20
S1(config-vlan)# name Management
S1(config-vlan)# vlan 30
S1(config-vlan)# name Operations
S1(config-vlan)# vlan 40
S1(config-vlan)# name Sales
S1(config-vlan)# vlan 999
S1(config-vlan)# name ParkingLot
S1(config-vlan)# vlan 1000
S1(config-vlan)# name Native
S1(config-vlan)# exit

S2(config)# vlan 20
S2(config-vlan)# name Management
S2(config-vlan)# vlan 30
S2(config-vlan)# name Operations
S2(config-vlan)# vlan 40
S2(config-vlan)# name Sales
S2(config-vlan)# vlan 999
S2(config-vlan)# name ParkingLot
S2(config-vlan)# vlan 1000
S2(config-vlan)# name Native
S2(config-vlan)# exit
```
b.	Настройте интерфейс управления и шлюз по умолчанию на каждом коммутаторе, используя информацию об IP-адресе в таблице адресации. 

```
S1(config)# interface vlan 20
S1(config-if)# ip address 10.20.0.2 255.255.255.0
S1(config-if)# no shutdown
S1(config-if)# exit
S1(config)# ip default-gateway 10.20.0.1
S1(config)# end

S2(config)# interface vlan 20
S2(config-if)# ip address 10.20.0.3 255.255.255.0
S2(config-if)# no shutdown
S2(config-if)# exit
S2(config)# ip default-gateway 10.20.0.1
S2(config)# end
```

c.	Назначьте все неиспользуемые порты коммутатора VLAN Parking Lot, настройте их для статического режима доступа и административно деактивируйте их.

```
S1(config)# interface range f0/2 - 4, f0/7 - 24, g0/1 - 2
S1(config-if-range)# switchport mode access
S1(config-if-range)# switchport access vlan 999
S1(config-if-range)# shutdown
S1(config-if-range)# end

S2(config)# interface range f0/2 - 17, f0/19 - 24, g0/1 - 2
S2(config-if-range)# switchport mode access
S2(config-if-range)# switchport access vlan 999
S2(config-if-range)# shutdown
S2(config-if-range)# end
```

Примечание. Команда interface range полезна для выполнения этой задачи с помощью необходимого количества команд. 

# Шаг 2. Назначьте сети VLAN соответствующим интерфейсам коммутатора.

a.	Назначьте используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и настройте их для режима статического доступа.

```
S1(config)# interface f0/6
S1(config-if)# switchport mode access
S1(config-if)# switchport access vlan 30

S2(config)# interface f0/18
S2(config-if)# switchport mode access
S2(config-if)# switchport access vlan 40
```

b.	Выполните команду show vlan brief, чтобы убедиться, что сети VLAN назначены правильным интерфейсам.

```
S1# show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/5
10   Management                       active
20   Sales                            active    Fa0/6
30   Operations                       active
999  ParkingLot                       active    Fa0/2, Fa0/3, Fa0/4, Fa0/7
                                                Fa0/8, Fa0/9, Fa0/10, Fa0/11
                                                Fa0/12, Fa0/13, Fa0/14, Fa0/15
                                                Fa0/16, Fa0/17, Fa0/18, Fa0/19
                                                Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                Fa0/24, Gi0/1, Gi0/2
1000 Native                           active
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup
S2# show vlab brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1
10   Management                       active
30   Operations                       active
40   Sales                            active    Fa0/18
999  ParkingLot                       active    Fa0/2, Fa0/3, Fa0/4, Fa0/5
                                                Fa0/6, Fa0/7, Fa0/8, Fa0/9
                                                Fa0/10, Fa0/11, Fa0/12, Fa0/13
                                                Fa0/14, Fa0/15, Fa0/16, Fa0/17
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23, Fa0/24, Gi0/1, Gi0/2
```

Закройте окно настройки.

# Часть 3. ·Настройте транки (магистральные каналы).

# Шаг 1. Вручную настройте магистральный интерфейс F0/1.

Откройте окно конфигурации

a.	Измените режим порта коммутатора на интерфейсе F0/1, чтобы принудительно создать магистральную связь. Не забудьте сделать это на обоих коммутаторах.

```
S1(config)# interface f0/1
S1(config-if)# switchport mode trunk

S2(config)# interface f0/1
S2(config-if)# switchport mode trunk
```

b.	В рамках конфигурации транка установите для native vlan значение 1000 на обоих коммутаторах. При настройке двух интерфейсов для разных собственных VLAN сообщения об ошибках могут отображаться временно.

```
S1(config-if)# switchport trunk native vlan 1000

S2(config-if)# switchport trunk native vlan 1000
```

c.	В качестве другой части конфигурации транка укажите, что VLAN 10, 20, 30 и 1000 разрешены в транке.

```
S1(config-if)# switchport trunk allowed vlan 20,30,40,1000

S2(config-if)# switchport trunk allowed vlan 20,30,40,1000
```

d.	Выполните команду show interfaces trunk для проверки портов магистрали, собственной VLAN и разрешенных VLAN через магистраль.

```
S1# show interfaces trunk

Port        Mode             Encapsulation  Status        Native vlan
Fa0/1       on               802.1q         trunking      1000

Port        Vlans allowed on trunk
Fa0/1       20,30,40,1000

Port        Vlans allowed and active in management domain
Fa0/1       20,30,40,1000

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       20,30,40,1000

S2# show interface trunk

Port        Mode             Encapsulation  Status        Native vlan
Fa0/1       on               802.1q         trunking      1000

Port        Vlans allowed on trunk
Fa0/1       20,30,40,1000

Port        Vlans allowed and active in management domain
Fa0/1       30,40,1000

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       30,40
```

# Шаг 2. Вручную настройте магистральный интерфейс F0/5 на коммутаторе S1.

a.	Настройте интерфейс S1 F0/5 с теми же параметрами транка, что и F0/1. Это транк до маршрутизатора.

```
S1(config)# interface f0/5
S1(config-if)# switchport mode trunk
S1(config-if)# switchport trunk native vlan 1000
S1(config-if)# switchport trunk allowed vlan 20,30,40,1000
```

b.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

```
S1# copy running-config startup-config
```

c.	Используйте команду show interfaces trunk для проверки настроек транка.

Закройте окно настройки.

# Часть 4. Настройте маршрутизацию.

# Шаг 1. Настройка маршрутизации между сетями VLAN на R1.

Откройте окно конфигурации

a.	Активируйте интерфейс G0/0/1 на маршрутизаторе.

```
R1(config)# interface g0/0/1
R1(config-if)# no shutdown
```

b.	Настройте подинтерфейсы для каждой VLAN, как указано в таблице IP-адресации. Все подинтерфейсы используют инкапсуляцию 802.1Q. Убедитесь, что подинтерфейс для собственной VLAN не имеет назначенного IP-адреса. Включите описание для каждого подинтерфейса.

```
R1(config)# interface g0/0/1.20
R1(config-subif)# description Management Network
R1(config-subif)# encapsulation dot1q 20
R1(config-subif)# ip address 10.20.0.1 255.255.255.0
R1(config-subif)# interface g0/0/1.30
R1(config-subif)# encapsulation dot1q 30
R1(config-subif)# description Operations Network
R1(config-subif)# ip address 10.30.0.1 255.255.255.0
R1(config-subif)# interface g0/0/1.40
R1(config-subif)# encapsulation dot1q 40
R1(config-subif)# description Sales Network
R1(config-subif)# ip address 10.40.0.1 255.255.255.0
R1(config-subif)# interface g0/0/1.1000
R1(config-subif)# encapsulation dot1q 1000 native
R1(config-subif)# description Native VLAN
```

c.	Настройте интерфейс Loopback 1 на R1 с адресацией из приведенной выше таблицы.

```
R1(config)# interface Loopback 1
R1(config-if)# ip address 172.16.1.1 255.255.255.0
```

d.	С помощью команды show ip interface brief проверьте конфигурацию подынтерфейса.

```
R1# show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0/0   unassigned      YES unset  administratively down down
GigabitEthernet0/0/1   unassigned      YES unset  up                    up
Gi0/0/1.20             10.20.0.1       YES manual up                    up
Gi0/0/1.30             10.30.0.1       YES manual up                    up
Gi0/0/1.40             10.40.0.1       YES manual up                    up
Gi0/0/1.1000           unassigned      YES unset  up                    up
Serial0/1/0            unassigned      NO  unset  down                  down
Serial0/1/1            unassigned      NO  unset  down                  down
GigabitEthernet0       unassigned      YES unset  administratively down down
Loopback1              172.16.1.1      YES manual up                    up
```

# Шаг 2. Настройка интерфейса R2 g0/0/1 с использованием адреса из таблицы и маршрута по умолчанию с адресом следующего перехода 10.20.0.1

```
R2(config)# interface g0/0/1
R2(config-if)# ip address 10.20.0.4 255.255.255.0
R2(config-if)# no shutdown
R2(config-if)# exit
R2(config)# ip route 0.0.0.0 0.0.0.0 10.20.0.1
```

Закройте окно настройки.

# Часть 5. Настройте удаленный доступ

# Шаг 1. Настройте все сетевые устройства для базовой поддержки SSH.

Откройте окно конфигурации

a.	Создайте локального пользователя с именем пользователя SSHadmin и зашифрованным паролем $cisco123!

```
R1(config)# username SSHadmin secret $cisco123!
```

b.	Используйте ccna-lab.com в качестве доменного имени.

```
R1(config)# ip domain name ccna-lab.com
```

c.	Генерируйте криптоключи с помощью 1024 битного модуля.

```
R1(config)# crypto key generate rsa general-keys modulus 1024
```

d.	Настройте первые пять линий VTY на каждом устройстве, чтобы поддерживать только SSH-соединения и с локальной аутентификацией.

```
R1(config)# line vty 0 4
R1(config-line)# transport input ssh
R1(config-line)# login local
R1(config-line)# exit

# Шаг 2. Включите защищенные веб-службы с проверкой подлинности на R1.

a.	Включите сервер HTTPS на R1.

R1(config)# ip http secure-server 

b.	Настройте R1 для проверки подлинности пользователей, пытающихся подключиться к веб-серверу.

R1(config)# ip http authentication local

# Часть 6. Проверка подключения

# Шаг 1. Настройте узлы ПК.

Адреса ПК можно посмотреть в таблице адресации.

# Шаг 2. Выполните следующие тесты. Эхозапрос должен пройти успешно.

| От | Протокол| Назначение	| 
| ---------------- |:------------------:| -----------------:| 
| PC-A           |	Ping | 10.40.0.10 |
| PC-A           |	Ping | 10.20.0.1 |
| PC-B           |	Ping | 10.30.0.10 |
| PC-B            |	Ping | 10.20.0.1 |
| PC-B            |	Ping | 172.16.1.1 |
| PC-B            |	HTTPS | 10.20.0.1 |
| PC-B            |	HTTPS | 172.16.1.1 |
| PC-B            |	SSH | 10.20.0.1 |
| PC-B            |	SSH | 172.16.1.1 |

# Часть 7. Настройка и проверка списков контроля доступа (ACL)

При проверке базового подключения компания требует реализации следующих политик безопасности:

Политика1. Сеть Sales не может использовать SSH в сети Management (но в  другие сети SSH разрешен). 

Политика 2. Сеть Sales не имеет доступа к IP-адресам в сети Management с помощью любого веб-протокола (HTTP/HTTPS). Сеть Sales также не имеет доступа к интерфейсам R1 с помощью любого веб-протокола. Разрешён весь другой веб-трафик (обратите внимание — Сеть Sales  может получить доступ к интерфейсу Loopback 1 на R1).

Политика3. Сеть Sales не может отправлять эхо-запросы ICMP в сети Operations или Management. Разрешены эхо-запросы ICMP к другим адресатам. 

Политика 4: Cеть Operations  не может отправлять ICMP эхозапросы в сеть Sales. Разрешены эхо-запросы ICMP к другим адресатам. 

# Шаг 1. Проанализируйте требования к сети и политике безопасности для планирования реализации ACL.

Ответ:

Ответы могут отличаться. Перечисленные выше требования требуют реализации двух списков расширенного доступа. Следуя рекомендациям по размещению списков расширенного доступа как можно ближе к источнику фильтруемого трафика, эти списки управления доступом будут доступны на интерфейсах G0 / 0 / 0.30 и G0 / 0 / 0.40

# Шаг 2. Разработка и применение расширенных списков доступа, которые будут соответствовать требованиям политики безопасности.

Откройте окно конфигурации

```
R1(config)# access-list 101 remark ACL 101 fulfills policies 1, 2, and 3
R1(config)# access-list 101 deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 22
R1(config)# access-list 101 deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 80
R1(config)# access-list 101 deny tcp 10.40.0.0 0.0.0.255 10.30.0.1 0.0.0.0 eq 80
R1(config)# access-list 101 deny tcp 10.40.0.0 0.0.0.255 10.40.0.1 0.0.0.0 eq 80
R1(config)# access-list 101 deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 443
R1(config)# access-list 101 deny tcp 10.40.0.0 0.0.0.255 10.30.0.1 0.0.0.0 eq 443
R1(config)# access-list 101 deny tcp 10.40.0.0 0.0.0.255 10.40.0.1 0.0.0.0 eq 443
R1(config)# access-list 101 deny icmp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 echo
R1(config)# access-list 101 deny icmp 10.40.0.0 0.0.0.255 10.30.0.0 0.0.0.255 echo
R1(config)# access-list 101 permit ip any any
R1(config)# interface g0/0/1.40
R1(config-subif)# ip access-group 101 in

R1(config)# access-list 102 remark ACL 102 fulfills policy 4
R1(config)# access-list 102 deny icmp 10.30.0.0 0.0.0.255 10.40.0.0 0.0.0.255 echo
R1(config)# access-list 102 permit ip any any
R1(config)# interface g0/0/1.30
R1(config-subif)# ip access-group 102 in
```

Закройте окно настройки.

# Шаг 3. Убедитесь, что политики безопасности применяются развернутыми списками доступа.

Выполните следующие тесты. Ожидаемые результаты показаны в таблице:

| От | Протокол| Назначение	| Результат	| 
| ---------------- |:------------------:| -----------------:| -----------------:| 
| PC-A           |	Ping | 10.40.0.10 |Сбой| 
| PC-A           |	Ping | 10.20.0.1 |Успех|
| PC-B           |	Ping | 10.30.0.10 |Сбой|
| PC-B            |	Ping | 10.20.0.1 |Сбой|
| PC-B            |	Ping | 172.16.1.1 |Успех|
| PC-B            |	HTTPS | 10.20.0.1 |Сбой|
| PC-B            |	HTTPS | 172.16.1.1 |Успех|
| PC-B            |	SSH | 10.20.0.1 |Сбой|
| PC-B            |	SSH | 172.16.1.1 |Успех|
