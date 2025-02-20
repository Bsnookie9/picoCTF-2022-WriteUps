# Overview
`Medium` `Reverse Engineering` `picoCTF 2022` `obfuscation`

# Description
Can you get the flag?   
Run this [Python program](https://artifacts.picoctf.net/c/104/bloat.flag.py) in the same directory as this [encrypted flag](https://artifacts.picoctf.net/c/104/flag.txt.enc).

# Hints
(None)

# Solution
The first step is to download the Python script and the encrytped flag using the `wget` command:

<kbd>![Screenshot](https://github.com/user-attachments/assets/096a90cb-aeab-4949-857c-747b37224838)</kbd>

Next we need to make sure that both items are in the same directory. I created a directory called `bloat.py`

Once that is complete, we can first look at the Python script using `vi` and see what it is doing:

```python
import sys
a = "!\"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ"+ \
            "[\\]^_`abcdefghijklmnopqrstuvwxyz{|}~ "
def arg133(arg432):
  if arg432 == a[71]+a[64]+a[79]+a[79]+a[88]+a[66]+a[71]+a[64]+a[77]+a[66]+a[68]:
    return True
  else:
    print(a[51]+a[71]+a[64]+a[83]+a[94]+a[79]+a[64]+a[82]+a[82]+a[86]+a[78]+\
a[81]+a[67]+a[94]+a[72]+a[82]+a[94]+a[72]+a[77]+a[66]+a[78]+a[81]+\
a[81]+a[68]+a[66]+a[83])
    sys.exit(0)
    return False
def arg111(arg444):
  return arg122(arg444.decode(), a[81]+a[64]+a[79]+a[82]+a[66]+a[64]+a[75]+\
a[75]+a[72]+a[78]+a[77])
def arg232():
  return input(a[47]+a[75]+a[68]+a[64]+a[82]+a[68]+a[94]+a[68]+a[77]+a[83]+\
a[68]+a[81]+a[94]+a[66]+a[78]+a[81]+a[81]+a[68]+a[66]+a[83]+\
a[94]+a[79]+a[64]+a[82]+a[82]+a[86]+a[78]+a[81]+a[67]+a[94]+\
a[69]+a[78]+a[81]+a[94]+a[69]+a[75]+a[64]+a[70]+a[25]+a[94])
def arg132():
  return open('flag.txt.enc', 'rb').read()
def arg112():
  print(a[54]+a[68]+a[75]+a[66]+a[78]+a[76]+a[68]+a[94]+a[65]+a[64]+a[66]+\
a[74]+a[13]+a[13]+a[13]+a[94]+a[88]+a[78]+a[84]+a[81]+a[94]+a[69]+\
a[75]+a[64]+a[70]+a[11]+a[94]+a[84]+a[82]+a[68]+a[81]+a[25])
def arg122(arg432, arg423):
    arg433 = arg423
    i = 0
    while len(arg433) < len(arg432):
        arg433 = arg433 + arg423[i]
        i = (i + 1) % len(arg423)        
    return "".join([chr(ord(arg422) ^ ord(arg442)) for (arg422,arg442) in zip(arg432,arg433)])
arg444 = arg132()
arg432 = arg232()
arg133(arg432)
arg112()
arg423 = arg111(arg444)
print(arg423)
sys.exit(0)
```
----

What we can see here is that the script has several obfuscated sections. 
For example, `if arg432 == a[71]+a[64]+a[79]+a[79]+a[88]+a[66]+a[71]+a[64]+a[77]+a[66]+a[68]` is obfuscated.

----
> _**Obfuscation**_ is the action of making something obscure, unclear, or unintelligible
----
Each `a[i]` represents an index value of the variable `a` defined at the very top of the script. We can use an online Python compiler to help translate each function. Below is a breakdown of each deobfuscated section of the script:

1. Character Set `a`:

   ```python
    a = "!\"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ[\\]^_`abcdefghijklmnopqrstuvwxyz{|}~ "
   ```
   
2. Password Check Function `arg133`:

   ```python
    def arg133(arg432):
      if arg432 == "happychance":
        return True
      else:
        print("The password is incorrect")
        sys.exit(0)
        return False
   ```
   
3. Decryption Function `arg111`:

   ```python
    def arg111(arg444):
      return arg122(arg444.decode(), "rapscallion")
   ```
   
4. User Input Function `arg232`:

   ```python
    def arg232():
      return input("Please enter the password: ")
   ```
   
5. Reading Encrypted Flag:

   ```python
    def arg132():
      return open('flag.txt.enc', 'rb').read()
   ```
   
6. Print Welcome Message Function `arg112`:

   ```python
    def arg112():
      print("Flag decrypted successfully")
   ```
   
7. XOR Decryption Function `arg122`:

   ```python
    def arg122(arg432, arg423):
      arg433 = arg423
      i = 0
      while len(arg433) < len(arg432):
        arg433 = arg433 + arg423[i]
        i = (i + 1) % len(arg423)        
      return "".join([chr(ord(arg422) ^ ord(arg442)) for (arg422,arg442) in zip(arg432,arg433)])
   ```

8. Main Script Execution  

   ```python
    arg444 = arg132()
    arg432 = arg232()
    arg133(arg432)
    arg112()
    arg423 = arg111(arg444)
    print(arg423)
    sys.exit(0)

   ```
----

This now looks similar to the unpackme.py challenge I previously solved. The password the script compares to the user input is `happychance`. Let's first change the file permissions to allow execution:

```console
┌──(thor㉿kali)-[~/CTF/bloat.py]
└─$ chmod +x bloat.flag.py
```

Now let's run the script using Python:

```console
┌──(thor㉿kali)-[~/CTF/bloat.py]
└─$ python3 bloat.flag.py
Please enter correct password for flag: happychance
Welcome back... your flag, user:
picoCTF{d30bfu5c4710n_f7w_161a4f09}
```

# Flag
`picoCTF{d30bfu5c4710n_f7w_161a4f09}`
