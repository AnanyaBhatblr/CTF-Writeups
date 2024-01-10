- On examining the disassembled main code, we see that num is at rbp-0x8 and the string is stored at rbp-0x20,
- rbp-0x8 indicates that num is at an offset of 8 bytes from rbp
- rbp-0x20 indicates that the string is at an offset of 32 bytes from rbp(hex to dec)
- So the number of bytes between num and the string is 32-8=24
- So in order to cause buffer overflow to overwrite num, the 25th character of the string must be ‘A’ (as it’s ascii value is 65)
- On an entering a 25 letter long string with last letter A we get the flag

```bash
$ nc saturn.picoctf.net [REDACTED]
Enter a string: 123456789012345678901234A

num is 65
You win!
picoCTF{l0c4l5_1n_5c0p3_7bd3fee1}
```

**FLAG - `picoCTF{l0c4l5_1n_5c0p3_7bd3fee1}`**

