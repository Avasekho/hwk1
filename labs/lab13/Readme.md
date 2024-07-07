<h1> Лабораторная работа - Настройка протоколов CDP, LLDP и NTP </h1> 

<h2> Топология </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/topology.png>

<h2> Таблица адресации </h2>

| Устройство |  Интерфейс  |   IP-адрес   |  Маска подсети  | Шлюз по умолчанию |
|:----------:|:-----------:|:------------:|:---------------:|:-----------------:|
| R1         | Loopback 1  | 172.16.1.1   | 255.255.255.0   |         -         |
|            | G0/0/1      | 10.22.0.1    | 255.255.255.0   |         -         |
| S1         | SVI VLAN 1  | 10.22.0.2    | 255.255.255.0   | 10.22.0.1         |
| S2         | SVI VLAN 1  | 10.22.0.3    | 255.255.255.0   | 10.22.0.1         |

<h2> Задачи </h2>

<ol>
  <li> Создание сети и настройка основных параметров устройства </li>
  <li> Обнаружение сетевых ресурсов с помощью протокола CDP </li>
  <li> Обнаружение сетевых ресурсов с помощью протокола LLDP </li>
  <li> Настройка и проверка NTP </li>
</ol>


<h2> Часть 1. Создание сети и настройка основных параметров устройства </h2> 

<h2>Настроим маршрутизаторы:</h2>

<blockquote>
a.	Назначьте маршрутизатору имя устройства.
</blockquote>
<p> hostname R1 </p>

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
e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.
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
h.	Настройка интерфейсов, перечисленных в таблице выше
</blockquote>
<p> > interface G0/0/1</p>
<p> > ip address 10.22.0.1 255.255.255.0</p>
<p> > no shutdown</p>
<p> > exit</p>
<p> > interface loopback 1</p>
<p> > ip address 172.16.1.1 255.255.255.0</p>
<p> > exit</p>

<blockquote>
i.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
</blockquote>
<p> > copy running-config startup-config </p>



<h2>Настроим коммутаторы:</h2>

<blockquote>
a.	Назначьте коммутатору имя устройства.
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
h.	Отключите неиспользуемые интерфейсы
</blockquote>

<p> > interface range F0/2-4</p>
<p> > shutdown</p>
<p> > exit</p>
<p> > interface range F0/6-24</p>
<p> > shutdown</p>
<p> > exit</p>
<p> > interface range G0/1-2</p>
<p> > shutdown</p>
<p> > exit</p>

// аналогично для S2

<p> > interface range F0/2-24</p>
<p> > shutdown</p>
<p> > exit</p>
<p> > interface range G0/1-2</p>
<p> > shutdown</p>
<p> > exit</p>


<blockquote>
i.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
</blockquote>
<p> > copy running-config startup-config </p>


<h2> Часть 2. Обнаружение сетевых ресурсов с помощью протокола CDP </h2> 

<blockquote>
a.	На R1 используйте соответствующую команду show cdp, чтобы определить, сколько интерфейсов включено CDP, сколько из них включено и сколько отключено.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/cdp_1.png>

<blockquote>
Сколько интерфейсов участвует в объявлениях CDP? Какие из них активны?
</blockquote>

<p>3 интерфейса, активен GigabitEthernet0/0/1</p>


<blockquote>
b.	На R1 используйте соответствующую команду show cdp, чтобы определить версию IOS, используемую на S1.
</blockquote>

<p>Version :</p>
<p>Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE4</p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/cdp_2.png>


<blockquote>
c.	На S1 используйте соответствующую команду show cdp, чтобы определить, сколько пакетов CDP было выданных.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/cdp_3.png>

<p>Команда не поддерживается в Packet Tracer</p>

<blockquote>
d.	Настройте SVI для VLAN 1 на S1 и S2, используя IP-адреса, указанные в таблице адресации выше. Настройте шлюз по умолчанию для каждого коммутатора на основе таблицы адресов.
</blockquote>

S1:
<p> > interface Vlan1</p>
<p> > ip address 10.22.0.2 255.255.255.0</p>
<p> > no shutdown</p>
<p> > exit</p>
<p> > ip default-gateway 10.22.0.1</p>

S2:
<p> > interface Vlan1</p>
<p> > ip address 10.22.0.3 255.255.255.0</p>
<p> > no shutdown</p>
<p> > exit</p>
<p> > ip default-gateway 10.22.0.1</p>


