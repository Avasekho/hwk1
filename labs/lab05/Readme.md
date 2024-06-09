<h1> Лабораторная работа. Доступ к сетевым устройствам по протоколу SSH </h1> 

<h2> Топология </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab05/topology.png>

<h2> Таблица адресации </h2>

| Устройство | Интерфейс |   IP-адрес   |  Маска подсети  | Шлюз по умолчанию |
|:----------:|:---------:|:------------:|:---------------:|:-----------------:|
| R1         | G0/0/1    | 192.168.1.1  | 255.255.255.0   |         -         |
| S1         | VLAN 1    | 192.168.1.11 | 255.255.255.0   | 192.168.1.1       |
| PC-A       | NIC       | 192.168.1.3  | 255.255.255.0   | 192.168.1.1       |

<h2> Задачи </h2>

<ol>
  <li> Настройка основных параметров устройства. </li>
  <li> Настройка маршрутизатора для доступа по протоколу SSH. </li>
  <li> Настройка коммутатора для доступа по протоколу SSH. </li>
  <li> SSH через интерфейс командной строки (CLI) коммутатора. </li>
</ol>


<h2> Часть 1. Настройка основных параметров устройств</h2> 

Настроим маршрутизатор:

a.	Подключитесь к маршрутизатору с помощью консоли и активируйте привилегированный режим EXEC.
b.	Войдите в режим конфигурации.
<p> enable >> configure terminal </p>
c.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.
<p> > no ip domain-lookup </p>
d.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
<p> > enable secret class </p>
e.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.
<p> > line console 0 </p>
<p> > password cisco </p>
f.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.
<p> > line vty 0 15 </p>
<p> > password cisco </p>
g.	Зашифруйте открытые пароли.
<p> > service password-encryption </p>
h.	Создайте баннер, который предупреждает о запрете несанкционированного доступа.
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>
i.	Настройте и активируйте на маршрутизаторе интерфейс G0/0/1, используя информацию, приведенную в таблице адресации.

interface g0/0/1
ip address 192.168.1.1 255.255.255.0
no shutdown

j.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
<p> > copy running-config startup-config </p>

Настроим сетевой интерфейс на ПК:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab05/pc_1.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab05/pc_2.png>

Проверим доступность маршрутизатора:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab05/ping_1.png>


<h2> Часть 2. Настройка маршрутизатора для доступа по протоколу SSH </h2>

Настроим аутентификацию устройств:

<p> > hostname R1 </p>
<p> > ip domain-name otus </p>

Создадим ключ шифрования с указанием его длины:

<p> > crypto key generate rsa general-keys modulus 2048 </p>

Создадим имя пользователя в локальной базе учетных записей:

<p> > username admin password Adm1nP@55 </p>

Активируем протокол SSH на линиях VTY:

<p> > line vty 0 15 </p>
<p> > transport input ssh </p>

// одновременно разрешить SSH и Telnet с помощью команды transport input ssh telnet Packet Tracer не дает - команду не принимает, выдаает ошибку

Добавим проверку пользователей по локальной базе учетных записей:

<p> > line vty 0 15 </p>
<p> > login local </p>

Сохраним текущую конфигурацию в файл загрузочной конфигурации:
<p> > copy running-config startup-config </p>

Проверим возможность соединения по SSH:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab05/ssh_1.png>

Маршрутизатор доступен.


<h2> Часть 3. Настройка коммутатора для доступа по протоколу SSH </h2>

Настроим коммутатор:

a.	Подключитесь к маршрутизатору с помощью консоли и активируйте привилегированный режим EXEC.
b.	Войдите в режим конфигурации.
<p> enable >> configure terminal </p>
c.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.
<p> > no ip domain-lookup </p>
d.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
<p> > enable secret class </p>
e.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.
<p> > line console 0 </p>
<p> > password cisco </p>
f.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.
<p> > line vty 0 15 </p>
<p> > password cisco </p>
g.	Зашифруйте открытые пароли.
<p> > service password-encryption </p>
h.	Создайте баннер, который предупреждает о запрете несанкционированного доступа.
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>
i.	Настройте и активируйте на коммутаторе интерфейс VLAN 1, используя информацию, приведенную в таблице адресации.

