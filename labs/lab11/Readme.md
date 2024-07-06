<h1> ������������ ������. ��������� � �������� ����������� ������� �������� �������. </h1> 

<h2> ��������� </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/topology.png>

<h2> ������� ��������� </h2>

| ���������� |  ���������  |   IP-�����    |  ����� �������  | ���� �� ��������� |
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

<h2> ������� VLAN </h2>

|   VLAN   |     ���      |         ����������� ���������         |
|:--------:|:------------:|:-------------------------------------:|
| 20       | Management   | S2: F0/5                              | 
| 30       | Operations   | S1: F0/6                              |
| 40       | Sales        | S2: F0/18                             |
| 999      | Parking_Lot  | S1: F0/2-4, F0/7-24, G0/1-2           |
|          |              | S2: F0/2-4, F0/6-17, F0/19-24, G0/1-2 |
| 1000     | �����������  |                   -                   |

<h2> ������ </h2>

<ol>
  <li> �������� ���� � ��������� �������� ���������� ����������. </li>
  <li> ��������� � �������� ������� ������������ �������� �������. </li>
</ol>

<h2> ����� 1. �������� ���� � ��������� �������� ���������� ���������� </h2>


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


<h2> ����� 2. ��������� ����� VLAN �� ������������ </h2>


<h2> �������� ���� VLAN �� ������������ </h2>

<blockquote>
a.	�������� ����������� VLAN � �������� �� �� ������ ����������� �� ����������� ���� �������.
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
b.	��������� ��������� ���������� � ���� �� ��������� �� ������ �����������, ��������� ���������� �� IP-������ � ������� ���������. 
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
c.	��������� ��� �������������� ����� ����������� VLAN Parking Lot, ��������� �� ��� ������������ ������ ������� � ��������������� ������������� ��.
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


<h2> �������� ���� VLAN ��������������� ����������� �����������. </h2>

<blockquote>
a.	��������� ������������ ����� ��������������� VLAN (��������� � ������� VLAN ����) � ��������� �� ��� ������ ������������ �������.
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
b.	��������� ������� show vlan brief, ����� ���������, ��� ���� VLAN ��������� ���������� �����������.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/vlan_1.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/vlan_2.png>


<h2> ����� 3. ���������� ������ (������������� ������). </h2>


<h2> ������� �������� ������������� ��������� F0/1. </h2>


<blockquote>
a.	�������� ����� ����� ����������� �� ���������� F0/1, ����� ������������� ������� ������������� �����. �� �������� ������� ��� �� ����� ������������.
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
b.	� ������ ������������ ������ ���������� ��� native vlan �������� 1000 �� ����� ������������. ��� ��������� ���� ����������� ��� ������ ����������� VLAN ��������� �� ������� ����� ������������ ��������.
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
c.	� �������� ������ ����� ������������ ������ �������, ��� VLAN 10, 20, 30 � 1000 ��������� � ������.
</blockquote>

// VLAN 10 � ������ ������������ ���, �������� 40

S1:
<p> > interface f0/1</p>
<p> > switchport trunk allowed vlan 20,30,40,1000</p>
<p> > exit</p>

S2:
<p> > interface f0/1</p>
<p> > switchport trunk allowed vlan 20,30,40,1000</p>
<p> > exit</p>

<blockquote>
d.	��������� ������� show interfaces trunk ��� �������� ������ ����������, ����������� VLAN � ����������� VLAN ����� ����������.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/vlan_3.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/vlan_4.png>


<h2> ������� �������� ������������� ��������� F0/5 �� ����������� S1. </h2>


<blockquote>
a.	��������� ��������� S1 F0/5 � ���� �� ����������� ������, ��� � F0/1. ��� ����� �� ��������������.
</blockquote>

S1:
<p> > interface f0/5</p>
<p> > switchport mode trunk</p>
<p> > switchport trunk native vlan 1000</p>
<p> > switchport trunk allowed vlan 20,30,40,1000</p>
<p> > no shutdown</p>
<p> > exit</p>

<blockquote>
b.	��������� ������� ������������ � ���� ����������� ������������.
</blockquote>

<p> > copy running-config startup-config </p>


<blockquote>
c.	����������� ������� show interfaces trunk ��� �������� �������� ������.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/vlan_5.png>


<h2> ����� 4. ��������� �������������. </h2>


<h2> ��������� ������������� ����� ������ VLAN �� R1. </h2>

<blockquote>
a.	����������� ��������� G0/0/1 �� ��������������.
</blockquote>

<p> > interface g0/0/1</p>
<p> > no shutdown</p>

<blockquote>
b.	��������� ������������� ��� ������ VLAN, ��� ������� � ������� IP-���������. ��� ������������� ���������� ������������ 802.1Q. ���������, ��� ������������ ��� ����������� VLAN �� ����� ������������ IP-������. �������� �������� ��� ������� �������������.
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
c.	��������� ��������� Loopback 1 �� R1 � ���������� �� ����������� ���� �������.
</blockquote>


<p> > interface loopback 1</p>
<p> > ip address 172.16.1.1 255.255.255.0</p>

<blockquote>
d.	� ������� ������� show ip interface brief ��������� ������������ �������������.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/interfaces_1.png>


<h2> ��������� ���������� R2 g0/0/1 � �������������� ������ �� ������� � �������� �� ��������� � ������� ���������� �������� 10.20.0.1 </h2>


<p> > interface g0/0/1</p>
<p> > no shutdown</p>
<p> > ip address 10.20.0.4 255.255.255.0</p>
<p> > ip default-gateway 10.20.0.1</p>


