-  cryptography helps us exchange data over a secure channel.
-  types of encryption:
	- **Symmetric** encryption: uses the same **key** to encrypt and decrypt. Examples of symmetric encryption: **DES** (Data Encryption Standard), **3DES** (Triple DES) and **AES**(Advances Encryption Standard)
	- **Asymmetric** encryption: uses one public key to encrypt and a private one to decrypt. Examples of assymetric encryption: **RSA**, **Diffie-Hellman**, **Elliptic Curve cryptography** (ECC). asymmetric encryption tends to be slower.

###### Basic math
- XOR Operation (Is a binary operation that is commonly used for encryption and decryption of data. XOR operates on binary data (bits) and is based on the principles of Boolean algebra. The operation involves two bits. The result of the operation is "1" if the two bits are different, and "0" if they are the same.)
- Modulo Operation which returns the remainder of the division operation.

----------

- **Authentication**: You want to be sure you communicate with the right person, not someone else pretending.
- **Authenticity**: You can verify that the information comes from the claimed source.
- **Integrity**: You must ensure that no one changes the data you exchange.
- **Confidentiality**: You want to prevent an unauthorised party from eavesdropping on your conversations.

-----
###### RSA
- multiplication of prime numbers
- variables: p, q, m, n, e, d, and c
	- p and q are large prime numbers
	- n is the product of p and q (n = p x q)
	- The public key is n and e
	- The private key is n and d
	- m is used to represent the original message, i.e., plaintext
	- c represents the encrypted text, i.e., ciphertext
	- _ϕ_(_n_) = _n_ − _p_ − _q_ + 1

###### Diffie-Hellman Key Exchange
-  A = g^a % p
![[diffie-hellman_key_exchange.png]]

###### Digital Signature
-  Digital signatures are used to verify the authenticity and integrity of a document.

![[digital_signature_example.png]]
- Bob **encrypts** a hash of his document and shares it with Alice, along with the original document. Alice can **decrypt** the encrypted hash and compare it with the hash of the file she received. This approach proves the document’s integrity

