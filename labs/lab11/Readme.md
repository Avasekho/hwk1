<h1> Лабораторная работа. Настройка и проверка расширенных списков контроля доступа. </h1> 

<h2> Топология </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/topology.png>

<h2> Таблица адресации </h2>

| Устройство |  Интерфейс  |   IP-адрес    |  Маска подсети  | Шлюз по умолчанию |
|:----------:|:-----------:|:-------------:|:---------------:|:-----------------:|
| R1         | G0/0/1      |       -       |       -         |         -         |
|            | G0/0/1.20   | 10.20.0.1     | 255.255.255.0   |         -         |
|            | G0/0/1.30   | 10.30.0.1     | 255.255.255.0   |         -         |
|            | G0/0/1.30   | 10.40.0.1     | 255.255.255.0   |         -         |
|            | G0/0/1.1000 |       -       |       -         |         -         |
|            | Loopback 1  | 172.16.1.1    | 255.255.255.0   |         -         |
| R2         | G0/0/1      | 10.20.0.4     | 255.255.255.0   |         -         |
| S1         | VLAN 20     | 10.20.0.2     | 255.255.255.0   | 10.20.0.1         |
| S2         | VLAN 20     | 10.20.0.3     | 255.255.255.0   | 10.20.0.1         |
| PC-A       | NIC         | 10.30.0.10    | 255.255.255.0   | 10.30.0.1         |
| PC-b       | NIC         | 10.40.0.10    | 255.255.255.0   | 10.40.0.1         |

<h2> Таблица VLAN </h2>

|   VLAN   |     Имя      |         Назначенный интерфейс         |
|:--------:|:------------:|:-------------------------------------:|
| 20       | Management   | S2: F0/5                              | 
| 30       | Operations   | S1: F0/6                              |
| 40       | Sales        | S2: F0/18                             |
| 999      | Parking_Lot  | S1: F0/2-4, F0/7-24, G0/1-2           |
|          |              | S2: F0/2-4, F0/6-17, F0/19-24, G0/1-2 |
| 1000     | Собственная  |                   -                   |

<h2> Задачи </h2>

<ol>
  <li> Создание сети и настройка основных параметров устройства. </li>
  <li> Настройка и проверка списков расширенного контроля доступа. </li>
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
h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
</blockquote>
<p> > copy running-config startup-config </p>

// аналогично для R2


<h2>Настроим коммутаторы:</h2>

<blockquote>
a.	Назначьте коммутатору имя устройства.
</blockquote>
<p> hostname S1 </p>

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
h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
</blockquote>
<p> > copy running-config startup-config </p>

// аналогично для S2


<h2> Часть 2. Настройка сетей VLAN на коммутаторах </h2>


<h2> Создадим сети VLAN на коммутаторах </h2>

<blockquote>
a.	Создайте необходимые VLAN и назовите их на каждом коммутаторе из приведенной выше таблицы.
</blockquote>

<p> > vlan 20</p>
<p> > name Management</p>
<p> > exit</p>
<p> > vlan 30</p>
<p> > name Operations</p>
<p> > exit</p>
<p> > vlan 40</p>
<p> > name Sales</p>
<p> > exit</p>
<p> > vlan 999</p>
<p> > name ParkingLot</p>
<p> > exit</p>
<p> > vlan 1000</p>
<p> > name Private</p>
<p> > exit</p>

<blockquote>
b.	Настройте интерфейс управления и шлюз по умолчанию на каждом коммутаторе, используя информацию об IP-адресе в таблице адресации. 
</blockquote>

S1:
<p> > interface vlan20</p>
<p> > no shutdown</p>
<p> > ip address 10.20.0.2 255.255.255.0</p>
<p> > ip default-gateway 10.20.0.1</p>


S2:
<p> > interface vlan20</p>
<p> > no shutdown</p>
<p> > ip address 10.20.0.3 255.255.255.0</p>
<p> > ip default-gateway 10.20.0.1</p>


<blockquote>
c.	Назначьте все неиспользуемые порты коммутатора VLAN Parking Lot, настройте их для статического режима доступа и административно деактивируйте их.
</blockquote>


S1:
<p> > interface range f0/2-4</p>
<p> > switchport mode access</p>
<p> > switchport access vlan 999</p>
<p> > shutdown</p>
<p> > exit</p>
<p> > interface range f0/7-24</p>
<p> > switchport mode access</p>
<p> > switchport access vlan 999</p>
<p> > shutdown</p>
<p> > exit</p>
<p> > interface range g0/1-2</p>
<p> > switchport mode access</p>
<p> > switchport access vlan 999</p>
<p> > shutdown</p>
<p> > exit</p>

