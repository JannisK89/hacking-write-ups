# Boiler CTF

Intermediate level CTF. Just enumerate, you'll get there.

## Enumeration

### Nmap

Initial Nmap scan found the following:

```
PORT STATE SERVICE REASON VERSION
21/tcp open ftp syn-ack ttl 63 vsftpd 3.0.3
80/tcp open http syn-ack ttl 63 Apache httpd 2.4.18 ((Ubuntu))
10000/tcp open http syn-ack ttl 63 MiniServ 1.930 (Webmin httpd)
55007/tcp open ssh syn-ack ttl 63 OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

While running a fuff directory scan on the web server I checked out the other
ports:

### FTP

We gained access to the FTP server using an anynomous login.
We found a file called .info.txt which contained the following:

```
Whfg jnagrq gb frr vs lbh svaq vg. Yby. Erzrzore: Rahzrengvba vf gur xrl!
```

Which seems like a encoded message and using dencode I found out that is
was encoded using ROT13 and the message was:

```
Just wanted to see if you find it. Lol. Remember: Enumeration is the key!`
```

lol guess we were trolled.

### Webmin

I was not familiar with Webmin but reading the documentation I found out that
it is a web-based interface for system administration for Unix.

Opening the IP on port 10000 we accessed the webmin interface and we found a
login page. We tried the default credentials of root:root but they did not
work, darn. I'll keep looking around and return later in case of a hint
or to brute force the login.

### SSH

An SSH port but we currently don't have any credentials to login.

### Fuff

With fuff we managed to find a robots.txt file which contained the following:

```
User-agent: *
Disallow: /

/tmp
/.ssh
/yellow
/not
/a+rabbit
/hole
/or
/is
/it

079 084 108 105 077 068 089 050 077 071 078 107 079 084 086 104 090 071 086 104 077 122 073 051 089 122 085 048 077 084 103 121 089 109 070 104 078 084 069 049 079 068 081 075
```

None of the directories seemed to be accessible but the last line seemed like
an encoded message. Using the numbers with their corresponding ASCII values we
get:

```
OTliMDY2MGNgOTVhZGVhMzIYzU0MTgYmFhNTQzYmFhMzIzYzUwYmMzYmY0NTY
```

Which gives us a base64 encoded message.
Decoding it we get:

```
99b0660cd95adea327c54182baa51584
```

Which seemed like a hash and using a rainbow table we found the word

```
kidding
```

lol damn we propably got trolled again.

The message for this room said to keep enumerating so thats what I will do...
And after quite a while of fuzzing I finally found:

```
[Status: 301, Size: 322, Words: 20, Lines: 10, Duration: 48ms]
    * FIRST: joomla
    * SECOND: images
```

Now /joomla/images only gave us a blank page but now we have something to go by.
Having never heard of Joomla before I did some googling and found out that it
is a content management system (CMS) that allows you to build websites. So we
keep enumerating using joomla

