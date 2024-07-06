<h1> Лабораторная работа. Настройка протокола OSPFv2 для одной области </h1> 

<h2> Топология </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/topology.png>

<h2> Таблица адресации </h2>

| Устройство | Интерфейс |   IP-адрес   |  Маска подсети  |
|:----------:|:---------:|:------------:|:---------------:|
| R1         | G0/0/1    | 10.53.0.1    | 255.255.255.0   |
|            | Loopback1 | 172.16.1.1   | 255.255.255.0   |
| R2         | G0/0/1    | 10.53.0.2    | 255.255.255.0   |
|            | Loopback1 | 192.168.1.1  | 255.255.255.0   |

<h2> Задачи </h2>

<ol>
  <li> Создание сети и настройка основных параметров устройства. </li>
  <li> Настройка и проверка базовой работы протокола  OSPFv2 для одной области. </li>
  <li> Оптимизация и проверка конфигурации OSPFv2 для одной области. </li>
</ol>


<h2> Часть 1. Создание сети и настройка основных параметров устройства</h2>

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


<h2> Часть 2. Настройка и проверка базовой работы протокола OSPFv2 для одной области</h2>


<h2> Настроим адреса интерфейса и базового OSPFv2 на каждом маршрутизаторе.</h2>


<blockquote>
a.	Настройте адреса интерфейсов на каждом маршрутизаторе, как показано в таблице адресации выше.
</blockquote>

R1:
<p> > interface G0/0/1</p>
<p> > ip address 10.53.0.1 255.255.255.0</p>
<p> > no shutdown</p>
<p> > interface loopback 1</p>
<p> > ip address 172.16.1.1 255.255.255.0</p>

R2:
<p> > interface G0/0/1</p>
<p> > ip address 10.53.0.2 255.255.255.0</p>
<p> > no shutdown</p>
<p> > interface loopback 1</p>
<p> > ip address 192.168.1.1 255.255.255.0</p>


<blockquote>
b.	Перейдите в режим конфигурации маршрутизатора OSPF, используя идентификатор процесса 56.
</blockquote>
<p> > router ospf 56 </p>

<blockquote>
c.	Настройте статический идентификатор маршрутизатора для каждого маршрутизатора (1.1.1.1 для R1, 2.2.2.2 для R2).
</blockquote>
R1:
<p> > router-id 1.1.1.1 </p>

R2:
<p> > router-id 2.2.2.2 </p>

<blockquote>
d.	Настройте инструкцию сети для сети между R1 и R2, поместив ее в область 0.
</blockquote>
R1:
<p> > network 10.53.0.0 0.0.0.255 area 0 </p>

R2:
<p> > network 10.53.0.0 0.0.0.255 area 0 </p>

<blockquote>
e.	(Только на R2) добавьте конфигурацию, необходимую для объявления сети Loopback 1 в область OSPF 0.
</blockquote>
<p> > network 192.168.1.0 0.0.0.255 area 0 </p>

<blockquote>
f.	Убедитесь, что OSPFv2 работает между маршрутизаторами. Выполните команду, чтобы убедиться, что R1 и R2 сформировали смежность.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/ospf_1.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/ospf_2.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/ospf_3.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/ospf_4.png>


<blockquote>
Какой маршрутизатор является DR? Какой маршрутизатор является BDR? Каковы критерии отбора?
</blockquote>
<p>DR - R2; BDR - R1.</p>
<p>Критерии отбора:</p>
<p>- DR становится роутер с наивысшим приоритетом</p>
<p>- Если приоритеты равны, DR становится роутер с бОльшим значением Router-ID</p>
<p>- Роутер со вторым по старшинству значением приоритета или Router-ID становится BDR.</p>


<blockquote>
g.	На R1 выполните команду show ip route ospf, чтобы убедиться, что сеть R2 Loopback1 присутствует в таблице маршрутизации.
Обратите внимание, что поведение OSPF по умолчанию заключается в объявлении интерфейса обратной связи в качестве маршрута узла с использованием 32-битной маски.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/ospf_5.png>


<blockquote>
h.	Запустите Ping до  адреса интерфейса R2 Loopback 1 из R1. Выполнение команды ping должно быть успешным.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/ping_1.png>

Пинг идет


<h2> Часть 3. Оптимизация и проверка конфигурации OSPFv2 для одной области</h2>


<h2> Реализация различных оптимизаций на каждом маршрутизаторе.</h2>

<blockquote>
a.	На R1 настройте приоритет OSPF интерфейса G0/0/1 на 50, чтобы убедиться, что R1 является назначенным маршрутизатором.
</blockquote>

<p> > interface g0/0/1</p>
<p> > ip ospf priority 50</p>


<blockquote>
b.	Настройте таймеры OSPF на G0/0/1 каждого маршрутизатора для таймера приветствия, составляющего 30 секунд.
</blockquote>

<p> > interface g0/0/1</p>
<p> > ip ospf hello-interval 30</p>


<blockquote>
c.	На R1 настройте статический маршрут по умолчанию, который использует интерфейс Loopback 1 в качестве интерфейса выхода. Затем распространите маршрут по умолчанию в OSPF. Обратите внимание на сообщение консоли после установки маршрута по умолчанию.
</blockquote>

<p> > ip route 0.0.0.0 0.0.0.0 loopback 1</p>
<p> > router ospf 56</p>
<p> > default-information originate</p>


