![CI](https://github.com/kokke/tiny-AES-c/actions/workflows/c-cpp.yml/badge.svg)
### Tiny AES in C

This is a small and portable implementation of the AES [ECB](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Electronic_Codebook_.28ECB.29), [CTR](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Counter_.28CTR.29) and [CBC](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher_Block_Chaining_.28CBC.29) encryption algorithms written in C.


The API is very simple and looks like this (I am using C99 `<stdint.h>`-style annotated types):

```C
/* Initialize context calling one of: */
void AES_init_ctx(struct AES_ctx* ctx, const uint8_t* key, uint8_t keyLength);
void AES_init_ctx_iv(struct AES_ctx* ctx, const uint8_t* key, const uint8_t* iv, uint8_t keyLength);

/* ... or reset IV at random point: */
void AES_ctx_set_iv(struct AES_ctx* ctx, const uint8_t* iv);

/* Then start encrypting and decrypting with the functions below: */
void AES_ECB_encrypt(const struct AES_ctx* ctx, uint8_t* buf);
void AES_ECB_decrypt(const struct AES_ctx* ctx, uint8_t* buf);

void AES_CBC_encrypt_buffer(struct AES_ctx* ctx, uint8_t* buf, size_t length);
void AES_CBC_decrypt_buffer(struct AES_ctx* ctx, uint8_t* buf, size_t length);

/* Same function for encrypting as for decrypting in CTR mode */
void AES_CTR_xcrypt_buffer(struct AES_ctx* ctx, uint8_t* buf, size_t length);
```

Important notes: 
 * No padding is provided so for CBC and ECB all buffers should be multiples of 16 bytes. For padding [PKCS7](https://en.wikipedia.org/wiki/Padding_(cryptography)#PKCS7) is recommendable.
 * ECB mode is considered unsafe for most uses and is not implemented in streaming mode. If you need this mode, call the function for every block of 16 bytes you need encrypted. See [wikipedia's article on ECB](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Electronic_Codebook_(ECB)) for more details.
 * This library is designed for small code size and simplicity, intended for cases where small binary size, low memory footprint and portability is more important than high performance. If speed is a concern, you can try more complex libraries, e.g. [Mbed TLS](https://tls.mbed.org/), [OpenSSL](https://www.openssl.org/) etc.

You can choose to use any or all of the modes-of-operations, by defining the symbols CBC, CTR or ECB in [`aes.h`](https://github.com/kokke/tiny-AES-c/blob/master/aes.h) (read the comments for clarification).

C++ users should `#include` [aes.hpp](https://github.com/kokke/tiny-AES-c/blob/master/aes.hpp) instead of [aes.h](https://github.com/kokke/tiny-AES-c/blob/master/aes.h)

There is no built-in error checking or protection from out-of-bounds memory access errors as a result of malicious input.

It is one of the smallest implementations in C I've seen yet, but do contact me if you know of something smaller (or have improvements to the code here). 

This implementation is verified against the data in:

[National Institute of Standards and Technology Special Publication 800-38A 2001 ED](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-38a.pdf) Appendix F: Example Vectors for Modes of Operation of the AES.

The other appendices in the document are valuable for implementation details on e.g. padding, generation of IVs and nonces in CTR-mode etc.


A heartfelt thank-you to [all the nice people](https://github.com/kokke/tiny-AES-c/graphs/contributors) out there who have contributed to this project.


All material in this repository is in the public domain.
