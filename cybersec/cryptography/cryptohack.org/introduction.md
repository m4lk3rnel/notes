 -  In Python, the `chr()` function can be used to convert an ASCII ordinal number to a character (the `ord()` function does the opposite).
 -  `bytes.fromhex()` - hex to ASCII string.
 -  `bytes.hex()` - ASCII string to hex, e.g. 
```python
bytes.hex(b'crypto{You_will_be_working_with_hex_strings_a_lot}')

'63727970746f7b596f755f77696c6c5f62655f776f726b696e675f776974685f6865785f737472696e67735f615f6c6f747d'
```
-  working with `base64` in python:
```python
import base64
# encode a string to a base64 string
base64.b64encode(b'hi')
# decode a base64 string
base64.b64decode(b'SGk=')
```
- in the terminal: 
	- encode: `echo "Hi" | base64`
	- decode: `echo "SGk== | base64 -d`
- [how base64 works](https://www.redhat.com/en/blog/base64-encoding)
- decimal to hex: 
```python
hex(11515195063862318899931685488813747395775516287289682636499965282714637259206269)
```
- to convert a **BIT** string, e.g. "01100001" (binary representation for 'a') to **char**:
```python
char = chr(int(bit_string, 2)) # int(string, base)
```