<p> > interface Vlan1 </p>
<p> > ip address 192.168.1.11 255.255.255.0 </p>
<p> > ip default-gateway 192.168.1.1 </p>
<p> > no shutdown </p>

j.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
<p> > copy running-config startup-config </p>

Настроим коммутатор для соединения по протоколу SSH:

a.	Настройте имя устройства, как указано в таблице адресации.
<p> enable >> configure terminal </p>
<p> > hostname S1 </p>
b.	Задайте домен для устройства.
<p> > ip domain-name otus </p>
c.	Создайте ключ шифрования с указанием его длины.
<p> > crypto key generate rsa general-keys modulus 2048 </p>
d.	Создайте имя пользователя в локальной базе учетных записей.
<p> > username admin password Adm1nP@55 </p>
e.	Активируйте протоколы Telnet и SSH на линиях VTY.
<p> > line vty 0 15 </p>
<p> > transport input ssh </p>
f.	Измените способ входа в систему таким образом, чтобы использовалась проверка пользователей по локальной базе учетных записей.
<p> > line vty 0 15 </p>
<p> > login local </p>

Сохраним текущую конфигурацию в файл загрузочной конфигурации:
<p> > copy running-config startup-config </p>

Проверим возможность соединения по SSH:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab05/ssh_2.png>

Коммутатор доступен.


<h2> Часть 4. Настройка протокола SSH с использованием интерфейса командной строки (CLI) коммутатора </h2>

Посмотрим доступные параметры для клиента SSH в Cisco IOS:
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab05/ssh_3.png>

Как видим, в Packet Tracer на коммутаторе с IOS version 15.0 набор параметров несколько ограничен.


Установим с коммутатора S1 соединение с маршрутизатором R1 по протоколу SSH:

<p> > ssh -l admin 192.168.1.1 </p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab05/ssh_4.png>

Маршрутизатор доступен.

Переключаемся с удаленного устройства на коммутатор с помощью Ctrl+Shift+6 и "x":
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab05/ssh_5.png>

Завершение работы с удаленным устройством с помощью команды exit:
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab05/ssh_6.png>


Конфигурация маршрутизатора:

R1#sh run
<p>Building configuration... </p>
<p> </p>
<p>Current configuration : 930 bytes </p>
<p>! </p>
<p>version 15.4 </p>
<p>no service timestamps log datetime msec </p>
<p>no service timestamps debug datetime msec </p>
<p>service password-encryption </p>
<p>! </p>
<p>hostname R1 </p>
<p>! </p>
<p>enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1 </p>
<p>! </p>
<p>ip cef </p>
<p>no ipv6 cef </p>
<p>! </p>
<p>username admin password 7 0800484358173537475E </p>
<p>! </p>
<p>no ip domain-lookup </p>
<p>ip domain-name otus </p>
<p>! </p>
<p>spanning-tree mode pvst </p>
<p> </p>
<p>! </p>
<p>interface GigabitEthernet0/0/0 </p>
<p> no ip address </p>
<p> duplex auto </p>
<p> speed auto </p>
<p> shutdown </p>
<p>! </p>
<p>interface GigabitEthernet0/0/1 </p>
<p> ip address 192.168.1.1 255.255.255.0 </p>
<p> duplex auto </p>
<p> speed auto </p>
<p>! </p>
<p>interface Vlan1 </p>
<p> no ip address </p>
<p> shutdown </p>
<p>! </p>
<p>ip classless </p>
<p>! </p>
<p>ip flow-export version 9 </p>
<p>! </p>
<p>no cdp run </p>
<p>! </p>
<p>banner motd ^C Unauthorized access is strictly prohibited. ^C </p>
<p>! </p>
<p>line con 0 </p>
<p> password 7 0822455D0A16 </p>
<p>! </p>
<p>line aux 0 </p>
<p>! </p>
<p>line vty 0 4 </p>
<p> password 7 0822455D0A16 </p>
<p> login local </p>
<p> transport input ssh </p>
<p>line vty 5 15 </p>
<p> password 7 0822455D0A16 </p>
<p> login local </p>
<p> transport input ssh </p>
<p>! </p>
<p>end </p>

