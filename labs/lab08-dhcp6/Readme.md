<h1> Лабраторная работа - Настройка DHCPv6 </h1> 

<h2> Топология </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp6/topology.png>

<h2> Таблица адресации </h2>

| Устройство |  Интерфейс  |      IPv6-адрес       |
|:----------:|:-----------:|:---------------------:|
| R1         | G0/0/0      | 2001:db8:acad:2::1/64 |
|            | G0/0/0      | fe80::1               |
|            | G0/0/1      | 2001:db8:acad:1::1/64 |
|            | G0/0/1      | fe80::1               |
| R2         | G0/0/0      | 2001:db8:acad:2::2/64 |
|            | G0/0/0      | fe80::2               |
|            | G0/0/1      | 2001:db8:acad:3::1/64 |
|            | G0/0/1      | fe80::1               |
| PC-A       | NIC         | DHCP                  |
| PC-B       | NIC         | DHCP                  |

<h2> Задачи </h2>

<ol>
  <li> Создание сети и настройка основных параметров устройства. </li>
  <li> Проверка назначения адреса SLAAC от R1. </li>
  <li> Настройка и проверка сервера DHCPv6 без гражданства на R1. </li>
  <li> Настройка и проверка состояния DHCPv6 сервера на R1. </li>
  <li> Настройка и проверка DHCPv6 Relay на R2. </li>
</ol>

<h2> Часть 1. Настройка основных параметров устройств </h2>

Настроим базовые параметры каждого коммутатора:

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
h.	Отключите все неиспользуемые порты.
</blockquote>
<p> > interface range f0/1-4 </p>
<p> > shutdown </p>
<p> > interface range f0/7-24 </p>
<p> > shutdown </p>
<p> //аналогично для S2 </p>

<blockquote>
i.	Сохранение текущей конфигурации в качестве начальной.
</blockquote>
<p> > copy running-config startup-config </p>


Произведем базовую настройку маршрутизаторов:

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
h.	Активация IPv6-маршрутизации
</blockquote>
<p> > IPv6 unicast-routing </p>

<blockquote>
i.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
</blockquote>
<p> > copy running-config startup-config </p>
// аналогично для R2


Настроим интерфейсы и маршрутизацию для обоих маршрутизаторов:

<blockquote>
a.	Настройте интерфейсы G0/0/0 и G0/1 на R1 и R2 с адресами IPv6, указанными в таблице выше
</blockquote>

R1:
<p> > interface g0/0/0 </p>
<p> > ipv6 address 2001:db8:acad:2::1/64 </p>
<p> > ipv6 address fe80::1 link-local </p>
<p> > no shutdown </p>

<p> > interface g0/0/1 </p>
<p> > ipv6 address 2001:db8:acad:1::1/64 </p>
<p> > ipv6 address fe80::1 link-local </p>
<p> > no shutdown </p>


R2:

<p> > interface g0/0/0 </p>
<p> > ipv6 address 2001:db8:acad:2::2/64 </p>
<p> > ipv6 address fe80::2 link-local </p>
<p> > no shutdown </p>

<p> > interface g0/0/1 </p>
<p> > ipv6 address 2001:db8:acad:3::1/64 </p>
<p> > ipv6 address fe80::1 link-local </p>
<p> > no shutdown </p>


<blockquote>
b.	Настройте маршрут по умолчанию на каждом маршрутизаторе, который указывает на IP-адрес G0/0/0 на другом маршрутизаторе.
</blockquote>

R1:

<p> > ipv6 route ::/0 2001:db8:acad:2::2 </p>

R2:

<p> > ipv6 route ::/0 2001:db8:acad:2::1 </p>

<blockquote>
c.	Убедитесь, что маршрутизация работает с помощью пинга адреса G0/0/1 R2 из R1
</blockquote>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp6/ping_1.png>


<blockquote>
d.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
</blockquote>

<p> > copy running-config startup-config </p>


<h2> Часть 2. Проверка назначения адреса SLAAC от R1 </h2>

Включим PC-A и убедимся, что сетевой адаптер настроен для автоматической настройки IPv6:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp6/ipconfig_1.png>

<blockquote>
Откуда взялась часть адреса с идентификатором хоста?
</blockquote>
Генерируется на основе MAC-адреса интерфейса


<h2> Часть 3. Настройка и проверка сервера DHCPv6 на R1 </h2>

<blockquote>
В части 3 выполняется настройка и проверка состояния DHCP-сервера на R1. Цель состоит в том, чтобы предоставить PC-A информацию о DNS-сервере и домене.
</blockquote>

Проверим еще раз кофигурацию PC-A

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp6/ipconfig_2.png>


Настроим R1 для предоставления DHCPv6 без состояния для PC-A:


