<h1> ������������ ������. ������������� ������������� ���� � ���������� �������� </h1> 

<h2> ��������� </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab07/topology.png>

<h2> ������� ��������� </h2>

| ���������� |  ���������  |   IP-�����   | ����� ������� |
|:----------:|:-----------:|:------------:|:-------------:|
| S1         | VLAN 1      | 192.168.1.1  | 255.255.255.0 |
| S2         | VLAN 1      | 192.168.1.2  | 255.255.255.0 |
| S3         | VLAN 1      | 192.168.1.3  | 255.255.255.0 |


<h2> ������� VLAN </h2>

|   VLAN   |     ���      |     ����������� ���������     |
|:--------:|:------------:|:-----------------------------:|
| 10       | ����������   | S1: VLAN 10                   | 
|          |              | S2: VLAN 10                   |
| 20       | Sales        | S1: F0/6                      |
| 30       | Operations   | S2: F0/18                     |
| 999      | Parking_Lot  | S1: F0/2-4, F0/7-24, G0/1-2   |
|          |              | S2: F0/2-17, F0/19-24, G0/1-2 |
| 1000     | �����������  |               -               |

<h2> ������ </h2>

<ol>
  <li> �������� ���� � ��������� �������� ���������� ����������. </li>
  <li> ����� ��������� �����. </li>
  <li> ���������� �� ��������� ������ ���������� STP �����, ������ �� ��������� ������. </li>
  <li> ���������� �� ��������� ������ ���������� STP �����, ������ �� ���������� ������. </li>
</ol>

<h2> ����� 1. �������� ���� � ��������� �������� ���������� ���������� </h2>

�������� ������� ��������� ��� ������������:

<blockquote>
a.	��������� ����� DNS.
</blockquote>
<p> > no ip domain-lookup </p>

<blockquote>
b.	��������� ����� ����������� � ������������ � ����������.
</blockquote>
<p> hostname S1 </p>
<p> hostname S2 </p>
<p> hostname S3 </p>

<blockquote>
c.	��������� class � �������� �������������� ������ ������� � ������������������ ������.
</blockquote>
<p> > enable secret class </p>

<blockquote>
d.	��������� cisco � �������� ������� ������� � VTY � ����������� ���� ��� ������� � VTY �������.
</blockquote>
<p> > line console 0 </p>
<p> > password cisco </p>
<p> > line vty 0 15 </p>
<p> > password cisco </p>
<p> > service password-encryption </p>

<blockquote>
e.	��������� logging synchronous ��� ����������� ������.
</blockquote>
<p> > line console 0 </p>
<p> > logging synchronous </p>

<blockquote>
f.	��������� ��������� ��������� ��� (MOTD) ��� �������������� ������������� � ������� �������������������� �������.
</blockquote>
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>

<blockquote>
g.	������� IP-�����, ��������� � ������� ��������� ��� VLAN 1 �� ���� ������������.
</blockquote>
<p> > interface vlan 1 </p>
<p> > ip address 192.168.1.1 255.255.255.0 </p>
<p> > no shutdown </p>
// ���������� �� ������ ������������

<blockquote>
h.	���������� ������� ������������ � ���� ����������� ������������.
</blockquote>
<p> > copy running-config startup-config </p>

�������� �����:

<blockquote>
������� �� ����������� ���-������ �� ����������� S1 �� ���������� S2?
</blockquote>
����������� �������
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab07/ping_1.png>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab07/trace_1.png>

<blockquote>
������� �� ����������� ���-������ �� ����������� S1 �� ���������� S3?
</blockquote>
����������� �������
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab07/ping_2.png>

<blockquote>
������� �� ����������� ���-������ �� ����������� S2 �� ���������� S3?
</blockquote>
����������� �������
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab07/ping_3.png>


<h2> ����� 2. ����������� ��������� ����� </h2>

�������� ��� ����� �� ������������

<p> > interface range f0/1-24 </p>
<p> > shutdown </p>
<p> > interface range g0/1-2 </p>
<p> > shutdown </p>
// ���������� �� ������ ������������

�������� ������������ ����� � �������� ���������:

<p> > interface range F0/1-4 </p>
<p> > switchport trunk allowed vlan 1 </p>
// ���������� �� ������ ������������

������� ����� F0/2 � F0/4 �� ���� ������������:

<p> > interface F0/2 </p>
<p> > no shutdown </p>
<p> > interface F0/4 </p>
<p> > no shutdown </p>
// ���������� �� ������ ������������

��������� ������ ��������� spanning-tree:

S1:
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab07/stp_1.png>

S2:
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab07/stp_2.png>

S3:
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab07/stp_3.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab07/topology_2.png>

<blockquote>
����� ���������� �������� �������� ������?
</blockquote>
S3

<blockquote>
������ ���� ���������� ��� ������ ���������� spanning-tree � �������� ��������� �����?
</blockquote>



<blockquote>
����� ����� �� ����������� �������� ��������� �������?
</blockquote>



<blockquote>
����� ����� �� ����������� �������� ������������ �������?
</blockquote>



<blockquote>
����� ���� ������������ � �������� ��������������� � � ��������� ����� ������������?
</blockquote>




<blockquote>
������ �������� spanning-tree ������ ���� ���� � �������� ������������� (����������������) �����?
</blockquote>



