<h1> Лабораторная работа. Просмотр таблицы MAC-адресов коммутатора </h1> 

<h2> Топология </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab01/topology.png>

<h2> Таблица адресации </h2>

| Устройство | Интерфейс | IP-адрес / префикс |
|:----------:|:---------:|:------------------:|
| S1         | VLAN 1    | 192.168.1.11 /24   |
| S2         | VLAN 1    | 192.168.1.12 /24   |
| PC-A       | NIC       | 192.168.1.1 /24    |
| PC-B       | NIC       | 192.168.1.2 /24    |

<h2> Задачи </h2>

<ol>
  <li> Подключить сеть в соответствии с топологией. </li>
  <li> Настроить узлы ПК. </li>
  <li> Настроить базовые параметры коммутаторов. </li>
</ol>

<p> Базовые параметры коммутатора на примере S1: </p>

<p> enable >> configure terminal </p>
<p> > no ip domain-lookup </p>
<p> > hostname S1 </p>
<p> > service password-encryption </p>
<p> > enable secret class </p>
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>

<p> > interface Vlan1 </p>
<p> > ip address 192.168.1.11 255.255.255.0 </p>
<p> > no shutdown </p>


<p> > line console 0 </p>
<p> > line vty 0 15 </p>
<p> > password cisco </p>
<p> > exit </p>

<p> > copy running-config startup-config </p>

<p> MAC-адрес компьютера PC-A:  </p>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab01/pc-a.png>

<p> MAC-адрес компьютера PC-B:  </p>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab01/pc-b.png>

