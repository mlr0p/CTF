spi - easy
===========
**Category**: misc  **Points**: 70

Challenge Description
------
```
I used to be a hero. Now I can't even handle this: Mitschnitt
```
We were given a sound track and the audio of the sound track is a message of a bunch of numbers.

Solution
--------

I made up my mind to write down every number in the message

```
76 83 48 116 76 105 52 103 76 83 48 116 76 83 52 103 76 105 48 117 76 83 65 116 
76 83 48 116 76 105 65 116 76 105 48 117 76 83 52 103 76 83 48 116 76 83 48 103 
76 83 52 117 76 83 65 117 76 83 52 117 73 67 48 117 76 83 52 116 76 105 65 116 
76 83 48 117 73 67 48 116 76 83 48 116 73 67 48 117 76 83 52 116 76 105 65 116 
76 83 48 116 76 83 65 116 76 105 52 116 73 67 52 116 76 105 52 78 67 103 61 61
```
Bunch of numbers, but it looks like ASCII text.

Decoding it to ASCII: 

```
LS0tLi4gLS0tLS4gLi0uLSAtLS0tLiAtLi0uLS4gLS0tLS0gLS4uLSAuLS4uIC0uLS4tLiAtLS0uIC0tLS0tIC0uLS4tLiAtLS0tLSAtLi4tIC4tLi4NCg==
```

Nice! Base64! Decoding it: 

```
---.. ----. .-.- ----. -.-.-. ----- -..- .-.. -.-.-. ---. ----- -.-.-. ----- -..- .-..
```

Nice! Morse Code! Decoding it: 

```
89 9;0XL;!0;0XL
```

Umm...this is not right. Fortunately I used the website http://rumkin.com/tools/cipher/morse.php and there is the option
to swap - and . After swapping, the cipher decodes to :

```
34C4.5PY.V5.5PY
```

Flag: 34C4.5PY.V5.5PY
