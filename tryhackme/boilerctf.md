# Boiler CTF

Intermediate level CTF

## Enumeration

Initial Nmap scan found the following:
PORT STATE SERVICE REASON VERSION
21/tcp open ftp syn-ack ttl 63 vsftpd 3.0.3
80/tcp open http syn-ack ttl 63 Apache httpd 2.4.18 ((Ubuntu))
10000/tcp open http syn-ack ttl 63 MiniServ 1.930 (Webmin httpd)
55007/tcp open ssh syn-ack ttl 63 OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

### FTP

We gained access to the FTP server using an anynomous login.
We found a file called .info.txt which contained the following:

```
Whfg jnagrq gb frr vs lbh svaq vg. Yby. Erzrzore: Rahzrengvba vf gur xrl!
```

Which seems like a encoded message and using dencode we found out that is
was encoded using ROT13 and the message was:

```
Just wanted to see if you find it. Lol. Remember: Enumeration is the key!`
```

lol guess we were trolled.

### Webmin
