<h1> ������������ ������ - ��������� ���������� CDP, LLDP � NTP </h1> 

<h2> ��������� </h2>
<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/topology.png>

<h2> ������� ��������� </h2>

| ���������� |  ���������  |   IP-�����   |  ����� �������  | ���� �� ��������� |
|:----------:|:-----------:|:------------:|:---------------:|:-----------------:|
| R1         | Loopback 1  | 172.16.1.1   | 255.255.255.0   |         -         |
|            | G0/0/1      | 10.22.0.1    | 255.255.255.0   |         -         |
| S1         | SVI VLAN 1  | 10.22.0.2    | 255.255.255.0   | 10.22.0.1         |
| S2         | SVI VLAN 1  | 10.22.0.3    | 255.255.255.0   | 10.22.0.1         |

<h2> ������ </h2>

<ol>
  <li> �������� ���� � ��������� �������� ���������� ���������� </li>
  <li> ����������� ������� �������� � ������� ��������� CDP </li>
  <li> ����������� ������� �������� � ������� ��������� LLDP </li>
  <li> ��������� � �������� NTP </li>
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
h.	��������� �����������, ������������� � ������� ����
</blockquote>
<p> > interface G0/0/1</p>
<p> > ip address 10.22.0.1 255.255.255.0</p>
<p> > no shutdown</p>
<p> > exit</p>
<p> > interface loopback 1</p>
<p> > ip address 172.16.1.1 255.255.255.0</p>
<p> > exit</p>

<blockquote>
i.	��������� ������� ������������ � ���� ����������� ������������.
</blockquote>
<p> > copy running-config startup-config </p>



<h2>�������� �����������:</h2>

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
h.	��������� �������������� ����������
</blockquote>

<p> > interface range F0/2-4</p>
<p> > shutdown</p>
<p> > exit</p>
<p> > interface range F0/6-24</p>
<p> > shutdown</p>
<p> > exit</p>
<p> > interface range G0/1-2</p>
<p> > shutdown</p>
<p> > exit</p>

// ���������� ��� S2

<p> > interface range F0/2-24</p>
<p> > shutdown</p>
<p> > exit</p>
<p> > interface range G0/1-2</p>
<p> > shutdown</p>
<p> > exit</p>


<blockquote>
i.	��������� ������� ������������ � ���� ����������� ������������.
</blockquote>
<p> > copy running-config startup-config </p>


<h2> ����� 2. ����������� ������� �������� � ������� ��������� CDP </h2> 

<blockquote>
a.	�� R1 ����������� ��������������� ������� show cdp, ����� ����������, ������� ����������� �������� CDP, ������� �� ��� �������� � ������� ���������.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/cdp_1.png>

<blockquote>
������� ����������� ��������� � ����������� CDP? ����� �� ��� �������?
</blockquote>

<p>3 ����������, ������� GigabitEthernet0/0/1</p>


<blockquote>
b.	�� R1 ����������� ��������������� ������� show cdp, ����� ���������� ������ IOS, ������������ �� S1.
</blockquote>

<p>Version :</p>
<p>Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE4</p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/cdp_2.png>


<blockquote>
c.	�� S1 ����������� ��������������� ������� show cdp, ����� ����������, ������� ������� CDP ���� ��������.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/cdp_3.png>

<p>������� �� �������������� � Packet Tracer</p>

<blockquote>
d.	��������� SVI ��� VLAN 1 �� S1 � S2, ��������� IP-������, ��������� � ������� ��������� ����. ��������� ���� �� ��������� ��� ������� ����������� �� ������ ������� �������.
</blockquote>

S1:
<p> > interface Vlan1</p>
<p> > ip address 10.22.0.2 255.255.255.0</p>
<p> > no shutdown</p>
<p> > exit</p>
<p> > ip default-gateway 10.22.0.1</p>

S2:
<p> > interface Vlan1</p>
<p> > ip address 10.22.0.3 255.255.255.0</p>
<p> > no shutdown</p>
<p> > exit</p>
<p> > ip default-gateway 10.22.0.1</p>


