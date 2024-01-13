# Базовая настройка коммутатора

Задачи

Часть 1. Проверка конфигурации коммутатора по умолчанию

Часть 2. Создание сети и настройка основных параметров устройства

•	Настройте базовые параметры коммутатора.

•	Настройте IP-адрес для ПК.

Часть 3. Проверка сетевых подключений

•	Отобразите конфигурацию устройства.

•	Протестируйте сквозное соединение, отправив эхо-запрос.

•	Протестируйте возможности удаленного управления с помощью Telnet.

#	Таблица адресации

| Устройство       | Интерфейс          |IP-адрес / префикс |
| ---------------- |:------------------:| -----------------:|
| S1               |VLAN 1              | 192.168.1.2 /24   |
| PC-A             | NIC                |   192.168.1.10 /24|



Решение:

Часть 1. Создание сети и проверка настроек коммутатора по умолчанию



Шаг 1. Создайте сеть согласно топологии.

a.	Подсоедините консольный кабель, как показано в топологии. На данном этапе не подключайте кабель Ethernet компьютера PC-A.
Примечание. При использовании Netlab отключите интерфейс F0/6 на коммутаторе S1. Это имеет такой же эффект, как отсоединение компьютера PC-A от коммутатора S1.

b.	Установите консольное подключение к коммутатору с компьютера PC-A с помощью Tera Term или другой программы эмуляции терминала.

Вопрос:

Почему нужно использовать консольное подключение для первоначальной настройки коммутатора? Почему нельзя подключиться к коммутатору через Telnet или SSH?

Ответ: 

Для нового коммутатора вы должны использовать консольное подключение для первоначальной настройки. Это связано с тем, что у нового коммутатора изначально нет данных и IP-адреса. Как telnet, так и ssh нельзя использовать для подключения нового коммутатора, поскольку для установления соединения им обоим требуется IP-адрес.


Шаг 2. Проверьте настройки коммутатора по умолчанию.

a.	Предположим, что коммутатор не имеет файла конфигурации, сохраненного в энергонезависимой памяти (NVRAM). Консольное подключение к коммутатору с помощью Tera Term или другой программы эмуляции терминала предоставит доступ к командной строке пользовательского режима EXEC в виде Switch>. Введите команду enable, чтобы войти в привилегированный режим EXEC.

Откройте окно конфигурации

Обратите внимание, что измененная в конфигурации строка будет отражать привилегированный режим EXEC.

Убедитесь, что на коммутаторе находится пустой файл конфигурации по умолчанию, с помощью команды show running-config привилегированного режима EXEC. Если конфигурационный файл был предварительно сохранен, его нужно удалить. В зависимости от модели коммутатора и версии IOS ваша конфигурация может слегка отличаться. Тем не менее, настроенных паролей или IP-адресов в конфигурации быть не должно. Выполните очистку настроек и перезагрузите коммутатор, если ваш коммутатор имеет настройки, отличные от настроек по умолчанию.

Вводим команду sh running-config

```   sh running-config 
Building configuration...

Current configuration : 1080 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname Switch
!
!
!
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
!
interface FastEthernet0/2
!
interface FastEthernet0/3
!
interface FastEthernet0/4
!
interface FastEthernet0/5
!
interface FastEthernet0/6
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface Vlan1
 no ip address
 shutdown
!
!
!
!
line con 0
!
line vty 0 4
 login
line vty 5 15
 login
!
!
!
!
end
```

b.	Изучите текущий файл running configuration.

Вопросы:

Сколько интерфейсов FastEthernet имеется на коммутаторе 2960?

Сколько интерфейсов Gigabit Ethernet имеется на коммутаторе 2960?

Каков диапазон значений, отображаемых в vty-линиях?

Ответ:

Интерфейсов FastEthernet на коммутаторе 2960 имеется 24 штуки.

Интерфейсов Gigabit на коммутаторе 2960 имеется 2 штуки.

Диапазон значений vty представляет собой набор номеров, которые могут быть использованы для идентификации разных виртуальных терминальных линий. Номера vty принадлежат к диапазону от 0 до 4 095, и традиционно разделены на группы по 16 (0-15, 16-31, 32-47 и так далее).

c.	Изучите файл загрузочной конфигурации (startup configuration), который содержится в энергонезависимом ОЗУ (NVRAM).

Вопрос:

Почему появляется это сообщение?

Ответ:

Сообщение «Startup config is not present» указывает на то, что в системе отсутствует файл, содержащий настройки и конфигурацию для запуска операционной системы.

