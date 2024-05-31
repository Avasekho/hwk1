<h1> Лабораторная работа. Настройка NAT для IPv4 </h1> 

<h2> Топология </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/topology.png>

<h2> Таблица адресации </h2>

| Устройство | Интерфейс |      IP-адрес      |   Маска подсети    |
|:----------:|:---------:|:------------------:|:------------------:|
| R1         | G0/0/0    | 209.165.200.230    | 255.255.255.248    |
|            | G0/0/1    | 192.168.1.1        | 255.255.255.0      |
| R2         | G0/0/0    | 209.165.200.225    | 255.255.255.248    |
|            | Lo1       | 209.165.200.1      | 255.255.255.224    |
| S1         | VLAN 1    | 192.168.1.11       | 255.255.255.0      |
| S2         | VLAN 1    | 192.168.1.12       | 255.255.255.0      |
| PC-A       | NIC       | 192.168.1.2        | 255.255.255.0      |
| PC-B       | NIC       | 192.168.1.3        | 255.255.255.0      |

<h2> Задачи </h2>

<ol>
  <li> Создание сети и настройка основных параметров устройства. </li>
  <li> Настройка и проверка NAT для IPv4. </li>
  <li> Настройка и проверка PAT для IPv4. </li>
  <li> Настройка и проверка статического NAT для IPv4. </li>
</ol>

<h2> Создание сети и настройка основных параметров устройства.</h2> 
<h2> Конфигурация маршрутизатора по умолчанию (R2):</h2> 

a.	Назначьте маршрутизатору имя устройства.
<p> enable >> configure terminal </p>
<p> > hostname R2 </p>
b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.
<p> > no ip domain-lookup </p>
c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
<p> > enable secret class </p>
d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.
<p> > line console 0 </p>
<p> > password cisco </p>
e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.
<p> > line vty 0 15 </p>
<p> > password cisco </p>
f.	Зашифруйте открытые пароли.
<p> > service password-encryption </p>
g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>
h.	Настройте IP-адресации интерфейса, как указано в таблице выше.

interface g0/0/0
ip address 209.165.200.225 255.255.255.248

Добавляем loopback интерфейс

interface loopback 1
ip address 209.165.200.1 255.255.255.224

i.	Настройте маршрут по умолчанию. от R2 до  R1.
ip default-gateway 209.165.200.230

j.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
<p> > copy running-config startup-config </p>

Аналогично настройка для R1

<h2> Конфигурация коммутатора по умолчанию (S1):</h2> 

a.	Присвойте коммутатору имя устройства.
<p> enable >> configure terminal </p>
<p> > hostname S1 </p>

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.
<p> > no ip domain-lookup </p>

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
<p> > enable secret class </p>

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.
<p> > line console 0 </p>
<p> > password cisco </p>

e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.
<p> > line vty 0 15 </p>
<p> > password cisco </p>

f.	Зашифруйте открытые пароли.
<p> > service password-encryption </p>

g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>

h.	Настройте IP-адресации интерфейса, как указано в таблице выше.

<p> > interface Vlan1 </p>
<p> > ip address 192.168.1.11 255.255.255.0 </p>
<p> > no shutdown </p>

h.	Выключите все интерфейсы, которые не будут использоваться.

<p> > interface range f0/4 - 17 </p>
<p> > shutdown </p>
<p> > interface range f0/19 - 24 </p>
<p> > shutdown </p>
<p> > interface range g0/1 - 2 </p>
<p> > shutdown </p>

j.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
<p> > copy running-config startup-config </p>

Аналогично для коммутатора S2 

<h2> Настройка и проверка NAT для IPv4. </h2>

Шаг 1. Настроим NAT на R1, используя пул из трех адресов 209.165.200.226-209.165.200.228. 

Добавляем подсеть 192.168.1.0/24 в список доступа, заполняем пул адресов для NAT и привязываем пул к списку доступа

