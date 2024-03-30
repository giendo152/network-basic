# Лабраторная работа - Настройка DHCPv6 

# Топология

![Image alt](https://github.com/giendo152/network-basic/blob/main/practice/pra8/1.png)

# Таблица адресации

| Устройство | Интерфейс	| IPv6-адрес	| 
| ---------------- |:------------------:| -----------------:|
| R1               |	G0/0/0	| 2001:db8:acad:2::1/64 |	
| R1               |	G0/0/1	| fe80::1 |	
| R2               |	G0/0/0	| 2001:db8:acad:2::2/64 |	
| R2               |	G0/0/0	| fe80::2 |	
| R2               |	G0/0/1	| 2001:db8:acad:3::1/64 |
| R2               |	G0/0/1	| fe80::1 |
| PC-A              |	NIC	| DHCP |	
| PC-B              |	NIC	| DHCP |	