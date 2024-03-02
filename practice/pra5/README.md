# Лабораторная работа. Настройка IPv6-адресов на сетевых устройствах 

# Топология
![Image alt](https://github.com/giendo152/network-basic/blob/main/practice/pra5/1.png)
 
# Таблица адресации

| Устройство | Интерфейс	| IP-адрес	| Маска	| Шлюз по умолчанию |
| ---------------- |:------------------:| -----------------:|--------------:|------------: |
| R1               |	G0/0/1	| 192.168.1.1 |	255.255.255.0 |	— |
| S1 | VLAN 1	| 192.168.1.11	| 255.255.255.0	| 192.168.1.1 |
| PC-A |	NIC	| 192.168.1.3 |	255.255.255.0	| 192.168.1.1 |

# Задачи
# Часть 1. Настройка основных параметров устройства

# Часть 2. Настройка маршрутизатора для доступа по протоколу SSH

# Часть 3. Настройка коммутатора для доступа по протоколу SSH

# Часть 4. SSH через интерфейс командной строки (CLI) коммутатора

# Часть 1. Настройка основных параметров устройств

В части 1 потребуется настроить топологию сети и основные параметры, такие как IP-адреса интерфейсов, доступ к устройствам и пароли на маршрутизаторе.

# Шаг 1. Создайте сеть согласно топологии.

# Шаг 2. Выполните инициализацию и перезагрузку маршрутизатора и коммутатора.

# Шаг 3. Настройте маршрутизатор.

Откройте окно конфигурации

a.	Подключитесь к маршрутизатору с помощью консоли и активируйте привилегированный режим EXEC.

b.	Войдите в режим конфигурации.

c.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

d.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

e.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

f.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

g.	Зашифруйте открытые пароли.

h.	Создайте баннер, который предупреждает о запрете несанкционированного доступа.

i.	Настройте и активируйте на маршрутизаторе интерфейс G0/0/1, используя информацию, приведенную в таблице адресации.

j.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.


``` Router>en
Router#
Router#
Router#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname R1
R1(config)#banner
R1(config)#banner mot
R1(config)#banner motd 
% Incomplete command.
R1(config)#
R1#
%SYS-5-CONFIG_I: Configured from console by console

R1# conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#int g0/0/1
R1(config-if)#ip add
R1(config-if)#ip address 192.168.1.1 255.255.255.0
R1(config-if)#no sh
R1(config-if)#no shutdown 

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

R1(config-if)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

R1(config-if)#end
R1#
%SYS-5-CONFIG_I: Configured from console by console

R1#copy
R1#copy ru
R1#copy running-config
```

```S1>en
S1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#no ip do
S1(config)#no ip domain loo
S1(config)#no ip domain lookup 
S1(config)#en
S1(config)#ena
S1(config)#enable se
S1(config)#enable secret class
S1(config)#line console 0
S1(config-line)#pass
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#line vty 0 15
S1(config-line)#pass
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#int vlan 1
S1(config-if)#ip add
S1(config-if)#ip address 192.168.1.11 255.255.255.0
S1(config-if)#no sh
S1(config-if)#no shutdown 

S1(config-if)#
%LINK-5-CHANGED: Interface Vlan1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up

S1(config-if)#copy
S1(config-if)#end
S1#
%SYS-5-CONFIG_I: Configured from console by console

S1#copy
S1#copy runn
S1#copy running-config
```
```C:\>ping 192.168.1.1

Pinging 192.168.1.1 with 32 bytes of data:

Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.1.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>
```
