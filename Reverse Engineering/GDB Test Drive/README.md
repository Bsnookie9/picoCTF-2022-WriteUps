# Overview
`Medium` `Reverse Engineering` `picoCTF 2022` `binary` `gdb`

# Description
Can you get the flag?   
Download this [binary](https://artifacts.picoctf.net/c/87/gdbme).   
Here's the test drive instructions:

- `$ chmod +x gdbme`
- `$ gdb gdbme`
- `(gdb) layout asm`
- `(gdb) break *(main+99)`
- `(gdb) run`
- `(gdb) jump *(main+104)`

# Hints
(None)

# Solution
First step is to download the binary file using `wget`:

<kbd>![Screenshot](https://github.com/user-attachments/assets/2b289093-61a3-4ef8-8f9d-17912e1071c0)</kbd>

> **OPTIONAL** --- I elected to move this file into a different directory where I store other CTF challenges

Now we can run the following commands listed in the description:

```console
┌──(thor㉿kali)-[~/CTF/gdb_test_drive]
└─$ chmod +x gdbme
```
```console
┌──(thor㉿kali)-[~/CTF/gdb_test_drive]
└─$ gdb gdbme
GNU gdb (Debian 16.2-1) 16.2
Copyright (C) 2024 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from gdbme...
(No debugging symbols found in gdbme)
(gdb) layout asm
```

The `layout asm` command will open a window where you will enter the rest of the commands:

<kbd>![Screenshot](https://github.com/user-attachments/assets/dd771b35-c945-4189-a815-01a17af0c2d7)</kbd>

# Flag
`picoCTF{d3bugg3r_dr1v3_7776d758}`
