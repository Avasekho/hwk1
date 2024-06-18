<h1> ������������ ������ - ��������� ������������� ����� ������������ ���������� ������ </h1> 

<h2> ��������� </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab08/topology.png>

<h2> ������� ��������� </h2>

| ���������� |  ���������  |   IP-�����    |  ����� �������  | ���� �� ��������� |
|:----------:|:-----------:|:-------------:|:---------------:|:-----------------:|
| R1         | G0/0/1.10   | 192.168.10.1  | 255.255.255.0   |         -         |
|            | G0/0/1.20   | 192.168.20.1  | 255.255.255.0   |         -         |
|            | G0/0/1.30   | 192.168.30.1  | 255.255.255.0   |         -         |
|            | G0/0/1.1000 |       -       |       -         |         -         |
| S1         | VLAN 10     | 192.168.10.11 | 255.255.255.0   | 192.168.10.1      |
| S2         | VLAN 10     | 192.168.10.12 | 255.255.255.0   | 192.168.10.1      |
| PC-A       | NIC         | 192.168.20.3  | 255.255.255.0   | 192.168.20.1      |
| PC-b       | NIC         | 192.168.30.3  | 255.255.255.0   | 192.168.30.1      |

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
h.	��������� �� ������������ �����.
</blockquote>
<p> > clock set 16:02:00 09 June 2024 </p>

<blockquote>
i.	���������� ������� ������������ � �������� ���������.
</blockquote>
<p> > copy running-config startup-config </p>


���������� ������� ��������� ���������������:

<blockquote>
a.	������������ � �������������� � ������� ������� � ����������� ����������������� ����� EXEC.
b.	������� � ����� ������������.
</blockquote>
<p> enable >> configure terminal </p>

<blockquote>
c.	��������� �������������� ��� ����������.
</blockquote>
<p> hostname R1 </p>

<blockquote>
d.	��������� ����� DNS, ����� ������������� ������� �������������� ������� ��������������� ��������� ������� ����� �������, ��� ����� ��� �������� ������� �����.
</blockquote>
<p> > no ip domain-lookup </p>

<blockquote>
e.	��������� class � �������� �������������� ������ ������������������ ������ EXEC.
</blockquote>
<p> > enable secret class </p>

<blockquote>
f.	��������� cisco � �������� ������ ������� � �������� ���� � ������� �� ������.
</blockquote>
<p> > line console 0 </p>
<p> > password cisco </p>

<blockquote>
g.	���������� cisco � �������� ������ ������������ ��������� � ����������� ����.
</blockquote>
<p> > line vty 0 15 </p>
<p> > password cisco </p>

<blockquote>
h.	���������� �������� ������.
</blockquote>
<p> > service password-encryption </p>

<blockquote>
i.	�������� ������ � ��������������� � ������� �������������������� ������� � ����������.
</blockquote>
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>

<blockquote>
j.	��������� ������� ������������ � ���� ����������� ������������.
</blockquote>
<p> > copy running-config startup-config </p>

<blockquote>
k.	��������� �� �������������� �����.
</blockquote>
<p> > clock set 15:57:00 09 June 2024 </p>