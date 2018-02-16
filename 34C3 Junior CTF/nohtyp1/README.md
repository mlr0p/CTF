nohtyp1 - easy
========
**Category**: rev  **Points**: 70

Challenge Description
------
```
We love snakes.
```
We were given the following source codes
```python
____=input;__________________=print;___________=____();_________=map;__________=ord;_______________=zip;
____________________________=list;___=21;_____=lambda ______,_______:______+(_______^___);
______________={not not not ___ and not not ___:lambda:__________________('\x41\x6c\x6d\x6f\x73\x74\x21\x21'),
not not ___ and not not ___:lambda:__________________('\x43\x6f\x72\x72\x65\x63\x74\x21')};______________[[_____
(*________) for ________ in _______________(____________________________(_________(__________,___________)),
____________________________(_________(__________,___________))[::-1])][::-1]==[160,155,208,160,190,215,237,134,210,
126,212,222,224,238,128,240,164,213,183,192,162,178,163,162] and 'mo4r' in ___________ and '34C3_' in ___________ 
and ___________.split('_')[3] == 'tzzzz']()
```
Solutions
------------------
This does not look appealing, I converted it into something more readable.
```python
some_function=lambda six,seven:six+(seven^21)
some_list={not not not 21 and not not 21:lambda:print('Almost'),not not 21 and not not 21:lambda:print('Correct')}
some_list[[some_function(*x) for x in zip(list(map(ord,input())),list(map(ord,input()))[::-1])][::-1]==[160,155,208,160,190,215,237,134,210,126,212,222,224,238,128,240,164,213,183,192,162,178,163,162] and 'mo4r' in input() and '34C3_' in input() and input().split('_')[3] == 'tzzzz']()
print([y for y in [some_function(*x) for x in zip(list(map(ord,input())),list(map(ord,input()))[::-1])][::-1]])
```

Okay this program is basically taking into a stream of inputs, adding with the xored one indexed counting from backwards.

Ex. [1,2,3,4] the index three of the result would be 1 + (4^21), where 21 is a fixed value provided by the program

In addition, some texts are already given in the conditions
1. 34C3_ in the flag, and I assumed it's the first block of text
2. The fourth block is tzzzz
3. mo4r is in the text

Therefore, the plaintext would look something like `34C3_(sth)_(sth)_tzzzz`

I ran a small python program assuming the preceding texts are `34C3_mo4r`

```python
ciphers = [160,155,208,160,190,215,237,134,210,126,212,222,224,238,128,240,164,213,183,192,162,178,163,162]

text = '34C3_mo4r_'
last = ''
for i in range(len(text)):
  tmp = (ciphers[-i-1] - ord(text[i]))^21
  last = chr(tmp) + last
print(text+last)`

```
The ouput is actually `34C3_mo4r_4kes_tzzzz`, Yay 4 more characters to go

Now the third block of text looks something like `sn4kes`, therefore I tested with the same program
and the ouput was `34C3_mo4r_sn4kes_tzzzz`

Nice! Now we have two more characters to go. But I was stuck here for a while until I decided to brute force my way out.

```
for x12 in range(48, 128):
  for x13 in range(48, 128):
    if x12+(x13^21)==224 and x13+(x12^21)==222:
      print("34C3_mo4r_s"+chr(x12)+chr(x13)+"n4kes_tzzzz")
```

By piping the outputs to match the md5 provided in the hint, here goes our flag.


Flag: 34C3_mo4r_schn4kes_tzzzz
