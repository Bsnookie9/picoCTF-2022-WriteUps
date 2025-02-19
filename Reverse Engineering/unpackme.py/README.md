# Overview
`Medium` `Reverse Engineering` `picoCTF 2022` `packing`

# Description
Can you get the flag?  
Reverse engineer this [Python program](https://artifacts.picoctf.net/c/49/unpackme.flag.py).

# Hints
(None)

# Solution
First download the python file using the `wget command`:

<kbd>![Screenshot](https://github.com/user-attachments/assets/1dc70ce9-8c40-4559-8333-77eff3922577)</kbd>

Next we can use vim to view/edit the python file:

```python
import base64
from cryptography.fernet import Fernet

payload = b'gAAAAABkzWGSzE6VQNTzvRXOXekQeW4CY6NiRkzeImo9LuYBHAYw_hagTJLJL0c-kmNsjY33IUbU2IWlqxA3Fpp9S7RxNkiwMDZgLmRlI9-lGAEW-_i72RSDvylNR3QkpJW2JxubjLUC5VwoVgH62wxDuYu1rRD5KadwTADdABqsx2MkY6fKNTMCYY09Se6yjtRBftfTJUL-LKz2bwgXNd6O-WpbfXEMvCv3gNQ7sW4pgUnb-gDVZvrLNrug_1YFaIe3yKr0Awo0HIN3XMdZYpSE1c9P4G0sMQ=='

key_str = 'correctstaplecorrectstaplecorrec'
key_base64 = base64.b64encode(key_str.encode())
f = Fernet(key_base64)
plain = f.decrypt(payload)
exec(plain.decode())
```

Looking at this script, we can see that there is a `payload` that is then decrypted using a `key_str`.  
This decrypted `payload` is then converted into a string and executes the string as code.

Let's try running the script `python3 unpackme.flag.py`:

```console
bsnookie9-picoctf@webshell:~$ python3 unpackme.flag.py 
What's the password? pico
That password is incorrect.
```

That didn't work...  
Let's look at the code again

The script is executing the decrypted payload as code instead of being printed out.  
Let's edit the file by typing `i` to insert. Replace `exec(plain.decode())` with `print(plain.decode())`
Make sure to save your edit by hitting the `ESC` key and then typing `:w`. Then type `:q`

```python
import base64
from cryptography.fernet import Fernet

payload = b'gAAAAABkzWGSzE6VQNTzvRXOXekQeW4CY6NiRkzeImo9LuYBHAYw_hagTJLJL0c-kmNsjY33IUbU2IWlqxA3Fpp9S7RxNkiwMDZgLmRlI9-lGAEW-_i72RSDvylNR3QkpJW2JxubjLUC5VwoVgH62wxDuYu1rRD5KadwTADdABqsx2MkY6fKNTMCYY09Se6yjtRBftfTJUL-LKz2bwgXNd6O-WpbfXEMvCv3gNQ7sW4pgUnb-gDVZvrLNrug_1YFaIe3yKr0Awo0HIN3XMdZYpSE1c9P4G0sMQ=='

key_str = 'correctstaplecorrectstaplecorrec'
key_base64 = base64.b64encode(key_str.encode())
f = Fernet(key_base64)
plain = f.decrypt(payload)
print(plain.decode())
```

Now let's run the program again:

```console
bsnookie9-picoctf@webshell:~$ python3 unpackme.flag.py 

pw = input('What\'s the password? ')

if pw == 'batteryhorse':
  print('picoCTF{175_chr157m45_cd82f94c}')
else:
  print('That password is incorrect.')
```

# Flag
`picoCTF{175_chr157m45_cd82f94c}`
