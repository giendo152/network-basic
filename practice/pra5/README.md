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
# Шаг 4. Настройте компьютер PC-A.

a.	Настройте для PC-A IP-адрес и маску подсети.

b.	Настройте для PC-A шлюз по умолчанию.

# Шаг 5. Проверьте подключение к сети.

Пошлите с PC-A команду Ping на маршрутизатор R1. Если эхо-запрос с помощью команды ping не проходит, найдите и устраните неполадки подключения.


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

# Часть 2. Настройка маршрутизатора для доступа по протоколу SSH

Подключение к сетевым устройствам по протоколу Telnet сопряжено с риском для безопасности, поскольку вся информация передается в виде открытого текста. Протокол SSH шифрует данные сеанса и обеспечивает аутентификацию устройств, поэтому для удаленных подключений рекомендуется использовать именно этот протокол. В части 2 вам нужно настроить маршрутизатор для приема соединений SSH по линиям VTY.

# Шаг 1. Настройте аутентификацию устройств.

При генерации ключа шифрования в качестве его части используются имя устройства и домен. Поэтому эти имена необходимо указать перед вводом команды crypto key.

Откройте окно конфигурации

a.	Задайте имя устройства.

b.	Задайте домен для устройства.

# Шаг 2. Создайте ключ шифрования с указанием его длины.

```R1(config)#ip domain name domain.local
R1(config)#cry
R1(config)#crypto key ge
R1(config)#crypto key generate rsa
The name for the keys will be: R1.domain.local
Choose the size of the key modulus in the range of 360 to 2048 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.

How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]

R1(config)#
*Mar 1 1:33:48.912: %SSH-5-ENABLED: SSH 1.99 has been enabled
R1#
%SYS-5-CONFIG_I: Configured from console by console
```

# Шаг 3. Создайте имя пользователя в локальной базе учетных записей.

Настройте имя пользователя, используя admin в качестве имени пользователя и Adm1nP @55 в качестве пароля.

```R1(config)#username ad
R1(config)#username adm
R1(config)#username admin secret Adm1nP@55
% Password too short - must be at least 12 characters. Password not configured.
R1(config)#username admin secret Adm1nP@55555
```

# Шаг 4. Активируйте протокол SSH на линиях VTY.

a.	Активируйте протоколы Telnet и SSH на входящих линиях VTY с помощью команды transport input.

b.	Измените способ входа в систему таким образом, чтобы использовалась проверка пользователей по локальной базе учетных записей.

# Шаг 5. Сохраните текущую конфигурацию в файл загрузочной конфигурации.

```R1(config)#line vty 0 15
R1(config-line)#tr
R1(config-line)#transport in
R1(config-line)#transport input ssh
R1(config-line)#log
R1(config-line)#login local
R1(config-line)#end
R1#
%SYS-5-CONFIG_I: Configured from console by console

R1#cop
R1#copy ru
R1#copy running-config st
R1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```

# Шаг 6. Установите соединение с маршрутизатором по протоколу SSH.

a.	Запустите Tera Term с PC-A.

b.	Установите SSH-подключение к R1. Use the username admin and password Adm1nP@55. У вас должно получиться установить SSH-подключение к R1.

```C:\>ssh -l admin 192.168.1.1

Password: 
% Login invalid


Password: 
% Password:  timeout expired!
% Login invalid

[Connection to 192.168.1.1 closed by foreign host]
C:\>ssh -l admin 192.168.1.1

Password: 



R1>
```

# Часть 3. Настройка коммутатора для доступа по протоколу SSH

В части 3 вам предстоит настроить коммутатор для приема подключений по протоколу SSH, а затем установить SSH-подключение с помощью программы Tera Term.

# Шаг 1. Настройте основные параметры коммутатора.

Откройте окно конфигурации

a.	Подключитесь к коммутатору с помощью консольного подключения и активируйте привилегированный режим EXEC.

b.	Войдите в режим конфигурации.

c.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

d.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

e.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

f.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

g.	Зашифруйте открытые пароли.

h.	Создайте баннер, который предупреждает о запрете несанкционированного доступа.

i.	Настройте и активируйте на коммутаторе интерфейс VLAN 1, используя информацию, приведенную в таблице адресации.

j.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

```S1(config)#no ip domain look
S1(config)#no ip domain lookup 
S1(config)#ena
S1(config)#enable sec
S1(config)#enable secret class
S1(config)#line con
S1(config)#line console 0
S1(config-line)#password ci
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#line vty 0 15
S1(config-line)#pass
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#ser
S1(config)#service pass
S1(config)#service password-encryption 
S1(config)#int vlan 1
S1(config-if)#ip add
S1(config-if)#ip address 192.168.1.11 255.255.255.0
S1(config-if)#no s
S1(config-if)#no sh
S1(config-if)#no shutdown 
S1(config-if)#exit
S1(config)#ip def
S1(config)#ip default-gateway 192.168.1.1
S1(config)#end
S1#
%SYS-5-CONFIG_I: Configured from console by console

S1#
```

