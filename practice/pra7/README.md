# Лабораторная работа. Развертывание коммутируемой сети с резервными каналами

# Топология

![Image alt](https://github.com/giendo152/network-basic/blob/main/practice/pra7/1.png)
 
# Таблица адресации

| Устройство | Интерфейс	| IP-адрес	| Маска подсети |
| ---------------- |:------------------:| -----------------:|--------------:|
| S1               |	VLAN 1	| 192.168.1.1 |	255.255.255.0 |	
| S2               |	VLAN 1	| 192.168.1.2 |	255.255.255.0 |
| S3               |	VLAN 1	| 192.168.1.3|	255.255.255.0 |

# Цели

Часть 1. Создание сети и настройка основных параметров устройства

Часть 2. Выбор корневого моста

Часть 3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов

Часть 4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов

# Часть 1:	Создание сети и настройка основных параметров устройства

В части 1 вам предстоит настроить топологию сети и основные параметры маршрутизаторов.

# Шаг 1:	Создайте сеть согласно топологии.

Подключите устройства, как показано в топологии, и подсоедините необходимые кабели.

# Шаг 2:	Выполните инициализацию и перезагрузку коммутаторов.

# Шаг 3:	Настройте базовые параметры каждого коммутатора.

a.	Отключите поиск DNS.

b.	Присвойте имена устройствам в соответствии с топологией.

c.	Назначьте class в качестве зашифрованного пароля доступа к привилегированному режиму.

d.	Назначьте cisco в качестве паролей консоли и VTY и активируйте вход для консоли и VTY каналов.

e.	Настройте logging synchronous для консольного канала.

f.	Настройте баннерное сообщение дня (MOTD) для предупреждения пользователей о запрете несанкционированного доступа.

g.	Задайте IP-адрес, указанный в таблице адресации для VLAN 1 на всех коммутаторах.

h.	Скопируйте текущую конфигурацию в файл загрузочной конфигурации.

Шаг 3 пункты a-h

```
Switch(config)#hostname S1
S1(config)#no ip do
S1(config)#no ip domain lookup
S1(config)#ena
S1(config)#enable sec
S1(config)#enable secret class
S1(config)#line con
S1(config)#line console 0
S1(config-line)#pass
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#line vty 0 15
S1(config-line)#password cisco
S1(config-line)#login
S1(config)#logging synchronous
S1(config-line)#exit
S1(config)#banner motd "anyway"
S1(config)#int vlan 1
S1(config-if)#ip add
S1(config-if)#ip address 192.168.1.1 255.255.255.0
S1(config-if)#no sh
S1(config-if)#no shutdown 

S1(config-if)#
%LINK-5-CHANGED: Interface Vlan1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up

S1(config-if)#end
S1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```

# Шаг 4:	Проверьте связь.

Проверьте способность компьютеров обмениваться эхо-запросами.

Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S2?	______________

Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S3?	______________

Успешно ли выполняется эхо-запрос от коммутатора S2 на коммутатор S3?	______________

```
S1#ping 192.168.1.2

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.2, timeout is 2 seconds:
..!!!
Success rate is 60 percent (3/5), round-trip min/avg/max = 0/0/0 ms

S1#ping 192.168.1.3

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.3, timeout is 2 seconds:
..!!!
Success rate is 60 percent (3/5), round-trip min/avg/max = 0/0/0 ms

S1#
```
```
S2#ping 192.168.1.3

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.3, timeout is 2 seconds:
..!!!
Success rate is 60 percent (3/5), round-trip min/avg/max = 0/0/0 ms

S2#
```
