# Лабораторная работа. Настройка IPv6-адресов на сетевых устройствах 

# Топология
 
# Таблица адресации

![Image alt](https://github.com/giendo152/network-basic/blob/main/practice/pra4/рисунок.png)

| Устройство | Интерфейс	| IPv6-адрес	| Длина префикса	| Шлюз по умолчанию |
| ---------------- |:------------------:| -----------------:|--------------:|------------: |
| R1               |	G0/0/0	| 2001:db8:acad:a::1 |	64 |	— |
| R1 |  G0/0/1	| 2001:db8:acad:1::1 |	64 |	— |
| S1 | VLAN 1	| 2001:db8:acad:1::b	| 64	| — |
| PC-A |	NIC	| 2001:db8:acad:1::3 |	64	| fe80::1 |
| PC-B	| NIC	| 2001:db8:acad:a::3 |	64	| fe80::1 |

# Задачи

# Часть 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора

# Часть 2. Ручная настройка IPv6-адресов

# Часть 3. Проверка сквозного соединения

# Инструкции

# Часть 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора

После подключения сети, инициализации и перезагрузки маршрутизатора и коммутатора выполните следующие действия:

# Шаг 1. Настройте маршрутизатор.

Назначьте имя хоста и настройте основные параметры устройства.

# Шаг 2. Настройте коммутатор.

Назначьте имя хоста и настройте основные параметры устройства.

# Часть 2. Ручная настройка IPv6-адресов

# Шаг 1. Назначьте IPv6-адреса интерфейсам Ethernet на R1.


```
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#int g0/0/0
R1(config-if)#ipv6 address 2001:db8:acad:a::1/64
R1(config-if)#no sh
R1(config-if)#no shutdown 

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/0, changed state to up

R1(config-if)#in
R1(config-if)#int
R1(config-if)#int g0/0/1
R1(config-if)#ex
R1(config)#int g0/0/1
R1(config-if)#ipv6 address 2001:db8:acad:1::1/64
R1(config-if)#no sh
R1(config-if)#no shutdown 

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

R1(config-if)#end
R1#
%SYS-5-CONFIG_I: Configured from console by console
R1#show ipv6 interface brief 
GigabitEthernet0/0/0       [up/up]
    FE80::240:BFF:FE72:4901
    2001:DB8:ACAD:A::1
GigabitEthernet0/0/1       [up/up]
    FE80::240:BFF:FE72:4902
    2001:DB8:ACAD:1::1
GigabitEthernet0/0/2       [administratively down/down]
    unassigned
Vlan1                      [administratively down/down]
    unassigned
R1#
```

# Вопрос:

Какие группы многоадресной рассылки назначены интерфейсу G0/0?

Введите команду show ipv6 interface g0/0 (g0/0/0), чтобы узнать, какие группы многоадресной рассылки назначены интерфейсу G0/0. Обратите внимание, что в списке групп для интерфейса G0/0 отображается группа многоадресной рассылки всех маршрутизаторов (FF02::2).

```
R1#show ipv6 interface g0/0/0
GigabitEthernet0/0/0 is up, line protocol is up
  IPv6 is enabled, link-local address is FE80::240:BFF:FE72:4901
  No Virtual link-local address(es):
  Global unicast address(es):
    2001:DB8:ACAD:A::1, subnet is 2001:DB8:ACAD:A::/64
  Joined group address(es):
    FF02::1
    FF02::1:FF00:1
    FF02::1:FF72:4901
  MTU is 1500 bytes
  ICMP error messages limited to one every 100 milliseconds
  ICMP redirects are enabled
  ICMP unreachables are sent
  ND DAD is enabled, number of DAD attempts: 1
  ND reachable time is 30000 milliseconds
```

# Шаг 2. Активируйте IPv6-маршрутизацию на R1.

a.	В командной строке на PC-B введите команду ipconfig, чтобы получить данные IPv6-адреса, назначенного интерфейсу ПК.

Назначен ли индивидуальный IPv6-адрес сетевой интерфейсной карте (NIC) на PC-B?

```
C:\>ipconfig

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Link-local IPv6 Address.........: FE80::2E0:8FFF:FE17:3B66
   IPv6 Address....................: 2001:DB8:ACAD:A::3
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: FE80::1
                                     0.0.0.0

Bluetooth Connection:

   Connection-specific DNS Suffix..: 
   Link-local IPv6 Address.........: ::
   IPv6 Address....................: ::
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: ::
                                     0.0.0.0
```

b.	Активируйте IPv6-маршрутизацию на R1 с помощью команды IPv6 unicast-routing.

Примечание. Это позволит компьютерам получать IP-адреса и данные шлюза по умолчанию с помощью функции SLAAC (Stateless Address Autoconfiguration (Автоконфигурация без сохранения состояния адреса)).

c.	Теперь, когда R1 входит в группу многоадресной рассылки всех маршрутизаторов, еще раз введите команду ipconfig на PC-B. Проверьте данные IPv6-адреса.

Почему PC-B получил глобальный префикс маршрутизации и идентификатор подсети, которые вы настроили на R1?

На R1 все интерфейсы IPv6 теперь являются частью многоадресной группы All-router, FF02::2. Это позволяет маршрутизатору отправлять сообщения Router Advertisement (RA) с информацией о префиксе всем узлам в локальной сети. Обратите внимание, что R1 также отправил сообщение RA, используя свой локальный адрес канала, fe80::1, в качестве IPv6-адреса источника пакета. Устройства будут использовать его в качестве своего адреса шлюза по умолчанию. При использовании SLAAC для обеспечения правильных результатов интерфейс маршрутизатора должен использовать длину префикса /64.

# Шаг 3. Назначьте IPv6-адреса интерфейсу управления (SVI) на S1.

a.	Назначьте адрес IPv6 для S1. Также назначьте этому интерфейсу локальный адрес канала.

```
S1(config)#int vlan 1
S1(config-if)#ipv6 add 2001:db8:acad:1::b/64
```

b.	Проверьте правильность назначения IPv6-адресов интерфейсу управления с помощью команды show ipv6 interface vlan1.

```
S1#show ipv6 int vlan 1
Vlan1 is administratively down, line protocol is down
  IPv6 is tentative, link-local address is FE80::B [TEN]
  No Virtual link-local address(es):
  Global unicast address(es):
    2001:DB8:ACAD:1::B, subnet is 2001:DB8:ACAD:1::/64 [TEN]
  Joined group address(es):
    FF02::1
  MTU is 1500 bytes
  ICMP error messages limited to one every 100 milliseconds
  ICMP redirects are enabled
  ICMP unreachables are sent
  Output features: Check hwidb
  ND DAD is enabled, number of DAD attempts: 1
  ND reachable time is 30000 milliseconds
```

# Шаг 4. Назначьте компьютерам статические IPv6-адреса.

a.	Откройте окно Свойства Ethernet для каждого ПК и назначьте адресацию IPv6.

b.	Убедитесь, что оба компьютера имеют правильную информацию адреса IPv6. Каждый компьютер должен иметь два глобальных адреса IPv6: один статический и один SLACC



# Часть 3. Проверка сквозного подключения

С PC-A отправьте эхо-запрос на FE80::1. Это локальный адрес канала, назначенный G0/1 на R1.

Отправьте эхо-запрос на интерфейс управления S1 с PC-A.

```
C:\>ping fe80::1

Pinging fe80::1 with 32 bytes of data:

Reply from FE80::1: bytes=32 time<1ms TTL=255
Reply from FE80::1: bytes=32 time<1ms TTL=255
Reply from FE80::1: bytes=32 time<1ms TTL=255
Reply from FE80::1: bytes=32 time<1ms TTL=255

Ping statistics for FE80::1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```

Введите команду tracert на PC-A, чтобы проверить наличие сквозного подключения к PC-B.

```
C:\>tracert 2001:db8:acad:a::3

Tracing route to 2001:db8:acad:a::3 over a maximum of 30 hops: 

  1   0 ms      0 ms      0 ms      2001:DB8:ACAD:1::1
  2   0 ms      0 ms      0 ms      2001:DB8:ACAD:A::3

Trace complete.
```

С PC-B отправьте эхо-запрос на PC-A.

```
C:\>ping 2001:db8:acad:a::3

Pinging 2001:db8:acad:a::3 with 32 bytes of data:

Reply from 2001:DB8:ACAD:A::3: bytes=32 time<1ms TTL=127
Reply from 2001:DB8:ACAD:A::3: bytes=32 time<1ms TTL=127
Reply from 2001:DB8:ACAD:A::3: bytes=32 time<1ms TTL=127
Reply from 2001:DB8:ACAD:A::3: bytes=32 time<1ms TTL=127

Ping statistics for 2001:DB8:ACAD:A::3:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```

С PC-B отправьте эхо-запрос на локальный адрес канала G0/0 на R1.

```
C:\>ping 2001:db8:acad:a::1

Pinging 2001:db8:acad:a::1 with 32 bytes of data:

Reply from 2001:DB8:ACAD:A::1: bytes=32 time<1ms TTL=255
Reply from 2001:DB8:ACAD:A::1: bytes=32 time<1ms TTL=255
Reply from 2001:DB8:ACAD:A::1: bytes=32 time<1ms TTL=255
Reply from 2001:DB8:ACAD:A::1: bytes=32 time=4ms TTL=255

Ping statistics for 2001:DB8:ACAD:A::1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 4ms, Average = 1ms

C:\>ping 2001:db8:acad:1::1

Pinging 2001:db8:acad:1::1 with 32 bytes of data:

Reply from 2001:DB8:ACAD:1::1: bytes=32 time<1ms TTL=255
Reply from 2001:DB8:ACAD:1::1: bytes=32 time<1ms TTL=255
Reply from 2001:DB8:ACAD:1::1: bytes=32 time<1ms TTL=255
Reply from 2001:DB8:ACAD:1::1: bytes=32 time<1ms TTL=255

Ping statistics for 2001:DB8:ACAD:1::1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```

# Вопросы для повторения

1.	Почему обоим интерфейсам Ethernet на R1 можно назначить один и тот же локальный адрес канала — FE80::1?

Link-local пакеты никогда не покидают локальную сеть, поэтому один и тот же link-local адрес может использоваться на интерфейсе, связанном с другой локальной сетью.

3.	Какой идентификатор подсети в индивидуальном IPv6-адресе 2001:db8:acad::aaaa:1234/64?

 0 (ноль) или 0000 (нули). Если глобальный префикс маршрутизации — /48, он будет включать первые три шестнадцатеричных символа. Таким образом, четвертый гекстет будет идентификатором подсети IPv6-адреса с префиксом /64. В этом примере четвертый гекстет содержит все нули, а правило IPv6 «Пропуск всех нулевых сегментов» использует двойное двоеточие для отображения идентификатора подсети и первых двух гекстетов идентификатора интерфейса. Вот почему префикс глобального индивидуального адреса 2001:acad::aaaa:1234/64 равен 2001:db8:acad::/64.

# Сводная таблица по интерфейсам маршрутизаторов

| Модель маршрутизатора |	Интерфейс Ethernet № 1	| Интерфейс Ethernet № 2	| Последовательный интерфейс № 1 | 	Последовательный интерфейс № 2 |
| ---------------- |:------------------:| -----------------:|--------------:|---------------------:|
1 800 |	Fast Ethernet 0/0 (F0/0)	Fast Ethernet 0/1 (F0/1)	Serial 0/0/0 (S0/0/0)	Serial 0/0/1 (S0/0/1)
1900	| Gigabit Ethernet 0/0 (G0/0)	Gigabit Ethernet 0/1 (G0/1)	Serial 0/0/0 (S0/0/0)	Serial 0/0/1 (S0/0/1)
2801	| Fast Ethernet 0/0 (F0/0)	Fast Ethernet 0/1 (F0/1)	Serial 0/1/0 (S0/1/0)	Serial 0/1/1 (S0/1/1)
2811	| Fast Ethernet 0/0 (F0/0)	Fast Ethernet 0/1 (F0/1)	Serial 0/0/0 (S0/0/0)	Serial 0/0/1 (S0/0/1)
2900	| Gigabit Ethernet 0/0 (G0/0)	Gigabit Ethernet 0/1 (G0/1)	Serial 0/0/0 (S0/0/0)	Serial 0/0/1 (S0/0/1)
4221	| Gigabit Ethernet 0/0/0 (G0/0/0)	Gigabit Ethernet 0/0/1 (G0/0/1)	Serial 0/1/0 (S0/1/0)	Serial 0/1/1 (S0/1/1)
4300	| Gigabit Ethernet 0/0/0 (G0/0/0)	Gigabit Ethernet 0/0/1 (G0/0/1)	Serial 0/1/0 (S0/1/0)	Serial 0/1/1 (S0/1/1)

Примечание. Чтобы определить конфигурацию маршрутизатора, можно посмотреть на интерфейсы и установить тип маршрутизатора и количество его интерфейсов. Перечислить все комбинации конфигураций для каждого класса маршрутизаторов невозможно. Эта таблица содержит идентификаторы для возможных комбинаций интерфейсов Ethernet и последовательных интерфейсов на устройстве. Другие типы интерфейсов в таблице не представлены, хотя они могут присутствовать в данном конкретном маршрутизаторе. В качестве примера можно привести интерфейс ISDN BRI. Строка в скобках — это официальное сокращение, которое можно использовать в командах Cisco IOS для обозначения интерфейса.