d.	Изучите характеристики SVI для VLAN 1.

Вопросы:

Назначен ли IP-адрес сети VLAN 1?

Какой MAC-адрес имеет SVI? Возможны различные варианты ответов.

Данный интерфейс включен?

Ответ:

IP-адрес сети VLAN 1 была назначена - 192.168.2.1\24 , как и была представлена в таблице выше.

Данный интерфейс включен.

e.	Изучите IP-свойства интерфейса SVI сети VLAN 1.

Вопрос:

Какие выходные данные вы видите?

``` Vlan1 is up, line protocol is up
  Internet address is 192.168.1.2/24
  Broadcast address is 255.255.255.255 
  Address determined by setup command 
  MTU is 1500 bytes 
  Helper address is not set
  Directed broadcast forwarding is disabled 
  Outgoing access list is not set 
  Inbound  access list is not set 
  Proxy ARP is enabled 
  Local Proxy ARP is disabled 
  Security level is default 
  Split horizon is enabled 
  ICMP redirects are always sent 
  ICMP unreachables are always sent 
  ICMP mask replies are never sent 
  IP fast switching is disabled 
  IP fast switching on the same interface is disabled 
  IP Null turbo vector 
  IP multicast fast switching is disabled 
  IP multicast distributed fast switching is disabled 
  IP route-cache flags are None 
  Router Discovery is disabled 
  IP output packet accounting is disabled 
  IP access violation accounting is disabled 
  TCP/IP header compression is disabled 
  RTP/IP header compression is disabled 
  Probe proxy name replies are disabled 
  Policy routing is disabled 
  Network address translation is disable 
  WCCP Redirect outbound is disabled 
  WCCP Redirect inbound is disabled 
  WCCP Redirect exclude is disabled 
  BGP Policy Mapping is disabled
```

f. Подключите Ethernet-кабель компьютера PC-A к порту 6 на коммутаторе и изучите IP-свойства интерфейса SVI сети VLAN 1. Дождитесь согласования параметров скорости и дуплекса между коммутатором и ПК.

g.	Изучите сведения о версии ОС Cisco IOS на коммутаторе.

Вопросы:

Под управлением какой версии ОС Cisco IOS работает коммутатор?

Как называется файл образа системы?

Какой базовый MAC-адрес назначен коммутатору?

```64K bytes of flash-simulated non-volatile configuration memory.
Base ethernet MAC Address       : 00:E0:F7:C3:B3:48
Motherboard assembly number     : 73-10390-03
Power supply part number        : 341-0097-02
Motherboard serial number       : FOC10093R12
Power supply serial number      : AZS1007032H
Model revision number           : B0
Motherboard revision number     : B0
Model number                    : WS-C2960-24TT-L
System serial number            : FOC1010X104
Top Assembly Part Number        : 800-27221-02
Top Assembly Revision Number    : A0
Version ID                      : V02
CLEI Code Number                : COM3L00BRA
Hardware Board Revision Number  : 0x01


Switch Ports Model              SW Version            SW Image
------ ----- -----              ----------            ----------
*    1 26    WS-C2960-24TT-L    15.0(2)SE4            C2960-LANBASEK9-M
```

```FastEthernet0/6 is up, line protocol is up (connected)
  Hardware is Lance, address is 0001.c978.cb06 (bia 0001.c978.cb06)
 BW 100000 Kbit, DLY 1000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Full-duplex, 100Mb/s
  input flow-control is off, output flow-control is off
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:08, output 00:00:05, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue :0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     956 packets input, 193351 bytes, 0 no buffer
     Received 956 broadcasts, 0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     0 watchdog, 0 multicast, 0 pause input
     0 input packets with dribble condition detected
     2357 packets output, 263570 bytes, 0 underruns
     0 output errors, 0 collisions, 10 interface resets
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier
     0 output buffer failures, 0 output buffers swapped out
```

i.	Изучите параметры сети VLAN по умолчанию на коммутаторе.

Какое имя присвоено сети VLAN 1 по умолчанию?

Какие порты расположены в сети VLAN 1?

Активна ли сеть VLAN 1?

К какому типу сетей VLAN принадлежит VLAN по умолчанию?

```
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23, Fa0/24
                                                Gig0/1, Gig0/2
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
1    enet  100001     1500  -      -      -        -    -        0      0
1002 fddi  101002     1500  -      -      -        -    -        0      0   
1003 tr    101003     1500  -      -      -        -    -        0      0   
1004 fdnet 101004     1500  -      -      -        ieee -        0      0   
1005 trnet 101005     1500  -      -      -        ibm  -        0      0   

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------

Remote SPAN VLANs
------------------------------------------------------------------------------

Primary Secondary Type              Ports
------- --------- ----------------- ------------------------------------------
```

