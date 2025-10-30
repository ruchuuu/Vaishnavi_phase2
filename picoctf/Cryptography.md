# 1. miniRSA
> Let's decrypt this: ciphertext. The text file has:  
  N: 29331922499794985782735976045591164936683059380558950386560160105740343201513369939006307531165922708949619162698623675349030430859547825708994708321803705309459438099340427770580064400911431856656901982789948285309956111848686906152664473350940486507451771223435835260168971210087470894448460745593956840586530527915802541450092946574694809584880896601317519794442862977471129319781313161842056501715040555964011899589002863730868679527184420789010551475067862907739054966183120621407246398518098981106431219207697870293412176440482900183550467375190239898455201170831410460483829448603477361305838743852756938687673  
  e: 3  
  ciphertext (c): 2205316413931134031074603746928247799030155221252519872650080519263755075355825243327515211479747536697517688468095325517209911688684309894900992899707504087647575997847717180766377832435022794675332132906451858990782325436498952049751141 

## Solution:
- The name suggests that the text is RSA encoded. Using an online RSA decoder, I entered the values of N, e and c and got the flag.
## Flag:
```
picoCTF{n33d_a_lArg3r_e_d0cd6eae}
```
## Concept Learnt:
- RSA: The Public Key is used for encryption and is known to everyone, while the Private Key is used for decryption and must be kept secret by the receiver.
- e is the public exponent, a component of the public key and a number used to encrypt messages. A common and efficient choice for e is 65537.
- But here, a smaller e is used- single digit number.
## Resources:
- https://www.dcode.fr/rsa-cipher
- https://www.geeksforgeeks.org/computer-networks/rsa-algorithm-cryptography/
- https://di-mgt.com.au/rsa_alg.html#:~:text=e%20is%20known%20as%20the,)(q%E2%88%921).
  ***
# 2. Custom encryption
> Get sense of the code file and write the function that will decode the given encrypted file content.
## Solution:
- From the hint, we are supposed to analyze the encryption algorithm and come up with decryption algorithm.
- Upon reversing the logic in the source code, it can be used to decrypt the flag.
- Such as writing decrypt instead of encrypt function, ciphertext with plaintext and vice-versa. Also change the data type from array(int) to string (char) for cipher to plain.
- Enter the given values of a and b and ciphertext in message input.
- Then run the code to get the flag.
- The decryption code is below with comments at every change done from original source code.

```python
from random import randint
import sys


def generator(g, x, p):
    return pow(g, x) % p


def decrypt(ciphertext, key): # decrypt function that takes each integer from ciphertext and divides it by key which is then divided by 311 (since each char from plaintext is multiplied)
    plain = ""
    for num in ciphertext:
        plain+= chr(int((num / key / 311))) # num and key already converted to int, the division then converted to characters for plain string.
    return plain # return the plain string obtained



def is_prime(p):
    v = 0
    for i in range(2, p + 1):
        if p % i == 0:
            v = v + 1
    if v > 1:
        return False
    else:
        return True


def dynamic_xor_decrypt(ciphertext, text_key): # dynamic_xor_decrypt function loops each character of ciphertext to use bitwise XOR on ASCII value of each ciphertext character and ASCII of each key character.
    plain_text = ""
    key_length = len(text_key)
    for i, char in enumerate(ciphertext):
        key_char = text_key[i % key_length]
        decrypted_char = chr(ord(char) ^ ord(key_char))
        plain_text += decrypted_char # The decrypted characters get added to plain_text to make a string.
    return plain_text


def test(cipher_text, text_key):
    p = 97
    g = 31
    if not is_prime(p) and not is_prime(g):
        print("Enter prime numbers")
        return
    a = 90 #randint(p-10, p) # a set to 90
    b = 26 #randint(g-10, g) # b set to 26
    print(f"a = {a}")
    print(f"b = {b}")
    u = generator(g, a, p)
    v = generator(g, b, p)
    key = generator(v, a, p)
    b_key = generator(u, b, p)
    shared_key = None
    if key == b_key:
        shared_key = key
    else:
        print("Invalid key")
        return
    semi_cipher = decrypt(cipher_text, shared_key) # Performs the decrypt function on cipher_text and the shared_key generated through v,a,p
    plain = dynamic_xor_decrypt(semi_cipher, text_key) # dynamic_xor_decrypt performed on the semi_cipher with text_key to get the plain string.
    print(f'plain is: {plain[::-1]}') # Reverse the plain string because in the original code, the plaintext is reversed so the ciphertext also comes out to be reversed.


if __name__ == "__main__":
    message = [61578, 109472, 437888, 6842, 0, 20526, 129998, 526834, 478940, 287364, 0, 567886, 143682, 34210, 465256, 0, 150524, 588412, 6842, 424204, 164208, 184734, 41052, 41052, 116314, 41052, 177892, 348942, 218944, 335258, 177892, 47894, 82104, 116314] ## In the message, the argument is supposed to be the ciphertext. This will take to test function with this ciphertext with key "trudeau".
    test(message, "trudeau")
```
- Open in terminal to get the flag.
```
vaish@DESKTOP-59LUGSG:/mnt/c/Users/Vaishnavi/Downloads$ python3 custom.py
a = 90
b = 26
plain is: picoCTF{custom_d2cr0pt6d_49fbee5b}
```

## Flag:
```
picoCTF{custom_d2cr0pt6d_49fbee5b}
```
## Concepts Learnt:
- Upon searching, I found out that the generate function is similar to Diffie-Hellman key exchange, that is, a fundamental cryptographic protocol used to allow two parties to securely agree on a shared secret over an insecure channel without ever sending the secret itself.
- XOR is reversible: plaintext can be found by reversing the ciphertext logic.

## Notes:
- I had not converted `(num/ key/311)` into char and got an error because plain variable is declared as string.

## Resources:
- https://www.geeksforgeeks.org/dsa/xor-cipher/
- https://www.geeksforgeeks.org/computer-networks/implementation-diffie-hellman-algorithm/
***
