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
Building configuration...

Current configuration : 930 bytes
!
version 15.4
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname R1
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
!
ip cef
no ipv6 cef
!
username admin password 7 0800484358173537475E
!
no ip domain-lookup
ip domain-name otus
!
spanning-tree mode pvst

!
interface GigabitEthernet0/0/0
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface GigabitEthernet0/0/1
 ip address 192.168.1.1 255.255.255.0
 duplex auto
 speed auto
!
interface Vlan1
 no ip address
 shutdown
!
ip classless
!
ip flow-export version 9
!
no cdp run
!
banner motd ^C Unauthorized access is strictly prohibited. ^C
!
line con 0
 password 7 0822455D0A16
!
line aux 0
!
line vty 0 4
 password 7 0822455D0A16
 login local
 transport input ssh
line vty 5 15
 password 7 0822455D0A16
 login local
 transport input ssh
!
end

Конфигурация коммутатора:

S1#sh run
Building configuration...

Current configuration : 1459 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname S1
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
!
no ip domain-lookup
ip domain-name otus
!
username admin privilege 1 password 7 0800484358173537475E
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
!
interface FastEthernet0/2
!
interface FastEthernet0/3
!
interface FastEthernet0/4
!
interface FastEthernet0/5
!
interface FastEthernet0/6
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface Vlan1
 ip address 192.168.1.11 255.255.255.0
!
ip default-gateway 192.168.1.1
!
banner motd ^C Unauthorized access is strictly prohibited. ^C
!
line con 0
 password 7 0822455D0A16
!
line vty 0 4
 password 7 0822455D0A16
 login local
 transport input ssh
line vty 5 15
 password 7 0822455D0A16
 login local
 transport input ssh
!
end