<blockquote>
<p>e.	На R1 выполните команду show cdp entry S1 . </p>
<p>Вопрос:</p>
<p>Какие дополнительные сведения доступны теперь?</p>
</blockquote>

<p>IP-адрес</p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/cdp_4.png>


<blockquote>
f.	Отключить CDP глобально на всех устройствах. 
</blockquote>

<p> > no cdp run</p>


<h2> Часть 3. Обнаружение сетевых ресурсов с помощью протокола LLDP </h2> 

<blockquote>
a.	Введите соответствующую команду lldp, чтобы включить LLDP на всех устройствах в топологии.
</blockquote>

<p> > lldp run</p>


<blockquote>
b.	На S1 выполните соответствующую команду lldp, чтобы предоставить подробную информацию о S2. 
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/lldp_1.png>

<blockquote>
Что такое chassis ID  для коммутатора S2?
</blockquote>
<p> MAC-адрес порта который подключен к S1 </p>


<blockquote>
c.	Соединитесь через консоль на всех устройствах и используйте команды LLDP, необходимые для отображения топологии физической сети только из выходных данных команды show.
</blockquote>


<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/lldp_2.png>


<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/lldp_3.png>


<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/lldp_4.png>



<h2> Часть 4. Настройка NTP </h2> 


<h2> Выведем на экран текущее время </h2> 

<blockquote>
Введите команду show clock для отображения текущего времени на R1. Запишите отображаемые сведения о текущем времени в следующей таблице.
</blockquote>

R1:
<p> > show clock</p>

|    Дата    |    Время    | Часовой пояс | Источник времени |
|:----------:|:-----------:|:------------:|:----------------:|
| Mar 1 1993 | 1:59:50.860 |     UTC      |   R1 hardware    |

<h2> Установим время </h2> 

<blockquote>
С помощью команды clock set установите время на маршрутизаторе R1. Введенное время должно быть в формате UTC. 
</blockquote>

<p> > clock timezone msk 3</p>
<p> > clock set 18:00:00 07 July 2024</p>

<h2> Настроим главный сервер NTP </h2> 

<blockquote>
Настройте R1 в качестве хозяина NTP с уровнем слоя 4.
</blockquote>

<p> > ntp master 4</p>


<h2> Настроим клиент NTP </h2> 


<blockquote>
a.	Выполните соответствующую команду на S1 и S2, чтобы просмотреть настроенное время. Запишите текущее время,  в следующей таблице.
</blockquote>

|    Дата    |    Время    | Часовой пояс | Источник времени |
|:----------:|:-----------:|:------------:|:----------------:|
| Mar 1 1993 | 2:26:52.289 |     UTC      |   S1 hardware    |
| Mar 1 1993 | 2:0:14.111  |     UTC      |   S2 hardware    |


<blockquote>
b.	Настройте S1 и S2 в качестве клиентов NTP. Используйте соответствующие команды NTP для получения времени от интерфейса G0/0/1 R1, а также для периодического обновления календаря или аппаратных часов коммутатора.
</blockquote>


S1:
<p> > ntp server 10.22.0.1</p>

S2:
<p> > ntp server 10.22.0.1</p>


<h2> Проверим настройку NTP </h2> 

<blockquote>
a.	Используйте соответствующую команду show , чтобы убедиться, что S1 и S2 синхронизированы с R1.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/ntp_5.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/ntp_2.png>

<blockquote>
b.	Выполните соответствующую команду на S1 и S2, чтобы просмотреть настроенное время и сравнить ранее записанное время.
</blockquote>

<p>Часы обновляются:</p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/ntp_1.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/ntp_3.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/ntp_4.png>


Конфигурация коммутатора:

R1#sh run
<blockquote>
<p>Building configuration...</p>
<p></p>
<p>Current configuration : 924 bytes</p>
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
<p>clock timezone msk 3</p>
<p>!</p>
<p>ip cef</p>
<p>no ipv6 cef</p>
<p>!</p>
<p>lldp run</p>
<p>!</p>
<p>no ip domain-lookup</p>
<p>!</p>
<p>spanning-tree mode pvst</p>
<p>!</p>
<p>interface Loopback1</p>
<p> ip address 172.16.1.1 255.255.255.0</p>
<p>!</p>
<p>interface GigabitEthernet0/0/0</p>
<p> no ip address</p>
<p> duplex auto</p>
<p> speed auto</p>
<p> shutdown</p>
<p>!</p>
<p>interface GigabitEthernet0/0/1</p>
<p> ip address 10.22.0.1 255.255.255.0</p>
<p> duplex auto</p>
<p> speed auto</p>
<p>!</p>
<p>interface Vlan1</p>
<p> no ip address</p>
<p> shutdown</p>
<p>!</p>
<p>router rip</p>
<p>!</p>
<p>ip classless</p>
<p>!</p>
<p>ip flow-export version 9</p>
<p>!</p>
<p>no cdp run</p>
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
<p>ntp master 4</p>
<p>!</p>
<p>end</p>
</blockquote>