# Шаг 2. Настройте коммутатор для соединения по протоколу SSH.

Для настройки протокола SSH на коммутаторе используйте те же команды, которые применялись для аналогичной настройки маршрутизатора в части 2.

a.	Настройте имя устройства, как указано в таблице адресации.

b.	Задайте домен для устройства.

c.	Создайте ключ шифрования с указанием его длины.

d.	Создайте имя пользователя в локальной базе учетных записей.

e.	Активируйте протоколы Telnet и SSH на линиях VTY.

f.	Измените способ входа в систему таким образом, чтобы использовалась проверка пользователей по локальной базе учетных записей.

```S1(config)#use
*Mar 1 2:30:52.843: %SSH-5-ENABLED: SSH 1.99 has been enabled
S1(config)#username admin secret Adm1nP@55555
S1(config)#line vty 0 15
S1(config-line)#tra
S1(config-line)#transport in
S1(config-line)#transport input te
S1(config-line)#transport input telnet ssh
                                       ^
% Invalid input detected at '^' marker.
	
S1(config-line)#transport input ssh
S1(config-line)#log
S1(config-line)#login local
S1(config-line)#end
S1#
%SYS-5-CONFIG_I: Configured from console by console

S1#copy runn
S1#copy running-config st
S1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
S1#
```

# Шаг 3. Установите соединение с коммутатором по протоколу SSH.

Запустите программу Tera Term на PC-A, затем установите подключение по протоколу SSH к интерфейсу SVI коммутатора S1.

```C:\>ssh -l admin 192.168.1.11

Password: 



S1>
S1>
```

# Часть 4. Настройка протокола SSH с использованием интерфейса командной строки (CLI) коммутатора

Клиент SSH встроен в операционную систему Cisco IOS и может запускаться из интерфейса командной строки. В части 4 вам предстоит установить соединение с маршрутизатором по протоколу SSH, используя интерфейс командной строки коммутатора.

# Шаг 1. Посмотрите доступные параметры для клиента SSH в Cisco IOS.

Откройте окно конфигурации

Используйте вопросительный знак (?), чтобы отобразить варианты параметров для команды ssh.
```S1# ssh? 
  -c Select encryption algorithm
  -l Log in using this user name
  -m Select HMAC algorithm
  -o Specify options
  -p Connect to this port
  -v Specify SSH Protocol Version
  -vrf Specify vrf name
  WORD IP-адрес или имя хоста удаленной системы
```

# Шаг 2. Установите с коммутатора S1 соединение с маршрутизатором R1 по протоколу SSH.

a.	Чтобы подключиться к маршрутизатору R1 по протоколу SSH, введите команду –l admin. Это позволит вам войти в систему под именем admin. При появлении приглашения введите в качестве пароля Adm1nP@55

```S1# ssh -l admin 192.168.1.1
Password: 
Authorized Users Only!
R1>
```

b.	Чтобы вернуться к коммутатору S1, не закрывая сеанс SSH с маршрутизатором R1, нажмите комбинацию клавиш Ctrl+Shift+6. Отпустите клавиши Ctrl+Shift+6 и нажмите x. Отображается приглашение привилегированного режима EXEC коммутатора.
R1>
S1#

c.	Чтобы вернуться к сеансу SSH на R1, нажмите клавишу Enter в пустой строке интерфейса командной строки. Чтобы увидеть окно командной строки маршрутизатора, нажмите клавишу Enter еще раз.
S1#
[Resuming connection 1 to 192.168.1.1 ... ]

R1>

d.	Чтобы завершить сеанс SSH на маршрутизаторе R1, введите в командной строке маршрутизатора команду exit.
R1# exit

[Connection to 192.168.1.1 closed by foreign host]
S1#

Вопрос:
Какие версии протокола SSH поддерживаются при использовании интерфейса командной строки?

Ответ:

Это можно определить с помощью ssh –v ? в командной строке. Коммутатор 2960 под управлением IOS версии 15.0(2) поддерживает SSH v1 и V2.

Вопрос для повторения:

Как предоставить доступ к сетевому устройству нескольким пользователям, у каждого из которых есть собственное имя пользователя?

Ответ:

Вы должны добавить имя пользователя и пароль каждого пользователя в локальную базу данных с помощью команды username. Также можно использовать сервер RADIUS или TACACS, но это еще не рассмотрено.
