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

---------------------------------

# PlayFair Cipher
Playfair Cipher using with different key values

# AIM:

To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.

 
## DESIGN STEPS:

### Step 1:

Design of PlayFair Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

ALGORITHM DESCRIPTION:
The Playfair cipher uses a 5 by 5 table containing a key word or phrase. To generate the key table, first fill the spaces in the table with the letters of the keyword, then fill the remaining spaces with the rest of the letters of the alphabet in order (usually omitting "Q" to reduce the alphabet to fit; other versions put both "I" and "J" in the same space). The key can be written in the top rows of the table, from left to right, or in some other pattern, such as a spiral beginning in the upper-left-hand corner and ending in the centre.
The keyword together with the conventions for filling in the 5 by 5 table constitutes the cipher key. To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. Then apply the following 4 rules, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter. Encrypt the new pair and continue. Some   
   variants of Playfair use "Q" instead of "X", but any letter, itself uncommon as a repeated pair, will do.
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively (wrapping 
   around to the left side of the row if a letter in the original pair was on the right side of the row).
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively (wrapping around 
   to the top side of the column if a letter in the original pair was on the bottom side of the column).
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of 
   corners of the rectangle defined by the original pair. The order is important – the first letter of the encrypted pair is the one that 
    lies on the same row as the first letter of the plaintext pair.
To decrypt, use the INVERSE (opposite) of the last 3 rules, and the 1st as-is (dropping any extra "X"s, or "Q"s that do not make sense in the final message when finished).


## PROGRAM:
import re

def to_lower_case(text):
    return text.lower()

def remove_spaces(text):
    return re.sub(r'\s+', '', text)

def generate_key_table(key):
    key = remove_spaces(to_lower_case(key))
    key = key.replace('j', 'i')
    key_set = set()
    key_table = []
    
    for char in key:
        if char not in key_set and 'a' <= char <= 'z':
            key_set.add(char)
            key_table.append(char)
    
    for char in 'abcdefghiklmnopqrstuvwxyz':
        if char not in key_set:
            key_set.add(char)
            key_table.append(char)
    
    return [key_table[i * 5:(i + 1) * 5] for i in range(5)]

def search(key_table, char1, char2):
    if char1 == 'j':
        char1 = 'i'
    if char2 == 'j':
        char2 = 'i'
    
    pos = {}
    for i in range(5):
        for j in range(5):
            pos[key_table[i][j]] = (i, j)
    
    return [pos[char1][0], pos[char1][1], pos[char2][0], pos[char2][1]]

def mod5(num):
    return (num % 5 + 5) % 5

def prepare_text(text):
    text = remove_spaces(to_lower_case(text)).replace('j', 'i')
    prepared_text = ''
    i = 0
    while i < len(text):
        if i == len(text) - 1 or text[i] == text[i + 1]:
            prepared_text += text[i] + 'x'
            i += 1
        else:
            prepared_text += text[i] + text[i + 1]
            i += 2
    
    if len(prepared_text) % 2 != 0:
        prepared_text += 'z'
    
    return prepared_text

def encrypt(text, key_table):
    encrypted_text = ''
    for i in range(0, len(text), 2):
        a, b, c, d = search(key_table, text[i], text[i + 1])
        if a == c:
            encrypted_text += key_table[a][mod5(b + 1)] + key_table[c][mod5(d + 1)]
        elif b == d:
            encrypted_text += key_table[mod5(a + 1)][b] + key_table[mod5(c + 1)][d]
        else:
            encrypted_text += key_table[a][d] + key_table[c][b]
    return encrypted_text

def decrypt(text, key_table):
    decrypted_text = ''
    for i in range(0, len(text), 2):
        a, b, c, d = search(key_table, text[i], text[i + 1])
        if a == c:
            decrypted_text += key_table[a][mod5(b - 1)] + key_table[c][mod5(d - 1)]
        elif b == d:
            decrypted_text += key_table[mod5(a - 1)][b] + key_table[mod5(c - 1)][d]
        else:
            decrypted_text += key_table[a][d] + key_table[c][b]
    return decrypted_text

def playfair_cipher(text, key, mode='encrypt'):
    key_table = generate_key_table(key)
    text = prepare_text(text)
    return encrypt(text, key_table) if mode == 'encrypt' else decrypt(text, key_table)

if __name__ == "__main__":
    key = "Monopoly"
    plain_text = "VARSHINI"
    
    print("Simulating Playfair Cipher")
    print("Key text:", key)
    print("Plain text:", plain_text)
    
    encrypted_text = playfair_cipher(plain_text, key, 'encrypt')
    print("Cipher text:", encrypted_text)
    
    decrypted_text = playfair_cipher(encrypted_text, key, 'decrypt')
    print("Decrypted text:", decrypted_text)

## OUTPUT:
![ex2](https://github.com/user-attachments/assets/6d1171db-e1f0-49c4-a589-6026302198ef)

## RESULT:
The program is executed successfully

