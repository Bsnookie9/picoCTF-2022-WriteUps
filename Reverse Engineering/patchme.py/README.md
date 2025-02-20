# Overview
`Medium` `Reverse Engineering` `picoCTF 2022`

# Description
Can you get the flag?   
Run this [Python program](https://artifacts.picoctf.net/c/201/patchme.flag.py) in the same directory as this [encrypted flag](https://artifacts.picoctf.net/c/201/flag.txt.enc).

# Hints
(None)

# Solution
The first step is to download the Python script and the encrytped flag using the `wget` command:

<kbd>![Screenshot](https://github.com/user-attachments/assets/32f1ef12-2250-46a3-983e-57832abb3bdb)</kbd>

Next we need to make sure that both items are in the same directory. I created a directory called `patchme.py`

Once that is complete, we can first look at the Python script using `vi` and see what it is doing:

```python
def str_xor(secret, key):
    new_key = key
    i = 0
    while len(new_key) < len(secret):
        new_key = new_key + key[i]
        i = (i + 1) % len(key)
    return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c, new_key_c) in zip(secret, new_key)])

flag_enc = open('flag.txt.enc', 'rb').read()

def level_1_pw_check():
    user_pw = input("Please enter correct password for flag: ")
    if( user_pw == "ak98" + \
                   "-=90" + \
                   "adfjhgj321" + \
                   "sleuth9000"):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), "utilitarian")
        print(decryption)
        return
    print("That password is incorrect")

level_1_pw_check()
```

The Python script is designed to check if a user knows the correct password and, if so, decrypt the flag. The first function does not help us find the flag, as the comment mentions. The encrypted flag is read and then prompts the user to enter a password. If the entered password matches the predefined string `ak98-=90adfjhgj321sleuth9000`, the encrypted flag is decrypted using `str_xor` with the key `utilitarian` and prints the flag to the console. Otherwise, if the password entered does not match, the script prints out that the password is incorrect

Before running the script, we will need to change file permissions to allow execution:

```
┌──(thor㉿kali)-[~/CTF/patchme.py]
└─$ chmod +x patchme.flag.py
```

Now let's run the script using Python and enter the string `ak98-=90adfjhgj321sleuth9000`:

```
┌──(thor㉿kali)-[~/CTF/patchme.py]
└─$ python3 patchme.flag.py 
Please enter correct password for flag: ak98-=90adfjhgj321sleuth9000
Welcome back... your flag, user:
picoCTF{p47ch1ng_l1f3_h4ck_f01eabfa}
```

# Flag
`picoCTF{p47ch1ng_l1f3_h4ck_f01eabfa}`