<blockquote>
<p>e.	�� R1 ��������� ������� show cdp entry S1 . </p>
<p>������:</p>
<p>����� �������������� �������� �������� ������?</p>
</blockquote>

<p>IP-�����</p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/cdp_4.png>


<blockquote>
f.	��������� CDP ��������� �� ���� �����������. 
</blockquote>

<p> > no cdp run</p>


<h2> ����� 3. ����������� ������� �������� � ������� ��������� LLDP </h2> 

<blockquote>
a.	������� ��������������� ������� lldp, ����� �������� LLDP �� ���� ����������� � ���������.
</blockquote>

<p> > lldp run</p>


<blockquote>
b.	�� S1 ��������� ��������������� ������� lldp, ����� ������������ ��������� ���������� � S2. 
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/lldp_1.png>

<blockquote>
��� ����� chassis ID  ��� ����������� S2?
</blockquote>
<p> MAC-����� ����� ������� ��������� � S1 </p>


<blockquote>
c.	����������� ����� ������� �� ���� ����������� � ����������� ������� LLDP, ����������� ��� ����������� ��������� ���������� ���� ������ �� �������� ������ ������� show.
</blockquote>


<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/lldp_2.png>


<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/lldp_3.png>


<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/lldp_4.png>



<h2> ����� 4. ��������� NTP </h2> 


<h2> ������� �� ����� ������� ����� </h2> 

<blockquote>
������� ������� show clock ��� ����������� �������� ������� �� R1. �������� ������������ �������� � ������� ������� � ��������� �������.
</blockquote>

R1:
<p> > show clock</p>

|    ����    |    �����    | ������� ���� | �������� ������� |
|:----------:|:-----------:|:------------:|:----------------:|
| Mar 1 1993 | 1:59:50.860 |     UTC      |   R1 hardware    |

<h2> ��������� ����� </h2> 

<blockquote>
� ������� ������� clock set ���������� ����� �� �������������� R1. ��������� ����� ������ ���� � ������� UTC. 
</blockquote>

<p> > clock timezone msk 3</p>
<p> > clock set 18:00:00 07 July 2024</p>

<h2> �������� ������� ������ NTP </h2> 

<blockquote>
��������� R1 � �������� ������� NTP � ������� ���� 4.
</blockquote>

<p> > ntp master 4</p>


<h2> �������� ������ NTP </h2> 


<blockquote>
a.	��������� ��������������� ������� �� S1 � S2, ����� ����������� ����������� �����. �������� ������� �����,  � ��������� �������.
</blockquote>

|    ����    |    �����    | ������� ���� | �������� ������� |
|:----------:|:-----------:|:------------:|:----------------:|
| Mar 1 1993 | 2:26:52.289 |     UTC      |   S1 hardware    |
| Mar 1 1993 | 2:0:14.111  |     UTC      |   S2 hardware    |


<blockquote>
b.	��������� S1 � S2 � �������� �������� NTP. ����������� ��������������� ������� NTP ��� ��������� ������� �� ���������� G0/0/1 R1, � ����� ��� �������������� ���������� ��������� ��� ���������� ����� �����������.
</blockquote>


S1:
<p> > ntp server 10.22.0.1</p>

S2:
<p> > ntp server 10.22.0.1</p>


<h2> �������� ��������� NTP </h2> 

<blockquote>
a.	����������� ��������������� ������� show , ����� ���������, ��� S1 � S2 ���������������� � R1.
</blockquote>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/ntp_5.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/ntp_2.png>

<blockquote>
b.	��������� ��������������� ������� �� S1 � S2, ����� ����������� ����������� ����� � �������� ����� ���������� �����.
</blockquote>

<p>���� �����������:</p>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/ntp_1.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/ntp_3.png>

<img src=https://github.com/Avasekho/otus-networks-basic/blob/main/labs/lab13/ntp_4.png>


������������ �����������:

