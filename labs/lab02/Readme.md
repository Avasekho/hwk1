<h1> Лабораторная работа. Просмотр таблицы MAC-адресов коммутатора </h1> 

<h2> Топология </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab02/topology.png>

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

<h2> Базовые параметры коммутатора на примере S1: </h2>

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

<h2> MAC-адрес компьютера PC-A: </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab02/pc-a.png>

<h2> MAC-адрес компьютера PC-B: </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab02/pc-b.png>

<h2> Коммутатор S1 - порт F0/1: </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab02/s1-f01.png>

<h2> Коммутатор S2 - порт F0/1: </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab02/s2-f01.png>

<h2> Таблица MAC-адресов S2: </h2>
<p> до эхо-запросов: </p>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab02/s2-before.png>

<p> после эхо-запросов: </p>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab02/s2-after.png>

<p> Как видим, MAC-адрес соответствующий ПК PC-A находится на порту F0/1 (Коммутатор S1); MAC-адрес соответствующий ПК PC-B находится на порту F0/18 </p>

<p> После очистки динамическая таблица пуста: </p>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab02/s2-clear.png>

<p> Спустя 10 секунд виден второй коммутатор S1: </p>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab02/s2-later.png>

<h2> ARP-таблица на ПК PC-B: </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab02/pc-b-arp.png>

<p> В ARP-таблице виден только соседний коммутатор </p>

<p> Отправим Эхо-запросы другим устройствам сети: </p>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab02/pc-b-echo.png>

<p> После Эхо-запросов ARP-таблица на ПК PC-B наполнилась новыми записями: </p>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab02/pc-b-arp-after.png>

<p> ARP-таблица на коммутаторе S2 также наполнилась новыми записями: </p>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab02/s2-after-echo.png>
<p> Для коммутатора S1 есть запись MAC-адреса порта F0/1 и MAC-адреса интерфейса VLAN1 </p>

