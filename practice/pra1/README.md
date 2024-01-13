Базовая настройка коммутатора

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


