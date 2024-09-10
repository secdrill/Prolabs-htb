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

#### UP Host
```bash
10.10.110.2     # Firewall Out of Scope
10.10.110.100
```

### Port Scanning

**10.10.110.100**<br>
```bash
PORT      STATE SERVICE REASON
21/tcp    open  ftp     syn-ack ttl 62
22/tcp    open  ssh     syn-ack ttl 62
65000/tcp open  unknown syn-ack ttl 62
```

#### 21 Port
```bash
$ ncat 10.10.110.100 21
220 (vsFTPd 3.0.3)
```

#### 22 Port
```bash
$ ncat 10.10.110.100 22
SSH-2.0-OpenSSH_8.2p1 Ubuntu-4
```

#### 65000 Port
```bash
$ ncat 10.10.110.100 65000
 
HTTP/1.1 400 Bad Request
Date: Mon, 09 Sep 2024 16:39:05 GMT
Server: Apache/2.4.41 (Ubuntu)
Content-Length: 304
Connection: close
Content-Type: text/html; charset=iso-8859-1
```
![APache Default Page](/img/Apache_default_port6500.png)


## **Basic Information gathered**

**OS** - Ubuntu *Maybe Focal Fossa*
* 21 vsFTPd
* 22 SSH openssh 8.1_p1
* 65000 Apache 2.4.41

![OS Version based on SSH](/img/OSVersion_based_on_sshversion.png)

## Service Enumeration

### SSH Enumeration
```bash
$ sudo nmap -vv -sV -A -p21,22,65000  -oN host_100.txt 10.10.110.100

22/tcp    open  ssh     syn-ack ttl 62 OpenSSH 8.2p1 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 8f:a2:ff:cf:4e:3e:aa:2b:c2:6f:f4:5a:2a:d9:e9:da (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCtTLxLag6I25W/4MyXLNSNylWF6JL7BB9D/wK7yPZkTK0PX62N52x788lVBYZjuBvqN2wobnG5HMZvaneZaezpyi/bLGhdnERknUixrO6efcXebZFgJx5LyHENJpP5XxBpUdrczuM3/zBY1mpeBDWTMrJQLK31Sh/RxCNOlayM/DewYZmP8KCGnB0OR/BlR3dvtBOBdbuJQn+xoL6jbPjSQzTEFO/si2OwiIb0lW+PxC8RLIXulKav9k8wIFTZOqCICfnIGGIOg1LaUUtp/qt0csEQMDnCiTdgzFyi7m9yY6t8hZGCXMR8Z9RmbH8VuPbO8mRfMIxMda+rXmE8u0KUV2YW/ICeGNzle65o01YXzI4z/yzsj0HdANxMpzyYlSbNgIEo5yyGsnNHWBun3Vd5Px4QPwy//4X3od5tfi6W6XKHxK/ZFeT8nbGyoV47ozLxOFXYeTQ72RSYKENuFmn6VLyMH/C0JXFiwV5FNFqvJgmpEM9ba/3bDznTG0QUm48=
|   256 07:83:8e:b6:f7:e6:72:e9:65:db:42:fd:ed:d6:93:ee (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBIVJJ8GPg11pc5bNU14qHtur8E0nGBUzMRB+9M+jdVF/l6+zNeA9aKzsCs/tT/46e7Qb9xhfSyRpSNDa/I49FOc=
|   256 13:45:c5:ca:db:a6:b4:ae:9c:09:7d:21:cd:9d:74:f4 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIAXEXWWafJIXJTRj8o05r1Ia4C++zzVfM7t+8MzY1cMj
```

### FTP Enumeration

1. Using Nmap
```bash
$ sudo nmap -sV -p21 -sC -A 10.10.110.100
21/tcp open  ftp     vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.14.9
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: PASV IP 172.16.1.100 is not the same as 10.10.110.100
```
2. Trying to connect ftp using anonymous:anonymous (Username:Password)
 
```bash
$ ftp 10.10.110.100
Connected to 10.10.110.100.
220 (vsFTPd 3.0.3)
Name (10.10.110.100:user): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> 
```

FTP is in *Passive Mode* we have to off passive mode first
```bash
ftp> passive
Passive mode: off; fallback to active mode: off.
```
+ found `/Transfer/Outgoing/todo.txt` file
```bash
ftp> more todo.txt
- Finalize Wordpress permission changes - PENDING
- Update links to to utilize DNS Name prior to changing to port 80 - PENDING
- Remove LFI vuln from the other site - PENDING
- Reset James' password to something more secure - PENDING
- Harden the system prior to the Junior Pen Tester assessment - IN PROGRESS
 ```
**Download all FTP file**
```bash
wget -m --no-passive ftp://anonymous:anonymous@10.10.110.100
```
***No more information can be extracted from vsftpd, No public exploit available for this version***

### HTTP Enumeration
```bash
$ sudo nmap -vv -sV -A -p21,22,65000  -oN host_100.txt 10.10.110.100

65000/tcp open  http    syn-ack ttl 62 Apache httpd 2.4.41 ((Ubuntu))
| http-methods: 
|_  Supported Methods: OPTIONS HEAD GET POST
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
| http-robots.txt: 2 disallowed entries 
|_/wordpress DANTE{Y0u_Cant_G3t_at_m3_br0!}
```

> FIRST FLAG `DANTE{Y0u_Cant_G3t_at_m3_br0!}`

/wordpress - Website found

![Possible Wordpress version](/img/Wordpress_version.png)

**Directory Discovery using gobuster**
```bash
/.php                 (Status: 403) [Size: 281]
/.html                (Status: 403) [Size: 281]
/index.php            (Status: 301) [Size: 0] [--> http://10.10.110.100:65000/wordpress/]
/wp-content           (Status: 301) [Size: 338] [--> http://10.10.110.100:65000/wordpress/wp-content/]                                                                                      /wp-login.php         (Status: 200) [Size: 5090]
/license.txt          (Status: 200) [Size: 19915]
/wp-includes          (Status: 301) [Size: 339] [--> http://10.10.110.100:65000/wordpress/wp-includes/]
/readme.html          (Status: 200) [Size: 7278]
/robots.txt           (Status: 200) [Size: 34]
/wp-trackback.php     (Status: 200) [Size: 135]
/wp-admin             (Status: 301) [Size: 336] [--> http://10.10.110.100:65000/wordpress/wp-admin/]
/xmlrpc.php           (Status: 405) [Size: 42]

```
