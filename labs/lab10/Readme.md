<h1> ������������ ������. ��������� ��������� OSPFv2 ��� ����� ������� </h1> 

<h2> ��������� </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/topology.png>

<h2> ������� ��������� </h2>

| ���������� | ��������� |   IP-�����   |  ����� �������  |
|:----------:|:---------:|:------------:|:---------------:|
| R1         | G0/0/1    | 10.53.0.1    | 255.255.255.0   |
|            | Loopback1 | 172.16.1.1   | 255.255.255.0   |
| R2         | G0/0/1    | 10.53.0.2    | 255.255.255.0   |
|            | Loopback1 | 192.168.1.1  | 255.255.255.0   |

<h2> ������ </h2>

<ol>
  <li> �������� ���� � ��������� �������� ���������� ����������. </li>
  <li> ��������� � �������� ������� ������ ���������  OSPFv2 ��� ����� �������. </li>
  <li> ����������� � �������� ������������ OSPFv2 ��� ����� �������. </li>
</ol>


<h2> ����� 1. �������� ���� � ��������� �������� ���������� ����������</h2>

<h2>�������� ��������������:</h2>

<blockquote>
a.	��������� �������������� ��� ����������.
</blockquote>
<p> hostname R1 </p>

<blockquote>
b.	��������� ����� DNS, ����� ������������� ������� �������������� ������� ��������������� ��������� ������� ����� �������, ��� ����� ��� �������� ������� �����.
</blockquote>
<p> > no ip domain-lookup </p>

<blockquote>
c.	��������� class � �������� �������������� ������ ������������������ ������ EXEC.
</blockquote>
<p> > enable secret class </p>

<blockquote>
d.	��������� cisco � �������� ������ ������� � �������� ���� � ������� �� ������.
</blockquote>
<p> > line console 0 </p>
<p> > password cisco </p>

<blockquote>
e.	��������� cisco � �������� ������ VTY � �������� ���� � ������� �� ������.
</blockquote>
<p> > line vty 0 15 </p>
<p> > password cisco </p>

<blockquote>
f.	���������� �������� ������.
</blockquote>
<p> > service password-encryption </p>

<blockquote>
g.	�������� ������ � ��������������� � ������� �������������������� ������� � ����������.
</blockquote>
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>

<blockquote>
h.	��������� ������� ������������ � ���� ����������� ������������.
</blockquote>
<p> > copy running-config startup-config </p>

// ���������� ��� R2


<h2>�������� �����������:</h2>

<blockquote>
a.	��������� ����������� ��� ����������.
</blockquote>
<p> hostname S1 </p>

<blockquote>
b.	��������� ����� DNS, ����� ������������� ������� �������������� ������� ��������������� ��������� ������� ����� �������, ��� ����� ��� �������� ������� �����.
</blockquote>
<p> > no ip domain-lookup </p>

<blockquote>
c.	��������� class � �������� �������������� ������ ������������������ ������ EXEC.
</blockquote>
<p> > enable secret class </p>

<blockquote>
d.	��������� cisco � �������� ������ ������� � �������� ���� � ������� �� ������.
</blockquote>
<p> > line console 0 </p>
<p> > password cisco </p>

<blockquote>
e.	���������� cisco � �������� ������ ������������ ��������� � ����������� ����.
</blockquote>
<p> > line vty 0 15 </p>
<p> > password cisco </p>

<blockquote>
f.	���������� �������� ������.
</blockquote>
<p> > service password-encryption </p>

<blockquote>
g.	�������� ������ � ��������������� � ������� �������������������� ������� � ����������.
</blockquote>
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>

<blockquote>
h.	��������� ������� ������������ � ���� ����������� ������������.
</blockquote>
<p> > copy running-config startup-config </p>

// ���������� ��� S2


<h2> ����� 2. ��������� � �������� ������� ������ ��������� OSPFv2 ��� ����� �������</h2>


<h2> �������� ������ ���������� � �������� OSPFv2 �� ������ ��������������.</h2>


<blockquote>
a.	��������� ������ ����������� �� ������ ��������������, ��� �������� � ������� ��������� ����.
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
b.	��������� � ����� ������������ �������������� OSPF, ��������� ������������� �������� 56.
</blockquote>
<p> > router ospf 56 </p>

<blockquote>
c.	��������� ����������� ������������� �������������� ��� ������� �������������� (1.1.1.1 ��� R1, 2.2.2.2 ��� R2).
</blockquote>
R1:
<p> > router-id 1.1.1.1 </p>

R2:
<p> > router-id 2.2.2.2 </p>

<blockquote>
d.	��������� ���������� ���� ��� ���� ����� R1 � R2, �������� �� � ������� 0.
</blockquote>
R1:
<p> > network 10.53.0.0 0.0.0.255 area 0 </p>

R2:
<p> > network 10.53.0.0 0.0.0.255 area 0 </p>

<blockquote>
e.	(������ �� R2) �������� ������������, ����������� ��� ���������� ���� Loopback 1 � ������� OSPF 0.
</blockquote>
<p> > network 192.168.1.0 0.0.0.255 area 0 </p>

<blockquote>
f.	���������, ��� OSPFv2 �������� ����� ����������������. ��������� �������, ����� ���������, ��� R1 � R2 ������������ ���������.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/ospf_1.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/ospf_2.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/ospf_3.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/ospf_4.png>


