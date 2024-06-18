<h1> Лабораторная работа - Внедрение маршрутизации между виртуальными локальными сетями </h1> 

<h2> Топология </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/topology.png>

<h2> Таблица адресации </h2>

| Устройство |  Интерфейс  |   IP-адрес    |  Маска подсети  | Шлюз по умолчанию |
|:----------:|:-----------:|:-------------:|:---------------:|:-----------------:|
| R1         | G0/0/0      | 10.0.0.1      | 255.255.255.252 |         -         |
|            | G0/0/1      |       -       |        -        |         -         |
|            | G0/0/1.100  | 192.168.1.1   | 255.255.255.192 |         -         |
|            | G0/0/1.200  | 192.168.1.65  | 255.255.255.224 |         -         |
|            | G0/0/1.1000 |       -       |        -        |         -         |
| R2         | G0/0/0      | 10.0.0.2      | 255.255.255.252 |         -         |
|            | G0/0/1      | 192.168.1.97  | 255.255.255.240 |         -         |
| S1         | VLAN 200    | 192.168.1.66  | 255.255.255.224 | 192.168.1.65      |
| S2         | VLAN 1      |               |                 |                   |
| PC-A       | NIC         |     DHCP      |      DHCP       |       DHCP        |
| PC-B       | NIC         |     DHCP      |      DHCP       |       DHCP        |

<h2> Таблица VLAN </h2>

|   VLAN   |     Имя      |     Назначенный интерфейс     |
|:--------:|:------------:|:-----------------------------:|
| 1        | Нет          | S2: F0/18                     | 
| 100      | Клиенты      | S1: F0/6                      |
| 200      | Управление   | S1: VLAN 200                  |
| 999      | Parking_Lot  | S1: F0/1-4, F0/7-24, G0/1-2   |
| 1000     | Собственная  |               -               |

<h2> Задачи </h2>

<ol>
  <li> Создание сети и настройка основных параметров устройства. </li>
  <li> Настройка и проверка двух серверов DHCPv4 на R1. </li>
  <li> Настройка и проверка DHCP-ретрансляции на R2. </li>
</ol>

<h2> Часть 1. Настройка основных параметров устройств </h2>

<p>Создание схемы адресации:</p>
<p></p>
<p>Подсеть сети 192.168.1.0/24 в соответствии со следующими требованиями:</p>

<blockquote>
a.	Одна подсеть «Подсеть A», поддерживающая 58 хостов (клиентская VLAN на R1).
</blockquote>
<p>Минимальный размер сети - 64 хоста. Маска сети /26 или 255.255.255.192; пул адресов - 192.168.1.1 - 192.168.1.62 (+.0 и .63 в качестве служебных) </p>

<blockquote>
b.	Одна подсеть «Подсеть B», поддерживающая 28 хостов (управляющая VLAN на R1).
</blockquote>
<p>Минимальный размер сети - 32 хоста. Маска сети /27 или 255.255.255.224; пул адресов - 192.168.1.65 - 192.168.1.94 (+.64 и .95 в качестве служебных) </p>

<blockquote>
c.	Одна подсеть «Подсеть C», поддерживающая 12 узлов (клиентская сеть на R2).
</blockquote>
<p>Минимальный размер сети - 16 хостов. Маска сети /28 или 255.255.255.240; пул адресов - 192.168.1.97 - 192.168.1.110 (+.96 и .111 в качестве служебных) </p>


Произведем базовую настройку маршрутизаторов:
<blockquote>
a.	Назначьте маршрутизатору имя устройства.
</blockquote>
<p> > hostname R1 </p>

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
h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
</blockquote>
<p> > copy running-config startup-config </p>

<blockquote>
i.	Установите часы на маршрутизаторе на сегодняшнее время и дату.
</blockquote>
<p> > clock set 23:33:00 16 June 2024 </p>


Настроим маршрутизацию между сетями VLAN на маршрутизаторе R1:

