<h1> Лабораторная работа. Базовая настройка коммутатора </h1> 

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

<p> version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname S1
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
!
no ip domain-lookup
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
!
interface FastEthernet0/2
!
interface FastEthernet0/3
!
interface FastEthernet0/4
!
interface FastEthernet0/5
!
interface FastEthernet0/6
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface Vlan1
 ip address 192.168.1.2 255.255.255.0
!
banner motd # Unauthorized access is strictly prohibited. #
!
line con 0
 password 7 0822455D0A16
 logging synchronous
 login
!
line vty 0 4
 password 7 0822455D0A16
 login
line vty 5 15
 password 7 0822455D0A16
 login
!
end </p>

<h2> Эхо-запросы </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab00/ping.png/>

<h2> Telnet-соединение </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab00/telnet.png/>
