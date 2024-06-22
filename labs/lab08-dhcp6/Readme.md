<h1> ����������� ������ - ��������� DHCPv6 </h1> 

<h2> ��������� </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp6/topology.png>

<h2> ������� ��������� </h2>

| ���������� |  ���������  |      IPv6-�����       |
|:----------:|:-----------:|:---------------------:|
| R1         | G0/0/0      | 2001:db8:acad:2::1/64 |
|            | G0/0/0      | fe80::1               |
|            | G0/0/1      | 2001:db8:acad:1::1/64 |
|            | G0/0/1      | fe80::1               |
| R2         | G0/0/0      | 2001:db8:acad:2::2/64 |
|            | G0/0/0      | fe80::2               |
|            | G0/0/1      | 2001:db8:acad:3::1/64 |
|            | G0/0/1      | fe80::1               |
| PC-A       | NIC         | DHCP                  |
| PC-B       | NIC         | DHCP                  |

<h2> ������ </h2>

<ol>
  <li> �������� ���� � ��������� �������� ���������� ����������. </li>
  <li> �������� ���������� ������ SLAAC �� R1. </li>
  <li> ��������� � �������� ������� DHCPv6 ��� ����������� �� R1. </li>
  <li> ��������� � �������� ��������� DHCPv6 ������� �� R1. </li>
  <li> ��������� � �������� DHCPv6 Relay �� R2. </li>
</ol>

<h2> ����� 1. ��������� �������� ���������� ��������� </h2>

�������� ������� ��������� ������� �����������:

<blockquote>
a.	��������� ����������� ��� ����������.
</blockquote>
<p> hostname S1 </p>
<p> hostname S2 </p>

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
h.	��������� ��� �������������� �����.
</blockquote>
<p> > interface range f0/1-4 </p>
<p> > shutdown </p>
<p> > interface range f0/7-24 </p>
<p> > shutdown </p>
<p> //���������� ��� S2 </p>

<blockquote>
i.	���������� ������� ������������ � �������� ���������.
</blockquote>
<p> > copy running-config startup-config </p>


���������� ������� ��������� ���������������:

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
h.	��������� IPv6-�������������
</blockquote>
<p> > IPv6 unicast-routing </p>

<blockquote>
i.	��������� ������� ������������ � ���� ����������� ������������.
</blockquote>
<p> > copy running-config startup-config </p>
// ���������� ��� R2


�������� ���������� � ������������� ��� ����� ���������������:

<blockquote>
a.	��������� ���������� G0/0/0 � G0/1 �� R1 � R2 � �������� IPv6, ���������� � ������� ����
</blockquote>

R1:
<p> > interface g0/0/0 </p>
<p> > ipv6 address 2001:db8:acad:2::1/64 </p>
<p> > ipv6 address fe80::1 link-local </p>
<p> > no shutdown </p>

<p> > interface g0/0/1 </p>
<p> > ipv6 address 2001:db8:acad:1::1/64 </p>
<p> > ipv6 address fe80::1 link-local </p>
<p> > no shutdown </p>


R2:

<p> > interface g0/0/0 </p>
<p> > ipv6 address 2001:db8:acad:2::2/64 </p>
<p> > ipv6 address fe80::2 link-local </p>
<p> > no shutdown </p>

<p> > interface g0/0/1 </p>
<p> > ipv6 address 2001:db8:acad:3::1/64 </p>
<p> > ipv6 address fe80::1 link-local </p>
<p> > no shutdown </p>


<blockquote>
b.	��������� ������� �� ��������� �� ������ ��������������, ������� ��������� �� IP-����� G0/0/0 �� ������ ��������������.
</blockquote>

R1:

<p> > ipv6 route ::/0 2001:db8:acad:2::2 </p>

R2:

<p> > ipv6 route ::/0 2001:db8:acad:2::1 </p>

<blockquote>
c.	���������, ��� ������������� �������� � ������� ����� ������ G0/0/1 R2 �� R1
</blockquote>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp6/ping_1.png>


<blockquote>
d.	��������� ������� ������������ � ���� ����������� ������������.
</blockquote>

<p> > copy running-config startup-config </p>


<h2> ����� 2. �������� ���������� ������ SLAAC �� R1 </h2>

������� PC-A � ��������, ��� ������� ������� �������� ��� �������������� ��������� IPv6:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp6/ipconfig_1.png>

<blockquote>
������ ������� ����� ������ � ��������������� �����?
</blockquote>
������������ �� ������ MAC-������ ����������


<h2> ����� 3. ��������� � �������� ������� DHCPv6 �� R1 </h2>

<blockquote>
� ����� 3 ����������� ��������� � �������� ��������� DHCP-������� �� R1. ���� ������� � ���, ����� ������������ PC-A ���������� � DNS-������� � ������.
</blockquote>

�������� ��� ��� ����������� PC-A

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp6/ipconfig_2.png>