S2:
<p> > interface range f0/2-4</p>
<p> > switchport mode access</p>
<p> > switchport access vlan 999</p>
<p> > shutdown</p>
<p> > exit</p>
<p> > interface range f0/6-17</p>
<p> > switchport mode access</p>
<p> > switchport access vlan 999</p>
<p> > shutdown</p>
<p> > exit</p>
<p> > interface range f0/19-24</p>
<p> > switchport mode access</p>
<p> > switchport access vlan 999</p>
<p> > shutdown</p>
<p> > exit</p>
<p> > interface range g0/1-2</p>
<p> > switchport mode access</p>
<p> > switchport access vlan 999</p>
<p> > shutdown</p>
<p> > exit</p>


<h2> Назначим сети VLAN соответствующим интерфейсам коммутатора. </h2>

<blockquote>
a.	Назначьте используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и настройте их для режима статического доступа.
</blockquote>

S1:
<p> > interface range f0/6</p>
<p> > switchport mode access</p>
<p> > switchport access vlan 30</p>
<p> > no shutdown</p>
<p> > exit</p>

S2:
<p> > interface range f0/5</p>
<p> > switchport mode access</p>
<p> > switchport access vlan 20</p>
<p> > no shutdown</p>
<p> > exit</p>
<p> > interface range f0/18</p>
<p> > switchport mode access</p>
<p> > switchport access vlan 40</p>
<p> > no shutdown</p>
<p> > exit</p>


<blockquote>
b.	Выполните команду show vlan brief, чтобы убедиться, что сети VLAN назначены правильным интерфейсам.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/vlan_1.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/vlan_2.png>


<h2> Часть 3. ·Настройте транки (магистральные каналы). </h2>


<h2> Вручную настроим магистральный интерфейс F0/1. </h2>


<blockquote>
a.	Измените режим порта коммутатора на интерфейсе F0/1, чтобы принудительно создать магистральную связь. Не забудьте сделать это на обоих коммутаторах.
</blockquote>

S1:
<p> > interface f0/1</p>
<p> > switchport mode trunk</p>
<p> > no shutdown</p>
<p> > exit</p>

S2:
<p> > interface f0/1</p>
<p> > switchport mode trunk</p>
<p> > no shutdown</p>
<p> > exit</p>

<blockquote>
b.	В рамках конфигурации транка установите для native vlan значение 1000 на обоих коммутаторах. При настройке двух интерфейсов для разных собственных VLAN сообщения об ошибках могут отображаться временно.
</blockquote>

S1:
<p> > interface f0/1</p>
<p> > switchport trunk native vlan 1000</p>
<p> > exit</p>

S2:
<p> > interface f0/1</p>
<p> > switchport trunk native vlan 1000</p>
<p> > exit</p>

<blockquote>
c.	В качестве другой части конфигурации транка укажите, что VLAN 10, 20, 30 и 1000 разрешены в транке.
</blockquote>

// VLAN 10 в данной конфигурации нет, добавлен 40

S1:
<p> > interface f0/1</p>
<p> > switchport trunk allowed vlan 20,30,40,1000</p>
<p> > exit</p>

S2:
<p> > interface f0/1</p>
<p> > switchport trunk allowed vlan 20,30,40,1000</p>
<p> > exit</p>

<blockquote>
d.	Выполните команду show interfaces trunk для проверки портов магистрали, собственной VLAN и разрешенных VLAN через магистраль.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/vlan_3.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/vlan_4.png>


<h2> Вручную настроим магистральный интерфейс F0/5 на коммутаторе S1. </h2>


<blockquote>
a.	Настройте интерфейс S1 F0/5 с теми же параметрами транка, что и F0/1. Это транк до маршрутизатора.
</blockquote>

S1:
<p> > interface f0/5</p>
<p> > switchport mode trunk</p>
<p> > switchport trunk native vlan 1000</p>
<p> > switchport trunk allowed vlan 20,30,40,1000</p>
<p> > no shutdown</p>
<p> > exit</p>

<blockquote>
b.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
</blockquote>

<p> > copy running-config startup-config </p>


<blockquote>
c.	Используйте команду show interfaces trunk для проверки настроек транка.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/vlan_5.png>


