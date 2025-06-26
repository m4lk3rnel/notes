
- **Cube root attack** on a **RSA** ciphertext
#### Generating the public and the private keys for RSA
- `openssl genpkey -algorithm RSA -out private_key.pem -pkeyopt rsa_keygen_bits:2048`
- `openssl rsa -pubout -in private_key.pem -out public_key.pem`

#### Encrypting and decrypting the massage
-  the message we want to encrypt is in "message.txt"
	- e.g. `echo "test" > message.txt`
- `openssl pkeyutl -encrypt -in message.txt -pubin -inkey public_key.pem -out encrypted.bin`
- `openssl pkeyutl -decrypt -in encrypted.bin -inkey private_key.pem -out decrypted.txt`