<blockquote>
����� ������������� �������� DR? ����� ������������� �������� BDR? ������ �������� ������?
</blockquote>
<p>DR - R2; BDR - R1.</p>
<p>�������� ������:</p>
<p>- DR ���������� ������ � ��������� �����������</p>
<p>- ���� ���������� �����, DR ���������� ������ � ������� ��������� Router-ID</p>
<p>- ������ �� ������ �� ����������� ��������� ���������� ��� Router-ID ���������� BDR.</p>


<blockquote>
g.	�� R1 ��������� ������� show ip route ospf, ����� ���������, ��� ���� R2 Loopback1 ������������ � ������� �������������.
�������� ��������, ��� ��������� OSPF �� ��������� ����������� � ���������� ���������� �������� ����� � �������� �������� ���� � �������������� 32-������ �����.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/ospf_5.png>


<blockquote>
h.	��������� Ping ��  ������ ���������� R2 Loopback 1 �� R1. ���������� ������� ping ������ ���� ��������.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/ping_1.png>

���� ����


<h2> ����� 3. ����������� � �������� ������������ OSPFv2 ��� ����� �������</h2>


<h2> ���������� ��������� ����������� �� ������ ��������������.</h2>

<blockquote>
a.	�� R1 ��������� ��������� OSPF ���������� G0/0/1 �� 50, ����� ���������, ��� R1 �������� ����������� ���������������.
</blockquote>

<p> > interface g0/0/1</p>
<p> > ip ospf priority 50</p>


<blockquote>
b.	��������� ������� OSPF �� G0/0/1 ������� �������������� ��� ������� �����������, ������������� 30 ������.
</blockquote>

<p> > interface g0/0/1</p>
<p> > ip ospf hello-interval 30</p>


<blockquote>
c.	�� R1 ��������� ����������� ������� �� ���������, ������� ���������� ��������� Loopback 1 � �������� ���������� ������. ����� �������������� ������� �� ��������� � OSPF. �������� �������� �� ��������� ������� ����� ��������� �������� �� ���������.
</blockquote>

<p> > ip route 0.0.0.0 0.0.0.0 loopback 1</p>
<p> > router ospf 56</p>
<p> > default-information originate</p>


<blockquote>
d.	�������� ������������, ����������� ��� OSPF ��� ��������� R2 Loopback 1 ��� ���� �����-�����. ��� �������� � ����, ��� OSPF ��������� Loopback 1 ���������� ����� ������� ����������.
</blockquote>

// ������ ���� 192.168.1.0/24 � ������� ��� ��������� Loopback 1.
<p> > router ospf 56</p>
<p> > no network 192.168.1.0 0.0.0.255 area 0 </p>
<p> > interface loopback 1</p>
<p> > ip ospf 56 area 0</p>


<blockquote>
e.	(������ �� R2) �������� ������������, ����������� ��� �������������� �������� ���������� OSPF � ���� Loopback 1.
</blockquote>

<p> > router ospf 56</p>
<p> > passive-interface loopback 1</p>

<blockquote>
f.	�������� ������� ���������� ����������� ��� ���������������. ����� ���� ��������� ������������� OSPF � ������� ������� clear ip ospf process . �������� �������� �� ��������� ������� ����� ��������� ����� ������� ������ �����������.
</blockquote>

R1
<p> > router ospf 56</p>
<p> > auto-cost reference-bandwidth 10000</p>
<p> > clear ip ospf process</p>

R2
<p> > router ospf 56</p>
a<p> > auto-cost reference-bandwidth 10000</p>
<p> > clear ip ospf process</p>
// OSPF ������ ��������� ���������� �������� �� ���� ���������������

<h2> ��������, ��� ����������� OSPFv2 �������������</h2>


<blockquote>
a.	��������� ������� show ip ospf interface g0/0/1 �� R1 � ���������, ��� ��������� ���������� ���������� ������ 50, � ��������� ��������� � Hello 30, Dead 120, � ��� ���� �� ��������� � Broadcast
</blockquote>

<p> ������� dead-interval 120</p>
<p> > interface g0/0/1</p>
<p> > ip ospf dead-interval 120</p>
// � �� �� ����� �� R2 ����� ��� ���������

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/ospf_6.png>


<blockquote>
b.	�� R1 ��������� ������� show ip route ospf, ����� ���������, ��� ���� R2 Loopback1 ������������ � ������� �������������. �������� �������� �� ������� � ������� ����� ���� �������� � ���������� ��������. 
����� �������� ��������, ��� ����� ������ ���������� 24 ����, � ������� �� 32 �����, ����� �����������.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/route_2.png>


<blockquote>
c.	������� ������� show ip route ospf �� �������������� R2. ������������ ���������� � �������� OSPF ������ ���� ���������������� �� ��������� ������� R1.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/route_1.png>


<blockquote>
d.	��������� Ping �� ������ ���������� R1 Loopback 1 �� R2. ���������� ������� ping ������ ���� ��������.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/ping_2.png>

���� ����

<blockquote>
������:
������ ��������� OSPF ��� �������� �� ��������� ���������� �� ��������� OSPF � R1 ��� ���� 192.168.1.0/24?
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/route_3.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab10/route_4.png>

<p>R2 �������� ������� � R1, ��� �������� ������� ��������� ��� loopback �� ���������� 1.</p>
<p>R1 �������� ������� �� 192.168.1.0/24 �� OSPF ������� ������� �� ���������� G0/0/1 (���������� 100) � Loopback (���������� 1) - 100 + 1.</p>


������������ ��������������:


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