<h2> Часть 4. Настройте маршрутизацию. </h2>


<h2> Настройка маршрутизации между сетями VLAN на R1. </h2>

<blockquote>
a.	Активируйте интерфейс G0/0/1 на маршрутизаторе.
</blockquote>

<p> > interface g0/0/1</p>
<p> > no shutdown</p>

<blockquote>
b.	Настройте подинтерфейсы для каждой VLAN, как указано в таблице IP-адресации. Все подинтерфейсы используют инкапсуляцию 802.1Q. Убедитесь, что подинтерфейс для собственной VLAN не имеет назначенного IP-адреса. Включите описание для каждого подинтерфейса.
</blockquote>

<p> > interface G0/0/1.20</p>
<p> > ip address 10.20.0.1 255.255.255.0</p>
<p> > description Management</p>
<p> > encapsulation dot1q 20</p>
<p> > exit</p>
<p> > interface G0/0/1.30</p>
<p> > ip address 10.30.0.1 255.255.255.0</p>
<p> > description Operations</p>
<p> > encapsulation dot1q 30</p>
<p> > exit</p>
<p> > interface G0/0/1.40</p>
<p> > ip address 10.40.0.1 255.255.255.0</p>
<p> > description Sales</p>
<p> > encapsulation dot1q 40</p>
<p> > exit</p>
<p> > interface G0/0/1.1000</p>
<p> > description Private</p>
<p> > encapsulation dot1q 1000 native</p>
<p> > exit</p>

<blockquote>
c.	Настройте интерфейс Loopback 1 на R1 с адресацией из приведенной выше таблицы.
</blockquote>


<p> > interface loopback 1</p>
<p> > ip address 172.16.1.1 255.255.255.0</p>

<blockquote>
d.	С помощью команды show ip interface brief проверьте конфигурацию подынтерфейса.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/interfaces_1.png>


<h2> Настройка интерфейса R2 g0/0/1 с использованием адреса из таблицы и маршрута по умолчанию с адресом следующего перехода 10.20.0.1 </h2>


<p> > interface g0/0/1</p>
<p> > no shutdown</p>
<p> > ip address 10.20.0.4 255.255.255.0</p>
<p> > ip default-gateway 10.20.0.1</p>


<h2> Часть 5. Настройте удаленный доступ. </h2>


<h2> Настроим все сетевые устройства для базовой поддержки SSH. </h2>


<blockquote>
a.	Создайте локального пользователя с именем пользователя SSHadmin и зашифрованным паролем $cisco123!
</blockquote>

<p> > username SSHadmin password $cisco123!</p>


<blockquote>
b.	Используйте ccna-lab.com в качестве доменного имени.
</blockquote>

<p> > ip domain-name ccna-lab.com</p>


<blockquote>
c.	Генерируйте криптоключи с помощью 1024 битного модуля.
</blockquote>

<p> > crypto key generate rsa general-keys modulus 1024</p>

<blockquote>
d.	Настройте первые пять линий VTY на каждом устройстве, чтобы поддерживать только SSH-соединения и с локальной аутентификацией.
</blockquote>

<p> > line vty 0 5</p>
<p> > transport input ssh</p>
<p> > login local</p>


<h2> Включим защищенные веб-службы с проверкой подлинности на R1. </h2>

<blockquote>
<p>a.	Включите сервер HTTPS на R1.</p>
<p>R1(config)# ip http secure-server</p>
</blockquote>

// Команда не поддерживается в packet server

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/err_1.png>

<blockquote>
<p>b.	Настройте R1 для проверки подлинности пользователей, пытающихся подключиться к веб-серверу.</p>
<p>R1(config)# ip http authentication local</p>
</blockquote>

// Команда не поддерживается в packet server



<h2> Часть 6. Проверка подключения </h2>


<h2> Настроим узлы ПК. </h2>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/pc_1.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/pc_2.png>


<h2> Выполним следующие тесты. Эхозапрос должен пройти успешно. </h2>

|   От   | Протокол | Назначение | Результат |
|:------:|:--------:|:----------:|:---------:|
|  PC-A  |  Ping    | 10.40.0.10 |    +      |
|  PC-A  |  Ping    | 10.20.0.1  |    +      |
|  PC-B  |  Ping    | 10.30.0.10 |    +      |
|  PC-B  |  Ping    | 10.20.0.1  |    +      |
|  PC-B  |  Ping    | 172.16.1.1 |    +      |
|  PC-B  |  HTTPS   | 10.20.0.1  |           |
|  PC-B  |  HTTPS   | 172.16.1.1 |           |
|  PC-B  |  SSH     | 10.20.0.1  |    +      |
|  PC-B  |  SSH     | 172.16.1.1 |    +      |

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/ping_1.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/ping_2.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/ping_3.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/ssh_1.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/ssh_2.png>


