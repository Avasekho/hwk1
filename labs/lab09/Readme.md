<h1> Лабораторная работа. Настройка NAT для IPv4 </h1> 

<h2> Топология </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/topology.png>

<h2> Таблица адресации </h2>

| Устройство | Интерфейс  |    IP-адрес    |   Маска подсети    |
|:----------:|:----------:|:--------------:|:------------------:|
| R1         | G0/0/1     | 192.168.10.1   | 255.255.255.0      |
|            | Loopback 0 | 10.10.1.1      | 255.255.255.0      |
| S1         | VLAN 10    | 192.168.10.201 | 255.255.255.0      |
| S2         | VLAN 10    | 192.168.10.202 | 255.255.255.0      |
| PC-A       | NIC        | DHCP           | 255.255.255.0      |
| PC-B       | NIC        | DHCP           | 255.255.255.0      |

<h2> Задачи </h2>

<ol>
  <li> Настройка основного сетевого устройства. </li>
  <li> Настройка сетей VLAN. </li>
  <li> Настройки безопасности коммутатора. </li>
</ol>

<h2> Часть 1. Создание сети и настройка основных параметров устройства.</h2>

<h2> Настройка маршрутизатора (R1):</h2> 

<blockquote>
<p>a.	Загрузите следующий конфигурационный скрипт на R1.</p>
<p></p>
<p>enable</p>
<p>configure terminal</p>
<p>hostname R1</p>
<p>no ip domain lookup</p>
<p>ip dhcp excluded-address 192.168.10.1 192.168.10.9</p>
<p>ip dhcp excluded-address 192.168.10.201 192.168.10.202</p>
<p>!</p>
<p>ip dhcp pool Students</p>
<p> network 192.168.10.0 255.255.255.0</p>
<p> default-router 192.168.10.1</p>
<p> domain-name CCNA2.Lab-11.6.1</p>
<p>!</p>
<p>interface Loopback0</p>
<p> ip address 10.10.1.1 255.255.255.0</p>
<p>!</p>
<p>interface GigabitEthernet0/0/1</p>
<p> description Link to S1</p>
<p> ip dhcp relay information trusted</p>
<p> ip address 192.168.10.1 255.255.255.0</p>
<p> no shutdown</p>
<p>!</p>
<p>line con 0</p>
<p> logging synchronous</p>
<p> exec-timeout 0 0</p>
</blockquote>
<p> Скрипт загружен </p>

<blockquote>
<p>b.	b.	Проверьте текущую конфигурацию на R1, используя следующую команду:</p>
<p>R1# show ip interface brief</p>
</blockquote>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/ip_interface_1.png>

<blockquote>
c.	Убедитесь, что IP-адресация и интерфейсы находятся в состоянии up / up
</blockquote>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/ip_interface_2.png>


<h2> Настройка и проверка основных параметров коммутатора:</h2>

<blockquote>
a.	Настройте имя хоста для коммутаторов S1 и S2.
</blockquote>
<p> hostname S1 </p>
<p> hostname S2 </p>

<blockquote>
b.	Запретите нежелательный поиск в DNS.
</blockquote>
<p> > no ip domain-lookup </p>

<blockquote>
c.	Настройте описания интерфейса для портов, которые используются в S1 и S2.
</blockquote>
<p> >S1:</p>
<p> >interface f0/1</p>
<p> >description S2-Switch</p>
<p> >interface f0/5</p>
<p> >description R1-Router</p>
<p> >interface f0/6</p>
<p> >description PC-A</p>
<p></p>
<p> >S2:</p>
<p> >interface f0/1</p>
<p> >description S1-Switch</p>
<p> >interface f0/18</p>
<p> >description PC-B</p>


<blockquote>
d.	Установите для шлюза по умолчанию для VLAN управления значение 192.168.10.1 на обоих коммутаторах.
</blockquote>
<p> > ip default-gateway 192.168.10.1 </p>


<h2> Часть 2. Настройка сетей VLAN на коммутаторах.</h2>

<h2> Сконфигурируем VLAN 10 на коммутаторах:</h2> 
<p> > vlan 10 </p>
<p> > name Management </p>

