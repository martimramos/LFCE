### Linux Foundation Certified Systems Engineer (LFCE) Notes / Cheat Sheet

---

### **Conventions:**
```bash
[root@host ~]# This_is_a_command
$This_is_a_var
# This is a comment
[root@host ~]# iptables --flush  # This is a comment after a command
(ex: this_is_an_example)
```

---

## **Sysvinit**
```bash
[root@host ~]# chkconfig $service_name on|off
[root@host ~]# chkconfig $service_name start|status|stop
```
*Some services depend on `xinetd`.*

---

## **SystemD**
```bash
[root@host ~]# systemctl stop|status|start|enable|disable $service_name.service
[root@host ~]# systemctl list-unit-files  # List services
[root@host ~]# systemctl enable vncserver@1
```

---

## **Network Performance**
*Netstat replaced with `ss`.*
```bash
[root@host ~]# ss -s  # Summary stats (estab 1, closed 0, orphaned 0, synrecv 0, timewait 0/0)
[root@host ~]# ss -l  # All open ports on the system
[root@host ~]# ss -pl  # Processes open by socket
[root@host ~]# ss -4 state ESTABLISHED  # IPv4 traffic ESTABLISHED and LISTEN
[root@host ~]# ss -t -a  # All TCP sockets
[root@host ~]# ss -t -o  # All established
[root@host ~]# ss -tn sport = :22  # All connections on port 22
[root@host ~]# nmap -A localhost
[root@host ~]# nmap -A -sS localhost  # No remote or local trace
```

### **Network Stats**
```bash
[root@host ~]# iptraf
[root@host ~]# dstat
[root@host ~]# dstat 5 10  # Every 5 secs for 10 times
[root@host ~]# dstat -n  # Only network stats
```

---

## **Packet Filtering**
```bash
[root@host ~]# iptables -L
[root@host ~]# iptables --flush  # Flush iptables rules
[root@host ~]# iptables -A INPUT --protocol icmp --interface $interface_name -j DROP
```
*`REJECT` -> Clients get errors, confirming host existence/connection.*
*`DROP` -> Clients receive no confirmation or error, no indication of host existence.*

---

## **Reports on System Use, Outages, and User Requests**

### **CPU & RAM**
```bash
[root@host ~]# top
[root@host ~]# htop
[root@host ~]# free -m
[root@host ~]# w  # Load average
[root@host ~]# uptime  # Load average and more info
```

### **Filesystem**
```bash
[root@host ~]# df -h  # Space (human-readable)
[root@host ~]# df -hTi  # Inodes
[root@host ~]# du -hsc  # Disk use by directory + summary
```

### **Processes**
```bash
[root@host ~]# ps aux
[root@host ~]# ps -ef
[root@host ~]# ps ef
```

### **Logs**
```bash
[root@host ~]# dmesg  # Drivers, devices, boot, hardware, etc.
[root@host ~]# tail -f /var/log/messages
[root@host ~]# tail -f /var/log/*
```

---

## **Route IP Traffic Dynamically (Dynamic Routing)**
```bash
[root@host ~]# yum install quagga
[root@host ~]# vim /etc/quagga/daemons
# Change values:
zebra=1
ripd=1
[root@host ~]# service quagga restart
[root@host ~]# telnet localhost 2601
> enable
> configure terminal
> router rip
> version 2
> network 192.168.1.0/24
> exit
> write
```

---

## **Network Filesystems and File Services**
### **Standard Filesystems**
```bash
[root@host ~]# fdisk -l
[root@host ~]# mkfs -t ext4 /dev/$partition
[root@host ~]# mount -t ext4 /dev/$partition /chosen/path/to/mount
```

### **Encrypted Filesystems**
```bash
[root@host ~]# cryptsetup -y luksFormat /$drive$partition
[root@host ~]# cryptsetup luksOpen /dev/$my_encrypted_partition $myEncryptedDriveTag
[root@host ~]# mkfs -t ext4 /dev/mapper/$myEncryptedDriveTag
[root@host ~]# mount /dev/mapper/$myEncryptedDriveTag /mnt/somewhere
```

### **Network Filesystem (NFS)**
```bash
[root@NFS_SERVER ~]# cat /etc/exports
/path $_CLIENT_IP_ADDR (rw, no_root_squash)
[root@host ~]# mount -t nfs NFS_SERVER:/path /local_path
```

---

## **Configure the Firewall with IPTables**
```bash
[root@host ~]# iptables -L
[root@host ~]# iptables -F INPUT  # Flush input rules
[root@host ~]# iptables --flush  # Flush all rules
[root@host ~]# iptables -A INPUT --protocol icmp --in-interface enp0s3 -j REJECT
```

---

## **Install and Configure SSL with Apache**
```bash
[root@host ~]# yum install mod_ssl
[root@host ~]# mkdir -p /etc/httpd/certs
[root@host ~]# openssl req -X509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/httpd/certs/httpd.key -out /etc/httpd/certs/httpd.crt
```

### **Apache Virtual Hosts**
```bash
<VirtualHost *:80>
    ServerName fqdn.url.conf
    DocumentRoot /var/yourPath/www
    Alias /youralias /var/yourPath/www
    ErrorLog /gfg/fgfg/sds.log
    CustomLog /path/path combined
</VirtualHost>
```

---

## **Email Services**
```bash
[root@host ~]# yum install postfix
[root@host ~]# systemctl enable postfix
[root@host ~]# vim /etc/postfix/aliases
[root@host ~]# postalias /etc/postfix/aliases
[root@host ~]# systemctl restart postfix
```

---

## **If this was useful to you, consider donating a Satoshi!**

**BTC:** `1CUPMEpQtsEXLuudiEoRXpYNbCsh9A5Vvg`
**ETH:** `0xedFd8dE06aAB43252864F1d9dA6fdd34C562a117`
**LTC:** `La4MiQGGA9PYTadTbczuuRfyW9jt8LZTE4`

---

