# Лабораторная работа - Настройка протоколов CDP, LLDP и NTP 

# Топология

![Image alt](https://github.com/giendo152/network-basic/blob/main/practice/pra13/1.png)

# Таблица адресации

| Устройство | interface/vlan	| IP-адрес	| Маска подсети | Шлюз по умолчанию |
| ---------------- |:------------------:| -----------------:| -----------------:|-----------------:|
| R1               |	Loopback1	| 172.16.1.1 |	255.255.255.0 |- |
| R1               |	G0/0/1 	| 10.22.0.1 |	255.255.255.0 |- |
| S1               |	SVI VLAN 1 	| 10.22.0.2 | 255.255.255.0 |	10.22.0.1|
| S2               |	SVI VLAN 1 | 10.22.0.3 |	255.255.255.0 |10.22.0.1|

# Задачи

# Часть 1. Создание сети и настройка основных параметров устройства

# Часть 2. Обнаружение сетевых ресурсов с помощью протокола CDP

# Часть 3. Обнаружение сетевых ресурсов с помощью протокола LLDP

# Часть 4. Настройка и проверка NTP

# Часть 1. Создание сети и настройка основных параметров устройства

В первой части лабораторной работы вам предстоит создать топологию сети и настроить основные параметры для маршрутизатора и коммутаторов.

# Шаг 1. Создайте сеть согласно топологии.

Подключите устройства, как показано в топологии, и подсоедините необходимые кабели.

# Шаг 2. Настройте базовые параметры для маршрутизатора.

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

h.	Настройка интерфейсов, перечисленных в таблице выше

```
R1(config-if)# interface g0/0/1
R1(config-if)# ip address 10.22.0.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# end
```

i.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

```
R1# copy running-config startup-config
```

Закройте окно настройки.

# Шаг 3. Настройте базовые параметры каждого коммутатора.

Откройте окно конфигурации

a.	Присвойте коммутатору имя устройства.

```
switch(config)# hostname S1
switch(config)# hostname S2
```

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

```
S1(config)# no ip domain-lookup
S2(config)# no ip domain-lookup
```

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

```
S1(config)# enable secret class
S2(config)# enable secret class
```

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

```
S1(config)# line console 0
S1(config-line)# password cisco
S1(config-line)# login
```

```
S2(config)# line console 0
S2(config-line)# password cisco
S2(config-line)# login
```

e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

```
S1(config)# line vty 0 15
S1(config-line)# password cisco
S1(config-line)# login
```

```
S2(config)# line vty 0 15
S2(config-line)# password cisco
S2(config-line)# login
```

f.	Зашифруйте открытые пароли.

```
S1(config)# service password-encryption
```

g.	Создайте баннер, который предупреждает всех, кто обращается к устройству, видит баннерное сообщение «Только 
авторизованные пользователи!».  

```
S1(config)# banner motd $ Authorized Users Only! $
```

h.	Отключите неиспользуемые интерфейсы

```
S1(config)# interface range f0/2-4, f0/6-24, g0/1-2
S1(config-if-range)# shutdown
S1(config-if-range)# end
```

```
S2(config)# interface range f0/2-24, g0/1-2
S2(config-if-range)# shutdown
S2(config-if-range)# end
```

i.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

```
S1# copy running-config startup-config
```

Закройте окно настройки.

# Часть 2. Обнаружение сетевых ресурсов с помощью протокола CDP

На устройствах Cisco протокол CDP включен по умолчанию. Воспользуйтесь CDP, чтобы обнаружить порты, к которым подключены кабели.

Откройте окно конфигурации

a.	На R1 используйте соответствующую команду show cdp, чтобы определить, сколько интерфейсов включено CDP, сколько из них включено и сколько отключено.

```
R1# show cdp interface | include interfaces
 cdp enabled interfaces : 5
 interfaces up          : 4
 interfaces down        : 1
```
 
Вопрос:

Сколько интерфейсов участвует в объявлениях CDP? Какие из них активны?

Ответ:

Ответы будут разными. В выводе выше пять интерфейсов участвуют в CDP. Четыре подключены, один отключен.
 
b.	На R1 используйте соответствующую команду show cdp, чтобы определить версию IOS, используемую на S1.

```
R1 # show cdp entry  S1
-------------------------
Device ID: S1
Entry address(es):
Platform: cisco WS-C2960+24LC-L, Capabilities: Switch IGMP 
Interface: GigabitEthernet0/0/1, Port ID (outgoing port): FastEthernet0/5
Holdtime : 125 sec

Version :
Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.2(4)E8, RELEASE SOFTWARE (fc3) 
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2019 by Cisco Systems, Inc.
Compiled Fri 15-Mar-19 17:28 by prod_rel_team 

advertisement version: 2
VTP Management Domain: ''
Native VLAN: 1
Duplex: full
```

Вопрос:

Какая версия IOS используется на  S1?

Ответ:

Ответы могут отличаться. S1 в этом примере использует IOS версии 15.2 (4) E8
 
c.	На S1 используйте соответствующую команду show cdp, чтобы определить, сколько пакетов CDP было выданных.

```
S1# show cdp traffic
CDP counters : 
        Total packets output: 179, Input: 148 
        Hdr syntax: 0, Chksum error: 0, Encaps failed: 0 
        No memory: 0, Invalid packet: 0, 
        CDP version 1 advertisements output: 0, Input: 0 
        CDP version 2 advertisements output: 179, Input: 148
```

Вопрос:

Сколько пакетов имеет выход CDP с момента последнего сброса счетчика?

Ответ:

Ответы могут отличаться. В этом примере CDP выводит 179 пакетов
 
d.	Настройте SVI для VLAN 1 на S1 и S2, используя IP-адреса, указанные в таблице адресации выше. Настройте шлюз по умолчанию для каждого коммутатора на основе таблицы адресов.

```
S1(config)# interface vlan 1
S1(config-if)# ip address 10.22.0.2 255.255.255.0
S1(config-if)# no shutdown
S1(config-if)# exit
S1(config)# ip default-gateway 10.22.0.1
```

```
S2(config)# interface vlan 1
S2(config-if)# ip address 10.22.0.3 255.255.255.0
S2(config-if)# no shutdown
S2(config-if)# exit
S2(config)# ip default-gateway 10.22.0.1
```

e.	На R1 выполните команду show cdp entry S1 . 

Вопрос:

Какие дополнительные сведения доступны теперь?

Ответ:

Выходные данные включают IP-адрес управления для SVI VLAN 1 на S1, который был только что настроен.
 
```
R1 # show cdp entry  S1 
-------------------------
Device ID: S1
Entry address(es):
  IP address: 10.22.0.2
Platform: cisco WS-C2960+24LC-L, Capabilities: Switch IGMP 
Interface: GigabitEthernet0/0/1, Port ID (outgoing port): FastEthernet0/5
Holdtime : 133 sec

Version :
Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.2(4)E8, RELEASE SOFTWARE (fc3) 
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2019 by Cisco Systems, Inc.
Compiled Fri 15-Mar-19 17:28 by prod_rel_team 

advertisement version: 2
VTP Management Domain: ''
Native VLAN: 1
Duplex: full
Management address(es):
  IP address: 10.22.0.2 
```

f.	Отключить CDP глобально на всех устройствах. 

```
R1(config)# no cdp run
S1(config)# no cdp run
S2(config)# no cdp run
```

Закройте окно настройки.

# Часть 3. Обнаружение сетевых ресурсов с помощью протокола LLDP

На устройствах Cisco протокол LLDP может быть включен по умолчанию. Воспользуйтесь LLDP, чтобы обнаружить порты, к которым подключены кабели.

Откройте окно конфигурации

a.	Введите соответствующую команду lldp, чтобы включить LLDP на всех устройствах в топологии.

```
R1(config)# lldp run
S1(config)# lldp run
S2(config)# lldp run
```

b.	На S1 выполните соответствующую команду lldp, чтобы предоставить подробную информацию о S2. 

```
S1# show lldp entry S2

Capability codes:
    (R) Router, (B) Bridge, (T) Telephone, (C) DOCSIS Cable Device
    (W) WLAN Access Point, (P) Repeater, (S) Station, (O) Other
------------------------------------------------
Local Intf: Fa0/1  
Chassis id: c025.5cd7.ef00 
Port id: Fa0/1 
Port Description: FastEthernet0/1
System Name: S2

System Description:
Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.2(4)E8, RELEASE SOFTWARE (fc3) 
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2019 by Cisco Systems, Inc.
Compiled Fri 15-Mar-19 17:28 by prod_rel_team 

Time remaining: 109 seconds 
System Capabilities: B
Enabled Capabilities: B
Management Addresses:
    IP: 10.22.0.3 
Auto Negotiation - supported, enabled
Physical media capabilities:
    100base-TX(FD)
    100base-TX(HD)
    10base-T(FD)
    10base-T(HD)
Media Attachment Unit type: 16
Vlan ID: 1


Total entries displayed: 1
```

Вопрос:

Что такое chassis ID  для коммутатора S2?

Ответ:

Ответы могут отличаться. В этом примере идентификатор шасси для S2 равен c025.5cd7.ef00.
 
Закройте окно настройки.

c.	Соединитесь через консоль на всех устройствах и используйте команды LLDP, необходимые для отображения топологии физической сети только из выходных данных команды show.

Ответы могут отличаться, но основная команда, которую нужно использовать, - показать соседа по LLDP. Идея состоит в том, чтобы учащийся визуализировал топологию сети только по выводам LLDP.

# Часть 4. Настройка NTP

В части 4 необходимо настроить маршрутизатор R1 в качестве сервера NTP, а маршрутизатор R2 в качестве клиента NTP маршрутизатора R1. Необходимо выполнить синхронизацию времени для Syslog и отладочных функций. Если время не синхронизировано, сложно определить, какое сетевое событие стало причиной данного сообщения.

# Шаг 1. Выведите на экран текущее время.

Откройте окно конфигурации

Введите команду show clock для отображения текущего времени на R1. Запишите отображаемые сведения о текущем времени в следующей таблице.

|Дата|	Время|	Часовой пояс|	Источник времени|
| ---------------- |:------------------:| -----------------:|-----------------:|
|Ответы будут разными|Ответы будут разными|Ответы могут отличаться, обычно часовой пояс не задан|Ответы могут отличаться, обычно источник времени не задан|

# Шаг 2. Установите время.

С помощью команды clock set установите время на маршрутизаторе R1. Введенное время должно быть в формате UTC. 

```
R1# clock set 19:45:00 19 September 2019
```
 
# Шаг 3. Настройте главный сервер NTP.

Настройте R1 в качестве хозяина NTP с уровнем слоя 4.

```
R1(config)# ntp master 4
```
 
# Шаг 4. Настройте клиент NTP.

a.	Выполните соответствующую команду на S1 и S2, чтобы просмотреть настроенное время. Запишите текущее время,  в следующей таблице.

|Дата	|Время|	Часовой пояс|
| ---------------- |:------------------:| -----------------:|
|Ответы будут разными|Ответы будут разными|Ответы будут разными|
		
b.	Настройте S1 и S2 в качестве клиентов NTP. Используйте соответствующие команды NTP для получения времени от интерфейса G0/0/1 R1, а также для периодического обновления календаря или аппаратных часов коммутатора.

```
S1(config)# ntp server 10.22.0.1
S1(config)# ntp update-calendar
```

# Шаг 5. Проверьте настройку NTP.

a.	Используйте соответствующую команду show , чтобы убедиться, что S1 и S2 синхронизированы с R1.

Примечание. Синхронизация метки времени на маршрутизаторе R2 с меткой времени на маршрутизаторе R1 может занять несколько минут.

```
S1# show ntp status | include Clock

Clock is synchronized, stratum 5, reference is 10.22.0.1

S2# show ntp associations

 address ref clock st when poll reach delay offset disp
*~10.22.0.1 127.127.1.1 4 4 64 3 3.194 4.629 63.914
 * sys.peer, # selected, + candidate, - outlyer, x falseticker, ~ configured
```

b.	Выполните соответствующую команду на S1 и S2, чтобы просмотреть настроенное время и сравнить ранее записанное время.

Откройте окно конфигурации

Вопрос для повторения

Для каких интерфейсов в пределах сети не следует использовать протоколы обнаружения сетевых ресурсов? Поясните ответ.

Ответ:

Протоколы обнаружения не следует использовать на интерфейсах, обращенных к внешним сетям, поскольку эти протоколы дают представление о внутренней сети. Эта информация позволяет злоумышленникам получить ценную информацию о внутренней сети и может быть использована для взлома сети.