<blockquote>
<p>a.	Создайте пул DHCP IPv6 на R1 с именем R1-STATELESS. В составе этого пула назначьте адрес DNS-сервера как 2001:db8:acad: :1, а имя домена — как stateless.com.</p>
<p>Откройте окно конфигурации</p>
<p>R1(config)# ipv6 dhcp pool R1-STATELESS</p>
<p>R1(config-dhcp)# dns-server 2001:db8:acad::254</p>
<p>R1(config-dhcp)# domain-name STATELESS.com</p>
</blockquote>
<p> > ipv6 dhcp pool R1-STATELESS </p>
<p> > dns-server 2001:db8:acad::254 </p>
<p> > domain-name STATELESS.com </p>

<blockquote>
<p>b.	Настройте интерфейс G0/0/1 на R1, чтобы предоставить флаг конфигурации OTHER для локальной сети R1 и укажите только что созданный пул DHCP в качестве ресурса DHCP для этого интерфейса.</p>
<p>R1(config)# interface g0/0/1</p>
<p>R1(config-if)# ipv6 nd other-config-flag </p>
<p>R1(config-if)# ipv6 dhcp server R1-STATELESS</p>
</blockquote>
<p> > interface g0/0/1 </p>
<p> > ipv6 nd other-config-flag </p>
<p> > ipv6 dhcp server R1-STATELESS </p>

<blockquote>
c.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
</blockquote>
<p> > copy running-config startup-config </p>

<blockquote>
d.	Перезапустите PC-A.
</blockquote>
// done

<blockquote>
e.	Проверьте вывод ipconfig /all и обратите внимание на изменения.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp6/ipconfig_3.png>

// В настройках появился DNS-суффикс и адрес сервера


<blockquote>
f.	Тестирование подключения с помощью пинга IP-адреса интерфейса G0/1 R2.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp6/ping_2.png>

// адрес доступен


<h2> Часть 4. Настройка сервера DHCPv6 с сохранением состояния на R1 </h2>

<blockquote>
В части 4 настраивается R1 для ответа на запросы DHCPv6 от локальной сети на R2.
</blockquote>

<blockquote>
<p>a.	Создайте пул DHCPv6 на R1 для сети 2001:db8:acad:3:aaa::/80. Это предоставит адреса локальной сети, подключенной к интерфейсу G0/0/1 на R2. В составе пула задайте DNS-сервер 2001:db8:acad: :254 и задайте доменное имя STATEFUL.com.</p>
<p>Откройте окно конфигурации</p>
<p>R1(config)# ipv6 dhcp pool R2-STATEFUL</p>
<p>R1(config-dhcp)# address prefix 2001:db8:acad:3:aaa::/80</p>
<p>R1(config-dhcp)# dns-server 2001:db8:acad::254</p>
<p>R1(config-dhcp)# domain-name STATEFUL.com</p>
</blockquote>
<p> > ipv6 dhcp pool R2-STATEFUL </p>
<p> > address prefix 2001:db8:acad:3:aaa::/80 </p>
<p> > dns-server 2001:db8:acad::254 </p>
<p> > domain-name STATEFUL.com </p>

<blockquote>
<p>b.	Назначьте только что созданный пул DHCPv6 интерфейсу g0/0/0 на R1.</p>
<p>R1(config)# interface g0/0/0</p>
<p>R1(config-if)# ipv6 dhcp server R2-STATEFUL</p>
</blockquote>
<p> > interface g0/0/0 </p>
<p> > ipv6 dhcp server R2-STATEFUL </p>


<h2> Часть 5. Настройка и проверка ретрансляции DHCPv6 на R2. </h2>

Включим PC-B и проверим адрес SLAAC, который он генерирует:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp6/ipconfig_4.png>


Настроим R2 в качестве агента DHCP-ретрансляции для локальной сети на G0/0/1

<blockquote>
a.	Настройте команду ipv6 dhcp relay на интерфейсе R2 G0/0/1, указав адрес назначения интерфейса G0/0/0 на R1. Также настройте команду managed-config-flag .
Откройте окно конфигурации
R2 (конфигурация) # интерфейс g0/0/1
R2(config-if)# ipv6 nd managed-config-flag
R2(config-if)# ipv6 dhcp relay destination 2001:db8:acad:2::1 g0/0/0
</blockquote>
<p> > interface g0/0/1 </p>
<p> > ipv6 nd managed-config-flag </p>
<p> > ipv6 dhcp relay destination 2001:db8:acad:2::1 g0/0/0</p>

<p>// По всей видимости relay agent для IPv6 в Packet Tracer на данный момент не поддерживается</p>


<p>// UPD. Т.к. relay agent не поддерживается, то чтобы проверить работу STATEFUL DHCP пропишем настройки отдельно на R2 </p>
<p>// Настраиваем DHCP сервер на R2</p>
<p> > ipv6 dhcp pool R2-STATEFUL </p>
<p> > dns-server 2001:db8:acad::254 </p>
<p> > domain-name STATEFUL.com </p>
<p></p>
<p>// Добавляем DHCP на интерфейс g0/0/1</p>
<p> > interface g0/0/1 </p>
<p> > ipv6 dhcp server R2-STATEFUL </p>
<p> > ipv6 nd managed-config-flag </p>


