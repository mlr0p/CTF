Penguin
==================
**Category**: Rev  **Points**: 50

Challenge Description
------
We are given a binary
[penguin](penguin)

Solutions
----------

Just by running the binary, we get a prompt
```
GIB FLAG PLZ 
(./ezpz "WPI{some_flag_here}")

```
Tried with `./penguin WPI{some_flag_here}` and got `WRONG`

To further see what it's doing, I ltraced it

```
> ltrace ./penguin WPI{
strlen("WPI{")                                                               = 4
strncmp("WPI{", "WPI{", 4)                                                   = 0
strlen("strings_")                                                           = 8
strncmp("strings_", "", 8)                                                   = 115
puts("WRONG"WRONG
)                                                                = 6
+++ exited (status 0) +++
> ltrace ./penguin WPI{strings_
strlen("WPI{")                                                               = 4
strncmp("WPI{", "WPI{strings_", 4)                                           = 0
strlen("strings_")                                                           = 8
strncmp("strings_", "strings_", 8)                                           = 0
strlen("only_")                                                              = 5
strncmp("only_", "", 5)                                                      = 111
puts("WRONG"WRONG
)                                                                = 6
+++ exited (status 0) +++

```
Repeating this process we can get the final flag

`flag: WPI{strings_only_gives_you_wings,_you_still_have_to_learn_how_to_fly._try_hexedit}`
