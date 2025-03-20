# OTUS.Lesson28.Network
Архитектура сетей

Тестовый стенд 7 VM (2CPU, 2Gb RAM, 6Gb HDD, OS Ubuntu 24.04s)


Домашнее задание:

1. Скачать и развернуть Vagrant-стенд https://github.com/erlong15/otus-linux/tree/network
2. Построить следующую сетевую архитектуру:
Сеть office1
- 192.168.2.0/26      - dev
- 192.168.2.64/26     - test servers
- 192.168.2.128/26    - managers
- 192.168.2.192/26    - office hardware

Сеть office2
- 192.168.1.0/25      - dev
- 192.168.1.128/26    - test servers
- 192.168.1.192/26    - office hardware

Сеть central
- 192.168.0.0/28     - directors
- 192.168.0.32/28    - office hardware
- 192.168.0.64/26    - wifi

Теоретическая часть:
свободные сети:
192.168.0.16/28
192.168.0.48/28
192.168.0.128/25

192.168.255.8/29
192.168.255.16/28
192.168.255.32/27
192.168.255.64/26
192.168.255.128/25

Практическа часть:
Примечание к плейбуку: В этом задании я не использовал скрипт для восстановления правил фаервола при запуске, вместо этого использовалась утилита iptables-persistent.

Запускаем плейбук netowrk.yml и ждем пока он отработает
Далее с роутера inetrouter подключаемся через ssh к нашим серверам и проверяем доступность google-dns

zhurkin@centralserver:~$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=103 time=23.9 ms
^C
--- 8.8.8.8 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 23.886/23.886/23.886/0.000 ms
zhurkin@centralserver:~$ ip r
default via 192.168.0.1 dev ens19 proto static 
10.200.3.0/24 dev ens18 proto kernel scope link src 10.200.3.223 
192.168.0.0/28 dev ens19 proto kernel scope link src 192.168.0.2 



zhurkin@office1server:~$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=102 time=24.2 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=102 time=24.1 ms
^C
--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 24.070/24.134/24.198/0.064 ms
zhurkin@office1server:~$ ip r
default via 192.168.2.129 dev ens19 proto static 
10.200.3.0/24 dev ens18 proto kernel scope link src 10.200.3.226 
192.168.2.128/26 dev ens19 proto kernel scope link src 192.168.2.130 


zhurkin@office2server:~$ ip r 
default via 192.168.1.1 dev ens19 proto static 
10.200.3.0/24 dev ens18 proto kernel scope link src 10.200.3.227 
192.168.1.0/26 dev ens19 proto kernel scope link src 192.168.1.2 
zhurkin@office2server:~$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=102 time=24.3 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=102 time=24.1 ms
^C
--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 24.111/24.228/24.346/0.117 ms