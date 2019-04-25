Half and Half
========
**Category**: crypto  **Points**: 50

Challenge Description
------------
We were given the following code
```
from secret import flag

def xor(x, y):
	o = ''
	for i in range(len(x)):
		o += chr(ord(x[i])^ord(y[i]))
	return o

assert len(flag) % 2 == 0

half = len(flag)//2
milk = flag[:half]
cream = flag[half:]

assert xor(milk, cream) == '\x15\x02\x07\x12\x1e\x100\x01\t\n\x01"'

```
It cuts the flag in half and xor'd the two halves together.

Solution
-----------------

We know the flag starts with `actf{`, thus xor'ing back with the encrypted flag, we could get 5 characters in the flag

```
flag_1half = 'actf{'
flag_2half = ''

flag_enc = '\x15\x02\x07\x12\x1e\x100\x01\t\n\x01"'

for i in range(len(flag_1half)):
    flag_2half += chr(ord(flag_1half[i]) ^ ord(flag_enc[i]))
print flag_2half

```
We also know the flag ends with `}`

Therefore we know the flag looks like `actf{??????_taste??????}`

Next I had to take a guess from the wordings `milk` and `cream` in the source code,

I guessed cookie and got pretty close `actf{cookie_tastes_nbcd}`

Now I just replaced the last words with my first guess `good` and got the flag.

flag: `actf{coffee_tastes_good}`