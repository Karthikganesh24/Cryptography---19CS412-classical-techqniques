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

---------------------------

# Hill Cipher
Hill Cipher using with different key values

# AIM:

To develop a simple C program to implement Hill Cipher.

## DESIGN STEPS:

### Step 1:

Design of Hill Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
The Hill cipher is a substitution cipher invented by Lester S. Hill in 1929. Each letter is represented by a number modulo 26. To encrypt a message, each block of n letters is multiplied by an invertible n × n matrix, again modulus 26.
To decrypt the message, each block is multiplied by the inverse of the matrix used for encryption. The matrix used for encryption is the cipher key, and it should be chosen randomly from the set of invertible n × n matrices (modulo 26).
The cipher can, be adapted to an alphabet with any number of letters. All arithmetic just needs to be done modulo the number of letters instead of modulo 26.

## PROGRAM:

import numpy as np

def mod_inverse_matrix(matrix, mod):
    det = int(np.round(np.linalg.det(matrix)))
    det_inv = pow(det, -1, mod)  # Modular inverse of determinant
    adjugate = np.round(det * np.linalg.inv(matrix)).astype(int) % mod
    return (det_inv * adjugate) % mod

def text_to_numbers(text):
    return [ord(c) - 65 for c in text]

def numbers_to_text(numbers):
    return ''.join(chr(n % 26 + 65) for n in numbers)

def encode_block(block, key_matrix):
    block_vector = np.array(text_to_numbers(block)).reshape(-1, 1)
    encoded_vector = np.dot(key_matrix, block_vector) % 26
    return numbers_to_text(encoded_vector.flatten())

def decode_block(block, inv_key_matrix):
    block_vector = np.array(text_to_numbers(block)).reshape(-1, 1)
    decoded_vector = np.dot(inv_key_matrix, block_vector) % 26
    return numbers_to_text(decoded_vector.flatten())

def hill_cipher_encrypt(text, key_matrix):
    text = text.upper().replace(" ", "")
    while len(text) % 3 != 0:
        text += 'X'  # Padding with 'X'
    
    encrypted_text = ""
    for i in range(0, len(text), 3):
        encrypted_text += encode_block(text[i:i+3], key_matrix)
    return encrypted_text

def hill_cipher_decrypt(text, inv_key_matrix):
    decrypted_text = ""
    for i in range(0, len(text), 3):
        decrypted_text += decode_block(text[i:i+3], inv_key_matrix)
    return decrypted_text

if __name__ == "__main__":
    key_matrix = np.array([[1, 2, 1], [2, 3, 2], [2, 2, 1]])
    inv_key_matrix = mod_inverse_matrix(key_matrix, 26)
    
    message = "SecurityLaboratory"
    print("Simulation of Hill Cipher")
    print("Input message:", message)
    
    encrypted_message = hill_cipher_encrypt(message, key_matrix)
    print("Encoded message:", encrypted_message)
    
    decrypted_message = hill_cipher_decrypt(encrypted_message, inv_key_matrix)
    print("Decoded message:", decrypted_message)

## OUTPUT:
![image](https://github.com/user-attachments/assets/12bb27c1-3c9d-450d-b25c-0718a1b39643)

## RESULT:
The program is executed successfully

-------------------------------------------------

# Vigenere Cipher
Vigenere Cipher using with different key values

# AIM:

To develop a simple C program to implement Vigenere Cipher.

## DESIGN STEPS:

### Step 1:

Design of Vigenere Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
The Vigenere cipher is a method of encrypting alphabetic text by using a series of different Caesar ciphers based on the letters of a keyword. It is a simple form of polyalphabetic substitution.To encrypt, a table of alphabets can be used, termed a Vigenere square, or Vigenere table. It consists of the alphabet written out 26 times in different rows, each alphabet shifted cyclically to the left compared to the previous alphabet, corresponding to the 26 possible Caesar ciphers. At different points in the encryption process, the cipher uses a different alphabet from one of the rows used. The alphabet at each point depends on a repeating keyword.

## PROGRAM:
def vigenere_encrypt(text, key):
    encrypted_text = []
    key_length = len(key)
    
    for i, c in enumerate(text):
        if 'A' <= c <= 'Z':
            encrypted_text.append(chr((ord(c) - ord('A') + ord(key[i % key_length]) - ord('A')) % 26 + ord('A')))
        elif 'a' <= c <= 'z':
            encrypted_text.append(chr((ord(c) - ord('a') + ord(key[i % key_length]) - ord('A')) % 26 + ord('A')))
        else:
            encrypted_text.append(c)
    
    return ''.join(encrypted_text)

def vigenere_decrypt(text, key):
    decrypted_text = []
    key_length = len(key)
    
    for i, c in enumerate(text):
        if 'A' <= c <= 'Z':
            decrypted_text.append(chr((ord(c) - ord('A') - (ord(key[i % key_length]) - ord('A')) + 26) % 26 + ord('A')))
        elif 'a' <= c <= 'z':
            decrypted_text.append(chr((ord(c) - ord('A') - (ord(key[i % key_length]) - ord('A')) + 26) % 26 + ord('A')))
        else:
            decrypted_text.append(c)
    
    return ''.join(decrypted_text)

if __name__ == "__main__":
    print("Simulating Vigenere Cipher\n")
    
    key = "KEY"  # Replace with your desired key
    message = "SecurityLaboratory"
    
    print("Input Message:", message.upper())
    encrypted_message = vigenere_encrypt(message, key)
    print("Encrypted Message:", encrypted_message.upper())
    
    decrypted_message = vigenere_decrypt(encrypted_message, key)
    print("Decrypted Message:", decrypted_message.upper())

## OUTPUT:
![Screenshot 2025-03-26 091220](https://github.com/user-attachments/assets/6f1122d9-8b64-4982-bb05-93e8808c81f5)

## RESULT:
The program is executed successfully

-----------------------------------------------------------------------