<h2> Сконфигруриуем SVI для VLAN 10</h2>
<p> > interface vlan10 </p>
<p> > description Management </p>
<p> > no shutdown </p>
<p> > ip address 192.168.10.201 255.255.255.0 </p>


<h2> Настроим VLAN 333 с именем Native на S1 и S2</h2>
<p> > vlan 333 </p>
<p> > name Native </p>

<h2> Настроим VLAN 999 с именем ParkingLot на S1 и S2</h2>
<p> > vlan 999 </p>
<p> > name ParkingLot </p>

<h2> Часть 3. Настройки безопасности коммутатора.</h2>

<h2> Релизация магистральных соединений 802.1Q</h2>

<blockquote>
a.	Настройте все магистральные порты Fa0/1 на обоих коммутаторах для использования VLAN 333 в качестве native VLAN.
</blockquote>
<p> > interface F0/1 </p>
<p> > switchport mode trunk </p>
<p> > switchport trunk native vlan 333 </p>
<p> > switchport trunk allowed vlan 1,10,333,999 </p>


<blockquote>
b.	Убедитесь, что режим транкинга успешно настроен на всех коммутаторах.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/trunk_1.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/trunk_2.png>


<blockquote>
c.	Отключить согласование DTP F0/1 на S1 и S2.
</blockquote>
<p> > interface F0/1 </p>
<p> > switchport nonegotiate </p>

<blockquote>
d.	Проверьте с помощью команды show interfaces.
</blockquote>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/dtp_1.png>


<h2> Настройка портов доступа</h2>

<blockquote>
a.	На S1 настройте F0/5 и F0/6 в качестве портов доступа и свяжите их с VLAN 10.
</blockquote>
S1:
<p> > interface range F0/5-6 </p>
<p> > switchport mode access </p>
<p> > switchport access vlan 10 </p>

<blockquote>
b.	На S2 настройте порт доступа Fa0/18 и свяжите его с VLAN 10.
</blockquote>
S2:
<p> > interface F0/18 </p>
<p> > switchport mode access </p>
<p> > switchport access vlan 10 </p>

<h2> Безопасность неиспользуемых портов коммутатора</h2>

<blockquote>
a.	На S1 и S2 переместите неиспользуемые порты из VLAN 1 в VLAN 999 и отключите неиспользуемые порты.
</blockquote>
S1:
<p> > interface range F0/2-4 </p>
<p> > switchport mode access </p>
<p> > switchport access vlan 999 </p>
<p> > shutdown </p>
<p> > interface range F0/7-24 </p>
<p> > switchport mode access </p>
<p> > switchport access vlan 999 </p>
<p> > shutdown </p>
<p> > interface range g0/1-2 </p>
<p> > switchport mode access </p>
<p> > switchport access vlan 999 </p>
<p> > shutdown </p>

S2:
<p> > interface range F0/2-17 </p>
<p> > switchport mode access </p>
<p> > switchport access vlan 999 </p>
<p> > shutdown </p>
<p> > interface range F0/19-24 </p>
<p> > switchport mode access </p>
<p> > switchport access vlan 999 </p>
<p> > shutdown </p>
<p> > interface range g0/1-2 </p>
<p> > switchport mode access </p>
<p> > switchport access vlan 999 </p>
<p> > shutdown </p>

<blockquote>
b.	Убедитесь, что неиспользуемые порты отключены и связаны с VLAN 999, введя команду show interfaces status.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/interfaces_1.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/interfaces_2.png>


<h2> Документирование и реализация функций безопасности порта</h2>
<blockquote>
a.	На S1, введите команду show port-security interface f0/6  для отображения настроек по умолчанию безопасности порта для интерфейса F0/6. Запишите свои ответы ниже.
</blockquote>


| Конфигурация безопасности порта по умолчанию             |
| Функция                         | Настройка по умолчанию |
|:-------------------------------:|:----------------------:|
| Защита портов                   | Disabled               |
| Максимальное количество записей | 1                      |
|   MAC-адресов                   |                        |
| Режим проверки на нарушение     | Shutdown               |
|   безопасности                  |                        |
| Aging Time                      | 0 mins                 |
| Aging Type                      | Absolute               |
| Secure Static Address Aging     | Disabled               |
| Sticky MAC Address              | 0                      |

