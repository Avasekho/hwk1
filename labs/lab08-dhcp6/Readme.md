<h1> ����������� ������ - ��������� DHCPv6 </h1> 

<h2> ��������� </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08/topology.png>

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