# Python Криптография
## Задание
Реализуйте атаку грубой силой на текст, зашифрованный AES. В качестве пароля возьмите трёхзначное число.

### Решение

from Crypto.Cipher import AES
from Crypto import Random
import hashlib

BS = AES.block_size
pad = lambda s: s + (BS - len(s) % BS) * chr(BS - len(s) % BS)

plain_text = """ Secret
RedTeam
"""
key = input("Enter number from 100 to 999 : ")
key = int(key)
key = bytes(key)
key = hashlib.sha256(key).digest()
# print("Key:", key)

plain_text = pad(plain_text)
iv = Random.new().read(BS)
cipher = AES.new(key, AES.MODE_CBC, iv)
cipher_text = (iv + cipher.encrypt(plain_text.encode()))
# print("\nCiphered text:", cipher_text.hex())

for i in range(100, 1000):
    i = bytes(i)
    key1 = hashlib.sha256(i).digest()
    unpad = lambda s: s[:-ord(s[len(s)-1:])]
    iv = cipher_text[:BS]
    cipher = AES.new(key1, AES.MODE_CBC, iv)

    plain_text = unpad(cipher.decrypt(cipher_text[BS:]))
    print("Plain text:", plain_text)