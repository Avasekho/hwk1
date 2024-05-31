<h1> ������������ ������. ��������� NAT ��� IPv4 </h1> 

<h2> ��������� </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/topology.png>

<h2> ������� ��������� </h2>

| ���������� | ��������� |      IP-�����      |   ����� �������    |
|:----------:|:---------:|:------------------:|:------------------:|
| R1         | G0/0/0    | 209.165.200.230    | 255.255.255.248    |
|            | G0/0/1    | 192.168.1.1        | 255.255.255.0      |
| R2         | G0/0/0    | 209.165.200.225    | 255.255.255.248    |
|            | Lo1       | 209.165.200.1      | 255.255.255.224    |
| S1         | VLAN 1    | 192.168.1.11       | 255.255.255.0      |
| S2         | VLAN 1    | 192.168.1.12       | 255.255.255.0      |
| PC-A       | NIC       | 192.168.1.2        | 255.255.255.0      |
| PC-B       | NIC       | 192.168.1.3        | 255.255.255.0      |

<h2> ������ </h2>

<ol>
  <li> �������� ���� � ��������� �������� ���������� ����������. </li>
  <li> ��������� � �������� NAT ��� IPv4. </li>
  <li> ��������� � �������� PAT ��� IPv4. </li>
  <li> ��������� � �������� ������������ NAT ��� IPv4. </li>
</ol>

<h2> �������� ���� � ��������� �������� ���������� ����������.</h2> 
<h2> ������������ �������������� �� ��������� (R2):</h2> 

a.	��������� �������������� ��� ����������.
<p> enable >> configure terminal </p>
<p> > hostname R2 </p>
b.	��������� ����� DNS, ����� ������������� ������� �������������� ������� ��������������� ��������� ������� ����� �������, ��� ����� ��� �������� ������� �����.
<p> > no ip domain-lookup </p>
c.	��������� class � �������� �������������� ������ ������������������ ������ EXEC.
<p> > enable secret class </p>
d.	��������� cisco � �������� ������ ������� � �������� ���� � ������� �� ������.
<p> > line console 0 </p>
<p> > password cisco </p>
e.	��������� cisco � �������� ������ VTY � �������� ���� � ������� �� ������.
<p> > line vty 0 15 </p>
<p> > password cisco </p>
f.	���������� �������� ������.
<p> > service password-encryption </p>
g.	�������� ������ � ��������������� � ������� �������������������� ������� � ����������.
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>
h.	��������� IP-��������� ����������, ��� ������� � ������� ����.

interface g0/0/0
ip address 209.165.200.225 255.255.255.248

��������� loopback ���������

interface loopback 1
ip address 209.165.200.1 255.255.255.224

i.	��������� ������� �� ���������. �� R2 ��  R1.
ip default-gateway 209.165.200.230

j.	��������� ������� ������������ � ���� ����������� ������������.
<p> > copy running-config startup-config </p>

���������� ��������� ��� R1

<h2> ������������ ����������� �� ��������� (S1):</h2> 

a.	��������� ����������� ��� ����������.
<p> enable >> configure terminal </p>
<p> > hostname S1 </p>

b.	��������� ����� DNS, ����� ������������� ������� �������������� ������� ��������������� ��������� ������� ����� �������, ��� ����� ��� �������� ������� �����.
<p> > no ip domain-lookup </p>

c.	��������� class � �������� �������������� ������ ������������������ ������ EXEC.
<p> > enable secret class </p>

d.	��������� cisco � �������� ������ ������� � �������� ���� � ������� �� ������.
<p> > line console 0 </p>
<p> > password cisco </p>

e.	��������� cisco � �������� ������ VTY � �������� ���� � ������� �� ������.
<p> > line vty 0 15 </p>
<p> > password cisco </p>

f.	���������� �������� ������.
<p> > service password-encryption </p>

g.	�������� ������ � ��������������� � ������� �������������������� ������� � ����������.
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>

h.	��������� IP-��������� ����������, ��� ������� � ������� ����.

<p> > interface Vlan1 </p>
<p> > ip address 192.168.1.11 255.255.255.0 </p>
<p> > no shutdown </p>

h.	��������� ��� ����������, ������� �� ����� ��������������.

<p> > interface range f0/4 - 17 </p>
<p> > shutdown </p>
<p> > interface range f0/19 - 24 </p>
<p> > shutdown </p>
<p> > interface range g0/1 - 2 </p>
<p> > shutdown </p>

j.	��������� ������� ������������ � ���� ����������� ������������.
<p> > copy running-config startup-config </p>