<blockquote>
<p>b.	На S1 включите защиту порта на F0 / 6 со следующими настройками:</p>
<p>o	Максимальное количество записей MAC-адресов: 3</p>
<p>o	Режим безопасности: restrict</p>
<p>o	Aging time: 60 мин.</p>
<p>o	Aging type: неактивный</p>
</blockquote>
<p> > interface f0/6 </p>
<p> > switchport port-security maximum 3 </p>
<p> > switchport port-security violation restrict </p>
<p> > switchport port-security aging time 60 </p>
<p> > switchport port-security aging type inactivity </p>
// При вводе type inactivity packet tracer выдает ошибку - не знает таких аргументов
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/err_1.png>

<blockquote>
c.	Verify port security on S1 F0/6.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/port_security_1.png>

<blockquote>
d.	Включите безопасность порта для F0 / 18 на S2. Настройте каждый активный порт доступа таким образом, чтобы он автоматически добавлял адреса МАС, изученные на этом порту, в текущую конфигурацию.
</blockquote>

<p> > interface f0/18</p>
<p> > switchport port-security</p>
<p> > switchport port-security mac-address sticky</p>


<blockquote>
<p>e.	Настройте следующие параметры безопасности порта на S2 F / 18:</p>
<p>o	Максимальное количество записей MAC-адресов: 2</p>
<p>o	Тип безопасности: Protect</p>
<p>o	Aging time: 60 мин.</p>
</blockquote>

<p> > switchport port-security maximum 2</p>
<p> > switchport port-security violation protect</p>
<p> > switchport port-security aging time 60</p>


<blockquote>
f.	Проверка функции безопасности портов на S2 F0/18.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/port_security_2.png>

<p>MAC-адрес адрес записался в конфигурацию</p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/sticky_1.png>


<h2> Реализация безопасности DHCP snooping</h2>

<blockquote>
a.	На S2 включите DHCP snooping и настройте DHCP snooping во VLAN 10.
</blockquote>

<p> > ip dhcp snooping</p>
<p> > ip dhcp snooping vlan 10</p>

<blockquote>
b.	Настройте магистральные порты на S2 как доверенные порты.
</blockquote>

<p> > interface f0/1</p>
<p> > ip dhcp snooping trust</p>

<blockquote>
c.	Ограничьте ненадежный порт Fa0/18 на S2 пятью DHCP-пакетами в секунду.
</blockquote>

<p> > interface f0/18</p>
<p> > ip dhcp snooping limit rate 5</p>

<blockquote>
d.	Проверка DHCP Snooping на S2.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/dhcp_snooping_1.png>

<blockquote>
e.	В командной строке на PC-B освободите, а затем обновите IP-адрес.
</blockquote>

<p>// Получение адреса завершается провалом. Маршрутизатор отбрасывает пакеты.</p>
<p>// The device receives a DHCP DISCOVER message that contains DHCP Option-82. The device is not configured to trust DHCP Relay Information. The device drops the packet.</p>
<p>// Попробуем отключить Option 82 на коммутаторе</p>

<p> > no ip dhcp snooping information option</p>

<p>// Начал получать адрес по DHCP</p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/ipconfig_1.png>

<p>// Но Option 82 используется для информирования DHCP-сервера о том, от какого DHCP-ретранслятора и через какой его порт был получен запрос. Отключить его несколько противоречит поставленной задаче по настройке безопасности.</p>
<p>// Попробуем включить доверие на маршрутизаторе чтобы он принял этот пакет</p>

<p> > ip dhcp relay information trust-all</p>

<p>// Начал получать адрес по DHCP</p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/ipconfig_2.png>

<blockquote>
f.	Проверьте привязку отслеживания DHCP с помощью команды show ip dhcp snooping binding.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/dhcp_snooping_binding_1.png>

<h2> Реализация PortFast и BPDU Guard</h2>

