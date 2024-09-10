# Dante - Prolabs

**IP Range**
```
10.10.110.0/24
```

# Recon
### IP Range Scanning
**Using Nmap**
```bash
$ sudo nmap -sn 10.10.110.0/24
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-09-09 21:47 IST
Nmap scan report for 10.10.110.2
Host is up (0.16s latency).
Nmap scan report for 10.10.110.100
Host is up (0.16s latency).
Nmap done: 256 IP addresses (2 hosts up) scanned in 11.62 seconds
```

**Using fping**
```bash
$sudo fping -a -g 10.10.110.0 10.10.110.254
10.10.110.2
10.10.110.100
```

**Using masscan**
```bash
$ sudo masscan 10.10.110.0/24 --ping --interface tun0
Starting masscan 1.3.2 (http://bit.ly/14GZzcT) at 2024-09-09 16:30:12 GMT
Initiating ICMP Echo Scan
Scanning 256 hosts
Discovered open port 0/icmp on 10.10.110.2                                     
Discovered open port 0/icmp on 10.10.110.100    
```

#### up Host
```bash
10.10.110.2     # Firewall Out of Scope
10.10.110.100
```

### Port Scanning

**10.10.110.100**<br>
```
PORT      STATE SERVICE REASON
21/tcp    open  ftp     syn-ack ttl 62
22/tcp    open  ssh     syn-ack ttl 62
65000/tcp open  unknown syn-ack ttl 62

```