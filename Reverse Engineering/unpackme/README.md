# Overview
`Medium` `Reverse Engineering` `picoCTF 2022` `binary` `packed`

# Description
Can you get the flag?  
Reverse engineer this [binary](https://artifacts.picoctf.net/c/203/unpackme-upx).

# Hints
What is UPX?

# Solution
First download the python file using the `wget command`:

<kbd>![Screenshot](https://github.com/user-attachments/assets/44963fee-f123-4c7a-86ba-6c63e41b4226)</kbd>

Next we can use the hint that asks 'What is UPX?' 

>[!IMPORTANT]
> _**UPX**_ is a free, open-source tool that compresses executable files

Using upx we can use the `-d` flag which decompresses the binary:

```console
bsnookie9-picoctf@webshell:~$ upx -d unpackme-upx 
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2020
UPX 3.96        Markus Oberhumer, Laszlo Molnar & John Reiser   Jan 23rd 2020

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
   1002528 <-    379188   37.82%   linux/amd64   unpackme-upx

Unpacked 1 file.
```



# Flag
``
