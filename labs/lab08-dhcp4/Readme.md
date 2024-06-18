<h1> ������������ ������ - ��������� ������������� ����� ������������ ���������� ������ </h1> 

<h2> ��������� </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/topology.png>

<h2> ������� ��������� </h2>

| ���������� |  ���������  |   IP-�����    |  ����� �������  | ���� �� ��������� |
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

<h2> ������� VLAN </h2>

|   VLAN   |     ���      |     ����������� ���������     |
|:--------:|:------------:|:-----------------------------:|
| 1        | ���          | S2: F0/18                     | 
| 100      | �������      | S1: F0/6                      |
| 200      | ����������   | S1: VLAN 200                  |
| 999      | Parking_Lot  | S1: F0/1-4, F0/7-24, G0/1-2   |
| 1000     | �����������  |               -               |

<h2> ������ </h2>

<ol>
  <li> �������� ���� � ��������� �������� ���������� ����������. </li>
  <li> ��������� � �������� ���� �������� DHCPv4 �� R1. </li>
  <li> ��������� � �������� DHCP-������������ �� R2. </li>
</ol>

<h2> ����� 1. ��������� �������� ���������� ��������� </h2>

<p>�������� ����� ���������:</p>
<p></p>
<p>������� ���� 192.168.1.0/24 � ������������ �� ���������� ������������:</p>

<blockquote>
a.	���� ������� �������� A�, �������������� 58 ������ (���������� VLAN �� R1).
</blockquote>
<p>����������� ������ ���� - 64 �����. ����� ���� /26 ��� 255.255.255.192; ��� ������� - 192.168.1.1 - 192.168.1.62 (+.0 � .63 � �������� ���������) </p>

<blockquote>
b.	���� ������� �������� B�, �������������� 28 ������ (����������� VLAN �� R1).
</blockquote>
<p>����������� ������ ���� - 32 �����. ����� ���� /27 ��� 255.255.255.224; ��� ������� - 192.168.1.65 - 192.168.1.94 (+.64 � .95 � �������� ���������) </p>

<blockquote>
c.	���� ������� �������� C�, �������������� 12 ����� (���������� ���� �� R2).
</blockquote>
<p>����������� ������ ���� - 16 ������. ����� ���� /28 ��� 255.255.255.240; ��� ������� - 192.168.1.97 - 192.168.1.110 (+.96 � .111 � �������� ���������) </p>


���������� ������� ��������� ���������������:
<blockquote>
a.	��������� �������������� ��� ����������.
</blockquote>
<p> > hostname R1 </p>

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

<blockquote>
i.	���������� ���� �� �������������� �� ����������� ����� � ����.
</blockquote>
<p> > clock set 23:33:00 16 June 2024 </p>


�������� ������������� ����� ������ VLAN �� �������������� R1:

<blockquote>
a.	����������� ��������� G0/0/1 �� ��������������.
</blockquote>
<p> > interface G0/0/1</p>
<p> > no shutdown</p>

<blockquote>
b.	��������� ������������� ��� ������ VLAN � ������������ � ������������ ������� IP-���������. ��� ������������� ���������� ������������ 802.1Q � ����������� ������ �������� ����� �� ������������ ���� IP-�������. ���������, ��� ������������� ��� native VLAN �� �������� IP-�����. �������� �������� ��� ������� �������������.
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
c.	���������, ��� ��������������� ���������� ��������.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/subinterface_1.png>


�������� G0/1 �� R2, ����� G0/0/0 � ����������� ������������� ��� ����� ���������������:

<blockquote>
a.	��������� G0/0/1 �� R2 � ������ IP-������� ������� C, ������������ �����.
</blockquote>
<p> > interface G0/0/1</p>
<p> > no shutdown</p>
<p> > ip address 192.168.1.97 255.255.255.240 </p>


<blockquote>
b.	��������� ��������� G0/0/0 ��� ������� �������������� �� ������ ����������� ���� ������� IP-���������.
</blockquote>
<p> > interface G0/0/0</p>
<p> > no shutdown</p>
<p> > ip address 10.0.0.2 255.255.255.252 </p>
// ���������� ��� R1

<blockquote>
c.	��������� ������� �� ��������� �� ������ ��������������, ����������� �� IP-����� G0/0/0 �� ������ ��������������.
</blockquote>
<p> > ip default-gateway 10.0.0.1 </p>
// ���������� ��� R1

