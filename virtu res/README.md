## 1. .

Pour rappel : le tableau d'adressage.

|      Machines       | IP correspondate |
|:-------------------:|:----------------:|
|  `node1.tp1.efrei`  |  `10.1.1.1/24`   |
|  `node2.tp1.efrei`  |  `10.1.1.2/24`   |

---

### 1.1. Les adresses MAC.

> Machine 1:

```
PC1> show ip

NAME        : PC1[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 10000
RHOST:PORT  : 127.0.0.1:10001
MTU:        : 1500
```

> Machine 2:

```
PC2> show ip

NAME        : PC2[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 10008
RHOST:PORT  : 127.0.0.1:10009
MTU:        : 1500

PC2>
```

### 1.2. Les adresses IP.

> PC1:

```
PC1> ip 10.1.1.1/24
Checking for duplicate address...
PC1 : 10.1.1.1 255.255.255.0

PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.1.1.1/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 10000
RHOST:PORT  : 127.0.0
```

> PC2:

```
PC2> ip 10.1.1.2/24
Checking for duplicate address...
PC1 : 10.1.1.2 255.255.255.0

PC2> show ip

NAME        : PC2[1]
IP/MASK     : 10.1.1.2/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 10008
RHOST:PORT  : 127.0.0.1:10009
MTU:        : 1500
```

### 1.3. Ping.

> De PC1 à PC2:
```
PC1> ping 10.1.1.2
84 bytes from 10.1.1.2 icmp_seq=1 ttl=64 time=0.177 ms
84 bytes from 10.1.1.2 icmp_seq=2 ttl=64 time=0.475 ms
84 bytes from 10.1.1.2 icmp_seq=3 ttl=64 time=0.355 ms
```

> De PC2 à PC1:
```
PC2> ping 10.1.1.1
84 bytes from 10.1.1.1 icmp_seq=1 ttl=64 time=0.369 ms
84 bytes from 10.1.1.1 icmp_seq=2 ttl=64 time=0.370 ms
84 bytes from 10.1.1.1 icmp_seq=3 ttl=64 time=0.367 ms
```

### 1.4. Wireshark.

[Ping entre les deux machines](pcaps/Capture_pings.pcapng)

### 1.5 Requêtes ARP.

> après ping du PC1 vers PC2:
```
PC1> arp

00:50:79:66:68:01  10.1.1.2 expires in 117 seconds
```

> Ensuite, on ping du PC2 vers PC1:
```
PC2> arp

00:50:79:66:68:00  10.1.1.1 expires in 78 seconds
```

---

## 2. Switch.

|      Machines       | IP correspondate |
|:-------------------:|:----------------:|
|  `node1.tp1.efrei`  |  `10.1.1.1/24`   |
|  `node2.tp1.efrei`  |  `10.1.1.2/24`   |
|  `node3.tp1.efrei`  |  `10.1.1.3/24`   |

### 2.1 Adresses MAC + attribution IP.

> Machine 1:

```
PC1> ip 10.1.1.1/24
Checking for duplicate address...
PC1 : 10.1.1.1 255.255.255.0

PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.1.1.1/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 10006
RHOST:PORT  : 127.0.0.1:10007
MTU:        : 1500
```

> Machine 2:

```
PC2> ip 10.1.1.2/24
Checking for duplicate address...
PC1 : 10.1.1.2 255.255.255.0

PC2> show ip

NAME        : PC2[1]
IP/MASK     : 10.1.1.2/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 10008
RHOST:PORT  : 127.0.0.1:10009
MTU:        : 1500
```

> Machine 3:

```
PC3> ip 10.1.1.3/24
Checking for duplicate address...
PC3 : 10.1.1.3 255.255.255.0

PC3> show ip

NAME        : PC1[1]
IP/MASK     : 10.1.1.3/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:02
LPORT       : 10009
RHOST:PORT  : 127.0.0.1:10009
MTU:        : 1500
```

### 2.2. Ping.

> De node1 à node2:
```
PC1> ping 10.1.1.2
84 bytes from 10.1.1.2 icmp_seq=1 ttl=64 time=0.379 ms
84 bytes from 10.1.1.2 icmp_seq=2 ttl=64 time=0.574 ms
```

> De node2 à node3:
```
PC2> ping 10.1.1.3
84 bytes from 10.1.1.3 icmp_seq=1 ttl=64 time=0.393 ms
84 bytes from 10.1.1.3 icmp_seq=2 ttl=64 time=0.375 ms
84 bytes from 10.1.1.3 icmp_seq=3 ttl=64 time=0.362 ms
```

> De node1 à node3:
```
PC1> ping 10.1.1.3
84 bytes from 10.1.1.3 icmp_seq=1 ttl=64 time=0.336 ms
84 bytes from 10.1.1.3 icmp_seq=2 ttl=64 time=0.484 ms
84 bytes from 10.1.1.3 icmp_seq=3 ttl=64 time=0.459 ms
```

## 3. Serveur DHCP.