<p> > access-list 1 permit 192.168.1.0 0.0.0.255 </p>
<p> > ip nat pool PUBLIC_ACCESS 209.165.200.226 209.165.200.228 netmask 255.255.255.248 </p>
<p> > ip nat inside source list 1 pool PUBLIC_ACCESS </p>

Определяем внутренний интерфейс

<p> > interface g0/0/1 </p>
<p> > ip nat inside </p>

Определяем внешний интерфейс

<p> > interface g0/0/0 </p>
<p> > ip nat outside </p>

Попробуем проверить конфигурацию NAT:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_err_1.png>

Не работает :/

Пинги с PC-B до R2 не проходят, внутренний интерфейс R1 доступен, таблица NAT пуста, по статистике все в misses.
G0/0/0 на R2 c R1 пингуется.

Как выяснилось на ПК забыл указать шлюз по умолчанию. После этого стал доступен пинг до интерфейса R2, но loopback все еще не пингуется. 

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_err_2.png>

По всей видимости R1 не знает где искать этот адрес.

Пропишем статический адрес куда слать пакеты для 209.165.200.1:

<p> > ip route 209.165.200.0 255.255.255.224 GigabitEthernet0/0/0 </p>

Пинги от PC-B до loopback интерфейса пошли

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_1.png>

NAT-таблица также стала наполняться записями

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/nat_1.png>


Проверим пинги с PC-B до loopback:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_err_3.png>

Не ходит потому что тоже не указан шлюз по умолчанию. После того как настройки были поправлены - пинг идет, NAT-таблица содержит соответствующие записи

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_2.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/nat_2.png>

Попробуем попинговать 209.165.200.1 с коммутатора S1:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_err_4.png>

Ожидаемо не идет, т.к. на коммутаторе не был задан шлюз по умолчанию. Добавим шлюз:

<p> > ip default-gateway 192.168.1.1 </p>

Пинг пошел, таблица наполняется адресами. Все три адреса из пула NAT при деле.

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_3.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/nat_3.png>

Теперь попробуем попинговать 209.165.200.1 с коммутатора S2:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_err_5.png>

Ожидаемо, адреса в пуле закончились.

Команда "show ip nat translations verbose" работать отказывается

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/verbose_err1.png>


Очистим записи трансляций

<p> > clear ip nat translation * </p>


<h2>  Настройка и проверка PAT для IPv4 </h2>

Заменим настройки преобразования с NAT на PAT. Для этого уддалим старые настройки NAT на R1

<p> > no ip nat inside source list 1 pool PUBLIC_ACCESS </p>

И добавим новые

<p> > ip nat inside source list 1 pool PUBLIC_ACCESS overload </p>

Попробуем достучаться с PC-B до Lo1 - все проходит, адрес преобразуется

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_4.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/pat_1.png>


Аналогично с PC-A

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_5.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/pat_2.png>


Попробуем пустить ping сразу с двух устройств - R1 преобразует их в один адрес из пула, на разные порты

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/pat_3.png>

Очистим записи трансляций

<p> > clear ip nat translation * </p>


Теперь удалим команды преобразования nat pool

<p> > no ip nat inside source list 1 pool PUBLIC_ACCESS overload </p>
<p> > no ip nat pool PUBLIC_ACCESS </p>

Добавим команду PAT, связав её с внешним интерфейсом

 <p> > ip nat inside source list 1 interface g0/0/0 overload </p>

 Проверим работу трансляции адресов:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/pat_4.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/pat_5.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/pat_6.png>

Теперь R1 преобразует внутренние адреса в адрес внешнего интерфейса


<h2>  Настройка и проверка статического NAT для IPv4. </h2>


Очистим записи трансляций

<p> > clear ip nat translation * </p>


Настроим статическое сопоставление внутреннего адреса с внешним

<p> > ip nat inside source static 192.168.1.2 209.165.200.229 </p>

Проверим коррепктность работы трансляции

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_6.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/nat_4.png>

Также адрес 209.165.200.229 доступен для пинга с R2

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_7.png>

Таким образом, статический NAT работает