R1#sh run
<blockquote>
<p>Building configuration...</p>
<p></p>
<p>Current configuration : 924 bytes</p>
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
<p>clock timezone msk 3</p>
<p>!</p>
<p>ip cef</p>
<p>no ipv6 cef</p>
<p>!</p>
<p>lldp run</p>
<p>!</p>
<p>no ip domain-lookup</p>
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
<p> ip address 10.22.0.1 255.255.255.0</p>
<p> duplex auto</p>
<p> speed auto</p>
<p>!</p>
<p>interface Vlan1</p>
<p> no ip address</p>
<p> shutdown</p>
<p>!</p>
<p>router rip</p>
<p>!</p>
<p>ip classless</p>
<p>!</p>
<p>ip flow-export version 9</p>
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
<p>ntp master 4</p>
<p>!</p>
<p>end</p>
</blockquote>


������������ �����������:

S1#sh run
<blockquote>
<p>Building configuration...</p>
<p></p>
<p>Current configuration : 1604 bytes</p>
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
<p>!</p>
<p>lldp run</p>
<p>!</p>
<p>spanning-tree mode pvst</p>
<p>spanning-tree extend system-id</p>
<p>!</p>
<p>interface FastEthernet0/1</p>
<p>!</p>
<p>interface FastEthernet0/2</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/3</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/4</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/5</p>
<p>!</p>
<p>interface FastEthernet0/6</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/7</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/8</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/9</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/10</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/11</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/12</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/13</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/14</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/15</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/16</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/17</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/18</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/19</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/20</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/21</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/22</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/23</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/24</p>
<p> shutdown</p>
<p>!</p>
<p>interface GigabitEthernet0/1</p>
<p> shutdown</p>
<p>!</p>
<p>interface GigabitEthernet0/2</p>
<p> shutdown</p>
<p>!</p>
<p>interface Vlan1</p>
<p> ip address 10.22.0.2 255.255.255.0</p>
<p>!</p>
<p>ip default-gateway 10.22.0.1</p>
<p>!</p>
<p>no cdp run</p>
<p>!</p>
<p>banner motd ^C Unauthorized access is strictly prohibited. ^C</p>
<p>!</p>
<p>line con 0</p>
<p> password 7 0822455D0A16</p>
<p>!</p>
<p>line vty 0 4</p>
<p> password 7 0822455D0A16</p>
<p> login</p>
<p>line vty 5 15</p>
<p> password 7 0822455D0A16</p>
<p> login</p>
<p>!</p>
<p>ntp server 10.22.0.1</p>
<p>!</p>
<p>end</p>
</blockquote>



S2#sh run
<blockquote>
<p>Building configuration...</p>
<p></p>
<p>Current configuration : 1614 bytes</p>
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
<p>!</p>
<p>lldp run</p>
<p>!</p>
<p>spanning-tree mode pvst</p>
<p>spanning-tree extend system-id</p>
<p>!</p>
<p>interface FastEthernet0/1</p>
<p>!</p>
<p>interface FastEthernet0/2</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/3</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/4</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/5</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/6</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/7</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/8</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/9</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/10</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/11</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/12</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/13</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/14</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/15</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/16</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/17</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/18</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/19</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/20</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/21</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/22</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/23</p>
<p> shutdown</p>
<p>!</p>
<p>interface FastEthernet0/24</p>
<p> shutdown</p>
<p>!</p>
<p>interface GigabitEthernet0/1</p>
<p> shutdown</p>
<p>!</p>
<p>interface GigabitEthernet0/2</p>
<p> shutdown</p>
<p>!</p>
<p>interface Vlan1</p>
<p> ip address 10.22.0.3 255.255.255.0</p>
<p>!</p>
<p>ip default-gateway 10.22.0.1</p>
<p>!</p>
<p>no cdp run</p>
<p>!</p>
<p>banner motd ^C Unauthorized access is strictly prohibited. ^C</p>
<p>!</p>
<p>line con 0</p>
<p> password 7 0822455D0A16</p>
<p>!</p>
<p>line vty 0 4</p>
<p> password 7 0822455D0A16</p>
<p> login</p>
<p>line vty 5 15</p>
<p> password 7 0822455D0A16</p>
<p> login</p>
<p>!</p>
<p>ntp server 10.22.0.1</p>
<p>!</p>
<p>end</p>
</blockquote>