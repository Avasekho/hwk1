<h1> Лабораторная работа. Развертывание коммутируемой сети с резервными каналами </h1> 

<h2> Топология </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab07/topology.png>

<h2> Таблица адресации </h2>

| Устройство |  Интерфейс  |   IP-адрес   | Маска подсети |
|:----------:|:-----------:|:------------:|:-------------:|
| S1         | VLAN 1      | 192.168.1.1  | 255.255.255.0 |
| S2         | VLAN 1      | 192.168.1.2  | 255.255.255.0 |
| S3         | VLAN 1      | 192.168.1.3  | 255.255.255.0 |


<h2> Таблица VLAN </h2>

|   VLAN   |     Имя      |     Назначенный интерфейс     |
|:--------:|:------------:|:-----------------------------:|
| 10       | Управление   | S1: VLAN 10                   | 
|          |              | S2: VLAN 10                   |
| 20       | Sales        | S1: F0/6                      |
| 30       | Operations   | S2: F0/18                     |
| 999      | Parking_Lot  | S1: F0/2-4, F0/7-24, G0/1-2   |
|          |              | S2: F0/2-17, F0/19-24, G0/1-2 |
| 1000     | Собственная  |               -               |

<h2> Задачи </h2>

<ol>
  <li> Создание сети и настройка основных параметров устройства. </li>
  <li> Выбор корневого моста. </li>
  <li> Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов. </li>
  <li> Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов. </li>
</ol>

<h2> Часть 1. Создание сети и настройка основных параметров устройства </h2>

Настроим базовые параметры для коммутаторов:

<blockquote>
a.	Отключите поиск DNS.
</blockquote>
<p> > no ip domain-lookup </p>

<blockquote>
b.	Присвойте имена устройствам в соответствии с топологией.
</blockquote>
<p> hostname S1 </p>
<p> hostname S2 </p>
<p> hostname S3 </p>

<blockquote>
c.	Назначьте class в качестве зашифрованного пароля доступа к привилегированному режиму.
</blockquote>
<p> > enable secret class </p>

<blockquote>
d.	Назначьте cisco в качестве паролей консоли и VTY и активируйте вход для консоли и VTY каналов.
</blockquote>
<p> > line console 0 </p>
<p> > password cisco </p>
<p> > line vty 0 15 </p>
<p> > password cisco </p>
<p> > service password-encryption </p>

<blockquote>
e.	Настройте logging synchronous для консольного канала.
</blockquote>
<p> > line console 0 </p>
<p> > logging synchronous </p>

<blockquote>
f.	Настройте баннерное сообщение дня (MOTD) для предупреждения пользователей о запрете несанкционированного доступа.
</blockquote>
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>

<blockquote>
g.	Задайте IP-адрес, указанный в таблице адресации для VLAN 1 на всех коммутаторах.
</blockquote>
<p> > interface vlan 1 </p>
<p> > ip address 192.168.1.1 255.255.255.0 </p>
<p> > no shutdown </p>
// аналогично на других коммутаторах

<blockquote>
h.	Скопируйте текущую конфигурацию в файл загрузочной конфигурации.
</blockquote>
<p> > copy running-config startup-config </p>

Проверим связь:

<blockquote>
Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S2?
</blockquote>
Выполняется успешно
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab07/ping_1.png>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab07/trace_1.png>

<blockquote>
Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S3?
</blockquote>
Выполняется успешно
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab07/ping_2.png>

<blockquote>
Успешно ли выполняется эхо-запрос от коммутатора S2 на коммутатор S3?
</blockquote>
Выполняется успешно
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab07/ping_3.png>


<h2> Часть 2. Определение корневого моста </h2>

Отключим все порты на коммутаторах

<p> > interface range f0/1-24 </p>
<p> > shutdown </p>
<p> > interface range g0/1-2 </p>
<p> > shutdown </p>
// аналогично на других коммутаторах

Настроим подключенные порты в качестве транковых:

<p> > interface range F0/1-4 </p>
<p> > switchport trunk allowed vlan 1 </p>
// аналогично на других коммутаторах

Включим порты F0/2 и F0/4 на всех коммутаторах:

<p> > interface F0/2 </p>
<p> > no shutdown </p>
<p> > interface F0/4 </p>
<p> > no shutdown </p>
// аналогично на других коммутаторах

Отобразим данные протокола spanning-tree:

S1:
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab07/stp_1.png>

S2:
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab07/stp_2.png>

S3:
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab07/stp_3.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab07/topology_2.png>

<blockquote>
Какой коммутатор является корневым мостом?
</blockquote>
S3

<blockquote>
Почему этот коммутатор был выбран протоколом spanning-tree в качестве корневого моста?
</blockquote>



<blockquote>
Какие порты на коммутаторе являются корневыми портами?
</blockquote>



<blockquote>
Какие порты на коммутаторе являются назначенными портами?
</blockquote>



<blockquote>
Какой порт отображается в качестве альтернативного и в настоящее время заблокирован?
</blockquote>




<blockquote>
Почему протокол spanning-tree выбрал этот порт в качестве невыделенного (заблокированного) порта?
</blockquote>



