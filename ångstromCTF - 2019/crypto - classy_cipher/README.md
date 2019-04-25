Classy Cipher
========
**Category**: crypto  **Points**: 20

Challenge Description
------------
We were given the following code
```
from secret import flag, shift

def encrypt(d, s):
	e = ''
	for c in d:
		e += chr((ord(c)+s) % 0xff)
	return e

assert encrypt(flag, shift) == ':<M?TLH8<A:KFBG@V'
```

It shifts each character in the flag and modulo with 255.

Solution
-----------------

We just need to reverse the operation

```
from secret import flag, shift

def encrypt(d, s):
	e = ''
	for c in d:
		e += chr((ord(c)+s) % 0xff)
	return e

assert encrypt(flag, shift) == ':<M?TLH8<A:KFBG@V'

```

flag: `actf{so_charming}`