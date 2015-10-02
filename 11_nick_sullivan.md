# What's New in Go Crypto

### Presenter: Nick Sullivan ([@grittygrease](https://twitter.com/@grittygrease)), Cloudflare
----

- Go's crypto packages: mostly written by Adam Langley, then David Crawshaw, Russ Cox...
- Inpsired by Railgun, around Go 1.0.
- Connection encrypted with TLS... but it's a huge CPU hog. Added OpenSSL with cgo. Headache to maintain. Moved to Go Crypto RC4. Not secure enough.
- Vlad the Compiler - perf improvements for AES-GCM, available in Go 1.6
- 20x faster than standard lib, similar benchmarks as OpenSSL
- AES-GCM assembly using \*.s files
- Morsing: use CSRs, ECDSA certs
- CFSSL: started around 1.0, full-featured CA, X.509 cert chain budler, TLS configuration scanner
- Railgun uses Cloudflare APi to get its keys from CFSSL
- Interview: let's implement something in one day -> made it into stdlib
- crypto.Signer - private key interface in Go 1.4
- PKCS#11 (github.com/cloudflare/cfssl/crypto/pkcs11key) - want key on hardware, not in memory or on disk, implements Signer
- ^ will be used by Let's Encrypt!
- RRDNS: Cloudflare's DNS server and DNS proxy
- Cache poisoning, man-in-the-middle
  * Solution: DNSSEC - digital signatures in the DNS, live-signed answers, elliptic curve keys
- github.com/cloudflare/go - assembly implementation of P256, in Go soon (copyright with Intel), makes ECDSA 20-30x faster
- gokeyless: private-key-less TLS
  * private key only used once during TLS handshake, extract into remote server, perf benefit by connecting to a nearby server (faster), don't need the key
  * initial keyless implementation was in C
- `crypto.Decrypter` - implement Signer and Decryper to use with TSL
- Many additions in Go 1.5 contributed by Cloudflare
- Now possible: TLS load balancer with hardware key (PKCS#11, TPM coming soon), arbitrary RSA/ECDSA implementations
