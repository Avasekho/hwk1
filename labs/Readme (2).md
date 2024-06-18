<h1> Лабораторная работа - Внедрение маршрутизации между виртуальными локальными сетями </h1> 

<h2> Топология </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08/topology.png>

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
h.	Настройте на коммутаторах время.
</blockquote>
<p> > clock set 16:02:00 09 June 2024 </p>

<blockquote>
i.	Сохранение текущей конфигурации в качестве начальной.
</blockquote>
<p> > copy running-config startup-config </p>


Произведем базовую настройку маршрутизаторов:

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