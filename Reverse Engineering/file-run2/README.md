# Overview
`Medium` `Reverse Engineering` `picoCTF 2022`

# Description
Another program, but this time, it seems to want some input.   
What happens if you try to run it on the command line with input "Hello!"?  
Download the program [here](https://artifacts.picoctf.net/c/155/run).

# Hints
- Try running it and add the phrase "Hello!" with a space in front (i.e. "./run Hello!")

# Solution
This challenge is also very straightforward. First download the file using the `wget` command:

<kbd>![Screenshot](https://github.com/user-attachments/assets/31d8bf08-6339-418b-a828-7a6f1f167dc4)</kbd>

Next we can use the `ls -l` command to see file permissions:

```console
bsnookie9-picoctf@webshell:~$ ls -l run
-rw-rw-r-- 1 bsnookie9-picoctf bsnookie9-picoctf 16816 Mar 16  2023 run
```

We see there are only read and write permission. Using `chmod +x` adds execution permissions to the file

Using `ls -l` again we can see that the permission have been updated:

```console
bsnookie9-picoctf@webshell:~$ ls -l run
-rwxrwxr-x 1 bsnookie9-picoctf bsnookie9-picoctf 16816 Mar 16  2023 run
```

Executing the run file using the `./run` command like before doesn't work in this challenge. 

> [!IMPORTANT]
> Using `.` before the `/` indicates running in the current directory, without specifying a full path to that file

```console
bsnookie9-picoctf@webshell:~$ ./run
Run this file with only one argument
```

Instead we need to pass an argument into the run command. Let's try `./run "Hello!"`  
Here we are presented with the flag:

```console
bsnookie9-picoctf@webshell:~$ ./run "Hello!"
The flag is: picoCTF{F1r57_4rgum3n7_be0714da}
```

# Flag
`picoCTF{F1r57_4rgum3n7_be0714da}`
