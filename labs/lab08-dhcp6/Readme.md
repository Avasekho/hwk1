<h1> Лабраторная работа - Настройка DHCPv6 </h1> 

<h2> Топология </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08/topology.png>

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