Конфигурация коммутатора:

S1#sh run
<blockquote>
<p>Building configuration...</p>
<p></p>
<p>Current configuration : 1604 bytes</p>
<p>!</p>
<p>version 15.0</p>
<p>no service timestamps log datetime msec</p>
<p>no service timestamps debug datetime msec</p>
<p>service password-encryption</p>
<p>!</p>
<p>hostname S1</p>
<p>!</p>
<p>enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1</p>
<p>!</p>
<p>no ip domain-lookup</p>
<p>!</p>
<p>lldp run</p>
<p>!</p>
<p>spanning-tree mode pvst</p>
<p>spanning-tree extend system-id</p>
<p>!</p>
<p>interface FastEthernet0/1</p>
<p>!</p>
<p>interface FastEthernet0/2</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/3</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/4</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/5</p>
<p>!</p>
<p>interface FastEthernet0/6</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/7</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/8</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/9</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/10</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/11</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/12</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/13</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/14</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/15</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/16</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/17</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/18</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/19</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/20</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/21</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/22</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/23</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/24</p>
<p> shutdown</p>
<p>!</p>
<p>interface GigabitEthernet0/1</p>
<p> shutdown</p>
<p>!</p>
<p>interface GigabitEthernet0/2</p>
<p> shutdown</p>
<p>!</p>
<p>interface Vlan1</p>
<p> ip address 10.22.0.2 255.255.255.0</p>
<p>!</p>
<p>ip default-gateway 10.22.0.1</p>
<p>!</p>
<p>no cdp run</p>
<p>!</p>
<p>banner motd ^C Unauthorized access is strictly prohibited. ^C</p>
<p>!</p>
<p>line con 0</p>
<p> password 7 0822455D0A16</p>
<p>!</p>
<p>line vty 0 4</p>
<p> password 7 0822455D0A16</p>
<p> login</p>
<p>line vty 5 15</p>
<p> password 7 0822455D0A16</p>
<p> login</p>
<p>!</p>
<p>ntp server 10.22.0.1</p>
<p>!</p>
<p>end</p>
</blockquote>



S2#sh run
<blockquote>
<p>Building configuration...</p>
<p></p>
<p>Current configuration : 1614 bytes</p>
<p>!</p>
<p>version 15.0</p>
<p>no service timestamps log datetime msec</p>
<p>no service timestamps debug datetime msec</p>
<p>service password-encryption</p>
<p>!</p>
<p>hostname S2</p>
<p>!</p>
<p>enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1</p>
<p>!</p>
<p>no ip domain-lookup</p>
<p>!</p>
<p>lldp run</p>
<p>!</p>
<p>spanning-tree mode pvst</p>
<p>spanning-tree extend system-id</p>
<p>!</p>
<p>interface FastEthernet0/1</p>
<p>!</p>
<p>interface FastEthernet0/2</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/3</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/4</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/5</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/6</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/7</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/8</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/9</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/10</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/11</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/12</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/13</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/14</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/15</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/16</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/17</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/18</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/19</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/20</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/21</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/22</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/23</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/24</p>
<p> shutdown</p>
<p>!</p>
<p>interface GigabitEthernet0/1</p>
<p> shutdown</p>
<p>!</p>
<p>interface GigabitEthernet0/2</p>
<p> shutdown</p>
<p>!</p>
<p>interface Vlan1</p>
<p> ip address 10.22.0.3 255.255.255.0</p>
<p>!</p>
<p>ip default-gateway 10.22.0.1</p>
<p>!</p>
<p>no cdp run</p>
<p>!</p>
<p>banner motd ^C Unauthorized access is strictly prohibited. ^C</p>
<p>!</p>
<p>line con 0</p>
<p> password 7 0822455D0A16</p>
<p>!</p>
<p>line vty 0 4</p>
<p> password 7 0822455D0A16</p>
<p> login</p>
<p>line vty 5 15</p>
<p> password 7 0822455D0A16</p>
<p> login</p>
<p>!</p>
<p>ntp server 10.22.0.1</p>
<p>!</p>
<p>end</p>
</blockquote>