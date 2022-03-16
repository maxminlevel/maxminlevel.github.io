---
layout: post
bg: "/ctf.jpg"
title: "Write up: Crypto Hack General"
summary: "The first step towards knowledge is to know that we are ignorant. - Richard Cecil"
tags: ['CTF']
date: 2022-03-17

---

This week I done a quiz from Infomation Security course. Through this I've learned a lot of knowdledge like how to use pwntools, pwnlib, more about XOR, modulo and RSA. This is my approach to solve 19 exercises in this quiz.

It in consists of:
* [Encoding](#encoding)
* [XOR](#xor)
* [Mathematics](#-mathematics)
* [Data formats](#data-formats)

![Lemur](/assets/images/crypto-general/sol.png)
### Encoding
---
#### ASCII
<!-- ASCII is a 7-bit encoding standard which allows the representation of text using the integers 0-127. -->

Using the below integer array, convert the numbers to their corresponding ASCII characters to obtain a flag.

[99, 114, 121, 112, 116, 111, 123, 65, 83, 67, 73, 73, 95, 112, 114, 49, 110, 116, 52, 98, 108, 51, 125]

<!-- Dùng hàm chr(i) để chuyển số i về dạng ký tự -->
Use function chr() to convert integer to char 
```python
c = [99, 114, 121, 112, 116, 111, 123, 65, 83, 67, 73, 73, 95, 112, 114, 49, 110, 116, 52, 98, 108, 51, 125]
s = ""
for i in c:
    s += chr(i)
print(s) 
```
Flag: 
```
crypto{You_will_be_working_with_hex_strings_a_lot}
```
#### Hex
<!-- When we encrypt something the resulting ciphertext commonly has bytes which are not printable ASCII characters. If we want to share our encrypted data, it's common to encode it into something more user-friendly and portable across different systems. -->

Included below is a flag encoded as a hex string. Decode this back into bytes to get the flag.

```
63727970746f7b596f755f77696c6c5f62655f776f726b696e675f776974685f6865785f737472696e67735f615f6c6f747d
```

```python
c = "63727970746f7b596f755f77696c6c5f62655f776f726b696e675f776974685f6865785f737472696e67735f615f6c6f747d"
s = bytes.fromhex(c)
print(s)
```
Flag: 
```
crypto{You_will_be_working_with_hex_strings_a_lot}
```

#### Base64
<!-- Another common encoding scheme is Base64, which allows us to represent binary data as an ASCII string using 64 characters. One character of a Base64 string encodes 6 bits, and so 4 characters of Base64 encode three 8-bit bytes.

Base64 is most commonly used online, so binary data such as images can be easily included into HTML or CSS files. -->

Take the below hex string, decode it into bytes and then encode it into Base64.

```
72bca9b68fc16ac7beeb8f849dca1d8a783e8acf9679bf9269f7bf
```
```python
c = "72bca9b68fc16ac7beeb8f849dca1d8a783e8acf9679bf9269f7bf"
s1 = bytes.fromhex(c)
print(s1)
# b'r\xbc\xa9\xb6\x8f\xc1j\xc7\xbe\xeb\x8f\x84\x9d\xca\x1d\x8ax>\x8a\xcf\x96y\xbf\x92i\xf7\xbf'
import base64
s2 = base64.b64encode(s1)
print(s2)
```
Flag: 
```crypto/Base+64+Encoding+is+Web+Safe/
```
#### Bytes and Big Integers
<!-- Cryptosystems like RSA works on numbers, but messages are made up of characters. How should we convert our messages into numbers so that mathematical operations can be applied? 

The most common way is to take the ordinal bytes of the message, convert them into hexadecimal, and concatenate. This can be interpreted as a base-16 number, and also represented in base-10. -->

To illustrate:
```
message: HELLO
ascii bytes: [72, 69, 76, 76, 79]
hex bytes: [0x48, 0x45, 0x4c, 0x4c, 0x4f]
base-16: 0x48454c4c4f
base-10: 310400273487
```
Convert the following integer back into a message:

```
11515195063862318899931685488813747395775516287289682636499965282714637259206269
```
```python
from Crypto.Util.number import *
c = "11515195063862318899931685488813747395775516287289682636499965282714637259206269"
long_to_bytes(c)
```
Flag: 
```
crypto{3nc0d1n6_4ll_7h3_w4y_d0wn}
```

#### Encoding Challenge
Now you've got the hang of the various encodings you'll be encountering, let's have a look at automating it. Can you pass all 100 levels to get the flag?

The [13377.py](https://cryptohack.org/static/challenges/13377_43de0a0efed6ed7bd890d1c79db22fb1.py) file attached below is the source code for what's running on the server. The [pwntools_example.py](https://cryptohack.org/static/challenges/pwntools_example_f93ca6ccef2def755aa8f6d9aa6e9c5b.py) file provides the start of a solution using the incredibly convenient pwntools library. which we recommend. If you'd prefer to use Python's in-built telnetlib, [telnetlib_example.py](https://cryptohack.org/static/challenges/telnetlib_example_5b11a835055df17c7c8f8f2a08782c44.py) is also provided.

For more information about connecting to interactive challenges, see the [FAQ](https://cryptohack.org/faq#netcat). Feel free to skip ahead to the cryptography if you aren't in the mood for a coding challenge! 

Connect at **nc socket.cryptohack.org 13377**
Read file 13377.py I knew that server open on port 13377. When it's connected, it would take a random word in /usr/share/dict/words then use base64, hex, rot13, bigint or utf-8 to encrypt it then send to client. 

I write a simple code to recieved the string then decrypted it and send it back for 100 times.
```python
from pwn import *  # pip install pwntools
import json
from Crypto.Util.number import bytes_to_long, long_to_bytes
import base64
import codecs
import random
from binascii import unhexlify

r = remote('socket.cryptohack.org', 13377, level='debug')

def json_recv():
    line = r.recvline()
    return json.loads(line.decode())

def json_send(hsh):
    request = json.dumps(hsh).encode()
    r.sendline(request)

def list_to_string(s):
    output = ""
    return(output.join(s))

for i in range(0, 101):
    received = json_recv()
    if "flag" in received:
        print("\n[*] FLAG: {}".format(received["flag"]))
        break

    print("\n[-] Cycle: {}".format(i))
    print("[-] Received type: {}".format(received["type"]))
    print("[-] Received encoded value: {}".format(received["encoded"]))

    word = received["encoded"]
    encoding = received["type"]

    if encoding == "base64":
        decoded = base64.b64decode(word).decode('utf8').replace("'", '"')
    elif encoding == "hex":
        decoded = (unhexlify(word)).decode('utf8').replace("'", '"')
    elif encoding == "rot13":
        decoded = codecs.decode(word, 'rot_13')
    elif encoding == "bigint":
        decoded = unhexlify(word.replace("0x", "")).decode(
            'utf8').replace("'", '"')
    elif encoding == "utf-8":
        decoded = list_to_string([chr(b) for b in word])

    print("[-] Decoded: {}".format(decoded))
    print("[-] Decoded Type: {}".format(type(decoded)))

    to_send = {
        "decoded": decoded
    }

    json_send(to_send)
```
Flag: 
```
crypto{3nc0d3_d3c0d3_3nc0d3}
```

### XOR
--- 
#### XOR Starter
<!-- XOR is a bitwise operator which returns 0 if the bits are the same, and 1 otherwise. In textbooks the XOR operator is denoted by ⊕, but in most challenges and programming languages you will see the caret ^ used instead.
A	B	Output
0	0	0
0	1	1
1	0	1
1	1	0
For longer binary numbers we XOR bit by bit: 0110 ^ 1010 = 1100. We can XOR integers by first converting the integer from decimal to binary. We can XOR strings by first converting each character to the integer representing the Unicode character. -->

Given the string "label", XOR each character with the integer 13. Convert these integers back to a string and submit the flag as crypto{new_string}.

Normal way, when xor we would padding or xor one-by-one but the function XOR by pwnlib included flat (padding) so we just use it.
```python
import pwnlib
s = pwnlib.util.fiddling.xor("label", 13).decode()
```
Flag: 
```
crypto{aloha}
```

#### XOR Properties
<!-- In the last challenge, you saw how XOR worked at the level of bits. In this one, we're going to cover the properties of the XOR operation and then use them to undo a chain of operations that have encrypted a flag. Gaining an intuition for how this works will help greatly when you come to attacking real cryptosystems later, especially in the block ciphers category.

There are four main properties we should consider when we solve challenges using the XOR operator
```
Commutative: A ⊕ B = B ⊕ A
Associative: A ⊕ (B ⊕ C) = (A ⊕ B) ⊕ C
Identity: A ⊕ 0 = A
Self-Inverse: A ⊕ A = 0
```
Let's break this down. Commutative means that the order of the XOR operations is not important. Associative means that a chain of operations can be carried out without order (we do not need to worry about brackets). The identity is 0, so XOR with 0 "does nothing", and lastly something XOR'd with itself returns zero.

Let's try this out in action! Below is a series of outputs where three random keys have been XOR'd together and with the flag. Use the above properties to  -->
... undo the encryption in the final line to obtain the flag.
```
KEY1 = a6c8b6733c9b22de7bc0253266a3867df55acde8635e19c73313
KEY2 ^ KEY1 = 37dcb292030faa90d07eec17e3b1c6d8daf94c35d4c9191a5e1e
KEY2 ^ KEY3 = c1545756687e7573db23aa1c3452a098b71a7fbf0fddddde5fc1
FLAG ^ KEY1 ^ KEY3 ^ KEY2 = 04ee9855208a2cd59091d04767ae47963170d1660df7f56f5faf
```
The FLAG was xor for K1, K2, K3 so we would xor again with K1, K2 and K3 to reveal the FLAG again.
```python
KEY1 =  bytes.fromhex("a6c8b6733c9b22de7bc0253266a3867df55acde8635e19c73313")
KEY21 =  bytes.fromhex("37dcb292030faa90d07eec17e3b1c6d8daf94c35d4c9191a5e1e")
KEY23 =  bytes.fromhex("c1545756687e7573db23aa1c3452a098b71a7fbf0fddddde5fc1")
FK123 =  bytes.fromhex("04ee9855208a2cd59091d04767ae47963170d1660df7f56f5faf")
F = pwnlib.util.fiddling.xor(FK123, KEY23, KEY1)
print(F)
```
Flag: 
```
crypto{x0r_i5_ass0c1at1v3}
```
#### Favourite byte
<!-- For the next few challenges, you'll use what you've just learned to solve some more XOR puzzles. -->

I've hidden some data using XOR with a single byte, but that byte is a secret. Don't forget to decode from hex first.
```
73626960647f6b206821204f21254f7d694f7624662065622127234f726927756d
```
After decode the text from hex to bytes, I bruteforce it with a integer (limit in value of 1 byte).
```python
c = "73626960647f6b206821204f21254f7d694f7624662065622127234f726927756d"
s1 = bytes.fromhex(c)
for i in range(pow(2,9)):
    s = pwnlib.util.fiddling.xor(s1, i)
    s = s.decode()
    if s.startswith("crypto{"):
        print(s)
        print(i)
# crypto{0x10_15_my_f4v0ur173_by7e}
# 16
```
Flag: 
```
crypto{0x10_15_my_f4v0ur173_by7e}
```

#### You either know, XOR you don't
I've encrypted the flag with my secret key, you'll never be able to guess it.

```
0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d0e077e263451150104
```

The flag must start with "crypto{", I XOR the encrypted message with this word to have the key. The key is "myXORke", but when XOR it with cipher I didn't have } at the last position. Then I append the y character to key. And tada !!!
```python
c = "0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d0e077e263451150104"
s1 = bytes.fromhex(c)
key = pwnlib.util.fiddling.xor(s1[:7], b'crypto{').decode()
print(key)
# myXORke
key += 'y'
s = pwnlib.util.fiddling.xor(key.encode(), s1).decode()
print(s)
```
Flag: 
```
crypto{1f_y0u_Kn0w_En0uGH_y0u_Kn0w_1t_4ll}
```
#### Lemur XOR
I've hidden two cool images by XOR with the same secret key so you can't see them!
* Lemur image

![Lemur](/assets/images/crypto-general/lemur.png)
* Flag image

![Flag](../assets/images/crypto-general/flag.png)

The png file is a structure file, we only need to xor the 2d-array pixel in data part. I use cv2 bitwise to do.
```python
import cv2

img1 = cv2.imread('flag.png')
img2 = cv2.imread('lemur.png')

solve = cv2.bitwise_xor(img1, img2)
cv2.imwrite('solve.png', solve)
```
Solve image

![Solve](../assets/images/crypto-general/solve.png)

Flag: 
```
crypto{X0Rly_n0t!}
```
### Mathematics
---
#### Greatest Common Divisor
<!-- The Greatest Common Divisor (GCD), sometimes known as the highest common factor, is the largest number which divides two positive integers (a,b).

For a = 12, b = 8 we can calculate the divisors of a: {1,2,3,4,6,12} and the divisors of b: {1,2,4,8}. Comparing these two, we see that gcd(a,b) = 4.

Now imagine we take a = 11, b = 17. Both a and b are prime numbers. As a prime number has only itself and 1 as divisors, gcd(a,b) = 1.

We say that for any two integers a,b, if gcd(a,b) = 1 then a and b are coprime integers.

If a and b are prime, they are also coprime. If a is prime and b < a then a and b are coprime.

There are many tools to calculate the GCD of two integers, but for this task we recommend looking up Euclid's Algorithm.

Try coding it up; it's only a couple of lines. Use a = 12, b = 8 to test it. -->

Now calculate gcd(a,b) for a = 66528, b = 52920 and enter it below.
```python
def gcd(a, b):
    if a==0:
        return b
    else:
        return gcd(b%a, a)

a = 66528
b = 52920
gcd(a,b)
# 1512
```
Result: 1512
#### Extended GCD
<!-- Let a and b be positive integers.

The extended Euclidean algorithm is an efficient way to find integers u,v such that

a * u + b * v = gcd(a,b) -->

Using the two primes p = 26513, q = 32321, find the integers u,v such that:

```
p * u + q * v = gcd(p,q)
```

Enter whichever of u and v is the lower number as the flag.
```python
def gcdExtended(a, b): 
    if a == 0 :  
        return b,0,1
             
    gcd,x1,y1 = gcdExtended(b%a, a) 
     
    x = y1 - (b//a) * x1 
    y = x1 
     
    return gcd,x,y

p = 26513
q = 32321
gcd, u, v = gcdExtended(p, q) # (1, 10245, -8404)
print(min(u, v)) # -8404
```
Result: -8404

#### Modular Arithmetic 1
Calculate the following integers:
```
11 ≡ x mod 6
8146798528947 ≡ y mod 17
```
The solution is the smaller of the two integers.
```python
r1 = 11 % 6
r2 = 8146798528947 % 17
print(min(r1, r2))
# 4
```
Result: 4

#### Modular Arithmetic 2
Calculate 27324678765465536 mod 65537

Fermat little Theorem: If p is a prime and a is any integer not divisible by p, then a^(p − 1) − 1 is divisible by p.

273246787654^65536 - 1 === 0 mod 65537 so 273246787654^65536 === 1 mod 65537
```python
273246787654 % 65537 == 31167
# True
# Not divisible
```
Result: 1
#### Modular Inverting
<!-- As we've seen, we can work within a finite field Fp, adding and multiplying elements, and always obtain another element of the field.

For all elements g in the field, there exists a unique integer d such that g * d ≡ 1 mod p.

This is the multiplicative inverse of g. -->

Example: 7 * 8 = 56 ≡ 1 mod 11
What is the inverse element: 3 * d ≡ 1 mod 13?
```python
gcdExtended(3, 13)
# (1, -4, 1)
```
Result: -4

### DATA FORMATS
---     
#### Privacy-Enhanced Mail?
<!-- As we've seen in the encoding section, cryptography involves dealing with data in a wide variety of formats: big integers, raw bytes, hex strings and more. A few structured formats have been standardised to help send and receive cryptographic data. It helps to be able to recognise and manipulate these common data formats.

PEM is a popular format for sending keys, certificates, and other cryptographic material. It looks like:
```
-----BEGIN RSA PUBLIC KEY-----
MIIBCgKC... (a whole bunch of base64)
-----END RSA PUBLIC KEY-----
```
It wraps base64-encoded data by a one-line header and footer to indicate how to parse the data within. Perhaps unexpectedly, it's important for there to be the correct number of hyphens in the header and footer, otherwise cryptographic tools won't be able to recognise the file.

The data that gets base64-encoded is DER-encoded ASN.1 values. Confused? Here is more information about what these acronyms mean but the complexity is there for historical reasons and going too deep into the details may drive you insane. -->

Extract the private key d as a decimal integer from this PEM-formatted RSA key.
[privacy_enhanced_mail.pem](https://cryptohack.org/static/challenges/privacy_enhanced_mail_1f696c053d76a78c2c531bb013a92d4a.pem)

PEM is a format file to encapsulate the encrypted key. To get the key we read the pem file on asn1 format. 
[RSA_DER](../assets/images/crypto-general/rsa_der_format.png)

Run command below to extract info from pem file. It's include: 

```python
!openssl asn1parse -i -in privacy_enhanced_mail.pem
# Result
0:d=0  hl=4 l=1187 cons: SEQUENCE          
    4:d=1  hl=2 l=   1 prim:  INTEGER           :00
    7:d=1  hl=4 l= 257 prim:  INTEGER           :CEF283B7E10EF80EA81352C8B52BA791627CBCDE9381CBBC0A4D3BEE3A060DF1B636DC43E2FC90C464D09E0AFA8C4289B95CF6308C0371D5ED14B205F88497F477C02D0DF289E74D4BCD27FD347504D7094A5CC8BAA2E617175D9A399F52F1C5B852EFC605D181ADE6837AFBA254D6FB57F1392D1955E0C9A8509A39146FA0060E80B82E776FC4D1A69A262F5C37B0A399D27B4379BAAF21AE0B0A84BD9D4A8155966BB7CA705A299E5FDF72C49E7306C35798567E6BF1880509CB598D48FC247F9621190FBCDE0F4DE7CF11A4B2CC5E682DE8A8A6B625A726CAAD0CF465638063C5DA6E57743251DA36CA6AA7AAE66713FBE553F867EBFB3CF9CB5D4EA7D20B
  268:d=1  hl=2 l=   3 prim:  INTEGER           :010001
  273:d=1  hl=4 l= 256 prim:  INTEGER           :7C3B1D534F299B43C1260876303C0A95BE17BF91A5DF2F1CACDA7C75A0236E4F81E1210D27C0126FB34D80F27A41A4D7E48CA7C5B0E78878B19FD0D6C0BF6830FB8A4401B16D938AD54C4D0B356862056CB0554EB2AB8390AD1825B31DAFBF2FC05D194F38C2F22420D3210ADA0230242640CAE005EB85CBC8DCCA1825EA7496D9B170C5CBFE354FE19A63102B82F38D5D7C251735208B83A54240927F899848C16A5FE70CE950DAFF7BF9F4B71B598101A52048CD30C16CB994330B10592D2C95D4D0E579F5287FF74A88268D0389698C8F7B9AE813F39246893D02661CF08D9CBCEC9F722CF76D0E96F1E17737E29ECE8676767CB6E1DF0DBD2D731ED848B1
  533:d=1  hl=3 l= 129 prim:  INTEGER           :EDFB4715EBA93BC4C2CBE712C8081027CC86A8D28D2C78C9720E6DE6F68031E0E34FFA5EEF0FD1D085AE49C0A800388BF7EE98A94A77E1181E603924B3B3BB9DCE97B80062F2830C8F11983DFADD55F1F9CE5362992E14C25F776EF7DACEEB719E1CF9F2F62F4BA6D003DE4D427EEB5A4D9815644FCE1255931BDF2BA37FC7A7
  665:d=1  hl=3 l= 129 prim:  INTEGER           :DE9DB5C35D2562F1CD3622342818C7BEBA0333207EDFDBC3F2648E6D1410B8914974A5AE32AFA8E4EAE40B42ADA5867E1B0E332FD0D0A2C8A9DE1ADBEDBD81F9BAB4C8FEC8CE3E660155E2CD04C6925B93FD88AFBE05DCC552A836E353A931209B23A13E7EB0F8FA919C44AC485CE37D6ADA8530AB56899C6669D44C5874AEFD
  797:d=1  hl=3 l= 128 prim:  INTEGER           :444CDEBCFAD2AA35B15685EE0CFCCB6E30B3E115F4B073C614F6F131DD43338D808FBEA2AA67D6E6CAC717A1B455C3E4DFF6595814E84CF0F81ED3A7A5EF8A8422FBC6324E339DCAE7F0BBC9E60ACA14D58612C67482163126E70731195A53965B33A3C4C84510A8428129B6F0C3AE564F78BB82FBA87FF8916CE96303DCB377
  928:d=1  hl=3 l= 129 prim:  INTEGER           :D3F4F73E16EEE4E173510A89FC6F73A79E3633B4C9F85CA7999FB2981AD5BCD5E049A70250123E4E0F73A7610A32A2F668CE4160528283AB694926EBA5D59CEE689D7F0E4FA5477619E96B73670BA60879C49923335B2393E11A76804584BF58DB3DB665E97C98E30246F67FCEBA5A836C7CB8F9D8F92136FFAFDDC9FF22C205
 1060:d=1  hl=3 l= 128 prim:  INTEGER           :76BC5D830BCC7EB721E87AF55645FFB8CEDDDDE56782E5304613D0117BB329DF7BDABAC7BB3489AF5B7FAFD00A498EC4F0BCEBCAA138C8124B8F0BF9330A9903504A6F5BF68CB620B94B034283B17E4EFC5A328B3D6C730AFB9E1EAD67EB5540246F16F88810691A5DD12204DE1E4DB7237DCE6677FBBD780E4DDB53F381DFC6
```
I use Crypto librabry to convert hex in line 273 to long.
```python
from Crypto.Util import number
hex = "7C3B1D534F299B43C1260876303C0A95BE17BF91A5DF2F1CACDA7C75A0236E4F81E1210D27C0126FB34D80F27A41A4D7E48CA7C5B0E78878B19FD0D6C0BF6830FB8A4401B16D938AD54C4D0B356862056CB0554EB2AB8390AD1825B31DAFBF2FC05D194F38C2F22420D3210ADA0230242640CAE005EB85CBC8DCCA1825EA7496D9B170C5CBFE354FE19A63102B82F38D5D7C251735208B83A54240927F899848C16A5FE70CE950DAFF7BF9F4B71B598101A52048CD30C16CB994330B10592D2C95D4D0E579F5287FF74A88268D0389698C8F7B9AE813F39246893D02661CF08D9CBCEC9F722CF76D0E96F1E17737E29ECE8676767CB6E1DF0DBD2D731ED848B1"
number.bytes_to_long(bytes.fromhex(hex))
# 15682700288056331364787171045819973654991149949197959929860861228180021707316851924456205543665565810892674190059831330231436970914474774562714945620519144389785158908994181951348846017432506464163564960993784254153395406799101314760033445065193429592512349952020982932218524462341002102063435489318813316464511621736943938440710470694912336237680219746204595128959161800595216366237538296447335375818871952520026993102148328897083547184286493241191505953601668858941129790966909236941127851370202421135897091086763569884760099112291072056970636380417349019579768748054760104838790424708988260443926906673795975104689
```
#### CERTainly not
<!-- As mentioned in the previous challenge, PEM is just a nice wrapper above DER encoded ASN.1. In some cases you may come across DER files directly; for instance many Windows utilities prefer to work with DER files by default. However, other tools expect PEM format and have difficulty importing a DER file, so it's good to know how to convert one format to another.

An SSL certificate is a crucial part of the modern web, binding a cryptographic key to details about an organisation. We'll cover more about these and PKI in the TLS category. Presented here is a DER-encoded x509 RSA certificate. -->
Find the modulus of the certificate, giving your answer as a decimal.

[2048b-rsa-example-cert.der](https://cryptohack.org/static/challenges/2048b-rsa-example-cert_3220bd92e30015fe4fbeb84a755e7ca5.der)
At first, I convert der file to pem file then get the public key. As the above challenges, I parst the string in line 19 then turn it to long number.
```python
!openssl x509 -inform der -in 2048b-rsa-example-cert.der -out cert.pem
!openssl x509 -pubkey -in cert.pem -out 2048b-rsa-example-cert.pem
!openssl asn1parse -i -in 2048b-rsa-example-cert.pem
#     0:d=0  hl=4 l= 290 cons: SEQUENCE          
#     4:d=1  hl=2 l=  13 cons:  SEQUENCE          
#     6:d=2  hl=2 l=   9 prim:   OBJECT            :rsaEncryption
#    17:d=2  hl=2 l=   0 prim:   NULL              
#    19:d=1  hl=4 l= 271 prim:  BIT STRING   
!openssl asn1parse -i -in 2048b-rsa-example-cert.pem -strparse 19
# 0:d=0  hl=4 l= 266 cons: SEQUENCE          
#     4:d=1  hl=4 l= 257 prim:  INTEGER           :B4CFD15E3329EC0BCFAE76F5FE2DC899C67879B918F80BD4BAB4D79E02520609F418934CD470D142A0291392735077F60489AC032CD6F106ABAD6CC0D9D5A6ABCACD5AD2562651E54B088AAFCC190F253490B02A29410F55F16B93DB9DB3CCDCECEBC75518D74225DE49351432929C1EC669E33CFBF49AF8FB8BC5E01B7EFD4F25BA3FE596579A2479491727D7894B6A2E0D8751D9233D068556F858310EEE81997868CD6E447EC9DA8C5A7B1CBF24402948D1039CEFDCAE2A5DF8F76AC7E9BCC5B059F695FC16CBD89CEDC3FC129093785A75B45683FAFC4184F6647934351CAC7A850E73787201E72489259EDA7F65BCAF8793198CDB7515B6E030C708F859
#   265:d=1  hl=2 l=   3 prim:  INTEGER           :010001
hex = "B4CFD15E3329EC0BCFAE76F5FE2DC899C67879B918F80BD4BAB4D79E02520609F418934CD470D142A0291392735077F60489AC032CD6F106ABAD6CC0D9D5A6ABCACD5AD2562651E54B088AAFCC190F253490B02A29410F55F16B93DB9DB3CCDCECEBC75518D74225DE49351432929C1EC669E33CFBF49AF8FB8BC5E01B7EFD4F25BA3FE596579A2479491727D7894B6A2E0D8751D9233D068556F858310EEE81997868CD6E447EC9DA8C5A7B1CBF24402948D1039CEFDCAE2A5DF8F76AC7E9BCC5B059F695FC16CBD89CEDC3FC129093785A75B45683FAFC4184F6647934351CAC7A850E73787201E72489259EDA7F65BCAF8793198CDB7515B6E030C708F859"
number.bytes_to_long(bytes.fromhex(hex))
# 22825373692019530804306212864609512775374171823993708516509897631547513634635856375624003737068034549047677999310941837454378829351398302382629658264078775456838626207507725494030600516872852306191255492926495965536379271875310457319107936020730050476235278671528265817571433919561175665096171189758406136453987966255236963782666066962654678464950075923060327358691356632908606498231755963567382339010985222623205586923466405809217426670333410014429905146941652293366212903733630083016398810887356019977409467374742266276267137547021576874204809506045914964491063393800499167416471949021995447722415959979785959569497
```
#### SSH Keys
<!-- Secure Shell Protocol (SSH) is a network protocol that uses cryptography to establish a secure channel over an insecure network (i.e. the internet). SSH enables developers and system administrators to run commands on servers from the other side of the world, without their password being sniffed or data being stolen. It is therefore critical to the security of the web.

In the old days, system administrators used to logon to their servers using telnet. This works similarly to our interactive challenges that involve connecting to socket.cryptohack.org - data is sent to a remote server, which performs actions based on what is sent. There is no transport encryption, so anyone listening in on the network (such as the WiFi access point owner, your ISP, or the NSA) can see all the telnet traffic.

As the internet became increasingly hostile, people realised the need for both authentication and encryption for administrative network traffic. SSH, first released in 1995, achieves these goals and much more, with advanced functionality built into the software like port forwarding, X11 forwarding, and SFTP (Secure File Transfer Protocol). SSH uses a client-server architecture, meaning the server runs SSH as a service daemon which is always online and waiting to receive connections, and the user runs an SSH client to make a connection to it.

Most commonly, SSH is configured to use public-private key pairs for authentication. On the server, a copy of the user's public key is stored. The user's private key is stored locally on their laptop.

Now let's say Bruce wants to connect as his user account bschneier to his server bruces-server. From his laptop he runs ssh bschneier@bruces-server. His SSH client opens a connection to the server on port 22 where the SSH daemon listens. First, the ciphers that will be used are agreed upon, then a session key to encrypt the connection is established using Diffie-Hellman Key exchange, but we won't go into the details on that here. Then, the server sends a random challenge message encrypted with Bruce's public key. Bruce uses his private key to decrypt the challenge and send a hash of the random challenge message back, proving that he owns the correct private key and he therefore authenticates himself to the server as bschneier. Now, the server gives Bruce a shell to run commands. If public-private key cryptography doesn't make sense to you yet, don't worry - we'll cover it extensively in the RSA category.

An SSH private key is stored in the PEM format, which we discussed in the "Privacy-Enhanced Mail" challenge. So it looks like this and is stored on Bruce's laptop at /home/bschneier/.ssh/id_rsa:
```
-----BEGIN RSA PRIVATE KEY-----
MIIBCgKC... (a whole bunch of base64)
-----END RSA PRIVATE KEY-----
```
SSH public keys, however, use a different format:
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCtPLqba+GFvDHdFVs1Vvdk56cKqqw5cdomlu034666UsoFIqkig8H5kNsNefSpaR/iU7G0ZKCiWRRuAbTsuHN+Cz526XhQvzgKTBkTGYXdF/WdG/6/umou3Z0+wJvTZgvEmeEclvitBrPZkzhAK1M5ypgNR4p8scJplTgSSb84Ckqul/Dj/Sh+fwo6sU3S3j92qc27BVGChpQiGwjjut4CkHauzQA/gKCBIiLyzoFcLEHhjOBOEErnvrRPWCIAJhALkwV2rUbD4g1IWa7QI2q3nB0nlnjPnjjwaR7TpH4gy2NSIYNDdC1PZ8reBaFnGTXgzhQ2t0ROBNb+ZDgH8Fy+KTG+gEakpu20bRqB86NN6frDLOkZ9x3w32tJtqqrJTALy4Oi3MW0XPO61UBT133VNqAbNYGE2gx+mXBVOezbsY46C/V2fmxBJJKY/SFNs8wOVOHKwqRH0GI5VsG1YZClX3fqk8GDJYREaoyoL3HKQt1Ue/ZW7TlPRYzAoIB62C0= bschneier@facts
```
This format makes it easier for these public keys to be added as lines to the file /home/bschneier/.ssh/authorized_keys on the server. Adding the public key to this file allows the corresponding private key to be used to authenticate on the server.

The ssh-keygen command is used to produce these public-private keypairs. -->

Extract the modulus n as a decimal integer from Bruce's SSH public key.

[bruce_rsa.pub](https://cryptohack.org/static/challenges/bruce_rsa_6e7ecd53b443a97013397b1a1ea30e14.pub)

I use ssh-keygen to get the pem format. Then do the same CERTainly not challenge.

```python
!ssh-keygen -f bruce_rsa.pub -e -m PKCS8 >> bruce_rsa.pem
!openssl asn1parse -i -in bruce_rsa.pem
#     0:d=0  hl=4 l= 418 cons: SEQUENCE          
#     4:d=1  hl=2 l=  13 cons:  SEQUENCE          
#     6:d=2  hl=2 l=   9 prim:   OBJECT            :rsaEncryption
#    17:d=2  hl=2 l=   0 prim:   NULL              
#    19:d=1  hl=4 l= 399 prim:  BIT STRING  
!openssl asn1parse -i -in bruce_rsa.pem -strparse 19
#  0:d=0  hl=4 l= 394 cons: SEQUENCE          
#     4:d=1  hl=4 l= 385 prim:  INTEGER           :AD3CBA9B6BE185BC31DD155B3556F764E7A70AAAAC3971DA2696ED37E3AEBA52CA0522A92283C1F990DB0D79F4A9691FE253B1B464A0A259146E01B4ECB8737E0B3E76E97850BF380A4C19131985DD17F59D1BFEBFBA6A2EDD9D3EC09BD3660BC499E11C96F8AD06B3D99338402B5339CA980D478A7CB1C26995381249BF380A4AAE97F0E3FD287E7F0A3AB14DD2DE3F76A9CDBB0551828694221B08E3BADE029076AECD003F80A0812222F2CE815C2C41E18CE04E104AE7BEB44F58220026100B930576AD46C3E20D4859AED0236AB79C1D279678CF9E38F0691ED3A47E20CB6352218343742D4F67CADE05A1671935E0CE1436B7444E04D6FE643807F05CBE2931BE8046A4A6EDB46D1A81F3A34DE9FAC32CE919F71DF0DF6B49B6AAAB25300BCB83A2DCC5B45CF3BAD54053D77DD536A01B358184DA0C7E99705539ECDBB18E3A0BF5767E6C41249298FD214DB3CC0E54E1CAC2A447D0623956C1B56190A55F77EA93C1832584446A8CA82F71CA42DD547BF656ED394F458CC0A0807AD82D
#   393:d=1  hl=2 l=   3 prim:  INTEGER           :010001
hex = "AD3CBA9B6BE185BC31DD155B3556F764E7A70AAAAC3971DA2696ED37E3AEBA52CA0522A92283C1F990DB0D79F4A9691FE253B1B464A0A259146E01B4ECB8737E0B3E76E97850BF380A4C19131985DD17F59D1BFEBFBA6A2EDD9D3EC09BD3660BC499E11C96F8AD06B3D99338402B5339CA980D478A7CB1C26995381249BF380A4AAE97F0E3FD287E7F0A3AB14DD2DE3F76A9CDBB0551828694221B08E3BADE029076AECD003F80A0812222F2CE815C2C41E18CE04E104AE7BEB44F58220026100B930576AD46C3E20D4859AED0236AB79C1D279678CF9E38F0691ED3A47E20CB6352218343742D4F67CADE05A1671935E0CE1436B7444E04D6FE643807F05CBE2931BE8046A4A6EDB46D1A81F3A34DE9FAC32CE919F71DF0DF6B49B6AAAB25300BCB83A2DCC5B45CF3BAD54053D77DD536A01B358184DA0C7E99705539ECDBB18E3A0BF5767E6C41249298FD214DB3CC0E54E1CAC2A447D0623956C1B56190A55F77EA93C1832584446A8CA82F71CA42DD547BF656ED394F458CC0A0807AD82D"
number.bytes_to_long(bytes.fromhex(hex))
# 3931406272922523448436194599820093016241472658151801552845094518579507815990600459669259603645261532927611152984942840889898756532060894857045175300145765800633499005451738872081381267004069865557395638550041114206143085403607234109293286336393552756893984605214352988705258638979454736514997314223669075900783806715398880310695945945147755132919037973889075191785977797861557228678159538882153544717797100401096435062359474129755625453831882490603560134477043235433202708948615234536984715872113343812760102812323180391544496030163653046931414723851374554873036582282389904838597668286543337426581680817796038711228401443244655162199302352017964997866677317161014083116730535875521286631858102768961098851209400973899393964931605067856005410998631842673030901078008408649613538143799959803685041566964514489809211962984534322348394428010908984318940411698961150731204316670646676976361958828528229837610795843145048243492909
```
#### Transparency
<!-- When you connect to a website over HTTPS, the first TLS message sent by the server is the ServerHello containing the server TLS certificate. Your browser verifies that the TLS certificate is valid, and if not, will terminate the TLS handshake. Verification includes ensuring that:

- the name on the certificate matches the domain

- the certificate has not expired

- the certificate is ultimately signed (via a "chain of trust") by a root key of a Certificate Authority (CA) that's trusted by your browser or operating system

Since CAs have the power to sign any certificate, the security of the internet depends upon these organisations to issue TLS certificates to the correct people: they must only issue certificates to the real domain owners. However with Windows trusting root certificates from over 100 organisations by default, there's a number of opportunities for hackers, politics, or incompetence to break the whole model. If you could trick just a single CA to issue you a certificate for microsoft.com, you could use the corresponding private key to sign malware and bypass trust controls on Windows. CAs are strongly incentivised to be careful since their business depends upon people trusting them, however in practice they have failed several times.

In 2011 Comodo CA was compromised and the hacker was able to issue certificates for Gmail and other services. In 2016, Symantec was found to have issued over 150 certificates without the domain owner's knowledge, as well as 2400 certificates for domains that were never registered.

Due to such events, together with the fact that fraudulent certificates can take a long time to be discovered, since 2018 Certificate Transparency has been enforced by Google Chrome. Every CA must publish all certificates that they issue to a log, which anyone can search.

Attached is an RSA public key in PEM format. -->
Find the subdomain of cryptohack.org which uses these parameters in its TLS certificate, and visit that subdomain to obtain the flag.

[transparency.pem](https://cryptohack.org/static/challenges/transparency_afff0345c6f99bf80eab5895458d8eab.pem)

TLS search commonly used sha256 encryption, em will generate sha256 file after converting pem file to der file. Then I use https://search.censys.io/ to find the domain using this certificate.

Web: [Subdomain](https://thetransparencyflagishere.cryptohack.org/)

Flag: crypto{thx_redpwn_for_inspiration}