<blockquote>
a.	Активируйте интерфейс G0/0/1 на маршрутизаторе.
</blockquote>
<p> > interface G0/0/1</p>
<p> > no shutdown</p>

<blockquote>
b.	Настройте подинтерфейсы для каждой VLAN в соответствии с требованиями таблицы IP-адресации. Все субинтерфейсы используют инкапсуляцию 802.1Q и назначаются первый полезный адрес из вычисленного пула IP-адресов. Убедитесь, что подинтерфейсу для native VLAN не назначен IP-адрес. Включите описание для каждого подинтерфейса.
</blockquote>
<p> > interface G0/0/1.100 </p>
<p> > ip address 192.168.1.1 255.255.255.192 </p>
<p> > description Clients </p>
<p> > encapsulation dot1q 100 </p>
<p> > interface G0/0/1.200 </p>
<p> > ip address 192.168.1.65 255.255.255.224 </p>
<p> > description Management </p>
<p> > encapsulation dot1q 200 </p>
<p> > interface G0/0/1.1000 </p>
<p> > description Private </p>
<p> > encapsulation dot1q 1000 native </p>

<blockquote>
c.	Убедитесь, что вспомогательные интерфейсы работают.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/subinterface_1.png>


Настроим G0/1 на R2, затем G0/0/0 и статическую маршрутизацию для обоих маршрутизаторов:

<blockquote>
a.	Настройте G0/0/1 на R2 с первым IP-адресом подсети C, рассчитанным ранее.
</blockquote>
<p> > interface G0/0/1</p>
<p> > no shutdown</p>
<p> > ip address 192.168.1.97 255.255.255.240 </p>


<blockquote>
b.	Настройте интерфейс G0/0/0 для каждого маршрутизатора на основе приведенной выше таблицы IP-адресации.
</blockquote>
<p> > interface G0/0/0</p>
<p> > no shutdown</p>
<p> > ip address 10.0.0.2 255.255.255.252 </p>
// аналогично для R1

<blockquote>
c.	Настройте маршрут по умолчанию на каждом маршрутизаторе, указываемом на IP-адрес G0/0/0 на другом маршрутизаторе.
</blockquote>
<p> > ip default-gateway 10.0.0.1 </p>
// аналогично для R1

<blockquote>
d.	Убедитесь, что статическая маршрутизация работает с помощью пинга до адреса G0/0/1 R2 от R1.
</blockquote>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/ping_1.png>

<blockquote>
e.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
</blockquote>
<p> > copy running-config startup-config </p>


Настроим базовые параметры для коммутаторов:

<blockquote>
a.	Присвойте коммутатору имя устройства.
</blockquote>
<p> > hostname S1 </p>
<p> > hostname S2 </p>

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
<p> > clock set 20:42:00 17 June 2024 </p>

<blockquote>
i.	Сохранение текущей конфигурации в качестве начальной.
</blockquote>
<p> > copy running-config startup-config </p>


Создадим сети VLAN на коммутаторе S1:

<blockquote>
a.	Создайте необходимые VLAN на коммутаторе 1 и присвойте им имена из приведенной выше таблицы.
</blockquote>
<p> > vlan 100 </p>
<p> > name Clients </p>
<p> > vlan 200 </p>
<p> > name Management </p>
<p> > vlan 999 </p>
<p> > name Parking_Lot </p>
<p> > vlan 1000 </p>
<p> > name Private </p>

<blockquote>
b.	Настройте и активируйте интерфейс управления на S1 (VLAN 200), используя второй IP-адрес из подсети, рассчитанный ранее. Кроме того установите шлюз по умолчанию на S1.
</blockquote>
<p> > interface vlan 200 </p>
<p> > ip address 192.168.1.66 255.255.255.224 </p>
<p> > ip default-gateway 192.168.1.65 </p>

