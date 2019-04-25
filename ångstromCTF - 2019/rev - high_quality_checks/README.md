High Quality Checks
========
**Category**: rev  **Points**: 110

Challenge Description
------------
A binary high_quality_checks were given.

Solution
-----------------

### Overview
Opening this in Ghidra, we can see the main functions

#### main() function: 

```
undefined8 main(void)

{
  int iVar1;
  size_t sVar2;
  long in_FS_OFFSET;
  char local_28 [24];
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  puts("Enter your input:");
  __isoc99_scanf(&DAT_00400b96,local_28);
  sVar2 = strlen(local_28);
  //check length >= 19
  if (sVar2 < 0x13) {
    puts("Flag is too short.");
  }
  else {
    iVar1 = check(local_28);
    if (iVar1 == 0) {
      puts("That\'s not the flag.");
    }
    else {
      puts("You found the flag!");
    }
  }
  if (local_10 == *(long *)(in_FS_OFFSET + 0x28)) {
    return 0;
  }
                    /* WARNING: Subroutine does not return */
  __stack_chk_fail();
}
```
#### check() function: 

```
undefined8 check(char *flag)

{
  int iVar1;
  //return (ulong)(*piParm1 == 0x30313763);
  iVar1 = d(flag + 0xc);

  //flag = 0x
  if ((((((iVar1 != 0) && (iVar1 = v((ulong)(uint)(int)*flag), iVar1 != 0)) &&
        (iVar1 = u((ulong)(uint)(int)flag[0x10],(ulong)(uint)(int)flag[0x11],
                   (ulong)(uint)(int)flag[0x11]), iVar1 != 0)) &&
       ((iVar1 = k((ulong)(uint)(int)flag[5]), iVar1 == 0 &&
        (iVar1 = k((ulong)(uint)(int)flag[9]), iVar1 == 0)))) &&
      ((iVar1 = w(flag + 1), iVar1 != 0 &&
       ((iVar1 = b(flag,0x12), iVar1 != 0 && (iVar1 = b(flag,4), iVar1 != 0)))))) &&
     ((iVar1 = z(flag,0x6c), iVar1 != 0 && (iVar1 = s(flag), iVar1 != 0)))) {
    return 1;
  }
  return 0;
}
```
#### Approach: 
So in order to recover the flag, we need to satisfy the check function. We know from main() that the flag has to be at least 19 characters, so we could assume the flag looks like

`current flag: actf{?????????????}`

#### Condition: d(flag + 0xc) == 0x30313763

When the flag pointer points to the 12th character, it has to be equal to 0x30313763, since it's little endian, we know flag[12] ~ flag[15] is `c710`

`current flag: actf{???????c710??}`

#### Condition: u(flag[16], flag[17], flag[17]) != 0

The u function takes two arguments (it is weird that three parameters are passed in), however, we can still reverse the function.

```
undefined8 u(char cParm1,char cParm2)

{
  int iVar1;
  
  iVar1 = right_shift_one(0xdc);
  if (((int)cParm1 == iVar1) && (iVar1 = o((ulong)(uint)(int)cParm2), iVar1 == 0x35053505)) {
    return 1;
  }
  return 0;
}
```

I have renamed the function n to right_shift_one because that's what it does. From here we can infer that

```
flag[16] = 0xdc >> 1 = 110 ('n')
o(flag[17]) = 0x35053505  # o is another function
=> 257c - (either 0x30 or 0x57) = 0x3505
-> flag[17] = 53 ('5')
```

`current flag: actf{???????c710??}`

#### Condition: k(flag[5]) != 0 && k(flag[9]) != 0

function k() bascially says that o(param) has to equal 0x660f660f, 
therefore we can use the above function o() logic to calculate flag[5], flag[9] = 102 ('f')

`current flag: actf{f???f??c710??}`

#### Condition: w(flag + 1) != 0

w[flag + 1] => flag[1] + flag[3] * 0x10000 + flag[2] * 0x100 = 0x667463

Since we've already known the flag format, we can just verify that we were right, it turns out we are.

`current flag: actf{f???f??c710??}`

#### Condition: b(flag, 0x12) != 0

b() function: 

```
ulong b(long lParm1,uint uParm2)

{
  char cVar1;
  int iVar2;
  int iVar3;
  
  cVar1 = *(char *)(lParm1 + (long)(int)uParm2);
  iVar2 = right_shift_one(0xf6);
  iVar3 = e((ulong)uParm2);
  return (ulong)((int)cVar1 == iVar3 * 2 + iVar2);
}
```

flag[18] = e(0x12) * 2 + 0x12 >> 1 = 125 ('}')

This is the same as previous, just verifying that we have the correct flag format

`current flag: actf{f???f??c710??}`

#### Condition: b(flag,4) != 0

Using the previous b function, we know flag[4] = 125 ('}')

Again, we already know this.

current flag: actf{f???f??c710??}

#### Condition: z(flag,0x6c) != 0

z() function is the longest:

```
#include <stdio.h>
#include <stdint.h>

typedef uint8_t byte;
int z(long flag,char char_l)
{
  char cVar1;
  int iVar2;
  char local_17;
  char local_16;
  unsigned int counter;
  
  local_17 = 0;
  local_16 = 0;
  counter = 0;
  while ((int)counter < 8) {
    cVar1 = (char)(((int)char_l & 1 << (byte)counter) >> (byte)counter);
    //printf("%c\n", cVar1);
    if ((counter & 1) == 0) {
      local_16 += (char)((int)cVar1 << ((byte)(counter >> 1) & 0x1f));
    }
    else {
      local_17 += (char)((int)cVar1 << ((byte)(counter >> 1) & 0x1f));
    }
    counter += 1;
  }
  printf("local_16: %d\n", local_16);
  printf("local_17: %d\n", local_17);
  if ((((*(char *)(flag + (long)local_17) == 'u') &&
       (cVar1 = *(char *)(flag + (long)local_17 + 1), iVar2 = (0xdc >> 1), (int)cVar1 == iVar2)) &&
      (cVar1 = *(char *)(flag + (long)local_16), iVar2 = (0xea >> 1), (int)cVar1 == iVar2)) &&
     (*(char *)(flag + (long)local_16 + 1) == 'n')) {
    return 1;
  }
  printf("check failed\n");
  return 0;
}

```

We can see that there are two variables: local_16 & local_17 generated after the while loop, which is used to determine the location of the characters in the flag.
upon executing this z function, we can see that local_16: 10, local_17: 6, from here we can determine some characters

flag[6] = 'u'

flag[7] = 0xdc >> 1 = 'n'

flag[10] = 0xea >> 1 = 'u'

flag[11] = 'n'


final flag: `actf{fun_func710n5}`