j.	Изучите флеш-память.

Выполните одну из следующих команд, чтобы изучить содержимое флеш-каталога.

Switch# show flash 

Switch# dir flash: 

В конце имени файла указано расширение, например .bin. Каталоги не имеют расширения файла.

Вопрос:

Какое имя присвоено образу Cisco IOS?

```Directory of flash:/

    1  -rw-     4670455          <no date>  2960-lanbasek9-mz.150-2.SE4.bin

64016384 bytes total (59345929 bytes free)
```


#Часть 2. Настройка базовых параметров сетевых устройств

Во второй части необходимо будет настроить основные параметры коммутатора и компьютера.

#Шаг 1. Настройте базовые параметры коммутатора.

a.	В режиме глобальной конфигурации скопируйте следующие базовые параметры конфигурации и вставьте их в файл на коммутаторе S1. 

```no ip domain-lookup
hostname S1
service password-encryption
enable secret class
banner motd #
Unauthorized access is strictly prohibited. #
```

b.	Назначьте IP-адрес интерфейсу SVI на коммутаторе. Благодаря этому вы получите возможность удаленного управления коммутатором.
Прежде чем вы сможете управлять коммутатором S1 удаленно с компьютера PC-A, коммутатору нужно назначить IP-адрес. Согласно конфигурации по умолчанию коммутатором можно управлять через VLAN 1. Однако в базовой конфигурации коммутатора не рекомендуется назначать VLAN 1 в качестве административной VLAN.

Поэтому возьмем Vlan 99 для административных целей.
Для начала создайте на коммутаторе новую VLAN 99. Затем настройте IP-адрес коммутатора на 192.168.1.2 с маской подсети 255.255.255.0 на внутреннем виртуальном интерфейсе (SVI) VLAN 99.

```S1# configure terminal
S1(config)# vlan 99
S1(config-vlan)# exit
S1(config)# interface vlan99
%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan99, changed state to down
S1(config-if)# ip address 192.168.1.3 255.255.255.0
S1(config-if)# no shutdown
S1(config-if)# exit
S1(config)#
```
Обратите внимание, что интерфейс VLAN 99 выключен, несмотря на то, что вы ввели команду no shutdown. В настоящее время интерфейс выключен, поскольку сети VLAN 99 не назначены порты коммутатора.

Ассоциируйте все пользовательские порты с VLAN 99.
S1(config)# interface range f0/1 – 24,g0/1 - 2
S1(config-if-range)# switchport access vlan 99
S1(config-if-range)# exit
S1(config)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to down
%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan99, changed state to up

Чтобы установить подключение между узлом и коммутатором, порты, используемые узлом , должны находиться в той же VLAN, что и коммутатор. Обратите внимание, что в выходных данных выше интерфейс VLAN 1 выключен, поскольку ни один из портов не назначен сети VLAN 1. Через несколько секунд VLAN 99 включится, потому что как минимум один активный порт (F0/6, к которому подключён компьютер PC-A) назначен сети VLAN 99.

Чтобы убедиться, что все пользовательские порты находятся в сети VLAN 99, выполните команду show vlan brief

```VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    
99   VLAN0099                         active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23, Fa0/24
                                                Gig0/1, Gig0/2
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active
```
Настройте IP-шлюз по умолчанию для коммутатора S1. Если не настроен ни один шлюз по умолчанию, коммутатором нельзя управлять из удалённой сети, на пути к которой имеется более одного маршрутизатора. Он не отвечает на эхо -запросы из удалённой сети. Хотя в этом упражнении не учитывается внешний IP-шлюз, представьте, что впоследствии вы подключите LAN к маршрутизатору для обеспечения внешнего доступа. При условии, что интерфейс LAN маршрутизатора равен 192.168.1.1, настройте шлюз по умолчанию для коммутатора.

c.	Доступ через порт консоли также следует ограничить  с помощью пароля. Используйте cisco в качестве пароля для входа в консоль в этом задании. Конфигурация по умолчанию разрешает все консольные подключения без пароля. Чтобы консольные сообщения не прерывали выполнение команд, используйте параметр logging synchronous.

```S1(config)# line con 0
S1(config-line)# logging synchronous 
```

