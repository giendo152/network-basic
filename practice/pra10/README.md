# Лабораторная работа. Настройка протокола OSPFv2 для одной области

# Топология

![Image alt](https://github.com/giendo152/network-basic/blob/main/practice/pra10/1.png)

# Таблица адресации

| Устройство | interface | IP-адрес	| Маска подсети |
| ---------------- |:------------------:| -----------------:| -----------------:|
| R1               |	G0/0/1	| 10.53.0.1 |	255.255.255.0 |
| R1               |	Loopback1	| 172.16.1.1 |	255.255.255.0 |
| R2               |	G0/0/1	| 10.53.0.2 |	255.255.255.0 |
| R2               |	Loopback1	| 192.168.1.1 |	255.255.255.0 |

# Цели

# Часть 1. Создание сети и настройка основных параметров устройства

# Часть 2. Настройка и проверка базовой работы протокола  OSPFv2 для одной области

# Часть 3. Оптимизация и проверка конфигурации OSPFv2 для одной области

# Инструкции

# Часть 1. Создание сети и настройка основных параметров устройства

# Шаг 1. Создайте сеть согласно топологии.

Подключите устройства, как показано в топологии, и подсоедините необходимые кабели.

# Шаг 2. Произведите базовую настройку маршрутизаторов.

Откройте окно конфигурации

a.	Назначьте маршрутизатору имя устройства.

```
router(config)# hostname R1
router(config)# hostname R2
```

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

```
R1(config)# no ip domain lookup
R2(config)# no ip domain lookup
```
c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

```
R1(config)# enable secret class
R2(config)# enable secret class
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

# Шаг 3. Настройте базовые параметры каждого коммутатора.

a.	Назначьте коммутатору имя устройства.

```
switch(config)# hostname S1

switch(config)# hostname S2
```

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

```
S1(config)# no ip domain lookup
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

# Часть 2. Настройка и проверка базовой работы протокола OSPFv2 для одной области

# Шаг 1. Настройте адреса интерфейса и базового OSPFv2 на каждом маршрутизаторе.

a.	Настройте адреса интерфейсов на каждом маршрутизаторе, как показано в таблице адресации выше.

Откройте окно конфигурации

```
R1(config)# interface g0/0/1
R1(config-if)# ip address 10.53.0.1 255.255.255.0
R1(config-if)# no shut
R1(config-if)# exit
R1(config)# interface loopback 1
R1(config-if)# ip address 172.16.1.1 255.255.255.0
R1(config-if)# exit

R2(config)# interface g0/0/1
R2(config-if)# ip address 10.53.0.2 255.255.255.0
R2(config-if)# no shut
R2(config-if)# exit
R2(config)# interface loopback 1
R2(config-if)# ip address 192.168.1.1 255.255.255.0
R2(config-if)# exit
```

b.	Перейдите в режим конфигурации маршрутизатора OSPF, используя идентификатор процесса 56.

```
R1(config)# router ospf 56
```

c.	Настройте статический идентификатор маршрутизатора для каждого маршрутизатора (1.1.1.1 для R1, 2.2.2.2 для R2).

```
R1(config-router)# router-id 1.1.1.1

R2(config-router)# router-id 2.2.2.2
```

d.	Настройте инструкцию сети для сети между R1 и R2, поместив ее в область 0.

```
R1(config-router)# network 10.53.0.0 0.0.0.255 area 0

R2(config-router)# network 10.53.0.0 0.0.0.255 area 0
```

e.	Только на R2 добавьте конфигурацию, необходимую для объявления сети Loopback 1 в область OSPF 0.

```
R2(config-router)# network 192.168.1.0 0.0.0.255 area 0
```

f.	Убедитесь, что OSPFv2 работает между маршрутизаторами. Выполните команду, чтобы убедиться, что R1 и R2 сформировали смежность.

```
R1# show ip ospf neighbor
Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           1   FULL/DR         00:00:33    10.53.0.2       GigabitEthernet0/0/1

R2# show ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface
1.1.1.1           1   FULL/BDR        00:00:37    10.53.0.1       GigabitEthernet0/0/1
```


Вопрос:

Какой маршрутизатор является DR? Какой маршрутизатор является BDR? Каковы критерии отбора?

Ответ:

Ответы будут разными. В этом примере R1 был настроен первым и использовал OSPF перед R2. Итак, во время выбора OSPF только R1 был настроен для OSPF и стал DR. После того, как R2 был настроен для OSPF, он стал BDR в гигабитном сегменте. Маршрутизатор с наибольшим идентификатором маршрутизатора используется при выборе DR и BDR.

g.	На R1 выполните команду show ip route ospf, чтобы убедиться, что сеть R2 Loopback1 присутствует в таблице маршрутизации. Обратите внимание, что поведение OSPF по умолчанию заключается в объявлении интерфейса обратной связи в качестве маршрута узла с использованием 32-битной маски.

```
R1# show ip route ospf
<output omitted>
Gateway of last resort is not set
 192.168.1.0/32 is subnetted, 1 subnets
O 192.168.1.1 [110/2] via 10.53.0.2, 00:03:12, GigabitEthernet0/0/1
```

h.	Запустите Ping до  адреса интерфейса R2 Loopback 1 из R1. Выполнение команды ping должно быть успешным.

```
R1# ping 192.168.1.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```

# Часть 3. Оптимизация и проверка конфигурации OSPFv2 для одной области

# Шаг 1. Реализация различных оптимизаций на каждом маршрутизаторе.

Откройте окно конфигурации

a.	На R1 настройте приоритет OSPF интерфейса G0/0/1 на 50, чтобы убедиться, что R1 является назначенным маршрутизатором.

```
R1(config)# interface g0/0/1
R1(config-if)# ip ospf priority 50
```

b.	Настройте таймеры OSPF на G0/0/1 каждого маршрутизатора для таймера приветствия, составляющего 30 секунд.

```
R1(config)# interface g0/0/1
R1(config-if)# ip ospf hello-interval 30

R2(config)# interface g0/0/1
R2(config-if)# ip ospf hello-interval 30
```

c.	На R1 настройте статический маршрут по умолчанию, который использует интерфейс Loopback 1 в качестве интерфейса выхода. Затем распространите маршрут по умолчанию в OSPF. Обратите внимание на сообщение консоли после установки маршрута по умолчанию.

```
R1(config)# ip route 0.0.0.0 0.0.0.0 loopack 1
%Default route without gateway, if not a point-to-point interface, may impact performance
R1(config)# router ospf 56
R1(config-router)# default-information originate
```

d.	добавьте конфигурацию, необходимую для OSPF для обработки R2 Loopback 1 как сети точка-точка. Это приводит к тому, что OSPF объявляет Loopback 1 использует маску подсети интерфейса.

```
R2(config)# interface loopback 1
R2(config-if)# ip ospf network point-to-point
R2(config-if)# exit
```

e.	Только на R2 добавьте конфигурацию, необходимую для предотвращения отправки объявлений OSPF в сеть Loopback 1.

```
R2(config)# router ospf 56
R2(config-router)# passive-interface loopback 1
R2(config-router)# exit
```

f.	Измените базовую пропускную способность для маршрутизаторов. После этой настройки перезапустите OSPF с помощью команды clear ip ospf process . Обратите внимание на сообщение консоли после установки новой опорной полосы пропускания.

```
R1(config)# router ospf 56
R1(config-router)# auto-cost reference-bandwidth 1000
%OSPF: Reference bandwidth is changed.
Please ensure reference bandwidth is consistent across all routers.
R1(config-router)# end
R1# clear ip ospf process
Reset ALL OSPF processes? [no]: yes

R2(config)# router ospf 56
R2(config-router)# auto-cost reference-bandwidth 1000
%OSPF: Reference bandwidth is changed.
Please ensure reference bandwidth is consistent across all routers.
R2(config-router)# end
R2# clear ip ospf process
Reset ALL OSPF processes? [no]: yes
```

# Шаг 2. Убедитесь, что оптимизация OSPFv2 реализовалась.

a.	Выполните команду show ip ospf interface g0/0/1 на R1 и убедитесь, что приоритет интерфейса установлен равным 50, а временные интервалы — Hello 30, Dead 120, а тип сети по умолчанию — Broadcast

```
R1#show ip ospf interface g0/0/1

GigabitEthernet0/0/1 is up, line protocol is up
  Internet address is 10.53.0.1/24, Area 0
  Process ID 56, Router ID 1.1.1.1, Network Type BROADCAST, Cost: 10
  Transmit Delay is 1 sec, State DR, Priority 50
  Designated Router (ID) 1.1.1.1, Interface address 10.53.0.1
  Backup Designated Router (ID) 2.2.2.2, Interface address 10.53.0.2
  Timer intervals configured, Hello 30, Dead 40, Wait 40, Retransmit 5
    Hello due in 00:00:24
  Index 1/1, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 1, Adjacent neighbor count is 1
    Adjacent with neighbor 2.2.2.2  (Backup Designated Router)
  Suppress hello for 0 neighbor(s)
R1#
```

b.	На R1 выполните команду show ip route ospf, чтобы убедиться, что сеть R2 Loopback1 присутствует в таблице маршрутизации. Обратите внимание на разницу в метрике между этим выходным и предыдущим выходным. 
Также обратите внимание, что маска теперь составляет 24 бита, в отличие от 32 битов, ранее объявленных.

```
R1#show ip route ospf
O    192.168.1.0 [110/10] via 10.53.0.2, 00:01:08, GigabitEthernet0/0/1

R1#
```

c.	Введите команду show ip route ospf на маршрутизаторе R2. Единственная информация о маршруте OSPF должна быть распространяемый по умолчанию маршрут R1.

```
R2# show ip route ospf
<output omitted>
Gateway of last resort is 10.53.0.1 to network 0.0.0.0
O*E2  0.0.0.0/0 [110/1] via 10.53.0.1, 00:08:01, GigabitEthernet0/0/1
```

d.	Запустите Ping до адреса интерфейса R1 Loopback 1 из R2. Выполнение команды ping должно быть успешным.

```
R2# ping 172.16.1.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```

Вопрос:

Почему стоимость OSPF для маршрута по умолчанию отличается от стоимости OSPF в R1 для сети 192.168.1.0/24?

Ответ:

Статическому маршруту по умолчанию, импортированному в OSPF, по умолчанию присваивается тип показателя “E2” или внешний тип 2. Значение “E2” по умолчанию сохраняет одинаковую стоимость OSPF во всей сети OSPF. В этом случае метрика для маршрута по умолчанию была равна 1, поэтому он имеет метрику, равную 1, везде в сети OSPF 56. Сеть 192.168.1.0 / 24 представляет собой внутренний маршрут OSPF, показатель которого является кумулятивным.
