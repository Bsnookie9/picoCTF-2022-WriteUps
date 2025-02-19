# Overview
`Medium` `Reverse Engineering` `picoCTF 2022`

# Description
A program has been provided to you, what happens if you try to run it on the command line?  
Download the program [here](https://artifacts.picoctf.net/c/220/run).

# Hints
- To run the program at all, you must make it executable (i.e. `$ chmod +x run`)
- Try running it by adding a '.' in front of the path to the file (i.e. `$ ./run`)

# Solution
This challenge is very straightforward. First download the file using the `wget` command:

<kbd>![Screenshot](https://github.com/user-attachments/assets/dcb5ed33-6224-4efa-9ccf-831aef2c3747)</kbd>

Next we can use the `ls -l` command to see file permissions:

```console
bsnookie9-picoctf@webshell:~$ ls -l run
-rw-rw-r-- 1 bsnookie9-picoctf bsnookie9-picoctf 16736 Mar 16  2023 run
```

We see there are only read and write permission. Using `chmod +x` adds execution permissions to the file

Using `ls -l` again we can see that the permission have been updated:

```console
bsnookie9-picoctf@webshell:~$ ls -l run
-rwxrwxr-x 1 bsnookie9-picoctf bsnookie9-picoctf 16736 Mar 16  2023 run
```

Now we can execute the run file using the `./run` command. 

> [!IMPORTANT]
> Using `.` before the `/` indicates running in the current directory, without specifying a full path to that file

Here we are presented with the flag:

```console
bsnookie9-picoctf@webshell:~$ ./run
The flag is: picoCTF{U51N6_Y0Ur_F1r57_F113_47cf2b7b}
```

# Flag
`picoCTF{U51N6_Y0Ur_F1r57_F113_47cf2b7b}`
