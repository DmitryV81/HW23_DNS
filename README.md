Домашняя работа № 23. "Настраиваем Сплит ДНС"

Задание:

взять стенд https://github.com/erlong15/vagrant-bind

добавить еще один сервер client2

завести в зоне dns.lab

имена

web1 - смотрит на клиент1

web2 смотрит на клиент2

завести еще одну зону newdns.lab

завести в ней запись

www - смотрит на обоих клиентов

настроить split-dns

клиент1 - видит обе зоны, но в зоне dns.lab только web1

клиент2 видит только dns.lab

Ход работы.

Поднимаем стенд: vagrant up. Далее идет provision  помощью ansible

Проверяем работу split dns:

1 На клиенте client

```
[root@client ~]# ping www.newdns.lab
PING www.newdns.lab (192.168.50.15) 56(84) bytes of data.
64 bytes from client (192.168.50.15): icmp_seq=1 ttl=64 time=0.025 ms
64 bytes from client (192.168.50.15): icmp_seq=2 ttl=64 time=0.039 ms
64 bytes from client (192.168.50.15): icmp_seq=3 ttl=64 time=0.030 ms
^C
--- www.newdns.lab ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2013ms
rtt min/avg/max/mdev = 0.025/0.031/0.039/0.007 ms
[root@client ~]# ping web1.dns.lab
PING web1.dns.lab (192.168.50.15) 56(84) bytes of data.
64 bytes from client (192.168.50.15): icmp_seq=1 ttl=64 time=0.025 ms
64 bytes from client (192.168.50.15): icmp_seq=2 ttl=64 time=0.028 ms
^C
--- web1.dns.lab ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1003ms
rtt min/avg/max/mdev = 0.025/0.026/0.028/0.005 ms
[root@client ~]# ping web2.dns.lab
ping: web2.dns.lab: Name or service not known
```

2 На клиенте client2
```
[root@client2 ~]# ping www.newdns.lab
ping: www.newdns.lab: Name or service not known
[root@client2 ~]# ping web1.dns.lab
PING web1.dns.lab (192.168.50.15) 56(84) bytes of data.
64 bytes from 192.168.50.15 (192.168.50.15): icmp_seq=1 ttl=64 time=0.674 ms
64 bytes from 192.168.50.15 (192.168.50.15): icmp_seq=2 ttl=64 time=0.719 ms
^C
--- web1.dns.lab ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1003ms
rtt min/avg/max/mdev = 0.674/0.696/0.719/0.034 ms
[root@client2 ~]# ping web2.dns.lab
PING web2.dns.lab (192.168.50.16) 56(84) bytes of data.
64 bytes from client2 (192.168.50.16): icmp_seq=1 ttl=64 time=0.026 ms
64 bytes from client2 (192.168.50.16): icmp_seq=2 ttl=64 time=0.028 ms
64 bytes from client2 (192.168.50.16): icmp_seq=3 ttl=64 time=0.043 ms
^C
--- web2.dns.lab ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2005ms
rtt min/avg/max/mdev = 0.026/0.032/0.043/0.008 ms
```