<blockquote>
d.	добавьте конфигурацию, необходимую для OSPF для обработки R2 Loopback 1 как сети точка-точка. Это приводит к тому, что OSPF объявляет Loopback 1 использует маску подсети интерфейса.
</blockquote>

// Уберем сеть 192.168.1.0/24 и добавим сам интерфейс Loopback 1.
<p> > router ospf 56</p>
<p> > no network 192.168.1.0 0.0.0.255 area 0 </p>
<p> > interface loopback 1</p>
<p> > ip ospf 56 area 0</p>


<blockquote>
e.	(Только на R2) добавьте конфигурацию, необходимую для предотвращения отправки объявлений OSPF в сеть Loopback 1.
</blockquote>

<p> > router ospf 56</p>
<p> > passive-interface loopback 1</p>

<blockquote>
f.	Измените базовую пропускную способность для маршрутизаторов. После этой настройки перезапустите OSPF с помощью команды clear ip ospf process . Обратите внимание на сообщение консоли после установки новой опорной полосы пропускания.
</blockquote>

R1
<p> > router ospf 56</p>
<p> > auto-cost reference-bandwidth 10000</p>
<p> > clear ip ospf process</p>

R2
<p> > router ospf 56</p>
a<p> > auto-cost reference-bandwidth 10000</p>
<p> > clear ip ospf process</p>
// OSPF просит поставить идентичную скорость на всех маршрутизаторах

<h2> Убедимся, что оптимизация OSPFv2 реализовалась</h2>


<blockquote>
a.	Выполните команду show ip ospf interface g0/0/1 на R1 и убедитесь, что приоритет интерфейса установлен равным 50, а временные интервалы — Hello 30, Dead 120, а тип сети по умолчанию — Broadcast
</blockquote>

<p> Добавим dead-interval 120</p>
<p> > interface g0/0/1</p>
<p> > ip ospf dead-interval 120</p>
// И то же самое на R2 иначе все отвалится

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/ospf_6.png>


<blockquote>
b.	На R1 выполните команду show ip route ospf, чтобы убедиться, что сеть R2 Loopback1 присутствует в таблице маршрутизации. Обратите внимание на разницу в метрике между этим выходным и предыдущим выходным. 
Также обратите внимание, что маска теперь составляет 24 бита, в отличие от 32 битов, ранее объявленных.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/route_2.png>


<blockquote>
c.	Введите команду show ip route ospf на маршрутизаторе R2. Единственная информация о маршруте OSPF должна быть распространяемый по умолчанию маршрут R1.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/route_1.png>


<blockquote>
d.	Запустите Ping до адреса интерфейса R1 Loopback 1 из R2. Выполнение команды ping должно быть успешным.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/ping_2.png>

Пинг идет

<blockquote>
Вопрос:
Почему стоимость OSPF для маршрута по умолчанию отличается от стоимости OSPF в R1 для сети 192.168.1.0/24?
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/route_3.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/route_4.png>

<p>R2 получает маршрут с R1, для которого маршрут считается как loopback со стоимостью 1.</p>
<p>R1 получает маршрут до 192.168.1.0/24 по OSPF который состоит из интерфейса G0/0/1 (стоимостью 100) и Loopback (стоимостью 1) - 100 + 1.</p>


Конфигурация маршрутизатора:


R1# sh run
<blockquote>
<p>Building configuration...</p>
<p></p>
<p>Current configuration : 1125 bytes</p>
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
<p> ip address 10.53.0.1 255.255.255.0</p>
<p> ip ospf hello-interval 30</p>
<p> ip ospf dead-interval 120</p>
<p> ip ospf priority 50</p>
<p> duplex auto</p>
<p> speed auto</p>
<p>!</p>
<p>interface Vlan1</p>
<p> no ip address</p>
<p> shutdown</p>
<p>!</p>
<p>router ospf 56</p>
<p> router-id 1.1.1.1</p>
<p> log-adjacency-changes</p>
<p> auto-cost reference-bandwidth 10000</p>
<p> network 10.53.0.0 0.0.0.255 area 0</p>
<p> default-information originate</p>
<p>!</p>
<p>ip classless</p>
<p>ip route 0.0.0.0 0.0.0.0 Loopback1 </p>
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


R2#sh run
<blockquote>
<p>Building configuration...</p>
<p></p>
<p>Current configuration : 1086 bytes</p>
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
<p>no ip domain-lookup</p>
<p>!</p>
<p>spanning-tree mode pvst</p>
<p>!</p>
<p>interface Loopback1</p>
<p> ip address 192.168.1.1 255.255.255.0</p>
<p> ip ospf 56 area 0</p>
<p>!</p>
<p>interface GigabitEthernet0/0/0</p>
<p> no ip address</p>
<p> duplex auto</p>
<p> speed auto</p>
<p> shutdown</p>
<p>!</p>
<p>interface GigabitEthernet0/0/1</p>
<p> ip address 10.53.0.2 255.255.255.0</p>
<p> ip ospf hello-interval 30</p>
<p> ip ospf dead-interval 120</p>
<p> duplex auto</p>
<p> speed auto</p>
<p>!</p>
<p>interface Vlan1</p>
<p> no ip address</p>
<p> shutdown</p>
<p>!</p>
<p>router ospf 56</p>
<p> router-id 2.2.2.2</p>
<p> log-adjacency-changes</p>
<p> passive-interface Loopback1</p>
<p> auto-cost reference-bandwidth 10000</p>
<p> network 10.53.0.0 0.0.0.255 area 0</p>
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