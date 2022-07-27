# Reverse Engineering
[You can totally do this, don't get a virus!](#you-can-totally-do-this-dont-get-a-virus---2pts)<br>
[Say my name, say my name](#say-my-name-say-my-name---4pts)<br>
[System Check](#system-check---10pts)<br>
*(some challenges missing)*

## You can totally do this, don't get a virus! - 2pts
> What is the name of the file that belongs to this SHA256 hash?
> 2ba412368d12490756b42153ca8564b761c2b451901f2bcf0778c77a8c00c229
> 
> Author: WSC

Given a SHA256 hash, something you can't reverse unless you brute force it is a bit tricky to use. Luckily the internet exists, and with it, massive databases of hashes. Looking online, VirusTotal offers a search by hash feature:

![VirusTotal](https://i.imgur.com/sjQ0lhP.png)

Searching the hash gives us the following:

![VirusTotal](https://i.imgur.com/vUzYX0V.png)

And we have our file name:
```
wsc_2022.exe
```

## Say my name, say my name - 4pts
> What is the name of the file that belongs to this hash:
> 99135e5e2d1038f63f7a60ea9623904d546620b5
> 
> Author: WSC

Looks like another hash, but it looks like a SHA-1 hash. Let's check:

```
┌──(kali㉿kali)-[~]
└─$ hash-identifier 99135e5e2d1038f63f7a60ea9623904d546620b5
   #########################################################################
   #     __  __                     __           ______    _____           #
   #    /\ \/\ \                   /\ \         /\__  _\  /\  _ `\         #
   #    \ \ \_\ \     __      ____ \ \ \___     \/_/\ \/  \ \ \/\ \        #
   #     \ \  _  \  /'__`\   / ,__\ \ \  _ `\      \ \ \   \ \ \ \ \       #
   #      \ \ \ \ \/\ \_\ \_/\__, `\ \ \ \ \ \      \_\ \__ \ \ \_\ \      #
   #       \ \_\ \_\ \___ \_\/\____/  \ \_\ \_\     /\_____\ \ \____/      #
   #        \/_/\/_/\/__/\/_/\/___/    \/_/\/_/     \/_____/  \/___/  v1.2 #
   #                                                             By Zion3R #
   #                                                    www.Blackploit.com #
   #                                                   Root@Blackploit.com #
   #########################################################################
--------------------------------------------------

Possible Hashs:
[+] SHA-1
[+] MySQL5 - SHA-1(SHA-1($pass))

Least Possible Hashs:
...
```

Just like the past challenge `You can totally do this, don't get a virus!`, we'll use VirusTotal's search feature to find the file name.

![VirusTotal](https://i.imgur.com/W1Gu0IQ.png)

And just like that we have the file name, great!

![VirusTotal](https://i.imgur.com/FqzIdB4.png)

We can submit the file name:
```
nojk5154blj.xls
```

## System Check - 10pts
> Is your system ready?
>
> Created by US Cyber Range
>
> [systemCheck]()

So downloading the file we can see a bit of info:
```
┌──(kali㉿kali)-[~]
└─$ file systemCheck 
systemCheck: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=777267c8fc4bcb4ef228a23a31225f750d3e09ed, for GNU/Linux 3.2.0, not stripped
```

We can tell that first off that its an ELF file and is not stripped, meaning that the function names are not obfuscated.

Lets import this into Ghidra and take a look at the main function:

Here's what the main function looks like:

```c
undefined8 main(void) {
  char cVar1;
  int iVar2;
  int iVar3;
  char *pcVar4;
  FILE *pFVar5;
  long in_FS_OFFSET;
  char local_1018 [4104];
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  cVar1 = '\0';
  puts("Pass the checks and win!");
  malloc(10);
  pcVar4 = getlogin();
  iVar2 = strcmp(pcVar4,"check1");
  if (iVar2 == 0) {
    puts("Check 1 passed");
  }
  else {
    puts("Check 1 failed");
  }
  pcVar4 = getcwd(local_1018,0x1000);
  if (pcVar4 == (char *)0x0) {
    puts("Check 2 failed");
  }
  else {
    iVar3 = strcmp(local_1018,"/etc/check2");
    if (iVar3 == 0) {
      puts("Check 2 passed");
      cVar1 = '\x01';
    }
    else {
      puts("Check 2 failed");
    }
  }
  pFVar5 = fopen("./check3","r");
  if (pFVar5 != (FILE *)0x0) {
    puts("Check 3 passed");
  }
  else {
    puts("Check 3 failed");
  }
  if ((char)((pFVar5 != (FILE *)0x0) + (iVar2 == 0) + cVar1) == '\x03') {
    puts("System checks passed.");
    accessGranted();
  }
  else {
    puts("System checks failed. Exiting.");
  }
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return 0;
}
```

Hm, looks like there's a `accessGranted()` function that prints the flag once all checks are done. But looking more into it, it seems like you can't get the flag string as its obfuscated through a ton of different calls. A simple solution, our friend gdb.

We can launch gdb attached to the binary:

```
┌──(kali㉿kali)-[~]
└─$ gdb systemCheck
GNU gdb (Debian 10.1-2) 10.1.90.20210103-git

Reading symbols from systemCheck...
```

Lets run it so we can call functions:

```
(gdb) run
Starting program: /home/kali/systemCheck 
Pass the checks and win!

Program received signal SIGSEGV, Segmentation fault.
__strcmp_avx2 () at ../sysdeps/x86_64/multiarch/strcmp-avx2.S:101
```

Then let's call the function we saw earlier:

```
(gdb) print (void) accessGranted()
Access Granted!

flag:{@ll_Sist3ms_Go,Go,Go!}

$1 = 0x0
```

We can then submit our flag:

```
flag:{@ll_Sist3ms_Go,Go,Go!}
```