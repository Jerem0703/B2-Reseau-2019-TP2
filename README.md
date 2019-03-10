# B2-Reseau-2019-TP2

## I. Mise en place du lab

 ### 2/Routage Statique : 
 

  *Expliquer pourquoi on ne peut pas ping server1 depuis router1*
  ##### Car Server 1 ne connaît pas la route vers net12




  ### 3/Visualisation du routage avec Wireshark :
   
   *Expliquer la différence entre les captures whireshark*
    
   #### Sur le pcap de l'interface dans net12, on remarque une requête TCP en plus


## II. NAT et services d'infra

 ### 1. Mise en place du NAT :


``` 
[root@server1 ~]# curl google.com
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML> 
```


### 2. DHCP server

```
[root@dhcp-server ~]# systemctl start dhcpd
```

```
[root@client2 ~]# ip r s
default via 10.2.1.254 dev enp0s8 proto dhcp metric 100
10.2.1.0/24 dev enp0s8 proto kernel scope link src 10.2.1.50 metric 100
```


 ### 3. NTP server

```
[root@server1 ~]# chronyc sources
210 Number of sources = 4
MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================================
^- ntp-3.arkena.net              2   6    17     0  -1926us[-1926us] +/-   53ms
^- orfeo.duckcorp.org            2   6     7     2  +1482ns[ -333ms] +/-   61ms
^* ip139.ip-5-196-160.eu         2   6     7     1    +20us[ -333ms] +/-   19ms
^- shattrath.sceen.net           2   6    17     0   -724us[ -724us] +/-   63ms
[root@server1 ~]# chronyc traking
Unrecognized command
[root@server1 ~]# chronyc tracking
Reference ID    : 05C4A08B (ip139.ip-5-196-160.eu)
Stratum         : 3
Ref time (UTC)  : Sun Mar 10 16:01:23 2019
System time     : 0.000000446 seconds slow of NTP time
Last offset     : -0.000710880 seconds
RMS offset      : 0.000710880 seconds
Frequency       : 0.000 ppm slow
Residual freq   : -141.039 ppm
Skew            : 1000000.000 ppm
Root delay      : 0.035261650 seconds
Root dispersion : 12.886185646 seconds
Update interval : 1.9 seconds
Leap status     : Normal
```


 ### 4. Web server

```
[root@server1 ~]# systemctl status nginx
● nginx.service - The nginx HTTP and reverse proxy server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; vendor preset: disabled)
   Active: active (running) since Sun 2019-03-10 17:04:25 CET; 59s ago
  Process: 3530 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
  Process: 3527 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
  Process: 3526 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, status=0/SUCCESS)
 Main PID: 3532 (nginx)
   CGroup: /system.slice/nginx.service
           ├─3532 nginx: master process /usr/sbin/nginx
           └─3533 nginx: worker process

Mar 10 17:04:24 server1.net2.b2 systemd[1]: Starting The nginx HTTP and reverse proxy server...
Mar 10 17:04:25 server1.net2.b2 nginx[3527]: nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
Mar 10 17:04:25 server1.net2.b2 nginx[3527]: nginx: configuration file /etc/nginx/nginx.conf test is successful
Mar 10 17:04:25 server1.net2.b2 systemd[1]: Failed to read PID from file /run/nginx.pid: Invalid argument
Mar 10 17:04:25 server1.net2.b2 systemd[1]: Started The nginx HTTP and reverse proxy server.
```