<blockquote>
a.	Настройте PortFast на всех портах доступа, которые используются на обоих коммутаторах.
</blockquote>

S1:
<p> > interface f0/6</p>
<p> > spanning-tree portfast</p>

S2:
<p> > interface f0/18</p>
<p> > spanning-tree portfast</p>

<blockquote>
b.	Включите защиту BPDU на портах доступа VLAN 10 S1 и S2, подключенных к PC-A и PC-B.
</blockquote>

S1:
<p> > interface f0/6</p>
<p> > spanning-tree bpduguard enable</p>

S2:
<p> > interface f0/18</p>
<p> > spanning-tree bpduguard enable</p>

<blockquote>
c.	Убедитесь, что защита BPDU и PortFast включены на соответствующих портах.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/spanning_tree_1.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/spanning_tree_2.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/spanning_tree_3.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/spanning_tree_4.png>


<h2> Проверка наличия сквозного ⁪подключения </h2>

Ping PC-A to PC-B
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/ping_1.png>

Ping PC-B to PC-A
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab09/ping_2.png>


Конфигурация маршрутизатора:

R1#sh run
<blockquote>
<p>Building configuration...</p>
<p></p>
<p>Current configuration : 988 bytes</p>
<p>!</p>
<p>version 15.4</p>
<p>no service timestamps log datetime msec</p>
<p>no service timestamps debug datetime msec</p>
<p>no service password-encryption</p>
<p>!</p>
<p>hostname R1</p>
<p>!</p>
<p>ip dhcp relay information trust-all</p>
<p>!</p>
<p>ip dhcp excluded-address 192.168.10.1 192.168.10.9</p>
<p>ip dhcp excluded-address 192.168.10.201 192.168.10.202</p>
<p>!</p>
<p>ip dhcp pool Students</p>
<p> network 192.168.10.0 255.255.255.0</p>
<p> default-router 192.168.10.1</p>
<p> domain-name CCNA2.Lab-11.6.1</p>
<p>!</p>
<p>ip cef</p>
<p>no ipv6 cef</p>
<p>!</p>
<p>no ip domain-lookup</p>
<p>!</p>
<p>spanning-tree mode pvst</p>
<p>!</p>
<p>interface Loopback0</p>
<p> ip address 10.10.1.1 255.255.255.0</p>
<p>!</p>
<p>interface GigabitEthernet0/0/0</p>
<p> no ip address</p>
<p> duplex auto</p>
<p> speed auto</p>
<p> shutdown</p>
<p>!</p>
<p>interface GigabitEthernet0/0/1</p>
<p> description Link to S1</p>
<p> ip address 192.168.10.1 255.255.255.0</p>
<p> duplex auto</p>
<p> speed auto</p>
<p>!</p>
<p>interface Vlan1</p>
<p> no ip address</p>
<p> shutdown</p>
<p>!</p>
<p>ip classless</p>
<p>!</p>
<p>ip flow-export version 9</p>
<p>!</p>
<p>no cdp run</p>
<p>!</p>
<p>line con 0</p>
<p> exec-timeout 0 0</p>
<p> logging synchronous</p>
<p>!</p>
<p>line aux 0</p>
<p>!</p>
<p>line vty 0 4</p>
<p> login</p>
<p>!</p>
<p>end</p>
</blockquote>


Конфигурация коммутаторов:

S1#sh run
<blockquote>
<p>Building configuration...</p>
<p></p>
<p>Current configuration : 3109 bytes</p>
<p>!</p>
<p>version 15.0</p>
<p>no service timestamps log datetime msec</p>
<p>no service timestamps debug datetime msec</p>
<p>no service password-encryption</p>
<p>!</p>
<p>hostname S1</p>
<p>!</p>
<p>no ip domain-lookup</p>
<p>!</p>
<p>spanning-tree mode pvst</p>
<p>spanning-tree extend system-id</p>
<p>!</p>
<p>interface FastEthernet0/1</p>
<p> description S2-Switch</p>
<p> switchport trunk native vlan 333</p>
<p> switchport trunk allowed vlan 1,10,333,999</p>
<p> switchport mode trunk</p>
<p> switchport nonegotiate</p>
<p>!</p>
<p>interface FastEthernet0/2</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/3</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/4</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/5</p>
<p> description R1-Router</p>
<p> switchport access vlan 10</p>
<p> switchport mode access</p>
<p>!</p>
<p>interface FastEthernet0/6</p>
<p> description PC-A</p>
<p> switchport access vlan 10</p>
<p> switchport mode access</p>
<p> switchport port-security maximum 3</p>
<p> switchport port-security violation restrict </p>
<p> switchport port-security aging time 60</p>
<p> spanning-tree portfast</p>
<p> spanning-tree bpduguard enable</p>
<p>!</p>
<p>interface FastEthernet0/7</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/8</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/9</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/10</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/11</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/12</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/13</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/14</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/15</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/16</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/17</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/18</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/19</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/20</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/21</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/22</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/23</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/24</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface GigabitEthernet0/1</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface GigabitEthernet0/2</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface Vlan1</p>
<p> no ip address</p>
<p> shutdown</p>
<p>!</p>
<p>interface Vlan10</p>
<p> description Management</p>
<p> ip address 192.168.10.201 255.255.255.0</p>
<p>!</p>
<p>ip default-gateway 192.168.10.1</p>
<p>!</p>
<p>line con 0</p>
<p>!</p>
<p>line vty 0 4</p>
<p> login</p>
<p>line vty 5 15</p>
<p> login</p>
<p>!</p>
<p>end</p>
</blockquote>


S2#sh run
<blockquote>
<p>Building configuration...</p>
<p></p>
<p>Current configuration : 3325 bytes</p>
<p>!</p>
<p>version 15.0</p>
<p>no service timestamps log datetime msec</p>
<p>no service timestamps debug datetime msec</p>
<p>no service password-encryption</p>
<p>!</p>
<p>hostname S2</p>
<p>!</p>
<p>no ip domain-lookup</p>
<p>!</p>
<p>ip dhcp snooping vlan 10</p>
<p>ip dhcp snooping</p>
<p>!</p>
<p>spanning-tree mode pvst</p>
<p>spanning-tree extend system-id</p>
<p>!</p>
<p>interface FastEthernet0/1</p>
<p> description S1-Switch</p>
<p> switchport trunk native vlan 333</p>
<p> switchport trunk allowed vlan 1,10,333,999</p>
<p> ip dhcp snooping trust</p>
<p> switchport mode trunk</p>
<p> switchport nonegotiate</p>
<p>!</p>
<p>interface FastEthernet0/2</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/3</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/4</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/5</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/6</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/7</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/8</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/9</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/10</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/11</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/12</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/13</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/14</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/15</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/16</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/17</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/18</p>
<p> description PC-B</p>
<p> switchport access vlan 10</p>
<p> ip dhcp snooping limit rate 5</p>
<p> switchport mode access</p>
<p> switchport port-security</p>
<p> switchport port-security maximum 2</p>
<p> switchport port-security mac-address sticky </p>
<p> switchport port-security violation protect </p>
<p> switchport port-security mac-address sticky 00D0.BC8A.4EA5</p>
<p> switchport port-security aging time 60</p>
<p> spanning-tree portfast</p>
<p> spanning-tree bpduguard enable</p>
<p>!</p>
<p>interface FastEthernet0/19</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/20</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/21</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/22</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/23</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/24</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface GigabitEthernet0/1</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface GigabitEthernet0/2</p>
<p> switchport access vlan 999</p>
<p> switchport mode access</p>
<p> shutdown</p>
<p>!</p>
<p>interface Vlan1</p>
<p> no ip address</p>
<p> shutdown</p>
<p>!</p>
<p>interface Vlan10</p>
<p> description Management</p>
<p> ip address 192.168.10.202 255.255.255.0</p>
<p>!</p>
<p>ip default-gateway 192.168.10.1</p>
<p>!</p>
<p>line con 0</p>
<p>!</p>
<p>line vty 0 4</p>
<p> login</p>
<p>line vty 5 15</p>
<p> login</p>
<p>!</p>
<p>end</p>
</blockquote>