<h2> Часть 7. Настройка и проверка списков контроля доступа (ACL) </h2>

<blockquote>
<p>При проверке базового подключения компания требует реализации следующих политик безопасности:</p>
<p>Политика 1. Сеть Sales не может использовать SSH в сети Management (но в  другие сети SSH разрешен). </p>
<p>Политика 2. Сеть Sales не имеет доступа к IP-адресам в сети Management с помощью любого веб-протокола (HTTP/HTTPS). Сеть Sales также не имеет доступа к интерфейсам R1 с помощью любого веб-протокола. 
Разрешён весь другой веб-трафик (обратите внимание — Сеть Sales  может получить доступ к интерфейсу Loopback 1 на R1).</p>
<p>Политика 3. Сеть Sales не может отправлять эхо-запросы ICMP в сети Operations или Management. Разрешены эхо-запросы ICMP к другим адресатам. </p>
<p>Политика 4: Cеть Operations  не может отправлять ICMP эхозапросы в сеть Sales. Разрешены эхо-запросы ICMP к другим адресатам. </p>
</blockquote>

<h2> Проанализируем требования к сети и политике безопасности для планирования реализации ACL. </h2>

<p>Политика 1. Сеть Sales не может использовать SSH в сети Management (но в  другие сети SSH разрешен).</p>
<p>Тут будет достаточно заблокировать исходящий SSH из Sales на вход в Management</p>

<p>2. Политика 2. Сеть Sales не имеет доступа к IP-адресам в сети Management с помощью любого веб-протокола (HTTP/HTTPS). Сеть Sales также не имеет доступа к интерфейсам R1 с помощью любого веб-протокола. </p>
<p>Разрешён весь другой веб-трафик (обратите внимание — Сеть Sales  может получить доступ к интерфейсу Loopback 1 на R1).</p>
<p>Заблокировать исходяший траффик из Sales по 80/443 порты маршрутизатора и в Management</p>

<p>3. Политика 3. Сеть Sales не может отправлять эхо-запросы ICMP в сети Operations или Management. Разрешены эхо-запросы ICMP к другим адресатам.</p>
<p>Заблокировать исходящий ICMP из Sales в Operations и Management</p>

<p>4. Политика 4: Cеть Operations  не может отправлять ICMP эхозапросы в сеть Sales. Разрешены эхо-запросы ICMP к другим адресатам</p>
<p>Заблокировать исходящий ICMP из Operations в Sales</p>

<h2> Разработка и применение расширенных списков доступа, которые будут соответствовать требованиям политики безопасности. </h2>

<p> > ip access-list extended vlan40-in</p>
<p> > deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 22</p>
<p> > deny tcp 10.40.0.0 0.0.255.255 10.20.0.0 0.0.255.255 eq 80</p>
<p> > deny tcp 10.40.0.0 0.0.255.255 10.20.0.0 0.0.255.255 eq 443</p>
<p> > deny tcp 10.40.0.0 0.0.255.255 host 10.20.0.1 eq 80</p>
<p> > deny tcp 10.40.0.0 0.0.255.255 host 10.20.0.1 eq 443</p>
<p> > deny tcp 10.40.0.0 0.0.255.255 host 10.30.0.1 eq 80</p>
<p> > deny tcp 10.40.0.0 0.0.255.255 host 10.30.0.1 eq 443</p>
<p> > deny tcp 10.40.0.0 0.0.255.255 host 10.40.0.1 eq 80</p>
<p> > deny tcp 10.40.0.0 0.0.255.255 host 10.40.0.1 eq 443</p>
<p> > deny icmp 10.40.0.0 0.0.255.255 10.20.0.0 0.0.255.255</p>
<p> > deny icmp 10.40.0.0 0.0.255.255 10.30.0.0 0.0.255.255</p>
<p> > permit ip any any</p>
// т.к по умолчанию cisco ставит в конец deny any any явно разрешим в конце для Sales все что не запрещено.

<p> > ip access-list extended vlan30-in</p>
<p> > deny icmp 10.30.0.0 0.0.255.255 10.40.0.0 0.0.255.255</p>


