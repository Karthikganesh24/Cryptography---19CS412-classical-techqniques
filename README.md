# Cryptography---19CS412-classical-techqniques
# Caeser Cipher
Caeser Cipher using with different key values

# AIM:

To encrypt and decrypt the given message by using Ceaser Cipher encryption algorithm.


## DESIGN STEPS:

### Step 1:

Design of Caeser Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

1.	In Ceaser Cipher each letter in the plaintext is replaced by a letter some fixed number of positions down the alphabet.
2.	For example, with a left shift of 3, D would be replaced by A, E would become B, and so on.
3.	The encryption can also be represented using modular arithmetic by first transforming the letters into numbers, according to the   
    scheme, A = 0, B = 1, Z = 25.
4.	Encryption of a letter x by a shift n can be described mathematically as,
                       En(x) = (x + n) mod26
5.	Decryption is performed similarly,
                       Dn (x)=(x - n) mod26


## PROGRAM:
def caesar_cipher(text, key, encrypt=True):
    result = ""
    for char in text:
        if char.isalpha():
            shift = key if encrypt else -key
            base = ord('A') if char.isupper() else ord('a')
            result += chr((ord(char) - base + shift) % 26 + base)
        else:
            result += char
    return result
plain_text = input("Enter the plain text: ")
key = int(input("Enter the key value: "))
encrypted_text = caesar_cipher(plain_text, key, encrypt=True)
print("\nPLAIN TEXT:", plain_text)
print("ENCRYPTED TEXT:", encrypted_text)
decrypted_text = caesar_cipher(encrypted_text, key, encrypt=False)
print("DECRYPTED TEXT:", decrypted_text)

## OUTPUT:
![image](https://github.com/user-attachments/assets/aa88c4ca-f844-40b7-9993-f212a41de6d3)

## RESULT:
The program is executed successfully
