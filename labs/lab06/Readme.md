<h1> Лабораторная работа - Внедрение маршрутизации между виртуальными локальными сетями </h1> 

<h2> Топология </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab06/topology.png>

<h2> Таблица адресации </h2>

| Устройство |  Интерфейс  |   IP-адрес    |  Маска подсети  | Шлюз по умолчанию |
|:----------:|:-----------:|:-------------:|:---------------:|:-----------------:|
| R1         | G0/0/1.10   | 192.168.10.1  | 255.255.255.0   |         -         |
|            | G0/0/1.20   | 192.168.20.1  | 255.255.255.0   |         -         |
|            | G0/0/1.30   | 192.168.30.1  | 255.255.255.0   |         -         |
|            | G0/0/1.1000 |       -       |       -         |         -         |
| S1         | VLAN 10     | 192.168.10.11 | 255.255.255.0   | 192.168.10.1      |
| S2         | VLAN 10     | 192.168.10.12 | 255.255.255.0   | 192.168.10.1      |
| PC-A       | NIC         | 192.168.20.3  | 255.255.255.0   | 192.168.20.1      |
| PC-b       | NIC         | 192.168.30.3  | 255.255.255.0   | 192.168.30.1      |

<h2> Таблица VLAN </h2>

|   VLAN   |     Имя      |     Назначенный интерфейс     |
|:--------:|:------------:|:-----------------------------:|
| 10       | Управление   | S1: VLAN 10                   | 
|          |              | S2: VLAN 10                   |
| 20       | Sales        | S1: F0/6                      |
| 30       | Operations   | S2: F0/18                     |
| 999      | Parking_Lot  | S1: F0/2-4, F0/7-24, G0/1-2   |
|          |              | S2: F0/2-17, F0/19-24, G0/1-2 |
| 1000     | Собственная  |               -               |

<h2> Задачи </h2>

<ol>
  <li> Создание сети и настройка основных параметров устройства. </li>
  <li> Создание сетей VLAN и назначение портов коммутатора. </li>
  <li> Настройка транка 802.1Q между коммутаторами. </li>
  <li> Настройка маршрутизации между сетями VLAN. </li>
  <li> Проверка, что маршрутизация между VLAN работает. </li>
</ol>

<h2> Часть 1. Настройка основных параметров устройств </h2> 

Настроим базовые параметры маршрутизатора:

<blockquote>
a.	Подключитесь к маршрутизатору с помощью консоли и активируйте привилегированный режим EXEC.
b.	Войдите в режим конфигурации.
</blockquote>
<p> enable >> configure terminal </p>

<blockquote>
c.	Назначьте маршрутизатору имя устройства.
</blockquote>
<p> hostname R1 </p>

<blockquote>
d.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.
</blockquote>
<p> > no ip domain-lookup </p>

<blockquote>
e.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
</blockquote>
<p> > enable secret class </p>

<blockquote>
f.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.
</blockquote>
<p> > line console 0 </p>
<p> > password cisco </p>

<blockquote>
g.	Установите cisco в качестве пароля виртуального терминала и активируйте вход.
</blockquote>
<p> > line vty 0 15 </p>
<p> > password cisco </p>

<blockquote>
h.	Зашифруйте открытые пароли.
</blockquote>
<p> > service password-encryption </p>

<blockquote>
i.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.
</blockquote>
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>

<blockquote>
j.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
</blockquote>
<p> > copy running-config startup-config </p>

<blockquote>
k.	Настройте на маршрутизаторе время.
</blockquote>
<p> > clock set 15:57:00 09 June 2024 </p>

Настроим базовые параметры для коммутаторов:

<blockquote>
a.	Присвойте коммутатору имя устройства.
</blockquote>
<p> hostname S1 </p>
<p> hostname S2 </p>

<blockquote>
b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.
</blockquote>
<p> > no ip domain-lookup </p>

<blockquote>
c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
</blockquote>
<p> > enable secret class </p>

<blockquote>
d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.
</blockquote>
<p> > line console 0 </p>
<p> > password cisco </p>