���������� ��� ����������� S2 

<h2> ��������� � �������� NAT ��� IPv4. </h2>

��� 1. �������� NAT �� R1, ��������� ��� �� ���� ������� 209.165.200.226-209.165.200.228. 

��������� ������� 192.168.1.0/24 � ������ �������, ��������� ��� ������� ��� NAT � ����������� ��� � ������ �������

<p> > access-list 1 permit 192.168.1.0 0.0.0.255 </p>
<p> > ip nat pool PUBLIC_ACCESS 209.165.200.226 209.165.200.228 netmask 255.255.255.248 </p>
<p> > ip nat inside source list 1 pool PUBLIC_ACCESS </p>

���������� ���������� ���������

<p> > interface g0/0/1 </p>
<p> > ip nat inside </p>

���������� ������� ���������

<p> > interface g0/0/0 </p>
<p> > ip nat outside </p>

��������� ��������� ������������ NAT:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_err_1.png>

�� �������� :/

����� � PC-B �� R2 �� ��������, ���������� ��������� R1 ��������, ������� NAT �����, �� ���������� ��� � misses.
G0/0/0 �� R2 c R1 ���������.

��� ���������� �� �� ����� ������� ���� �� ���������. ����� ����� ���� �������� ���� �� ���������� R2, �� loopback ��� ��� �� ���������. 

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_err_2.png>

�� ���� ��������� R1 �� ����� ��� ������ ���� �����.

�������� ����������� ����� ���� ����� ������ ��� 209.165.200.1:

<p> > ip route 209.165.200.0 255.255.255.224 GigabitEthernet0/0/0 </p>

����� �� PC-B �� loopback ���������� �����

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_1.png>

NAT-������� ����� ����� ����������� ��������

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/nat_1.png>


�������� ����� � PC-B �� loopback:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_err_3.png>

�� ����� ������ ��� ���� �� ������ ���� �� ���������. ����� ���� ��� ��������� ���� ���������� - ���� ����, NAT-������� �������� ��������������� ������

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_2.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/nat_2.png>

��������� ����������� 209.165.200.1 � ����������� S1:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_err_4.png>

�������� �� ����, �.�. �� ����������� �� ��� ����� ���� �� ���������. ������� ����:

<p> > ip default-gateway 192.168.1.1 </p>

���� �����, ������� ����������� ��������. ��� ��� ������ �� ���� NAT ��� ����.

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_3.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/nat_3.png>

������ ��������� ����������� 209.165.200.1 � ����������� S2:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_err_5.png>

��������, ������ � ���� �����������.

������� "show ip nat translations verbose" �������� ������������

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/verbose_err1.png>


������� ������ ����������

<p> > clear ip nat translation * </p>


<h2>  ��������� � �������� PAT ��� IPv4 </h2>

������� ��������� �������������� � NAT �� PAT. ��� ����� ������� ������ ��������� NAT �� R1

<p> > no ip nat inside source list 1 pool PUBLIC_ACCESS </p>

� ������� �����

<p> > ip nat inside source list 1 pool PUBLIC_ACCESS overload </p>

��������� ����������� � PC-B �� Lo1 - ��� ��������, ����� �������������

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_4.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/pat_1.png>


���������� � PC-A

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_5.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/pat_2.png>


��������� ������� ping ����� � ���� ��������� - R1 ����������� �� � ���� ����� �� ����, �� ������ �����

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/pat_3.png>

������� ������ ����������

<p> > clear ip nat translation * </p>


������ ������ ������� �������������� nat pool

<p> > no ip nat inside source list 1 pool PUBLIC_ACCESS overload </p>
<p> > no ip nat pool PUBLIC_ACCESS </p>

������� ������� PAT, ������ � � ������� �����������

 <p> > ip nat inside source list 1 interface g0/0/0 overload </p>

 �������� ������ ���������� �������:

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/pat_4.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/pat_5.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/pat_6.png>

������ R1 ����������� ���������� ������ � ����� �������� ����������


<h2>  ��������� � �������� ������������ NAT ��� IPv4. </h2>


������� ������ ����������

<p> > clear ip nat translation * </p>


�������� ����������� ������������� ����������� ������ � �������

<p> > ip nat inside source static 192.168.1.2 209.165.200.229 </p>

�������� ������������� ������ ����������

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_6.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/nat_4.png>

����� ����� 209.165.200.229 �������� ��� ����� � R2

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab12/ping_7.png>

����� �������, ����������� NAT ��������