<blockquote>
c.	Настройте и активируйте интерфейс управления на S2 (VLAN 1), используя второй IP-адрес из подсети, рассчитанный ранее. Кроме того, установите шлюз по умолчанию на S2
</blockquote>
<p>(Подсеть C)</p>

<p> > interface vlan 1 </p>
<p> > no shutdown </p>
<p> > ip address 192.168.1.98 255.255.255.240 </p>
<p> > ip default-gateway 192.168.1.97 </p>

<blockquote>
d.	Назначьте все неиспользуемые порты S1 VLAN Parking_Lot, настройте их для статического режима доступа и административно деактивируйте их. На S2 административно деактивируйте все неиспользуемые порты.
</blockquote>

S1:

<p> > interface range F0/1-4 </p>
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

S2:

<p> > interface range F0/1-4 </p>
<p> > shutdown </p>
<p> > interface range F0/6-17 </p>
<p> > shutdown </p>
<p> > interface range F0/19-24 </p>
<p> > shutdown </p>
<p> > interface range G0/1-2 </p>
<p> > shutdown </p>


Назначим сети VLAN соответствующим интерфейсам коммутатора:

<blockquote>
a.	Назначьте используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и настройте их для режима статического доступа.
Откройте окно конфигурации
</blockquote>

S1:

<p> > interface F0/6 </p>
<p> > switchport mode access </p>
<p> > switchport access vlan 100 </p>

S2:

<p> > interface F0/18 </p>
<p> > switchport mode access </p>
<p> > switchport access vlan 1 </p>


<blockquote>
b.	Убедитесь, что VLAN назначены на правильные интерфейсы.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/vlan_1.png>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/vlan_2.png>

<blockquote>
Почему интерфейс F0/5 указан в VLAN 1?
</blockquote>
Это сеть по умолчанию, а данный порт не настраивался на другие сети.


Вручную настроим интерфейс S1 F0/5 в качестве транка 802.1Q:

<blockquote>
a.	Измените режим порта коммутатора, чтобы принудительно создать магистральный канал.
</blockquote>
<p> > interface F0/5 </p>
<p> > switchport mode trunk </p>

<blockquote>
b.	В рамках конфигурации транка  установите для native  VLAN значение 1000.
</blockquote>
<p> > switchport trunk native vlan 1000 </p>

<blockquote>
c.	В качестве другой части конфигурации магистрали укажите, что VLAN 100, 200 и 1000 могут проходить по транку.
</blockquote>
<p> > switchport trunk allowed vlan 100,200,1000 </p>

<blockquote>
d.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
</blockquote>
<p> > copy running-config startup-config </p>

<blockquote>
e.	Проверьте состояние транка.
</blockquote>
<p> > show interfaces f0/5 switchport </p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/vlan_3.png>

 
<blockquote>
Какой IP-адрес был бы у ПК, если бы он был подключен к сети с помощью DHCP?
</blockquote>
f0/6 - порт доступа для VLAN 100, значит адрес из пула 192.168.1.2 - 192.168.1.62


<h2> Часть 2. Настройка и проверка двух серверов DHCPv4 на R1 </h2>


Настроим R1 с пулами DHCPv4 для двух поддерживаемых подсетей:

<blockquote>
a.	Исключите первые пять используемых адресов из каждого пула адресов.
</blockquote>
<p>VLAN 100</p>
<p> > ip dhcp excluded-address 192.168.1.1 192.168.1.5</p>
<p>//</p>
<p>VLAN 200</p>
<p> > ip dhcp excluded-address 192.168.1.65 192.168.1.70</p>

<blockquote>
b.	Создайте пул DHCP (используйте уникальное имя для каждого пула).
</blockquote>
<p> > ip dhcp pool R1_Client_LAN</p>

<blockquote>
c.	Укажите сеть, поддерживающую этот DHCP-сервер.
</blockquote>
<p> > network 192.168.1.0 255.255.255.192</p>