�������� R1 ��� �������������� DHCPv6 ��� ��������� ��� PC-A:


<blockquote>
<p>a.	�������� ��� DHCP IPv6 �� R1 � ������ R1-STATELESS. � ������� ����� ���� ��������� ����� DNS-������� ��� 2001:db8:acad: :1, � ��� ������ � ��� stateless.com.</p>
<p>�������� ���� ������������</p>
<p>R1(config)# ipv6 dhcp pool R1-STATELESS</p>
<p>R1(config-dhcp)# dns-server 2001:db8:acad::254</p>
<p>R1(config-dhcp)# domain-name STATELESS.com</p>
</blockquote>
<p> > ipv6 dhcp pool R1-STATELESS </p>
<p> > dns-server 2001:db8:acad::254 </p>
<p> > domain-name STATELESS.com </p>

<blockquote>
<p>b.	��������� ��������� G0/0/1 �� R1, ����� ������������ ���� ������������ OTHER ��� ��������� ���� R1 � ������� ������ ��� ��������� ��� DHCP � �������� ������� DHCP ��� ����� ����������.</p>
<p>R1(config)# interface g0/0/1</p>
<p>R1(config-if)# ipv6 nd other-config-flag </p>
<p>R1(config-if)# ipv6 dhcp server R1-STATELESS</p>
</blockquote>
<p> > interface g0/0/1 </p>
<p> > ipv6 nd other-config-flag </p>
<p> > ipv6 dhcp server R1-STATELESS </p>

<blockquote>
c.	��������� ������� ������������ � ���� ����������� ������������.
</blockquote>
<p> > copy running-config startup-config </p>

<blockquote>
d.	������������� PC-A.
</blockquote>
// done

<blockquote>
e.	��������� ����� ipconfig /all � �������� �������� �� ���������.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp6/ipconfig_3.png>

// � ���������� �������� DNS-������� � ����� �������


<blockquote>
f.	������������ ����������� � ������� ����� IP-������ ���������� G0/1 R2.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp6/ping_2.png>

// ����� ��������


<h2> ����� 4. ��������� ������� DHCPv6 � ����������� ��������� �� R1 </h2>

<blockquote>
� ����� 4 ������������� R1 ��� ������ �� ������� DHCPv6 �� ��������� ���� �� R2.
</blockquote>

<blockquote>
<p>a.	�������� ��� DHCPv6 �� R1 ��� ���� 2001:db8:acad:3:aaa::/80. ��� ����������� ������ ��������� ����, ������������ � ���������� G0/0/1 �� R2. � ������� ���� ������� DNS-������ 2001:db8:acad: :254 � ������� �������� ��� STATEFUL.com.</p>
<p>�������� ���� ������������</p>
<p>R1(config)# ipv6 dhcp pool R2-STATEFUL</p>
<p>R1(config-dhcp)# address prefix 2001:db8:acad:3:aaa::/80</p>
<p>R1(config-dhcp)# dns-server 2001:db8:acad::254</p>
<p>R1(config-dhcp)# domain-name STATEFUL.com</p>
</blockquote>
<p> > ipv6 dhcp pool R2-STATEFUL </p>
<p> > address prefix 2001:db8:acad:3:aaa::/80 </p>
<p> > dns-server 2001:db8:acad::254 </p>
<p> > domain-name STATEFUL.com </p>

<blockquote>
<p>b.	��������� ������ ��� ��������� ��� DHCPv6 ���������� g0/0/0 �� R1.</p>
<p>R1(config)# interface g0/0/0</p>
<p>R1(config-if)# ipv6 dhcp server R2-STATEFUL</p>
</blockquote>
<p> > interface g0/0/0 </p>
<p> > ipv6 dhcp server R2-STATEFUL </p>


<h2> ����� 5. ��������� � �������� ������������ DHCPv6 �� R2. </h2>

������� PC-B � �������� ����� SLAAC, ������� �� ����������:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp6/ipconfig_4.png>


�������� R2 � �������� ������ DHCP-������������ ��� ��������� ���� �� G0/0/1

<blockquote>
a.	��������� ������� ipv6 dhcp relay �� ���������� R2 G0/0/1, ������ ����� ���������� ���������� G0/0/0 �� R1. ����� ��������� ������� managed-config-flag .
�������� ���� ������������
R2 (������������) # ��������� g0/0/1
R2(config-if)# ipv6 nd managed-config-flag
R2(config-if)# ipv6 dhcp relay destination 2001:db8:acad:2::1 g0/0/0
</blockquote>
<p> > interface g0/0/1 </p>
<p> > ipv6 nd managed-config-flag </p>
<p> > ipv6 dhcp relay destination 2001:db8:acad:2::1 g0/0/0</p>

<p>// �� ���� ��������� relay agent ��� IPv6 � Packet Tracer �� ������ ������ �� ��������������</p>


