easy-ecc
=====

A simple and secure ECDH and ECDSA library.

Features
--------

 * Supports Windows and Linux/*nix (needs /dev/urandom).
 * Resistant to known side-channel attacks.
 * Twice as fast as OpenSSL for ECDSA verify and ECDH.
 * Written in C.
 * No dynamic memory allocation.
 * Support for 4 standard curves: secp128r1, secp192r1, secp256r1, and secp384r1
 * BSD 2-clause license.

Usage
-----

I recommend just copying (or symlink) ecc.h and ecc.c into your project. Then just `#include "ecc.h"` to use the easy-ecc functions.

Function documentation
----------------------

#### ecc_make_key
```c
int ecc_make_key(
	uint8_t p_publicKey[ECC_BYTES+1],
	uint8_t p_privateKey[ECC_BYTES]
);
```
Create a public/private key pair.
    
Outputs:
 * `p_publicKey`  - Will be filled in with the public key.
 * `p_privateKey` - Will be filled in with the private key.

Returns 1 if the key pair was generated successfully, 0 if an error occurred.

#### ecdh_shared_secret
```c
int ecdh_shared_secret(
	const uint8_t p_publicKey[ECC_BYTES+1],
	const uint8_t p_privateKey[ECC_BYTES],
	uint8_t p_secret[ECC_BYTES]
);
```
Compute a shared secret given your secret key and someone else's public key.
Note: It is recommended that you hash the result of `ecdh_shared_secret` before using it for symmetric encryption or HMAC.

Inputs:
 * `p_publicKey`  - The public key of the remote party.
 * `p_privateKey` - Your private key.

Outputs:
 * `p_secret` - Will be filled in with the shared secret value.

Returns 1 if the shared secret was generated successfully, 0 if an error occurred.

#### ecdsa_sign
```c
int ecdsa_sign(
	const uint8_t p_privateKey[ECC_BYTES],
	const uint8_t p_hash[ECC_BYTES],
	uint8_t p_signature[ECC_BYTES*2]
);
```
Generate an ECDSA signature for a given hash value.

Usage: Compute a hash of the data you wish to sign (SHA-2 is recommended) and pass it in to
this function along with your private key.

Inputs:
 * `p_privateKey` - Your private key.
 * `p_hash`       - The message hash to sign.

Outputs:
 * `p_signature`  - Will be filled in with the signature value.

Returns 1 if the signature generated successfully, 0 if an error occurred.

#### ecdsa_verify
```c
int ecdsa_verify(
	const uint8_t p_publicKey[ECC_BYTES+1],
	const uint8_t p_hash[ECC_BYTES],
	const uint8_t p_signature[ECC_BYTES*2]
);
```
Verify an ECDSA signature.

Usage: Compute the hash of the signed data using the same hash as the signer and
pass it to this function along with the signer's public key and the signature values (r and s).

Inputs:
    `p_publicKey` - The signer's public key
    `p_hash`      - The hash of the signed data.
    `p_signature` - The signature value.

Returns 1 if the signature is valid, 0 if it is invalid.
