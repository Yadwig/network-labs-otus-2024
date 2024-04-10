## Домашнее задание 1

###  Configure Router-on-a-Stick Inter-VLAN Routing

#### Лабораторная среда 
Установлена EVE NG в виртуальной среде на VMware Workstation Player.
Создана лабораторная с устройствами в следующей конфигурации:
![scheme](https://github.com/Yadwig/network-labs-otus-2024/blob/main/homework-1/pictures/eve-scheme.png?raw=true)

Линукс представлен ОС Debian. Остальные устройства Cisco IOS.
Итоговые конфиги после всех действий выложены в [гитхаб](https://github.com/Yadwig/network-labs-otus-2024/tree/main/homework-1/config).

  
#### Проделанная работа
1) Устройствам Cisco присвоены hostname в соответствии с обозначениями на картине (R1, S1 и т.д.).

Заданы логин/пароль для enable.
Отключен dns lookup: no ip domain-lookup.
Задан баннер: banner motd #Unauthorized access to this device is prohibited!#
Задана временная зона и время: clock timezone GMT +3 и clock set

2) Заданы следующие ip на интерфейсы:

Linux1 – 192.168.3.3 – gateway 192.168.3.1
Linux2 – 192.168.4.3 – gateway 192.168.4.1

3) Созданы VLAN.

На S1 во vlan 3 (Management) помещен порт Gi0/2, создан vlan 4 (Operations).
На S2 во vlan 4 (Operations) помещен порт Gi0/1, cоздан vlan 3 (Management).
На обоих свичах создан vlan 7, в который помещены все незанятые порты. Потом незанятые порты административно выключены.

![sh-vlan-brief](https://github.com/Yadwig/network-labs-otus-2024/blob/main/homework-1/pictures/sh-vlan-brief.png?raw=true)

 
4) Созданы транки: от S1 до S2 и от S1 до R2. При создании транков указан нейтив vlan 8.

![sh-int-trunk](https://github.com/Yadwig/network-labs-otus-2024/blob/main/homework-1/pictures/sh-int-trunk.png?raw=true)

  
4) На роутере созданы 3 сабинтерфейса: для VLAN 3, 4 и native VLAN 8.

![sh-ip-int-brief-r1](https://github.com/Yadwig/network-labs-otus-2024/blob/main/homework-1/pictures/sh-ip-int-brief-r1.png?raw=true)

Созданы интерфейсы для управления роутерами:  
S1 - vlan3 – 192.168.3.11
S2 - vlan3 – 192.168.3.12
Для свичей задан шлюз по умолчанию 192.168.3.1 (ip default-gateway 192.168.3.1)

![sh-ip-int-brief-s1-s2](https://github.com/Yadwig/network-labs-otus-2024/blob/main/homework-1/pictures/sh-ip-int-brief-s1-s2.png?raw=true)
 
  

####  Итог

1) Linux1 пингует R1 (default gateway) по 192.168.3.1 и R2 по 192.168.3.12

![enter image description here](https://github.com/Yadwig/network-labs-otus-2024/blob/main/homework-1/pictures/ping-from-linux1.png?raw=true)

2) Linux2 пингует R1 по 192.168.3.1 и Linux1 по 192.168.3.3

![enter image description here](https://github.com/Yadwig/network-labs-otus-2024/blob/main/homework-1/pictures/ping-from-linux2.png?raw=true)
