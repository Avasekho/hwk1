<h1> Лабораторная работа. Базовая настройка коммутатора </h1> 
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab00/topology.png/>

<h2> Задачи </h2>

<ol>
  <li> Настроить базовые параметры коммутатора. </li>
  <li> Настроить IP-адрес для ПК. </li>
  <li> Отобразить конфигурацию устройства. </li>
  <li> Протестирровать сквозное соединение, отправив эхо-запрос. </li>
  <li> Протестировать возможность удаленного управления с помощью Telnet. </li>
</ol>

<h2> Настройки сети для ПК </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab00/pc%20config.png/>

<h2> Конфигурация маршрутизатора </h2>

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
<p> banner motd # Unauthorized access is strictly prohibited. # </p>
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
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab00/ping.png/>

<h2> Telnet-соединение </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab00/telnet.png/>