<h2> ����� 5. ��������� ��������� ������. </h2>


<h2> �������� ��� ������� ���������� ��� ������� ��������� SSH. </h2>


<blockquote>
a.	�������� ���������� ������������ � ������ ������������ SSHadmin � ������������� ������� $cisco123!
</blockquote>

<p> > username SSHadmin password $cisco123!</p>


<blockquote>
b.	����������� ccna-lab.com � �������� ��������� �����.
</blockquote>

<p> > ip domain-name ccna-lab.com</p>


<blockquote>
c.	����������� ����������� � ������� 1024 ������� ������.
</blockquote>

<p> > crypto key generate rsa general-keys modulus 1024</p>

<blockquote>
d.	��������� ������ ���� ����� VTY �� ������ ����������, ����� ������������ ������ SSH-���������� � � ��������� ���������������.
</blockquote>

<p> > line vty 0 5</p>
<p> > transport input ssh</p>
<p> > login local</p>


<h2> ������� ���������� ���-������ � ��������� ����������� �� R1. </h2>

<blockquote>
<p>a.	�������� ������ HTTPS �� R1.</p>
<p>R1(config)# ip http secure-server</p>
</blockquote>

// ������� �� �������������� � packet server

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/err_1.png>

<blockquote>
<p>b.	��������� R1 ��� �������� ����������� �������������, ���������� ������������ � ���-�������.</p>
<p>R1(config)# ip http authentication local</p>
</blockquote>

// ������� �� �������������� � packet server



<h2> ����� 6. �������� ����������� </h2>


<h2> �������� ���� ��. </h2>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/pc_1.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/pc_2.png>


<h2> �������� ��������� �����. ��������� ������ ������ �������. </h2>

|   ��   | �������� | ���������� | ��������� |
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


<h2> ����� 7. ��������� � �������� ������� �������� ������� (ACL) </h2>

<blockquote>
<p>��� �������� �������� ����������� �������� ������� ���������� ��������� ������� ������������:</p>
<p>�������� 1. ���� Sales �� ����� ������������ SSH � ���� Management (�� �  ������ ���� SSH ��������). </p>
<p>�������� 2. ���� Sales �� ����� ������� � IP-������� � ���� Management � ������� ������ ���-��������� (HTTP/HTTPS). ���� Sales ����� �� ����� ������� � ����������� R1 � ������� ������ ���-���������. 
�������� ���� ������ ���-������ (�������� �������� � ���� Sales  ����� �������� ������ � ���������� Loopback 1 �� R1).</p>
<p>�������� 3. ���� Sales �� ����� ���������� ���-������� ICMP � ���� Operations ��� Management. ��������� ���-������� ICMP � ������ ���������. </p>
<p>�������� 4: C��� Operations  �� ����� ���������� ICMP ���������� � ���� Sales. ��������� ���-������� ICMP � ������ ���������. </p>
</blockquote>

<h2> �������������� ���������� � ���� � �������� ������������ ��� ������������ ���������� ACL. </h2>

<p>�������� 1. ���� Sales �� ����� ������������ SSH � ���� Management (�� �  ������ ���� SSH ��������).</p>
<p>��� ����� ���������� ������������� ��������� SSH �� Sales �� ���� � Management</p>

<p>2. �������� 2. ���� Sales �� ����� ������� � IP-������� � ���� Management � ������� ������ ���-��������� (HTTP/HTTPS). ���� Sales ����� �� ����� ������� � ����������� R1 � ������� ������ ���-���������. </p>
<p>�������� ���� ������ ���-������ (�������� �������� � ���� Sales  ����� �������� ������ � ���������� Loopback 1 �� R1).</p>
<p>������������� ��������� ������� �� Sales �� 80/443 ����� �������������� � � Management</p>

<p>3. �������� 3. ���� Sales �� ����� ���������� ���-������� ICMP � ���� Operations ��� Management. ��������� ���-������� ICMP � ������ ���������.</p>
<p>������������� ��������� ICMP �� Sales � Operations � Management</p>

<p>4. �������� 4: C��� Operations  �� ����� ���������� ICMP ���������� � ���� Sales. ��������� ���-������� ICMP � ������ ���������</p>
<p>������������� ��������� ICMP �� Operations � Sales</p>

<h2> ���������� � ���������� ����������� ������� �������, ������� ����� ��������������� ����������� �������� ������������. </h2>

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
// �.� �� ��������� cisco ������ � ����� deny any any ���� �������� � ����� ��� Sales ��� ��� �� ���������.

<p> > ip access-list extended vlan30-in</p>
<p> > deny icmp 10.30.0.0 0.0.255.255 10.40.0.0 0.0.255.255</p>


<p> > interface G0/0/1.30</p>
<p> > ip access-group vlan30-in IN</p>

<p> > interface G0/0/1.40</p>
<p> > ip access-group vlan40-in IN</p>


<h2> ��������, ��� �������� ������������ ����������� ������������ �������� �������. </h2>

|   ��   | �������� | ���������� | ��������� |
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


// �.�. ������� �� ������������� HTTP-������ �� ������� (��� ����� ������� � packet tracer) �� ���� ��������� ��� ����.

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/ping_4.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/ping_5.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/ping_6.png>


10.20.0.1 ���������� �� SSH

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/ssh_4.png>


172.16.1.1 �������� �� SSH

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab11/ssh_5.png>


������������ ��������������:

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


������������ �����������:


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
