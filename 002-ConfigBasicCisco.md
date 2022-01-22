# Konfigurasi Dasar Cisco Router

Dalam lab pertama tentang konfigurasi Cisco router ini, kita akan mencoba untuk menerapkan konfigurasi dasar yang biasa dibuat pertama kali seperti memberikan nama device, memberikan password, memberikan IP Address, menampilkan, menyimpan dan menghapus konfigurasi yang telah kita buat dan lain sebagainya.

Sebagai bahan latihan disini kita akan menggunakan bantuan software Packet Tracer dengan topology seperti berikut:

<img src="https://drive.google.com/uc?export=view&id=1EHQfzz4v93D4yKt36FqYz8zMBYExToBp">

## Mengenal Mode dalam Konfigurasi Cisco Router
Sebelum melakukan konfigurasi, ada baiknya kita mengetahui terlebih dahulu, bahwa dalam perintah Cisco terdapat 3 mode yaitu <b>User Mode, Privilege Mode, dan Global Configuration Mode.</b>
### User Mode
Mode ini adalah mode awal saat melakukan console pada router. Dalam User Mode anda akan melihat tanda <b>Router></b>. Dalam mode ini kita hanya bisa melihat informasi dasar pada <b>router.</b>
### Privilege Mode
Mode selanjutnya adalah Privilege Mode, kita bisa masuk ke mode ini dengan mengetikkan <b>enable</b> atau cukup ketik <b>en</b> saja. Setelah masuk maka tanda akan berubah menjadi <b>Router#.</b>
### Global Configuration Mode
Mode ini merupakan mode tertinggi dimana kita bisa melakukan hampir semua konfigurasi terhadap Router. Untuk bisa mengakses mode ini setelah ketik <b>configure terminal</b> atau bisa juga diketik <b>conf t</b> saat kita berada pada mode Privilege Mode, Setelah masuk maka tanda akan berubah menjadi <b>Router(config)#</b>.

Untuk melihat perintah-perintah apa saja yang dapat dijalankan pada User Mode, Privilege Mode atau Global Configuration Mode, cukup ketikkan tanda <b>?</b> pada masing-masing mode tersebut.

## Step by step Konfigurasi Dasar Cisco Router
Sebelum mengkonfigurasi Router, kita akan cek dulu konfigurasi yang saat ini, dengan perintah <b>show running-confi</b> untuk konfigurasi saat ini yang sedang running atau dengan perintah <b>show startup-config</b> untuk melihat konfigurasi yang telah tersimpan dan akan di-load pada saat router start-up.

<pre>Router>en
Router# show startup-config 
startup-config is not present</pre>

<pre>Router>en
Router#show running-config 
Building configuration...

Current configuration : 553 bytes
!
version 12.4
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname Router
!
!
!
!
!
!
!
!
ip cef
no ipv6 cef
!
!
!
!
!
!
!
!
!
!
!
!
spanning-tree mode pvst
!
!
!
!
!
!
interface FastEthernet0/0
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface FastEthernet0/1
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface Vlan1
 no ip address
 shutdown
!
ip classless
!
ip flow-export version 9
!
!
!
!
!
!
!
!
line con 0
!
line aux 0
!
line vty 0 4
 login
!
!
!
end</pre>

Router>en
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname pcnetlabs-R0
pcnetlabs-R0(config)#

pcnetlabs-R0>en
pcnetlabs-R0#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
pcnetlabs-R0(config)#enable password pcnetpass
pcnetlabs-R0(config)#enable secret pcnetsec

pcnetlabs-R0(config)#line console 0
pcnetlabs-R0(config-line)#password pcnetconsolepass
pcnetlabs-R0(config-line)#exec-timeout 5
pcnetlabs-R0(config-line)#login
pcnetlabs-R0(config-line)#

pcnetlabs-R0(config)#line vty 0 4
pcnetlabs-R0(config-line)#password pcnetvtypass
pcnetlabs-R0(config-line)#exec-timeout 5
pcnetlabs-R0(config-line)#login


pcnetlabs-R0(config)#line aux 0
pcnetlabs-R0(config-line)#password pcnetauxpass
pcnetlabs-R0(config-line)#exec-timeout 5
pcnetlabs-R0(config-line)#login

pcnetlabs-R0(config)#interface fastEthernet 0/0
pcnetlabs-R0(config-if)#ip address 192.168.2.1 255.255.255.0
pcnetlabs-R0(config-if)#no shut

pcnetlabs-R0(config)#interface fastEthernet 0/1
pcnetlabs-R0(config-if)#ip address 10.0.0.1 255.255.255.0
pcnetlabs-R0(config-if)#no shut

pcnetlabs-R0#ping 192.168.2.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.2.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/1/4 ms

pcnetlabs-R0#ping 10.0.0.2

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.0.0.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/1 ms

pcnetlabs-R0#write memory 
Building configuration...
[OK]

pcnetlabs-R0#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]

pcnetlabs-R0#show startup-config 
Using 750 bytes
!
version 12.4
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname pcnetlabs-R0
!
!
!
enable secret 5 $1$mERr$bmas.R1n8VP6RVcpQIqsk0
enable password pcnetpass
!
!
!
!
!
!
ip cef
no ipv6 cef
!
!
!
!
!
!
!
!
!
!
!
!
spanning-tree mode pvst
!
!
!
!
!
!
interface FastEthernet0/0
 ip address 192.168.2.1 255.255.255.0
 duplex auto
 speed auto
!
interface FastEthernet0/1
 ip address 10.0.0.1 255.255.255.0
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
!
!
!
!
!
!
!
line con 0
 exec-timeout 5 0
 password pcnetconsolepass
 login
!
line aux 0
!
line vty 0 4
 exec-timeout 5 0
 password pcnetvtypass
 login
!
!
!
end