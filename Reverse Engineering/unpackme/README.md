# Overview
`Medium` `Reverse Engineering` `picoCTF 2022` `binary` `packed`

# Description
Can you get the flag?  
Reverse engineer this [binary](https://artifacts.picoctf.net/c/203/unpackme-upx).

# Hints
What is UPX?

# Solution
First download the binary file using the `wget` command:

<kbd>![Screenshot](https://github.com/user-attachments/assets/bc2b61f8-9f13-4f42-a902-633602cd13d3)</kbd>

Next we can use the hint that asks 'What is UPX?' 

>[!IMPORTANT]
> _**UPX**_ is a free, open-source tool that compresses executable files

Using upx we can use the `-d` flag which decompresses the binary:

```
┌──thor㉿kali)-[~/CTF/reverse_engineering/unpackme]
└─$ upx -d unpackme-upx
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2024
UPX 4.2.2       Markus Oberhumer, Laszlo Molnar & John Reiser    Jan 3rd 2024

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
   1006445 <-    379188   37.68%   linux/amd64   unpackme-upx

Unpacked 1 file.
```
Next we can use Ghidra to analyze the file

>[!IMPORTANT]
> _**Ghidra**_ is a free, open-source reverse engineering software tool that helps users analyze compiled software 

```
┌──(thor㉿kali)-[~/CTF/reverse_engineering/unpackme]
└─$ /opt/ghidra/ghidraRun
Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true
Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true
```
This will open the Ghidra GUI and allow you to create a new project and import the file:

<kbd>![Screenshot](https://github.com/user-attachments/assets/2683b61f-163d-4f61-acc6-4156a22e3264)</kbd>

You'll then get to this page below. Click `Yes` and then `Analyze`:

<kbd>![Screenshot](https://github.com/user-attachments/assets/18e4a43f-66cc-4fba-97d3-c2de63df2912)</kbd>

Once the program finishes the analysis of the file, we want to find the main function. We can do so by typing in the filter box for `main`:

<kbd>![Screenshot](https://github.com/user-attachments/assets/2a77d93f-ae55-4175-8e4d-7b4aee87b132)</kbd>

Look off to the right to see the C code.  
The user is presented with a question `What's my favorite number?`. Then it takes the user input and places it in a if statement:

```c
  if (local_44 == 0xb83cb) {
    local_40 = (char *)rotate_encrypt(0,&local_38);
    fputs(local_40,(FILE *)stdout);
    putchar(10);
    free(local_40);
  }
  else {
    puts("Sorry, that\'s not it!");
  }
```
If the user input equals the value `0xb83cb`, the flag will be displayed. If not, the user sees `Sorry, that's not it!`. 

We can run python and place in `0xb83cb` to get the value:

```
┌──(thor㉿kali)-[~/CTF/reverse_engineering/unpackme]
└─$ python3
Python 3.11.8 (main, Feb  7 2024, 21:52:08) [GCC 13.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 0xb83cb
754635
```

Before we can run the unpackme-upx, we have to change the file permissions:
```
┌──(thor㉿kali)-[~/CTF/reverse_engineering/unpackme]
└─$ chmod +x unpackme-upx
```

Now let's run unpackme-upx and enter that value as the number:

```
┌──(thor㉿kali)-[~/CTF/reverse_engineering/unpackme]
└─$ ./unpackme-upx     
What's my favorite number? 754635
picoCTF{up><_m3_f7w_77ad107e}
```

# Flag
`picoCTF{up><_m3_f7w_77ad107e}`