d.	Настройте каналы виртуального соединения для удаленного управления (vty), чтобы коммутатор разрешил доступ через Telnet. Если не настроить пароль VTY, будет невозможно подключиться к коммутатору по протоколу Telnet.

```S1(config)# line vty 0 15
S1(config-line)# password cisco
S1(config-line)# login
S1(config-line)# end
S1#
```

Вопрос:

Для чего нужна команда login?

Команда login обеспечивает процесс аутентификации пользователя и является обязательной для линий подключенияIOS-коммутаторов. 

#Шаг 2. Настройте IP-адрес на компьютере PC-A.

Назначьте компьютеру IP-адрес и маску подсети в соответствии с таблицей адресации.

1)	Перейдите в Панель управления. (Control Panel)

2)	В представлении «Категория» выберите « Просмотр состояния сети и задач».

3)	Щелкните Изменение параметров адаптера на левой панели.

4)	Щелкните правой кнопкой мыши интерфейс Ethernet и выберите «Свойства» .

5)	Выберите Протокол Интернета версии 4 (TCP/IPv4) > Свойства.

6)	Выберите Использовать следующий IP-адрес и введите IP-адрес и маску подсети  и нажмите ОК. (Ip у нас будет - 192.168.1.10/24)

#Часть 3. Проверка сетевых подключений

В третьей части лабораторной работы вам предстоит проверить и задокументировать конфигурацию коммутатора, протестировать сквозное соединение между компьютером PC-A и коммутатором S1, а также протестировать возможность удаленного управления коммутатором.


#Шаг 1. Отобразите конфигурацию коммутатора.
Используйте консольное подключение на компьютере PC-A для отображения и проверки конфигурации коммутатора. Команда show run позволяет постранично отобразить всю текущую конфигурацию. Для пролистывания используйте клавишу пробела.

a.	Пример конфигурации приведен ниже. Параметры, которые вы настроили, выделены желтым. Другие параметры конфигурации — значения IOS по умолчанию.

Откройте окно конфигурации

```S1# show run
Building configuration...

Current configuration : 2206 bytes
!
version 15.2
no service pad
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname S1
!
boot-start-marker
boot-end-marker
!
enable secret 5 $1$mtvC$6NC.1VKr3p6bj7YGE.jNg0
!
no aaa new-model
system mtu routing 1500 
!
!
no ip domain-lookup
!
<output omitted>
!
interface FastEthernet0/24
 switchport access vlan 99
!
interface GigabitEthernet0/1
 switchport access vlan 99
!
interface GigabitEthernet0/2
 switchport access vlan 99
!
interface Vlan1
 ip address 192.168.1.2 255.255.255.0
!
ip default-gateway 192.168.1.1
ip http server
ip http secure-server
!
banner motd ^C
Unauthorized access is strictly prohibited. ^C
!
line con 0
 password 7 00071A150754
 logging synchronous
 login
line vty 0 4
 password 7 121A0C041104
 login
line vty 5 15
 password 7 121A0C041104
 login
!
end
```

b.	Проверьте параметры VLAN 1.

S1# show interface vlan 1 

```Vlan1 is up, line protocol is down
  Hardware is CPU Interface, address is 00e0.f7c3.b348 (bia 00e0.f7c3.b348)
  Internet address is 192.168.1.2/24
  MTU 1500 bytes, BW 100000 Kbit, DLY 1000000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 21:40:21, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     1682 packets input, 530955 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicast)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     563859 packets output, 0 bytes, 0 underruns
     0 output errors, 23 interface resets
     0 output buffer failures, 0 output buffers swapped out
```

Какова полоса пропускания этого интерфейса?

В каком состоянии находится VLAN 1?

В каком состоянии находится канальный протокол?

#Шаг 2. Протестируйте сквозное соединение, отправив эхо-запрос.

a.	В командной строке компьютера PC-A с помощью утилиты ping проверьте связь сначала с адресом PC-A.
C:\> ping 192.168.1.10 

```%SYS-5-CONFIG_I: Configured from console by console

S1#ping 192.168.1.10

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.10, timeout is 2 seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max = 0/0/0 ms
```

b.	Из командной строки компьютера PC-A отправьте эхо-запрос на административный адрес интерфейса SVI коммутатора S1.
C:\> ping 192.168.1.2

```Pinging 192.168.1.2 with 32 bytes of data:

Request timed out.
Reply from 192.168.1.2: bytes=32 time<1ms TTL=255
Reply from 192.168.1.2: bytes=32 time<1ms TTL=255
Reply from 192.168.1.2: bytes=32 time=4ms TTL=255
```

