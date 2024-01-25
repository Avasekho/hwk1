<h1> Лабораторная работа. Базовая настройка коммутатора </h1> 

<h2> Топология </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab00/topology.png/>

<h2> Таблица адресации </h2>

| Устройство | Интерфейс | IP-адрес / префикс |
|:----------:|:---------:|:------------------:|
| S1         | VLAN 1    | 192.168.1.2 /24    |
| PC-A       | NIC       | 192.168.1.10 /24   |

<h2> Задачи </h2>

<ol>
  <li> Настроить базовые параметры коммутатора. </li>
  <li> Настроить IP-адрес для ПК. </li>
  <li> Отобразить конфигурацию устройства. </li>
  <li> Протестирровать сквозное соединение, отправив эхо-запрос. </li>
  <li> Протестировать возможность удаленного управления с помощью Telnet. </li>
</ol>

<h2> Настройки коммутатора по умолчанию: </h2>

<p> &gt;&gt; show running-config </p>

<blockquote>

<p> Building configuration... </p>
<p> Current configuration : 1080 bytes </p>
<p> version 15.0 </p>
<p> no service timestamps log datetime msec </p>
<p> no service timestamps debug datetime msec </p>
<p> no service password-encryption </p>
<p> hostname Switch </p>
<p> spanning-tree mode pvst </p>
<p> spanning-tree extend system-id </p>
<p> interface FastEthernet0/1 </p>
<p> interface FastEthernet0/2 </p>
<p> interface FastEthernet0/3 </p>
<p> interface FastEthernet0/4 </p>
<p> interface FastEthernet0/5 </p>
<p> interface FastEthernet0/6 </p>
<p> interface FastEthernet0/7 </p>
<p> interface FastEthernet0/8 </p>
<p> interface FastEthernet0/9 </p>
<p> interface FastEthernet0/10 </p>
<p> interface FastEthernet0/11 </p>
<p> interface FastEthernet0/12 </p>
<p> interface FastEthernet0/13 </p>
<p> interface FastEthernet0/14 </p>
<p> interface FastEthernet0/15 </p>
<p> interface FastEthernet0/16 </p>
<p> interface FastEthernet0/17 </p>
<p> interface FastEthernet0/18 </p>
<p> interface FastEthernet0/19 </p>
<p> interface FastEthernet0/20 </p>
<p> interface FastEthernet0/21 </p>
<p> interface FastEthernet0/22 </p>
<p> interface FastEthernet0/23 </p>
<p> interface FastEthernet0/24 </p>
<p> interface GigabitEthernet0/1 </p>
<p> interface GigabitEthernet0/2 </p>
<p> interface Vlan1 </p>
<p>  no ip address </p>
<p>  shutdown </p>
<p> line con 0 </p>
<p> line vty 0 4 </p>
<p>  login </p>
<p> line vty 5 15 </p>
<p>  login </p>
<p> end </p>

</blockquote>

<h2> Конфигурация маршрутизатора </h2>

<p> Добавлены настройки: </p>

<p> enable >> configure terminal </p>
<p> > no ip domain-lookup </p>
<p> > hostname S1 </p>
<p> > service password-encryption </p>
<p> > enable secret class </p>
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>

<p> Настроен адрес VLAN1: </p>

<p> > interface Vlan1 </p>
<p> > ip address 192.168.1.2 255.255.255.0 </p>
<p> > line console 0 </p>
<p> > line vty 0 15 </p>
<p> > password cisco </p>

<p> Сохраняем изменения: </p>

<p> > copy running-config startup-config </p>

<h2> Настройки сети для ПК </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab00/pc%20config%20cmd.png>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab00/pc%20config%20gui.png>

<h2> Проверяем доступность коммутатора </h2>

<p> Пробуем достучаться до коммутатора </p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab00/no%20ping.png>

<p> и... оно не работает. Потому что интерфейс VLAN1 на коммутаторе выключен. Видимо в процессе конфигурации что-то пошло не так. </p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab00/vlan%20off.png>

<p> включаем интерфейс: </p>

<p> enable >> configure terminal </p>
<p> > interface vlan 1 </p>
<p> > no shutdown </p>

<p> коммутатор стал доступен: </p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab00/ping%202.png>

<h2> Настройки коммутатора после внесения всех изменений: </h2>

<blockquote>
<p> version 15.0 </p>
<p> no service timestamps log datetime msec </p>
<p> no service timestamps debug datetime msec </p>
<p> service password-encryption </p>
<p> hostname S1 </p>
<p> enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1 </p>
<p> no ip domain-lookup </p>
<p> spanning-tree mode pvst </p>
<p> spanning-tree extend system-id </p>
<p> interface FastEthernet0/1 </p>
<p> interface FastEthernet0/2 </p>
<p> interface FastEthernet0/3 </p>
<p> interface FastEthernet0/4 </p>
<p> interface FastEthernet0/5 </p>
<p> interface FastEthernet0/6 </p>
<p> interface FastEthernet0/7 </p>
<p> interface FastEthernet0/8 </p>
<p> interface FastEthernet0/9 </p>
<p> interface FastEthernet0/10 </p>
<p> interface FastEthernet0/11 </p>
<p> interface FastEthernet0/12 </p>
<p> interface FastEthernet0/13 </p>
<p> interface FastEthernet0/14 </p>
<p> interface FastEthernet0/15 </p>
<p> interface FastEthernet0/16 </p>
<p> interface FastEthernet0/17 </p>
<p> interface FastEthernet0/18 </p>
<p> interface FastEthernet0/19 </p>
<p> interface FastEthernet0/20 </p>
<p> interface FastEthernet0/21 </p>
<p> interface FastEthernet0/22 </p>
<p> interface FastEthernet0/23 </p>
<p> interface FastEthernet0/24 </p>
<p> interface GigabitEthernet0/1 </p>
<p> interface GigabitEthernet0/2 </p>
<p> interface Vlan1 </p>
<p>  ip address 192.168.1.2 255.255.255.0 </p>
<p> banner motd ^C Unauthorized access is strictly prohibited. ^C </p>
<p> line con 0 </p>
<p>  password 7 0822455D0A16 </p>
<p>  logging synchronous </p>
<p>  login </p>
<p> line vty 0 4 </p>
<p>  password 7 0822455D0A16 </p>
<p>  login </p>
<p> line vty 5 15 </p>
<p>  password 7 0822455D0A16 </p>
<p>  login </p>
<p> end </p>
</blockquote>

<h2> Эхо-запросы </h2>

<p> Ping с ПК до коммутатора и ПК на самого себя </p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab00/ping.png/>

<h2> Telnet-соединение </h2>

<p> Проверка подключения по telnet </p>
<p> При подключении просит пароль, при входе в привелегированный режим - просит пароль </p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab00/telnet%201.png>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab00/telnet%202.png>