<blockquote>
d.	���������, ��� ����������� ������������� �������� � ������� ����� �� ������ G0/0/1 R2 �� R1.
</blockquote>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/ping_1.png>

<blockquote>
e.	��������� ������� ������������ � ���� ����������� ������������.
</blockquote>
<p> > copy running-config startup-config </p>


�������� ������� ��������� ��� ������������:

<blockquote>
a.	��������� ����������� ��� ����������.
</blockquote>
<p> > hostname S1 </p>
<p> > hostname S2 </p>

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
h.	��������� �� ������������ �����.
</blockquote>
<p> > clock set 20:42:00 17 June 2024 </p>

<blockquote>
i.	���������� ������� ������������ � �������� ���������.
</blockquote>
<p> > copy running-config startup-config </p>


�������� ���� VLAN �� ����������� S1:

<blockquote>
a.	�������� ����������� VLAN �� ����������� 1 � ��������� �� ����� �� ����������� ���� �������.
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
b.	��������� � ����������� ��������� ���������� �� S1 (VLAN 200), ��������� ������ IP-����� �� �������, ������������ �����. ����� ���� ���������� ���� �� ��������� �� S1.
</blockquote>
<p> > interface vlan 200 </p>
<p> > ip address 192.168.1.66 255.255.255.224 </p>
<p> > ip default-gateway 192.168.1.65 </p>

<blockquote>
c.	��������� � ����������� ��������� ���������� �� S2 (VLAN 1), ��������� ������ IP-����� �� �������, ������������ �����. ����� ����, ���������� ���� �� ��������� �� S2
</blockquote>
<p>(������� C)</p>

<p> > interface vlan 1 </p>
<p> > no shutdown </p>
<p> > ip address 192.168.1.98 255.255.255.240 </p>
<p> > ip default-gateway 192.168.1.97 </p>

<blockquote>
d.	��������� ��� �������������� ����� S1 VLAN Parking_Lot, ��������� �� ��� ������������ ������ ������� � ��������������� ������������� ��. �� S2 ��������������� ������������� ��� �������������� �����.
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


�������� ���� VLAN ��������������� ����������� �����������:

<blockquote>
a.	��������� ������������ ����� ��������������� VLAN (��������� � ������� VLAN ����) � ��������� �� ��� ������ ������������ �������.
�������� ���� ������������
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
b.	���������, ��� VLAN ��������� �� ���������� ����������.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/vlan_1.png>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/vlan_2.png>

<blockquote>
������ ��������� F0/5 ������ � VLAN 1?
</blockquote>
��� ���� �� ���������, � ������ ���� �� ������������ �� ������ ����.


������� �������� ��������� S1 F0/5 � �������� ������ 802.1Q:

<blockquote>
a.	�������� ����� ����� �����������, ����� ������������� ������� ������������� �����.
</blockquote>
<p> > interface F0/5 </p>
<p> > switchport mode trunk </p>

<blockquote>
b.	� ������ ������������ ������  ���������� ��� native  VLAN �������� 1000.
</blockquote>
<p> > switchport trunk native vlan 1000 </p>

<blockquote>
c.	� �������� ������ ����� ������������ ���������� �������, ��� VLAN 100, 200 � 1000 ����� ��������� �� ������.
</blockquote>
<p> > switchport trunk allowed vlan 100,200,1000 </p>

<blockquote>
d.	��������� ������� ������������ � ���� ����������� ������������.
</blockquote>
<p> > copy running-config startup-config </p>

<blockquote>
e.	��������� ��������� ������.
</blockquote>
<p> > show interfaces f0/5 switchport </p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/vlan_3.png>

 
<blockquote>
����� IP-����� ��� �� � ��, ���� �� �� ��� ��������� � ���� � ������� DHCP?
</blockquote>
f0/6 - ���� ������� ��� VLAN 100, ������ ����� �� ���� 192.168.1.2 - 192.168.1.62


<h2> ����� 2. ��������� � �������� ���� �������� DHCPv4 �� R1 </h2>


�������� R1 � ������ DHCPv4 ��� ���� �������������� ��������:

<blockquote>
a.	��������� ������ ���� ������������ ������� �� ������� ���� �������.
</blockquote>
<p>VLAN 100</p>
<p> > ip dhcp excluded-address 192.168.1.1 192.168.1.5</p>
<p>//</p>
<p>VLAN 200</p>
<p> > ip dhcp excluded-address 192.168.1.65 192.168.1.70</p>