<blockquote>
b.	Сохраните конфигурацию.
</blockquote>
<p> > copy running-config startup-config </p>

Попытка получить адрес IPv6 из DHCPv6 на PC-B

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp6/ipconfig_5.png>

<p>// Из-за ограничений Packet Tracer мы не получаем в настройках ни DNS-суффикса STATEFUL.com, ни адреса сервера.</p>
<p>// G0/0/1 на R1 также недоступен</p>


<p>// UPD. Получаем адрес с DHCP сервера на R2 </p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp6/ipconfig_6.png>

Конфигурация маршрутизатора:

R1#sh run
<blockquote>
<p>Building configuration...</p>
<p></p>
<p>Current configuration : 1295 bytes</p>
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
<p>ipv6 unicast-routing</p>
<p>!</p>
<p>no ipv6 cef</p>
<p>!</p>
<p>ipv6 dhcp pool R1-STATELESS</p>
<p> dns-server 2001:DB8:ACAD::254</p>
<p> domain-name STATELESS.com</p>
<p>!</p>
<p>ipv6 dhcp pool R2-STATEFUL</p>
<p> address prefix 2001:db8:acad:3:aaa::/80 lifetime 172800 86400</p>
<p> dns-server 2001:DB8:ACAD::254</p>
<p> domain-name STATEFUL.com</p>
<p>!</p>
<p>no ip domain-lookup</p>
<p>!</p>
<p>spanning-tree mode pvst</p>
<p>!</p>
<p>interface GigabitEthernet0/0/0</p>
<p> no ip address</p>
<p> duplex auto</p>
<p> speed auto</p>
<p> ipv6 address FE80::1 link-local</p>
<p> ipv6 address 2001:DB8:ACAD:2::1/64</p>
<p> ipv6 dhcp server R2-STATEFUL</p>
<p>!</p>
<p>interface GigabitEthernet0/0/1</p>
<p> no ip address</p>
<p> duplex auto</p>
<p> speed auto</p>
<p> ipv6 address FE80::1 link-local</p>
<p> ipv6 address 2001:DB8:ACAD:1::1/64</p>
<p> ipv6 nd other-config-flag</p>
<p> ipv6 dhcp server R1-STATELESS</p>
<p>!</p>
<p>interface Vlan1</p>
<p> no ip address</p>
<p> shutdown</p>
<p>!</p>
<p>ip classless</p>
<p>!</p>
<p>ip flow-export version 9</p>
<p>!</p>
<p>ipv6 route ::/0 2001:DB8:ACAD:2::2</p>
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
<p>end
</blockquote>



R2#sh run
<blockquote>
<p>Building configuration...</p>
<p></p>
<p>Current configuration : 1115 bytes</p>
<p>!</p>
<p>version 15.4</p>
<p>no service timestamps log datetime msec</p>
<p>no service timestamps debug datetime msec</p>
<p>service password-encryption</p>
<p>!</p>
<p>hostname R2</p>
<p>!</p>
<p>enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1</p>
<p>!</p>
<p>ip cef</p>
<p>ipv6 unicast-routing</p>
<p>!</p>
<p>no ipv6 cef</p>
<p>!</p>
<p>ipv6 dhcp pool R2-STATEFUL</p>
<p> dns-server 2001:DB8:ACAD::254</p>
<p> domain-name STATEFUL.com</p>
<p>!</p>
<p>no ip domain-lookup</p>
<p>!</p>
<p>spanning-tree mode pvst</p>
<p>!</p>
<p>interface GigabitEthernet0/0/0</p>
<p> no ip address</p>
<p> duplex auto</p>
<p> speed auto</p>
<p> ipv6 address FE80::2 link-local</p>
<p> ipv6 address 2001:DB8:ACAD:2::2/64</p>
<p>!</p>
<p>interface GigabitEthernet0/0/1</p>
<p> no ip address</p>
<p> duplex auto</p>
<p> speed auto</p>
<p> ipv6 address FE80::1 link-local</p>
<p> ipv6 address 2001:DB8:ACAD:3::1/64</p>
<p> ipv6 nd managed-config-flag</p>
<p> ipv6 dhcp server R2-STATEFUL</p>
<p>!</p>
<p>interface Vlan1</p>
<p> no ip address</p>
<p> shutdown</p>
<p>!</p>
<p>ip classless</p>
<p>!</p>
<p>ip flow-export version 9</p>
<p>!</p>
<p>ipv6 route ::/0 2001:DB8:ACAD:2::1</p>
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
<p>end</p>
</blockquote>