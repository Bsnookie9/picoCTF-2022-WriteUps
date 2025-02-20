# Overview
`Medium` `Reverse Engineering` `picoCTF 2022` `binary` `obfuscation`

# Description
Can you get the flag?   
Reverse engineer this [binary](https://artifacts.picoctf.net/c/46/bbbbloat).

# Hints
(None)

# Solution
First download the binary file using the `wget` command:

<kbd>![image](https://github.com/user-attachments/assets/8326469d-6053-4f6c-bd6f-8bc7ce6715fe)</kbd>

> **OPTIONAL** --- I elected to move this file into a different directory where I store other CTF challenges

Next we can use Ghidra to analyze the file

>[!IMPORTANT]
> _**Ghidra**_ is a free, open-source reverse engineering software tool that helps users analyze compiled software

```
┌──(thor㉿kali)-[~/CTF/bbbbloat/bbbbloat]
└─$ /opt/ghidra/ghidraRun
Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true
Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true
```
This will open the Ghidra GUI and allow you to create a new project and import the file:

<kbd>![Screenshot](https://github.com/user-attachments/assets/669b47fa-f765-48f7-90de-373d8cfd6a23)</kbd>

You'll then get to this page below. Click `Yes` and then `Analyze`:

<kbd>![Screenshot](https://github.com/user-attachments/assets/33090ef0-52f1-4ef9-8df8-c313f3e957be)</kbd>

Looking through the functions, we can see the following in function `FUN_00101307`:

```C
undefined8 FUN_00101307(void)

{
  long in_FS_OFFSET;
  int local_48;
  undefined4 local_44;
  char *local_40;
  undefined8 local_38;
  undefined8 local_30;
  undefined8 local_28;
  undefined8 local_20;
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  local_38 = 0x4c75257240343a41;
  local_30 = 0x3062396630664634;
  local_28 = 0x68653066635f3d33;
  local_20 = 0x4e623665625f64;
  local_44 = 0xd2c49;
  printf("What\'s my favorite number? ");
  local_44 = 0xd2c49;
  __isoc99_scanf(&DAT_00102020,&local_48);
  local_44 = 0xd2c49;
  if (local_48 == 0x86187) {
    local_44 = 0xd2c49;
    local_40 = (char *)FUN_00101249(0,&local_38);
    fputs(local_40,stdout);
    putchar(10);
    free(local_40);
  }
  else {
    puts("Sorry, that\'s not it!");
  }
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();

```

The user is presented with a question `What's my favorite number?`. Then it takes the user input and places it in a if statement: 

```C
  if (local_48 == 0x86187) {
    local_44 = 0xd2c49;
    local_40 = (char *)FUN_00101249(0,&local_38);
    fputs(local_40,stdout);
    putchar(10);
    free(local_40);
  }
  else {
    puts("Sorry, that\'s not it!");
  }
```

If the user input equals the value `0x86187`, the flag will be displayed. If not, the user sees `Sorry, that's not it!`. 

We can run python and place in `0x86187` to get the value:

```
┌──(thor㉿kali)-[~/CTF/bbbbloat]
└─$ python3              
Python 3.11.8 (main, Feb  7 2024, 21:52:08) [GCC 13.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 0x86187
549255
```

Before we can run the bbbbloat file, we have to change the file permissions:
```
┌──(thor㉿kali)-[~/CTF/bbbbloat]
└─$ chmod +x bbbbloat
```

Now let's run bbbbloat and enter that value as the number:

```
┌──(thor㉿kali)-[~/CTF/bbbbloat]
└─$ ./bbbbloat            
What's my favorite number? 549255
picoCTF{cu7_7h3_bl047_695036e3}
```

# Flag
`picoCTF{cu7_7h3_bl047_695036e3}`