|      Machines       | IP correspondate |
|:-------------------:|:----------------:|
|  `node1.tp1.efrei`  |      `N/A`       |
| `node2.tp1.efrei`  |        `N/A`     |
|  `node3.tp1.efrei`  |      `N/A`       |
|  `dhcp.tp1.efrei`   | `10.1.1.253/24`  |

### 3.1.

#### 3.1.1. Configuration du serveur DHCP.

```shell
\[root@localhost \~\]# curl ifconfig.me 84.235.235.54
\[root@localhost \~\]# ping kali.download
PING kali.download (104.17.254.239) 56(84) bytes of data.
64 bytes from 104.17.254.239 (104.17.254.239): icmp_seq=1 ttl=57 time=24.9 ms
64 bytes from 104.17.254.239 (104.17.254.239): icmp_seq=2 ttl=57 time=24.2 ms
64 bytes from 104.17.254.239 (104.17.254.239): icmp_seq=3 ttl=57 time=24.4 ms
^C
--- kali.download ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2005ms
rtt min/avg/max/mdev = 24.224/24.482/24.863/0.274 ms
[root@localhost ~]#
```

> Configuration IP:

```shell
[root@localhost ~]# cat /etc/sysconfig/network-scripts/ifcfg-en0s3
DEVICE=enp0s3
NAME=Meow

ONBOOT=YES
BOOTPROTO=static

IPADDR=10.1.1.253
NETMASK=255.255.255.0
[root@localhost ~]#
```

```shell
[root@localhost ~]# dnf-y install dhcp-server
[root@localhost ~]# vim /etc/dhcp/dhcpd.conf
```

> Contenu de /etc/dhcp/dhcpd.conf :

```
authoritative;
subnet 10.1.1.0 netmask 255.255.255.0 {
	range 10.1.1.10 10.1.1.50;
	option broadcast-address 10.1.1.1;
	option routers 10.1.1.1;
}
```

```shell
systemctl enable --now dhcpd
```

#### 3.1.2. Récupération d'une IP depuis un client.

> Machine 1:
```
C1> dhcp
DORA IP 10.1.1.10/24 GW 10.1.1.1

PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.1.1.10/24
GATEWAY     : 10.1.1.1
DNS         :
DHCP SERVER : 10.1.1.253
DHCP LEASE  : 42892, 42898/21449/37535
MAC         : 00:50:79:66:68:02
LPORT       : 10008
RHOST:PORT  : 127.0.0.1:10009
MTU:        : 1500

PC1>
```

> Machine 2:
```
PC2> dhcp
DORA IP 10.1.1.11/24 GW 10.1.1.1

PC2> show ip

NAME        : PC2[1]
IP/MASK     : 10.1.1.11/24
GATEWAY     : 10.1.1.1
DNS         :
DHCP SERVER : 10.1.1.253
DHCP LEASE  : 43089, 43191/21595/37792
MAC         : 00:50:79:66:68:01
LPORT       : 10010
RHOST:PORT  : 127.0.0.1:10011
MTU:        : 1500

PC2>
```

> Machine 3:
```
PC3> dhcp
DDORA IP 10.1.1.12/24 GW 10.1.1.1

PC3> show ip

NAME        : PC3[1]
IP/MASK     : 10.1.1.12/24
GATEWAY     : 10.1.1.1
DNS         :
DHCP SERVER : 10.1.1.253
DHCP LEASE  : 43152, 43200/21600/37800
MAC         : 00:50:79:66:68:00
LPORT       : 10012
RHOST:PORT  : 127.0.0.1:10013
MTU:        : 1500

PC3>
```

### 3.2. DHCP spoofing.

#### 3.2.1 Configuration du serveur

```bash
dnf install -y dnsmasq
```

> Contenu de /etc/dnsmasq.conf :

```
port=0
dhcp-range=10.1.1.210,10.1.1.250,255.255.255.0,12h
dhcp-authoritative
interface=enp0s3
```

```bash
systemctl enable --now dnsmasq
sudo dnf remove iptables
```

> Nouveau VPCS:

```
C3> dhcp
DDORA IP 10.1.1.245/24 GW 10.1.1.251

PC3> show ip

NAME        : PC3[1]
IP/MASK     : 10.1.1.245/24
GATEWAY     : 10.1.1.251
DNS         :
DHCP SERVER : 10.1.1.251
DHCP LEASE  : 43049, 43200/21600/37800
MAC         : 00:50:79:66:68:00
LPORT       : 10012
RHOST:PORT  : 127.0.0.1:10013
MTU:        : 1500
```

#### 3.2.2. Spoofing & Race.

> Machine 3:
```
PC3> dhcp
DORA IP 10.1.1.12/24 GW 10.1.1.1

PC3>
```
> Machine 4:
```
PC4> dhcp
DDORA IP 10.1.1.248/24 GW 10.1.1.251

PC4>
```
> Machine 5:
```
PC5> dhcp
DDORA IP 10.1.1.249/24 GW 10.1.1.251

PC5>
```

---
