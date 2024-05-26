<h1> Лабораторная работа. Настройка IPv6-адресов на сетевых устройствах </h1> 

<h2> Топология </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/Topology.png>

<h2> Таблица адресации </h2>

| Устройство | Интерфейс |     IPv6-адрес     | Длина префикса | Шлюз по умолчанию |
|:----------:|:---------:|:------------------:|:--------------:|:-----------------:|
| R1         | G0/0/0    | 2001:db8:acad:a::1 |       64       |         -         |
|            | G0/0/1    | 2001:db8:acad:1::1 |       64       |         -         |
| S1         | VLAN 1    | 2001:db8:acad:1::b |       64       |         -         |
| PC-A       | NIC       | 2001:db8:acad:1::3 |       64       |      fe80::1      |
| PC-B       | NIC       | 2001:db8:acad:a::3 |       64       |      fe80::1      |

<h2> Задачи </h2>

<ol>
  <li> Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора. </li>
  <li> Ручная настройка IPv6-адресов. </li>
  <li> Проверка сквозного соединения. </li>
</ol>


<h2> Часть 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора.</h2> 
<h2> Конфигурация маршрутизатора (R1):</h2> 

Назначаем маршрутизатору имя устройства.
<p> enable >> configure terminal </p>
<p> > hostname R1 </p>

Отключаем поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.
<p> > no ip domain-lookup </p>

Назначим class в качестве зашифрованного пароля привилегированного режима EXEC.
<p> > enable secret class </p>

Назначим cisco в качестве пароля консоли и включите вход в систему по паролю.
<p> > line console 0 </p>
<p> > password cisco </p>

Назначим cisco в качестве пароля VTY и включите вход в систему по паролю.
<p> > line vty 0 15 </p>
<p> > password cisco </p>

Зашифруем открытые пароли.
<p> > service password-encryption </p>

Создадим баннер с предупреждением о запрете несанкционированного доступа к устройству.
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>

Настроим IP-адресации интерфейса, как указано в таблице выше.

<p> > interface g0/0/0 </p>
<p> > ipv6 address 2001:db8:acad:a::1/64 </p>
<p> > ipv6 address fe80::1 link-local </p>
<p> > no shutdown </p>

<p> > interface g0/0/1 </p>
<p> > ipv6 address 2001:db8:acad:1::1/64 </p>
<p> > ipv6 address fe80::1 link-local </p>
<p> > no shutdown </p>

Сохраним текущую конфигурацию в файл загрузочной конфигурации.
<p> > copy running-config startup-config </p>


<h2> Конфигурация коммутатора (S1):</h2> 

Присвоим коммутатору имя устройства.
<p> enable >> configure terminal </p>
<p> > hostname S1 </p>

Отключим поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.
<p> > no ip domain-lookup </p>

Назначим class в качестве зашифрованного пароля привилегированного режима EXEC.
<p> > enable secret class </p>

Назначим cisco в качестве пароля консоли и включите вход в систему по паролю.
<p> > line console 0 </p>
<p> > password cisco </p>

Назначим cisco в качестве пароля VTY и включите вход в систему по паролю.
<p> > line vty 0 15 </p>
<p> > password cisco </p>

Зашифруем открытые пароли.
<p> > service password-encryption </p>

Создадим баннер с предупреждением о запрете несанкционированного доступа к устройству.
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>

Сохраним текущую конфигурацию в файл загрузочной конфигурации.
<p> > copy running-config startup-config </p>

<h2> Часть 2. Ручная настройка IPv6-адресов.</h2>

Шаг 1 - IPv6-адреса назначены интерфейсам Ethernet на R1
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/r1_show.png>

Шаг 2 - активируем IPv6-маршрутизацию на R1

PC-B адрес с настройками получения автоматически, без включенного IPv6 unicast-routing
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/pc_b_no_slaac.png>

Включим IPv6 unicast-routing на маршрутизаторе

<p> > configure terminal </p>
<p> > IPv6 unicast-routing </p>
<p> > exit </p>

PC-B адрес с настройками получения автоматически, с включенным IPv6 unicast-routing
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/pc_b_slaac.png>

Шаг 3. Назначьте IPv6-адреса интерфейсу управления (SVI) на S1.

Включение IPv6-адресации с помощью команды sdm prefer dual-ipv4-and-ipv6 default.
<p> > configure terminal </p>
<p> > sdm prefer dual-ipv4-and-ipv6 default </p>
<p> > end </p>
<p> > reload </p>

Настройка IP-адресации интерфейса, как указано в таблице выше.

<p> > interface Vlan1 </p>
<p> > ipv6 address 2001:db8:acad:1::b/64 </p>
<p> > no shutdown </p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/s1_show_vlan.png>

Шаг 4. Назначьте компьютерам статические IPv6-адреса.

В свойства Ethernet для каждого ПК и назначаем адресацию IPv6
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/pc_a_eth.png>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/pc_a_ipconfig.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/pc_b_eth.png>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/pc_b_ipconfig.png>



<h2> Часть 3. Проверка сквозного подключения.</h2>

С PC-A отправьте эхо-запрос на FE80::1. Это локальный адрес канала, назначенный G0/1 на R1.
Отправьте эхо-запрос на интерфейс управления S1 с PC-A.

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/ping_1.png>

Введите команду tracert на PC-A, чтобы проверить наличие сквозного подключения к PC-B.

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/ping_2.png>

С PC-B отправьте эхо-запрос на PC-A.
С PC-B отправьте эхо-запрос на локальный адрес канала G0/0 на R1.

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/ping_3.png>
