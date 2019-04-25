Just Letters
========
**Category**: misc  **Points**: 60

Challenge Description
------------
We were pointed to the esolang AlphaBeta (https://esolangs.org/wiki/AlphaBeta)

When we connec to the server, we were presented with 

```
Welcome to the AlphaBeta interpreter! The flag is at the start of memory. You get one line:
>

```

Our goal is to read the flag from the start of the memory.

Solution
-----------------

There is a link to an implementation at the end of the page https://host-a.net/f/56668-abcpp

First we can see the variable declaraed at the top

```
string Result;
int Register1 = 0;
int Register2 = 0;
int Register3 = 0;
int Register4[] = { 0, 0 };
int Mode = 0;
int Memory[1024];
int Temp;
```

We can see the registers were implemented as an int and the Memory is an array.

We can also see the particular line `if ( Program[Position] == "G" ) { Register1 = Memory[Register4[0]] ;}`

This means with the command `G` we can read from the memory with Register4 into Register 1

Here are other helpful commands

```
if ( Program[Position] == "C" ) { Register3 = Register1 ;}
if ( Program[Position] == "L" ) { cout << char(Register3) ;}
if ( Program[Position] == "S" ) { Register4[Mode] += 1 ;}

```

So our steps would be
1. Read memory address at position Register4
2. Store Register1 in Register3
3. Print Register3
4. Increment Register4

Execute this a couple of times and we get the flag.

flag: `actf{esolangs_sure_are_fun!}`