# Лабораторная работа - Настройка NAT для IPv4

# Топология

![Image alt](https://github.com/giendo152/network-basic/blob/main/practice/pra12/1.png)

# Таблица адресации

| Устройство | interface| IP-адрес	| Маска подсети |
| ---------------- |:------------------:| -----------------:| -----------------:|
| R1               |	G0/0/0	| 209.165.200.230 |	255.255.255.248 |
| R1               |	G0/0/1 	| 192.168.1.1 |	255.255.255.0 |
| R2               |	G0/0/0	| 209.165.200.225 | 255.255.255.248 |	
| R2               |	Lo1	| 209.165.200.1 |	255.255.255.224 |
| S1             |	VLAN 1 	| 192.168.1.11|	255.255.255.0 |
| S2           |	VLAN 1	| 192.168.1.12|	255.255.255.0 |
| PC-A             |	NIC 	| 192.168.1.2|	255.255.255.0 |
| PC-B         |	NIC	| 192.168.1.3|	255.255.255.0 |

# Цели

# Часть 1. Создание сети и настройка основных параметров устройства

# Часть 2. Настройка и проверка NAT для IPv4

# Часть 3. Настройка и проверка PAT для IPv4

# Часть 4. Настройка и проверка статического NAT для IPv4.

# Инструкции

# Часть 1. Создание сети и настройка основных параметров устройства

В первой части лабораторной работы вам предстоит создать топологию сети и настроить базовые параметры для узлов ПК и коммутаторов.

# Шаг 1. Подключите кабели сети согласно приведенной топологии.

Подключите устройства в соответствии с топологией и подсоедините соответствующие кабели.

# Шаг 2. Произведите базовую настройку маршрутизаторов.

Откройте окно конфигурации

a.	Назначьте маршрутизатору имя устройства.

```
router(config)# hostname R1
```

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким 
образом, как будто они являются именами узлов.

```
R1(config)# no ip domain-lookup
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

h.	Настройте IP-адресации интерфейса, как указано в таблице выше.

```
R1(config)# interface g0/0/0
R1(config-if)# ip address 209.165.200.230 255.255.255.248
R1(config-if)# no shutdown
R1(config-if)# interface g0/0/1
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit
```

```
R2(config)# interface g0/0/0
R2(config-if)# ip address 209.165.200.225 255.255.255.248
R2(config-if)# no shutdown
R2(config-if)# interface loopback 1
R2(config-if)# ip address 209.165.200.1 255.255.255.224
R2(config-if)# no shutdown
R2(config-if)# end
```

i.	Настройте маршрут по умолчанию. от R2 до  R1.

```
R1(config)# ip route 0.0.0.0 0.0.0.0 209.165.200.225
```

j.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

```
R1(config)# exit
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

h.	Выключите все интерфейсы, которые не будут использоваться.

```
S1(config)# interface range f0/2-4, f0/7-24, g0/1-2
S1(config-if-range)# shutdown
```
```
S2(config)# interface range f0/2-17, f0/19-24, g0/1-2
S2(config-if-range)# shutdown
```

i.	Настройте IP-адресации интерфейса, как указано в таблице выше.

```
S1(config)# interface vlan 1
S1(config-if)# ip address 192.168.1.11 255.255.255.0
S1(config-if)# no shutdown
S1(config-if)# exit
S1(config)# ip default-gateway 192.168.1.1
```
```
S2(config)# interface vlan 1
S2(config-if)# ip address 192.168.1.12 255.255.255.0
S2(config-if)# no shutdown
S2(config-if)# exit
S2(config)# ip default-gateway 192.168.1.1
S2(config)# end
```

j.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

```
S1# copy running-config startup-config
```

Закройте окно настройки.

# Часть 2. Настройка и проверка NAT для IPv4.

В части 2 необходимо настроить и проверить NAT для IPv4.

# Шаг 1. Настройте NAT на R1, используя пул из трех адресов 209.165.200.226-209.165.200.228. 

Откройте окно конфигурации

a.	Настройте простой список доступа, который определяет, какие хосты будут разрешены для трансляции. В этом случае все устройства в локальной сети R1 имеют право на трансляцию.

```
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255 
```

b.	Создайте пул NAT и укажите ему имя и диапазон используемых адресов.

```
R1(config)# ip nat pool PUBLIC_ACCESS 209.165.200.226 209.165.200.228 netmask 255.255.255.248 
```

Примечание. Параметр маски сети не является разделителем IP-адресов. Это должна быть правильная маска подсети для назначенных адресов, даже если вы используете не все адреса подсети в пуле. 

c.	Настройте перевод, связывая ACL и пул с процессом преобразования.

```
R1(config)# ip nat inside source list 1 pool PUBLIC_ACCESS 
```

Примечание: Три очень важных момента. Во-первых, слово «inside» имеет решающее значение для работы такого рода NAT. Если вы опустить его, NAT не будет работать. Во-вторых, номер списка — это номер ACL, настроенный на предыдущем шаге. В-третьих, имя пула чувствительно к регистру. 

d.	Задайте внутренний (inside) интерфейс. 

```
R1(config)# interface g0/0/1
R1(config-if)# ip nat inside
```

e.	Определите внешний (outside) интерфейс.

```
R1(config)# interface g0/0/0
R1(config-if)# ip nat outside
```

# Шаг 2. Проверьте и проверьте конфигурацию. 

a.	С PC-B,  запустите эхо-запрос интерфейса Lo1 (209.165.200.1) на R2. Если эхо-запрос не прошел, выполните процес поиска и устранения неполадок. На R1 отобразите таблицу NAT на R1 с помощью команды show ip nat translations.

```
R1# show ip nat translations
Pro Inside global Inside local Outside local Outside global
--- 209.165.200.226 192.168.1.3 --- --- 
226:1 192.168.1. 3:1 209.165.200. 1:1 209.165.200. 1:1 
Total number of translations: 2
```

Вопросы:

Во что был транслирован внутренний локальный адрес PC-B?
Введите ваш ответ здесь.

Ответ:

209.165.200.226

Какой тип адреса NAT является переведенным адресом?

Ответ:

Внутри Глобального

b.	С PC-A, запустите  эхо-запрос интерфейса Lo1 (209.165.200.1) на R2. Если эхо-запрос не прошел, выполните отладку. На R1 отобразите таблицу NAT на R1 с помощью команды show ip nat translations.

```
R1# show ip nat translations 
Pro Inside global Inside local Outside local Outside global
--- 209.165.200.227 192.168.1.2 --- ---
--- 209.165.200.226 192.168.1.3 --- ---
227:1 192.168.1. 2:1 209.165.200. 1:1 209.165.200. 1:1
226:1 192.168.1. 3:1 209.165.200. 1:1 209.165.200. 1:1
Total number of translations: 4
```

c.	Обратите внимание, что предыдущая трансляция для PC-B все еще находится в таблице. Из S1, эхо-запрос интерфейса Lo1 (209.165.200.1) на R2. Если эхо-запрос не прошел, выполните отладку. На R1 отобразите таблицу NAT на R1 с помощью команды show ip nat translations.

```
R1# show ip nat translations
Pro Inside global Inside local Outside local Outside global
--- 209.165.200.227 192.168.1.2 --- ---
--- 209.165.200.226 192.168.1.3 --- ---
--- 209.165.200.228 192.168.1.11 --- ---
226:1 192.168.1. 3:1 209.165.200. 1:1 209.165.200. 1:1
228:0 192.168.1. 11:0 209.165.200. 1:0 209.165.200. 1:0 209.165.200. 1:0
Total number of translations: 5
```

d.	Теперь запускаем пинг R2 Lo1 из S2. На этот раз перевод завершается неудачей, и вы получаете эти сообщения (или аналогичные) на консоли R1:
Sep 23 15:43:55.562: %IOSXE-6-PLATFORM: R0/0: cpp_cp: QFP:0.0 Thread:000 TS:00000001473688385900 %NAT-6-ADDR_ALLOC_FAILURE: Address allocation failed; pool 1 may be exhausted [2]

e.	Это ожидаемый результат, потому что выделено только 3 адреса, и мы попытались ping Lo1 с четырех устройств. Напомним, что NAT — это трансляция «один-в-один». Как много выделено трансляций? Введите команду show ip nat translations verbose , и вы увидите, что ответ будет 24 часа.

```
R1# show ip nat translations verbose 
Pro Inside global Inside local Outside local Outside global
--- 209.165.200.226 192.168.1.3 --- ---
  create: 09/23/19 15:35:27, use: 09/23/19 15:35:27, timeout: 23:56:42
  Map-Id(In): 1
<output omitted>
```

f.	Учитывая, что пул ограничен тремя адресами, NAT для пула адресов недостаточно для нашего приложения. Очистите преобразование NAT и статистику, и мы перейдем к PAT.

```
R1# clear ip nat translations * 
R1# clear ip nat statistics 
```

Закройте окно настройки.

# Часть 3. Настройка и проверка PAT для IPv4.

В части 3 необходимо настроить замену NAT на PAT в пул адресов, а затем на PAT с помощью интерфейса.

# Шаг 1. Удалите команду преобразования на R1.

Откройте окно конфигурации

Компоненты конфигурации преобразования адресов в основном одинаковы; что-то (список доступа) для идентификации адресов, пригодных для перевода, дополнительно настроенный пул адресов для их преобразования и команды, необходимые для идентификации внутреннего и внешнего интерфейсов. Из части 1 наш список доступа (список доступа 1) по-прежнему корректен для сетевого сценария, поэтому нет необходимости воссоздавать его. Мы будем использовать один и тот же пул адресов, поэтому нет необходимости воссоздавать эту конфигурацию. Кроме того, внутренний и внешний интерфейсы не меняются. Чтобы начать работу в части 3, удалите команду, связывающую ACL и пул вместе.

```
R1(config)# no ip nat inside source list 1 pool PUBLIC_ACCESS 
```

# Шаг 2. Добавьте команду PAT на R1.

Теперь настройте преобразование PAT в пул адресов (помните, что ACL и Pool уже настроены, так что это единственная команда, которую нам нужно изменить с NAT на PAT).

```
R1(config)# ip nat inside source list 1 pool PUBLIC_ACCESS overload 
```

# Шаг 3. Протестируйте и проверьте конфигурацию.

a.	Давайте проверим, что PAT работает. С PC-B,  запустите эхо-запрос интерфейса Lo1 (209.165.200.1) на R2. Если эхо-запрос не прошел, выполните отладку. На R1 отобразите таблицу NAT на R1 с помощью команды show ip nat translations.

```
R1# show ip nat translations
Pro Inside global Inside local Outside local Outside global
226:1 192.168.1. 3:1 209.165.200. 1:1 209.165.200. 1:1
Total number of translations: 1#
```

Вопросы:

Во что был транслирован внутренний локальный адрес PC-B?

Ответ:

209.165.200.226

Какой тип адреса NAT является переведенным адресом?

Ответ:

Внутри Глобального

Чем отличаются выходные данные команды show ip nat translations из упражнения NAT?

Ответ:

Ответы могут быть разными. В списке нет специального перевода между внутренним и внешним адресами.
 
b.	С PC-A, запустите эхо-запрос интерфейса Lo1 (209.165.200.1) на R2. Если эхо-запрос не прошел, выполните отладку. На R1 отобразите таблицу NAT на R1 с помощью команды show ip nat translations.

```
R1# show ip nat translations
Pro Inside global Inside local Outside local Outside global
226:1 192.168.1. 2:1 209.165.200. 1:1 209.165.200. 1:1
Total number of translations: 1
```

Обратите внимание, что есть только одна трансляция. Отправьте ping еще раз, и быстро вернитесь к маршрутизатору и введите команду show ip nat translations verbose , и вы увидите, что произошло.

```
R1# show ip nat translations verbose 
Pro Inside global Inside local Outside local Outside global
icmp 209.165.200.226:1 192.168.1.2:1 209.165.200.1:1 209.165.200.1:1 
  create: 09/23/19 16:57:22, use: 09/23/19 16:57:25, timeout: 00:01:00
<output omitted>
```

Как вы можете видеть, время ожидания перевода было отменено с 24 часов до 1 минуты.

c.	Генерирует трафик с нескольких устройств для наблюдения PAT. На PC-A и PC-B используйте параметр -t с командой ping, чтобы отправить безостановочный ping на интерфейс Lo1 R2 (ping -t 209.165.200.1), затем вернитесь к R1 и выполните команду show ip nat translations:

```
R1# show ip nat translations
Pro Inside global Inside local Outside local Outside global
icmp 209.165.200.226:1 192.168.1.2:1 209.165.200.1:1 209.165.200.1:1 
226:2 192.168.1. 3:1 209.165.200. 1:1 209.165.200. 1:2 
Total number of translations: 2 
```

Обратите внимание, что внутренний глобальный адрес одинаков для обоих сеансов. 

Вопрос:

Как маршрутизатор отслеживает, куда идут ответы? 

Ответ:

Присваиваются уникальные номера портов

d.	PAT в пул является очень эффективным решением для малых и средних организаций. Тем не менее есть неиспользуемые адреса IPv4, задействованные в этом сценарии. Мы перейдем к PAT с перегрузкой интерфейса, чтобы устранить эту трату IPv4 адресов. Остановите ping на PC-A и PC-B с помощью комбинации клавиш Control-C, затем очистите трансляции и статистику:

```
R1# clear ip nat translations * 
R1# clear ip nat statistics 
```

# Шаг 4. На R1 удалите команды преобразования nat pool.

Опять же, наш список доступа (список доступа 1) по-прежнему корректен для сетевого сценария, поэтому нет необходимости воссоздавать его. Кроме того, внутренний и внешний интерфейсы не меняются. Чтобы начать работу с PAT к интерфейсу, очистите конфигурацию, удалив пул NAT и команду, связывающую ACL и пул вместе.

```
R1(config)# no ip nat inside source list 1 pool PUBLIC_ACCESS overload 
R1(config)# no ip nat pool PUBLIC_ACCESS
```

# Шаг 5. Добавьте команду PAT overload, указав внешний интерфейс.

Добавьте команду PAT, которая вызовет перегрузку внешнего интерфейса.

R1(config)# ip nat inside source list 1 interface g0/0/0 overload 

# Шаг 6. Протестируйте и проверьте конфигурацию. 

a.	Давайте проверим PAT, чтобы интерфейс работал. С PC-B,  запустите эхо-запрос интерфейса Lo1 (209.165.200.1) на R2. Если эхо-запрос не прошел, выполните отладку. На R1 отобразите таблицу NAT на R1 с помощью команды show ip nat translations.

```
R1# show ip nat translations
Pro Inside global Inside local Outside local Outside global
209.165.200. 230:1 192.168.1. 3:1 209.165.200. 1:1 209.165.200. 1:1 
Total number of translations: 1 
```

b.	Сделайте трафик с нескольких устройств для наблюдения PAT. На PC-A и PC-B используйте параметр -t с командой ping для отправки безостановочного ping на интерфейс Lo1 R2 (ping -t 209.165.200.1). На S1 и S2 выполните привилегированную команду exec ping 209.165.200.1 повторить 2000. Затем вернитесь к R1 и выполните команду show ip nat translations.

```
R1# show ip nat translations
Pro Inside global Inside local Outside local Outside global
209.165.200. 230:3 192.168.1. 11:1 209.165.200. 1:1 209.165.200. 1:3 
209.165.200. 230:2 192.168.1. 2:1 209.165.200. 1:1 209.165.200. 1:2 
209.165.200. 230:4 192.168.1. 3:1 209.165.200. 1:1 209.165.200. 1:4 
209.165.200. 230:1 192.168.1. 12:1 209.165.200. 1:1 209.165.200. 1:1 
Total number of translations: 4 
```

Теперь все внутренние глобальные адреса сопоставляются с IP-адресом интерфейса g0/0/0.

Остановите все пинги. На PC-A и PC-B, используя комбинацию клавиш CTRL-C.

Закройте окно настройки.

# Часть 4. Настройка и проверка статического NAT для IPv4.

В части 4 будет настроена статическая NAT таким образом, чтобы PC-A был доступен напрямую из Интернета. PC-A будет доступен из R2 по адресу 209.165.200.229.

Примечание. Конфигурация, которую вы собираетесь завершить, не соответствует рекомендуемым практикам для шлюзов, подключенных к Интернету. Эта лаборатория полностью опускает стандартные методы безопасности, чтобы сосредоточиться на успешной конфигурации статического NAT. В производственной среде решающее значение для удовлетворения этого требования будет иметь тщательная координация между сетевой инфраструктурой и группами безопасности. 

# Шаг 1. На R1 очистите текущие трансляции и статистику.

Откройте окно конфигурации

```
R1# clear ip nat translations * 
R1# clear ip nat statistics 
```

# Шаг 2. На R1 настройте команду NAT, необходимую для статического сопоставления внутреннего адреса с внешним адресом.

Для этого шага настройте статическое сопоставление между 192.168.1.11 и 209.165.200.1 с помощью следующей команды:

R1(config)# ip nat inside source static 192.168.1.2 209.165.200.229 

# Шаг 3. Протестируйте и проверьте конфигурацию.

a.	Давайте проверим, что статический NAT работает. На R1 отобразите таблицу NAT на R1 с помощью команды show ip nat translations, и вы увидите статическое сопоставление.

```
R1# show ip nat translations
Pro Inside global Inside local Outside local Outside global
--- 209.165.200.229 192.168.1.2 --- ---
Total number of translations: 1
```

b.	Таблица перевода показывает, что статическое преобразование действует. Проверьте это, запустив ping  с R2 на 209.165.200.229. Плинги должны работать.

Примечание. Возможно, вам придется отключить брандмауэр ПК для работы pings.

c.	На R1 отобразите таблицу NAT на R1 с помощью команды show ip nat translations, и вы увидите статическое сопоставление и преобразование на уровне порта для входящих pings.

```
R1# show ip nat translations
Pro Inside global Inside local Outside local Outside global
--- 209.165.200.229 192.168.1.2 --- ---
229:3 192.168.1. 2:3 209.165.200. 225:3 209.165.200. 225:3 209.165.200. 
Total number of translations: 2
```

Это подтверждает, что статический NAT работает.