<p>// UPD. �.�. relay agent �� ��������������, �� ����� ��������� ������ STATEFUL DHCP �������� ��������� �������� �� R2 </p>
<p>// ����������� DHCP ������ �� R2</p>
<p> > ipv6 dhcp pool R2-STATEFUL </p>
<p> > dns-server 2001:db8:acad::254 </p>
<p> > domain-name STATEFUL.com </p>
<p></p>
<p>// ��������� DHCP �� ��������� g0/0/1</p>
<p> > interface g0/0/1 </p>
<p> > ipv6 dhcp server R2-STATEFUL </p>
<p> > ipv6 nd managed-config-flag </p>


<blockquote>
b.	��������� ������������.
</blockquote>
<p> > copy running-config startup-config </p>

������� �������� ����� IPv6 �� DHCPv6 �� PC-B

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp6/ipconfig_5.png>

<p>// ��-�� ����������� Packet Tracer �� �� �������� � ���������� �� DNS-�������� STATEFUL.com, �� ������ �������.</p>
<p>// G0/0/1 �� R1 ����� ����������</p>


<p>// UPD. �������� ����� � DHCP ������� �� R2 </p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08-dhcp6/ipconfig_6.png>

������������ ��������������:

R1#sh run
<blockquote>
<p>Building configuration...</p>
<p></p>
<p>Current configuration : 1295 bytes</p>
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
<p>ipv6 unicast-routing</p>
<p>!</p>
<p>no ipv6 cef</p>
<p>!</p>
<p>ipv6 dhcp pool R1-STATELESS</p>
<p> dns-server 2001:DB8:ACAD::254</p>
<p> domain-name STATELESS.com</p>
<p>!</p>
<p>ipv6 dhcp pool R2-STATEFUL</p>
<p> address prefix 2001:db8:acad:3:aaa::/80 lifetime 172800 86400</p>
<p> dns-server 2001:DB8:ACAD::254</p>
<p> domain-name STATEFUL.com</p>
<p>!</p>
<p>no ip domain-lookup</p>
<p>!</p>
<p>spanning-tree mode pvst</p>
<p>!</p>
<p>interface GigabitEthernet0/0/0</p>
<p> no ip address</p>
<p> duplex auto</p>
<p> speed auto</p>
<p> ipv6 address FE80::1 link-local</p>
<p> ipv6 address 2001:DB8:ACAD:2::1/64</p>
<p> ipv6 dhcp server R2-STATEFUL</p>
<p>!</p>
<p>interface GigabitEthernet0/0/1</p>
<p> no ip address</p>
<p> duplex auto</p>
<p> speed auto</p>
<p> ipv6 address FE80::1 link-local</p>
<p> ipv6 address 2001:DB8:ACAD:1::1/64</p>
<p> ipv6 nd other-config-flag</p>
<p> ipv6 dhcp server R1-STATELESS</p>
<p>!</p>
<p>interface Vlan1</p>
<p> no ip address</p>
<p> shutdown</p>
<p>!</p>
<p>ip classless</p>
<p>!</p>
<p>ip flow-export version 9</p>
<p>!</p>
<p>ipv6 route ::/0 2001:DB8:ACAD:2::2</p>
<p>!</p>
<p>no cdp run</p>
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
<p>end
</blockquote>



R2#sh run
<blockquote>
<p>Building configuration...</p>
<p></p>
<p>Current configuration : 1115 bytes</p>
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
<p>ipv6 unicast-routing</p>
<p>!</p>
<p>no ipv6 cef</p>
<p>!</p>
<p>ipv6 dhcp pool R2-STATEFUL</p>
<p> dns-server 2001:DB8:ACAD::254</p>
<p> domain-name STATEFUL.com</p>
<p>!</p>
<p>no ip domain-lookup</p>
<p>!</p>
<p>spanning-tree mode pvst</p>
<p>!</p>
<p>interface GigabitEthernet0/0/0</p>
<p> no ip address</p>
<p> duplex auto</p>
<p> speed auto</p>
<p> ipv6 address FE80::2 link-local</p>
<p> ipv6 address 2001:DB8:ACAD:2::2/64</p>
<p>!</p>
<p>interface GigabitEthernet0/0/1</p>
<p> no ip address</p>
<p> duplex auto</p>
<p> speed auto</p>
<p> ipv6 address FE80::1 link-local</p>
<p> ipv6 address 2001:DB8:ACAD:3::1/64</p>
<p> ipv6 nd managed-config-flag</p>
<p> ipv6 dhcp server R2-STATEFUL</p>
<p>!</p>
<p>interface Vlan1</p>
<p> no ip address</p>
<p> shutdown</p>
<p>!</p>
<p>ip classless</p>
<p>!</p>
<p>ip flow-export version 9</p>
<p>!</p>
<p>ipv6 route ::/0 2001:DB8:ACAD:2::1</p>
<p>!</p>
<p>no cdp run</p>
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