<p> > interface G0/0/1.30</p>
<p> > ip access-group vlan30-in IN</p>

<p> > interface G0/0/1.40</p>
<p> > ip access-group vlan40-in IN</p>


<h2> Убедимся, что политики безопасности применяются развернутыми списками доступа. </h2>

|   От   | Протокол | Назначение | Результат |
|:------:|:--------:|:----------:|:---------:|
|  PC-A  |  Ping    | 10.40.0.10 |    -      |
|  PC-A  |  Ping    | 10.20.0.1  |    +      |
|  PC-B  |  Ping    | 10.30.0.10 |    -      |
|  PC-B  |  Ping    | 10.20.0.1  |    -      |
|  PC-B  |  Ping    | 172.16.1.1 |    +      |
|  PC-B  |  HTTPS   | 10.20.0.1  |    -      |
|  PC-B  |  HTTPS   | 172.16.1.1 |    +      |
|  PC-B  |  SSH     | 10.20.0.1  |    -      |
|  PC-B  |  SSH     | 172.16.1.1 |    +      |


// Т.к. поднять на маршрутизатор HTTP-сервер не удается (нет такой команды в packet tracer) то пока обойдемся без него.

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/ping_4.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/ping_5.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/ping_6.png>


10.20.0.1 недоступен по SSH

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/ssh_4.png>


172.16.1.1 доступен по SSH

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/ssh_5.png>


Конфигурация маршрутизатора:

R1#sh run
<blockquote>
<p>Building configuration...</p>
<p></p>
<p>Current configuration : 2285 bytes</p>
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
<p>username SSHadmin password 7 08654F471A1A0A4640584D</p>
<p>!</p>
<p>no ip domain-lookup</p>
<p>ip domain-name ccna-lab.com</p>
<p>!</p>
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
<p> no ip address</p>
<p> duplex auto</p>
<p> speed auto</p>
<p>!</p>
<p>interface GigabitEthernet0/0/1.20</p>
<p> description Management</p>
<p> encapsulation dot1Q 20</p>
<p> ip address 10.20.0.1 255.255.255.0</p>
<p>!</p>
<p>interface GigabitEthernet0/0/1.30</p>
<p> description Operations</p>
<p> encapsulation dot1Q 30</p>
<p> ip address 10.30.0.1 255.255.255.0</p>
<p> ip access-group vlan30-in in</p>
<p>!</p>
<p>interface GigabitEthernet0/0/1.40</p>
<p> description Sales</p>
<p> encapsulation dot1Q 40</p>
<p> ip address 10.40.0.1 255.255.255.0</p>
<p> ip access-group vlan40-in in</p>
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
<p>ip access-list extended vlan40-in</p>
<p> deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 22</p>
<p> deny tcp 10.40.0.0 0.0.255.255 10.20.0.0 0.0.255.255 eq www</p>
<p> deny tcp 10.40.0.0 0.0.255.255 10.20.0.0 0.0.255.255 eq 443</p>
<p> deny tcp 10.40.0.0 0.0.255.255 host 10.20.0.1 eq www</p>
<p> deny tcp 10.40.0.0 0.0.255.255 host 10.20.0.1 eq 443</p>
<p> deny tcp 10.40.0.0 0.0.255.255 host 10.30.0.1 eq www</p>
<p> deny tcp 10.40.0.0 0.0.255.255 host 10.30.0.1 eq 443</p>
<p> deny tcp 10.40.0.0 0.0.255.255 host 10.40.0.1 eq www</p>
<p> deny tcp 10.40.0.0 0.0.255.255 host 10.40.0.1 eq 443</p>
<p> deny icmp 10.40.0.0 0.0.255.255 10.20.0.0 0.0.255.255</p>
<p> deny icmp 10.40.0.0 0.0.255.255 10.30.0.0 0.0.255.255</p>
<p> permit ip any any</p>
<p>ip access-list extended vlan30-in</p>
<p> deny icmp 10.30.0.0 0.0.255.255 10.40.0.0 0.0.255.255</p>
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
<p> login local</p>
<p> transport input ssh</p>
<p>line vty 5</p>
<p> password 7 0822455D0A16</p>
<p> login local</p>
<p> transport input ssh</p>
<p>line vty 6 15</p>
<p> password 7 0822455D0A16</p>
<p> login</p>
<p>!</p>
<p>end</p>
<p></p>
</blockquote>