###### Certificates: Prove Who You Are!
- How does your web know that is talking with the real **tryhackme.com** (example)? Through **certificates**. The web server has a certificate that says it is the real tryhackme.com
- Let’s say you have a website and want to use HTTPS. This step requires having a TLS certificate. You can get one from the various certificate authorities for an annual fee. Furthermore, you can get your own TLS certificates for domains you own using [Let's Encrypt](https://letsencrypt.org/) for free. If you run a website, it’s worth setting up and switching to HTTPS, as any modern website would do.

###### PGP (Pretty Good Privacy)
-  GnuPG or GPG: open implementation of OpenPGP
- `gpg2john`
- `gpg --import backup.key`
- `gpg --decrypt confidential_message.gpg`

###### Cryptoanalysis
- **Cryptanalysis** is the study of methods to break or bypass cryptographic security systems without knowing the key.
- Brute force attack vs Dictionary attack: 
	- **Brute-Force Attack** is an attack method that involves trying every possible key or password to decrypt a message.
	- **Dictionary Attack** is an attack method where the attacker tries dictionary words or combinations of them.

###### Hashing basics
-  A **hash value** is a fixed-sized string or characters computed by a **hash function**.
-  A **hash function** takes an arbitrary-sized input and returns an output of fixed length (hash value)
-  Hash functions are different from encryption. Hash functions do not use a **key**. And it is meant to be impossible to go from the output (hash value) back to the input.
 -  `md5sum file.txt`
 -  `sha1sum file.txt`
 -  `sha256sum file.txt`
 -  **Hash Collision** happens when a hash function give the same output for two or more different inputs.
- `Yes, the fixed-size output of a hash function is a primary reason for the pigeonhole effect in this context. Since a hash function maps an input of any size to a fixed-size output, the number of possible outputs is limited. Consequently, there will always be more potential inputs than outputs, making it inevitable for at least two different inputs to produce the same output, leading to a hash collision. This situation is an application of the pigeonhole principle, which highlights that, in a set of more items than containers, some containers must contain more than one item.`
--------
-  A **Rainbow Table** is a lookup table for hashes. You can quickly find what password a user had just from the hash. Websites like [CrackStation](https://crackstation.net/) and [Hashes.com](https://hashes.com/en/decrypt/hash) use a Rainbow Table.
-  To protect against rainbow tables use a salt (a random value that is added to the hash)
-  The salt is added to either the start or the end of the password before it’s hashed, and this means that every user will have a different password hash even if they have the same password. Hash functions like Bcrypt and Scrypt handle this automatically. Salts don’t need to be kept private.
-  Examples of secure hashing algorithms: `Argon2`, `Scrypt`, `Bcrypt`, `PBKDF2`

###### Gnu Privacy Guard
-  encryption: `gpg --symmetric --cipher-algo CIPHER message.txt` (add `--armor` for armored output)
-  decryption: `gpg --output original_message.txt --decrypt message.gpg`

###### OpenSSL Project
-  encryption: `openssl aes-256-cbc -e -in message.txt -out encrypted_message`
	-  to make it more secure against brute-force attacks: `-pbkdf2` (Password-Based Key Derivation Function 2)
	-  `-iter NUMBER`: number of iterations on the password to derive the encryption key
-  decryption: `openssl aes-256-cbc -d -in encrypted_message -out original_message.txt`

- `openssl genrsa -out private-key.pem 2048`: With `openssl`, we used `genrsa` to generate an RSA private key. Using `-out`, we specified that the resulting private key is saved as `private-key.pem`. We added `2048` to specify a key size of 2048 bits.
- `openssl rsa -in private-key.pem -pubout -out public-key.pem`: Using `openssl`, we specified that we are using the RSA algorithm with the `rsa` option. We specified that we wanted to get the public key using `-pubout`. Finally, we set the private key as input using `-in private-key.pem` and saved the output using `-out public-key.pem`.
- `openssl rsa -in private-key.pem -text -noout`: We are curious to see real RSA variables, so we used `-text -noout`. The values of _p_, _q_, _N_, _e_, and _d_ are `prime1`, `prime2`, `modulus`, `publicExponent`, and `privateExponent`, respectively.

----
###### Diffie-Hellman Key Exchange

1. Alice and Bob agree on _q_ and _g_. For this to work, _q_ should be a prime number, and _g_ is a number smaller than _q_ that satisfies certain conditions. (In modular arithmetic, _g_ is a generator.) In this example, we take _q_ = 29 and _g_ = 3.
2. Alice chooses a random number _a_ smaller than _q_. She calculates _A_ = (_g__a_) mod _q_. The number _a_ must be kept a secret; however, _A_ is sent to Bob. Let’s say that Alice picks the number _a_ = 13 and calculates _A_ = 313%29 = 19 and sends it to Bob.
3. Bob picks a random number _b_ smaller than _q_. He calculates _B_ = (_g__b_) mod _q_. Bob must keep _b_ a secret; however, he sends _B_ to Alice. Let’s consider the case where Bob chooses the number _b_ = 15 and calculates _B_ = 315%29 = 26. He proceeds to send it to Alice.
4. Alice receives _B_ and calculates _k__e__y_ = _B__a_ mod _q_. Numeric example _k__e__y_ = 2613 mod 29 = 10.
5. Bob receives _A_ and calculates _k__e__y_ = _A__b_ mod _q_. Numeric example _k__e__y_ = 1915 mod 29 = 10.

- Diffie-Hellman is an asymmetric encryption algorithm. Can be used to share a secret key.
![[diffie-hellman.png]]

- generate dh (diffie-hellman) params: `openssl dhparam -in dhparams.pem -text -noout`
- `hmac256 <key> message.txt`

![[mallory_mitm_diffie_hellman.png]]
`openssl req -new -nodes -newkey rsa:4096 -keyout key.pem -out cert.csr` - generate a certificate signing request
`openssl req -x509 -newkey -nodes rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 365` - generate a self-signed certificate
`openssl x509 -in cert.pem -text` - inspect a certificate