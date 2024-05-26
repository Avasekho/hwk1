<h1> ������������ ������. ��������� IPv6-������� �� ������� ����������� </h1> 

<h2> ��������� </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/Topology.png>

<h2> ������� ��������� </h2>

| ���������� | ��������� |     IPv6-�����     | ����� �������� | ���� �� ��������� |
|:----------:|:---------:|:------------------:|:--------------:|:-----------------:|
| R1         | G0/0/0    | 2001:db8:acad:a::1 |       64       |         -         |
|            | G0/0/1    | 2001:db8:acad:1::1 |       64       |         -         |
| S1         | VLAN 1    | 2001:db8:acad:1::b |       64       |         -         |
| PC-A       | NIC       | 2001:db8:acad:1::3 |       64       |      fe80::1      |
| PC-B       | NIC       | 2001:db8:acad:a::3 |       64       |      fe80::1      |

<h2> ������ </h2>

<ol>
  <li> ��������� ��������� � ������������ �������� ���������� �������������� � �����������. </li>
  <li> ������ ��������� IPv6-�������. </li>
  <li> �������� ��������� ����������. </li>
</ol>


<h2> ����� 1. ��������� ��������� � ������������ �������� ���������� �������������� � �����������.</h2> 
<h2> ������������ �������������� (R1):</h2> 

��������� �������������� ��� ����������.
<p> enable >> configure terminal </p>
<p> > hostname R1 </p>

��������� ����� DNS, ����� ������������� ������� �������������� ������� ��������������� ��������� ������� ����� �������, ��� ����� ��� �������� ������� �����.
<p> > no ip domain-lookup </p>

�������� class � �������� �������������� ������ ������������������ ������ EXEC.
<p> > enable secret class </p>

�������� cisco � �������� ������ ������� � �������� ���� � ������� �� ������.
<p> > line console 0 </p>
<p> > password cisco </p>

�������� cisco � �������� ������ VTY � �������� ���� � ������� �� ������.
<p> > line vty 0 15 </p>
<p> > password cisco </p>

��������� �������� ������.
<p> > service password-encryption </p>

�������� ������ � ��������������� � ������� �������������������� ������� � ����������.
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>

�������� IP-��������� ����������, ��� ������� � ������� ����.

<p> > interface g0/0/0 </p>
<p> > ipv6 address 2001:db8:acad:a::1/64 </p>
<p> > ipv6 address fe80::1 link-local </p>
<p> > no shutdown </p>

<p> > interface g0/0/1 </p>
<p> > ipv6 address 2001:db8:acad:1::1/64 </p>
<p> > ipv6 address fe80::1 link-local </p>
<p> > no shutdown </p>

�������� ������� ������������ � ���� ����������� ������������.
<p> > copy running-config startup-config </p>


<h2> ������������ ����������� (S1):</h2> 

�������� ����������� ��� ����������.
<p> enable >> configure terminal </p>
<p> > hostname S1 </p>

�������� ����� DNS, ����� ������������� ������� �������������� ������� ��������������� ��������� ������� ����� �������, ��� ����� ��� �������� ������� �����.
<p> > no ip domain-lookup </p>

�������� class � �������� �������������� ������ ������������������ ������ EXEC.
<p> > enable secret class </p>

�������� cisco � �������� ������ ������� � �������� ���� � ������� �� ������.
<p> > line console 0 </p>
<p> > password cisco </p>

�������� cisco � �������� ������ VTY � �������� ���� � ������� �� ������.
<p> > line vty 0 15 </p>
<p> > password cisco </p>

��������� �������� ������.
<p> > service password-encryption </p>

�������� ������ � ��������������� � ������� �������������������� ������� � ����������.
<p> > banner motd # Unauthorized access is strictly prohibited. # </p>

�������� ������� ������������ � ���� ����������� ������������.
<p> > copy running-config startup-config </p>

<h2> ����� 2. ������ ��������� IPv6-�������.</h2>

��� 1 - IPv6-������ ��������� ����������� Ethernet �� R1
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/r1_show.png>

��� 2 - ���������� IPv6-������������� �� R1

PC-B ����� � ����������� ��������� �������������, ��� ����������� IPv6 unicast-routing
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/pc_b_no_slaac.png>

������� IPv6 unicast-routing �� ��������������

<p> > configure terminal </p>
<p> > IPv6 unicast-routing </p>
<p> > exit </p>

PC-B ����� � ����������� ��������� �������������, � ���������� IPv6 unicast-routing
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/pc_b_slaac.png>

��� 3. ��������� IPv6-������ ���������� ���������� (SVI) �� S1.

��������� IPv6-��������� � ������� ������� sdm prefer dual-ipv4-and-ipv6 default.
<p> > configure terminal </p>
<p> > sdm prefer dual-ipv4-and-ipv6 default </p>
<p> > end </p>
<p> > reload </p>

��������� IP-��������� ����������, ��� ������� � ������� ����.

<p> > interface Vlan1 </p>
<p> > ipv6 address 2001:db8:acad:1::b/64 </p>
<p> > no shutdown </p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/s1_show_vlan.png>

��� 4. ��������� ����������� ����������� IPv6-������.

� �������� Ethernet ��� ������� �� � ��������� ��������� IPv6
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/pc_a_eth.png>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/pc_a_ipconfig.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/pc_b_eth.png>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/pc_b_ipconfig.png>



<h2> ����� 3. �������� ��������� �����������.</h2>

� PC-A ��������� ���-������ �� FE80::1. ��� ��������� ����� ������, ����������� G0/1 �� R1.
��������� ���-������ �� ��������� ���������� S1 � PC-A.

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/ping_1.png>

������� ������� tracert �� PC-A, ����� ��������� ������� ��������� ����������� � PC-B.

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/ping_2.png>

� PC-B ��������� ���-������ �� PC-A.
� PC-B ��������� ���-������ �� ��������� ����� ������ G0/0 �� R1.

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab04/ping_3.png>
