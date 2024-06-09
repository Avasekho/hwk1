<h1> ������������ ������. ������ � ������� ����������� �� ��������� SSH </h1> 

<h2> ��������� </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab05/topology.png>

<h2> ������� ��������� </h2>

| ���������� | ��������� |   IP-�����   |  ����� �������  | ���� �� ��������� |
|:----------:|:---------:|:------------:|:---------------:|:-----------------:|
| R1         | G0/0/1    | 192.168.1.1  | 255.255.255.0   |         -         |
| S1         | VLAN 1    | 192.168.1.11 | 255.255.255.0   | 192.168.1.1       |
| PC-A       | NIC       | 192.168.1.3  | 255.255.255.0   | 192.168.1.1       |

<h2> ������ </h2>

<ol>
  <li> ��������� �������� ���������� ����������. </li>
  <li> ��������� �������������� ��� ������� �� ��������� SSH. </li>
  <li> ��������� ����������� ��� ������� �� ��������� SSH. </li>
  <li> SSH ����� ��������� ��������� ������ (CLI) �����������. </li>
</ol>


<h2> ����� 1. ��������� �������� ���������� ���������</h2> 

�������� �������������:

a.	������������ � �������������� � ������� ������� � ����������� ����������������� ����� EXEC.
b.	������� � ����� ������������.
<p> enable >> configure terminal </p>
c.	��������� ����� DNS, ����� ������������� ������� �������������� ������� ��������������� ��������� ������� ����� �������, ��� ����� ��� �������� ������� �����.
<p> > no ip domain-lookup </p>
d.	��������� class � �������� �������������� ������ ������������������ ������ EXEC.
<p> > enable secret class </p>
e.	��������� cisco � �������� ������ ������� � �������� ���� � ������� �� ������.
<p> > line console 0 </p>
<p> > password cisco </p>
f.	��������� cisco � �������� ������ VTY � �������� ���� � ������� �� ������.
<p> > line vty 0 15 </p>
<p> > password cisco </p>
g.	���������� �������� ������.
<p> > service password-encryption </p>
h.	�������� ������, ������� ������������� � ������� �������������������� �������.
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>
i.	��������� � ����������� �� �������������� ��������� G0/0/1, ��������� ����������, ����������� � ������� ���������.

interface g0/0/1
ip address 192.168.1.1 255.255.255.0
no shutdown

j.	��������� ������� ������������ � ���� ����������� ������������.
<p> > copy running-config startup-config </p>

�������� ������� ��������� �� ��:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab05/pc_1.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab05/pc_2.png>

�������� ����������� ��������������:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab05/ping_1.png>


<h2> ����� 2. ��������� �������������� ��� ������� �� ��������� SSH </h2>

�������� �������������� ���������:

<p> > hostname R1 </p>
<p> > ip domain-name otus </p>

�������� ���� ���������� � ��������� ��� �����:

<p> > crypto key generate rsa general-keys modulus 2048 </p>

�������� ��� ������������ � ��������� ���� ������� �������:

<p> > username admin password Adm1nP@55 </p>

���������� �������� SSH �� ������ VTY:

<p> > line vty 0 15 </p>
<p> > transport input ssh </p>

// ������������ ��������� SSH � Telnet � ������� ������� transport input ssh telnet Packet Tracer �� ���� - ������� �� ���������, ������� ������

������� �������� ������������� �� ��������� ���� ������� �������:

<p> > line vty 0 15 </p>
<p> > login local </p>

�������� ������� ������������ � ���� ����������� ������������:
<p> > copy running-config startup-config </p>

�������� ����������� ���������� �� SSH:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab05/ssh_1.png>

������������� ��������.


<h2> ����� 3. ��������� ����������� ��� ������� �� ��������� SSH </h2>

�������� ����������:

a.	������������ � �������������� � ������� ������� � ����������� ����������������� ����� EXEC.
b.	������� � ����� ������������.
<p> enable >> configure terminal </p>
c.	��������� ����� DNS, ����� ������������� ������� �������������� ������� ��������������� ��������� ������� ����� �������, ��� ����� ��� �������� ������� �����.
<p> > no ip domain-lookup </p>
d.	��������� class � �������� �������������� ������ ������������������ ������ EXEC.
<p> > enable secret class </p>
e.	��������� cisco � �������� ������ ������� � �������� ���� � ������� �� ������.
<p> > line console 0 </p>
<p> > password cisco </p>
f.	��������� cisco � �������� ������ VTY � �������� ���� � ������� �� ������.
<p> > line vty 0 15 </p>
<p> > password cisco </p>
g.	���������� �������� ������.
<p> > service password-encryption </p>
h.	�������� ������, ������� ������������� � ������� �������������������� �������.
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>
i.	��������� � ����������� �� ����������� ��������� VLAN 1, ��������� ����������, ����������� � ������� ���������.

<p> > interface Vlan1 </p>
<p> > ip address 192.168.1.11 255.255.255.0 </p>
<p> > ip default-gateway 192.168.1.1 </p>
<p> > no shutdown </p>

j.	��������� ������� ������������ � ���� ����������� ������������.
<p> > copy running-config startup-config </p>

�������� ���������� ��� ���������� �� ��������� SSH:

a.	��������� ��� ����������, ��� ������� � ������� ���������.
<p> enable >> configure terminal </p>
<p> > hostname S1 </p>
b.	������� ����� ��� ����������.
<p> > ip domain-name otus </p>
c.	�������� ���� ���������� � ��������� ��� �����.
<p> > crypto key generate rsa general-keys modulus 2048 </p>
d.	�������� ��� ������������ � ��������� ���� ������� �������.
<p> > username admin password Adm1nP@55 </p>
e.	����������� ��������� Telnet � SSH �� ������ VTY.
<p> > line vty 0 15 </p>
<p> > transport input ssh </p>
f.	�������� ������ ����� � ������� ����� �������, ����� �������������� �������� ������������� �� ��������� ���� ������� �������.
<p> > line vty 0 15 </p>
<p> > login local </p>

�������� ������� ������������ � ���� ����������� ������������:
<p> > copy running-config startup-config </p>

�������� ����������� ���������� �� SSH:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab05/ssh_2.png>

���������� ��������.


<h2> ����� 4. ��������� ��������� SSH � �������������� ���������� ��������� ������ (CLI) ����������� </h2>

��������� ��������� ��������� ��� ������� SSH � Cisco IOS:
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab05/ssh_3.png>

��� �����, � Packet Tracer �� ����������� � IOS version 15.0 ����� ���������� ��������� ���������.


��������� � ����������� S1 ���������� � ��������������� R1 �� ��������� SSH:

<p> > ssh -l admin 192.168.1.1 </p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab05/ssh_4.png>

������������� ��������.

������������� � ���������� ���������� �� ���������� � ������� Ctrl+Shift+6 � "x":
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab05/ssh_5.png>

���������� ������ � ��������� ����������� � ������� ������� exit:
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab05/ssh_6.png>


������������ ��������������:

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

������������ �����������:

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