Поскольку компьютеру PC-A нужно преобразовать МАС-адрес коммутатора S1 с помощью ARP, время ожидания передачи первого пакета может истечь. Если эхо-запрос не удается, найдите и устраните неполадки базовых настроек устройства. Проверьте как физические кабели, так и логическую адресацию.

#Шаг 3. Проверьте удаленное управление коммутатором S1.

После этого используйте удаленный доступ к устройству с помощью Telnet. В этой лабораторной работе устройства PC-A и S1 расположены рядом. В производственной сети коммутатор может находиться в коммутационном шкафу на последнем этаже, в то время как административный компьютер находится на первом этаже. На данном этапе вам предстоит использовать Telnet для удаленного доступа к коммутатору S1 через его административный адрес SVI. Telnet — это не безопасный протокол, но вы можете использовать его для проверки удаленного доступа. В случае с Telnet вся информация, включая пароли и команды, отправляется через сеанс в незашифрованном виде. В последующих лабораторных работах вы будете использовать протокол SSH для удаленного доступа к сетевым устройствам.

a.	Откройте Tera Term или другую программу эмуляции терминала с возможностью Telnet. 
C:\Users\User1> telnet 192.168.1.2
b.	Выберите сервер Telnet и укажите адрес управления SVI для подключения к S1.  Пароль: cisco.

c.	После ввода пароля cisco вы окажетесь в командной строке пользовательского режима. Для перехода в исполнительский режим EXEC введите команду enable и используйте секретный пароль class.

d.	Сохраните конфигурацию.

e.	Чтобы завершить сеанс Telnet, введите exit.

```C:\>telnet 192.168.1.3
Trying 192.168.1.3 ...Open



User Access Verification

Password: 
Password: 
S1>en
Password: 
S1#co
S1#cop
S1#copy ru
S1#copy running-config st
S1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
S1#
```

# Вопросы для повторения

1.	Зачем необходимо настраивать пароль VTY для коммутатора?

Если не настроить пароль VTY, будет невозможно подключиться к коммутатору по протоколу Telnet.

2.	Что нужно сделать, чтобы пароли не отправлялись в незашифрованном виде?
	
Необходимо использовать глобальную команду service password encryption.

	Приложение А. Инициализация и перезагрузка коммутатора
a.	Подключитесь к коммутатору с помощью консоли и войдите в привилегированный режим EXEC.
Откройте окно конфигурации
Switch> enable
Switch#
b.	Воспользуйтесь командой show flash, чтобы определить, были ли созданы сети VLAN на коммутаторе.
Switch# show flash
Каталог flash:/

```Directory of flash:/

    1  -rw-     4670455          <no date>  2960-lanbasek9-mz.150-2.SE4.bin
    3  -rw-        2034          <no date>  config.text
    2  -rw-         616          <no date>  vlan.dat

64016384 bytes total (59343279 bytes free)
```

c.	Если во флеш-памяти обнаружен файл vlan.dat, удалите его.
Switch# delete vlan.dat
Delete filename [vlan.dat]?
d.	Появится запрос о проверке имени файла. Если вы ввели имя правильно, нажмите клавишу Enter. В противном случае вы можете изменить имя файла.
Будет предложено подтвердить удаление этого файла. Нажмите клавишу Enter для подтверждения.
Delete flash:/vlan.dat? [confirm]
Switch#
e.	Введите команду erase startup-config, чтобы удалить файл загрузочной конфигурации из NVRAM. Появится запрос об удалении конфигурационного файла. Нажмите клавишу Enter для подтверждения.
Switch# erase startup-config
Erasing the nvram filesystem will remove all configuration files! Продолжить? [confirm]
[OK]
Erase of nvram: complete
Switch#
f.	Перезагрузите коммутатор, чтобы удалить устаревшую информацию о конфигурации из памяти. Затем появится запрос на подтверждение перезагрузки коммутатора. Нажмите клавишу Enter, чтобы продолжить.
Switch# reload
Proceed with reload? (Команда reload запускается на активном модуле, будет перезагружен весь стек. Продолжить ее выполнение?) [confirm]
Примечание. До перезагрузки коммутатора может появиться запрос о сохранении текущей конфигурации. Чтобы ответить, введите no и нажмите клавишу Enter.
System configuration has been modified. Save? [yes/no]: no
g.	После перезагрузки коммутатора появится запрос о входе в диалоговое окно начальной конфигурации. Чтобы ответить, введите no и нажмите клавишу Enter.
Would you like to enter the initial configuration dialog? [yes/no]: no
Switch>