<blockquote>
b.	�������� ��� DHCP (����������� ���������� ��� ��� ������� ����).
</blockquote>
<p> > ip dhcp pool R1_Client_LAN</p>

<blockquote>
c.	������� ����, �������������� ���� DHCP-������.
</blockquote>
<p> > network 192.168.1.0 255.255.255.192</p>

<blockquote>
d.	� �������� ����� ������ ������� CCNA-lab.com.
</blockquote>
<p> > domain-name CCNA-lab.com </p>

<blockquote>
e.	��������� ��������������� ���� �� ��������� ��� ������� ���� DHCP.
</blockquote>
<p> > default-router 192.168.1.1</p>

<blockquote>
f.	��������� ����� ������ �� 2 ��� 12 ����� � 30 �����.
</blockquote>
<p> > lease 2 12 30</p>
//������� lease �� �������������� � packet tracer

<blockquote>
g.	����� ��������� ������ ��� DHCPv4, ��������� ��� ���� R2_Client_LAN � ��������� ����, ������������� �� ���������, � ����������� �� �� ��� ������ � ����� ������, ��� � ���������� ��� DHCP.
</blockquote>
<p> > ip dhcp pool R2_Client_LAN</p>
<p> > network 192.168.1.64 255.255.255.224</p>
<p> > domain-name CCNA-lab.com </p>
<p> > default-router 192.168.1.65</p>
<p> > lease 2 12 30</p>

�������� ������������:
<p> > copy running-config startup-config </p>


�������� ������������ ������� DHCPv4:

<blockquote>
a.	����� ����������� �������� � ����, ��������� ������� show ip dhcp pool.
</blockquote>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/dhcp_1.png>

<blockquote>
b.	��������� ������� show ip dhcp bindings ��� �������� ������������� ���������� ������� DHCP.
</blockquote>
<p> > show ip dhcp binding </p>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/dhcp_2.png>

<blockquote>
c.	��������� ������� show ip dhcp server statistics ��� �������� ��������� DHCP.
</blockquote>
//������� �� �������������� � packet tracer

������� �������� IP-����� �� DHCP �� PC-A

<blockquote>
a.	�� ��������� ������ ���������� PC-A ��������� ������� ipconfig /all.
</blockquote>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/ipconfig_1.png>

<blockquote>
b.	����� ���������� �������� ���������� ��������� ������� ipconfig ��� ��������� ����� ���������� �� IP-������.
</blockquote>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/ipconfig_2.png>

<blockquote>
c.	��������� ����������� � ������� ����� IP-������ ���������� R0 G0/0/1(???).
</blockquote>
<p> ���� �� R1 G0/0/0 </p>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/ping_2.png>

<p> ���� �� R1 G0/0/1.100 </p>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/ping_3.png>

<p> ���� �� R1 G0/0/1.200 </p>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/ping_4.png>


<h2> ����� 3. ��������� � �������� DHCP-������������ �� R2 </h2> 


��������� R2 � �������� ������ DHCP-������������ ��� ��������� ���� �� G0/0/1

<blockquote>
a.	��������� ������� ip helper-address �� G0/0/1, ������ IP-����� G0/0/0 R1.
</blockquote>
<p> > interface G0/0/1 </p>
<p> > ip helper-address 10.0.0.1 </p>

<blockquote>
b.	��������� ������������.
</blockquote>
<p> > copy running-config startup-config </p>

������� �������� IP-����� �� DHCP �� PC-B

<blockquote>
a.	�� ��������� ������ ���������� PC-B ��������� ������� ipconfig /all.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/ipconfig_3.png>

<blockquote>
b.	����� ���������� �������� ���������� ��������� ������� ipconfig ��� ��������� ����� ���������� �� IP-������.
</blockquote>
// ����� �� ��������, ���� �� ����
// R2 �� �������� ������ ���������
// The DHCP server does not have a pool configured for the received port. It drops the packet.
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp4/err_1.png>




<blockquote>
c.	��������� ����������� � ������� ����� IP-������ ���������� R1 G0/0/1.
</blockquote>


<blockquote>
d.	��������� show ip dhcp binding ��� R1 ��� �������� ���������� ������� � DHCP.
</blockquote>


<blockquote>
e.	��������� ������� show ip dhcp server statistics ��� �������� ��������� DHCP.
</blockquote>


