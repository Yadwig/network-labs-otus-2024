# Домашнее задание 3

  

###Implement DHCPv4

  
  

#### Лабораторная среда

В EVE NG создана лабораторная с устройствами Cisco IOS в следующей конфигурации:

![enter image description here](https://github.com/Yadwig/network-labs-otus-2024/blob/main/homework-3/pictures/topology.png?raw=true)

Конфиги устройств перед выложены в [гитхаб](https://github.com/Yadwig/network-labs-otus-2024/tree/main/homework-3/config).

  

Подсеть 192.168.1.0/24 разбита на подсети следующим образом:

1) “Подсеть A” должна поддерживать 58 хостов ( client VLAN на R1).

192.168.1.0/26
Первый адрес подсети назначен для R1 Gi0/1.100

2), “Подсеть B” должна поддерживать 28 хостов  ( management VLAN на R1).

192.168.1.64/27
Первый адрес назначен для R1 Gi0/1.200. Второй -- на S1 VLAN 200 (шлюз по умолчанию R1 Gi0/1.200)

3) “Подсеть C” должна поддерживать 12 хостов (client на R2).
192.168.1.96/28

Итоговая таблица адресации:

![](https://github.com/Yadwig/network-labs-otus-2024/blob/main/homework-3/pictures/adressing-table.png?raw=true)

#### Проделанная работа

1) На интерфейсах свичей созданы интерфейсы, в которые помещены следующие порты:

VLAN  | Name | Interface Assigned

1 | N/A | S2: Gi0/2

100 | Clients | S1: Gi0/2

200 | Management | S1: VLAN 200

999 | Parking_Lot | “все остальные” на S1

1000 | Native | N/A (на S2 native остаётся 1)

2) CПрисвоены ip как на таблице сверху.

3) На роутере R1 настроен dhcp:

1. dhcp пул Clients (для подключенных к s1)

2. dhcp пул Clients2 (для подключенных к s2)

Одинаковое время резервирование адреса в 3 дня, 12 часов, 30 минут.

![](https://github.com/Yadwig/network-labs-otus-2024/blob/main/homework-3/pictures/show-ip-dhcp-pool.png?raw=true)

На Linux1 сразу по dhcp получен ip:

![](https://github.com/Yadwig/network-labs-otus-2024/blob/main/homework-3/pictures/linux1-dhcp.png?raw=true)

Чтобы Linux2 получил ip, на R2 настраивается dhcp relay (ip helper-address 10.0.0.1 ), который пересылает broadcast dhcp-запрос клиента на dhcp-сервер по указанному адресу.
В итоге на R1 видим получивших адреса клиентов.

![](https://github.com/Yadwig/network-labs-otus-2024/blob/main/homework-3/pictures/show-ip-dhcp-binding.png?raw=true)



## #### Вопрос по работе

  

На S1 случайно было настроено следующее (результаты из show run):

![](https://github.com/Yadwig/network-labs-otus-2024/blob/main/homework-3/pictures/%D0%B2%D0%BE%D0%BF%D1%80%D0%BE%D1%811.png?raw=true)

Сначала была задана команда switchport access vlan 999, а потом команда switchport trunk.

S1 выполняет свою задачу, и linux1 подключилась без проблем. Но разве команда switchport trunk не должна была заменить настройки switchport access?