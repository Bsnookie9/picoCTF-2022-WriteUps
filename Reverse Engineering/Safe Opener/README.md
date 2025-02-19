# Overview
`Medium` `Reverse Engineering` `picoCTF 2022`

# Description
Can you open this safe?  
I forgot the key to my safe but this [program](https://artifacts.picoctf.net/c/83/SafeOpener.java) is supposed to help me with retrieving the lost key.   
Can you help me unlock my safe?  
Put the password you recover into the picoCTF flag format like: `picoCTF{password}`

# Hints
None

# Solution
Using the webshell, we can use the `wget` command to download the java program listed in the description

<kbd>![Screenshot](https://github.com/user-attachments/assets/8afbe207-88fc-4462-a177-47c722aeaf1a)</kbd>

Once downloaded, we can use the `ls` command to see the file name which is 'SafeOpener.java'

Open the file using the `vi` editor using this command: `vi SafeOpener.java`

```java
import java.io.*;
import java.util.*;
public class SafeOpener {
    public static void main(String args[]) throws IOException {
        BufferedReader keyboard = new BufferedReader(new InputStreamReader(System.in));
        Base64.Encoder encoder = Base64.getEncoder();
        String encodedkey = "";
        String key = "";
        int i = 0;
        boolean isOpen;


        while (i < 3) {
            System.out.print("Enter password for the safe: ");
            key = keyboard.readLine();

            encodedkey = encoder.encodeToString(key.getBytes());
            System.out.println(encodedkey);

            isOpen = openSafe(encodedkey);
            if (!isOpen) {
                System.out.println("You have  " + (2 - i) + " attempt(s) left");
                i++;
                continue;
            }
            break;
        }
    }

    public static boolean openSafe(String password) {
        String encodedkey = "cGwzYXMzX2wzdF9tM18xbnQwX3RoM19zYWYz";

        if (password.equals(encodedkey)) {
            System.out.println("Sesame open");
            return true;
        }
        else {
            System.out.println("Password is incorrect\n");
            return false;
        }
    }
}
```

Here we can see that Base64 encoding is used when taking input from the user and then checking against a saved password

In the `openSafe` function, we see `String encodedkey = "cGwzYXMzX2wzdF9tM18xbnQwX3RoM19zYWYz";`

This is a string of Base64 without the `==` at the end. Let's use CyberChef to decode this:

<kbd>![Screenshot](https://github.com/user-attachments/assets/357cb400-359c-4f92-851f-83d80f6aa2a4)</kbd>

# Flag
`picoCTF{pl3as3_l3t_m3_1nt0_th3_saf3}`