R2#sh run
<blockquote>
<p>Building configuration...</p>
<p></p>
<p>Current configuration : 1000 bytes</p>
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
<p>no ipv6 cef</p>
<p>!</p>
<p>username SSHadmin password 7 08654F471A1A0A4640584D</p>
<p>!</p>
<p>no ip domain-lookup</p>
<p>ip domain-name ccna-lab.com</p>
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
<p> ip address 10.20.0.4 255.255.255.0</p>
<p> duplex auto</p>
<p> speed auto</p>
<p>!</p>
<p>interface Vlan1</p>
<p> no ip address</p>
<p> shutdown</p>
<p>!</p>
<p>ip default-gateway 10.20.0.1</p>
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
<p> login local</p>
<p> transport input ssh</p>
<p>line vty 5</p>
<p> password 7 0822455D0A16</p>
<p> login local</p>
<p> transport input ssh</p>
<p>line vty 6 15</p>
<p> password 7 0822455D0A16</p>
<p> login</p>
<p>!</p>
<p>end</p>
</blockquote>


Конфигурация коммутатора:


S1#sh ru
<blockquote>
<p>Building configuration...</p>
<p></p>
<p>Current configuration : 3237 bytes</p>
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
<p>ip domain-name ccna-lab.com</p>
<p>!</p>
<p>username SSHadmin privilege 1 password 7 08654F471A1A0A4640584D</p>
<p>!</p>
<p>spanning-tree mode pvst</p>
<p>spanning-tree extend system-id</p>
<p>!</p>
<p>interface FastEthernet0/1</p>
<p> switchport trunk native vlan 1000</p>
<p> switchport trunk allowed vlan 20,30,40,1000</p>
<p> switchport mode trunk</p>
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
<p> switchport trunk native vlan 1000</p>
<p> switchport trunk allowed vlan 20,30,40,1000</p>
<p> switchport mode trunk</p>
<p>!</p>
<p>interface FastEthernet0/6</p>
<p> switchport access vlan 30</p>
<p> switchport mode access</p>
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
<p>interface Vlan20</p>
<p> ip address 10.20.0.2 255.255.255.0</p>
<p>!</p>
<p>ip default-gateway 10.20.0.1</p>
<p>!</p>
<p>banner motd ^C Unauthorized access is strictly prohibited. ^C</p>
<p>!</p>
<p>line con 0</p>
<p> password 7 0822455D0A16</p>
<p>!</p>
<p>line vty 0 4</p>
<p> password 7 0822455D0A16</p>
<p> login local</p>
<p> transport input ssh</p>
<p>line vty 5</p>
<p> password 7 0822455D0A16</p>
<p> login local</p>
<p> transport input ssh</p>
<p>line vty 6 15</p>
<p> password 7 0822455D0A16</p>
<p> login</p>
<p>!</p>
<p>end</p>
<p></p>
</blockquote>


S2#sh run
<blockquote>
<p>Building configuration...</p>
<p></p>
<p>Current configuration : 3185 bytes</p>
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
<p>ip domain-name ccna-lab.com</p>
<p>!</p>
<p>username SSHadmin privilege 1 password 7 08654F471A1A0A4640584D</p>
<p>!</p>
<p>spanning-tree mode pvst</p>
<p>spanning-tree extend system-id</p>
<p>!</p>
<p>interface FastEthernet0/1</p>
<p> switchport trunk native vlan 1000</p>
<p> switchport trunk allowed vlan 20,30,40,1000</p>
<p> switchport mode trunk</p>
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
<p> switchport access vlan 20</p>
<p> switchport mode access</p>
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
<p> switchport access vlan 40</p>
<p> switchport mode access</p>
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
<p>interface Vlan20</p>
<p> ip address 10.20.0.3 255.255.255.0</p>
<p>!</p>
<p>ip default-gateway 10.20.0.1</p>
<p>!</p>
<p>banner motd ^C Unauthorized access is strictly prohibited. ^C</p>
<p>!</p>
<p>line con 0</p>
<p> password 7 0822455D0A16</p>
<p>!</p>
<p>line vty 0 4</p>
<p> password 7 0822455D0A16</p>
<p> login local</p>
<p> transport input ssh</p>
<p>line vty 5</p>
<p> password 7 0822455D0A16</p>
<p> login local</p>
<p> transport input ssh</p>
<p>line vty 6 15</p>
<p> password 7 0822455D0A16</p>
<p> login</p>
<p>!</p>
<p>end</p>
<p></p>
</blockquote>