<blockquote>
d.	В качестве имени домена укажите CCNA-lab.com.
</blockquote>
<p> > domain-name CCNA-lab.com </p>

<blockquote>
e.	Настройте соответствующий шлюз по умолчанию для каждого пула DHCP.
</blockquote>
<p> > default-router 192.168.1.1</p>

<blockquote>
f.	Настройте время аренды на 2 дня 12 часов и 30 минут.
</blockquote>
<p> > lease 2 12 30</p>
//команда lease не поддерживается в packet tracer

<blockquote>
g.	Затем настройте второй пул DHCPv4, используя имя пула R2_Client_LAN и вычислите сеть, маршрутизатор по умолчанию, и используйте то же имя домена и время аренды, что и предыдущий пул DHCP.
</blockquote>
<p> > ip dhcp pool R2_Client_LAN</p>
<p> > network 192.168.1.64 255.255.255.224</p>
<p> > domain-name CCNA-lab.com </p>
<p> > default-router 192.168.1.65</p>
<p> > lease 2 12 30</p>

Сохраним конфигурацию:
<p> > copy running-config startup-config </p>


Проверка конфигурации сервера DHCPv4:

<blockquote>
a.	Чтобы просмотреть сведения о пуле, выполните команду show ip dhcp pool.
</blockquote>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/dhcp_1.png>

<blockquote>
b.	Выполните команду show ip dhcp bindings для проверки установленных назначений адресов DHCP.
</blockquote>
<p> > show ip dhcp binding </p>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/dhcp_2.png>

<blockquote>
c.	Выполните команду show ip dhcp server statistics для проверки сообщений DHCP.
</blockquote>
//команда не поддерживается в packet tracer

Попытка получить IP-адрес от DHCP на PC-A

<blockquote>
a.	Из командной строки компьютера PC-A выполните команду ipconfig /all.
</blockquote>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/ipconfig_1.png>

<blockquote>
b.	После завершения процесса обновления выполните команду ipconfig для просмотра новой информации об IP-адресе.
</blockquote>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/ipconfig_2.png>

<blockquote>
c.	Проверьте подключение с помощью пинга IP-адреса интерфейса R0 G0/0/1(???).
</blockquote>
<p> Пинг до R1 G0/0/0 </p>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/ping_2.png>

<p> Пинг до R1 G0/0/1.100 </p>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/ping_3.png>

<p> Пинг до R1 G0/0/1.200 </p>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/ping_4.png>


<h2> Часть 3. Настройка и проверка DHCP-ретрансляции на R2 </h2> 


Настройка R2 в качестве агента DHCP-ретрансляции для локальной сети на G0/0/1

<blockquote>
a.	Настройте команду ip helper-address на G0/0/1, указав IP-адрес G0/0/0 R1.
</blockquote>
<p> > interface G0/0/1 </p>
<p> > ip helper-address 10.0.0.1 </p>

<blockquote>
b.	Сохраните конфигурацию.
</blockquote>
<p> > copy running-config startup-config </p>

Попытка получить IP-адрес от DHCP на PC-B

<blockquote>
a.	Из командной строки компьютера PC-B выполните команду ipconfig /all.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/ipconfig_3.png>

<blockquote>
b.	После завершения процесса обновления выполните команду ipconfig для просмотра новой информации об IP-адресе.
</blockquote>
// адрес не получает, пинг не идет
// R2 не передает дальше бродкасты
// The DHCP server does not have a pool configured for the received port. It drops the packet.
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/err_1.png>




<blockquote>
c.	Проверьте подключение с помощью пинга IP-адреса интерфейса R1 G0/0/1.
</blockquote>


<blockquote>
d.	Выполните show ip dhcp binding для R1 для проверки назначений адресов в DHCP.
</blockquote>


<blockquote>
e.	Выполните команду show ip dhcp server statistics для проверки сообщений DHCP.
</blockquote>