<blockquote>
e.	Установите cisco в качестве пароля виртуального терминала и активируйте вход.
</blockquote>
<p> > line vty 0 15 </p>
<p> > password cisco </p>

<blockquote>
f.	Зашифруйте открытые пароли.
</blockquote>
<p> > service password-encryption </p>

<blockquote>
g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.
</blockquote>
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>

<blockquote>
h.	Настройте на коммутаторах время.
</blockquote>
<p> > clock set 16:02:00 09 June 2024 </p>

<blockquote>
i.	Сохранение текущей конфигурации в качестве начальной.
</blockquote>
<p> > copy running-config startup-config </p>

Настроим интерфейсы на ПК:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab06/pc_1.png>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab06/pc_2.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab06/pc_3.png>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab06/pc_4.png>


<h2> Часть 2. Создание сетей VLAN и назначение портов коммутатора </h2> 

Создадим сети VLAN на коммутаторах:

<p> > vlan 10 </p>
<p> > name Management </p>
<p> > vlan 20 </p>
<p> > name Sales </p>
<p> > vlan 30 </p>
<p> > name Operations </p>
<p> > vlan 999 </p>
<p> > name Parking_Lot </p>
<p> > vlan 1000 </p>
<p> > name Private </p>

Назначим интерфейс управления на коммутаторах:
S1:
<p> > interface vlan 10 </p>
<p> > ip address 192.168.10.11 255.255.255.0 </p>
<p> > ip default-gateway 192.168.10.1 </p>

S2:
<p> > interface vlan 10 </p>
<p> > ip address 192.168.10.12 255.255.255.0 </p>
<p> > ip default-gateway 192.168.10.1 </p>

Назначим неиспользуемые интерфейсы сети Parking_Lot и деактивируем их:

<p> > interface range F0/2-4 </p>
<p> > switchport mode access </p>
<p> > switchport access vlan 999 </p>
<p> > shutdown </p>
<p> > interface range F0/7-24 </p>
<p> > switchport mode access </p>
<p> > switchport access vlan 999 </p>
<p> > shutdown </p>
<p> > interface range G0/1-2 </p>
<p> > switchport mode access </p>
<p> > switchport access vlan 999 </p>
<p> > shutdown </p>

Аналогично для S2

Назначим VLAN на интерфейсы в соответствии с таблицей:

S1:
<p> > interface F0/6 </p>
<p> > switchport mode access </p>
<p> > switchport access vlan 20 </p>

S2:
<p> > interface F0/18 </p>
<p> > switchport mode access </p>
<p> > switchport access vlan 30 </p>

Результат:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab06/vlan_1.png>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab06/vlan_2.png>

Не настроенными остались только интерфейсы для транк-портов.


<h2> Часть 3. Конфигурация магистрального канала стандарта 802.1Q между коммутаторами </h2>

Настроим магистральный интерфейс F0/1 на коммутаторах S1 и S2:

S1:

<p> > interface F0/1 </p>
<p> > switchport mode trunk </p>
<p> > switchport trunk native vlan 1000 </p>
<p> > switchport trunk allowed vlan 10,20,30,1000 </p>

S2:

<p> > interface F0/1 </p>
<p> > switchport mode trunk </p>
<p> > switchport trunk native vlan 1000 </p>
<p> > switchport trunk allowed vlan 10,20,30,1000 </p>

Результаты:
<p> > show interfaces f0/1 switchport </p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab06/vlan_3.png>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab06/vlan_4.png>

Настроим магистральный интерфейс F0/5 на коммутаторе S1:

<p> > interface F0/5 </p>
<p> > switchport mode trunk </p>
<p> > switchport trunk native vlan 1000 </p>
<p> > switchport trunk allowed vlan 10,20,30,1000 </p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab06/vlan_5.png>

<h2> Часть 4. Настройка маршрутизации между сетями VLAN </h2>

Настроим подинтерфейсы для каждой VLAN, в соответствии с таблицей:

<p> > interface G0/0/1.10 </p>
<p> > ip address 192.168.10.1 255.255.255.0 </p>
<p> > description Management </p>
<p> > encapsulation dot1q 10 </p>
<p> > interface G0/0/1.20 </p>
<p> > ip address 192.168.20.1 255.255.255.0 </p>
<p> > description Sales </p>
<p> > encapsulation dot1q 20 </p>
<p> > interface G0/0/1.30 </p>
<p> > ip address 192.168.30.1 255.255.255.0 </p>
<p> > description Operations </p>
<p> > encapsulation dot1q 30 </p>
<p> > interface G0/0/1.1000 </p>
<p> > description Private </p>
<p> > encapsulation dot1q 1000 native </p>

Интерфейсы доступны, пинг идет
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab06/vlan_6.png>


<h2> Часть 5. Проверьте, работает ли маршрутизация между VLAN </h2>

Выполним тесты с PC-A:

<blockquote>
a.	Отправьте эхо-запрос с PC-A на шлюз по умолчанию.
</blockquote>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab06/ping_1.png>

<blockquote>
b.	Отправьте эхо-запрос с PC-A на PC-B.
</blockquote>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab06/ping_2.png>

<blockquote>
c.	Отправьте команду ping с компьютера PC-A на коммутатор S2.
</blockquote>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab06/ping_3.png>

Выполним тесты с PC-B:

<blockquote>
В окне командной строки на PC-B выполните команду tracert на адрес PC-A.
</blockquote>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab06/ping_4.png>


Конфигурация маршрутизатора:

R1#sh run
<blockquote>
<p>Building configuration...</p>
<p></p>
<p>Current configuration : 1242 bytes</p>
<p>!</p>
<p>version 15.4</p>
<p>no service timestamps log datetime msec</p>
<p>no service timestamps debug datetime msec</p>
<p>service password-encryption</p>
<p>!</p>
<p>hostname R1</p>
<p>!</p>
<p>enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1</p>
<p>!</p>
<p>ip cef</p>
<p>no ipv6 cef</p>
<p>!</p>
<p>no ip domain-lookup</p>
<p>!</p>
<p>!</p>
<p>spanning-tree mode pvst</p>
<p>!</p>
<p>interface GigabitEthernet0/0/0</p>
<p> no ip address</p>
<p> duplex auto</p>
<p> speed auto</p>
<p> shutdown</p>
<p>!</p>
<p>interface GigabitEthernet0/0/1</p>
<p> no ip address</p>
<p> duplex auto</p>
<p> speed auto</p>
<p>!</p>
<p>interface GigabitEthernet0/0/1.10</p>
<p> description Management</p>
<p> encapsulation dot1Q 10</p>
<p> ip address 192.168.10.1 255.255.255.0</p>
<p>!</p>
<p>interface GigabitEthernet0/0/1.20</p>
<p> description Sales</p>
<p> encapsulation dot1Q 20</p>
<p> ip address 192.168.20.1 255.255.255.0</p>
<p>!</p>
<p>interface GigabitEthernet0/0/1.30</p>
<p> description Operations</p>
<p> encapsulation dot1Q 30</p>
<p> ip address 192.168.30.1 255.255.255.0</p>
<p>!</p>
<p>interface GigabitEthernet0/0/1.1000</p>
<p> description Private</p>
<p> encapsulation dot1Q 1000 native</p>
<p> no ip address</p>
<p>!</p>
<p>interface Vlan1</p>
<p> no ip address</p>
<p> shutdown</p>
<p>!</p>
<p>ip classless</p>
<p>!</p>
<p>ip flow-export version 9</p>
<p>!</p>
<p>banner motd ^C Unauthorized access is strictly prohibited. ^C</p>
<p>!</p>
<p>line con 0</p>
<p> password 7 0822455D0A16</p>
<p>!</p>
<p>line aux 0</p>
<p>!</p>
<p>line vty 0 4</p>
<p> password 7 0822455D0A16</p>
<p> login</p>
<p>line vty 5 15</p>
<p> password 7 0822455D0A16</p>
<p> login</p>
<p>!</p>
<p>end</p>
</blockquote>