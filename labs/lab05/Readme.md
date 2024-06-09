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

������������ �����������:

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