Конфигурация коммутатора:

S1#sh run
<p>Building configuration... </p>
<p> </p>
<p>Current configuration : 1459 bytes </p>
<p>! </p>
<p>version 15.0 </p>
<p>no service timestamps log datetime msec </p>
<p>no service timestamps debug datetime msec </p>
<p>service password-encryption </p>
<p>! </p>
<p>hostname S1 </p>
<p>! </p>
<p>enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1 </p>
<p>! </p>
<p>no ip domain-lookup </p>
<p>ip domain-name otus </p>
<p>! </p>
<p>username admin privilege 1 password 7 0800484358173537475E </p>
<p>! </p>
<p>spanning-tree mode pvst </p>
<p>spanning-tree extend system-id </p>
<p>! </p>
<p>interface FastEthernet0/1 </p>
<p>! </p>
<p>interface FastEthernet0/2 </p>
<p>! </p>
<p>interface FastEthernet0/3 </p>
<p>! </p>
<p>interface FastEthernet0/4 </p>
<p>! </p>
<p>interface FastEthernet0/5 </p>
<p>! </p>
<p>interface FastEthernet0/6 </p>
<p>! </p>
<p>interface FastEthernet0/7 </p>
<p>! </p>
<p>interface FastEthernet0/8 </p>
<p>! </p>
<p>interface FastEthernet0/9 </p>
<p>! </p>
<p>interface FastEthernet0/10 </p>
<p>! </p>
<p>interface FastEthernet0/11 </p>
<p>! </p>
<p>interface FastEthernet0/12 </p>
<p>! </p>
<p>interface FastEthernet0/13 </p>
<p>! </p>
<p>interface FastEthernet0/14 </p>
<p>! </p>
<p>interface FastEthernet0/15 </p>
<p>! </p>
<p>interface FastEthernet0/16 </p>
<p>! </p>
<p>interface FastEthernet0/17 </p>
<p>! </p>
<p>interface FastEthernet0/18 </p>
<p>! </p>
<p>interface FastEthernet0/19 </p>
<p>! </p>
<p>interface FastEthernet0/20 </p>
<p>! </p>
<p>interface FastEthernet0/21 </p>
<p>! </p>
<p>interface FastEthernet0/22 </p>
<p>! </p>
<p>interface FastEthernet0/23 </p>
<p>! </p>
<p>interface FastEthernet0/24 </p>
<p>! </p>
<p>interface GigabitEthernet0/1 </p>
<p>! </p>
<p>interface GigabitEthernet0/2 </p>
<p>! </p>
<p>interface Vlan1 </p>
<p> ip address 192.168.1.11 255.255.255.0 </p>
<p>! </p>
<p>ip default-gateway 192.168.1.1 </p>
<p>! </p>
<p>banner motd ^C Unauthorized access is strictly prohibited. ^C </p>
<p>! </p>
<p>line con 0 </p>
<p> password 7 0822455D0A16 </p>
<p>! </p>
<p>line vty 0 4 </p>
<p> password 7 0822455D0A16 </p>
<p> login local </p>
<p> transport input ssh </p>
<p>line vty 5 15 </p>
<p> password 7 0822455D0A16 </p>
<p> login local </p>
<p> transport input ssh </p>
<p>! </p>
<p>end </p>