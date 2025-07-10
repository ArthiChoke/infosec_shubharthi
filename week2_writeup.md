#CHALLENGE 1

##part_1

1.For binary part, used pythoncode:

```python
binary = "01101000 01101001"
binary_list = binary.split()
print(binary_list)
for b in binary_list:
    number = int(b, 2)
    letter = chr(number)
    print(letter)
message = ''.join([chr(int(b, 2)) for b in binary_list])
```

So the message we got is “hi”

2.Second part consisted of numbers from 0-7, so it is octal

Used pythoncode:

```python
octal_str = "173 61 144 63 156 67 61 146 171"
tokens = octal_str.split()
chars = []
for o in tokens:
	temp = int(o, 8)
	char = chr(temp)
	chars.append(char)
message = ''.join(chars)
print(message)
```

The message we got is “{1d3n71fy”

3.Third part looks like decimal values of ASCII characters

Used pythoncode:

```python
decimal_str = "49 110 103 95 100 49 66 66 33 72 33"
tokens = decimal_str.split()
chars = []
for d in tokens:
    code = int(d, 10)
    ch = chr(code)
    chars.append(ch)
message = ''.join(chars)
print(message)
```

Message found = “1ng_d1BB!H!”

4.Fourth part is hexdata

Used pythoncode:

```python
hex_str = "6e 37 5f 33"
tokens = hex_str.split()
chars = []
for h in tokens:
    code = int(h, 16)
    ch = chr(code)
    chars.append(ch)
message = ''.join(chars)
print(message)
```

Message found = n7_3

5.Fifth part is just base64 encoded data

In terminal, used command:

```bash
echo bmMwZDFuZzV9 | base64 -d
```

Message found = “nc0d1ng5}”

The final string is {1d3n71fy1ng_d1BB!H!n7_3nc0d1ng5}

##part_2

1.The source.enc file looked like base64 encoded data  
2.After decoding it i found that it’s pythoncode which reads a file named flag.txt and writes the output in a file named output.txt  
3.I copied the data from the output.txt file and pasted it into flag.txt using the command:  
```bash
echo -n 43104f0c32077b0230455f346e5e77285868722d345a643b6256350636027d77 > flag.txt
```
4.I saved the pythoncode in a file named decoded.py and ran the command:
```bash
python3 decoded.py
```
5.then i used:
```bash
cat output.txt
```
and found some hexdata  
6.I decoded the hexdata using pyhtoncode and found the flag:  
CO2{0_nwXr4db56}


# CHALLENGE 2 - CRYPTOHACK

## GENERAL

### 1. ASCII
Convert decimal ASCII codes into characters.
```python
numbers = [99, 114, 121, 112, 116, 111, 123, 65, 83, 67, 73, 73, 95, 112, …]
for number in numbers:
    letter = chr(number)
    print(letter)
```

### 2. Hex
Decode a hex string to raw bytes.
```bash
echo 63727970746f7b596f755f77696c6c5f62655f776f726b696e675f776974685f6865785f737472696e67735f615f6c6f747d | xxd -r -p
```

### 3. Base64
Decode hex to bytes to base64.
```bash
echo 72bca9b68fc16ac7beeb8f849dca1d8a783e8acf9679bf9269f7bf | xxd -r -p | base64
# Output: crypto/Base+64+Encoding+is+Web+Safe/
```

### 4. Bytes and Big Integers
Convert big integers to bytes.
```python
data = 1151519506386231889993168548881374739577551628728968263649996528271
hexdata = hex(data)
print(hexdata)
message = bytes.fromhex(hexdata[2:])
print(message)
```

---

## XOR

### 1. XOR Starter
```python
from pwn import xor
data = b"label"
key = 13
value = xor(data, key)
print(value.decode())
```

### 2. XOR Properties
```python
from pwn import xor
key1 = bytes.fromhex("a6c8b6733c9b22de7bc0253266a3867df55acde8...")
key23 = bytes.fromhex("c1545756687e7573db23aa1c3452a098b71a7fb...")
fkey123 = bytes.fromhex("04ee9855208a2cd59091d04767ae47963170d...")
key123 = xor(key1, key23)
flag = xor(key123, fkey123)
print(flag)
```

### 3. Favourite Byte
```python
from pwn import xor
data = bytes.fromhex("73626960647f6b206821204f21254f7d694f7624...")
for key in range(256):
    message = xor(data, key).decode()
    if "crypto" in message:
        print(message)
        break
```

### 4. You Either Know, XOR You Don’t
```python
from pwn import xor
data = bytes.fromhex("0e0b213f26041e480b26217f27342e175d0e070a...")
known_pt = b"crypto{"
keys = bytearray(len(known_pt))
for i in range(len(known_pt)):
    keys[i] = data[i] ^ known_pt[i]
fullkey = b"myXORkey"
message = bytearray(len(data))
for i in range(len(data)):
    message[i] = data[i] ^ fullkey[i % len(fullkey)]
print(message)
```

---

## MATHEMATICS

### 1. Greatest Common Divisor
```python
def gcd(a, b):
    while a != b:
        if a > b:
            a -= b
        else:
            b -= a
    return a
print(gcd(12, 8))
print(gcd(66528, 52920))
```

### 2. Modular Arithmetic 1
```python
x = 11 % 6
y = 8146798528947 % 17
print("Result =", min(x, y))
```

### 3. Modular Arithmetic 2
```python
base = 273246787654
exp = 65536
mod = 65537
result = pow(base, exp, mod)
print(result)
```

### 4. Modular Inversion
```python
for i in range(100):
    if (3 * i) % 13 == 1:
        print(i)
        break
```

---

## SYMMETRIC CIPHERS: AES

### 1. Keyed Permutations
mathematical term for a one-to-one correspondence is bijection

### 2. Resisting Bruteforce
name for the best single-key attack against AES is biclique attack

### 3. Structure of AES
```python
def matrix2bytes(matrix):
    return bytes(sum(matrix, []))

matrix = [
    [99, 114, 121, 112],
    [116, 111, 123, 105],
    [110, 109, 97, 116],
    [114, 105, 120, 125],
]
print(matrix2bytes(matrix))
```

---

## RSA

### 1. Modular Exponentiation
```python
flag = pow(101, 17, 22663)
print(flag)
```

### 2. Public Keys
```python
plaintext = 12
n = 17 * 23
e = 65537
ciphertext = pow(plaintext, e, n)
print(ciphertext)
```

### 3. Euler’s Totient
```python
p = 857504083339712752489993810777
q = 1029224947942998075080348647219
phi = (p - 1) * (q - 1)
print(phi)
```

### 4. Private Keys
```python
e = 65537
phi = (p - 1) * (q - 1)
d = pow(e, -1, phi)
print(d)
```

### 5. RSA Decryption
```python
N = 882564595536224140639625987659416029426239230804614613279163
c = 77578995801157823671636298847186723593814843845525223303932
d = pow(e, -1, phi)
plaintext = pow(c, d, N)
print(plaintext)
```

### 6. RSA Signatures
```python
from Crypto.Hash import SHA256
from Crypto.Util.number import bytes_to_long

flag = b"crypto{immut4ble_m3ssaging}"
h = SHA256.new(flag).digest()
h_int = bytes_to_long(h)
sig = pow(h_int, d, N)
print(sig)
```
